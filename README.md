rbbeamer - ruby-beamer
======================

rbbeamer is a preprocessor script to convert a structured file with informations about your slides into a .tex file. If pdflatex is installed, rbbeamer can go on and start pdflatex to build your PDF.

Most of work is done by the LaTeX-Beamer-package. But writing TEX-Files for beamer can be a little bit annoying if you usually use only a small (and everytime the same) part of the things beamer is capable to do. So if you need individual slides using all beamer-capbilities, you should use beamer directly by writing a .tex-file.

rbbeamer gives you access to (right now) just a small part of beamer-capabilities. But you can input your data very fast with nearly no hazzle about formatting and counting clicks and all this stuff.

Example
-------

Let's say we need a couple of slides about rbbeamer. Open an ASCII-Editor of your choice and enter:

    #sd|My Tests with rbbeamer  | John Doe | April 2013
    #toc|Agenda
    #sec|Introduction
    #fi| My first slide
    #i| rbbeamer is for setting up slides very fast
    #i|a tool for people who need lots of beautiful slides
    #i| Have a closer look
    #Fi

    #sec|Details
    #fd| Advantage and disadvantages
    #d| advantage| you only type what is needed
    #d|disadvantage | you have to read a \emph{small} README.
    #Fd

    #sub|This is my subsection
    #fx|Starting an empty frame
    #bd|starting a description
    #d|description|bd starts a description-environment
    #d|second|this is just an example
    #Bd
    #bi|starting an itemize
    #i|bi starts an itemize-environment
    #i|this is just an example too
    #Bi
    #Fx

Save this file as demo.rbt (rbt is default extension for rbbeamer-files). Now start rbbeamer:

if pdflatex is installed:

    rbbeamer demo.rbt -p 

or, if you just want to create a TEX-File for later use

    rbbeamer demo.rbt

The resulting .TEX file will create a presentation with a title page, a table of content titled "Agenda" and two slides with data. Both slides will step from one item to the next by mouseclick

### Install

Install with `gem install rbbeamer`. 

rbbeamer compiles .rbt-files into .tex-files. If you want to create your PDF, you should also have a latex-installation (otherwise rbbeamer will just create a TEX-file that you can use to work with at your own). If latex is not in your $PATH, you can specify a path in ~/.rbbeamer.conf. Default binary-name ist pdflatex.

### System prerequisites

rbbeamer ist a script written in [Ruby][ruby]. You need a running Ruby-Installation (tested: 1.9.3, 2.0.0p0), most modern operating systems include one. rbbeamer ist developed and tested under Linux - it should run under MAC OS/X and Windows too - but I didn' t test it until now. Drop me a mail if you have additional info.

Creating pictures you will need *graphviz* in your path.
### Commands

Commands start with a "#" as first char in a line. Every line not starting with # is ignored (so a blank as the first char means: this is a comment). You can use this to specify additional informations not shown on slide or just to deactivate a command for testing purposes. The end of a command is defined by line break or seperator-char (default: |)

    #sd|<Title Of Presentation> | <Author> | <Date> [ | sources]
start definition-command: Defines a titlepage with three entities. If you want to use citations with biblatex, just give a fourth option with name of .bib-file (specify without extension), i.e. `#sd| My Title Of Presentation | John Doe | April 2013 | sources`. 
rbbeamer will then try to include a file named `sources.bib`.

    #toc [| Agenda | <DEPTH>]
Table-Of-Content: includes a TOC-page based on defined sections and subsections. Optional: you can specify a title (default: Agenda) and a depth for building TOC (default: 1).

    #sec|<yourtitle>
    #sub|<yourtitle>
    #ssub|<yourtitle>
Defines a section, subsection or subsubsection (are your sure using subsubsections in a presentation?) named \<yourtitle>.

    #fi|<yourtitle>
Defines a frame (one slide) with different items using `itemize`. \<yourtitle> will be the title of frame.

    #i| <content>
