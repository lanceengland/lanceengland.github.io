---
date: 2014-05-23
tags: analysis automation
title: Processing SSAS with SSIS script tasks
---
# Processing SSAS with SSIS script tasks

When processing SSAS from SSIS (2008), you have a few choices:

1. Use the "Analysis Services Processing Task"
2. Use the "Analysis Services Execute DDL Task"
3. Use the AMO via the script task

The "Analysis Services Processing Task" is a GUI that builds the XMLA for you and stores it encode in the DTSX file. This option is quickest to use, but is not flexible. Adding a new dimension, for example, requires you to edit the package (and specifically this task).

The "Analysis Services Execute DDL Task" requires you to generate the XMLA for the objects to process. This means you can decouple your XMLA from your package, similar to configuration files or stored procedures references by a package could be edited without haveing to edit and redeploy the package. The only downside to this is you have to remember to regenerate your XMLA when you add a dimension.

Both of the first two choices are limited in dynamically processing measure group partitions, so that you only process the most current partition. I suppose you could use logic to build your XMLA before you execute it via "Analysis Services Execute DDL Task". But how do you want to generate that XML? Angle brackets and string concatenation? Excuse while I go throw up up thinking about that.

But, you have another option behind door #3! You could use Analysis Management Objects (AMO) to process your database! The remainder of this post is to show you a super-simple processing strategy. A future post will look at a more complicate scenario.

AMO is an API to interact with all the objects inside of Analysis Server. And best of all, when executing methods, behind the scenes it is gerating the XMLA and submitting it to SSAS. You can fire up a trace and see the commands sent if you're into that sort of thing ;)

For this simple processing scenario, the requirements are few:

1. Break up processing so that dimensions are processed first, then the cube and measure groups second.
2. Have the ability to specify process update or process full through configuration
3. Be able to add a cube dimension and have it be processed automatically without having to editing our package or re-generate our XMLA.

With that in mind, we have two script tasks. First the code, then a little explanation.

## PROCESS DIMENSIONS

<pre data-enlighter-language="csharp">
Dts.TaskResult = (int)ScriptResults.Failure;

bool ssasProcessFullDimensions = false;
try
{
    ssasProcessFullDimensions = Convert.ToBoolean(Dts.Variables["User::SSASProcessFullDimensions"].Value);
}
catch (InvalidCastException ex)
{
    Dts.Log(ex.ToString(), 0, null);
    return;
}

using (ConnectionManager cm = Dts.Connections["SSAS.BP_BI_Gypsum_Mfg"])
{
    if (cm == null) { return; }
    using (SSAS.Server srv = new Microsoft.AnalysisServices.Server())
    {
        srv.Connect(cm.ConnectionString);
        using (SSAS.Database db = srv.Databases.FindByName(srv.ConnectionInfo.Catalog))
        {
            if (db == null) { return; }
            using (SSAS.Cube cube = db.Cubes.FindByName("Quality")) // could/should be a parameter
            {
                if (cube == null) { return; }

                // process dimensions in batch mode by capturing the XMLA and executing it after the loop
                srv.CaptureXml = true;
                foreach (SSAS.CubeDimension cubeDim in cube.Dimensions)
                {
                    using (cubeDim)
                    {
                        SSAS.Dimension dim = cubeDim.Dimension;
                        if (dim == null) { return; }
                        using (dim)
                        {
                            if (dim.State == SSAS.AnalysisState.Unprocessed || ssasProcessFullDimensions)
                            {
                                dim.Process(SSAS.ProcessType.ProcessFull);
                            }
                            else
                            {
                                dim.Process(SSAS.ProcessType.ProcessUpdate);
                            }
                        }
                    }
                }
                srv.CaptureXml = false;
                SSAS.XmlaResultCollection results = srv.ExecuteCaptureLog(true, true, false);

                bool hasErrors = false;
                foreach (SSAS.XmlaResult result in results)
                {
                    foreach (SSAS.XmlaMessage message in result.Messages)
                    {
                        if (message is SSAS.XmlaError)
                        {
                            Dts.Log(message.Description, 0, null); // useful if logging is set up
                            hasErrors = true;
                        }
                    }
                }
                if (hasErrors)
                {
                    return; // TaskResult is failure
                }
            }
        }
    }
}
Dts.TaskResult = (int)ScriptResults.Success;
</pre>

