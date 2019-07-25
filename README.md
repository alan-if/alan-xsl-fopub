# XSL FOP/PDF Template for Alan IF

A customized DocBook XSL template and the required fonts for building Alan documentation in PDF format via [asciidoctor-fopub].

- https://github.com/alan-if/alan-xsl-fopub


-----

**Table of Contents**

<!-- MarkdownTOC autolink="true" bracket="round" autoanchor="false" lowercase="only_ascii" uri_encoding="true" levels="1,2,3" -->

- [Repository Contents](#repository-contents)
- [Introduction](#introduction)
- [Features](#features)
    - [Syntax Highlighting](#syntax-highlighting)
    - [PDF Samples](#pdf-samples)
- [Tech Specs](#tech-specs)
- [Instructions](#instructions)
    - [Dependencies](#dependencies)
        - [Asciidoctor-fopub](#asciidoctor-fopub)
    - [Git Setup](#git-setup)
        - [Update Strategy](#update-strategy)
    - [Using the Template with Asciidoctor](#using-the-template-with-asciidoctor)
        - [Example Script](#example-script)
        - [AsciiDoc Usage](#asciidoc-usage)
- [License](#license)
    - [Fonts](#fonts)
    - [DocBook Template](#docbook-template)
- [Reference Links](#reference-links)
    - [Asciidoctor/FOP](#asciidoctorfop)
    - [XSLTHL Syntax Highlighting](#xslthl-syntax-highlighting)
    - [DocBook](#docbook)
    - [DocBook XSL Stylesheets](#docbook-xsl-stylesheets)
    - [Git Submodules](#git-submodules)

<!-- /MarkdownTOC -->

-----

# Repository Contents

- [`/fonts/`][fonts] — required third party fonts.
- [`/xsl-fopub/`][xsl-fopub] — DocBook XSL template for Alan.
- [`XSL-DevNotes.md`][XSL-DevNotes] — misc. XSL work notes and links.

# Introduction

This DocBook PDF template was originally created for the [Alan Docs] project, a repository for the documentation of the [Alan Interactive Fiction] authoring system. Since other Alan-related repositories are now switching to the AsciiDoc format for their documentation, we decided to move the DocBook XSL PDF template and its assets to an independent repository to allow other projects to include it via [Git submodules].

Hopefully, a shared template will reduce the maintainance burden and maximize the collaborative effort among the various Alan projects. The Alan community is a small niche, with limited active members and resources, so our strength lies in tight collaboration and sharing of resources.

Working with XSL stylesheets is not an easy task, so providing a ready-to-use template taylored for Alan documentation is a good service to the Alan community, who can then focus its energy on writing contents.

# Features

The template was customized to resemble the original layout and styles of the _[Alan Manual]_. The goal is to create a dedicated PDF template shared by all Alan-related documentation.

The template was built from the original XSL stylesheets of the [asciidoctor-fopub] distribution, by [Dan Allen]  (MIT License).

## Syntax Highlighting

One of the key features of this template is our custom Alan syntax definition for [XSLTHL], the FOP syntax highlighter that ships with [asciidoctor-fopub], and a dedicated color scheme for Alan code examples blocks.

- [`xsl-fopub/xslthl/alan-hl.xml`][alan-hl] — Alan syntax definition.

[alan-hl]: ./xsl-fopub/xslthl/alan-hl.xml "View source file"

## PDF Samples

For a preview of how the PDF documents generated using this template look like, refer to the following PDF samples from the [Alan Docs] project:

- [`tests-syntax-highlighting.pdf`][tests-syntax-highlighting.pdf]
- [`tests-typography.pdf`][tests-typography.pdf]
- [`styles-preview.pdf`][styles-preview.pdf]

These PDFs contain detailed examples of all the template features, with instructions on how to use the template styles in your AsciiDoc project.

# Tech Specs

Asciidoctor-fopub uses the following components versions:

| Software Project            | Version |
| :-------------------------- | :------ |
| Apache FOP                  | 2.1     |
| DocBook XSL                 | 1.78.1  |
| Apache Commons XML Resolver | 1.2     |
| Xalan                       | 2.6.0   |
| JEuclid                     | 3.1.9   |
| XSLTHL                      | 2.1.0   |
| Gradle                      | 2.0     |

# Instructions

Setting up the [asciidoctor-fopub] PDF toolchain, and using this template as a Git submodule, require a few careful steps. I'll provide some helpful notes to simplify the whole process, especially for Windows users.

## Dependencies

This DocBook template is designed to work with Asciidoctor and [asciidoctor-fopub]. You'll need to install the following tools on your machine:

- [Ruby] and the following Ruby Gems:
    + [Asciidoctor]
    + [asciidoctor-fopub]
- [Java JDK12]

Windows users should install Ruby via [RubyInstaller], or using [Chocolatey]/[Chocolatey GUI] and the [Chocolatey Ruby] package.

### Asciidoctor-fopub

- https://github.com/asciidoctor/asciidoctor-fopub

The AsciiDoc to PDF toolchain requires setting up asciidoctor-fopub on your machine; this tool is required to convert from DocBook to PDF.

Here are some instructions on how to setup asciidoctor-fopub:

1. __JDK12__ — Download and install [Java JDK12].

    If you have other versions of Java SE/JDK uninstall them. In order to use asciidoctor-fopub you'll need Java JDK 12 (versions 6, 7 and 8 are also known to work, but are not recomended for security reasons).

    > __NOTE__ — The previous [incompatibility problems with gradle] that prevented using Java >=9 is now fixed in [asciidoctor-fopub] and all users are now advised to update to the the latests JDK, and stop using JDK 8!

2. __Clone asciidoctor-fopub__ — There is no installation for this tool, just clone it somewhere on your hard disk using Git:

    ```
    git clone https://github.com/asciidoctor/asciidoctor-fopub
    ```

    On Windows, I also had to carry out the following one-time operation to stop some gradle errors that were preventing using asciidoctor-fopub:

    - Open the CMD in the cloned asciidoctor-fopub folder, and type:

        ```
        gradlew installapp
        ```

        This (undocumented) step seems necessary to complete the gradle setup of asciidoctor-fopub.

3. __Add to PATH__ —  After cloning asciidoctor-fopub locally, you should add its path to your system PATH in order to make it available to the scripts in this project.

See also the [setup instructions found at the asciidoctor-fopub] repository.

[setup instructions found at the asciidoctor-fopub]: https://github.com/asciidoctor/asciidoctor-fopub#prerequisites
[Java JDK12]: https://www.oracle.com/technetwork/java/javase/downloads/jdk12-downloads-5295953.html
[incompatibility problems with gradle]: https://github.com/asciidoctor/asciidoctor-fopub/issues/87


## Git Setup

The easiest way to include this template into your project and keep it up to date is via [Git submodules]:

> Submodules allow you to keep a Git repository as a subdirectory of another Git repository. This lets you clone another repository into your project and keep your commits separate.
> 
> — _[Pro Git]_ book, [§7.11 - Submodules][Git submodules]

To add the template as a submodule within your project, use the `git submodule add` command:

    $ git submodule add https://github.com/alan-if/alan-xsl-fopub

This will clone the template subproject into a directory named the same as the repository (i.e. `alan-xsl-fopub/`). If you wish to use a different folder name, just specify it after the repository url.

Don't forget to initialize the submodules:

    $ git submodule update --init

For more info on working with submodules avoiding common pitfalls (_beware, there are plenty of them_), see the references linked at the end of this document.

### Update Strategy

You can safely keep the submodule in your project pointing to the tip of `master` branch in this repository — development of the template will occur in dev branches, and every commit in `master` will always be a fully working template.

We'll strive to keep all template updates backward-compatible, but since this can't be guaranteed 100%, you're strongly adviced to consult this document for the list of changes before moving the pointer of your submodule in your project.

Currently we haven't decided a formal update strategy for this project: it doesn't employ a versioning scheme and there are no releases. Until further notice, feel safe to always update your submodule to the tip of `master` branch. 

In the future we might adopt SemVer schemed releases, which could simplify intuitively detecting backward-breaking changes and manually updating the submodule pointer accordingly. Any proposal and advice on this topic is most welcome; feel free to [open an Issue].

## Using the Template with Asciidoctor

In order to use these XSL Stylesheets, your invoking script must set the working path to the root folder of this submodule/repository (i.e. "`alan-xsl-fopub/`", unless you choose a different folder name). This is due to the fact that the "[`fop-config.xml`][fop-config.xml]" configuration file relies on relative paths to locate the fonts folder:

```xml
        <!-- Register all the fonts found in this directory -->
        <directory recursive="true">fonts</directory>
```

Therefore, the invoking script must CD to the "`alan-xsl-fopub/`" folder when invoking [asciidoctor-fopub]. This also means that you'll have to use an absolute path for the DocBook source file, and that you'll only need to pass `-t alan-xsl-fopub` to inform asciidoctor-fopub about where to find the XSL Stylesheets.

### Example Script

As a reference example, we'll use the _[Alan Manual]_ PDF toolchain from the [Alan Docs] project. Study the [`manual/PDF_BUILD.bat`][PDF_BUILD.bat] batch script used in Alan Docs to create the PDF version of the _Alan Manual_. You'll notice that it stores the current directory and then swtiches to the "`_assets/alan-xsl-fopub/`" folder before invoking asciidoctor-fopub.

[PDF_BUILD.bat]: https://github.com/alan-if/alan-docs/blob/master/manual/PDF_BUILD.bat "View the batch script on the Alan Docs project"

Here is an exemplified version of that script, with comments:


```batch
:: "manual\PDF_BUILD.bat"
:: --------------------------------------------------
:: This batch is invoked inside the "manual\" folder.
:: The Alan XSL template repository is submoduled in:
::    _assets\alan-xsl-fopub\
:: --------------------------------------------------

:: Preseve current directory:
SET "CURRDIR=%CD%"

:: Define path to DocBook XSL Template folder (this repo as Git submodule):
SET "FOPUB_DIR=..\_assets\alan-xsl-fopub\"

:: Covnert from AsciiDoc to DocBook via Asciidoctor:
CALL asciidoctor^
  -b docbook^
  -d book^
  -a data-uri!^
  --safe-mode unsafe^
  --verbose^
  manual.asciidoc

:: Need to switch working directory to DocBook XSL Template for FOP:
CD %FOPUB_DIR%

CALL fopub^
  -t alan-xsl-fopub^
  %CURRDIR%\manual.xml

:: Restore origignal script working directory:
CD %CURRDIR%
EXIT /B
```

I couldn't find a more elegant solution to handle an XSL Stylesheets folder used by different documents and inside a repository shared across different machines — but the current solution isn't complicate to use.

### AsciiDoc Usage

For guidelines on how to use the template in your documents refer to the [PDF sample files](#pdf-samples).

# License

The DocBook template and the bundled fonts have separate licenses.

## Fonts

- [`/fonts/`][fonts]

|       Font Name       |             Author            |     License     |
|-----------------------|-------------------------------|-----------------|
| [Fira Sans Condensed] | Carrois Apostrophe / Mozzilla | [SIL Open Font] |
| [Noto Sans]           | Steve Matteson                | [Apache 2.0]    |
| [Roboto Mono]         | Christian Robertson / Google  | [Apache 2.0]    |

The `LICENSE` file of each font can be found inside its subfolder.


## DocBook Template

The XSL stylesheets inside the [`/xsl-fopub/`][xsl-fopub] directory are released under MIT License:

- [`/xsl-fopub/LICENSE`][LICENSE]

The original XSL stylesheets in this folder tree were taken from the "`/src/dist/docbook-xsl/`" folder of the [asciidoctor-fopub] project, Copyright © 2013 [Dan Allen]  (MIT License):

- https://github.com/asciidoctor/asciidoctor-fopub/tree/9afeab8/src/dist/docbook-xsl

They were subsequently edited by [Tristano Ajmone] on behalf of the [Alan Interactive Fiction Development team] to adapt them to the styling needs of Alan documentation. The adaptation work may include deleting some of the original files, as well as adding new ones.

```
The MIT License

Copyright (C) 2019 Alan Interactive Fiction Development team
Copyright (C) 2018, 2019 Tristano Ajmone
Copyright (C) 2013 Dan Allen

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```


-------------------------------------------------------------------------------

# Reference Links

## Asciidoctor/FOP

- [asciidoctor-fopub] — at GiHub.
- [Apache™ FOP][FOP] (Formatting Objects Processor)
    + [Apache™ FOP v2.1]:
        * [FOP Configuration]
        * [FOP Fonts]

## XSLTHL Syntax Highlighting

- [XSLTHL]  (XSLT syntax highlighting) — at Sourceforge.
- [XSLTHL Wiki] — at Sourceforge.

## DocBook

- [DocBook 5: The Definitive Guide] — (DocBook 5.0) by Norman Walsh, online book.
- [DocBook Wiki] — (rebooted) on GitHub.
- [Wikipedia » DocBook][WP DocBook]

## DocBook XSL Stylesheets

- [DocBook XSL: The Complete Guide] — (4th Edition) by Bob Stayton, online book.
- [DocBook XSL Stylesheets: Reference Documentation] — by Norman Walsh, online book.
- [Wikipedia » DocBook XSL][WP DocBook XSL]
- https://github.com/docbook/xslt20-stylesheets
- https://github.com/docbook/xslt10-stylesheets

## Git Submodules

- [_Pro Git_ book » Git Submodules][Git Submodules] — by Scott Chacon and Ben Straub.
- [_Learn Version Control with Git_ » Submodules] — by Git Tower.
- [Using submodules in Git - Tutorial] — by Lars Vogel.

<!-----------------------------------------------------------------------------
                               REFERENCE LINKS                                
------------------------------------------------------------------------------>

[open an Issue]: https://github.com/alan-if/alan-xsl-fopub/issues/new

<!-- project folders -->

[fonts]: ./fonts/ "Navigate to folder"
[xsl-fopub]: ./xsl-fopub/ "Navigate to folder"

[Fira Sans Condensed]: ./fonts/Fira_Sans_Condensed/ "Navigate to folder"
[Noto Sans]: ./fonts/Noto_Sans/ "Navigate to folder"
[Roboto Mono]: ./fonts/Roboto_Mono/ "Navigate to folder"

<!-- project files -->

[LICENSE]: ./xsl-fopub/LICENSE "View MIT License file"
[XSL-DevNotes]: ./XSL-DevNotes.md/ "View document"
[fop-config.xml]: ./xsl-fopub/fop-config.xml "View source file"

<!-- PDF samples -->

[tests-syntax-highlighting.pdf]: https://github.com/alan-if/alan-docs/blob/master/_dev/styles-tests/tests-syntax-highlighting.pdf "Download PDF samle"
[tests-typography.pdf]: https://github.com/alan-if/alan-docs/blob/master/_dev/styles-tests/tests-typography.pdf "Download PDF samle"
[styles-preview.pdf]: https://github.com/alan-if/alan-docs/blob/master/_dev/styles-tests/styles-preview.pdf "Download PDF samle"

<!-- Alan links -->

[Alan Interactive Fiction]: https://www.alanif.se/ "Visit Alan website"
[Alan]: https://www.alanif.se/ "Visit Alan website"

[Alan Docs]: https://github.com/alan-if/alan-docs "Visit the Alan Docs repository on GitHub"
[Alan Manual]: https://github.com/alan-if/alan-docs/tree/master/manual "Visit the Alan Manual project"

<!-- third party tools -->

[Asciidoctor]: https://github.com/asciidoctor/asciidoctor "Visit Asciidoctor repository on GitHub"
[asciidoctor-fopub]: https://github.com/asciidoctor/asciidoctor-fopub "Visit the asciidoctor-fopub repository on GitHub"

[XSLTHL]: https://sourceforge.net/projects/xslthl/
[XSLTHL Wiki]: https://sourceforge.net/p/xslthl/wiki/Home/
[Syntax Highlighters]: https://sourceforge.net/p/xslthl/wiki/Syntax%20Highlighters/

[Ruby]: https://www.ruby-lang.org "Visit Ruby website"
[RubyInstaller]: https://rubyinstaller.org/ "Visit RubyInstaller for Windows website"
[Chocolatey Ruby]: https://chocolatey.org/packages/ruby "View the Chocolatey package for Ruby"
[Chocolatey]: https://chocolatey.org "Visit Chocolatey website"
[Chocolatey GUI]: https://chocolatey.org/packages/ChocolateyGUI "View the Chocolatey GUI package"

<!-- Licenses -->

[SIL Open Font]: https://scripts.sil.org/cms/scripts/page.php?site_id=nrsi&id=OFL_web "Read the online text of the SIL Open Font License"
[Apache 2.0]: http://www.apache.org/licenses/LICENSE-2.0 "Read the online text of the Apache License v2.0"

<!-- people -->

[Tristano Ajmone]: https://github.com/tajmone "View Tristano Ajmone's GitHub profile"
[Dan Allen]: https://github.com/mojavelinux "View Dan Allen's GitHub profile"

<!-- organizations -->

[Alan-IF]: https://github.com/alan-if "View the Alan Interactive Fiction Development team on GitHub"
[Alan Interactive Fiction Development team]: https://github.com/alan-if "View the Alan Interactive Fiction Development team on GitHub"

<!-- reference links --------------------------------------------------------->

<!-- Apache FOP -->

[FOP]: https://xmlgraphics.apache.org/fop/ "Visit the Apache™ FOP Project"
[Apache™ FOP v2.1]: https://xmlgraphics.apache.org/fop/2.1/

[FOP Configuration]: https://xmlgraphics.apache.org/fop/2.1/configuration.html
[FOP Fonts]: https://xmlgraphics.apache.org/fop/2.1/fonts.html
[Fonts and FOP]: https://pages.lip6.fr/Jean-Francois.Perrot/XML-Int/Session6/FOPFonts/index.html

<!-- DocBook -->

[DocBook 5: The Definitive Guide]: https://tdg.docbook.org/tdg/5.0/docbook.html
[DocBook Wiki]: https://github.com/docbook/wiki/wiki
[WP DocBook]: https://en.wikipedia.org/wiki/DocBook

<!-- DocBook XSL Stylesheetsn -->

[DocBook XSL: The Complete Guide]: http://www.sagehill.net/docbookxsl/index.html
[DocBook XSL Stylesheets: Reference Documentation]: http://docbook.sourceforge.net/release/xsl/current/doc/
[WP DocBook XSL]: https://en.wikipedia.org/wiki/DocBook_XSL

<!-- Git references -->

[Pro Git]: https://git-scm.com/book "'Pro Git' book online"
[Git Submodules]: https://git-scm.com/book/en/v2/Git-Tools-Submodules "Read the chapter on Git Submodules from the 'Pro Git' book"

[Using submodules in Git - Tutorial]: https://www.vogella.com/tutorials/GitSubmodules/article.html
[_Learn Version Control with Git_ » Submodules]: https://www.git-tower.com/learn/git/ebook/en/command-line/advanced-topics/submodules#start

<!-- EOF -->