---
layout: post
comments: true
title:  "Create your documentation website with sphinx and GitHub"
ref: sphinx_doc
date:   2020-12-11 08:00:00 +0200
categories: blog dev
category: blog
lang: en
---


Previously I announced a preview of the Audio Loader library.
It prepares audio batches for Neural Network libraries (have a look [here](https://github.com/vroger11/audio_loader)).
For this library I planned to add a documentation website.
A preliminary version is now available [here](https://website.vincent-roger.fr/audio_loader/).
As Audio Loader is a Python library, I chose [Sphinx](https://www.sphinx-doc.org/) to generate the documentation as it is a classic with Python libraries.
It uses the reStructuredText file format to create the documentation.
Nethertheless, I prefer the markdown syntax as it is simpler and I use it more often.
Hence, I use as much markdown files as I can to create Sphinx documentation.
Next, to host this documentation I chose GitHub Pages as I already used it on my website.

In this post, I will explain how to create a documentation using Sphinx and how to use as much as possible markdown files.
Finally, I will explain how to set up your repository on GitHub to host your documentation.

# Create your first documentation

In this section, we will see how to create a documentation using sphinx.

## Install Sphinx

First let us install sphinx:

```bash
pip install sphinx
```

Now we have everything we need to create our documentation 😄.

## Create the initial files

Sphinx organize the documentation in three things:

- makefiles to generate the documentation
- source pages that contain the documentation instruction
- the resulting documentation (pdf file, html webpages and so on)

Now let's create this organization using `quickstart` tool from sphinx:

```bash
mkdir docs
cd docs
sphinx-quickstart
```

In the quickstart instructions, choose separate source folder and build folder (as it will be more convenient for next steps).
Fill the rest as you please.

Afterwards, sphinx created two folders (`docs/sources` and `docs/build`) and a makefile (`docs/Makefile`).
Sphinx source files are in `docs/sources` and the `index.rst` file represents the first page of your documentation (the equivalent of the `index.html` of a website).
To compile your documentation into a website or a pdf file you can use the makefile (using `make html` or `make pdf`).
The resulted documentation format will be putted into `docs/build`.

# Include/link files into a rst file

Here I will not detail how the rst format works as I avoid it as much as I can.
Instead, I will show you how to add documents or link documents into a rst file.
If you want to learn how the rst format works, go [there](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html).

I prefer to avoid using rst files as much as I can as it is possible to use markdown for most of the time.
However, in cases where rst files are needed (such as `index.rst`) it is useful to know how to include or link files.
Therefore this is what I will explain in this section.

## Link a rst file into a doctree

To add a link to a file (say `extra_document.rst`) you need to add `extra_document` to the doctrine as follows:

```rst
.. toctree::
    :maxdepth: 2

    extra_document
```

**Warning:**

The `extra_document` indentation must be like the indentation of the `:maxdepth: 2` line.
If it is not the case, you will have a warning telling you sphinx can't find your file (took me hours to figure it out).

## Include a rst file into a rst file:

To add a `rst` file inline of a `rst` file, you just need to add the following line:

```bash
.. include:: my_file.rst
```

The document that contains this line will be filled with the `my_file.rst` content.

# Write sphinx documentation using markdown and autodoc

## Configure sphinx with markdown

To add markdown support for sphinx I use `m2r` instead of `recommonmark` (advised in the official documentation of sphinx).
The reason is simple: `recommonmark` do not support `mdinclude` to include markdown documents into `rst` files.
Now let's install `m2r`:

```bash
pip install m2r
```

Then let's edit the `source/config.py` file to add this line:

```python
extensions.append("m2r")
```

Now you can use markdown files in your documentation.
You can add it in the doctree (same as for rst files) or include markdown files into rst files using `mdinclude`.
One little example with `mdinclude`:

```bash
.. mdinclude:: my_file.md
```

## Autodoc

While creating your documentation, it is nice to use the docstring of your python source files to fill your content.
Here I will explain how you can do it using the autodoc extension.

### Install

To enable autodoc you have to add the extension in the `source/config.py` file:

```python
extensions.append('sphinx.ext.autodoc')
```

Also add the following lines to the beginning of the `source/config.py` file:

```python
import os
import sys
sys.path.insert(0, os.path.abspath('../..'))
```

To use the numpy format in the docstring you also have to add the napoleon extension:

```python
extensions.append('sphinx.ext.napoleon')
```

Now we are ready to use autodoc.

### Show elements of a docstring

All the following commands works in rst files (I still search a simple way to use it in markdown files, don't hesite to share in comments 😄).
For more details follow the [official documentation](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html).

#### Use the docstring of the module

```rst
.. automodule:: project_folder.module
```

#### Use all the docstrings of a module

```rst
.. automodule:: project_folder.module
	:members:
```

#### Use the docstring of a specific class

```rst
.. autoclass:: project_folder.module.class
```

# Sphinx theming

Now let's be funky 😄.
We will use themes in there: [showcase of themes](https://sphinx-themes.org/).

On this website, choose your desired documentation style, then click on the pypi hyperlink to install the theme using the `pip` command.
To apply your theme click on the associated `conf.py` hyperlink and look for the `html_theme` variable.
Then copy the corresponding code to modify your `html_theme` variable in your `source/config.py` file.

# Host your documentation on GitHub

Now we know how to create a documentation, it will be nice to upload it into a server.
Here I will explain how I do it for GitHub servers.

## Prepare the structure of the projects

For my projects, I think the cleanest structure is as follows:

```
my_project
|-  my_project_code      --> folder containing your source codes and documentation sources
|-  my_project_gh_pages  --> folder containing your built website
|   |-  html             --> folder containing your documentation (synced with your gh_pages branch)
```

Now let us create this structure.
First, let's create a directory for your project:

```bash
mkdir my_project
cd my_project
```

Now let's create `my_project/my_project_code` and `my_project/my_project_gh_pages` folders:

```bash
mkdir my_project_gh_pages
git clone https://github.com/username/my_project
mv my_project my_project_code
```

Now let's create and prepare the `html` folder:

```bash
git clone https://github.com/username/my_project
mv my_project html
cd html
git branch gh-pages

git symbolic-ref HEAD refs/heads/gh-pages  # auto-switches branches to gh-pages
# remove all files and git indexes for the branch
rm .git/index
git clean -fdx
```

## Modify Sphinx makefile

Now our makefile require modifications to use our gh-pages branch and `my_project_gh_pages` folder.

Edit your `my_project/my_project_code/docs/Makefile` file such as your `BUILDDIR` variable is such as the following:

```
BUILDDIR      = ../../my_project_gh_pages/
```

Now you can generate your documentation:

```bash
make html
```

## Build html files for GitHub

Your documentation is now under `my_project/my_project_gh_pages/html`.
Now `commit` and `push` your documentation such as:

```bash
cd my_project_gh_pages/
git add html
git commit -m "First documentation commit"
git push --set-upstream origin gh-pages
```

It is now available under `https://username.github.io/my_project`, unless you have made a redirection for your GitHub Pages (like for this blog).

## Force no jekyll

GitHub server use jekyll which provoque errors while interpreting html files generated by sphinx.
Hopefully, we can disable jekyll.
Inside the `my_project/my_project_gh_pages/html` folder type the following commands:

```bash
touch .nojekyll
git add .nojekyll
git commit -m "added .nojekyll"
git push origin gh-pages
```

## Build and commit automatically

You can add a command inside your Makefile:

```bash
    pushhtml: html

    cd $(BUILDDIR)/html; git add . ; git commit -m "rebuilt docs"; git push origin gh-pages
```

Now by tipping `make pushhtml` you will build your documentation, commit it and push it on GitHub automatically.
It comes alongside the `make html` command that I use to test some modifications without pushing it.

# Sources/inspirations

- [Official sphinx Documentation](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

- [Getting started with sphinx](https://docs.readthedocs.io/en/stable/intro/getting-started-with-sphinx.html)

- [Getting started with autodoc](https://medium.com/@eikonomega/getting-started-with-sphinx-autodoc-part-1-2cebbbca5365)

- [Publishing sphinx documentation on GitHub](https://daler.github.io/sphinxdoc-test/includeme.html)

Hope it helps some of you.

Cheers, Vincent
