# Nano

Nano is an ultra minimalist theme for [`vox`](https://github.com/NonEntityDev/vox) with a focus on
simplicity, readability and easy access to content.

## How Nano templates are fed

Every template receives its data from `vox` under one of three names:

| Variable | Populated from | Where it shows up |
|----------|-----------------|--------------------|
| `content` | A single FrontMatter document, rendered on its own page. | Content templates (`article.html`, `page.html`). |
| `items` | A collection of FrontMatter documents, rendered as a list. | Indexing templates (`index.html`, `feed.xml`). |
| `settings` | `settings.yaml`, the site-wide configuration file. | All templates, plus `shared/_navbar.html` and `shared/_footer.html`. |

`content` exposes a document's fields mostly as they are written in its FrontMatter (`title`,
`category.name`, `tags`, ...). `items`, on the other hand, exposes each document as a normalized,
schema.org-flavoured object, so field names differ from the source FrontMatter — see
[FrontMatter → item field mapping](#frontmatter--item-field-mapping) below.

## Available templates

### Content templates

Content templates render one FrontMatter document at a time, made available as `content`.

#### `article.html`

The layout for blog posts and other long-form articles: category, title, optional subtitle,
byline, tags, body and an optional "See also..." section, plus OpenGraph tags and a
`schema.org/Article` JSON-LD block.

| Source | Name | Description | Domain |
|--------|------|-------------|--------|
| FrontMatter | `title` | Article title. Used as the page `<title>`, `og:title`, JSON-LD `headline` and the page heading. | String |
| FrontMatter | `subtitle` | Optional lead-in shown under the title. | String |
| FrontMatter | `excerpt` | Summary used as `description`/`og:description` meta tags. | String |
| FrontMatter | `category.name` | Category display name, shown above the title and in JSON-LD `about.name`. | String |
| FrontMatter | `category.slug` | Slugified category name, used to link to `/categories/<slug>.html`. | String |
| FrontMatter | `tags` | Taxonomy for the content. Rendered as a dot-separated list of links and as `keywords`/`article:tag` meta tags. | List of `{name, slug}` |
| FrontMatter | `type` | Content type, used as `og:type`. | String |
| FrontMatter | `created_at` | Publication date (`yyyy-mm-dd`). Used as `article:published_time` and JSON-LD `datePublished`. | Date |
| FrontMatter | `modified_at` | Last-modified date (`yyyy-mm-dd`). Used as `article:modified_time` and JSON-LD `dateModified`. | Date |
| FrontMatter | `author` | Name of the content author. | String |
| FrontMatter | `permalink_path` | Canonical URL/path of the document. Used as both the `identifier` meta tag and the JSON-LD `identifier`. | String |
| FrontMatter | `body` | Rendered document body. | HTML |
| FrontMatter | `related_content` | Optional list of external/related links rendered under "See also...". | List of `{url, title}` |
| settings.yaml | `settings.site.base_url` | Site base URL, used to build the JSON-LD `about` category link. | String |

Dependencies: `shared/_navbar.html`, `shared/_footer.html`.

#### `page.html`

The layout for static pages (e.g. "About"): a lighter-weight variant of `article.html`, without a
subtitle. Same FrontMatter contract as `article.html` minus `subtitle`.

| Source | Name | Description | Domain |
|--------|------|-------------|--------|
| FrontMatter | `title` | Page title. Used as the page `<title>`, `og:title`, JSON-LD `headline` and the page heading. | String |
| FrontMatter | `excerpt` | Summary used as `description`/`og:description` meta tags. | String |
| FrontMatter | `category.name` | Category display name, shown above the title. | String |
| FrontMatter | `category.slug` | Slugified category name, used to link to `/categories/<slug>.html`. | String |
| FrontMatter | `tags` | Taxonomy for the content. Rendered as a dot-separated list of links and as `keywords`/`article:tag` meta tags. | List of `{name, slug}` |
| FrontMatter | `type` | Content type, used as `og:type`. | String |
| FrontMatter | `created_at` | Publication date (`yyyy-mm-dd`). Used as `article:published_time` and JSON-LD `datePublished`. | Date |
| FrontMatter | `modified_at` | Last-modified date (`yyyy-mm-dd`). Used as `article:modified_time` and JSON-LD `dateModified`. | Date |
| FrontMatter | `author` | Name of the content author. | String |
| FrontMatter | `permalink_path` | Canonical URL/path of the document. Used as both the `identifier` meta tag and the JSON-LD `identifier`. | String |
| FrontMatter | `body` | Rendered document body. | HTML |
| FrontMatter | `related_content` | Optional list of external/related links rendered under "See also...". | List of `{url, title}` |

Dependencies: `shared/_navbar.html`, `shared/_footer.html`.

A reusable FrontMatter scaffold for content templates can be found at
[templates/article.md](templates/article.md).

### Indexing templates

Indexing templates render a collection of FrontMatter documents, made available as `items`. Each
entry in `items` uses the normalized field names described in
[FrontMatter → item field mapping](#frontmatter--item-field-mapping), not the raw FrontMatter names.

#### `index.html`

The site's home/listing page: for every item, it renders the category, headline, byline, tags and
description.

| Source | Name | Description | Domain |
|--------|------|-------------|--------|
| `items[].about.name` | Category display name. | String |
| `items[].about.url` | Link to the category index page. | String |
| `items[].identifier` | Link to the item. | String |
| `items[].headline` | Item title. | String |
| `items[].author[0].name` | Name of the item's author. | String |
| `items[].datePublished` | Publication date. | Date |
| `items[].keywords` | Item tags, rendered as a list of links to `/tags/<tag>.html`. | List of Strings |
| `items[].description` | Item summary, linked to the item. | String |
| settings.yaml | `settings.site.excerpt` | Optional site description, used as `description`/`og:description` meta tags. | String |
| settings.yaml | `settings.site.title` | Site title, used as the page `<title>` and `og:title`. | String |
| settings.yaml | `settings.keywords` | Optional list of site-wide keywords, used as `keywords`/`article:tag` meta tags. | List of Strings |
| — | `pagination.current_page` | Current page number, supplied by `vox` (not from `settings.yaml` or FrontMatter). Used as the `page` meta tag. | Number |

Dependencies: `shared/_navbar.html`, `shared/_footer.html`.

#### `feed.xml`

An RSS 2.0 feed for the site's items.

| Source | Name | Description | Domain |
|--------|------|-------------|--------|
| `items[].headline` | Item title. | String |
| `items[].identifier` | Path to the item, appended to `settings.site.base_url` to build the item link/guid. | String |
| `items[].datePublished` | Optional publication date, used as `pubDate`. | Date |
| `items[].author[0].name` | Optional author name. | String |
| `items[].about.name` | Optional category name, used as `category`. | String |
| `items[].description` | Optional item summary, used as `description` and `content:encoded`. | String |
| settings.yaml | `settings.site.title` | Feed/channel title. | String |
| settings.yaml | `settings.site.base_url` | Site base URL, used to build item links/guids and, as a fallback, the feed's own `atom:link`. | String |
| settings.yaml | `settings.site.description` | Feed/channel description. | String |
| settings.yaml | `settings.site.feed_url` | Optional canonical feed URL, used for the feed's own `atom:link`. | String |
| settings.yaml | `settings.site.url` | Fallback base URL used to build the feed's own `atom:link` when `feed_url` is not set. | String |
| settings.yaml | `settings.site.last_build_date` | Optional feed build date, used as `lastBuildDate`. | Date |

### Shared fragments

These are not standalone templates — they're included by the content and indexing templates above.

| Template | Source | Name | Description | Domain |
|----------|--------|------|-------------|--------|
| `shared/_navbar.html` | settings.yaml | `settings.site.name` | Site name/brand, shown in the navbar. | String |
| `shared/_footer.html` | settings.yaml | `settings.site.author` | Name of the site owner, shown in the footer. | String |

## FrontMatter → item field mapping

The same FrontMatter document is exposed differently depending on whether it is being rendered on
its own page (`content`) or listed alongside others (`items`):

| FrontMatter field | `content.*` name | `items[].*` name |
|--------------------|-------------------|--------------------|
| `title` | `title` | `headline` |
| `excerpt` / body | `excerpt` | `description` |
| `category.name` | `category.name` | `about.name` |
| `category.slug` | `category.slug` | `about.url` (already resolved to a link) |
| `tags` | `tags` (list of `{name, slug}`) | `keywords` (list of strings) |
| `author` | `author` (string) | `author[0].name` (list of `{name}`) |
| `created_at` | `created_at` | `datePublished` |
| `permalink_path` | `permalink_path` | `identifier` |

## Example FrontMatter document

The following is a valid example of a FrontMatter file ready to use with `article.html` or
`page.html`:

```yaml
---
title: Sample Article
excerpt: ->
    This is a sample FrontMatter file that contains all the necessary fields to
    properly be rendered using the article.html template of Nano theme.
category:
    name: Documentation
    slug: documentation
tags:
    - name: Theme
      slug: theme
    - name: Nano
      slug: nano
type: article
created_at: 2026-08-19
modified_at: 2026-08-20
author: NonEntityDev
related_content:
    - url: http://domain.com/docs
      title: Official Theme Documentation
---

This is an **example** of a FrontMatter document for articles that is ready to be
rendered using the ```article.html``` template of the Nano theme.
```

## Example `settings.yaml`

The following covers every setting read by Nano's templates:

```yaml
site:
    name: My Site
    title: My Site
    excerpt: A minimalist blog about things.
    description: A minimalist blog about things.
    author: NonEntityDev
    base_url: https://domain.com
    url: https://domain.com
    feed_url: https://domain.com/feed.xml
    last_build_date: 2026-08-19
keywords:
    - blog
    - minimalist
```

## Assets

Nano bundles [Bootstrap 5](https://getbootstrap.com) (`css/bootstrap.min.css`,
`js/bootstrap.bundle.min.js`) for layout and components, a small custom stylesheet
(`css/style.css`), and self-hosted webfonts under `fonts/`.
