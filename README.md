unilaiva-songbook
=================

unilaiva-songbook is a collection of song lyrics etc for the contributors' private use.

The PDF compiled from these sources is available at
[https://unilaiva.aavalla.net/](https://unilaiva.aavalla.net/)


Environment
-----------

The project is written in [LaTeX](https://www.latex-project.org/).

#### Requirements ####

  * LaTeX 2e distribution (TeX Live is recommended) with some standard packages and the binaries
    `pdflatex` and `texlua`
  * Lilypond installation with the binary `lilypond-book`
  * Font 'Noto' for LaTeX

Our project requires only some pretty standard LaTeX packages, which are included in many LaTeX
installations by default, to be installed on the system. They are all included with `\usepackage`
commands in the beginning of the project's main file: `unilaiva-songbook.tex`.

One of the package dependencies, [songs](http://songs.sourceforge.net/), is included in the project
tree and used instead of a one possibly installed on the system. This is because of compatibility
reasons to ensure a certain version: the package is used heavily and some of its macros are
redefined.

##### Example: Ubuntu 18.04 #####

For example, on Ubuntu 18.04 LTS, to install all required dependencies and to download the
project's source, you need to run the following two commands:

  1. `sudo apt install git lilypond texlive texlive-fonts-extra texlive-lang-english
     texlive-lang-european texlive-latex-extra`
  2. `git clone git://github.com/unilaiva/unilaiva-songbook.git` (read-only, doesn't require a
     Github account)


Compiling the songbook to PDF
-----------------------------

If you're on an UNIX system, you can simply use the provided `compile_unilaiva-songbook.sh` shell
script to build a PDF document out of our project.

Otherwise, you should run the following commands or their equivalents in the following sequence:

  1. `lilypond-book -f latex --output temp unilaiva-songbook.tex`
  2. `cd temp ; ln -s ../ext_packages ./ ; ln -s ../../content/img ./content/ ;
     ln -s ../tags.can ./`
  3. `pdflatex unilaiva-songbook.tex`
  4. `texlua ext_packages/songs/songidx.lua idx_unilaiva-songbook_title.sxd
     idx_unilaiva-songbook_title.sbx`
  5. `texlua ext_packages/songs/songidx.lua idx_unilaiva-songbook_auth.sxd
     idx_unilaiva-songbook_auth.sbx`
  6. `texlua ext_packages/songs/songidx.lua -b tags.can idx_unilaiva-songbook_tag.sxd
     idx_unilaiva-songbook_tag.sbx`
  7. `pdflatex unilaiva-songbook.tex` (again)


Printing
--------

The book is designed to be in **paper size A5**, preferably double-sided. It looks good as black
and white, but color printing gets better results (while using the non-black colors sparingly).

You can simply print the main document, `unilaiva-songbook.pdf`. A5 paper should be used (and
selected in the printing software). Otherwise you will get big scaled up pages or pages with wide
margins.

If printing double sided, ensure that the pages face each other in such a way that odd pages are
on the right side (recto) and even pages are on the left side (verso) of a spread. That way
minimizes flipping pages within a song. Also, the margins, page number positions etc. are
optimized for that order.

Note that margins ought to be set to zero in the printing software and the printer drivers setup,
if such options are available. On Linux and MacOS, the `lp` program is recommended for printing
without extra margins. Simply state e.g.
`lp -o PageSize=A4 printout_unilaiva-songbook_A5_on_A4_doublesided_folded.pdf`
without the `fit-to-page` option that some GUI programs like to pass to it.

There are also special printing options, like printing multiple A5 sized pages on an A4 sized
paper. They are defined in files named `printout_unilaiva-songbook*.context` and are to be
inputted to ConTeXt program, which needs to be installed on the system. They operate on a
previously compiled `unilaiva-songbook.pdf` file. See comments in the beginning of each such
file. The compilation script will process these too, if it finds the `context` binary.

#### Printing double sided on a single sided printer ####

To print double sided on a printer without a duplexer, one needs to print odd pages first, then
flip each page around, feed them to the printer, and then print the even pages. With the main
document, `unilaiva-songbook.pdf`, the pages need to be flipped on the long edge. The other
files, named `printout_unilaiva-songbook*.pdf`, with multiple pages on an A4 sheet, should be
flipped on the short edge.

To flip pages *on the short edge* manually: put the printed stack of papers in front of you
upside down (printed side unseen), make a new stack by moving each sheet from the top of the old
stack to the top of the new stack (do not turn in any way, just "translate"), one by one. Feed
the new stack to the printer. Be careful to put it in there in the correct way.

If your printing software is limited, you can extract odd and even pages with, for example,
`pdftk` like this:

  * `pdftk unilaiva-songbook.pdf cat 1-endodd output unilaiva-songbook_odd.pdf`
  * `pdftk unilaiva-songbook.pdf cat 1-endeven output unilaiva-songbook_even.pdf`

#### larva's example procedure for printing ####

This procedure prints the whole book on A4 sized papers with a printer capable of single-sided
printing only. Flipping pages, cutting and binding are done by hand. The end result is a book
consisting of two-sided A5 pages, which is the preferred format.

  1. `./compile_unilaiva-songbook.sh`
  2. `pdftk printout_unilaiva-songbook_A5_on_A4_doublesided_folded.pdf cat 1-endodd output
     unilaiva-songbook_odd.pdf`
  3. `pdftk printout_unilaiva-songbook_A5_on_A4_doublesided_folded.pdf cat 1-endeven output
     unilaiva-songbook_even.pdf`
  4. `lp -o PageSize=A4 unilaiva-songbook_odd.pdf`
  5. flip pages manually on the short edge and feed them to the printer
  6. `lp -o PageSize=A4 unilaiva-songbook_even.pdf`
  7. cut the A4 pages in half to get the A5 pages and put them in correct order
  8. punch holes and bind


Project structure and guidelines
--------------------------------

Project's main file is `unilaiva-songbook.tex`. It includes all the other files in the project and
contains configuration.

Long `\renewcommand`s ought to be put into `unilaiva-songbook-extra.tex` to maintain readability
of the main file.

Song data and other *content* will be in various files inside `content` subdirectory and will be
inputed into the main file. Images are put into `content/img`.

External packages (only `songs` for now) are in `ext_packages` subdirectory.

See `songs` package documentation. It is included as a PDF file in `ext_packages/songs/songs.pdf`.
It can also be viewed online at
[http://songs.sourceforge.net/songsdoc/songs.html](http://songs.sourceforge.net/songsdoc/songs.html).

Stuff inside `songs` environment (the files in `content` directory named with a prefix `songs_`)
ought to contain only individual songs (and data related to them) between `\beginsong` and
`\endsong` macros plus other data wrapped in an `intersong` environment.

Use `\sclearpage` to jump to the beginning of a new page and `\scleardpage` to hop to the beginning
of a new left-side page. Suggest a good page brake spot with `\brk`. These are mostly not needed,
as the `songs` package does quite a good job in deciding these.

Lines in the source ought to be less than 100 characters long (is anyone using 80 column terminals
still?). Exceptions are allowed.

### Repeats and choruses ###

By putting a verse between `\beginchorus` and `\endchorus` instead of `\beginverse` and
`\endverse`, a vertical line will be shown on the left side of the verse in question. In this
songbook that visual que is used to mark an immediate repetition of the verse.

When some other phrase (than a verse) is repeated, the repeated part is to be put between `\lrep`
and `\rrep` macros. If the repeat count is anything else than two, it will be indicated by putting
`\rep{3}` after the `\rrep` macro (replacing `3` with the actual repeat count). If the span of the
repeat is clear (for example exactly one line), `\rep{}` macro can be used by itself.

Actual choruses i.e. verses that are jumped to more than once throughout the song can be marked
with `\beginchorus` or `\beginverse` (depending on their repeat behaviour), but each lyrics line
within them ought to be prefixed with `\chorusindent` macro, which indents the line a bit.
Elsewhere in the song, you can mark the spots from where to jump to chorus with
`\gotochorus{Beginning words of the chorus}`.

### Measure bars and beats ###

If using measure bars and a measure ends at the end of a lyric line, put a bar line `|` to *both*
the end of the lyric line and the beginning of the next one (if one exists). If a measure in the
end of a lyric line continues into the next line, but there are no lyrics after the last bar line
on the first lyric line, add ` -` after the last bar line on the first line to signify that there
indeed is a (partial) measure, like this: `| -`

If it is necessary to mark beats, use `.` as a chord name, like this: `\[.]`. Also, underlining
for lyrics can be used as a last resort.

### Melodies ###

To show some individual note names on the chord line, use the `\note` command or its positioned
siblings (see comments in `unilaiva-songbook-extra.tex` for help on which one to choose) within
a chord block. An example: `\[Am\note{E}]Love` will display an *Am* chord with an encircled
melody note name *e* above the word *Love* of the lyrics. Note that the note name must be
specified in upper case for transposing to work, even though the result is actually displayed
as lower case.

Full melodies are written using `lilypond` syntax. See documentation in
[http://lilypond.org/](http://lilypond.org/). `lilypond` parts must be put outside of verses
(but inside of a song).

### Tags ###

Tags can be attached  to songs using a peculiar syntax (scripture reference system of songs package
is used for this purpose). All tags must be listed in file `tags.can`. Define tags for a song by
adding `tags=` keyval to `\beginsong` macro. Note that a ` 1` must be appended to the tag name,
like this: `\beginsong{Song name}[tags={love 1, smile 1}]`.

The tag index is found at the end of the result document.


Tentative TODO
--------------

*  Add more songs (especially Finnish ones)
*  Add chords for existing songs with tag `(chords missing)`
*  Add translations and explanations for existing songs
*  Improve existing translations and explanations
*  Improve the introduction for mantras and the Finnish mythology section
*  Review tags and add more of them
*  Review phases
*  Organize songs better and decide the categories (= chapters / parts)
*  Possibly add poems, prayers etc in between songs or in their own category(?)
*  Further develop visual style of the end document
