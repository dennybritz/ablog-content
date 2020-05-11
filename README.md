`ablog-svelte` is a static blog generator based on [svelte](https://svelte.dev/) and [sapper](https://sapper.svelte.dev/). It's short for "academic blog" because it comes with LaTeX, citation, and bibtex support. But you can use it for non-academic blogs or personal websites too.

## Features

- **Static sites.** Once you export your site, you get static HTML and JS pages that can be deployed to any hosting provider such as Github pages or Netlify.
- **Write content in any format** that pandoc [can read](https://pandoc.org/), including Markdown, LaTeX, org-mode, or Jupyter notebooks. It's automatically transformed into HTML pages.
- **Citations** are automatically rendered, added as popups in the text, and linked, like [@NIPS2017_7181], at the bottom of the article in a bibliography section. You can also add your own BibTeX entry for your posts.
- Built-in **syntax highlighting** with [HighlightJS](https://highlightjs.org/) and **math** support with [KaTeX](https://katex.org/)
- **Fast** - Because it's based on [svelte](https://svelte.dev/) and [sapper](https://sapper.svelte.dev/) the generated websites are snappy, with almost perfect [Lighthouse](https://web.dev/measure/) scores.
- **Extensible** - Just change the files in `src/` to modify the layout or to add pages. It's standard [svelte](https://svelte.dev/) + [sapper](https://sapper.svelte.dev/) app, both of which have plenty of documentation.

<!-- - Write content in any format pandoc [can read](https://pandoc.org/), including Markdown, LaTeX, org-mode, or Jupyter notebooks
- We automatically convert your content into HTML pages and downloadable PDF and Markdown versions (you can add more formats)
- **No magic** - The "magic" of reading documents and transforming them into HTML is only ~300 lines of code, in `src/lib`. If you want to modify it, you can. No need to walk through 5 layers of framework abstractions. The rest is a standard NextJS + React app, both of which have plenty of documentation.
- Citations [passed to pandoc](https://pandoc.org/MANUAL.html#citations) are automatically rendered, added as popups in the text, and linked, like [@NIPS2017_7181], at the bottom of your article in a bibliography section. You can also add your own bibtex at the end of your article.
- Syntax highlighting with [Prism](https://prismjs.com/) and math support with [KaTeX](https://katex.org/)
- Everything is rendered during the build process and the output is static HTML, so you can [deploy](https://nextjs.org/docs/deployment) your site to any static hosting provider such as Netlify or Github pages. No server needed.
 -->

## Getting Started

Requirements:

- Docker

First, let's understand how content gets into ablog. Unlike many other blog generators, ablog doesn't read content from local Markdown files. Instead, it reads content from an API. This sounds overly complex, but... This mean you can in theory source content from anywhere, like arbitrary websites, as long as there is a compatible API. It also means that content and presentation are cleanly separated. For example, they could live in different Github repositories.

It's important to note that ablog-svelte is a **static site generator**. This means, the API is only needed while you are developing the site and writing content (similar to a local development server), not when you deploy your site. Once you export your site, you will get static HTML files without any dependencies that you can deploy to Github Pages, Netlify, or any other static site host.

Most people want to use local files as blog content. So that's why we will be using [`ablog-api-local`](https://github.com/dennybritz/ablog-api-local), which is a simple binary that monitors local files and exposes them via an API. It also handles transformations via [pandoc](https://pandoc.org/). This means you can write your blog posts in Markdown, Jupyter, or any other pandoc-compatible format.

So, let's start with writing something:

```bash
# Create a new content directory
# It doesn't matter where you create this
mkdir -p content/my-first-post
```

The way `ablog-api-local` finds content is by looking for `build.yaml` files. Each post has a `build.yaml` that contains metadata, such as title and date and the format. Let's start with a simple one:

```bash
touch content/my-first-post/build.yaml
```

```yaml
title: My first Post
date: "2020-03-03"

format: pandoc
pandoc:
  # Anything below here are just options passed to pandoc
  # see https://pandoc.org/MANUAL.html#default-files
  input-files: my-first-post.md
```

Here, we use pandoc to transform out markdown file to HTML. Most likely this is what you will use most often since it can handle all kinds of file formats. You can read the [`ablog-api-local README`](https://github.com/dennybritz/ablog-api-local) for more options. Since we've specified `my-first-post.md` as our input file, let's create it and write something:

```bash
echo "This is a **Markdown** post!" > content/my-first-post/my-first-post.md
```

Then, start a local development server.

```bash
# this directory will be searched and monitored for new content
export ABLOG_CONTENT_DIR="content/"

docker-compose up dev
```

You can now visit your website at `localhost:3000`. To add more content, check out some of the examples in the [ablog-content](https://github.com/dennybritz/ablog-content) repo. This README you are reading right now is one of them.

### Exporting your Site

```
docker-compose run export
```
