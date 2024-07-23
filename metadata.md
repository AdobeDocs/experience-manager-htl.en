---
solution: Experience Manager
type: Documentation
product: adobe experience manager
git-repo: https://github.com/AdobeDocs/experience-manager-htl.en
index: y
recommendations: noDisplay
---

# Metadata for internal use

The GitHub authoring system defines metadata hierarchically, with increasing levels of precedent as seen in the following:

1. metadata.md
1. ToC
1. Article

The metadata defined in the metadata.md file apply to the entire repo, but can be overridden at the ToC and article levels. Any overriding of the metadata should be done at the lowest level possible.

The metadata in the `experience-manager-core-components.en` repo is the minimum required.

metadata.md

* `product`
* `git-repo`
* `index: y`

No longer used:

* `solution-title`
* `solution-hub-url`
* `getting-started-title`
* `getting-started-url`
* `tutorials-title`
* `tutorials-url`

ToCs

* `sub-product`
* `user-guide-title`

Article

* `title`
* `description`
* `index: n` (only for previous versions of components)

