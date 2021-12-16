---
layout: post
comments: true
published: true
title: Getting the most from markdown in GIFT
header-img: img/posts/MarkdownGIFT.png
background: '/img/posts/MarkdownGIFT.png'
---

Markdown <i class="fab fa-markdown"></i> is a format supported within GIFT (see: [Creating questions quickly in Moodle with GIFT]({% post_url 2020-12-18-Creating-questions-quickly-with-GIFT %})), which can be used for simple formatting text in a question, in answers and feedback:

```gift
::Simple markdown::
[markdown]If you're standing somewhere on the equator, the **sun** 
rises in the\: {=*east*#**Super!** ~*west* ~*north* ~*south*}
```

But, formatting code blocks or nested lists in Moodle requires some deeper understanding, because of the meanings of characters in the two formats. 
The solutions require escaping certain characters, which complicates the GIFT files. 
The good news is that using a smart editor, such as the [GIFT Format Pack extension in VSCode](https://marketplace.visualstudio.com/items?itemName=ethan-ou.vscode-gift-pack), can make it less painful.

### Moodle markdown is not GitHub markdown

When you want to format questions for Moodle, the markdown is Moodle-specific. 
Please see [Moodle's documentation on markdown](https://docs.moodle.org/311/en/Markdown) for various specificities. 
There are differences from GitHub's markdown (see the bulleted list example below), so it's best to look at the Moodle documentation if something's not formatting the way you expect.
Another trouble is that the Moodle GIFT parser doesn't really care if the text labeled as `[markdown]` is in the right markdown format.
It's just a tag that gets passed to the question engine (and other parts of Moodle that display it) *after* the GIFT parsing occurs.

### Where can I use markdown format in GIFT?

Markdown can be used in a question stem, the text of an answer and even in feedback. 
But it does not apply to matching answers, which appear in drop-down menus as unformatted text.
As long as you start a GIFT question (stem) with `[markdown]`, the formatable elements will expect the markdown syntax.

> You can actually specify a different format (markdown, html, etc.) for the stem, each answer and feedback by labeling the respective text with the tag of the format you want. 
> However, that usage is out of the scope of this blog post.

### Reminder: GIFT's control characters

It's necessary to escape any GIFT control characters in the text, regardless of markdown.
For example:

```gift
::Escaped control character::
[markdown]Two plus two\: {=four =4}
```

Notice the `\:` inside the question stem. This is necessary because `:` is one of [GIFT's special characters `~ = # { } :`](https://docs.moodle.org/311/en/GIFT_format#Special_Characters_.7E_.3D_.23_.7B_.7D), which generally must be escaped.
Again, using a tool that understands GIFT will help with this.
> Escaping is actually not always needed depending on context (I think you don't need to escape `=` in a stem). But since it doesn't hurt to escape control characters when they aren't supposed to take their GIFT meaning, it's simpler to just do it 👑.

### Line breaks

In GIFT, line breaks are considered white space and don't mean anything, unless there are two in succession - a blank line - to separate questions. 
Since markdown *does* use line breaks as part of its formatting, there must be a way to escape them so they don't get processed as GIFT.
The Moodle GIFT parser will convert the text `\n` (two characters) of a markdown text into a line break. Here's an example:

```
::Multi-line question stem::
[markdown]How much is\n\n2 + 2?{=four =4}
```

This produces a question with the line breaks as follows:

     How much is

     2 + 2

### Indentation

Markdown is sensitive to white space in some cases, e.g. indentation. 
Nested lists, for instance, must be indented:

```markdown
- Item A
  - Item A.1
  - Item A.2
- Item B
```

In the example above, we need to specify line breaks *and* indentation. 
It's also an example of a gotcha in markdown formatting, because [Moodle's documentation on markdown](https://docs.moodle.org/311/en/Markdown#Bullet_point_lists) states it uses `*` (rather than `-`) as the character for bulleted lists.

Here's an example of a nested, bulleted list in markdown that displays properly inside of Moodle (v.311):

```
::Nested bulleted list::
[markdown]This is a nested list\:\n\n* Item A\n  * Item A.1\n  * Item A.2\n* Item B{TRUE}
```

The `:` is escaped, there are several `\n` to indicate line breaks, and the number of spaces after the `\n` specifies how much indentation occurs.

I purposefully didn't use real line breaks in the GIFT (within the nested list in markdown), because it interfered with the formatting when I imported it.
That makes it less readable to have it one one line, but I know it works like this.

### Inline code

Formatting inline code is very straightforward. Here's an example:

```gift
::Inline code question::
[markdown]What's the result of the following expression in JavaScript\: 
`3 % 2`?
{=`1` ~`5` ~`NaN`}
```

This makes a question that looks something like:

     What's the result of the following expression in JavaScript: `3 % 2`?  
       🔘 `1`   
       🔘 `5`  
       🔘 `NaN`  

It's worth noting that the inline formatting on my Moodle site is different than the formatting of code blocks (see below). 
This may be due to how the Moodle theme (Cascading Style Sheets?) is configured on my Moodle site.

### Code blocks (fences)

I teach courses in software engineering, so many of my quizzes have multi-line examples of code in the stems and the choices (for multiple-choice questions).
It's possible to use markdown for these, but it is extra tricky because of the GIFT control characters that appear in code (especially `{ } : =`), the new lines and indenting.

However, indenting works differently in code blocks in Moodle! Multiple spaces will be ignored when displaying code, unless a [non-breaking space](https://en.wikipedia.org/wiki/Non-breaking_space) "&nbsp;" is the first character of the line. 
Furthermore, the non-breaking space has to be a character, **not** some HTML `&nbsp;` or unicode `U+00A0` specification that can be entered easily by keyboard.

Let's see a complicated 😱 example.

````
::Code blocks in stem and answers::
[markdown]
// Warning: there are some non-breaking spaces (look like normal spaces)
Complete the following TypeScript class so it has a method named `go` 
that accepts a string argument and always returns true.
\n```\nexport class A \{\n    // complete\n\}\n```
{
    =\n```\ngo(a\: string) \{\n    return true;\n\}\n\n```
    ~\n```\ngo(string a) \{\n    return true;\n\}\n```
}
````

The above GIFT with markdown produces a multiple-choice question that displays like this in my Moodle:

[![Preview of imported GIFT question in Moodle, markdown format for code blocks in the question stem and answers]({{site.baseurl}}/img/posts/MoodlePreviewCodeBlocksStemAnswers.png){:class="img-responsive"}]({{site.baseurl}}/img/posts/MoodlePreviewCodeBlocksStemAnswers.png)

To help you as a mere mortal to create the markdown inside GIFT (especially with the non-breaking space in the right place), the VSCode GIFT extension provides a nice feature:
- Select source code you want to format in markdown for GIFT
- **<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> > Gift: Escape code block (Markdown)**

Here's a demo of how I used it to prepare the question above. Look for the non-breaking spaces highlighted 🟧 (see [this VSCode extension](https://marketplace.visualstudio.com/items?itemName=viktorzetterstrom.non-breaking-space-highlighter) but it is not required). Click the animated image below to view the demo in full screen:

[![Escaping code blocks for GIFT markdown in VSCode with the GIFT extension]({{site.baseurl}}/img/posts/EscapingCodeBlocksWithVSCodeGIFT.gif){:class="img-responsive"}]({{site.baseurl}}/img/posts/EscapingCodeBlocksWithVSCodeGIFT.gif)

You can undo the escape operation if you want to change your block of code embedded in the question:  
**<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> > Gift:  Unescape code block (Markdown)**.

### But I don't like tools...

Here's the solution without using the VSCode extension (it's part of a Stack Overflow [answer](https://stackoverflow.com/a/70383459/1168342)). 
Note that I've replaced the non-breaking spaces as `.` at the start of lines that require indenting:

````
::Code blocks in stem and answers ::
[markdown]
Complete the following TypeScript class so it has a method named `go` that
accepts a string argument and always returns true.
```
export class A \{
.   // complete
\}
```
{
=
```
go(a\: string) \{
.   return true;
\}
```
~
```
go(string a) \{
.   return true;
\}
```
}
````

You must escape all control characters and insert the non-breaking space as the first character of each indented line of code blocks. Here's how to enter a non-breaking space on your keyboard:

| OS         | Input method                      |
|------------| ----------------------------------|
| MacOS      | <kbd>Option</kbd>+<kbd>space</kbd> (<kbd>Alt</kbd>+<kbd>space</kbd>)          |
| Linux      | <kbd>Compose</kbd> <kbd>space</kbd> <kbd>space</kbd> or <kbd>AltGr</kbd>+<kbd>space</kbd>|
| Windows    | <kbd>Alt</kbd>+<kbd>0</kbd>+<kbd>1</kbd>+<kbd>6</kbd>+<kbd>0</kbd> ([using the number pad](https://www.freecodecamp.org/news/alt-codes-special-characters-keyboard-symbols-windows-list/))|

Some text editors use <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>space</kbd>.
Some editors will prevent you from typing non-breaking spaces at all (so you might have to copy/paste the character from somewhere else).

### My markdown still doesn't display properly!

When I get stuck 😤, here's how I debug markdown in GIFT with Moodle:

- Try writing a question with markdown format, then import it as GIFT. I use a question category called "GIFT-debugging" for this purpose. 
- If it doesn't preview in Moodle properly, I change it inside of Moodle's editor (making sure that the format is `markdown` in the web editor) until it previews properly. Remember to check the Moodle markdown documentation cited above.
- Export the question to GIFT again (and see what you've learned).

## Conclusion

GIFT can get pretty ugly when you're embedding markdown in it. But I still think it's worth using, especially with the [VSCode extension from Ethan Ou 🎉](https://marketplace.visualstudio.com/items?itemName=ethan-ou.vscode-gift-pack).

If you know of any other gotchas or work-arounds using markdown <i class="fab fa-markdown"></i> in Moodle's GIFT format, please leave a comment!
