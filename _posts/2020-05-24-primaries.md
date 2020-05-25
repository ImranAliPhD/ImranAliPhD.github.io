---
title: "US Prsidential Primaries 2016"
date: 2020-05-24
tags: [machine learning, support vector machines, data wranggling]
header:
  image: "/images/perceptron.jpeg"
excerpt: "Some description of project that I will write later."
mathjax: "true"
---
<h2>1. Presidential Primaries:</h2>
<h3>1.1 Project Objective, Dataset, Tools and Libraries</h3>

Predict the winners of US presidential primaries in each of the US geographical units identified by Federal Information Processing Standards (FIPS) codes, typically (but not necessarily) a county. That is, based upon features encompassing demographical make-up and socio-economic status of the population in FIPS code area, we predict *which candidate across both parties will score highest number of votes in that FIPS code area during the presidential primaries across both parties*.

*Two important facts about this data and project goals*:

1. At the US State level, except for a few swing states, it is remarkably easy to predict the winner of a political election. But for a more basic geographical unit, such as the one identified by FIPS code, voter's political opinion should be a little more challenging. Furthermore, to make things interesting, I have decided to predict the particular candidate of any party who will score highest votes in a county. That is to say, machine learning model will not only have to account for differences between the fans of Donald Trump and Bernie Sanders (very easy) but also between fans of Donald Trump and Ted Cruz (a little more challenging).

2. A <b>CAVEAT</b>: More importantly, the two facts that, (<b>1</b>) in the early stages of presidential primaries, there are more candidates competing (specially in Republican primaries) than in the end, (<b>2</b>) the demographic features of States with late primaries can not account for dropped out primaries' candidates because samples are not arranged in time-series. (While we do engineer a new feature Date of Primaries by outsourcing information from Wikipedia to address this, it will still impose a limitation on the machine learning model, which may affect classification accuracy).

<h3>1.2 Tools and Libraries</h3>

We will use the well-known sklearn library to build, train and tune the models, in Python programming language. We will use pandas to manipulate the dataset, using dataframe application programming interface (API). We will also utilise some other common Python libraries, such as numpy. We will build the project in Jupyter notebook.

<h3>1.3 About the Dataset</h3>

This dataset was chosen to demonstrate working with raw data. It is not a part of any official Kaggle competition, but was contributed by members to the Kaggle and can be downloaded from Kaggle here. In its original form, dataset comprises of several files, many of them not related to the goals of this project. The files do not arrange feature and label columns properly, as we would have like them and we will do quite a bit of column computing, arranging, mixing and merging to get a dataset with which we can work.

We will download three files, the `county_facts.csv`, `primary_results.csv` and `county_facts_dictionary.csv`. The last file just provides names of columns of `county_facts.csv`, since they are quite long. The `primary_results.csv` file is in a format not very useful to us and we will manipulate to make it a column of `county_facts.csv` dataset.

Let us import pandas and load our files.

```python
import pandas as pd
county_facts = pd.read_csv("county_facts.csv")
columns = pd.read_csv("county_facts_dictionary.csv")
results = pd.read_csv("primary_results.csv")
```

<h2>2. Cleaning and Manipulation of Dataset</h2>

In its raw form, this set comprises of multiple files with required information scattered around. We will perform a series of steps for data cleansing and preparation steps.

<h3>2.1 Fixing Missing Values</h3>

We start by checking the number of missing entries in the two datasets (third files provides list of columns). Here is the subset of `county_facts` dataframe which has `NaN` in any row.

```python
county_facts_nan = county_facts[county_facts
                                .isnull().any(axis=1)]
```
