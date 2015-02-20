# Source files
TARGET    := sdmx-json-data-message
STATUS    := ../0-status.md
TITLE     := title.txt
BODY      := ../1-sdmx-json-field-guide.md ../2-references.md ../3-full-example-with-comments.md

# Pandoc options
COMMON_OPTS := --number-sections --standalone --toc --toc-depth=4 --smart
PDF_STY     := options.sty
PDF_OPTS    := -B status.tex -H $(PDF_STY) -V documentclass:scrreprt
HTML_STYLE  := doc-style.html
HTML_OPTS   := -B status.html -H $(HTML_STYLE) -t html5

status.tex: $(STATUS)
		pandoc -o status.tex $(STATUS)

$(TARGET).pdf: status.tex $(PDF_STY) $(TITLE) $(BODY)
		pandoc $(PDF_OPTS) $(COMMON_OPTS) -o $@ $(TITLE) $(BODY)

status.html: $(STATUS)
		pandoc -t html5 -o status.html $(STATUS)

$(TARGET).html: status.html $(HTML_STYLE) $(TITLE) $(BODY)
		pandoc $(HTML_OPTS) $(COMMON_OPTS) -o $@ $(TITLE) $(BODY)

pdf: $(TARGET).pdf

html: $(TARGET).html

clean:
		rm -f $(TARGET).pdf $(TARGET).html status.tex status.html

.PHONY: clean pdf html

