---
title: Guide to Markdown Mode for Emacs
author: Jason R. Blevins
date: July 2017
tags: Emacs, markdown, markdown-mode, writing, plain text
---

{mainmatter}

# Introduction {#intro}

![Markdown Mode Screenshot](images/markdown-mode-2.3.png)

This guide describes how to install, use, and customize [Markdown
Mode][mm], a major mode for editing Markdown-formatted text files
in [Emacs][em].  This guide is based on **Markdown Mode version 2.3**.

Markdown Mode and this guide were written by [Jason Blevins][jb].
Markdown Mode is free software, licensed under the [GNU General
Public License (GPL), version 2][gp], and it may be freely obtained
from the [Markdown Mode homepage][mm].

Funds from [sales of this book][bk] help support development of both
Markdown Mode and this Guide.  If you're reading this, then you're
likely one of the many people who rely on Markdown Mode to get your
work done.  If you haven't already, please consider supporting the
project by [buying the complete Guide][bk].

## Quick Reference

If you are a seasoned Markdown Mode user, here is a quick reference table:

| Action                                              | Keybinding        |
|-----------------------------------------------------|-------------------|
| **Headings**                                        |                   |
| Insert heading depending on context                 | `C-c C-s h`       |
| Insert heading, prefer Setext                       | `C-c C-s H`       |
| Insert atx heading of level # = 1, 2, … 6           | `C-c C-s #`       |
| Insert Setext heading of level 1                    | `C-c C-s !`       |
| Insert Setext heading of level 2                    | `C-c C-s @`       |
| **Inline Elements**                                 |                   |
| Bold                                                | `C-c C-s b`       |
| Italics                                             | `C-c C-s i`       |
| Inline code                                         | `C-c C-s c`       |
| `kbd` tag                                           | `C-c C-s k`       |
| Wiki link                                           | `C-c C-s w`       |
| **Block Elements**                                  |                   |
| Preformatted/code block (active region)             | `C-c C-s p`       |
| Preformatted/code block (region)                    | `C-c C-s P`       |
| Blockquote (active region)                          | `C-c C-s q`       |
| Blockquote (region)                                 | `C-c C-s Q`       |
| GFM code block                                      | `C-c C-s C`       |
| Edit code block in indirect buffer                  | `C-c '`           |
| **Links and Images**                                |                   |
| Insert or edit link (inline, reference, or URL)     | `C-c C-l`         |
| Insert or edit image (inline or reference)          | `C-c C-i`         |
| Follow link at point                                | `C-c C-o`         |
| Jump between reference link and definition          | `C-c C-d`         |
| Move to next link                                   | `M-n`             |
| Move to previous link                               | `M-p`             |
| **Footnotes**                                       |                   |
| Insert footnote                                     | `C-c C-s f`       |
| Jump between footnote and definition                | `C-c C-d`         |
| **List Items & List Editing**                       |                   |
| Insert new list item (same level)                   | `M-RET`           |
| Insert new list item (same level)                   | `C-c C-j`         |
| Insert new list item (parent level)                 | `C-u C-c C-j`     |
| Insert new list item (child level)                  | `C-u C-u C-c C-j` |
| Move list item up                                   | `C-c <up>`        |
| Move list item down                                 | `C-c <down>`      |
| Outdent/promote list item                           | `C-c <left>`      |
| Indent/demote list item                             | `C-c <right>`     |
| Toggle GFM checkbox                                 | `C-c C-x C-x`     |
| **Horizontal Rules**                                |                   |
| Insert default horizontal rule string               | `C-c C-s -`       |
| **Killing and Yanking**                             |                   |
| Kill element and keep text in kill ring             | `C-c C-k`         |
| Yank text back into buffer                          | `C-y`             |
| **Movement by Paragraph**                           |                   |
| Backward paragraph                                  | `M-{`             |
| Forward paragraph                                   | `M-}`             |
| Mark paragraph                                      | `M-h`             |
| **Movement by Block**                               |                   |
| Backward block                                      | `C-M-{`           |
| Forward block                                       | `C-M-}`           |
| Mark paragraph                                      | `C-c M-h`         |
| Narrow to block                                     | `C-x n b`         |
| Widen                                               | `C-x n w`         |
| **Movement by Section (Defun)**                     |                   |
| Beginning of current section                        | `C-M-a`           |
| End of current section                              | `C-M-e`           |
| Mark current section                                | `C-M-h`           |
| Narrow to current section                           | `C-x n d`         |
| Narrow to current subtree                           | `C-x n s`         |
| Widen                                               | `C-x n w`         |
| **Outline/List Movement**                           |                   |
| Next heading/list item (any level)                  | `C-c C-n`         |
| Previous heading/list item (any level)              | `C-c C-p`         |
| Next heading/list item at same level                | `C-c C-f`         |
| Previous heading/list item at same level            | `C-c C-b`         |
| Move up to parent heading/list item                 | `C-c C-u`         |
| **Outline Visibility Cycling**                      |                   |
| Cycle visibility: top-level only, all headings, all | `S-TAB`           |
| Cycle visibility of heading at point                | `TAB`             |
| **Outline Subtree Editing**                         |                   |
| Move subtree up                                     | `C-c <up>`        |
| Move subtree down                                   | `C-c <down>`      |
| Promote subtree                                     | `C-c <left>`      |
| Demote subtree                                      | `C-c <right>`     |
| **Region Editing**                                  |                   |
| Indent region                                       | `C-c >`           |
| Exdent region                                       | `C-c <`           |
| **Promotion and Demotion**                          |                   |
| Promote element (heading, list item, etc.) at point | `C-c -`           |
| Demote element (heading, list item, etc.) at point  | `C-c =`           |
| **Markup Completion**                               |                   |
| Complete markup at point or in region               | `C-c C-]`         |
| Complete markup in buffer                           | `C-c C-c ]`       |
| **Markdown & Utility Commands**                     |                   |
| Run Markdown, output to temporary buffer            | `C-c C-c m`       |
| Run Markdown, export to file                        | `C-c C-c e`       |
| Run Markdown and preview                            | `C-c C-c p`       |
| Run Markdown, export, and preview                   | `C-c C-c v`       |
| Run Markdown and save to kill ring                  | `C-c C-c w`       |
| Toggle live preview mode                            | `C-c C-c l`       |
| Open external previewer                             | `C-c C-c o`       |
| Check references in buffer                          | `C-c C-c c`       |
| Renumber ordered lists in buffer                    | `C-c C-c n`       |
| **Toggles and Settings**                            |                   |
| Toggle markup hiding                                | `C-c C-x C-m`     |
| Toggle URL hiding                                   | `C-c C-x C-l`     |
| Toggle native code block font lock                  | `C-c C-x C-f`     |
| Toggle inline images                                | `C-c C-x C-i`     |
| Toggle LaTeX math support                           | `C-c C-x C-e`     |
| Toggle GFM checkbox                                 | `C-c C-x C-x`     |

## Markdown

[Markdown][md] is a lightweight markup language, originally created
by [John Gruber][df] in 2004.  Markdown aims to be simultaneously easy
to write and easy to read.  It takes many cues from previous, similar
attempts (e.g., [atx][at] and [Setext][st]) as well as from existing
conventions for marking up plain text email.

“Markdown” is also used to refer to the original Markdown processor,
also written by Gruber, which is a Perl script that converts plain
Markdown-formatted text input to XHTML output.  It is distributed
under a BSD-style license and is included with, or available as a
plugin for, several content management systems.  In the remainder I
will refer to the processor by its filename, Markdown.pl, to
distinguish the two.  After the original Perl implementation, Markdown
processors have been implemented by others in a variety of programming
languages.

### Markdown Syntax Reference

For reference, below is a plain-text summary of Markdown's syntax.
Additional details about each type of element are explained in the
course of describing the available Markdown Mode commands.  See the
original [Markdown syntax page][sx] for more complete details.

``` markdown
# First-Level Header

## Second-Level Header

### Third-Level header

Paragraphs are separated by a blank line.

Text styles: _italic_, *italic*, __bold__, **bold**, `code`.

Horizontal rules:

---
***

Unordered list:

  * Item
  * Item
  * Item

Ordered list:

  1. apples
  2. oranges
  3. pears

Inline [link](http://jblevins.org/).

Reference [link][tag].

[tag]: http://jblevins.org/

Implicit reference [link][].

[link]: http://jblevins.org/projects/markdown-mode/

Use two spaces at the end of a line  
to create a hard line break.
```

Characters can be backslash-escaped in Markdown.  That is, characters
which are ordinarily interpreted by Markdown as formatting commands
will instead be interpreted literally if preceded by a backslash.  For
example, when you need to type a literal asterisk or underscore:

``` markdown
This is *italic*, but this \*is not\*.
```

Markdown.pl also does not transform any text within “raw” block-level
XHTML elements (although some other processors do).  Thus it is
possible to include sections of XHTML within a Markdown source
document by wrapping them in block-level XHTML tags.  However, with a
small number of exceptions (e.g., comments and `kbd` tags) Markdown
Mode does not explicitly support inline XHTML.

### Additional Information

For additional information on Markdown syntax, you can refer to the
following links:

