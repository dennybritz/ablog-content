`ablog-svelte` is a static blog generator based on [svelte](https://svelte.dev/) and [sapper](https://sapper.svelte.dev/). It's short for "academic blog" because it comes with LaTeX, citation, and BibTeX support. Of course, you can use it for non-academic blogs or personal websites too.

## Features

- **Static sites.** Get static HTML + Javascript pages that can be deployed to any hosting provider, including Github pages and Netlify.
- **Write content in any format** that pandoc [can read](https://pandoc.org/), including Markdown, LaTeX, org-mode, or Jupyter notebooks. It's automatically transformed into HTML pages.
- **Citations** are automatically rendered, added as popups in the text, and linked, like [@NIPS2017_7181], at the bottom of the article in a bibliography section. You can also add a BibTeX entry to your posts.
- Built-in **syntax highlighting** with [HighlightJS](https://highlightjs.org/) and **math** support with [KaTeX](https://katex.org/)
- **Fast** - Because it's based on [svelte](https://svelte.dev/) and [sapper](https://sapper.svelte.dev/) the generated websites are performant, with almost perfect [Lighthouse](https://web.dev/measure/) scores.
- **Extensible** - Just change the files in `src/` to modify the layout or add pages. It's standard [svelte](https://svelte.dev/) + [sapper](https://sapper.svelte.dev/) app, both of which have plenty of documentation.


## Getting Started

The following walks you through setting up a local development environment and deploying a copy of this site to your own Github pages.

Requirements:

- [Docker](https://www.docker.com/)
- [optional] [node.js](https://nodejs.org/en/) and [yarn](https://yarnpkg.com/getting-started/install). While you can run your development server within Docker without installing anything, it's often more convenient to run it locally to get features such as live-reload upon file changes.

Let's talk how content gets into ablog. Unlike many other blog generators, ablog doesn't read content from local Markdown files. Instead, it reads content from an API. This sounds overly complex, but it has a few advantages. First, content and presentation are cleanly separated. For example, the content for the website you are looking at right now, lives in [ablog-content](https://github.com/dennybritz/ablog-content). If you want to move to another static site generator you can easily take your content with you. It also means you can, in theory, source content from anywhere, as long as you build a compatible API. 

It's important to note that ablog-svelte is still a **static site generator**. The API is only needed for local development, not when your site is deployed. Once you export your site, you get static HTML files without any dependencies that can be deploy to Github Pages, Netlify, or any other static site host. The static files will contain the content that comes from the API.

Most people want to use local files. That's where [`ablog-api-local`](https://github.com/dennybritz/ablog-api-local) comes in, which is a simple server that monitors local files and exposes them via a compatible API. It also handles format conversion using [pandoc](https://pandoc.org/). This means you can write your blog posts in Markdown, Jupyter, or any other pandoc-compatible format. In other words:

```text
Content (Markdown, etc files)
-> API server (ablog-api-local)
-> Site generator (ablog-svelte)
-> Static HTML and JS
```

Let's start by cloning this repository and getting some example content:

```bash
git clone https://github.com/dennybritz/ablog-svelte/
cd ablog-svelte

# We also clone some example content to play with
git clone https://github.com/dennybritz/ablog-content content/
```

To make sure everything works as expected, let's start the server:

```bash
# This directory will be searched and monitored for new content
export ABLOG_CONTENT_DIR="content/"

# Run everything in docker
docker-compose up dev

# OR if you prefer to run the webserver locally to use live-reload
# you can run only the api in Docker
docker-compose up api
yarn dev
```

You should now have a copy of this website running locally at [`http://localhost:3000`](http://localhost:3000).


Let's talk about about what's going on here. The way `ablog-api-local` finds content is by looking for `build.yaml` files. Each post corresponds to a `build.yaml` that contains metadata such as  title and date. For example, the build file for this README is [here](https://github.com/dennybritz/ablog-content/blob/master/build.yaml):

```yaml
title: README
date: "2020-05-11"
description: |
  Documentation for setting up ablog. Of course, written using ablog itself.
slug: "readme"
authors:
  - name: Denny Britz
    email: "dennybritz@gmail.com"
    url: https://twitter.com/dennybritz
bibtex: |
  ...
pandoc:
  input-files:
    - README.md
```

Check the [`ablog-api-local README`](https://github.com/dennybritz/ablog-api-local) for more metadata options. To add more content, simply add more `build.yaml` files to your `content/` folder. There are several examples you can copy from.

Next, let's deploy your site to Github pages.


### Exporting your Site

Using [sapper's export](https://sapper.svelte.dev/docs/#Exporting) functionality you can export your static site into the `__sapper__` folder:

```bash
# In docker
docker-compose run export

# OR locally
yarn export
```

