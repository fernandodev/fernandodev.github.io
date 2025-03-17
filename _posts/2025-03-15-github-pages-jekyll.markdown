---
layout: post
title:  "Jekyll without githubpages gem limitation"
date:   2025-03-15 21:20:58 +0100
categories: jekyll github-pages
tags:
  - jekyll
keywords:
  - Blog setup
  - GitHub Pages
  - Jekyll
---

<img src="https://cdn.leonardo.ai/users/cb839a97-ac87-4b03-b1f8-a6069e1c7916/generations/317ffff3-add0-451d-8426-0f530ec56acc/Leonardo_Anime_XL_a_human_baby_inside_of_an_ultra_high_tech_eg_1.jpg" alt="a human baby inside of an ultra high tech egg" loading="lazy">

Finally! It is there and alive! This blog is now fully functional without the limitations of github-pages gem. But hey! How did I even do that? Can I still remember it? Let's see the digested steps:

<!-- more -->

## Getting started

- *Did you even GPT?*
- Of course. But honestly it did not resolve or the reply was too lost in itself.

I've took sometime to read and learn a bit about the github actions and wow! I am amazed! 🤯 It is really powerful and versatile.

So here is the problem:

1. The theme of this blog is not supported by github pages https://pages.github.com/themes/
2. jekyll-archives plugin is also not supported

Then I was scretching my head, how come? Such simple thing! It just needs to build the project and then run the server.

So the only way to "bypass" that limitation is by either doing it manually (which is fine, tbh) or creating a github action that will do:

1. fetch the project
2. install the gems
3. build the project
4. push to your target branch the generated `_site`

## Setting up the repo

**On settings/page**

![page setup](https://i.postimg.cc/yYw2CTy9/Screenshot-2025-03-17-at-20-31-55.png)

Source: deploy from a branch
Branch: gh-pages (the script will create this branch automatically)

**then on settings/actions/general**

![gh action](https://i.postimg.cc/7Z3t8r68/Screenshot-2025-03-17-at-20-34-05.png)

This will give sufficient rights to `GITHUB_TOKEN` reserved env secret

## Creating the action

This will create the workflow that GH will pick to run

```
mkdir -p .github/workflows
touch jekyll-github-pages.yml
```

and in the file **jekyll-github-pages.yml**

```yaml
name: Build and Deploy a Jekyll Site to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  jekyll:
    runs-on: macos-latest
    steps:
      - name: 📂 setup
        uses: actions/checkout@v2

        # include the lines below if you are using jekyll-last-modified-at
        # or if you would otherwise need to fetch the full commit history
        # however this may be very slow for large repositories!
        # with:
        # fetch-depth: '0'
      - name: 💎 setup ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3.2

      - name: 🔨 install dependencies & build site
        uses: limjh16/jekyll-action-ts@v2
        with:
          enable_cache: true

      - name: 🚀 deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./_site
          # if the repo you are deploying to is <username>.github.io, uncomment the line below.
          # if you are including the line below, make sure your source files are NOT in the "main" branch:
          # publish_branch: main
```

You literally don't need to change anything.

Now I have to say that [this blog](https://www.moncefbelyamani.com/making-github-pages-work-with-latest-jekyll) has helped me a lot.

With a bit of effort, debugging, in no time i got it running correctly.

In the end the script will create a `gh-pages` branch and there will upload ONLY the contents of `_site` with a special file called `.nojekyll`. This file will tell Github to serve the contents as it is, skipping the jekyll build phase.

## Reflection of the day

My friend, [Leonardo](https://holyswift.app/) once told me:

> After you reach a plateau in your career, it is very likely that if you stop studying, your skill will actually start to degrade over time. The only way to avoid it is to simply learn everyday a little bit of this and that. With time, it will accumulate and you'll have a wide range of knowledge. If you compare yourself with someone that reached its plateau, but did not act to continue learn, the gap between the two of you will be huge, sometimes so big that it won't be able to follow each other.

He then dropped me a chart:

![Career progress](https://i.imgflip.com/9np2o0.jpg)

I can only agree.

## Music

Porcupine Tree is one of the bands that I often listen to, and it never disappoints me.

<iframe style="border-radius:12px" src="https://open.spotify.com/embed/track/4McAd0Ki8W0L5VMAzQfgwj?utm_source=generator" width="100%" height="120" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>
