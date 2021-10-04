---
solution: Experience Manager
type: Documentation
product: adobe experience manager
git-repo: https://github.com/AdobeDocs/experience-manager-htl.en
index: y
---

# Metadata for internal use

Metadata in the GitHub authoring system is hierarchal and is defined the the following increasing levels of precedent.

1. metadata.md
1. ToC
1. Article

Metadata defined in the metadata.md file apply to the entire repo, but can be overridden at the ToC and article levels. Any overriding of the metadata should be done at the lowest level possible.

The metadata in the experience-manager-core-components.en repo is the minimum required.

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

Additional information about the metadata can be found in the [internal authoring guide.](https://experienceleague.adobe.com/docs/authoring-guide-exl/using/authoring/features/metadata.html#solution)
