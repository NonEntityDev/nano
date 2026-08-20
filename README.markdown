# Nano

Nano is a ultra minimalist theme for ```vox``` with focus on simplicity, readability and easy access
to content.

## Available templates.

### Articles and Pages

There is no structural difference between an article (blog post if you prefer) and a page and both
should use the ```article.html```. This template expects the following fields:

| Source | Name | Description | Domain  |
|--------|------|-------------|---------|
| FrontMatter | title | Article or page title. Will be used both as the page title in the header section and in the page header of the page. | String |
| FrontMatter | excerpt | Article or page summary. Will be used as page description in the page header and in the json-ld object. | String |
| FrontMatter | category.name | Name of the category associated to the content. | String |
| FrontMatter | category.slug | Slugyfied name of the category associated to the content. Used to link with the category index page. | String |
| FrontMatter | tags | Article or page summary keywords. Will be used as a taxonomy of the content being presented as a list of tags in page. Will also be used as keywords meta tag in the page header section. | List of Strings |
| FrontMatter | type | Always 'article'. | String |
| FrontMatter | created_at | Date in the format 'yyyy-mm-dd' of when the content was created. | Date |
| FrontMatter | modified_at | Data in the format 'yyyy-mm-dd' of when the content was lastly modified. | Date |
| FrontMatter | author | Name of the author of the content. | String |
| FrontMatter | related_content | Optional list of related content to be linked in the 'See Also...' section of the page. | List of objects |
| settings.yaml | author | Name of the site author or owner. Only presented at the site footer section. | String |

The following is a valid example of a ```FrontMatter``` file ready to use this template:

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
    - name: Official Theme Documentation
---

This is an **example** of a FrontMatter document that is ready to be rendered using the ```article.html``` template
of the Nano theme.
---
```

#### Dependencies.

This template requires the following two fragmets:

* ```shared/_navbar.html``` - to render the site's main navbar.
* ```shared/_footer.html``` - to render the siteś default footer at the bottom of the page.

