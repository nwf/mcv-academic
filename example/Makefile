# Our languages
LANGS = en es

# Default source options
NAME = example
SOURCE = $(NAME: %=%.tex)
DETAILS = true

# Rules
all: $(LANGS)

# Default make to build these rules instead of files in case they exist
.PHONY: all clean veryclean $(LANGS)

# we need to clean before compiling a new language
$(LANGS): clean	
	# compile the language
	$(eval INPUT="\def\defaultlang{\selectshortlanguage{$@}\details$(DETAILS)}\input{$(SOURCE)}")
	pdflatex -interaction=nonstopmode $(INPUT) -jobname=$(NAME) || true
	pdflatex -interaction=nonstopmode $(INPUT) -jobname=$(NAME) 
	biber $(NAME)
	pdflatex -interaction=nonstopmode $(INPUT) -jobname=$(NAME) || true
	pdflatex -interaction=nonstopmode $(INPUT) -jobname=$(NAME) 
	# if it finishes move the file to the correct name
	mv $(NAME).pdf $(NAME)-$@.pdf


# ignoramos los errores con -
clean:
	rm *.log *.aux *.out *.blg *.bbl *.run.xml *.bcf || true

veryclean: clean
	rm *.pdf || true