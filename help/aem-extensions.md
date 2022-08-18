---
title: AEM Extensions
description: AEM offers extensions of HTL specific to AEM for your convenience as a developer.
---

# AEM Extensions {#aem-extensions}
Similar to the [Apache Sling extensions of the HTL Specification](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#extensions-of-the-htl-specification-1), AEM offers some additional expression options that make working with AEM concepts a bit easier directly in the HTL scripts.

## `i18n`
The same [three additional options](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#i18n) like in Apache Sling can be used together with `i18n`:
* `locale`
* `hint`
* `basename`

However, in AEM the [internationalizaton support](/docs/experience-manager-65/developing/components/internationalization/i18n-dev.html) for HTL is implemented with the help of the API from the `com.day.cq.i18n` package.

## `data-sly-include`
In AEM `data-sly-include` can take an additional `wcmmode` option that controls the [WCM Mode](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/WCMMode.html) for the included script. The allowed values are the names of the available enum constants.

# `data-sly-resource`
In addition to paths and `Resources`, the `data-sly-resource` block element can also work with [`Maps`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Map.html) or [`Records`](https://github.com/apache/sling-org-apache-sling-scripting-sightly-runtime/blob/master/src/main/java/org/apache/sling/scripting/sightly/Record.java). With both approaches, the `resourceName` String property must be provided. Its value is used to create a [Synthetic Resource](https://www.javadoc.io/doc/org.apache.sling/org.apache.sling.api/latest/org/apache/sling/api/resource/SyntheticResource.html) that will be included in the rendering context. The rest of the properties from the `Record` or `Map` that was passed to `data-sly-resource` will be used as normal `Resource` properties. If the `sling:resourceType` property is missing from this map, the resource type will be assumed to be either the value of the `resourceType` [expression option](https://github.com/adobe/htl-spec/blob/1.4/SPECIFICATION.md#229-resource) or the resource type of the current resource that drives the rendering.

Given the following map/record properties available in the script scope as `map`:
```javascript
{
    resourceName: "myText",
    "sling:resourceType": "core/wcm/components/text/v2/text",
    "text": "Hello World!"
}
```

and the following markup:
```html
<div class="outer" data-sly-resource="${map}"></div>
```

the following output is expected:
```html
<div class="outer">
    <div class="myText">
        <div data-cmp-data-layer="{&quot;text-e58d65c472&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;xdm:text&quot;:&quot;<p>Hello world!</p>&quot;}}" id="text-e58d65c472" class="cmp-text">
            <p>Hello world!</p>
        </div>
  </div>
</div>
```