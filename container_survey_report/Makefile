# This Makefile describes a set of common tasks for managing the
# build environment for the SKA Container Survey
#
# type: `make` to list the available commands.
#

IN_FILE ?= ./How do you want to use Containers?.csv
OUT_FILE ?= ./container-survey-data.csv

#
# Defines a default make target so that help is printed if make is called
# without a target
#
.DEFAULT_GOAL := help

.PHONY: all gen genpdf genhtml anonymise requirements run nb

help:  ## show this help.
	@grep -hE '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

requirements:  ## install the requirements for project
	if ! command -v pandoc; then \
	 echo "please install pandoc first (eg: apt install pandoc)"; \
	 exit 1; \
	fi
	if ! command -v xelatex; then \
	 echo "please install xelatex first (eg: apt install texlive-xetex)"; \
	 exit 1; \
	fi
	pip3 install pipenv
	pipenv install matplotlib pandas numpy plotly \
	               "notebook>=5.3" "ipywidgets>=7.2" jupyterlab pandoc \
								 jupyter_contrib_nbextensions
	pipenv run jupyter nbextension enable --py widgetsnbextension
	pipenv run jupyter labextension install @jupyterlab/plotly-extension
	pipenv run jupyter contrib nbextension install --user
	pipenv run jupyter nbextension enable spellchecker/main
	pipenv run jupyter nbextension enable python-markdown/main
	pipenv run jupyter nbextension enable toc2/main
	pipenv run jupyter nbextension enable collapsible_headings/main

anonymise: clean  ## anonymise survey data
	pipenv run python -c 'import pandas; df = pandas.read_csv("$(IN_FILE)", index_col=None); df["Username"] = df["Username"].astype("category").cat.codes; df.to_csv("$(OUT_FILE)")'

nb:  ## Run the notebook
	pipenv run jupyter-notebook container-survey.ipynb

run: nb  ## Alias for nb

genpdf:  ## generate notebook to PDF
	pipenv run jupyter nbconvert --to pdf \
	       --TemplateExporter.exclude_input=True \
				 --TagRemovePreprocessor.remove_cell_tags='{"remove_cell"}' \
				 container-survey.ipynb
	mv container-survey.pdf docs/container-survey.pdf

genhtml:  ## generate notebook to HTML
	pipenv run jupyter nbconvert --to html \
	       --TemplateExporter.exclude_input=True \
				 --TagRemovePreprocessor.remove_cell_tags='{"remove_cell"}' \
				 container-survey.ipynb
	mv container-survey.html docs/container-survey.html
	cp docs/container-survey.html docs/index.html

gen: genpdf genhtml  ## Generate HTML and PDF output

all: clean requirements gen

clean: ## clean out files
	rm -f '$(OUT_FILE)' docs/container-survey.pdf docs/container-survey.html docs/index.html
	@echo "I'm clean!"
