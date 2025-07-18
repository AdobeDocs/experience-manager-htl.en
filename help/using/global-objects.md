---
title: HTL Global Objects
description: Learn about enumerable objects and Java-backed objects in HTL.
exl-id: ca590b92-f1b3-4e44-a04a-a2c10dff256f
---

# HTL Global Objects {#htl-global-objects}

Without having to specify anything, HTL provides access to many objects useful to the developer. These objects are in addition to any that may be introduced through the [Use-API](java-use-api.md).

>[!NOTE]
>
>For developers familiar with JSP development in AEM, HTL provides access to all objects that were commonly available in JSP after including `global.jsp`.

## Enumerable Objects {#enumerable-objects}

These objects provide convenient access to commonly used information. Their content can be accessed with dot notation, and they can be iterated-through using `data-sly-list` or `data-sly-repeat`.

|Variable Name|Description| Backed By |
|--- |--- |--- |
|`properties`|List of properties of the current resource | [`org.apache.sling.api.resource.ValueMap`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ValueMap.html)|
|`pageProperties`|List of page properties of the current page | [`org.apache.sling.api.resource.ValueMap`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ValueMap.html)|
|`inheritedPageProperties`|List of inherited page properties of the current page | [`org.apache.sling.api.resource.ValueMap`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ValueMap.html)|

## Java-backed Objects {#java-backed-objects}

The corresponding Java object backs each of the following objects.

| Variable Name |Description |
|---|---|
| `component` | `com.day.cq.wcm.api.components.Component` |
| `componentContext` | `com.day.cq.wcm.api.components.ComponentContext` |
| `currentContentPolicy`| `com.day.cq.wcm.api.policies.ContentPolicy` |
| `currentContentPolicyProperties`| `com.day.cq.wcm.api.policies.ContentPolicy` |
| `currentDesign` | `com.day.cq.wcm.api.designer.Design` |
| `currentNode` | `javax.jcr.Node` |
| `currentPage` | `com.day.cq.wcm.api.Page` |
| `currentSession` | `javax.servlet.http.HttpSession` |
| `currentStyle` | `com.day.cq.wcm.api.designer.Style` |
| `designer` | `com.day.cq.wcm.api.designer.Designer` |
| `editContext` | `com.day.cq.wcm.api.components.EditContext` |
| `log` | `org.slf4j.Logger` |
| `out` | `java.io.PrintWriter` |
| `pageManager` | `com.day.cq.wcm.api.PageManager` |
| `reader` | `java.io.BufferedReader` |
| `request` | `org.apache.sling.api.SlingHttpServletRequest` |
| `resolver` | `org.apache.sling.api.resource.ResourceResolver` |
| `resource` | `org.apache.sling.api.resource.Resource` |
| `resourceDesign` | `com.day.cq.wcm.api.designer.Design` |
| `resourcePage` | `com.day.cq.wcm.api.Page` |
| `response` | `org.apache.sling.api.SlingHttpServletResponse` |
| `sling` | `org.apache.sling.api.scripting.SlingScriptHelper` |
| `slyWcmHelper` | `com.adobe.cq.sightly.WCMScriptHelper` |
| `wcmmode` | `com.adobe.cq.sightly.SightlyWCMMode` |
| `xssAPI` | `com.adobe.granite.xss.XSSAPI` |

## JavaScript-backed Objects {#javascript-backed-objects}

It is possible to back HTL logic with JavaScript. However, the preferred or recommended method is by using [Sling Models](https://sling.apache.org/documentation/bundles/models.html).

>[!NOTE]
>
>[The JavaScript Use API](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#42-javascript-use-api) is now deprecated for use with AEM as a Cloud Service. Instead, use [the Java Use API](https://experienceleague.adobe.com/en/docs/experience-manager-htl/content/java-use-api).
>
>For more information on deprecated and removed features, see the [AEM as a Cloud Service release notes](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features).
