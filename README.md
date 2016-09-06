# About

This style an extension for the [`moderncv`](moderncv) package for creating academic (or closely related) CVs. It aims to provide an easy way to maintain the entries in your CV by using [`biblatex`](biblatex) custom drivers to typeset the CV in multiple languages with minimal changes to the main document.

I designed this interface because I needed to maintain my CV in several languages, and it was difficult to keep track of the different changes in all the versions. This style aims to ease that problem. 

The other problem I'm trying to solve with this style is to make the process of changing parts of the CV more manageable.

## Goals

The two main goals of this style are

* Multi language support: easy way to export the CV in different language without updating the document in between languages
* Easy to maintain and update the entries of the CV: easy way to re arrange, omit, or add entries.


# Install

The style should be installed in your local `texmf` tree. 

For example, you can execute the following

```bash
mkdir -p ~/texmf/tex/latex/
cd ~/texmf/tex/latex/
git clone git@gitlab.com:adin/mcv-academic.git
```

Or you can extract the files directly into the same local `texmf` directory.

# Quick start

## How to build a CV

Go to the `example` folder inside the repository, and execute

```bash
make
```

That will execute the example CV and will create two files `example-en.pdf` and `example-es.pdf` (and other auxiliary files).

For a particular language, you can execute

```bash
make en
```

Note that the languages are defined within the `Makefile` and should be the same as the ones defined within the style.

## Customizing your CV

If you want to start quickly, just open the `example.tex` and change the example for your personal data:

```tex
\firstname{John}
\familyname{Doe}
\title{Curriculum Vit\ae} % optional

\address{Fake Street 42}{}{Fakeland, FKL}

\email{johndoe@fake.com}
\mobile{(+1) 234 5678 } % optional
%\phone{+2~(345)~678~901} % optional
%\fax{+3~(456)~789~012} % optional

\homepage{johndoe.com/\textasciitilde joe}% note that the tilde (~) needs to be escaped for moderncv 
\social[scholar]{ABC123}
```

You can customize the strings used in the headers by updating

```tex
\newmlcommand[english=Education, spanish=Educación]{\education}
\newmlcommand[english=Degrees, spanish={Grados Académicos}]{\degrees}
\newmlcommand[english=Technical, spanish={Estudios Técnicos}]{\technical}
% the rest was ommited for space
```

And for your entries, you can modify the `cvdata.bib`, which is a custom bibliography file (similar to `bibtex` entries) for entries that will be render in the CV.

For example

```tex
@Study{phd,
  date={2000/2010},
  title-en = {Ph.D.  in  Foo},
  title-es = {Doctorado  en  Foo},
  organization-en={Univeristy  of  Bar},
  organization-es={Universidad  de  Bar},
  country = {Fakeland},
  description-es = {Hice  descubrimientos  novedosos  sobre  Bar.},
  description-en = {Did  novel  discoveries  about  Bar.},
  keywords = {degrees}
}
```

is a `study` entry, and it will by printed by

```tex
\printstudies[heading=subbibliography, title={\degrees}, keyword=degrees]
```

For more details, read on.

# Academic Style

The main idea of this style is to hold academic CV, as they have several entries of related items. For example, you want to put all your publications, guided theses, talks, etc. Most of the time, you will want to sort them in a given order, or print some of them, or do some strange things.

Moreover, you may want to have all those entries in different languages and you may need to have the same version in different languages. If that is your case, fear no more. This probably help you easing that task.

## How the style works

The whole style is build around the idea of printing similar items in a similar fashion. For that reason, I chose to create `biblatex` drivers to handle the common entries that appeared in academic CVs.

The `cv.tex` (that is, the main file that you will compile) only holds the way you want to layout your document. While all the data is maintained in other files that must be `.bib`. It depends on your choice, but you can have the files in one file or spread around.

Then, another goal was to have multi language support. For that reason, the style gives commands to define languages variations of your strings and entries, and allow you to compile the entire document in a given language without making further changes to the document.

## Entries

In general, the entries follow the same logic of the `.bib` files. Thus you create them with the syntax

```tex
@entry {
  field = {content},
  field = {content},
  ...
  field = {content}
}
```

where `entry` is the name of the respective item (see options below), and `field` is any of the possible fields of each entry.

