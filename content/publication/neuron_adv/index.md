---
title: "One Neuron to Fool Them All"

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

date: "2020-06-09T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2020-06-09T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["3"]

# Publication name and optional abbreviated publication name.
publication: In *arXiv*
publication_short: In *arXiv*

abstract: Despite vast research in adversarial examples, the root causes of model susceptibility are not well understood. Instead of looking at attack-specific robustness, we propose a notion that evaluates the sensitivity of individual neurons in terms of how robust the model’s output is to direct perbturbations of that neuron’s output. Analyzing models from this perspective reveals distinctive characteristics of standard as well as adversarially-trained robust models, and leads to several curious results. In our experiments on CIFAR-10 and ImageNet, we find that attacks using a loss function that targets just a single sensitive neuron find adversarial examples nearly as effectively as ones that target the full model. We analyze the properties of these sensitive neurons to propose a regularization term that can help a model achieve robustness to a variety of different perturbation constraints while maintaining accuracy on natural data distributions

# Summary. An optional shortened abstract.
summary: We propose a notion of neuron sensitivity in terms of adversarial robustness, along with an attack that works as well as PGD. The notion can be extended as a regularization term, providing adversarial robustness without adversarial training.

tags: ["adversarial"]

# Display this page in the Featured widget?
featured: false

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://arxiv.org/pdf/2003.09372.pdf'
url_code: 'https://github.com/iamgroot42/sauron/'
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