*   [Markdown Syntax][sx] - the original syntax description by John Gruber
*   [Markdown Dingus](http://daringfireball.net/projects/markdown/dingus) - an online converter by John Gruber using the
    original Markdown.pl
*   [Wikipedia article](http://en.wikipedia.org/wiki/Markdown) - includes several examples of syntax as well as
    some background information on Markdown
*   [Mastering Markdown](https://guides.github.com/features/mastering-markdown/) - a guide to Markdown and GFM by GitHub
*   [Pandoc User's Guide](http://pandoc.org/MANUAL.html) - covers basic Markdown syntax as well as
    extensions supported by Pandoc
*   [PHP Markdown Reference](https://michelf.ca/projects/php-markdown/reference/) - covers basic Markdown syntax and PHP
    Markdown Extra extensions
*   [Write better Markdown](http://brettterpstra.com/2015/08/24/write-better-markdown/) - a style guide by Brett Terpstra

## History

<!-- FIXME: Update version number and release date. -->

As of this writing, the latest stable version is Markdown Mode version
2.3, released on **[RELEASE DATE]**.  See the [release notes][v2.3] for
details.  A complete listing of release notes can also be found
in [the appendix](#release-notes).

Markdown Mode was originally written and is still maintained
by [Jason Blevins][jb].  The initial release, version 1.1, was created on
May 24, 2007.

Historically, version numbering began at 1.1 because the source code
was originally stored in RCS (Revision Control System), which numbers
revisions starting at 1.1 by default.  After version 1.5, the source
was moved to Git, where it still resides today.  See the chapter
on [Markdown Mode Development](#devel) for instructions to obtain the source
code.

  * 2007-05-24: [Version 1.1][v1.1]
  * 2007-05-25: [Version 1.2][v1.2]
  * 2007-06-05: [Version 1.3][v1.3]
  * 2007-06-29: [Version 1.4][v1.4]
  * 2007-10-11: [Version 1.5][v1.5]
  * 2008-06-04: [Version 1.6][v1.6]
  * 2009-10-01: [Version 1.7][v1.7]
  * 2011-08-12: [Version 1.8][v1.8]
  * 2011-08-15: [Version 1.8.1][v1.8.1]
  * 2013-01-25: [Version 1.9][v1.9]
  * 2013-03-24: [Version 2.0][v2.0]
  * 2016-01-09: [Version 2.1][v2.1]
  * 2017-05-26: [Version 2.2][v2.2]

## Acknowledgments

Markdown Mode has benefited greatly from the efforts of the many
volunteers who have sent patches, test cases, bug reports,
suggestions, helped with packaging, etc.  Thank you for your
contributions!  Markdown Mode is much more robust and includes many
additional features as a result of your efforts.  Below is a partial
list of contributors (in alphabetical order) who sent substantial
patches, test cases, bug reports, suggestions, helped with packaging,
etc.  See the [GitHub contributors graph][cg] and the commit log for more
details.

<!--
git shortlog -s -n | awk '{ $1=""; print $0 }'
M-x sort-regexp-fields RET \(.*\) \(.*?\)$ RET 2 RET
-->

> Masayuki Ataka, Hilko Bengen, Jonas Bernoulli, Greg Bognar, Roger
> Bolsius, Daniel Brotsky, Cyril Brulebois, Daniel Burrows, Donald
> Ephraim Curtis, Julien Danjou, Werner Dittmann, Jeremiah Dodds,
> Carsten Dominik, Michael Dwyer, Dmitry Dzhus, Peter Eisentraut,
> Conal Elliott, Bryan Fink, Gunnar Franke, Samuel Freilich, Shigeru
> Fukaya, Peter S. Galbraith, Francois Gannaz, David Glasser, Matus
> Goljer, Kévin Le Gouguec, Marijn Haverbeke, Isaac Hodes, Philippe
> Ivaldi, Zhenlei Jia, Peter Jones, Antonis Kanouras, Marcin
> Kasperski, Keshav Kini, Dave Kleinschmidt, Vasily Korytov, Joost
> Kremers, Bryan Kyle, Kévin Le Gouguec, Chris Lott, Christopher
> J. Madsen, Tom May, Danny McClanahan, Matt McClure, Howard Melman,
> Eric Merritt, Nelson Minar, Stefan Monnier, Makoto Motohashi, Jon
> Mountjoy, Akinori Musha, Pierre Neidhardt, Spanti Nicola, Theresa
> O'Connor, George Ogata, Paul W. Rankin, Jose A. Ortega Ruiz, Max
> Penet, Nicolas Petton, Jorge Israel Peña, Scott Pfister, Kevin
> Porter, Steve Purcell, Alec Resnick, Christophe Rhodes, Dale
> Sedivec, Ankit Solanki, Tyler Smith, Michael Sperber, Mike Sperber,
> Vitalie Spinu, Philipp Stephani, Moogen Tian, Vegard Vesterheim, Tim
> Visher, Ben Voui, Sebastian Wiesner, Peter Williams, Tianxiang
> Xiong, Ian Yang, Syohei Yoshida, and Google, Inc.

# Installation {#install}

There are many ways to load packages in Emacs, but here I will cover
three: installation using the built-in Emacs package manager
(recommended), manual installation, and installation using a
system-wide package manager.

Markdown Mode is compatible with Emacs 24.3 and later, and it has few
dependencies.  To enable editing of code blocks in indirect buffers
using `C-c '`, you will need to install the [edit-indirect][ei]
package.

*Note:* This chapter covers installing the stable version of Markdown
Mode, but installing the development version is similar.  If you'd
like to test the development version see the [Development](#devel)
chapter at the end.

## Emacs Package Manager

The recommended way to install Markdown Mode is to use the built-in
Emacs package manager, `package.el`, which has been included since
Emacs 23.  Markdown Mode can be installed via the `markdown-mode`
package in the [MELPA Stable repository][ms].[^MELPA]

First, you'll need to configure the package manager to use the MELPA
Stable repository by adding the following to your `.emacs`, `init.el`,
or equivalent init file:

``` emacs-lisp
(require 'package)
(add-to-list 'package-archives
    '("melpa-stable" . "https://stable.melpa.org/packages/"))
(package-initialize)
```

Then, after restarting Emacs or evaluating the above statements, issue
the following command: `M-x package-install RET markdown-mode RET`.
When installed this way, the major modes `markdown-mode` and
`gfm-mode` will be autoloaded and `markdown-mode` will be invoked
automatically for file names ending in either `.md` or `.markdown`.

Alternatively, if you manage loading packages with [use-package][up]
(a practice I highly recommend for advanced users) then you can
automatically install and configure `markdown-mode` by adding a
declaration such as this one to your init file (as an example; adjust
settings as desired):

``` emacs-lisp
(use-package markdown-mode
  :ensure t
  :commands (markdown-mode gfm-mode)
  :mode (("README\\.md\\'" . gfm-mode)
         ("\\.md\\'" . markdown-mode)
         ("\\.markdown\\'" . markdown-mode))
  :init (setq markdown-command "multimarkdown"))
```

Finally, if you prefer to install and use the development version of
Markdown Mode, which may be unstable or possibly even unusable at
times, you can install the `markdown-mode` package from the
(unstable) [MELPA repository][ml] instead.  In this case, you might
need to tell the package manager about the repository in your init
file, like so:

``` emacs-lisp
(require 'package)
(add-to-list 'package-archives
    '("melpa" . "http://melpa.org/packages/"))
(package-initialize)
```

## OS Package Manager

Markdown Mode is also available in various package managers on several
operating systems.  You should confirm that the package you install
contains the latest stable version (and please notify the package
maintainer if not).  These packages also typically do not byte compile
the source files or configure them to load automatically, so you may
still need to follow the manual installation instructions in the
following section.

**MacOS:** MacOS users who use Homebrew can install
the [homebrew/emacs/markdown-mode][hb] formula, which in turn
obtains the latest stable version of Markdown Mode from GitHub.

``` shell
brew update
brew install dunn/emacs/markdown-mode
```

This will install files in
`/usr/local/share/emacs/site-lisp/markdown-mode`.  Depending on where
you obtained Emacs, this directory may or may not be in your
`load-path`, so you may need to add it as described below for manual
installation.

If you are a MacPorts user, there does not seem to be an up to date
port.  The existing [markdown-mode.el][mp] port currently points to a
Git revision from 2014.  There is also a [pending][mt] ticket for a
new port, but there has been no activity.  Rather than installing from
MacPorts, I recommend using the Emacs package manager as described
above.

**Debian and Ubuntu Linux:** On Debian-based distributions, Markdown
Mode can be installed via the [elpa-markdown-mode][dp] package.

``` shell
sudo apt-get update
sudo apt-get install elpa-markdown-mode
```

**FreeBSD:** FreeBSD users can install
the [textproc/markdown-mode.el][fb] port, but note that the
installed files are not byte compiled.  Users who install this port
will also need to edit their init files following the Manual
installation instructions below, to load Markdown Mode.

**NetBSD:** Markdown Mode is available in pkgsrc
as [textproc/markdown-mode][nb], but (as of May 8 2017) it installs and
byte-compiles an outdated version of Markdown Mode.  If you use this
package, you'll also need to edit your init file following the Manual
installation instructions below, to load Markdown Mode.  Rather than
installing this package, use the Emacs package manager as described
above.

``` shell
pkg_add textproc/markdown-mode
```

## Manual Download

Alternatively you can manually download and install Markdown Mode.
First, download the [latest stable version][el] and save
the file where Emacs can find it (i.e., a directory in your
`load-path`).  You can then configure `markdown-mode` and `gfm-mode`
to load automatically by adding the following to your init file:

``` emacs-lisp
(autoload 'markdown-mode "markdown-mode"
   "Major mode for editing Markdown files" t)
(add-to-list 'auto-mode-alist
    '("\\.markdown\\'" . markdown-mode))
(add-to-list 'auto-mode-alist
    '("\\.md\\'" . markdown-mode))

(autoload 'gfm-mode "markdown-mode"
   "Major mode for GitHub Flavored Markdown files" t)
(add-to-list 'auto-mode-alist
    '("README\\.md\\'" . gfm-mode))
```


# Customization {#customization}

![Markdown customize group in Emacs](images/customize-group-markdown.png)

Although strictly speaking no configuration is *necessary*, there are
a few settings that users should usually customize.  When in Markdown
Mode, the `M-x customize-mode` command provides an interface to all of
the possible customizations.  If you are currently using a different
mode, then you'll need to be more specific: `M-x customize-mode RET
markdown-mode`.  Alternatively, you can customize variables using `M-x
customize-group RET markdown` (i.e., by customizing the *group* rather
than the *mode*).

**Note:** If you use this built-in customize interface, be sure to
save your changes before closing the buffer by clicking the "Apply and
Save" button.

### Markdown Command

The main variable you'll most likely need to customize is
`markdown-command`.  Markdown Mode expects that a Markdown binary is
available, determined by the value of `markdown-command`.  You should
customize this variable so that Markdown Mode uses your Markdown
processor of choice.

`markdown-command`

:   The `markdown-command` variable contains the command used to run
    Markdown.  The default is to look for an executable named
    `markdown` in the Emacs `exec-path`.

You can either provide the full path to the executable, or you can
simply provide the name if the executable is in your Emacs
`exec-path`.  See below for details on determining and configuring
your `exec-path`.

If you are using the customize interface, you simply need to type the
path to your Markdown script or binary in the "Markdown Command" field
in the customize interface (e.g., `/usr/local/bin/multimarkdown`).  If
you know the name of the executable, but you're not sure where it is
located, and you are using macOS or Linux, you can find it by typing
`which multimarkdown` at the command line.

To set this variable in your init file, add a line such as the
following:

``` emacs-lisp
(setq markdown-command "/usr/local/bin/multimarkdown")
```

### Markdown Command and `exec-path`

You can always set `markdown-command` to be the full path to the
executable, but a better solution is to set your `exec-path` properly.
A simple way to do this is to use the `exec-path-from-shell` package,
which sets the Emacs `exec-path` using your system `$PATH` variable,
allowing Emacs to find any commands you can also use from the command
line.

If you attempt to preview or export your buffer and you see an error
regarding the `markdown` command, then most likely you need to check
your `markdown-command` setting or install a Markdown processor (or
both).  Below are some common error messages that indicate that either
the `markdown` binary cannot be found or `markdown-command` should be
customized to your system:

```
/bin/bash: markdown: command not found
zsh:1: command not found: markdown
'markdown' is not recognized as an internal or external
    command, operable program or batch file.
```

If your Markdown executable is in your Emacs `exec-path`, then Emacs
can find it without the full path.  You can check this in two ways.
First, to see the value of `exec-path`, issue `M-x describe-variable
RET exec-path`.  If you see the directory containing your Markdown
executable in the path, then the name of the program by itself should
suffice without the path (e.g., `multimarkdown`).

Second, you can also check to see if Emacs can find your executable by
using the `executable-find` command.  This isn't an interactive
command, but you can run it using `eval-expression` via `M-:`.  For
example, to check whether Emacs can find an executable named `pandoc`,
you can issue `M-: (executable-find "pandoc")`.  The return value will
be displayed in the minibuffer.  If you see the path to `pandoc`, then
Emacs can find it.  If you see `nil`, then it could not be found in
`exec-path`.

### Configuring Markdown on macOS with Homebrew

If you use Homebrew, then you can install Markdown.pl or Pandoc by
issuing one of the following commands:

```
brew install markdown
brew install pandoc
```

Then, in Emacs, type `M-x customize-mode RET markdown-mode` and set
'Markdown Command' to the path of the executable you just installed:
`/usr/local/bin/markdown` or `/usr/local/bin/pandoc` for Pandoc.

### Configuring Markdown on Windows

On Windows, you'll need to use the full path including the drive
letter.  For example, if you installed Pandoc in `C:\Utils\Console`,
then you'd set `markdown-command like this:

``` emacs-lisp
(setq markdown-command "c:/Utils/Console/pandoc.exe")
```

If you need to run Markdown as a script, with an interpreter, then
you'll need to add that as well.  For example, to use Markdown.pl with
Perl (both need to be installed), something like this should suffice:

``` emacs-lisp
(setq markdown-command "c:/path/to/perl.exe c:/path/to/Markdown.pl")
```

### Passing Command Line Options to Markdown

You can also customize this variable to pass any necessary
command line options to your Markdown processor.  For example, to ask
MultiMarkdown to enable smart typography and footnotes, you can define
`markdown-command` as follows:

``` emacs-lisp
(setq markdown-command "multimarkdown --smart")
```

By default, Markdown Mode assumes that your Markdown processor accepts
input via `stdin`.  That is, it assumes that if you were using it from
the command line, you could *pipe* input to it like so:

``` shell
cat document.md | markdown
```

### Passing a File Name to Markdown

If your Markdown processor needs to be passed a file name, you'll need
to set another variable: `markdown-command-needs-filename`.  Set this
variable to `t` if `markdown-command` does not accept standard input.
The default value is `nil`.  When this variable is `nil`,
Markdown Mode will pass the Markdown content to `markdown-command`
using standard input (`stdin`).  When set to `t`, Markdown Mode will
pass the name of the file as the final command line argument to
`markdown-command`.  Note that in the latter case, you will only be
able to run `markdown-command` from buffers which are visiting a file.

------------------------------------------------------------------------------

# Markdown Mode Development {#devel}

Markdown Mode is an open source project.  The primary repository for
Markdown Mode is [on GitHub][gh].  Anyone can easily browse the
project files by visiting the project's GitHub page
at <https://github.com/jrblevin/markdown-mode>.

If you would like to contribute to Markdown Mode development, the best
way to start is to clone the Git repository.  You can clone the
repository using several protocols:

* HTTPS: `git clone https://github.com/jrblevin/markdown-mode.git`
* Git: `git clone git://github.com/jrblevin/markdown-mode.git`
* SSH: `git clone ssh://git@github.com:jrblevin/markdown-mode.git`

## Testing the Development Version

To test the current development version, you may either download
`markdown-mode.el` from the [GitHub repository][gh] or install the
`markdown-mode` package from [MELPA][ml].

Note that by nature, the development version of Markdown Mode may be
unstable or even unusable at times.  I do not recommend it for daily
use unless you are fine with things occasionally breaking.  Function
names, features, keybindings, etc. may change without notice.

Supposing you have stored `markdown-mode.el` in directory named
`/path/to/markdown-mode/`, then you'll probably need to make sure that
Emacs can find it by adding something like the following to your init
file:

``` emacs-lisp
(add-to-list 'load-path "/path/to/markdown-mode/")
```

## Reporting Bugs and Issues

Markdown Mode is developed and tested primarily for compatibility with
GNU Emacs 24.3 and later.  If you are using an older version of Emacs,
please update to a more recent version first to see if the issue
persists.

If you do find a bug in Markdown Mode, please construct a test
case—and if possible a patch—and open a ticket on the
[GitHub issue tracker][is].

------------------------------------------------------------------------------

{backmatter}

# Release Notes {#release-notes}

## Markdown Mode 2.3

*Under development*

*   **Breaking changes:**

    -   `markdown-mode` now requires Emacs 24.3 or later.
    -   Markup insertion and replacement keybindings under `C-c
        C-s` (_s_ for style) have been revised to make them
        easier to remember.  Now, when the prefix `C-c C-s`
        is pressed, a short minibuffer help prompt is presented as a
        reminder of a few of the most frequently used keys.  The major
        changes are that bold is now inserted with `b`
        (previously `s`) and italic is now `i`
        (previously `e`).  As a result, blockquote is now
        `q` (previously `b`) and strikethrough
        markup is inserted with `s` (previously
        `d`).  Press `C-c C-s C-h` for a complete
        list of markup insertion keybindings.  Heading insertion
        commands are also now under `C-c C-s`.
    -   Link insertion and editing has been consolidated into one
        command, `markdown-insert-link`, bound to `C-c C-l`.
        As such, the previous separate link insertion keybindings have
        been removed: `C-c C-a l`, `C-c C-a L`,
        `C-c C-a r`, and `C-c C-a u`.
    -   Image insertion and editing has been consolidated into one
        command, `markdown-insert-image`, bound to `C-c C-i`.
        As such, the previous separate image insertion keybindings have
        been removed: `C-c C-i i` and `C-c C-i I`.
    -   Footnote and wiki link insertion have been moved to the
        markup insertion prefix, as `C-c C-s f` and
        `C-c C-s w`.
    -   The list and outline editing commands have been removed from
        the top-level positions (previously `M-<left>`,
        `M-<right>`, `M-<up>`, `M-<down>`)
        and moved to major mode keybindings under `C-c` to
        `C-c <left>`, `C-c <right>`,
        `C-c <up>`, and `C-c <down>`, respectively.
        ([GH-164][])
    -   The list and outline editing commands have also been unified
        so that they all operate on entire subtrees of list items and
        subtrees of atx headings, symmetrically.  Previously there were
        separate commands for editing heading subtrees, but promoting
        a single section is easy enough by directly inserting or
        removing a hash mark or using the markup replacement commands.
    -   Jumping between references and reference definitions via
        `markdown-jump`, previously bound to `C-c C-l`, has
        been moved to `C-c C-d` and rebranded as
        `markdown-do`, which attempts to do something sensible with
        the object at the point.
    -   Rename internal `markdown-link-link` to `markdown-link-url`
        for clarity.
    -   The old inline image toggling command `C-c C-i C-t`
        has been removed and replaced `C-c C-x C-i` in order
        to allow for the new interactive image insertion command at
        `C-c C-i`.  Toggling keybindings are currently being
        grouped under `C-c C-x`.
    -   `markdown-blockquote-face` is now applied to the entire
        blockquote, including the leading `>`, so it can be used to
        apply a background if desired.
    -   In `markdown-regex-header`, groups 4 and 6 now include
        whitespace surrounding hash marks in atx headings.
    -   Font lock for `~~strikethrough~~` is now supported in
        `markdown-mode` in addition to `gfm-mode`.
    -   Introduced a new face for horizontal rules: `markdown-hr-face`.
        Previously, `markdown-header-delimiter-face` was used.
    -   Markdown Mode is now distributed under the GNU GPL version 3
        or later.
    -   Rename `markdown-fill-forward-paragraph-function` to
        `markdown-fill-forward-paragraph`.
    -   Rename `markdown-footnote-face` to `markdown-footnote-marker-face`.
    -   Functions `markdown-insert-inline-link-dwim` and
        `markdown-insert-reference-link-dwim` have been combined and
        replaced with `markdown-insert-link`.
    -   Functions `markdown-exdent-region` and `markdown-exdent-or-delete`
        are now named `markdown-outdent-region` and
        `markdown-outdent-or-delete`, respectively.
    -   The non-interactive image insertion commands have been
        refactored to mirror the corresponding link insertion
        commands.  `markdown-insert-image` (for inline images) has
        been renamed `markdown-insert-inline-image` and it now takes
        three arguments (previously one optional argument).
        `markdown-insert-reference-image` now takes four arguments
        (previously none).

*   New features:

    -   Markup hiding: Add a custom variable `markdown-hide-markup`,
        which determines whether to hide or otherwise beautify
        Markdown markup.  For example, for inline links the brackets,
        URL, and title will be hidden and only the (clickable) link
        text will remain.  The URL can be seen by hovering with the
        mouse pointer and edited by deleting one of the invisible
        brackets or parentheses.  This can be toggled interactively
        using `C-c C-x C-m` (`markdown-toggle-markup-hiding`).
        This setting supersedes URL hiding (below).  ([GH-130][])
    -   Unicode bullets are used to replace ASCII list item markers
        for unordered lists when markup hiding is enabled.  The list
        of characters used, in order of list level, can be specified
        by setting the variable `markdown-list-item-bullets`.
        ([GH-130][])
    -   When markup hiding is enabled, the characters used for
        replacing certain markup can be changed by customizing the
        corresponding variables:
        `markdown-blockquote-display-char`,
        `markdown-hr-display-char`, and
        `markdown-definition-display-char`.
    -   URL and reference label hiding: URLs for inline links and
        labels for reference links can now be hidden if desired.  This is
        configurable via `markdown-hide-urls`.  URLs will appear as
        `[link](∞)` instead of
        `[link](http://perhaps.a/very/long/url/)`.  To change the
        placeholder character used, set `markdown-url-compose-char`.
        This feature can be toggled using `C-c C-x C-l`
        (`markdown-toggle-url-hiding`).  If full markup hiding (above)
        is enabled, then URL hiding has no additional effect.
    -   Native code block font-lock: Add a custom variable
        `markdown-fontify-code-blocks-natively`, which determines
        whether to fontify code in code blocks using the native major
        mode.  This only works for fenced code blocks where the
        language is specified where we can automatically determine the
        appropriate mode to use.  The language to mode mapping may be
        customized by setting the variable `markdown-code-lang-modes`.
        ([GH-123][], [GH-185][])
    -   When the [`edit-indirect`](https://github.com/Fanael/edit-indirect/)
        package is installed, `C-c '` (`markdown-edit-code-block`)
        can be used to edit a code block in an indirect buffer in the native
        major mode.  Press `C-c C-c` to commit changes and return
        or `C-c C-k` to cancel.
    -   Add command `C-c C-x C-f` for toggling native font lock
        for code blocks (`markdown-toggle-fontify-code-blocks-natively`).
    -   Add "page" movement, marking, and narrowing commands, where a
        "page" in Markdown is defined to be a top-level subtree:
        `markdown-forward-page` (`C-x ]`),
        `markdown-backward-page` (`C-x [`),
        `markdown-mark-page` (`C-x C-p`), and
        `markdown-narrow-to-page` (`C-x n p`).  ([GH-191][])
    -   Add subtree marking and narrowing functions:
        `markdown-mark-subtree` (`C-c C-M-h`) and
        `markdown-narrow-to-subtree` (`C-x n s`).
        ([GH-191][])
    -   Add syntax-aware Markdown paragraph movement commands:
        `M-{` (`markdown-backward-paragraph`) and
        `M-}` (`markdown-forward-paragraph`).  To mark a
        paragraph, use `M-h` (`markdown-mark-paragraph`).
        These move at a more granular level than the block movement
        commands.  ([GH-191][])
    -   The previous block movement and marking commands are now at
        `C-M-{`, `C-M-}`, and `C-c M-h`.
        In terms of lists, paragraph movement commands now stop at
        each list item while block commands move over entire lists.
        ([GH-191][])
    -   Add `subtree` as a possible value for
        `markdown-reference-location` and
        `markdown-footnote-location`.
    -   Ask flyspell to ignore words in URLs, code fragments,
        comments, and reference labels.
    -   Make inline links, reference links, angle bracket URLs, and
        plain URLs clickable.
    -   Add an additional keybinding for toggling inline image
        display, `C-c C-x C-i`.
    -   Add a keybinding for toggling LaTeX math (_e_quation) support:
        `C-c C-x C-e`.
    -   Support Leanpub blocks (asides, info blocks, warnings, etc.).
        These are simple extensions of the usual blockquote syntax.
    -   Font lock, with markup hiding, for subscripts (e.g., `H~2~0`)
        and superscripts (e.g., `334^10^`).  Thanks to Syohei Yoshida
        for a patch on which this is based.  ([GH-134][])
    -   Add basic font-lock support for inline attribute lists or
        inline identifiers used by Pandoc, Python Markdown, PHP
        Markdown Extra, Leanpub, etc.
    -   Add basic font-lock support for Leanpub section identifiers and
        page breaks.
    -   Add basic font-lock support for common file inclusion syntax:
        `<<(file)`, `<<[title](file)`, `<<[file]`, and `<<{file}`.
    -   Add font lock support for Pandoc inline footnotes. ([GH-81][])
    -   Raise footnote markers and inline footnote text, and
        optionally hide markup.
    -   Filling with now respects Pandoc line blocks.  ([GH-144][])
    -   Add interactive link editing and insertion command
        `markdown-insert-link`.  ([GH-199][])
    -   Added `C-c C-d`, `markdown-do`, which is a
        replacement for `C-c C-l`, `markdown-jump`.  In
        addition to jumping between reference/footnote labels and
        definitions, it also toggles GFM checkboxes.
    -   Outline movement keys `C-c C-p`, `C-c C-n`,
        `C-c C-f`, `C-c C-b`, and `C-c C-u`
        now move between list items, when the point is in a list,
        and move between headings otherwise.

*   Improvements:

    -   Document customizable variables added in version 2.2 with
        `:package-version` tags.
    -   Better consistency of function names: predicate functions
        ending in `-p` shouldn't modify match data.
    -   Generalize rebinding of paragraph movement commands in case users
        have customized `{forward,backward,mark}-paragraph` bindings.
    -   Adjust point so that it is left at beginning of setext
        headings in heading navigation commands.
    -   Prevent inline link matching in code blocks.
    -   When inserting a new reference definition, don't add blank
        line after existing reference definitions.
    -   `markdown-toggle-inline-images` now displays the status in the
        minibuffer.
    -   Increased default heading scaling range slightly, to make
        level differences more pronounced when markup is hidden.
    -   Reuse existing windows, when possible, rather than splitting
        again in preferred direction. ([GH-129][])
    -   Update known languages in `markdown-gfm-recognized-languages`.
    -   Filling with `fill-region` now leaves code blocks unmodified.
        ([GH-192][])
    -   Avoid error when live-previewing a buffer that's not visiting
        a file.  Thanks to Tianxiang Xiong for a patch.
        ([GH-200][], [GH-201][])
    -   Adaptive filling for Leanpub blocks.
    -   Set variable `comment-use-syntax`.  ([GH-213][])
    -   Support `electric-quote-inhibit-functions` for inhibiting
        electric quoting in code spans and blocks.  Thanks to Philipp
        Stephani for patches to both Emacs and Markdown Mode.
        ([GH-220][])

*   Bug fixes:

    -   Fix spurious bold/italic faces in inline code. ([GH-172][])
    -   Fix defun movement at end of buffer. ([GH-197][])
    -   Fix bug with adjacent bold font-locking in a list
        item. ([GH-176][])
    -   Prevent matching italics, bold, and inline code in comments.
    -   Prevent matching italics and bold in URLs.
    -   Prevent matching links in inline code or comment spans.
    -   Avoid infinite loop when promoting or demoting last section in
        a buffer.
    -   Fix font lock for subsequent inline links after a malformed
        inline link.  ([GH-209][])
    -   Prevent clobbering match data in
        `markdown-font-lock-extend-region-function`.  Thanks to
        Philipp Stephani for a patch.  ([GH-221][])

  [gh-81]:  https://github.com/jrblevin/markdown-mode/issues/81
  [gh-123]: https://github.com/jrblevin/markdown-mode/issues/123
  [gh-130]: https://github.com/jrblevin/markdown-mode/issues/130
  [gh-134]: https://github.com/jrblevin/markdown-mode/issues/134
  [gh-144]: https://github.com/jrblevin/markdown-mode/issues/144
  [gh-164]: https://github.com/jrblevin/markdown-mode/issues/164
  [gh-172]: https://github.com/jrblevin/markdown-mode/issues/172
  [gh-176]: https://github.com/jrblevin/markdown-mode/issues/176
  [gh-185]: https://github.com/jrblevin/markdown-mode/issues/185
  [gh-191]: https://github.com/jrblevin/markdown-mode/issues/191
  [gh-192]: https://github.com/jrblevin/markdown-mode/issues/192
  [gh-197]: https://github.com/jrblevin/markdown-mode/issues/197
  [gh-199]: https://github.com/jrblevin/markdown-mode/issues/199
  [gh-200]: https://github.com/jrblevin/markdown-mode/issues/200
  [gh-201]: https://github.com/jrblevin/markdown-mode/issues/201
  [gh-209]: https://github.com/jrblevin/markdown-mode/issues/209
  [gh-213]: https://github.com/jrblevin/markdown-mode/issues/213
  [gh-220]: https://github.com/jrblevin/markdown-mode/pull/220
  [gh-221]: https://github.com/jrblevin/markdown-mode/pull/221

## Markdown Mode 2.2

*May 26, 2017*

Version 2.2 is a major new stable release and all users are encouraged
to upgrade.  Thanks to everyone who submitted bug reports, feature
suggestions, and especially patches.

*   **Breaking changes:**

    -   Now use `C-c C-j` for inserting list items, like
        AUCTeX and similar to other programming modes.  Since `C-c
        C-j` was used for `markdown-jump` (for moving between
        reference link/footnote markers and their definitions), it has
        been changed to `C-c C-l` (think "leap" or "loop"
        instead of jump).  It's also close to `C-c C-o` (used
        for opening links).  ([GH-26][])
    -   Insertion of `kbd` tags with `C-c C-s k` or
        `markdown-insert-kbd`.
    -   Add YAML metadata parsing.  Also allow multiple Pandoc
        metadata, with tests.  Thanks to Danny McClanahan and Syohei
        Yoshida.  ([GH-66][], [GH-91][], [GH-155][], [GH-156][],
        [GH-157][])
    -   Change the behavior of `C-c C-o`
        (`markdown-follow-link-at-point`) so that if a link is a
        complete URL, it will open in a browser.  Otherwise, open it
        with `find-file` after stripping anchors and/or query strings.
        ([GH-132][])
    -   Make font lock for missing wiki links optional and disabled by
        default.  Add new custom variable
        `markdown-wiki-link-fontify-missing` to control this behavior.
    -   The _function_ `markdown-enable-math` has been made obsolete
        and renamed to `markdown-toggle-math`.  When called without an
        argument, the result is to toggle this extension rather than
        enable it.

*   New features:

    -   Filling for definition list items. ([GH-13][])
    -   Added option `markdown-gfm-downcase-languages` to use
        lowercase language name in GFM code blocks.
        ([GH-71][], [GH-73][])
    -   Customizable live preview window split direction via
        `markdown-split-window-direction`.  ([GH-129][], [GH-188][])
    -   Variable-height headings via
        `markdown-header-scaling`. ([GH-121][])
    -   Implement inline image previews via
        `markdown-toggle-inline-images` and `C-c C-i C-t`.
        Thanks to Syohei Yoshida.  ([GH-122][], [GH-128][])
    -   Added `markdown-wiki-link-search-subdirectories` to enable
        searching for wiki link files in subdirectories. ([GH-174][])
    -   Added option to automatically continue lists when `RET` is
        pressed.  `markdown-indent-on-enter` now has three settings.
        ([GH-179][])
    -   Match fenced code blocks with language and info strings.
        ([GH-184][])
    -   Add smart Markdown block navigation commands `M-{`
        and `M-}`.  These replace the
        regular-expression-based "paragraph" movement commands
        provided by Emacs, which do not recognize Markdown syntax
        (e.g., headings inside of code blocks).  Also use
        `M-h` for marking a block and `C-x n b` to
        narrow to a block.
    -   Add `markdown-nested-imenu-heading-index` as a customizable
        option.  It may be disabled to instead generate a flat imenu
        index.
    -   Basic font lock and filling for definition lists.  As a side
        effect, list item navigation and movement should also work.
    -   Add command for toggling GFM task list items via
        `C-c C-c C-x` (`markdown-toggle-gfm-checkbox`).
    -   Ability to toggle wiki link support via a new custom variable
        `markdown-enable-wiki-links`.  This may be set in a file local
        variable.  Also added function `markdown-toggle-wiki-links`
        and a menu item.

*   Improvements:

    -   Menubar reorganization.  Grouped related actions together,
        added missing commands, and added several toggle options to
        the menu. ([GH-147][])
    -   Use `toggle` menu style for macOS compatibility.
    -   Remove autoload for `.text` files.  Thanks to Steve Purcell.
        ([GH-118][])
    -   Set own `adaptive-fill-regexp` so that `fill-paragraph` works
        for list items.  Thanks to Syohei Yoshida for the patch.
        ([GH-79][], [GH-80][])
    -   Suppress minibuffer output when generated HTML is small.
        Thanks to Syohei Yoshida.  ([GH-83][], [GH-86][])
    -   Use GitHub fetcher for `markdown-mode` on MELPA.  ([GH-84][])
    -   Improve fenced code block parsing.  Thanks to Danny McClanahan.
        ([GH-85][], [GH-95][])
    -   Markdown Mode is now automatically tested against Emacs
        24.1-24.5 and 25.1-25.2.  ([GH-99][])
    -   Make live-preview mode follow min or max point.  Thanks to
        Danny McClanahan.  ([GH-102][])
    -   Improved font-lock performance. ([GH-119][])
    -   Maintain cursor position when indenting instead of moving to
        the beginning of the line.  Thanks to Isaac Hodes.
        ([GH-125][])
    -   Add used language names to front of list of known languages.
        ([GH-135][])
    -   Support basic TOML metadata.  Thanks to Jorge Israel Peña.
        ([GH-137][])
    -   Prohibit setext heading text from starting with hyphens,
        spaces or tabs, so that there is no ambiguity between setext
        headings and in-progress lists.  ([GH-139][], [GH-143][])
    -   Ignore heading lines in `fill-paragraph`.  Thanks to Syohei
        Yoshida.  ([GH-159][], [GH-162][])
    -   Improve matching of multiple math blocks with non-math text in
        between.  Thanks to Dave Kleinschmidt for a patch.
        ([GH-168][])
    -   Prevent `fill-paragraph` from filling lines in code blocks.
        ([GH-169][])
    -   Fix font lock for links with URLs containing parentheses.
        ([GH-170][])
    -   `fill-paragraph` now respects paragraph boundaries within
        blockquotes.  ([GH-186][])
    -   Set mark when calling `markdown-up-heading`.
    -   Improved font locking after empty GFM code block insertion.
    -   Fix spurious italics from underscores in URLs.
    -   Respect `font-lock-mode` being nil.  Only call
        `font-lock-refresh-defaults` if `font-lock-mode` is non-nil to
        prevent it from being turned on when disabled by user.  Thanks
        to Tom May for the patch.
    -   Fix list item insertion on ordered lists with hash marks
        (Pandoc "fancy lists").
    -   Treat polymode blocks as code blocks when parsing the buffer.
    -   Require whitespace atx heading hashmarks, as required by the
        original atx specification (but not enforced by Markdown.pl).
        The benefit is that it prevents false positives for #hashtags
        and things like "Engine #1" when lines wrap.
    -   Complete heading markup when point is on an setext heading and
        `markdown-insert-header-dwim` is invoked
        (`C-c C-t h`).
    -   Better point position after inserting asymmetric atx headings.

*   Bug fixes:

    -   Fix `scripts/get-recognized-gfm-languages.el`, which skipped
        languages with spaces.  ([GH-72][], [GH-82][])
    -   `README.md` specified Arch (AUR) package (`emacs-goodies-el`),
        which did not exist.  ([GH-74][])
    -   Don't accidentally override user entries in `auto-mode-alist`.
        ([GH-127][])
    -   Fix `markdown-cycle` issue with heading-like strings in code
        blocks.  Thanks to Syohei Yoshida.  ([GH-75][], [GH-76][])
    -   Fix moving same level heading over code block issue.  Thanks
        to Syohei Yoshida.  ([GH-77][], [GH-78][])
    -   Don't insert empty title strings for links.  Thanks to
        Sebastian Wiesner for the patch.  ([GH-89][])
    -   Fix possible infinite loop in `markdown-cleanup-list-numbers`.
        Thanks to Danny McClanahan.  ([GH-98][], [GH-100][])
    -   Fix an args-out-of-range error due to the syntax-propertize
        function returning point which is larger than `point-max`.
        Thanks to Syohei Yoshida. ([GH-142][])
    -   Respect narrowed region in `markdown-find-previous-prop`.
        Thanks to Vitalie Spinu.  ([GH-109][])
    -   Move point at least 1 char in
        `markdown-match-propertized-text` to avoid possible infinite
        loop in font-lock.  Thanks to Vitalie Spinu.  ([GH-110][])
    -   Fix issues where buffers could be marked as modified when no
        modifications were made.  ([GH-115][], [GH-116][], [GH-146][])
    -   Fix an issue where comments of the form `<!-- > comment -->`
        were not correctly identified.  ([GH-117][])
    -   Prevent spurious bold fontification.  Thanks to Kévin Le
        Gouguec.  ([GH-124][])
    -   Keep metadata visible when cycling visibility.  ([GH-136][])
    -   `markdown-syntax-propertize-extend-region` should not
        overwrite match-data, which caused issues with
        `replace-regexp`, etc.  ([GH-104][], [GH-105][])
    -   Don't list heading-like lines in code blocks or metadata in
        imenu.  Thanks to Syohei Yoshida.  ([GH-145][], [GH-154][])
    -   Fix an issue where fill paragraph wouldn't work following
        unclosed left square brackets.  ([GH-148][], [GH-161][])
    -   Fix default language presented when inserting GFM code blocks.
        Thanks to Conal Elliot for a patch.  ([GH-152][])
    -   Backspace now always deletes characters if a region is
        specified.  Thanks to Syohei Yoshida.
        ([GH-166][], [GH-167][])
    -   Fix `markdown-header-face` inherit from nil error, e.g., when
        exporting HTML from an Org mode file containing a Markdown
        source block.  Thanks to Moogen Tian for a patch.
        ([GH-190][], [GH-193][])
    -   Inserting a reference link no longer causes an "args out of
        range" commit error when the existing reference label is a
        single commit character.
    -   Fix to honor location setting when inserting reference
        definitions.
    -   Fixed an issue where, if there is special markup at the end of
        the buffer, deleting a character backward would cause the
        font-lock faces to disappear.
    -   Fix incorrect matching of italic text due to underscores in
        math mode.  Thanks also to Dave Kleinschmidt.
    -   Fix italic highlighting issue when each line or both lines are list.
    -   Handle false positive italics across list items.

  [gh-13]: https://github.com/jrblevin/markdown-mode/issues/13
  [gh-26]: https://github.com/jrblevin/markdown-mode/issues/26
  [gh-66]: https://github.com/jrblevin/markdown-mode/issues/66
  [gh-71]: https://github.com/jrblevin/markdown-mode/issues/71
  [gh-72]: https://github.com/jrblevin/markdown-mode/issues/72
  [gh-73]: https://github.com/jrblevin/markdown-mode/issues/73
  [gh-74]: https://github.com/jrblevin/markdown-mode/issues/74
  [gh-75]: https://github.com/jrblevin/markdown-mode/issues/75
  [gh-76]: https://github.com/jrblevin/markdown-mode/pull/76
  [gh-77]: https://github.com/jrblevin/markdown-mode/pull/77
  [gh-78]: https://github.com/jrblevin/markdown-mode/pull/78
  [gh-79]: https://github.com/jrblevin/markdown-mode/issues/79
  [gh-80]: https://github.com/jrblevin/markdown-mode/pull/80
  [gh-82]: https://github.com/jrblevin/markdown-mode/pull/82
  [gh-83]: https://github.com/jrblevin/markdown-mode/issues/83
  [gh-84]: https://github.com/jrblevin/markdown-mode/issues/84
  [gh-86]: https://github.com/jrblevin/markdown-mode/pull/86
  [gh-85]: https://github.com/jrblevin/markdown-mode/issues/85
  [gh-89]: https://github.com/jrblevin/markdown-mode/pull/89
  [gh-91]: https://github.com/jrblevin/markdown-mode/pull/91
  [gh-95]: https://github.com/jrblevin/markdown-mode/pull/95
  [gh-98]: https://github.com/jrblevin/markdown-mode/issues/98
  [gh-99]: https://github.com/jrblevin/markdown-mode/pull/99
  [gh-100]: https://github.com/jrblevin/markdown-mode/pull/100
  [gh-102]: https://github.com/jrblevin/markdown-mode/pull/102
  [gh-104]: https://github.com/jrblevin/markdown-mode/issues/104
  [gh-105]: https://github.com/jrblevin/markdown-mode/pull/105
  [gh-109]: https://github.com/jrblevin/markdown-mode/pull/109
  [gh-110]: https://github.com/jrblevin/markdown-mode/pull/110
  [gh-115]: https://github.com/jrblevin/markdown-mode/issues/115
  [gh-116]: https://github.com/jrblevin/markdown-mode/pull/116
  [gh-117]: https://github.com/jrblevin/markdown-mode/issues/117
  [gh-118]: https://github.com/jrblevin/markdown-mode/pull/118
  [gh-119]: https://github.com/jrblevin/markdown-mode/issues/119
  [gh-121]: https://github.com/jrblevin/markdown-mode/issues/121
  [gh-122]: https://github.com/jrblevin/markdown-mode/issues/122
  [gh-124]: https://github.com/jrblevin/markdown-mode/issues/124
  [gh-125]: https://github.com/jrblevin/markdown-mode/pull/125
  [gh-127]: https://github.com/jrblevin/markdown-mode/issues/127
  [gh-128]: https://github.com/jrblevin/markdown-mode/pull/128
  [gh-129]: https://github.com/jrblevin/markdown-mode/issues/129
  [gh-132]: https://github.com/jrblevin/markdown-mode/pull/132
  [gh-135]: https://github.com/jrblevin/markdown-mode/issues/135
  [gh-136]: https://github.com/jrblevin/markdown-mode/issues/136
  [gh-137]: https://github.com/jrblevin/markdown-mode/issues/137
  [gh-139]: https://github.com/jrblevin/markdown-mode/issues/139
  [gh-142]: https://github.com/jrblevin/markdown-mode/pull/142
  [gh-143]: https://github.com/jrblevin/markdown-mode/issues/143
  [gh-145]: https://github.com/jrblevin/markdown-mode/issues/145
  [gh-154]: https://github.com/jrblevin/markdown-mode/pull/154
  [gh-146]: https://github.com/jrblevin/markdown-mode/pull/146
  [gh-147]: https://github.com/jrblevin/markdown-mode/issues/147
  [gh-148]: https://github.com/jrblevin/markdown-mode/issues/148
  [gh-152]: https://github.com/jrblevin/markdown-mode/issues/152
  [gh-155]: https://github.com/jrblevin/markdown-mode/issues/155
  [gh-156]: https://github.com/jrblevin/markdown-mode/issues/156
  [gh-157]: https://github.com/jrblevin/markdown-mode/pull/157
  [gh-159]: https://github.com/jrblevin/markdown-mode/issues/159
  [gh-161]: https://github.com/jrblevin/markdown-mode/issues/161
  [gh-162]: https://github.com/jrblevin/markdown-mode/pull/162
  [gh-166]: https://github.com/jrblevin/markdown-mode/issues/166
  [gh-167]: https://github.com/jrblevin/markdown-mode/pull/167
  [gh-168]: https://github.com/jrblevin/markdown-mode/pull/168
  [gh-169]: https://github.com/jrblevin/markdown-mode/issues/169
  [gh-170]: https://github.com/jrblevin/markdown-mode/issues/170
  [gh-174]: https://github.com/jrblevin/markdown-mode/issues/174
  [gh-179]: https://github.com/jrblevin/markdown-mode/issues/179
  [gh-184]: https://github.com/jrblevin/markdown-mode/issues/184
  [gh-186]: https://github.com/jrblevin/markdown-mode/issues/186
  [gh-188]: https://github.com/jrblevin/markdown-mode/pull/188
  [gh-190]: https://github.com/jrblevin/markdown-mode/pull/190
  [gh-193]: https://github.com/jrblevin/markdown-mode/issues/193

## Markdown Mode 2.1

*January 9, 2016*

Version 2.1 is a major new stable release and all users are encouraged
to upgrade.  The many new features and bug fixes included are
described below.

Markdown Mode is developed and tested primarily for compatibility with
GNU Emacs versions 24.3 and later.  It requires `cl-lib` version 0.5
or later.  This library has been bundled with GNU Emacs since version
24.3.  Users of GNU Emacs 24.1 and 24.2 can install `cl-lib` using
`M-x package-install RET cl-lib`.

This release of Markdown Mode contains patches written by many
individuals including Masayuki Ataka, Jonas Bernoulli, Roger Bolsius,
Daniel Brotsky, Julien Danjou, Samuel Freilich, David Glasser, Marijn
Haverbeke, Antonis Kanouras, Keshav Kini, Vasily Korytov, Danny
McClanahan, Matt McClure, Howard Melman, Makoto Motohashi, Jon
Mountjoy, Pierre Neidhardt, Spanti Nicola, Paul W. Rankin, Christophe
Rhodes, Tim Visher, and Syohei Yoshida.  Many others also submitted
bug reports. Thanks to everyone for your contributions.

*   **Breaking changes:**

    -   In GFM Mode, `visual-line-mode` is no longer enabled by
        default.  A `gfm-mode-hook` was added, which could be used to
        keep `visual-line-mode` on by default in `gfm-mode`.
        ([GH-31][])

*   New features:

    -   Add automatically updating live preview functionality
        (`C-c C-c l`) via the native `eww` browser.
        ([GH-36][], [GH-53][], [GH-57][], [GH-58][], [GH-63][])
    -   Use `autoload` to enable `markdown-mode` in `auto-mode-alist`
        for files with `.text`, `.markdown`, and `.md` extensions.
    -   Use Travis CI for automated build testing.
    -   ATX heading subtree promotion and demotion via
        `M-S-LEFT`, and `M-S-RIGHT`.
    -   ATX heading subtree moving up and down via `M-S-UP`
        and `M-S-DOWN`.
    -   Convert inline links to reference links when
        `markdown-insert-reference-link-dwim` is used when the point
        is at an inline link.
    -   Allow linking to multiple stylesheets in `markdown-css-paths`
        list.  Use stylesheets for both preview and export. Previous
        `markdown-css-path` (singular) is now deprecated.
    -   Customizable default unordered list marker via
        `markdown-unordered-list-item-prefix`.
    -   Add asymmetric ATX heading adornment option
        `markdown-asymmetric-header`.
    -   Font lock for `kbd` tags.
    -   Support GFM-style code blocks in `markdown-mode` (as well as
        `gfm-mode`).  ([GH-2][])
    -   New function `markdown-electric-backquote` will prompt for a
        language name for GFM code blocks.  This can be disabled by
        customizing the variable
        `markdown-gfm-use-electric-backquote`.  ([GH-9][])
    -   Completion of programming language names for GFM code blocks.
        A list of pre-defined languages is included, but this can be
        augmented by setting `markdown-gfm-additional-languages`.
        ([GH-38][], [GH-54][], [GH-59][], [GH-60][], [GH-64][])
    -   Strikethrough support in `gfm-mode`.
    -   Support for GFM toggling checkboxes `mouse-1` or
        `RET`.  This is controlled by a new custom variable,
        `markdown-make-gfm-checkboxes-buttons`.  Thanks to Howard
        Melman for a patch.  ([GH-7][])
    -   Font lock and filling for Pandoc "fancy lists," which use `#`
        as the list marker.  ([GH-3][])
    -   Basic support for filling of definition lists.  ([GH-20][])
    -   Support [Ikiwiki](http://ikiwiki.info/)-style search for wiki links that allows
        links relative to parent directories.  Enable this by setting
        `markdown-wiki-link-search-parent-directories`.
        ([GH-8][], [GH-21][])

*   Improvements:

    -   General font lock improvements for comments, code blocks,
        blockquotes, headings, horizontal rules, bold, and italics.
        ([GH-67][], [GH-68][])
    -   Separate highlighting for Markdown markup characters
        (asterisks, underscores, backquotes, etc.) to aid in
        readability.
    -   Font lock for bold, italics, and LaTeX math work inside block
        elements such as headings and blockquotes.  ([GH-39][])
    -   Display a link to the GitHub repository in the MELPA
        description.  ([GH-37][])

*   Bug fixes:

    -   Fix bug in `markdown-complete-region/buffer` where level-two
        Setext headings could be confused with horizontal rules.
        Includes a unit test.  Thanks to Gunnar Franke for the report.
    -   Fix filling when a decimal number appears at column zero,
        which could be confused with an ordered list item.
    -   Fix buffer-wide markup completion.
    -   Fix font-lock for GFM code blocks without language keywords.
    -   Improved Setext header insertion to support wide characters.
    -   Fix expensive `paragraph-separate` regular expression.
    -   Make `comment-auto-fill-only-comments` a buffer-local
        variable, which allows for better default filling behavior in
        cases where the global variable is non-`nil`.
    -   Fix Emacs 23 compatibility by checking for
        `font-lock-refresh-defaults` before calling it.
    -   Handle reference definitions when filling paragraphs.
    -   Improve filling of list items with indentation.
    -   Properly handle footnotes when filling.
    -   Fix issues with markdown-footnote-kill and related functions.
    -   Improve font lock for fenced code blocks.
    -   Avoid avoid overwriting source file when exporting if source
        file has `.html` extension.
    -   Fix and improve ordered list behavior to preserve digit
        spacing and avoid an infinite loop in certain cases.  Adjust
        ordered list whitespace when marker digit count increases.
    -   Improve reference definition regular expression to avoid
        matching multiple reference links in one line.
    -   Allow spaces in fenced code language identifiers.  ([GH-22][])
    -   Improve font lock for preformatted blocks and fenced code
        blocks.
    -   Fix out-of-order HTML output.  ([GH-14][])
    -   Add console-friendly backspace and tab bindings.  ([GH-15][])
    -   Better treatment of files without extensions for wiki links.
        When files have no extensions, don't append a lone period.
        ([GH-23][])
    -   Call `looking-back` with two arguments for compatibility with
        Emacs 25.1.
    -   Make `(beginning-of-defun -1)` go to next title when point is
        at beginning of defun.  ([GH-34][])
    -   Ignore headings in code blocks for font lock, movement, and
        visibility cycling.
        ([GH-27][], [GH-33][], [GH-35][], [GH-40][], [GH-41][])
    -   Don't highlight wiki links in code blocks.  ([GH-17][])
    -   Don't move to links in code blocks with `C-c C-p` and
        `C-c C-n`.
    -   Fix hanging indentation for list items and single-line
        preformatted blocks.  ([GH-16][], [GH-28][], [GH-30][])
    -   Better rejection of false positives for italics with respect
        to other inline elements (inline code and bold).
    -   Predicate functions should not modify match data.
    -   Use correct list marker from previous list level when using
        `C-u M-RET` to insert a dedented list item.  Prevent
        an infinite loop in some cases.  ([GH-4][])
    -   Reduce lag when scrolling or inserting text into large files.
        ([GH-30][], [GH-101][])
    -   Avoid confusing tramp errors with malformed wiki links.
        ([GH-65][])

  [gh-2]: https://github.com/jrblevin/markdown-mode/pull/2
  [gh-3]: https://github.com/jrblevin/markdown-mode/pull/3
  [gh-4]: https://github.com/jrblevin/markdown-mode/issues/4
  [gh-7]: https://github.com/jrblevin/markdown-mode/issues/7
  [gh-8]: https://github.com/jrblevin/markdown-mode/issues/8
  [gh-9]: https://github.com/jrblevin/markdown-mode/issues/9
  [gh-14]: https://github.com/jrblevin/markdown-mode/issues/14
  [gh-15]: https://github.com/jrblevin/markdown-mode/issues/15
  [gh-16]: https://github.com/jrblevin/markdown-mode/issues/16
  [gh-17]: https://github.com/jrblevin/markdown-mode/issues/17
  [gh-18]: https://github.com/jrblevin/markdown-mode/issues/18
  [gh-20]: https://github.com/jrblevin/markdown-mode/issues/20
  [gh-21]: https://github.com/jrblevin/markdown-mode/issues/21
  [gh-22]: https://github.com/jrblevin/markdown-mode/issues/22
  [gh-23]: https://github.com/jrblevin/markdown-mode/issues/23
  [gh-27]: https://github.com/jrblevin/markdown-mode/issues/27
  [gh-28]: https://github.com/jrblevin/markdown-mode/issues/28
  [gh-30]: https://github.com/jrblevin/markdown-mode/issues/30
  [gh-31]: https://github.com/jrblevin/markdown-mode/issues/31
  [gh-32]: https://github.com/jrblevin/markdown-mode/pull/32
  [gh-33]: https://github.com/jrblevin/markdown-mode/issues/33
  [gh-34]: https://github.com/jrblevin/markdown-mode/pull/34
  [gh-35]: https://github.com/jrblevin/markdown-mode/pull/35
  [gh-36]: https://github.com/jrblevin/markdown-mode/pull/36
  [gh-37]: https://github.com/jrblevin/markdown-mode/issues/37
  [gh-38]: https://github.com/jrblevin/markdown-mode/issues/38
  [gh-39]: https://github.com/jrblevin/markdown-mode/issues/39
  [gh-40]: https://github.com/jrblevin/markdown-mode/pull/40
  [gh-41]: https://github.com/jrblevin/markdown-mode/pull/41
  [gh-53]: https://github.com/jrblevin/markdown-mode/pull/53
  [gh-54]: https://github.com/jrblevin/markdown-mode/pull/54
  [gh-57]: https://github.com/jrblevin/markdown-mode/pull/57
  [gh-58]: https://github.com/jrblevin/markdown-mode/pull/58
  [gh-59]: https://github.com/jrblevin/markdown-mode/pull/59
  [gh-60]: https://github.com/jrblevin/markdown-mode/pull/60
  [gh-63]: https://github.com/jrblevin/markdown-mode/pull/63
  [gh-64]: https://github.com/jrblevin/markdown-mode/pull/64
  [gh-65]: https://github.com/jrblevin/markdown-mode/pull/65
  [gh-67]: https://github.com/jrblevin/markdown-mode/pull/67
  [gh-68]: https://github.com/jrblevin/markdown-mode/pull/68
  [gh-101]: https://github.com/jrblevin/markdown-mode/issues/101

## Markdown Mode 2.0

*March 24, 2013*

Version 2.0 is a major new stable release with many new features,
including some changes to keybindings for element insertion and
outline navigation.  In summary, Markdown Mode now has improved
keybindings, smarter markup insertion commands, a general markup
removal command, markup completion (normalization), markup promotion
and demotion, list and region editing, many syntax highlighting
improvements, new and improved movement commands, and generalized link
following and movement.

*   **Breaking changes:**

    -    Physical style element insertion commands prefixed by
         `C-c C-p` have been removed in favor of their
         logical style counterparts prefixed by `C-c C-s`.
    -    Shift is now the preferred way to distinguish keybindings for
         two related elements.  For example, you can insert an inline
         link with `C-c C-a l` or a reference link with
         `C-c C-a L`.  The latter keybinding is new and
         preferred over `C-c C-a r`, which is deprecated.
    -    Footnote keybindings have been moved away from the
         `C-c C-f n` prefix.
    -    Several other new keybindings have been introduced and are
         described in more detail below.
    -    Removed wiki link following with `RET` and
         `markdown-follow-wiki-link-on-enter` setting.  Use the
         unified following (open link) command `C-c C-o`
         instead.

*   New features:

    -    Fast heading insertion with a single command which
         automatically calculates the type (atx or setext) and level.
         Prefix with `C-u` to promote the heading by one
         level or `C-u C-u` to demote the heading by one
         level.  Headings with a specific level or type can still be
         inserted quickly with specific keybindings.
    -    Easily kill an element (e.g., a link or reference definition)
         at the point with `C-c C-k` and store the most
         important part in the kill ring (e.g., the link text or URL).
    -    Markup completion (`C-c C-]`) normalizes the markup
         for an element (e.g., it balances hash marks and removing
         extra whitespace for atx headings).
    -    Markup promotion and demotion via `C-c C--` and
         `C-c C-=`, respectively.  The sequences
         `M-<up>` and `M-<down>` may
         also be used.
    -    List editing: move list items up and down with
         `M-<up>` and `M-<down>`.
         Indent and exdent list items with `M-<left>`
         and `M-<right>`.
    -    Region editing: indent and exdent regions, with tab stops
         determined by context, using `C-c <` and
         `C-c >` (as in `python-mode`).
    -    Smart list item insertion with `M-RET`, with
         indentation and marker determined by the surrounding context.
         Prefix with `C-u` to decrease the indentation by one
         level or `C-u C-u` to increase the indentation one
         level.
    -    Quickly jump between reference definitions and
         reference-style links and between footnote markers and
         footnote text with `C-c C-j`.  Create undefined
         references when jumping from a reference link.  When jumping
         back, present a buffer with buttons for selecting which link
         to jump to.
    -    Revised outline navigation commands, following `org-mode`.
         This frees up the sexp navigation keys `C-M-f` and
         `C-M-b` which can be useful in Markdown documents
         which have many matching delimiters, as well as the defun
         navigation keys `C-M-a` and `C-M-e`.
    -    Previous/next section movement with `C-M-a` and
         `C-M-e` (in Emacs parlance, this is movement by
         defun).  Mark the current section with `C-M-h`.
    -    Previous/next paragraph movement via `M-{` and
         `M-}`.
    -    Previous/next block movement with `C-u M-{` and
         `C-u M-}`.
    -    Customizable reference link location via
         `markdown-reference-location`.
    -    Font lock for title strings in inline links.
    -    Subtle syntax highlighting for hard line breaks.
    -    In GFM Mode, change italic font lock behavior to match GFM
         specification regarding underscores in words.
    -    Insertion command (`C-c C-s P`) for GFM quoted code
         blocks.
    -    Syntax highlighting for MultiMarkdown metadata and Pandoc
         title blocks.
    -    Added before and after export hooks
         `markdown-before-export-hook` and
         `markdown-after-export-hook`.
    -    Added a library of regression tests which currently contains
         160 unit tests.

*   Improvements:

    -    ATX heading insertion will use current line as heading text
         if not blank and there is no active region.
    -    Setext heading insertion will prompt for heading title when
         there is no active region.
    -    When the point is at a heading, the heading insertion
         commands will replace the heading at point with a heading of
         the requested level and type.
    -    When there is no active region, the bold, italic, code, link,
         and image insertion commands will operate on the word at
         point, if any, so that you don't have to have an active
         selection for simple modifications.
    -    Repeating the bold, italic, or code insertion commands when
         the point is at an element of the corresponding type will
         remove the markup.
    -    Indentation of preformatted text and blockquotes will be
         adjusted automatically in contexts where more indentation is
         required, as in nested lists.  (For example, in Markdown, a
         preformatted text block inside a first-level list item must
         have eight spaces of indentation.)
    -    Improved reference link insertion with label completion:
           + Use word at point as link text, if possible, when there
             is no active region.
           + Tab completion of reference labels from the set of
             currently defined references.
           + Reference link insertion no longer prompts for a URL or
             title if the label is already defined.
           + If no URL is given, create an empty reference definition
             and move the point there.
    -    Basic reference-style image markup insertion.
    -    Multiple horizontal rule styles, `markdown-hr-strings`,
         customizable as a list of strings, which can be cycled
         through.
    -    New URL insertion command for inserting plain URLs delimited
         by angle brackets (`C-c C-a u`).  Works on URL at
         point, if any, when there is no active region.
    -    Generally improved insertion commands with respect to
         insertion of surrounding whitespace and point position after
         insertion (e.g., ensuring blank lines before and after newly
         inserted headings and horizontal rules).
    -    Unified link following: open links in a browser and wiki
         links in a new buffer with the same keybinding (`C-c
         C-o`).  This supersedes the separate wiki link following
         command (`C-c C-w`).
    -    Generalized link movement and following: move between and
         open all link types (inline, reference, wiki, angle URIs)
         using the same key bindings (`M-n` and
         `M-p`).  Previously, these commands only moved
         between wiki links, but with the above following enhancement,
         moving between hyperlinks of all types is more useful.
    -    Syntax highlighting for GFM quoted code blocks with an
         optional language keyword.
    -    Dynamic loading and unloading for math support with
         refontification.
    -    Allow underscores and colons in equation labels in math mode.
    -    Syntax highlighting improvements: faster identification of
         preformatted blocks.  Markdown Mode adheres to the four space
         rule for nested list items: in a list item of level _n_,
         preformatted text must be indented at least 4(_n_ + 1)
         spaces.
    -    More inclusive blockquote regular expression highlights
         blockquotes with leading indentation, when appropriate.
    -    Regular expression optimization for URI matching.
    -    Numerous other improvements for more accurate syntax
         highlighting.
    -    Respect hard line breaks when filling paragraphs.
    -    Add indentation positions: preceding list markers and pre
         block position.
    -    Use button-map for navigating undefined references, so that
         references can be navigated via the keyboard by pressing
         `TAB` and `S-TAB`.
    -    Use newer `use-region-p` when possible to check for active
         region, with fallbacks for older Emacsen and Xemacs.
    -    Clean up whitespace after deleting footnote text.
    -    Use adaptive filling for list items and blockquotes.
    -    Treat all list items (any marker type) the same way with respect
         to filling.
    -    Retain the `>` prefix when filling blockquotes.
    -    Fill list items inside of blockquotes.
    -    Numerous other internal improvements to make the code base
         more robust.

*   Bug fixes:

    -    Fix bug in heading visibility cycling introduced in version
         1.9 where the level of headings was being calculated
         incorrectly.
    -    Fix problems inserting ATX headings at end of buffer.
    -    Support small Setext headings (with fewer than three
         characters).
    -    Several improvements to inline code syntax highlighting.
    -    Fix some edge cases regarding escaping, spaces, etc. for bold
         and italic font lock.
    -    Prohibit newlines and tabs immediately after opening bold and
         italic delimiters.  This fixes a bug where italics would not
         be highlighted following a horizontal rule
    -    Improved multi-line font lock performance for large files.
    -    Improved multi-line font lock at beginning of buffer.
    -    List items with any of the three markers are filled in the
         same way (previously list items starting with `+` were not
         filled with hanging indentation).
    -    Fix end detection for empty list items.  Don't skip over the
         whitespace following the marker on the same line.
         Previously, empty list items were not being detected properly
         by `markdown-cur-list-item-bounds` as a result of this.
    -    Don't exclude `[^]`, which is a valid reference tag (but
         let's please stick to alphanumeric characters).
    -    No longer highlight escaped wiki links.
    -    Fix line number buttons for reference checking and make all
         buttons clickable.
    -    Fix killing of footnotes with no text.
    -    Fix escaping in `markdown-xhtml-standalone-regexp`.
    -    Fix a font-lock edge case involving footnote markers
         preceding inline links.
    -    More accurate font-lock for ATX headings in edge cases.
    -    Fix killing of footnotes from footnote text.

--- ---

## Markdown Mode 1.9

*January 1, 2013*

Version 1.9 is a major new stable release with important bug fixes.

*   New features:

    -    Support for setext-style headers in `outline-mode`.  Thanks
         to Shigeru Fukaya.
    -    Font lock for tilde-fenced code blocks.
    -    Reference link insertion (`C-c C-a r`).
    -    Support two forms of aliased wiki links:
         `[[link text|PageName]]` and `[[PageName|link text]]`
         (`markdown-wiki-link-alias-first`).
    -    Footnote support: font lock and insertion, deletion, and
         navigation functions for footnotes (prefixed by
         `C-c C-f`; following for wiki links is now
         `C-c C-w`).  Thanks to Joost Kremers.
    -    Improved preview and export commands.  Thanks to Donald
         Ephraim Curtis.
    -    `imenu` support.  Thanks to Akinori Musha.
    -    Added autoload token for `gfm-mode`.  Thanks to Max Penet and
         Peter Eisentraut for the suggestion.
    -    Optional character set declaration in XHTML output.  Thanks
         to François Gannaz for the suggestion.
    -    Smart unindentation when pressing delete at the beginning of
         a line.  Thanks to Zhenlei Jia.
    -    Optional prefix argument to `C-c C-w` for opening
         wiki links in another window.
    -    Open inline and reference links and inline URIs in browser
         (`C-c C-o`).  Thanks to Peter Jones.
    -    Open files in a standalone previewer or editor
         (`C-c C-c o`).
    -    Clean up numbered/ordered lists (`C-c C-c n`).
         Thanks to Donald Ephraim Curtis.
    -    Save Markdown output to kill ring (copy to clipboard)
         (`C-c C-c w`).  Thanks to Donald Ephraim Curtis.

*   Improvements:

    -    Improve `markdown-mode-hook` docstring.  Thanks to Shigeru
         Fukaya for the more precise description.
    -    Don't require Common Lisp extensions at run time.  Thanks to
         Shigeru Fukaya.
    -    Prefer `visual-line-mode`, the replacement for
         `longlines-mode`, when in `gfm-mode`.  Thanks to Christopher
         J. Madsen.
    -    Proper GitHub wiki link handling in `gfm-mode`.  Thanks to
         Kevin Porter.
    -    XEmacs compatibility:
        -    Avoid malformed list errors during font definitions.
        -    Handle replace-regexp-in-string.
        -    Use text properties instead of overlays.
        -    Fall back to `set-buffer-modified-p` when
            `restore-buffer-modified-p` is unavailable.
        -    Many additional fixes.  Thanks to Michael Sperber.
    -    Handle wiki links in buffers not associated with a file.
    -    Update autoload documentation to support byte compilation.
    -    Option to use `-` instead of `_` for wiki links in
         `gfm-mode`.
    -    Add two tab stops to possible indentation positions following
         list items.

*   Bug fixes:

    -    Fixed a bug which caused unusual behavior in functions
         performing search, replace, and/or matching operations.
         Thanks to Christopher J. Madsen for the patch.
    -    Fixed a bug which caused an incompatibility with
         `orgtbl-mode`.  Thanks to Vegard Vesterheim for the report
         and to Carsten Dominik for a patch.
    -    Fixed a bug where reference links at the beginning of a line
         would be mistaken for reference definitions.
    -    Improved font lock for headers.
    -    Improved font lock for reference definitions.  Thanks to Ian
         Yang.
    -    Avoid byte compiler warning about `region-exists-p` in GNU
         Emacs.
    -    Additional key bindings for cross-platform header cycling
         compatibility.
    -    Fix problem with externally modified files on disk where the
         user would get stuck in a loop answering "really edit the
         buffer?"  Thanks to Bryan Fink for a detailed report.
    -    Font lock fix for URLs with underscores.
    -    Escape shell commands to handle filenames with spaces, etc.
         Thanks to Marcin Kasperski for a patch.
    -    Use `:slant` instead of unsupported `:italic` in font spec.
    -    Fix typo in `paragraph-fill` regexp.

## Markdown Mode 1.8.1

*August 15, 2011*

Version 1.8.1 is a bugfix version which addresses some minor issues in
version 1.8.

*   New features:

    -    Wiki link features now support aliased or piped wiki links of
         the form `[[PageName|link text]]`.

*   Bug fixes:

    -    Fixed an issue, reported by Werner Dittmann, where the
         default indentation position would be skipped over when
         `auto-fill-mode` was on.  This meant that when writing a
         normal paragraph, for example, the line would wrap to column
         4 instead of column 1.
    -    Require the `cl` package for `multiple-value-bind`.  Thanks
         to Werner Dittman for noticing this.
    -    Remove a leftover debug print message which would announce
         "ENTER" in the minibuffer when the enter key was pressed.

## Markdown Mode 1.8

*August 12, 2011*

*   New features:

    -   Add support for following wiki links in a buffer, either with
        `C-c C-f` or, optionally, `RET`, when the
        point is at a wiki link.
    -   Support Markdown processors which do not accept input from
        stdin (i.e, Python-Markdown) by adding the
        `markdown-command-needs-filename` custom option.  Thanks to
        Jeremiah Dodds for a patch.
    -   GitHub-Flavored Markdown mode (`gfm-mode`) turns on
        `longlines-mode` and `auto-fill-mode`.  Thanks to Edward
        O'Connor for the patch.
    -   Add outline-mode-like keybindings for fast outline navigation
        of atx-style headers (see the updated documentation for
        details).
    -   Arbitrary content may to be added to the `<head>` block during
        HTML output to allow for flexible local customizations.  See
        `markdown-xhtml-header-content`.
    -   New HTML export options: export to a file with
        `C-c C-c e` or export to a file and view with
        `C-c C-c v`.
    -   Support Markdown processors which produce standalone output
        (i.e., complete HTML documents).  If this is detected, by
        matching `markdown-xhtml-standalone-regexp` in the first five
        lines of output, then omit the `markdown-mode` header and
        footer.  Thanks to Philippe Ivaldi for this and the previous
        HTML-output-related patches.
    -   Customizable wiki link following behavior using
        `markdown-follow-wiki-link-on-enter`.
    -   Quick navigation to the previous and next wiki links using
        `M-p` and `M-n`.
    -   Wiki links to non-existent files (missing links) are
        highlighted differently.  Based on functionality from
        `wiki-mode` by Alex Schroeder.  Thanks to Eric Merritt for
        patches.

*   Improvements:

    -   Improve syntax highlighting of preformatted text blocks to
        better distinguish them from list items with hanging
        indentation and nested list items.
    -   Match italic and bold text and inline code fragments across
        line breaks, within a single block, but prevent them from
        matching across blocks.
    -   Generally improve multi-line font lock by extending the search
        region to include full blocks.
    -   Make indentation work when the previous line is not indented.
    -   Set tab width to 4, the natural tab width for Markdown
        documents.
    -   Stop announcing "TAB" in the minibuffer when tab is pressed.
    -   Use `html-mode` for viewing Markdown output.
    -   Ensure Markdown output buffer is always raised, even when the
        output is short.
    -   Make sure horizontal rules inserted by `C-c -`
        (`markdown-insert-hr`) are surrounded by blank lines.
    -   Added an `autoload` cookie.  Thanks to Peter S. Galbraith for
        the patch.
    -   Support a customizable horizontal rule string
        `markdown-hr-string`.  This replaces the previous but less
        flexible custom option `markdown-hr-length`, which has been
        removed.
    -   Followed wiki links are opened using `markdown-mode`.

*   Bug fixes:

    -   Fixed an issue, reported by Joost Kremners, where for
        multi-line lists, the position of the list marker was not
        being added to the list of possible indentation levels.
    -   Avoid a problem where indentation positions were getting
        skipped over when tab cycling.
    -   Fixed an issue when column 0 is the natural automatic
        indentation stop.
    -   Prevent infinite loops in blockquote (`C-c C-s b`)
        and preformatted (`C-c C-s p`) block insertion while
        at the beginning or end of the buffer.

## Markdown Mode 1.7

*October 1, 2009*

  * New features:

    -    Support filling of list items.
    -    Allow customization of font-lock faces (thanks to intrigeri
         for the patch).
    -    Automatic indentation when pressing enter or tab (with
         cycling upon subsequent tab presses) (thanks to Bryan Kyle).
    -    Generate real XHTML for previewing (thanks to Hilko Bengen
         for the patch).  This fixes [Debian bug #490865](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=490865).  The CSS
         stylesheet for XHTML output is customizable via
         `markdown-css-path` (thanks to Ankit Solanki for the patch).
    -    Better URL matching (protocols are customizable through
         `markdown-uri-types`).
    -    LaTeX syntax highlighting can be enabled or disabled using
         the customize system (`markdown-enable-math`).
    -    Support for HTML comments (font lock, `comment-dwim`, etc.).
    -    Support filling of definition lists (thanks to Peter Williams
         for the patch).

*   Improvements:

    -    Add support level 5 and 6 atx-style headers (thanks to Alec
         Resnick for the report).
    -    Fill-paragraph no longer breaks lines inside of square
         brackets (thanks to Peter Williams for the patch).

*   Bug fixes:

    -    Fixed several font lock issues.
    -    Fixed a bug where pressing enter did nothing in
         longlines-mode (thanks to Ankit Solanki for the patch).
    -    Fixed a problem where Emacs snapshot would hang during
         `markdown-enter` and `markdown-cycle` (thanks to Alec Resnick
         for the report).
    -    Fixed an issue with auto-fill-mode and markdown-mode's
         indentation (thanks to Joost Kremers for the report and
         analysis).

## Markdown Mode 1.6

*June 4, 2008*

*   **Breaking changes:**

    -    Rename `blockquote-region` to `markdown-blockquote-region` in
         accordance with the Emacs major mode coding conventions.
    -    Several new keybindings.
    -    Reverted to using colors instead of bold, italic, and
         fixed-width faces.

*   New features:

    -    Implemented org-mode style visibility cycling.
    -    Support outline minor mode.
    -    Undefined reference checking (via `C-c C-c c`, using code by
         Dmitry Dzhus).
    -    Wiki links: syntax highlighting and element insertion
         (`C-c C-a w`).
    -    Allow syntax highlighting faces to be customized.
    -    Insertion of preformatted text sections (`C-c C-s p`).
    -    Font locking for inline URIs and email addresses.
    -    Markdown mode menu (thanks to Greg Bognar for the initial
         code).

*   Improvements:

    -    Markdown customize group moved to `wp` (word processing).
    -    Derive from `text-mode` instead of `fundamental-mode`.
    -    Properly prefix regex definitions and `wrap-or-insert`
         function.

*   Bug fixes:

    -    Highlight wrapped inline link definitions (thanks to Blake
         Winton).
    -    Fix adjacent wiki link font lock bug.
    -    Support escaping of backticks.
    -    Many other small font lock tweaks.
    -    Don't check for `transient-mark-mode` in
         `markdown-blockquote-region`.  This fixes Debian bug #456592
         (thanks to Daniel Burrows for the report).
    -    Apply Greg Bognar's fix for `markdown` with an active region.
    -    Don't use the kill ring to store wrapped text.

## Markdown Mode 1.5

*October 11, 2007*

Version 1.5 is a major revision compared to previous releases.  The
functionality has not changed very much, all of the keybindings are
the same, but it is internally quite different.

Most of the changes involve syntax highlighting.  The regular
expressions have been significantly improved and the corresponding
font lock faces have been tweaked (hopefully for the better, but feel
free to customize them).  Various other small bugs have been fixed and
the documentation and website have been updated.

The two changes in functionality, both of which are optional, are
syntax highlighting for embedded mathematics via LaTeX-like
expressions, and highlighting for bracketed wiki links.

 [itex]: http://golem.ph.utexas.edu/~distler/blog/itex2MMLcommands.html

## Markdown Mode 1.4

*June 29, 2007*

Version 1.4 includes a small fix to the regular expression syntax to
fix the Emacs 21 "Invalid escape character syntax." error.  Thanks to
Edward O'Connor for the fix.

## Markdown Mode 1.3

*June 5, 2007*

Version 1.3 provides syntax highlighting, insertion commands for all
basic HTML elements, and preview commands for viewing the resulting
HTML in a new buffer of an external browser.

## Markdown Mode 1.2

*May 25, 2007*

Version 1.2 adds element insertion commands and keys for links,
horizontal rules, headers, inline code, and bold and italic text.

Added element insertion commands and keys for links, horizontal rules, headers, inline code, and bold and italic text.

## Markdown Mode 1.1

*May 24, 2007*

Version 1.1 is the initial release of Markdown Mode for Emacs, a major
mode to edit Markdown files in Emacs.  This version provides basic
syntax highlighting and element insertion commands for Markdown files.



  [^MELPA]: MELPA is Milkypostman's Emacs Lisp Package Archive at <http://melpa.org/>.

  [at]: http://www.aaronsw.com/2002/atx/
  [bk]: https://leanpub.com/markdown-mode
  [cg]: https://github.com/jrblevin/markdown-mode/graphs/contributors
  [df]: https://daringfireball.net
  [dp]: https://packages.debian.org/sid/lisp/elpa-markdown-mode
  [du]: http://packages.ubuntu.com/search?keywords=elpa-markdown-mode
  [ei]: https://github.com/Fanael/edit-indirect/
  [el]: http://jblevins.org/projects/markdown-mode/markdown-mode.el
  [em]: https://www.gnu.org/software/emacs/
  [fb]: http://svnweb.freebsd.org/ports/head/textproc/markdown-mode.el
  [gh]: https://github.com/jrblevin/markdown-mode
  [gp]: https://www.gnu.org/licenses/old-licenses/gpl-2.0.html
  [hb]: https://github.com/dunn/homebrew-emacs/blob/master/Formula/markdown-mode.rb
  [is]: https://github.com/jrblevin/markdown-mode/issues
  [jb]: http://jblevins.org/
  [md]: http://daringfireball.net/projects/markdown/
  [ml]: https://melpa.org/#/markdown-mode
  [mm]: http://jblevins.org/projects/markdown-mode/
  [mp]: https://trac.macports.org/browser/trunk/dports/editors/markdown-mode.el/Portfile
  [ms]: https://stable.melpa.org/#/markdown-mode
  [mt]: http://trac.macports.org/ticket/35716
  [nb]: http://pkgsrc.se/textproc/markdown-mode
  [st]: http://docutils.sourceforge.net/mirror/setext.html 
  [sx]: http://daringfireball.net/projects/markdown/syntax
  [up]: https://github.com/jwiegley/use-package
  [v1.1]: http://jblevins.org/projects/markdown-mode/rev-1-1
  [v1.2]: http://jblevins.org/projects/markdown-mode/rev-1-2
  [v1.3]: http://jblevins.org/projects/markdown-mode/rev-1-3
  [v1.4]: http://jblevins.org/projects/markdown-mode/rev-1-4
  [v1.5]: http://jblevins.org/projects/markdown-mode/rev-1-5
  [v1.6]: http://jblevins.org/projects/markdown-mode/rev-1-6
  [v1.7]: http://jblevins.org/projects/markdown-mode/rev-1-7
  [v1.8.1]: http://jblevins.org/projects/markdown-mode/rev-1-8-1
  [v1.8]: http://jblevins.org/projects/markdown-mode/rev-1-8
  [v1.9]: http://jblevins.org/projects/markdown-mode/rev-1-9
  [v2.0]: http://jblevins.org/projects/markdown-mode/rev-2-0
  [v2.1]: http://jblevins.org/projects/markdown-mode/rev-2-1
  [v2.2]: http://jblevins.org/projects/markdown-mode/rev-2-2
  [v2.3]: http://jblevins.org/projects/markdown-mode/rev-2-3