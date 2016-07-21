---
title: Data Mining the Internet Archive Collection
authors:
- Peter Organisciak
- Boris Capitanu
- Ian Milligan
date: 2016-07-21
reviewers:
layout: default
---

## Data Analysis in Python through the HTRC Feature Reader
Summary: *Using the 4.8 million volume Extracted Features Dataset from the HathiTrust Research Center, we introduce you to the popular SciPy stack of data science tools in Python, particularly the Pandas library.*

The HathiTrust holds nearly 15 million digitized volumes from libraries around the world. In addition to their individual value, these works in aggregate are extremely valuable for historians. Spanning centuries and genres, a scholar can make inferences about cultural, linguistic, historic, and structural trends in the published word. To simplify access to this collection, the HathiTrust Research Center (HTRC) has released the Extracted Features dataset (Capitanu et al. 2015).

In this lesson, I introduce a library, the HTRC Feature Reader, for working with the Extracted Features dataset using the Python programming language. The HTRC Feature Reader is structured to support work using popular data science libraries, particularly Pandas. In teaching analysis of HathiTrust materials through the HTRC Feature Reader, this tutorial teaches skills that will benefit general data analysis in Python.


Today, you'll learn:

- How to work with "notebooks", a useful, interactive way of data science in Python;
- Methods to read and visualize text data for millions of books with the HTRC Feature Reader; and
- Data malleability: selecting, slicing, and summarizing Extracted Feature data using the flexible "DataFrame" structure.

## The Parts

The **HathiTrust Research Center** (**HTRC**) is the research arm of the HathiTrust, tasked with supporting research usage of the works held by the HathiTrust. Particularly, this support involves mediating large-scale access to material in a non-consumptive manner, which aims to allow research over a work without enabling that work to be traditionally enjoyed or read by a human reader.  Huge digital collections can be of public benefit by allowing scholars to make insights about history and culture, and the non-consumptive model allows for these uses to be sought within the restrictions of intellectual property law.

As part of their mission, the HTRC has released the **Extracted Features** dataset, of features derived for every page of 4.8 million 'volumes' (a generalized term referring to the different types of materials in the HathiTrust collection, of which books are the most prevalent type).

What is a feature? A **feature** is a quantifiable marker of something measurable, a datum. A computer cannot understand the meaning of a sentence implicitly, but it can understand the counts of various words and word forms, or the presence or absence of stylistic markers, from which it can be trained to better understand text. In other words, in text analytics a feature is text converted to the language of the algorithm. A dataset of features can be non-consumptive, assuming there isn't enough information to reconstruct the book text from the features, but is also likely the exact same information a researcher would traditionally have to extract themselves.

Not all features are useful, and not all algorithms use the same features. The HTRC Extracted Features Dataset includes information such as counts of word occurrences tagged by part of speech, line and sentence counts, and counts of characters at the leftmost and rightmost sides of a page. No positional information is provided, so the data does not specify if 'brown' was followed by 'dog', though the information is shared for every single page, so you can at least infer how often 'brown' and 'dog' occurred in the same general vicinity within a text.

With easy access and features already extracted, the Extracted Features dataset offers a great entry point to programmatic text analysis and text mining. To further simplify beginner usage, the HTRC has released the HTRC Feature Reader. The **HTRC Feature Reader** scaffolds use of the dataset with the Python programming language.

This tutorial teaches the fundamentals of using the Extracted Features dataset with the HTRC Feature Reader. The Feature Reader is designed to make use of data structures from the most popular scientific tools in Python, so the skills taught here will apply to other settings of data analysis. In this way, the Extracted Features dataset is a particularly good use case, one useful to historians, for more general skills.

## Possibilities

Historians have access to a huge, century-spanning dataset in the HTRC Extracted Features dataset. It provides extracted textual information for each page in 4 million scanned books. This dataset is useful for the access it provides to the published word, but in having data pre-extracted, it also reduces the number of steps a scholar needs to take in text analysis, and makes research easier for others to reproduce.