A few things to note.

First, all major AMO objects implent the IDisposable interface. This means every object has a Dispose() method that cleans up memory and other resources. WHile this method gets called automatically when an object is about to be garbage-collected, it is best practice to call this as soon as you are finished with an object. The easiest and slickest way to do this in C# is with the "using(...)" construct. This is syntaic sugar; a language construct that actually turns into a try/finally block in the MSIL, with the Dispose() method being called in the finally block. All this to say, wrap those AMO objects in using statements.

Second, you can reference package connections from a script task. You can also pass in other variables like cube name.

Third, here is the dynamic part: We can query the cube for a list of cube dimensions and process those dimensions. Now you don't have to maintain your XMLA or regenerate your "Analysis Services Processing Task".

Fourth, I pass in a variable to indicate whether to process update or process full.  

Fourth, we can still execute the taks in parallel by setting

<pre data-enlighter-language="csharp">
srv.CaptureXml = true;
</pre>

If this were false, everytime we called the Process() method, AMO would send an XMLA command to the server. This means the dimensions would be processed sequentially. Setting the CaptureXML property to true means all the commands are collected in a capture log. At the end of the enumeration through the dimensions, we can send the execute the capture log as a parallel task.

Finally, the srv.ExecuteCaptureLog returns a results collection. We enumerate through the collection looking for errors, and fail the task if found.

The code for processing the cube is similar but simpler. Instead of enumerating the dimensions in a cube, we just call the Process method on the cube object. In order to get the xml results for error handling, I still execute the capture log through the server object.

## PROCESS CUBE

<pre data-enlighter-language="csharp">
Dts.TaskResult = (int)ScriptResults.Failure;
try
{
    using (ConnectionManager cm = Dts.Connections["SSAS.BP_BI_Gypsum_Mfg"])
    {
        if (cm == null) { return; }
        using (SSAS.Server srv = new Microsoft.AnalysisServices.Server())
        {
            srv.Connect(cm.ConnectionString);
            using (SSAS.Database db = srv.Databases.FindByName(srv.ConnectionInfo.Catalog))
            {
                if (db == null) { return; }
                using (SSAS.Cube cube = db.Cubes.FindByName("Quality"))
                {
                    if (cube == null) { return; }
                    // have to execute command from server to get error messages returned
                    srv.CaptureXml = true;
                    cube.Process(SSAS.ProcessType.ProcessFull);

                    srv.CaptureXml = false;
                    SSAS.XmlaResultCollection results = srv.ExecuteCaptureLog(true, true, false);

                    bool hasErrors = false;
                    foreach (SSAS.XmlaResult result in results)
                    {
                        foreach (SSAS.XmlaMessage message in result.Messages)
                        {
                            if (message is SSAS.XmlaError)
                            {
                                Dts.Log(message.Description, 0, null); // useful if logging is set up
                                hasErrors = true;
                            }
                        }
                    }
                    if (hasErrors)
                    {
                        return; // TaskResult is failure
                    }
                }
            }
            srv.Disconnect();
        }
    }
}
catch (Exception ex)
{
    Dts.Log("Error in process cube script task: " + ex.ToString(), 0, null);
}
Dts.TaskResult = (int)ScriptResults.Success;
</pre>

## CONCLUSION

I hope this post has demonstrated how to use AMO to make an adapable processing strategy. With the full power of the C# language, we could handle increasingly more complex scenarios like partition management including dynamic creation, merging, and processing only most recent. As mentioned in the opening, this will probably be a future blog post.