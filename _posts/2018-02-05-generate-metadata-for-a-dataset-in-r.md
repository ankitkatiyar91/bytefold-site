---
layout: post
title: Generate metadata for a dataset in R
date: 2018-02-05 23:06:02.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- R
tags: []
meta:
  _edit_last: '1'
  _yoast_wpseo_content_score: '60'
  _yoast_wpseo_primary_category: '59'
  _yoast_wpseo_focuskw_text_input: easy way to generate metadata for your dataset
  _yoast_wpseo_focuskw: easy way to generate metadata for your dataset
  _yoast_wpseo_linkdex: '37'
author:
  login: ankitkatiyar91
  email: ankitkatiyar67@gmail.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/generate-metadata-for-a-dataset-in-r/"
---
Looking at some data in R is a crucial part of any analysis. Even before we start with anything we want to understand the data in hand. Sometimes our datasets are too big to look at each column one by one. Here comes an easy way to generate metadata&nbsp;for your dataset. Although there are various aspects and features you may want to look at, still there are few common that you can look with a simple utility package called **metadata** available on GitHub.&nbsp; You can simply install it and use it like a charm.

### Installation

As of now, this package is currently available only on Github so you need to have _ **devt**** ools**&nbsp;_plugin to install it.

Install devtools&nbsp;(if you don't have it)

```
install.packages("devtools")
```

Load _ **devtools** _&nbsp;and install _ **metadata** _

```
library(devtools)
devtools::install_github("ankitkatiyar91/metadata")
```

Yup! you are ready to use it.

_metadata_ defines two functions only (at least&nbsp;when I am writing this)

- getmode
- generateMeta

_**generateMeta()**_ is general purpose function that we can use to generate metadata. This function returns a&nbsp;dataset that contains names of the columns and some key properties about them.

let's try this on built-in&nbsp;iris dataset

```
metadata::generateMeta(iris)
```

**Output:**

```
name na_Count blanks unique min max range medians mean mode
1 Sepal.Length 0 0 35 4.3 7.9 3.6 5.80 5.84 5.0
2 Sepal.Width 0 0 23 2.0 4.4 2.4 3.00 3.06 3.0
3 Petal.Length 0 0 43 1.0 6.9 5.9 4.35 3.76 1.4
4 Petal.Width 0 0 22 0.1 2.5 2.4 1.30 1.20 0.2
5 Species 0 0 3 0.0 0.0 0.0 0.00 0.00 0.0
```

&nbsp;

Reference:&nbsp;[https://github.com/ankitkatiyar91/metadata](https://github.com/ankitkatiyar91/metadata)

