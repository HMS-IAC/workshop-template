[![CC BY 4.0][cc-by-shield]][cc-by]  
This work is licensed under a [Creative Commons Attribution 4.0 International License][cc-by].  

[cc-by]: http://creativecommons.org/licenses/by/4.0/  
[cc-by-shield]: https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg  

# Workshop Materials Template

This repository provides a template to host workshop materials using Sphinx to generate HTML documentation from reStructuredText (reST) files. You can use it to organize your workshop presentations, exercises, and materials in a structured and versioned manner.

## Folder Structure

The repository uses the following structure for organizing multiple workshop versions, each with its own content and slides:
    
```plaintext
workshop-name/
├── docs/
│   ├── build/                          # Contains the generated HTML output
│   ├── source/
│   │   ├── _static/                    # Global static files
│   │       ├── iac-hms-logo-light.png
│   │       ├── iac-hms-logo-dark.png
│   │       └── ...
│   │   ├── _templates/                 # Global templates
│   │   ├── versions/                   # Folder containing versions
│   │       ├── 2024_01_30/             # Version-specific folder
│   │           ├── _static/            # Version-specific static files e.g. PDF slides
│   │               ├── 01_slide.pdf
│   │               └── ...
│   │           ├── 01_slide.rst        # Version-specific content (reST files or Markdown)
│   │           ├── Makefile            # Version-specific Makefile
│   │           └── ...
│   │       ├── 2024_02_15/             # Another version-specific folder
│   │           ├── _static/
│   │               ├── 01_slide.pdf
│   │               └── ...
│   │           ├── 01_slide.rst
│   │           ├── Makefile
│   │           └── ...
│   │   ├── conf.py                     # Single global configuration for all versions
│   ├── build_versions.sh               # Bash script to build all versions
└── ...
```

Each folder in `versions/` corresponds to a specific workshop version, with its own set of slides and content.

## Instructions

### 1. Install dependencies

To install the necessary Python dependencies, run:

```bash
conda create -n fiji-workshop python=3.11
conda activate fiji-workshop
pip install -U sphinx pydata_sphinx_theme sphinx_copybutton
```

### 2. Create a new repository and clone this one

First, create a new repository on the IAC-HMS github to host your workshop materials. Then, download the content of this repository as ZIP, or fetch the template using git:

```bash
git remote add source https://github.com/HMS-IAC/workshop-template.git
git fetch source
git reset --hard source/main  # start exactly from the main branch
git remote remove source  # remove source remote
git push origin main  # push the template content to your new repository; only force the push for the first time
```

### 3. Customize the template

1. Update each `conf.py` file in the `source/versions` folder with the name of your workshop and your name.

```python
# -- Customizable Project Settings -------------------------------------------
project = 'workshop-template'
copyright = '2024, Antoine A. Ruzette'
author = 'Antoine A. Ruzette'
release = '0.0.1'
html_title = 'Template for IAC workshops'

# URLs
iac_url = "https://iac.hms.harvard.edu/"
github_url = "https://github.com/HMS-IAC"
repository_name = os.getenv('GITHUB_REPOSITORY', 'hms-iac/workshop-template').split('/')[-1]
base_url = f"/{repository_name}/" if repository_name else "/"
```

### 4. Add your content

1. Add your slides as PDF files in the `_static` folder of each version.
2. Add your content as reStructuredText (reST) files in the `source/versions` folder of each version.

### 5. Build the documentation

This bash script will build the documentation for each version in the `source/versions` folder, as well as creating a entry `index.html` file to redirect to the latest version of the workshop i.e. the most recent one. In the background, it runs the `make html` command independently for each version, and saves the produced html files in `docs/build/`.

It's good practice to start from a clean `build` folder. Note that the `docs` folder also contains a general purpose Makefile to clean up `build`. To do so, run:

```bash
cd docs
make clean
cd ..
```

Then, run the `build_versions.sh` bash script to build the documentation:

```bash
sh docs/build_versions.sh
```

### 6. Publish the documentation

Save in a temporary folder the content of the `docs/build` folder. Then, manually add – or override the existing content – the content of the `docs/build` folder to the root of the `gh-pages` branch of your repository. Make sure your `gh-pages` branch contains a `.nojekyll` file to prevent GitHub from ignoring the `_static` folder.

```bash
git checkout -b gh-pages
git add .
git commit -m "Add workshop materials"
git push origin gh-pages
```

### 7. Enable GitHub Pages

Go to the settings of your repository, and under the GitHub Pages section, select the `gh-pages` branch as the source for your GitHub Pages.