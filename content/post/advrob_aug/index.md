---
title: Reassessing the benefit of data augmentation to adversarial robustness
subtitle: "A recent [bug discovery](https://tanelp.github.io/posts/a-bug-that-plagues-thousands-of-open-source-ml-projects/) on Pytorch+Numpy got me thinking: how much does this impact adversarial robustness?"

# Summary for listings and search engines
summary: Welcome 👋 We know that first impressions are important, so we've populated your new site with some initial content to help you get familiar with everything in no time.

# Link this post with a project
projects: []

# Date published
date: "2021-06-16T00:00:00Z"

# Date updated
lastmod: "2021-06-16T00:00:00Z"

# Is this an unpublished draft?
draft: true

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Tanel Pärnamaa**](https://tanelp.github.io/posts/a-bug-that-plagues-thousands-of-open-source-ml-projects/)'
  focal_point: ""
  placement: 2
  preview_only: false

authors:
- admin

tags:
- adversarial
---

## Overview

A [post on Reddit ](https://www.reddit.com/r/MachineLearning/comments/mocpgj/p_using_pytorch_numpy_a_bug_that_plagues/) a couple of months ago: "Using PyTorch + NumPy? A bug that plagues thousands of open-source ML projects". Knowing nearly all of my projects use this combination, I read through the [linked blog](https://tanelp.github.io/posts/a-bug-that-plagues-thousands-of-open-source-ml-projects/) by Tanel Pärnamaa to see what it was all about. Quite honestly, I was a bit shocked that it took our community this long to notice a bug this severe! Nearly all data-loaders use more than one worker. Unfortunately, not many people (clearly, since it took us all so long to notice this bug) sit down to debug data augmentation at this level within their ML pipeline. 

Reading through this bug, I remembered how (proper) data-augmentation had been proposed as a means to reduce robust overfitting by authors at DeepMind[1](https://arxiv.org/pdf/2103.01946.pdf). Curious to see the impact of fixing data augmentation on adversarial robustness, I decided to run some experiments of my own.