The style defines the following entries (or types of data)

- **Study**. Refers to a course or study you did. For example, your Ph.D., master, among others. It supports the following fields
  * `date`: a range of dates in which the study happened. For example: to print 
      + a range of dates with their months `1990-01/1995-01`,
      + a range of defined years `1990/1995`,
      + a range of years that hasn't ended (it continues to the present) `1990/`, or
      + a single year `1990`
  * `title`: the name of the study or degree obtained. This field is a string.
  * `organization`: the place where you did the studies. This field is a string.
  * `country`: where you did the studies. This field is a string.
  * `description`: extra information you want to give about your studies.

- **Position**. Refers to work related entries. It supports the following fields
  * `date`: a range of dates in which you work in that position. It follows the same pattern of `study`.
  * `title`: the name of the position you had. This field is a string or a `bibstring`.
  * `organization`: the place where you worked. This field is a string.
  * `country`: where you did the work. This field is a string.
  * `description`: extra information you want to give about your position. What you did or achieve in such position.
  
- **Project**. Refers to research or industry projects you won or performed. For example, it aims to hold grants, industry projects, or related items. It supports the following fields
  * `role`: your role in the project. This field is a string or a `bibstring`.
  * `title`: the name of the project. This field is a string.
  * `number`: the identifier (or list of identifiers separated by `and`) for the project.
  * `funding`: the name of the funding agency or organization that was in charge.
  * `period`: the duration of the project. It should be an integer or a float in years (as the word `year` or `years` will be appended automatically when printed).
  * `year`: the starting year of the project. This field is an integer number representing the year.
  * `month`: the starting month of the project. This field should be a month that `biblatex` can understand (that is, a number or a three letter month representation).

- **Thesis**. Refers to the theses you guided. It supports the following fields
  * `author`: the student (author) of the theses that you guided. This field is a string.
  * `title`: the title of the thesis. This field is a string.
  * `date`: the date of the thesis. This field is a date from `biblatex`.
  * `organization`: the institution where the thesis was done. This field is a string.
  * `level`: the academic level of the degree that was awarded with the thesis. It supports the following `bibstrings`
     + `undergrad`
     + `master`
     + `doctor`
  * `note`: a small note to place at the end in parenthesis. This field is a string.
  
- **Course**. Refers to courses you taught. It supports the following fields
  * `title`: the title of the course taught. This field is a string.
  * `date`: the date of the course. It follows the same pattern of `study`.
  * `organization`: the institution where the course was taught. This field is a string.
  * `description`: general description of the course. This field is a string.
  
- **CVMisc**. Refers to general things that can be placed in the CV. For example, memberships, or items that need a short description and dates. It supports the following fields
  * `title`: a principal description of the entry. This field is a string.
  * `date`: the date of the entry. It follows the same pattern of `study`.
  * `description`: general description of the entry. This field is a string.

 
## Fields

The fields in these entries follow the same logic as the entries in `biblatex` or `bibtex` files. There are, however, some custom behaviors implemented.

### Date fields

The dates in the CV allow to hold ranges. For brevity, the style truncates the day of all the dates, and displays only the month and year.

The ranges allow to print the following options
+ a range of dates with their months `1990-01/1995-01`,
+ a range of defined years `1990/1995`,
+ a range of years that hasn't ended (it continues to the present) `1990/`, or
+ a single year `1990`.

### String fields

Most of the string fields have the options to create localized versions of each field. The different drivers will chose the correct language entry according to the current language set in the document. If the language is not found it will default to the field name without a language.

For example, the field `title` can be defined as
- `title`: default information to be used in case the current language version of the field is not found.
- `title-en`: English version of the field.
- `title-es`: Spanish version of the field.

And so on. The fields use the short version of the languages. Those can be set when the languages are defined.

A good rule of thumb is to define the default name with the language you want the field to appear, and then extend the field to other languages.

Another use to the default fields is when you have a field that will use the same name regardless of the set language. For example, a proper name that will be the same in all the languages. Instead of defining the same string several times, you just define it once.

Instead of defining
```tex
organization = {Proper name},
organization-en = {Proper name},
organization-es = {Proper name},
```
create a simpler version that will be used as default
```tex
organization = {Proper name},
```

### Defined `bibstrings`

