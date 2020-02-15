---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Remarkjs Hugoacademic"
subtitle: ""
summary: "How I created my first slide deck in Hugo using remark.js and the Academic theme"
authors: [admin]
tags: []
categories: []
date: 2020-02-15T09:45:38+01:00
lastmod: 2020-02-15T09:45:38+01:00
featured: false
draft: true

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

# Creating slide decks in Hugo Academic using remarkJS

- install hugo academic with git
- see instructions in remark git repo: using with hugo
- didn't work because couldn't push to academic repo
- didn't realize that there is supposed to be a top-level layouts folder (not there in hugo-academic)
- create it manually, add code from remark -using with hugo website
- replace header with basic template from remark website
- add custom CSS file for columns, references, etc
- add mathjax code from mathjax remark website
- add sound
- incremental slides don't work within columns (maybe if I improve CSS inheritance). proposed pragmatic solution
