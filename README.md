# SIMPLEX Documents

This repository stores Neurodata's SIMPLEX reporting documents. 

- The most recent submitted report is [here](./Reporting/reports/2017-02/simplex.pdf)
- The working version of the next report is [here](./Reporting/reports/2017-03Q1/simplex.pdf)

## Reporting Instructions

It is expected that everyone compile a montly 1-page progress report
with a nice figure (caption included). These will either go in the
"tools" or "data" sections of the report.
Every "tool" should fit on **one page**, complete with a paragraph
explaination of what was accomplished and a figure with included caption.

You'll need everything in the parent folder plus all of the figures in
the /Reporting/figs/ folder to compile the main document and
subfiles.  The easiest way to get everything and make sure it's in 
the right place is to clone the entire git repository.  

The file structure is as follows:
- The main file is named `simplex.tex` 
- subfiles/sections will be
named \<section_name\>.tex and included in the main file using the
options below
  - subfiles/sections may be included with `\input` or `\subfile`
  - [input](https://en.wikibooks.org/wiki/LaTeX/Modular_Documents#Getting_LaTeX_to_process_multiple_files) is simple and is like pasting your file into the document.
  - [subfile](https://en.wikibooks.org/wiki/LaTeX/Modular_Documents#Subfiles) is more complicated, but allows you to compile your file
    individually.  An example file that uses subfiles is called
`sub-file.tex`.

#### Compiling the document/subfiles
- To compile the entire document run `make` from the same directory as the
  main file.
- To compile your own section using the subfile format, run `xelatex
  file.tex`
- All figures are to live in `/Reporting/figs/`

#### Bibliographic  Content
Most won't need to worry about this, but it's here for the few who
might. If you don't need to specifically cite your bib content in your
section than all you have to do is add the bib entry to the
corresponding file.

- Bibliographic content is divided into 4 files, each citation requires
  a different cite command, given below.
  - `manuscripts.bib`: Command example `\citeman{Author16}`, used for  internally authored papers in preparation or accepted
    for publication since last report. 
  - `talks.bib`: Command example `\citetalks{Speaker16}`, used for talks given by lab members since last report.
  - `conferences.bib`: Command example `\citeconf{Conf16}`, used for conferences attended by lab members since last report, this can also include poster sessions.
  - `references.bib`: Command example `\citereferences{Ref16}`, used for regular citation of someone else's work.





