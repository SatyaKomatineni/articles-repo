---
description: 'Fix heading line comments'
mode: agent
---

Go through the file and edit it so that all heading comment sections are in the narrow format, like:

```
<!-- ********************* -->
# Heading
<!-- ********************* -->
```

This is called a heading comment section, and it should be formatted with the heading text immediately following the opening comment tag, without any empty lines in between. The closing comment tag should be on its own line after the heading text.

Apply this fix to every heading comment section in the file. 

That means even for headings 2 and 3 levels, you should ensure they are formatted correctly with the comment lines above and below them.

But do not alter any other lines outside of that heading comment section. 

For example, change this:

```
<!-- ********************* -->

# Heading

<!-- ********************* -->
```

to this:
```
<!-- ********************* -->
# Heading
<!-- ********************* -->
```

Make sure you apply this fix to all heading comment sections in the file, and ensure that the format is consistent throughout. Do not change any other content in the file, only focus on the heading comment sections.

**The headings should include heading 1, heading 2, and heading 3 levels, but not heading 4 or lower.**

Also if these headings don't have the comment lines above and below them, add those comment lines in the same format just above and below those sub headings. Don't touch any other lines in the file, just focus on the heading comment sections.