Some fields allow to use `bibstrings` instead of a given string.

A `bibstring` is a previously defined keyword that will be translated into a defined string. This allows to create common definitions (and their translations) to be used easily in the entries.

The `bibstrings` for general use are
  * `undergrad`: Undergraduate
  * `master`: Master
  * `doctor`: Ph.D.
  * `pi`: Principal Investigator
  * `ai`: Alternating Investigator
  * `professor`: Professor
  * `assisprofessor`: Assistant Professor
  * `assocprofessor`: Associate Professor
  * `researcher`: Researcher
  
and others that are used internally (but are also available for use) are  
  
 * `duration`: time frame
 * `years`: years
 * `year`: year
 * `present`: present
 * `presentabbr`: pres.
 * `theses`: Theses
 * `degree`: Degree
  
Note that all these have translations to the other languages as well (here are the English versions for illustrative purposes).

## Localization

### Within the style

Currently the style supports two languages: `english` and `spanish`. I'm planing on extending it to other languages when I have time (but contributions for the translations are welcomed).

The languages are defined in `cv.bbx` and `moderncvstyleacademic.sty` by declaring the supported languages through

```tex
\babelshortnames{en=english, es=spanish}
```

That is, a list of pairs `<abreviation>=<languagename>`. These languages are not necessarily the same as the ones declared in the main document.

The declaration needs to be repeated as the processing of the styles (`.bbx`) is done at a different time than the `moderncvstyle`, so the macro is not available later. Thus, the robust solution is to use `setlanguages.sty` configuration file in which this macro is declared.

### Within your CV

To localize the titles and other strings that will appear in your CV, the style provides the `\newmlcommand` which declares a command that will change according to the current language.

For example, to declare a new multi language command named `\degrees` you execute

```tex
\newmlcommand[english=Degrees, spanish={Grados académicos}]{\degrees}
```

where the square brackets (the options) allow for a list of languages and the corresponding string that will be expanded from the macro.

Then, it is as simple as using the defined macro in any place you want your string to appear, for example

```tex
\section{\degrees}
```

### Setting the language

The language changing mechanism is provided by the [`babel`](babel) package. Thus, you first need to include that in your document with the languages you want support for

```tex
\usepackage[english, spanish]{babel}
```

Then you select the language (**within** the document body, that is after the `\begin{document}`) using `\selectlanguage{<language>}`. Moreover, the style provides a macro to select the language through interpreting the short names that we defined before. You can use it as `\selectshortlanguage{<shortname>}`.

For aiding with automatic building (for example through `make`) you can define a macro, lets call it `\defaultopts`, that is declared outside of the document, like

```tex
% I use this for automatic creation of the cv
\ifdef{\defaultopts}%
  % default options passed outside of the cv
  {\defaultopts}{%
    % options by hand
    % \selectlanguage{spanish}
    \selectshortlanguage{en}
  }
```

## Printing the entries

The entries are printed using the `\printbibliography` macro provided by [`biblatex`](biblatex) package. The style provides some wrappers to this macro by establishing the default types for each entry with
* `\printstudies`
* `\printpositions`
* `\printprojects`
* `\printcourses`
* `\printtheses`
* `\printcvmiscs`

Note that all these macros support the same options as `\printbibliography`.

# Makefile

The style provides an example `Makefile` (inside the `example` folder) to show a way to generate automatically the CV in all the languages.

This write up doesn't aim to teach you `Makefile` syntax, but if you want a quick way to use this file for your projects you need to change the following parts

```makefile
# Our languages separated by a space
# We use the shortnames because it is easier to read, but you can use the long names
# but you will need to update `\defaultopts` in the `INPUT` variable
LANGS = en es
# Default source name
NAME = example
# Wheter to show details or not
DETAILS = true
```

# Acknowledgments

I will like to thank the community of [TeX Stack Exchange](tex-sx), which helped a lot solving problems and roadblocks that I faced in the way of making this.

[moderncv]: https://www.ctan.org/tex-archive/macros/latex/contrib/moderncv/?lang=en
[babel]: https://www.ctan.org/pkg/babel?lang=en
[biblatex]: https://www.ctan.org/pkg/biblatex?lang=en
[tex-sx]: http://tex.stackexchange.com/