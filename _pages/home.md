---
layout: default
permalink: /
hidden: true
header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/home/mm-home-page-feature.jpg
  actions:
    - label: "<i class='fas fa-user-circle'></i> About Me"
      url: "/portfolio/"
excerpt: >
  Welcome to my blog.<br />
feature_row:
  - image_path: /assets/images/logo/heart-beat.png
    alt: "portfolio"
    title: "My portfolio"
    excerpt: "Introduce Eunwoo Jung"
    url: "/portfolio/"
    btn_class: "btn--primary"
    btn_label: "Click here"
  - image_path: /assets/images/logo/heart-beat.png
    alt: "Blog"
    title: "My posts"
    excerpt: "My all posts"
    url: "/categories/"
    btn_class: "btn--primary"
    btn_label: "Click here"
excerpt: >
  Please write a guestbook.
comments: true  
---

{% include home.html %}


{% include feature_row %}
