---
title: LaTeX Crash Course
date: '2024-11-06'
author: Ezra Natanael
categories:
  - LaTeX
slug: latex-crash-course
toc: true
---

## Introduction

LaTeX is a typesetting system widely used for academic and scientific documents. Its popularity comes from its powerful control over document layout, especially for complex structures like mathematical formulas, tables, and references. In this crash course, we’ll explore the essentials of LaTeX, from basic setup to creating professional documents.

## Why Use LaTeX?

LaTeX is ideal for anyone working on:

- Academic papers, theses, or dissertations
- Mathematical or scientific documents
- Professional presentations and reports
- Documents with precise formatting needs

LaTeX shines with complex documents due to:

- High-quality typesetting
- Automatic numbering and referencing
- Consistent formatting
- Extensive package support for customization

## Getting Started with LaTeX

### Installation

To start with LaTeX, you need a LaTeX distribution and an editor.

- Distribution: Install a LaTeX distribution like [TeX Live](https://www.tug.org/texlive/acquire-netinstall.html) (cross-platform) or [MikTeX](https://miktex.org/download) (Windows)
- Editor: Use an editor with LaTeX support. Popular choices include:

    - [Overleaf](https://www.overleaf.com/): An online LaTeX editor with collaboration features.
    - [TeXmaker](https://www.xm1math.net/texmaker/download.html): Free and cross-platform.
    - [VS Code](https://code.visualstudio.com/Download): Install the LaTeX Workshop extension.

For the sake of simplicity, we'll be using [Overleaf](https://www.overleaf.com/) for this course. Head over to their website and sign up for a free account!

### Writing Your First Piece of LaTeX

Create a new project, then choose **Blank Project** and enter your project name. In this course, I'll name it "Tutorial". Once you create the project, you'll see that several lines of LaTeX code are automatically generated for you. We'll delete that and start from scratch.

Copy this code and paste it into your Overleaf editor:

```latex
\documentclass{article}
\begin{document}
First document. This is a simple example, with no 
extra parameters or packages included.
\end{document}
```

Once that's done, click the "Recompile" button at the top. You'll see the result on the document on the right.

The code above produces the following output:

![First Document](/images/first-document.png)

You can observe that LaTeX has automatically indented the first line of the paragraph, handling that formatting for you. Now, let’s examine what each component of our code does in more detail.

The line of code `\documentclass{article}` specifies the document class, determining its overall appearance. Different documents, such as a CV or a scientific paper, require different classes (e.g., book or report). For a comprehensive list of available LaTeX classes, refer to the [CTAN (Comprehensive TeX Archive Network)](https://www.ctan.org/topic/class).

After specifying the document class, the content of the document is placed between the `\begin{document}` and `\end{document}` commands. You can edit the text within this section and **recompile** the document to view the typeset PDF.

**Mini Task!**

> Try displaying your name, where you are from, and why you want to learn LaTeX!

That's a wrap! Congratulations on writing your first LaTeX document!

## The Preamble

A LaTeX document is stored by Overleaf as a file named `main.tex`. Files containing your document's LaTeX code are often named with the `.tex` file extension.

The preamble is the portion of your `.tex` file that appears **before** the `\begin{document}` command; it serves as the document's "setup" section. The preceding example demonstrated how document content was entered after that point. Define the document class (type) in the preamble, along with details like the languages to be used for authoring the document, the packages you want to load (more on this later), and other configuration kinds.

A minimal document preamble might look like this:

```latex
\documentclass[12pt, letterpaper]{article}
\usepackage{graphicx}
```

Where `\documentclass[12pt, letterpaper]{article}` specifies the document's general class (type). Square brackets `([...])` include additional parameters that are used to customize this instance of the article class, i.e., settings we want to apply for this specific *article-class-based* document. These parameters must be separated by commas.

In this example, the two parameters do the following:

- 12pt sets the font size
- `letterpaper` sets the paper size

Other font sizes, such as 9pt, 11pt, or 12pt, can be used, of course, but 10pt is the **default** if none is given. A4 paper and legal paper are additional options for the paper size. 

The preamble line

```latex
\usepackage{graphicx}
```

is an example of loading an external package (here, `graphicx`) to extend LaTeX capabilities, enabling it to import external graphics files. LaTeX packages are discussed in the section [Finding and using LaTeX packages]().

## Including the Title, Author, and Date

Three extra lines must be added to the **preamble** (not the body of the document) in order to include the title, author, and date. These are the lines:

```latex
\documentclass[12pt, letterpaper]{article}
\title{My first LaTeX document}
\author{Hubert Farnsworth\thanks{Funded by the Overleaf team.}}
\date{August 2022}
```

Let's explore the code more briefly.

1. `\title{My first LaTeX document}`
   -  Defines the document title
2. `\author{Hubert Farnsworth}`
   - Specifies the author's name
   - Can include the `\thanks` command within curly braces:
     - `\thanks{Funded by the Overleaf team.}`: Adds a superscript and footnote containing acknowledgment text. Useful for crediting supporting institutions.
3. `\date{August 2022}`
   - Manually enters the date
   - Alternatively, use the `\today` command to display the current date when the document is compiled

To typeset the title, author and date use the `\maketitle` command within the *body* of the document:

```latex
\begin{document}
\maketitle
We have now added a title, author and date to our first \LaTeX{} document!
\end{document}
```

The preamble and body can now be combined to produce a complete document which can be opened in Overleaf:

```latex
\documentclass[12pt, letterpaper]{article}
\title{My first LaTeX document}
\author{Hubert Farnsworth\thanks{Funded by the Overleaf team.}}
\date{August 2022}
\begin{document}
\maketitle
We have now added a title, author and date to our first \LaTeX{} document!
\end{document}
```

Run this code and compile it into your own document! Make sure to change the title, author, and date according to your own preferences.'

## Adding Comments

LaTeX is a typesetting program code that can be useful for document typesetting. It allows for the inclusion of comments, which are non-typeset sections of text, which can be used to add notes, provide explanations, or debug code.

To make a comment in LaTeX, use the `%` symbol at the beginning of the line, as demonstrated in the code example provided.

```latex
```latex
\documentclass[12pt, letterpaper]{article}
\title{My first LaTeX document}
\author{Hubert Farnsworth\thanks{Funded by the Overleaf team.}}
\date{August 2022}
\begin{document}
\maketitle

% This is a comment! It will not be typeset in your document.
\end{document}
```

The example generates the same output as the previous LaTeX code, which did not include the comment.

## Bolds, Italic, and Underlining

We'll now examine a few text formatting commands:

- **Bold**: The `\textbf{...}` command is used to typeset bold text in LaTeX.
- *Italics*: the `\textit{...}` command is used to create italicized text.
- <u>Underline</u>: Use the `\underline{...}` command to underline text.
  
The next example demonstrates these commands:

```latex
Some of the \textbf{greatest}
discoveries in \underline{science} 
were made by \textbf{\textit{accident}}.
```

This example produces the following output:

![Bold Italic Underline](/images/bold_italic_underline.png)

`\emph{argument}` is another very helpful command whose effect on its argument depends on the context. The emphasized text is italicized when it is inside regular text, while the opposite occurs when it is inside italicized content (see the following example):

```latex
Some of the greatest \emph{discoveries} in science 
were made by accident.

\textit{Some of the greatest \emph{discoveries} 
in science were made by accident.}

\textbf{Some of the greatest \emph{discoveries} 
in science were made by accident.}
```

This example produces the following output:

![Emphasis](/images/emph.png)

## Adding Images

We will examine the process of adding images to a LaTeX document in this section. There are three methods for adding photos to Overleaf:

1. Use the [**Insert Figure** button](https://learn.overleaf.com/learn/Kb/How_to_insert_figures_in_Overleaf#Using_Insert_Figure_to_add_a_figure_to_your_project)(![The Insert Figure button on the editor toolbar](https://sharelatex-wiki-cdn-671420.c.cdn77.org/learn-scripts/images/a/a8/InsertFigureButton.png)), located on the editor toolbar, to insert an image into **Visual Editor** or **Code Editor**.
2. [Copy and paste an image](https://www.overleaf.com/learn/how-to/How_to_paste_formatted_content_into_Overleaf%23Pasting_images_into_your_Overleaf_project "Kb/How to paste formatted content into Overleaf") into **Visual Editor** or **Code Editor**.
3. Use **Code Editor** to write LaTeX code that inserts a graphic.

Option 3 is introduced here; keep in mind that you will need to add those images to your Overleaf project. Options 1 and 2 automatically generate the LaTeX code needed to include images. The example that follows shows how to add a picture:

```latex
\documentclass{article}
\usepackage{graphicx} %LaTeX package to import graphics
\graphicspath{{images/}} %configuring the graphicx package
 
\begin{document}
The universe is immense and it seems to be homogeneous, 
on a large scale, everywhere we look.

% The \includegraphcs command is 
% provided (implemented) by the 
% graphicx package
\includegraphics{universe}  
 
There's a picture of a galaxy above.
\end{document}
```

This example produces the following output:

![Image](/images/images.png)

An add-on package that offers the functionality and commands necessary to incorporate external graphics files is required when importing graphics into a LaTeX document. The aforementioned example loads the `graphicx` package, which among many other commands, offers `\graphicspath{...}` to tell LaTeX where the graphics are located and `\includegraphics{...}` to import graphics.

Add the following line to the preamble of your Overleaf document in order to use the `graphicx` package:

```latex
\usepackage{graphicx}
```

The command `\graphicspath{{images/}}` in our example tells LaTeX that images are stored in the current directory's images folder:

The image is really inserted into the page with the `\includegraphics{universe}` command. In this case, the image file's name is universe, but it does not have an extension.

Take note: 

> - Although the `\includegraphics` command allows the complete file name, including its extension, it is often advised to leave the file extension out  as this will cause LaTeX to look for all acceptable formats.
> - When uploading picture files to Overleaf, it is generally advised to use lowercase letters for the file extension and to avoid using white spaces or multiple dots in the graphic's file name.