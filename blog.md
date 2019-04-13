# Blog Index

This page will list all entries and maybe categories.

<ul>
{% for post in site.posts %}
<li>[{{ post.title }}]({{ post.id }})</li>
{% endfor %}
</ul>

Tag Test
<ul>
{% for post in site.posts | :where "tags", "test  %}
<li>[{{ post.title }}]({{ post.id }})</li>
{% endfor %}
</ul>

## New Code Snippet

{%- highlight powershell -%}
Get-SqlMergeStatement -TargetTableName Tbl -JoinColumns a -MergeColumns a,b,c
{%- endhighlight -%}

{%- highlight sql -%}
WITH SRC AS
(
    /* your source query here */
)
MERGE INTO Tbl WITH (HOLDLOCK) AS TGT
    USING SRC ON (SRC.a = TGT.a)

WHEN NOT MATCHED BY TARGET THEN
    INSERT (
        a,
        b,
        c
    )
    VALUES (
        SRC.a,
        SRC.b,
        SRC.c
    )

WHEN MATCHED AND EXISTS (
    SELECT SRC.b, SRC.c
    EXCEPT
    SELECT TGT.b, TGT.c
    )
THEN
    UPDATE SET
    b = SRC.b,
    c = SRC.c

;
{%- endhighlight -%}