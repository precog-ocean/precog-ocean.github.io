---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: splash
author_profile: false
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/squid2_copy.png
  actions:
    - label: "Github Repositories"
      url: "https://github.com/precog-ocean"
  caption: "Photo credit: [**NOAA**](https://unsplash.com)"
excerpt: "PREdicting biological Carbon in the Ocean Globally."
feature_row:
  - image_path: assets/images/te_models.png
    alt: "placeholder image 1"
    title: "About"
    excerpt: "Learn more about the PRECOG project"
    url: "/about"
    btn_label: "Read More"
    btn_class: "btn--primary"
  - image_path: assets/images/IMG_2737.png
    #image_caption: "Image courtesy of [Unsplash](https://unsplash.com/)"
    alt: "placeholder image 2"
    title: "Research Team"
    excerpt: "Meet the Research Team"
    url: "/researchteam"
    btn_label: "Read More"
    btn_class: "btn--primary"
  - image_path: assets/images/bgc_books.png
    title: "Research Outputs"
    excerpt: "Read about the research"
    url: "/researchteam"
    btn_label: "Read More"
    btn_class: "btn--primary"
---

<img src="/assets/images/precog_logos.png" 
        alt="Picture" 
        width="800" 
        height="600" 
        style="display: block; margin: 0 auto" />

{% include feature_row id="intro" type="center" %}

{% include feature_row %}