Defines one item within a frame started with `#fi`. This item will be visible after a click in presentation (if you didn't use `handout`-option, see later)

    #Fi
Closes a frame started with `#fi`. 

    #fd|<yourtitle>
Defines a frame (one slide) with different items using `description`. \<yourtitle> will be the title of frame.

    #d| <keyword> | <content>
Defines one item within a frame started with `#fd`. This item and its description will be visible after a click in presentation (if you didn't use `handout`-option, see later). 

    #Fd
Closes a frame started with `#fd`. 

    #fx|<yourtitle>
Defines a frame (one slide) with no content. \<yourtitle> will be the title of frame. You can use this to define a frame at your own (see `#[`-command).

    #Fx
Closes a frame started with `#fx`. 

    #bi|<yourtitle>
Starts a new block inside a fx-frame named \<yourtitle>. Alle entries in block should follow with #i-command (itemize).

    #Bi
Closes a block started with `#bi`. 

    #bd|<yourtitle>
Starts a new block inside a fx-frame named \<yourtitle>. Alle entries in block should follow with #d-command (description).

    #Bd
Closes a block started with `#bd`. 

    #[|content
This command just copies all content of this line into resulting .TEX-file. Lets say, you want to specify a section with an additional short title. This you cannot do with `#sec`-command. Instead you can use `#[|\section[Short Title]{Really Long Title}`. Resulting .TEX-file will include this line: `\section[Short Title]{Really Long Title}`

    #{|
If you need more flexibility in defining your slides, you can use this command. All lines following this command are copied 1:1 to resulting .TEX-file until you close this command. Let's assume you need a frame with special content. Design your frame with beamer. If you are done with this, copy your beamer-sourcecode into your rbt-file between a #{| and a #} line. So from this time on your special designed tex-code will be copied into tex-file.

    #}
Closes the copy-mode.

    #dig{|<filename> [|scale]
Using [Graphviz][graphviz]-package you can include pictures into your presentation. With this command you define the filename being used for building and saving your picture. Have a look into example.rbt to find a graphviz-picture. Don' t forget to define an empty frame (using `#fx`) before defining a graphviz-picture. If you omit *scale-option*, 1.0 will be inserted as scale: Graph width will be 100% of textwidth.

    #input|<filename>
If you have large presentations, sometimes it is easier to use different source-files for different parts. Using `input`-command you can include another .rbt-file into your presentation (and yes, this one also can input a third one and so on).

    #attn
Using #attn one can mark some slides as very important. This mark is an red triangle at lower left corner (on handouts) or a small blue dot at same corner (on slides, it is only a signal for your talk). 

### Commandline Options
Starting rbbeamer at least you will have to specify your .rbt-file to compile. But beside this you can specify a lot of options. Try option `--help` or `-x` to see a short description.

      --seperator, -s <s>:   use this pattern as seperator (default: |)
           --logo, -l <s>:   Logo-File (without extension) for slides (default:
                             logo)
                --pdf, -p:   Start pdflatex after creating tex-file
            --handout, -h:   build a handout (no animations in pdf, add _handout
                             to filename)
          --theme, -t <s>:   Beamer-Theme to use (see Beamer-Doc for more info)
                             (default: Luebeck)
   --handoutcolor, -n <s>:   Colortheme for printing handouts (try DOVE to save ink) (default: default)
          --babel, -b <s>:   Language-option for babel-Package (default: english)
        --backend, -a <s>:   Backend for biblatex setting up references (default:
                             bibtex)
     --configfile, -c <s>:   Use this file as configfile (default:
                             ~/.rbbeamer.conf)
    --deleteworkfiles, -d:   Delete work-files from pdflatex:
                             .aux,.blg,.nav,.snm,.toc,.bbl,.log,.out,.run.xm,.tex,.eps,.run.xml,-blx.bib
    --writeconfigfile, -w:   write default config into ~/.rbbeamer.conf
                             (overwrite)
              --debug, -e:   Start in debugging mode
           --debugpdf, -u:   Show pdflatex-output during compilation
               --help, -x:   Show all options (this page)


### Global Config-directory
During startup rbbeamer tries to find config-dir (Linux: `~/.rbbeamer`). If it doesn't exist, it will be created. You can save some files used by a majority of your presentations in this dir (i.e. logo-files, bibtex-sources). rbbeamer tries to find this files in this dir, if you do not specify another directory. In my case, I store my bibtex-sourcefile with all entries I usually cite from in this dir. So I don't have to bother with copies of this file in different directories.

### Local Config-directory
In some cases you will prefer configs on a per-project basis. You can now use an additional config-file named `.rbbeamer.conf` in your working dir (where your `rbt-files` are stored. In this file you change perhaps your logo-file to a different one only used in this project. At startup rbbeamer will read global config file first and then tries to read a local config file in your working dir, which will overrule global settings.

### rbbeamer.conf 
Nearly all options regarding rbbeamer can be specified in command-line. But most times you don' t change your options. So ist is easier to use a config-file to save them. During startup rbbeamer sets all options to default values. If it finds rbbeamer.conf, options are overriden by those specified in config-file. If you also specified options in command-line, these values will override the former ones.

To create your global config-file, just specify all options needed for your work and add `--writeconfigfile (or -w)`. rbbeamer will then create a config (Linux: `~/.rbbeamer/rbbeamer.conf`) to save all options within. Next time just start rbbeamer and specify your .rbt-file. Thats it. You can edit your config-file with any editor.

    handout = false
    pdf = true
    deleteworkfiles = true
    debug = false
    debugpdf = false
    seperator = |
    babel = english
    addpackage = 
    logo = rbb_logo
    addpackage =
    theme = Luebeck
    handoutcolor = dove
    latexpath = 
    latexbinary = pdflatex
    backend = bibtex


If you need more packages to be declared in your .tex-file, just use addpackage option:

    rbbeamer.rb test.rbt --addpackage [myoption=true]{testpackage},{otherpackage}

or use addpackage-line in .rbbeamer.conf:

    addpackage = [myoption=true]{testpackage},{otherpackage}

This will lead to an additional entry in resulting .tex-file like this:

    \usepackage[myoption=true]{testpackage}
    \usepackage{otherpackage}


License
-------

rbbeamer is a preprocessor for creating TEX-files for LaTeX-Beamer.
Copyright (C) 2013 Thomas Romeyke (rubybeamer at googlemail.com).

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
    
[ruby]: http://www.ruby-lang.org
[graphviz]: http://www.graphviz.org

