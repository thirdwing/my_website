---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "GEMM on Raspberry Pi (2)"
subtitle: ""
summary: ""
authors: [Qiang Kou]
tags: [Raspberry Pi]
categories: []
date: 2019-10-28
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Besides the nested loop, there are alternative ways to compute GEMM.

## GEMM using GEMV

GEMM can be computed using GEMV as shown below.

<p><img src="Gemm_gemv.png" alt="center" /></p>

## GEMM using rank-1 update (GER)

GEMM can also be viewed as a series of rank-1 update (GER) operations.

<p><img src="MMM-via-multiple-rank-1-2.png" alt="center" /></p>

## Performance

If we don't use GEMV or GER from a good BLAS and just use a naive loop,
the performance can be even worse than our JPI loop.

<p><img src="gemv_ger.png" alt="center" /></p>
