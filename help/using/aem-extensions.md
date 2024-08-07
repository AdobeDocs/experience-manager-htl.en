---
title: AEM Extensions
description: AEM offers extensions of the HTL specification to AEM for your convenience as a developer.
exl-id: d78cb84d-f958-45e2-9c6c-df86a68277d5
---
# AEM Extensions {#aem-extensions}

Similar to the [Apache Sling extensions of the HTL specification](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#extensions-of-the-htl-specification-1), AEM offers some additional expression options that make working with AEM concepts a bit easier directly in the HTL scripts.

## i18n {#i18n}

The same [three additional options](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#i18n) as in Apache Sling can be used together with `i18n`:

* `locale`
* `hint`
* `basename`

However in AEM, the [internationalization support](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/components/internationalization/i18n-dev) for HTL is implemented with the help of the API from the `com.day.cq.i18n` package.

## `data-sly-include` {#data-sly-include}

In AEM, `data-sly-include` can take an additional `wcmmode` option that control the [WCM Mode](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/WCMMode.html) for the included script. The allowed values are the names of the available enum constants.

## `data-sly-resource` {#data-sly-resource}

In addition to paths and `Resources`, the `data-sly-resource` block element can also work with [`Maps`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Map.html) or [`Records`](https://github.com/apache/sling-org-apache-sling-scripting-sightly-runtime/blob/master/src/main/java/org/apache/sling/scripting/sightly/Record.java). With both approaches, the `resourceName` String property must be provided. Its value is used to create a [Synthetic Resource](https://www.javadoc.io/doc/org.apache.sling/org.apache.sling.api/latest/org/apache/sling/api/resource/SyntheticResource.html) that is included in the rendering context. The rest of the properties from the `Record` or the `Map` passed to `data-sly-resource` are used as normal `Resource` properties. If the `sling:resourceType` property is missing from this map, the resource type is assumed to be either the value of the `resourceType` [expression option](https://github.com/adobe/htl-spec/blob/1.4/SPECIFICATION.md#229-resource) or the resource type of the current resource that drives the rendering.

Given the following map/record properties available in the script scope as `map`:

```javascript
{
    resourceName: "myText",
    "sling:resourceType": "core/wcm/components/text/v2/text",
    "text": "Hello World!"
}
```

And given the following markup:

```html
<div class="outer" data-sly-resource="${map}"></div>
```

The following output is expected:

```html
<div class="outer">
    <div class="myText">
        <div data-cmp-data-layer="{&quot;text-e58d65c472&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;xdm:text&quot;:&quot;<p>Hello world!</p>&quot;}}" id="text-e58d65c472" class="cmp-text">
            <p>Hello world!</p>
        </div>
  </div>
</div>
```
