---
date: 2020-02-06
tags: automation
title: "PowerShell and Zip files"
---
# PowerShell and Zip files

Another day, another problem that PowerShell helped solve quickly and easily.

During the course of a typical work day, often I will have a problem in front of me that just needs a quick and easy solution. In these cases, more often than not, PowerShell has been my tool of choice.

Yesterday, I needed to cross-reference a large set of files in an archive directory against data from a SQL Server. The challenge was that the files were zipped, and the field to cross-reference was a string of text embedded in a fixed-width format.

The data I needed from SQL Server I queried from SSMS and copy/pasted into Excel. For the zipped file data, I used the (slightly scrubbed) script below.

Now, I'll admit, if I wanted to be extra fancy I would have queried the SQL Server from PowerShell (maybe with the [dbatools](https://dbatools.io/) module) and then created the Excel file from Doug Finke's [ImportExcel](https://github.com/dfinke/ImportExcel) module. However, time was a priority, so I just added all the cross-reference data I needed to a [StringBuilder](https://docs.microsoft.com/en-us/dotnet/api/system.text.stringbuilder?view=netframework-4.8) object and copied the full string to the clipboard and then I pasted into Excel. I quick VLOOKUP formula later and I had what I needed.

## Notes

You have to reference the System.IO.Compression.FileSystem assembly. The call to System.IO.Path.GetTempFileName() creates a file and returns the path. I did not see an option to overwrite files during the zip extraction, so I delete the temp file first, and also after for cleanup. Also, I used Write-Host for me, and [it no longer kills puppies](https://twitter.com/jsnover/status/727902887183966208?lang=en).

![Puppies are no longer harmed by Write-Host](/assets/img/pug.jpg)

If anybody has suggestions for improvement, reach out via one the the contact links at the bottom of the web site.

<pre data-enlighter-language="shell">
Clear-Host
Add-Type -assembly "System.IO.Compression.FileSystem"
$sb = New-Object -TypeName System.Text.StringBuilder

Write-Host "$(Get-Date)"
Get-ChildItem -Path '\\company.fileshare\blahblah\file\archive' |
    Select-Object FullName |
    ForEach-Object {
        $tmpFilePath = [System.IO.Path]::GetTempFileName()
        $archive = [System.IO.Compression.ZipFile]::OpenRead($_.FullName)

        [System.IO.File]::Delete($tmpFilePath) |Out-Null
        [System.IO.Compression.ZipFileExtensions]::ExtractToFile($archive.Entries[0], $tmpFilePath) | Out-Null

        $contents = [System.IO.File]::ReadAllLines($tmpFilePath)
        $filekey = $contents[0].Substring(4, 17)
        $sb.AppendLine($filekey) |Out-Null
        [System.IO.File]::Delete($tmpFilePath) |Out-Null
    }
    if ($sb.Length -gt 0) {
        [System.Windows.Forms.Clipboard]::SetText($sb.ToString()) |Out-Null
        Write-Host "List set to clipboard"
    }
Write-Host "$(Get-Date)"
</pre>