Though it is relatively new, the Extracted Features dataset is already seeing use by scholars, as seen on a [page collected by the HTRC](https://wiki.htrc.illinois.edu/display/COM/Extracted+Features+in+the+Wild). [Mimno (2014)](http://mimno.infosci.cornell.edu/wordsim/nearest.html) has processed word co-occurrence tables per year, allowing others to view how correlations between topics change over time. [Underwood (2015)](https://sharc.hathitrust.org/genre) has identified genre (fiction, poetry, drama) for 178k books and released genre-specific word counts, against which [Forster (2015)](http://cforster.com/2015/09/gender-in-hathitrust-dataset/) was able to study author gender by genre.

## Suggested Prior Skills

This lesson aims to provide a gentle but technical introduction to text analysis in Python with the HTRC Feature Reader. Most of the code is provided, but is most useful if you are comfortable tinkering with it and seeing how outputs change when you do.

We recommend a baseline knowledge of Python conventions, which can be learned with Turkel and Crymble's [series of Python lessions](http://programminghistorian.org/lessons/introduction-and-installation) on Programming Historian.

The skills taught here are focused on flexibly accessing and working with already-computed text features. For a better understanding of the process of deriving word features, Programming Historian provides a lesson on [Counting Frequencies](http://programminghistorian.org/lessons/counting-frequencies), by Turkel and Crymble.

# Download the Lesson Files

To follow along, download [lesson_files.zip](https://github.com/htrc/HTRC-Programming-Historian/releases/download/v.0.1/lesson_files.zip) and unzip it to any directory you choose.

The lesson files include a sample of files from the HTRC Extracted Features dataset. After you learn to use the EF data in this lesson, you may want to work with the entirety of the dataset. The details on how to do this are described in [Appendix: rsync](#Appendix: rsync).


## Installation

For this lesson, you need to install the HTRC Feature Reader library for Python alongside the data science libraries that it depends on.

For ease, this lesson will focus on installing Python through a scientific distribution called Anaconda. Anaconda is an easy-to-install Python distribution that already includes most of the dependencies for the HTRC Feature Reader.

To install Anaconda, download the installer for your system from the [Anaconda download page](https://www.continuum.io/downloads) and follow their instructions for installation of either the Windows 64-bit Graphical Installer or the Mac OS X 64-bit Graphical Installer. You can choose either version of Python for this lesson. If you have followed earlier lessons on Python at the *Programming Historian*, you are using Python 2, but the HTRC Feature Reader also supports Python 3.

{% include figure.html filename="conda-install.PNG" caption="Conda Install." %}

### Installing the HTRC Feature Reader

The HTRC Feature Reader can be installed by command line. First open a terminal application:

- *Windows*: Open 'Command Prompt' from the Start Menu and type: `activate`.
- *Mac OX/Linux*: Open 'Terminal' from Applications and type `source activate`.

If Anaconda was properly installed, you should see something similar to this:

{% include figure.html filename="activating-env.png" caption="Activating the default Anaconda environment." %}

Now, you need to type one command:

```bash
conda install -c organisciak htrc-feature-reader
```

This command installs the HTRC Feature Reader and its necessary dependencies. That's it! At this point you have everything necessary to start reading HTRC Feature Reader files.

> *psst*, advanced users: You can install the HTRC Feature Reader *without* Anaconda with `pip install htrc-feature-reader`, though for this lesson you'll need to install two additional libraries `pip install matplotlib jupyter`. Also, note that not all manual installations are alike because of hard-to-configure system optimizations: this is why we recommend Anaconda. If you think your code is going slow, you should check that Numpy has access to [BLAS and LAPACK libraries](http://stackoverflow.com/a/19350234/233577) and install [Pandas recommended packages](http://pandas.pydata.org/pandas-docs/version/0.15.2/install.html#recommended-dependencies). The rest is up to you, advanced user!

## Start a Notebook

Using Python the traditional way -- writing a script to a file and running it -- can become clunky for text analysis, where the ability to look at and interact with data is invaluable.
This lesson uses an alternative approach: Jupyter notebooks.

Jupyter gives you an interactive version of Python (called IPython) that you can access in a "notebook" format in your web browser. This format has many benefits. The interactivity means that you don't need to re-run an entire script each time: you can run or re-run blocks of code as you go along, without losing your enviroment (i.e. the variables and code that are already loaded). The notebook format makes it easier to examine bits of information as you go along, and allows for text blocks to intersperse a narrative.

Jupyter was installed alongside Anaconda in the previous section, so it should be available to load now.

{% include figure.html filename="notebook1.PNG" %}

From the Start Menu (Windows) or Applications directory (Mac OS), open "Jupyter notebook". This will start Jupyter on your computer and open a browser window. Keep the console window in the background, the browser is where the magic happens.

{% include figure.html filename="open-notebook.PNG" %}

If your web browser does not open automatically, Jupyter can be accessed by going to the address "localhost:8888" - or a different port number, which is noted in the console ("The Jupyter Notebook is running at..."):

{% include figure.html filename="notebook-start.png" %}

Jupyter is now showing a directory structure from your home folder. Navigate to the lesson folder where you unzipped [lesson_files.zip](https://github.com/htrc/HTRC-Programming-Historian/releases/download/v.0.1/lesson_files.zip).

In the lesson folder, open `Start Here.pynb`: your first notebook!

{% include figure.html filename="notebook-hello-world.png" caption="Hello world in a notebook." %}

Here there are instructions for editing a cell of text or code, and running it. Try editing and running a cell, and notice that it only affects itself. Here are a few tips for using the notebook as the lesson continues:

- New cells are created with the <i class="fa-plus fa"> Plus</i> button in the toolbar. When not editing, this can be done by pressing 'b' on your keyboard.
- New cells are "code" cells by default, but can be changed to "Markdown" (a type of text input) in a dropdown menu on the toolbar. In edit mode, you can paste in code from this lesson or type it yourself.
- Switching a cell to edit mode is done by pressing Enter.
- Running a cell is done by clicking <i class="fa-step-forward fa"> Play</i> in the toolbar, or with `Ctrl+Enter` (`Cmd+Return` on Mac OS). To run a cell and immediately move forward, use `Shift+Enter` instead.

> An example of a full-fledged notebook is included with the lesson files in `example/Lesson Draft.ipynb`.

Before continuing, click on the title to change it to something more descriptive than "Start Here".

## Reading your First Volume
The HTRC Feature Reader library has three main objects: **FeatureReader**, **Volume**, and **Page**.

The **FeatureReader** object is the interface for loading the dataset files and making sense of them. The files are originally in the JSON format and compressed, which FeatureReader decompresses and parses. It creates an iterator over files, allowing one-by-one access to the files as Volumes. A **Volume** is a representation of a single book or other work. This is where you access features about a work. Many features for a volume are collected from individual pages, to access Page information, you can use the **Page** object.

Let's load two volumes to understand how the FeatureReader works.


```python
from htrc_features import FeatureReader
# Remember to use '/' for windows, else '\'
paths = ['data/sample-file1.basic.json.bz2', 'data/sample-file2.basic.json.bz2']
fr = FeatureReader(paths)
for vol in fr.volumes():
    print(vol.title)
```

    June / by Edith Barnard Delano ; with illustrations.
    You never know your luck; being the story of a matrimonial deserter, by Gilbert Parker ... illustrated by W.L. Jacobs.


Here, the FeatureReader is imported and initialized with file paths pointing to two Extracted Features files. We wrote out the file paths directly here, though when preparing your code for multiple systems, there are better ways to [deal with paths](https://docs.python.org/2/library/os.path.html#os.path.join) in Python. An initialized FeatureReader can be iterated through with a `for` loop. The code in the loop is run for every single volume: the Volume() object for the first volume is assigned to the variable `vol`, the loop code is run, then the next volume is set to `vol`, and so on.

You may recognize `for` loops from past experience iterating through what is known as a `list` in Python. However, it is important to note that `fr.volumes()` is *not* a list. If you try to access it directly, it won't print all the volumes; rather, it identifies itself as a different data structure known as a generator:

{% include figure.html filename="generator.png" %}
