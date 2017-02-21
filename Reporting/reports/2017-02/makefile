LATEX = xelatex
SPELL = aspell
TEX = simplex
BIBTEX = bibtex

default: $(TEX).tex 
	$(LATEX) $(TEX); $(BIBTEX) $(TEX).aux; $(BIBTEX) man.aux; $(BIBTEX) talks.aux; $(BIBTEX) conf.aux; $(BIBTEX) references.aux;$(LATEX) $(TEX); $(LATEX) $(TEX);
	$(RM) -f  *.blg *.dvi *.log *.soc *.toc *.lof *.lot *.cb *.bbl *.brf *.out *.aux *.fdb_latexmk *.fls *.out *.synctex.gz $(TEX).ps 
	open $(TEX).pdf &

view: $(TEX).tex 
	$(LATEX) $(TEX); $(BIBTEX) $(TEX); $(LATEX) $(TEX); $(LATEX) $(TEX)
	open $(TEX).pdf &

clean:
	$(RM) -f *.blg *.dvi *.log *.soc *.toc *.lof *.lot *.cb *.bbl *.brf *.out *.aux *.fdb_latexmk *.fls *.out *.synctex.gz

check:
	@echo "Passing the check will cause make to report Error 1."
	$(LATEX) $(TEX)  | grep -i undefined
