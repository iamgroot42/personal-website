---
title: "Formalizing and Estimating Distribution Inference Risks"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here 
# and it will be replaced with their full name and linked to their profile.
authors:
- admin
- David Evans

# Author notes (optional)
# author_notes:
# - "Equal contribution"
# - "Equal contribution"

date: "2022-09-09T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2022-09-09T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: In *Proceedings on Privacy Enhancing Technologies, 2022*
publication_short: In *PETS, 2022*

abstract: Property inference attacks reveal statistical properties about a training set but are difficult to distinguish from the intrinsic purpose of statistical machine learning, namely to produce models that capture statistical properties about a distribution. Motivated by Yeom et al.'s membership inference framework, we propose a formal and general definition of property inference attacks. The proposed notion describes attacks that can distinguish between possible training distributions, extending beyond previous property inference attacks that infer the ratio of a particular type of data in the training data set such as the proportion of females. We show how our definition captures previous property inference attacks as well as a new attack that can reveal the average node degree or clustering coefficient of a training graph. Our definition also enables a theorem that connects the maximum possible accuracy of inference attacks distinguishing between distributions to the effective size of dataset leaked by the model. To quantify and understand property inference risks, we conduct a series of experiments across a range of different distributions using both black-box and white-box attacks. Our results show that inexpensive attacks are often as effective as expensive meta-classifier attacks, and that there are surprising asymmetries in the effectiveness of attacks. We also extend the state-of-the-art property inference attack to work on convolutional neural networks, and propose techniques to help identify parameters in a model that leak the most information, thus significantly lowering resource requirements for meta-classifier attacks.

# Summary. An optional shortened abstract.
summary: We propose a general definition for property inference attacks that supports arbitrary properties, along with a notion of effective dataset size to quantify property inference leakage. Experiments reveal how similar distributions can have starkly different attack success rates, and simple attacks can yield non-trivial accuracy.

tags: ["property inference", "distribution inference"]

# Display this page in the Featured widget?
# featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://arxiv.org/abs/2109.06024'
url_code: 'https://github.com/iamgroot42/form_est_dist_risks'
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
# image:
#   caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/pLCdAaMFLTE)'
#   focal_point: ""
#   preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
# projects:
# - example
projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
# slides: example
slides: ""
---

<!-- {{% callout note %}}
Click the *Cite* button above to demo the feature to enable visitors to import publication metadata into their reference management software.
{{% /callout %}}

{{% callout note %}}
Create your slides in Markdown - click the *Slides* button to check out the example.
{{% /callout %}}

Supplementary notes can be added here, including [code, math, and images](https://wowchemy.com/docs/writing-markdown-latex/). -->
