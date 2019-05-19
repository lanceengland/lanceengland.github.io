---
date: 2014-04-07
tags: data
title: T-SQL Study&#58; A SEQUENCE of Unfortunate Events
---
# T-SQL Study: A SEQUENCE of Unfortunate Events

Actually, sequences aren’t unfortunate at all :) And thusly, is today's topic in my ~~sprint~~ ~~marathon~~ ~~deathmarch~~ progress towards the 70-461 exam 'Querying with T-SQL yada yada yada'.

New to SQL Server 2012, [sequences](http://technet.microsoft.com/en-us/library/ff878058.aspx) are an ANSI-compliant way to generate sequential values typically used as keys. The main differences between sequences and identity are that sequences are not bound to a single table, and you can obtain the sequence value BEFORE you insert a row into table.

## Syntax

The two main parts of syntax for sequences is creating sequences and getting the next value for a sequence. Luckily, the sytax is basically CREATE SEQUENCE and NEXT VALUE FOR.

Creating a sequence is really easy if you accept all default options.

<pre data-enlighter-language="sql">
CREATE SEQUENCE dbo.Seq1;
</pre>

This will use a data type BIGINT, have a starting value of *-9,223,372,036,854,775,808* and increment by 1 and will NOT cycle back to the beginning when it hits the max value. Soooooo, maybe you don't want a gigantically large negative number as your starting point? Let's explore each option.

You can specify a **MINVALUE**:

<pre data-enlighter-language="sql">
CREATE SEQUENCE dbo.Seq1
  MINVALUE 1
;
</pre>

Conversely, you can also specify a **MAXVALUE**:

<pre data-enlighter-language="sql">
CREATE SEQUENCE dbo.Seq1
  MINVALUE 1
  MAXVALUE 100
;
</pre>

In the above example, if we were only generating values between 1 and 100, we should be good data stewards and declare a smaller data type with the **AS** keyword:

<pre data-enlighter-language="sql">
CREATE SEQUENCE dbo.Seq1
  AS tinyint
  MINVALUE 1
  MAXVALUE 100
;
</pre>

You can **INCREMENT BY** any non-zero integer within range of the sequence's data type *and* the absolute value of the increment must be less than or equal to the difference of the min and max value:

<pre data-enlighter-language="sql">
CREATE SEQUENCE dbo.Seq1
  AS tinyint
  MINVALUE 1
  MAXVALUE 100
  INCREMENT BY 10
;
</pre>

Note: Don't forget the "BY" keyword! I predict this will be a common omission!

You can also increm" n" number. This is called a descending sequence.

<pre data-enlighter-language="sql">
CREATE SEQUENCE dbo.Seq1
  AS smallint
  MINVALUE 1
  MAXVALUE 100
  INCREMENT BY -1
;
</pre>
For ascending sequences, the default starting value is MINVALUE. For *descending* sequences, the default starting value is MAXVALUE. If you want to specify the number to **START WITH**, just make sure it fall between the MINVALUE and the MAXVALUE:

<pre data-enlighter-language="sql">
CREATE SEQUENCE dbo.Seq1
  AS smallint
  MINVALUE 1
  MAXVALUE 100
  INCREMENT BY -1
  START WITH 90
;
</pre>

Note: I had to declare the sequence as SMALLINT because the -1 increment value is outside the range of TINYINT.

You can choose to have the sequence **CYCLE** back to it's starting value once it reaches it's max (or min, depending or if its an ascending or descending sequence). The default is to not cycle. The sequence will throw an error when exhausted.

<pre data-enlighter-language="sql">
CREATE SEQUENCE dbo.Seq1
  AS smallint
  MINVALUE 1
  MAXVALUE 100
  INCREMENT BY -1
  CYCLE
;
</pre>

Finally, we have the **CACHE** option. I suppose for high-performance apps that will generate lots of values a second, you will appreciate the control. Otherwise, seems you could probably be fine with the default number, which by the way, is unpublished becasue Microsoft reserves the right to change it in the future. Read up on the side effects on the [CREATE SEQUENCE](http://technet.microsoft.com/en-us/library/ff878091.aspx) Technet page. For purposes of the exam, I'm content for now to just know it exists.

## Usage

What good is a sequence if you can't get values from it? To accomplish this, you use the [NEXT VALUE FOR](http://technet.microsoft.com/en-us/library/ff878370.aspx) function. This function can be used in the following:

- SELECT

- INSERT

- UPDATE

- DECLARE/SET

- DEFAULT constraint

There are actually quite a few limitations and restrictions listed on the page I linked to above. I'm not going to re-list them here.

Some examples from the list (this assumes you have already created a sequence named *dbo.Seq1*:

SELECT

<pre data-enlighter-language="sql">
SELECT NEXT VALUE FOR dbo.Seq1;
</pre>

INSERT

<pre data-enlighter-language="sql">
declare @t table
(
    id int primary key
);
insert into @t (id) values (NEXT VALUE FOR dbo.Seq1), (NEXT VALUE FOR dbo.Seq1);
select * from @t;
</pre>

UPDATE

<pre data-enlighter-language="sql">
declare @t table
(
    id int primary key
);
insert into @t (id) values (NEXT VALUE FOR dbo.Seq1), (NEXT VALUE FOR dbo.Seq1);

update @t set id = NEXT VALUE FOR dbo.Seq1;

select * from @t;
</pre>

DECLARE/SET

<pre data-enlighter-language="sql">
declare @i int = NEXT VALUE FOR dbo.Seq1;
select @i;
</pre>

DEFAULT constraint

<pre data-enlighter-language="sql">
declare @t table
(
    id int primary key default (NEXT VALUE FOR dbo.Seq1),
    name varchar(50)
);
insert into @t (name) values ('Lance England');
select * from @t;
</pre>

## Conclusions

After playing around with them, I can appreciate the flexibility sequences offer. And it's never a bad thing to learn a cross-platform SQL feature.