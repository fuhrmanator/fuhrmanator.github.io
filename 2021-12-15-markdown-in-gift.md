## Making the most of markdown in GIFT

Markdown is a format supported in GIFT, but using it in Moodle requires some understanding. These things unfortunately complicate the formatting of GIFT files, so it's recommended to use a smart editor, such as the [GIFT Format Pack extension in VSCode](https://marketplace.visualstudio.com/items?itemName=ethan-ou.vscode-gift-pack). 

### Line breaks

GIFT uses two blank lines to separate questions. Otherwise line breaks are just white space and don't mean anything. However, markdown uses line breaks as part of its formatting, so there's some need to specify them inside of GIFT when using markdown. Luckily the Moodle parser will convert the text `\n` into a real line break after a GIFT import. Here's an example:

```
[markdown]How much is\n\n2 + 2?{=four =4}
```

Which should produce a question with the line breaks as follows:

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

In the example above, there's actually the need to specify line breaks *and* indentation. The problem with Moodle, however, is that it will supress multiple blank spaces during an import, unless a [non-breaking space](https://en.wikipedia.org/wiki/Non-breaking_space) "&nbsp;" is the first character of the line.

To do it in GIFT, you need to actually paste a non-breaking space (I'm not aware of how one can use `&nbsp;` or some other textual represenation inside GIFT):

```
[markdown]This is a nested list:\n\n- Item A\n  - Item A.1\n  = Item A.2\n-Item B{TRUE}
```
