# Writing academic papers using Sublime Text 3 + Pandoc

<!-- MarkdownTOC -->

- Prerequisites
    - Sublime Text 3 and plugins
    - Pandoc
- Usage
    - Sublime Text 3 and plugins
        - Adding citations use *Citer*
    - Pandoc
        - Transformation between .md, .docx, and pdf files
        - Adding references
        - Using templates
            - Modify the default docx template
            - Modify the default LaTeX template
        - Make pandoc compatible with Chinese \(Pandoc的中文输出\)

<!-- /MarkdownTOC -->


This post explains how to write academic papers using *Sublime Text 3* in *markdown* format and transform the .md file into .docx and pdf files using *pandoc*.

## Prerequisites

To write academic papers in *markdown* format requires a **markdown editor**, a **format trnasformer**, maybe also a **literature manager**. I use *Sublime Text 3* as the markdown edit, *Pandoc* as the format transformer, and *Zotero* as the literature manager. This section lists the things we need.

### Sublime Text 3 and plugins
[Sublime Text 3](https://www.sublimetext.com/3) is a text editor that can be used for code, markup and prose. It is lightweight and is suitable for long texts. The followings are several plugins (or packages) we need:

- [MarkdownEditing](http://ttscoff.github.io/MarkdownEditing/): writing markdown in Sublime Text 3;
- [MarkdownPreview](https://github.com/facelessuser/MarkdownPreview): previewing markdown in the browser;
- [SmartMarkdown](https://github.com/demon386/SmartMarkdown): folding and unfolding Headings;
- [Citer](https://github.com/mangecoeur/Citer): citing from a bibtex file;
- [AcademicMarkDown](https://github.com/mangecoeur/AcademicMarkdown): highlighting citations and CriticMarkup;
- [WordCount](https://github.com/titoBouzout/WordCount): counting words and characters;
- [CriticMarkup](http://criticmarkup.com/): highlighting revisions (e.g. add, delete, substitute, and comment).

To install packages in Sublime Text 3, first install *Package Control* (go to Preferences -> Install Package Control). Use `Ctrl+Shift+P` to activate the package control palatte, type `Install Packages`, select and then enter the names of whatever packages you want to install.

### Pandoc
[Pandoc](http://pandoc.org/) is a powerful tool for transforming file formats. Installing it on windows by downloading the .exe file or installing it in MacOS by simply typing `brew install pandoc` and  `brew install pandoc-cite-proc` in the Terminal (given that you have installed [HomeBrew](https://brew.sh/)).

## Usage
This section explains how 1) to use Sublime Text 3 to complete tasks like adding citations, 2) to tranform .md files to .docx and pdf files.

### Sublime Text 3 and plugins
It is common tasks to add citations, insert tables, and insert figures when writing academic papers. Sublime Text 3, along with various packages, provides us with powerful tools to compelete such tasks.

#### Adding citations use *Citer*
Once we have installed Citer via Package Control, we can modify the settings of Citer. Add the following lines into the user settings (Preferences -> Package Settings -> Citer -> Citer Setting-User), and change the "bibtex_file_path" parameter according to the path of your bibtex files. Multiple bibtex files can be add to the `bibtex_file_path` separated by a comma and surrounded with brackets (`[]`).

```
{
    //REQUIRED:

    "bibtex_file_path": "your bibtex path",

    //OPTIONAL:
  
    //By default Citer Search looks for your keyword in the 
    //author, title, year, and Citekey (id) fields
    "search_fields": ["author", "title", "year", "id"] ,
    //Default format is @Citekey
    "citation_format": "@%s",
    //list of scopes. Could be top level "text" or "source", or limit to
    // e.g "text.html.markdown"
    "completions_scopes": ["text"],
    "enable_completions": true
}
```

Save the setting file. Type `Ctrl-Shift-P` and the Package Control palette pops up. Type `show all`, and select the command `Citer: Show all`, all the entries in the bibtex files should pop up. Select the entry you would like to cite, and press `Enter`. The entry appears in the file in this form `@author1990` (@authorYear).

### Pandoc
Use Pandoc is easy and to modify the templates is rather challenging, especially if you wish to modify the LaTeX template. It implies that you know fairly well about LaTeX. To make Pandoc compatible with Chinese characters is another challenging task. Pandoc is a command line tool, so make sure you enter all the commands in the terminal (or `cmd` in Windows) 

#### Transformation between .md, .docx, and pdf files
Transforming .md files into .docx and .pdf files is straightforward. Go to the path where your .md files are, and type in the following commands:

```bash
cd path                              # change directory to where your md files are                  
pandoc file.md -o file.pdf           # md -> pdf
pandoc file.md -o file.docx          # md -> docx
```

#### Adding references
Adding references to a markdown file is also straightforward, simply use the `pandoc-citeproc` program, and specify the path of the bibtex file:

```bash
# md -> pdf
pandoc --filter pandoc-citeproc --bibliography=file.bib file.md -o file.pdf
# md -> docx
pandoc --filter pandoc-citeproc --bibliography=file.bib file.md -o file.docx
```

#### Using templates
You can output .md files to .docx and pdf files with various formats by modifying the default templates (Please also refer to the [*Pandoc User's Guide*]()).

##### Modify the default docx template
To modify the default docx template, first print the default dicx template used by Pandoc with the following command:

```bash
pandoc --print-default-data-file reference.docx > custom-reference.docx 
```

To modify the default docx reference file, it does not work to simply change the style of the contents. Rather, define the styles you would love to use in the Style options provided by MS Word:

![Style options in MS word](C:/Users/Hu/Desktop/Style_MS_word.png)

To use the sepcifies reference file:

```bash
pandoc file.md -o file.docx --reference-doc=custom-reference.docx
```

##### Modify the default LaTeX template

#### Make pandoc compatible with Chinese (Pandoc的中文输出)
To make pandoc compatible with Chinese, we have to specify the pdf engine and the font (this is not required when transforming to .docx):

```bash
pandoc --pdf-engine=xelatex -V mainfont="SimSun" file.md -o file.pdf 
```

To use whatever font you like, type in the font name after `mainfont=`. To know what fonts you have in your laptop, type `fc-list :lang=zh` in Windows cmd, or look up in *Font Book* in MacOS.

There is another serious problem with Chinese pdf files created with pandoc. It cannot automatically change line. So a Chinese pdf file simply spans beyond the range of the current file. To overcome this problem, we need to modify the default LaTeX template. 