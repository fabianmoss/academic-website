---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Creating slides in Hugo Academic using remarkJS"
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


In this post, I am going to describe how I installed and used a remark.js slide deck
within the framework of Hugo using the Academic theme. First, I describe how I installed
the relevant packages and then I am going to lay out what kinds of customizations I made.

## 1. Installation of relevant software

### 1.1 Hugo
The installation of the static website generator Hugo is pretty straight-forward and well
described on the official website.

### 1.2 The Academic theme

The Academic theme is also amazingly documented
and the I did not encounter any difficulties during the installation.

By default, reveal.js is used for to create slides in the Academic theme but,
after trying it out for a bit, I had the strong feeling that remark.js suits my needs better.
The talks I give usually involve a lot of figures and formulas. So how did I get remark.js to work?

## 2. Getting remark to work
Luckily, this is also well-documented on the Academic theme site.
- see instructions in remark git repo: using with hugo

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Title</title>
    <meta charset="utf-8">
    <style>
      @import url(https://fonts.googleapis.com/css?family=Yanone+Kaffeesatz);
      @import url(https://fonts.googleapis.com/css?family=Droid+Serif:400,700,400italic);
      @import url(https://fonts.googleapis.com/css?family=Ubuntu+Mono:400,700,400italic);

      body { font-family: 'Droid Serif'; }
      h1, h2, h3 {
        font-family: 'Yanone Kaffeesatz';
        font-weight: normal;
      }
      .remark-code, .remark-inline-code { font-family: 'Ubuntu Mono'; }
    </style>
  </head>
  <body>
    <textarea id="source">

class: center, middle

# Title

---

# Agenda

1. Introduction
2. Deep-dive
3. ...

---

# Introduction

    </textarea>
    <script src="https://remarkjs.com/downloads/remark-latest.min.js">
    </script>
    <script>
      var slideshow = remark.create();
    </script>
  </body>
</html>
```

- didn't work because couldn't push to academic repo
- didn't realize that there is supposed to be a top-level layouts folder (not there in hugo-academic)
- create it manually, add code from remark -using with hugo website
- replace header with basic template from remark website
- it works!

## 3. Customization

So far, everything has been pretty obvious and just entailed following the instructions
in the respective docs. But as soon as I got working on my first presentation in remark.js,
I realized that I would need some customizations.

### 3.1 Multi-columns

I created a custom CSS file containing some classes that I wanted to use.
Specifically, I wanted to have the possibility to display content in multiple columns,
e.g. in order to display a bullet-point list next to a figure, each column taking up
a certain percentage of the overall slide. I decided to go for two simple divisions, namely
50-50 and 60-40, the latter also in its reverse variant. This is what I went for:

```css
.left-col50 {
      width: 49%;
      float: left;
}

.right-col50 {
      width: 49%;
      float: right;
}

.left-col33 {
      width: 33%;
      float: left;
}

.right-col66 {
      width: 66%;
      float: right;
}

.left-col66 {
      width: 66%;
      float: left;
}

.right-col33 {
      width: 33%;
      float: right;
}
```

It has been a while that I dealt with CSS. I am quite sure that there is a better way
to do it. But since my talk was approaching fast, I opted for a pragmatic solution.
I can imagine that using inheritance and taking into account pre-existing classes from the
Academic theme could make the code more elegant and potentially shorter.

### 3.2 References

Since I was goint to present some research,  wanted to be able to display citations and references.
The references should appear on the lower-right bottom of the slide and have a smaller fontsize,
while in-text citations should be displayed in gray and also have a smaller fontsize:

```css
.ref {
  font-size: 60%;
  max-width: 66%;
  text-align: right;
  padding: 0 0 0 33%;
  position: absolute;
  bottom: 7%;
  right: 7%;
}

.cite {
  font-size: 80%;
  color: gray;
}
```

I would also love to find out how to get
the references in my slides directly from a BibTeX file instead of copying (and formatting)
the references on the slides themselves.

## 3.3 Figure captions

Finally, I added a class to display figure captions and one to display emphasized content:

```css
.caption {
  font-size: 75%;
  text-align: center;
}

.alert {
  color: red;
}
```

### 3.4 Math and LaTeX formulas

As I would need to display LaTeX formulas, I followed **THIS** instruction and
added the following code to `single.html`:

```javascript
// Setup MathJax
MathJax.Hub.Config({
    tex2jax: {
      skipTags: ['script', 'noscript', 'style', 'textarea', 'pre']
    }
});

MathJax.Hub.Configured();
```

### 3.5 Audio and video

Since virtually all my talks are somehow about music, including sound in the slideshow
is indispensable for me but since remark.js creates HTML files, I could simply add the
HTML5 `<audio>` tag and point the `src` argument to the file I wanted to play.

```html
<audio controls>
  <source src="file.wav" type="audio/wav">
  Your browser does not support the audio element.
</audio>
```

The `controls` parameter displays the play/stop buttons.
What I haven't figured out yet (not even whether it is possible in principle) is
how to make a sound start or stop on click/key press/etc. For videos, it should work analogously
by using the `<video>` tag.

Well, and that's it already. That's all one needs to know in order to use remark.js
together with the Hugo Academic theme and do some basic style customizations.

## 4. Issues

For some reason, I only got this to work when the `custom.css` file was put in the same directory
as the slides' `index.md` which is not optimal, since this would require me to create one CSS files
for each presentation. I asked this question in the **Spectrum** and got amazing help buy XXX.
I will check out his suggested solution soon, since the talk is now given and I can risk to destroy everything again.

My pragmatic approach for multi-column slides has a big shortcoming: I am unable to use `--` for incremental
slide display within a column. This is definitely annoying and I am planning to find out
how to alter this as soon as possible.

## 5. Conclusion

It remains to be seen whether I will stick to remark.js for all future presentations
but so far I do not see why not. Since everything is based on markdown (and HTML/CSS),
version-controlling the files is as easy as it could only be. Plus, the integration
into Hugo's system works flawlessly which is particularly nice.

I am a bit concerned once I want to show formulas and diagrams (potentially incrementally)
wich I did with LaTeX/Tizk before. Anyway, if you want to have a look at my first attempt
with remark.js, check out my very first [talk](20200218_Stuttgart).
