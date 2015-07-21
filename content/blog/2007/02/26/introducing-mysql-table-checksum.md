---
title: Introducing MySQL Table Checksum
date: "2007-02-26"
url: /blog/2007/02/26/introducing-mysql-table-checksum/
categories:
  - Databases
---

[MySQL Table Checksum](http://code.google.com/p/maatkit/) is a tool to efficiently verify the contents of any MySQL table in any storage engine. You can use it to compare tables across many servers at once. The output is friendly and easy to use, both by eyeball and in UNIX command-line scripts. The provided MySQL Checksum Filter helps you winnow output so you only see tables that have problems.

### MySQL Table Checksum

Following up on my earlier article about [how to calculate a table checksum in MySQL](/blog/2007/01/25/how-to-calculate-table-checksums-in-mysql/), I've integrated that methodology, with improvements suggested by the commenters and others, into a single easy-to-use tool. It is distributed as part of the [MySQL Toolkit](http://code.google.com/p/maatkit/), available from SourceForge.net.

The tool takes server-side checksums using user-variables, so it is very efficient. It can checksum tables on many servers at once, running in parallel for speed. It has options to help you guarantee your tables are in the same state on your master and slave servers, and you can even checksum only some rows. These features can help you verify replication without locking tables or taking servers offline.

Here's some sample output, in this case generated by comparing my server against itself:

<pre>DATABASE TABLE    HOST      ENGINE      COUNT                                 CHECKSUM TIME WAIT STAT  LAG
test     chapters localhost MyISAM         21                                218345624    0    0 NULL NULL
test     chapters 127.0.0.1 MyISAM         21                                218345624    0    0 NULL NULL
test     foo      localhost InnoDB          1 f14825835a0c07091c7b6a28c8a9f7120667815d    0    0 NULL NULL
test     foo      127.0.0.1 InnoDB          1 f14825835a0c07091c7b6a28c8a9f7120667815d    0    0 NULL NULL
test     samples  127.0.0.1 MyISAM          7                               2103838486    0    0 NULL NULL
test     samples  localhost MyISAM          7                               2103838486    0    0 NULL NULL</pre>

### MySQL Checksum Filter

For efficiency reasons and to be as general-purpose as possible, the checksum tool itself doesn't process its output, and in fact doesn't even output in sorted order. However, the output is specifically designed to be easy to parse and manipulate with standard command-line tools like `awk` and `sort`.

It's not in my nature to make you do that work yourself, so I included a tool that will do it for you. It sorts input and makes sure the checksums and row counts for a given table match on all servers. You can either pipe the checksums directly into it, or give it a list of files to process (handy when you need to run the checksum in different places, pipe their outputs to files, and then process the files).

If you use it to filter the output I showed above, you'll see nothing by default, because the tables have identical checksums -- thus there's nothing to see.

### About MySQL Toolkit

[MySQL Toolkit](http://code.google.com/p/maatkit/) is a new project I started on SourceForge to contain many of the MySQL utilities I've written and am writing (yes, there are more goodies in progress). Eventually these and other tools will all be bundled together so you can get them in one package.

### About me

I like making you happy. Make me happy in return: [donate](/blog/donate/).

