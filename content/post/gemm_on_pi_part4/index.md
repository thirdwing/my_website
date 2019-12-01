---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "GEMM on Raspberry Pi (4)"
subtitle: ""
summary: ""
authors: [Qiang Kou]
tags: [Raspberry Pi]
categories: []
date: 2019-11-30
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

## Blocking for cache

In this part, we are using a super simple memory model with cache,
so we added an additional blocking for cache.
With the kernels we developed in part3, we also used __192*192__ blocks.

<p><img src="PI_JI.png" alt="center" /></p>

With this simple technique, we can have a gemm with around 2 GFLOPS.
We didn't optimize the cache size, which means we can potentially improve more
by changing the block size.

However, we are still far behind OpenBLAS.

<p><img src="PI_JI_ref.png" alt="center" /></p>
