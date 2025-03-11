
<!-- ********************* -->
# Instructions for an LLM to specify the structure of a .md file while creating a .md file on request
<!-- ********************* -->

<!-- ********************* -->
# Goal
<!-- ********************* -->

These are instructions for an LLM to specify the structure of a .md file while creating a .md file on request. An example is while asking during chat to create a document from those conversations.

Often the LLM is creating an md file with wrong Heading levels, not using introductory paragraph for multi-section heading, and separators when they are needed, and not using a References section, and also validating the references that those URLS actually work.

<!-- ********************* -->
# Formal Instructions
<!-- ********************* -->

1. Follow these guidelines while creating or updating a .md file
2. Use languaget that is not exaggeration or over-emphasic. Keep an even tone.
3. Start all section headings with heading 1
4. If there are sub sections, introduce them before the sub sections with an introductory paragraph.
5. Encapsulate the title of each heading 1 and heading 2s in comments that look like the following below

```
<!-- ********************* -->
# Some heading
<!-- ********************* -->
```

6. Do not add a separator like `---` between sections
7. Add a reference section with links
8. For each link, write a line or two about what the link covers.
9. Add a numbered TOC at the top of the page