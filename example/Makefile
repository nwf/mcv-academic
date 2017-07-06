# Programs
MAKE = make

# Our languages, styles, and patterns
LANGS = en es pt
STYLES = classic oldstyle fancy banking casual academic friggeri
PATTERNS := $(foreach style, $(STYLES), $(foreach lang, $(LANGS), $(style)-$(lang)))

# Default source options
NAME = example
SOURCE = $(NAME: %=%.tex)
DETAILS = true

# Default make to build these rules instead of files in case they exist
.PHONY: all clean veryclean $(PATTERNS) $(LANGS) $(STYLES)

# Rules
all: $(PATTERNS)

$(LANGS): 
	@$(MAKE) $(foreach style, $(STYLES), $(style)-$@)

$(STYLE): 
	@$(MAKE) $(foreach lang, $(LANGS), $@-$(lang))

# we need to clean before compiling a new language
$(PATTERNS): clean	
	# compile the language
	$(eval STYLE = $(firstword $(subst -, ,$@)))
	$(eval LANG = $(lastword $(subst -, ,$@)))	
	$(eval INPUT="\def\defaultstyle{$(STYLE)}\def\defaultdetails{$(DETAILS)}\def\defaultopts{\selectshortlanguage{$(LANG)}}\input{$(SOURCE)}")
	pdflatex -interaction=nonstopmode $(INPUT) -jobname=$(NAME)
	biber $(NAME)
	pdflatex -interaction=nonstopmode $(INPUT) -jobname=$(NAME)
	pdflatex -interaction=nonstopmode $(INPUT) -jobname=$(NAME)
	# if it finishes move the file to the correct name
	mv $(NAME).pdf $(NAME)-$@.pdf


# Clean
clean:
	rm *.log *.aux *.out *.blg *.bbl *.run.xml *.bcf *.synctex.gz || true

veryclean: clean
	rm *.pdf || true