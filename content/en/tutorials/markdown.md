---
title: "Basics of Markdown"
description: "Futureproof your writing"
date: 2020-01-28T00:10:37+09:00
draft: false
weight: 1
---

Have you ever fought with Word or another word processor, trying to get things just right? Word processing is a mess. It conflates writing with typesetting and layout. Sometimes, you just want to get the words out. Other times, you want to make your writing as accessible as possible... but your intended recipient can't open your file, because they don't use the same word processor. Or perhaps you wrote up some great notes that you'd love to have in a slideshow; but you can't, because copying and pasting preserves a whole lot of extra gunk that messes up your materials.

The answer is to separate your content from your tool.

This is where the **Markdown syntax shines**. Markdown is a syntax for marking semantic elements within a document explicitly, not in some hidden layer. The idea is to identify units that are meaningful to humans, like titles, sections, subsections, footnotes, and illustrations. At the very least, your files will always remain comprehensible to you, even if the editor you are currently using stops working or "goes out of business."

Here are the main conventions:

**Headers**

`# A Level 1 Heading`

`## A Level 2 Heading`

`### A Level 3 Heading`

**Bullets**

`+ a bullet point`

`1. a numbered bullet point`

**Emphasis**
`_Text_`
	Displays text in italics
`**Text**`
	Displays the text in bold
`**_Text_**`
	Displays the text in bold and italics
`~~Text~~`
  Displays the text with a strikethrough effect

**Links & Images**

`[text link](https://duckduckgo.com)`

`![alt text](https://github.com/n48.png)`

Writing in this way liberates the author from the tool. Markdown can be written in any plain text editor. When you add a `.md` file to Github, Github understands the conventions and turns your plain text into a properly formatted webpage in html. Markdown is currently enjoying a period of growth, not just as as means for writing scholarly papers but as a convention for online editing in general.

NB A text editor is different from the default notepad app that comes with Windows or Mac. A text editor shows you exactly what is in a file, including tags, code, and other 'hidden' markup. That's why I suggest you install [Sublime Text](https://sublimetext.com) or [Atom](https://atom.io).

In this exercise, I want you to become familiar with Markdown syntax. (Feel free to check out [Sarah Simpkin's quick primer on Markdown](http://programminghistorian.org/lessons/getting-started-with-markdown)).

Have the [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) open in a new window.

1. Make a new subfolder in your hist3814 working folder, on your machine; you'll save your work into that folder. Open your text editor.

2. Using markdown conventions, write a short paragraph on your favourite piece of academic work you've completed so far at Carleton. Save it in that new folder. The important thing here is to use a variety of markdown conventions so that you get used to the idea.

3. Grab at least two Creative Commons images. The Creative Commons license allows re-use. You can [search for Creative Commons licensed images here](https://search.creativecommons.org/). Download them to your computer, and put them in the folder you are working in. Then use a markdown image link to insert them into your document. **Nb** the images won't appear just yet, so don't worry. An image link for an image in the same folder as the markdown file will look something like this:

`![title of the picture if you know it](image-filename.png)`

4. Add a few hyperlinks to relevant websites.

5. Make sure that when you save your document, you use the `.md` file extension.

6. Push your folder to [github](/tutorials/github) once you've got your repository set up; you'll need to complete the [github tutorial](/tutorials/github).

Moving forward, I want you to keep your log.md and your reflection.md files using these markdown conventions.
