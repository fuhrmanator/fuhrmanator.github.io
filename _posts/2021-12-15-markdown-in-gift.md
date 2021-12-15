---
layout: post
comments: true
published: false
title: Getting the most of markdown in GIFT
header-img: img/posts/questions.jpg
background: '/img/posts/questions.jpg'
---

## Getting the most of markdown in GIFT

Markdown is a format supported in GIFT, which can be used for simple formatting text in a question, in answers and feedback:

```gift
::Simple markdown::
[markdown]If you're standing somewhere on the quator, the **sun** rises in the\: {=*east*#**Super!** ~*west* ~*north* ~*south*}
```

But, formatting code blocks or nested lists in Moodle requires some deeper understanding, because of differences in formats between GIFT and markdown. The solutions complicate the formatting of GIFT files, so it's recommended to use a smart editor, such as the [GIFT Format Pack extension in VSCode](https://marketplace.visualstudio.com/items?itemName=ethan-ou.vscode-gift-pack).

### Moodle markdown is not GitHub markdown

Please see [Moodle's documentation on markdown](https://docs.moodle.org/311/en/Markdown) for various specificities. There are significant differences from GitHub's markdown (or other flavors), so it's best to look at the Moodle documentation.

### GIFT's control characters

Even when specifying a question stem in markdown format, you might still have to escape GIFT's control characters if they're part of the question stem (or answer or feedback). For example:

```gift
::Escaped character that is special::
[markdown]Two plus two\: {=four =4}
```

Notice the `\:` inside the question stem. This is necessary because `:` is one of [GIFT's special characters `~ = # { } :`](https://docs.moodle.org/311/en/GIFT_format#Special_Characters_.7E_.3D_.23_.7B_.7D), which generally must be escaped. Again, tools will help with this.

### Line breaks

In GIFT, line breaks are just white space and don't mean anything (unless there are two in succession - a blank line - which means a new question is coming). 
Since, markdown *does* use line breaks as part of its formatting, there's a way to specify them inside of GIFT. The Moodle parser will convert the text `\n` into a line break (for markdown) after the GIFT import. Here's an example:

```
::Multi-line question stem::
[markdown]How much is\n\n2 + 2?{=four =4}
```

This should produce a question with the line breaks as follows:

>How much is
>
>2 + 2

### Indentation

Markdown is sensitive to white space when it comes to indentation. For example, nested lists must be indented:

```markdown
- Item A
  - Item A.1
  - Item A.2
- Item B
```

In the example above, there's actually the need to specify line breaks *and* indentation. Also, [Moodle's documentation on markdown](https://docs.moodle.org/311/en/Markdown#Bullet_point_lists) states it uses `*` as bullet list character.

Here's an example of a nested, bulleted list in markdown that displays properly inside of Moodle (v.311):

```
::Nested List::
[markdown]This is a nested list\:\n\n* Item A\n  * Item A.1\n  * Item A.2\n* Item B{TRUE}
```

> Here's a (somewhat tedious) heuristic I used to debug markdown in GIFT with Moodle:
> - try a question with markdown format then import it as GIFT,
> - if it doesn't preview properly, change it inside of Moodle's editor (making sure that the format is still `markdown` in the web editor) until it previews properly
> - export the question to GIFT again

### Inline code

This formatting is very straightforward. Here's an example:

```gift
::Inline code question::
[markdown]What's the result of the following expression in JavaScript\: `3 % 2`? {=`1` ~`5` ~`NaN`}
```

This makes a question that looks something like:

> What's the result of the following expression in JavaScript: `3 % 2`?
> * `1` 
> * `5`
> * `NaN`

### Code blocks (fences)

I teach courses in software engineering, so many of my quizzes have snippets of code in the stems and the choices (for multiple-choice questions). It's possible to use markdown for these, but it is extra tricky because of the control characters, the new lines and indenting.

There's an additional twist with code blocks in Moodle with respect to indenting: multiple blank spaces will be ignored when displaying code, unless a [non-breaking space](https://en.wikipedia.org/wiki/Non-breaking_space) "&nbsp;" is the first character of the line. Furthermore, the non-breaking space has to be a character, **not** some HTML `&nbsp;` or unicode `U+00A0` specification that can be entered by keyboard.

Let's see a complicated example:

````
::Code blocks in stem and answers::
[markdown]
Complete the following TypeScript class so it has a method named `go` that accepts a string argument and always returns true.
\n```\nexport class A \{\n    // complete\n\}\n```
{
    =\n```\ngo(a\: string) \{\n    return true;\n\}\n\n```
    ~\n```\ngo(string a) \{\n    return true;\n\}\n```
}
````

The above GIFT with markdown produces a multiple-choice question that displays like this in my Moodle:

[![Preview of imported GIFT question in Moodle, markdown format for code blocks in the question stem and answers]({{site.baseurl}}/img/posts/MoodlePreviewCodeBlocksStemAnswers.png){:class="img-responsive"}]({{site.baseurl}}/img/posts/MoodlePreviewCodeBlocksStemAnswers.png)

To help you to format the markdown inside GIFT, the VSCode GIFT extension provides a nice feature:
- Select source code you want to fromat in markdown for GIFT
- **Ctrl-Shift-P > Gift: Escape code block (Markdown)**

Here's a demo of how I used it to prepare the question above:

[![Escaping code blocks for GIFT markdown in VSCode with the GIFT extension]({{site.baseurl}}/img/posts/EscapingCodeBlocksWithVSCodeGIFT.gif){:class="img-responsive"}]({{site.baseurl}}/img/posts/EscapingCodeBlocksWithVSCodeGIFT.gif)

You can "undo" the operation (if you want to change you code block embedded in the question) by using the **Ctrl-Shift-P > Gift:  Unescape code block (Markdown)**.

## Conclusion

GIFT can get pretty ugly when you're embedding markdown in it. But I still think it's worth using, especially with the VSCode extension.
If you know of any other gotchas or work-arounds using markdown in GIFT, please leave a comment!
