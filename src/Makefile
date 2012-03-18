# Makefile for Sphinx documentation

# You can set these variables from the command line.
SPHINXOPTS    =
SPHINXBUILD   = sphinx-build
PAPER         =
BUILDDIR      = ../_build
PUBLISHDIR    = ..

# Internal variables.
PAPEROPT_a4     = -D latex_paper_size=a4
PAPEROPT_letter = -D latex_paper_size=letter
ALLSPHINXOPTS   = -d $(BUILDDIR)/doctrees $(PAPEROPT_$(PAPER)) $(SPHINXOPTS) .
# the i18n builder cannot share the environment and doctrees with the others
I18NSPHINXOPTS  = $(PAPEROPT_$(PAPER)) $(SPHINXOPTS) .

.PHONY: default help clean html publish

default: html
help:
	@echo "Please use \`make <target>' where <target> is one of"
	@echo "  html       to make standalone HTML files"
	@echo "  publish    to publish all documents into tach.github.com"

publish: html
	rsync -rlvHS $(BUILDDIR)/html/ $(PUBLISHDIR)/
	find $(PUBLISHDIR) -mindepth 1 -type d \
	    ! -path $(BUILDDIR) ! -path $(BUILDDIR)/\* \
	    ! -path $(PUBLISHDIR)/src ! -path $(PUBLISHDIR)/src/\* \
	    ! -path $(PUBLISHDIR)/.git ! -path $(PUBLISHDIR)/.git/\* \
	    -exec git add {} \;
	find $(PUBLISHDIR) -mindepth 1 -type f \
	    ! -path $(BUILDDIR)/\* \
	    ! -path $(PUBLISHDIR)/src/\* \
	    ! -path $(PUBLISHDIR)/.git/\* \
	    -exec git add {} \;
	git commit -am "Published at `env LC_ALL=C date`"
	git push -u origin master

clean:
	-rm -rf $(BUILDDIR)/*

html:
	$(SPHINXBUILD) -b html $(ALLSPHINXOPTS) $(BUILDDIR)/html
	@echo
	@echo "Build finished. The HTML pages are in $(BUILDDIR)/html."
