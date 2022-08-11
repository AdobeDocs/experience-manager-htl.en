---
title: HTL global objects
description: Learn about enumerable objects, Java-backed objects, and JavaScript-backed objects in HTL.
exl-id: ca590b92-f1b3-4e44-a04a-a2c10dff256f
---

# HTL Global Objects {#htl-global-objects}

Without having to specify anything, HTL provides access to all objects that were commonly available in JSP after including `global.jsp`. These objects are in addition to any that may be introduced through the [Use-API.](use-api.md)

## Enumerable Objects {#enumerable-objects}

These objects provide convenient access to commonly used information. Their content can be accessed with dot notation, and they can be iterated-through using `data-sly-list` or `data-sly-repeat`.

|Variable Name|Description| Backed By |
|--- |--- |--- |
|`properties`|List of properties of the current resource | [`org.apache.sling.api.resource.ValueMap`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/org/apache/sling/api/resource/ValueMap.html)|
|`pageProperties`|List of page properties of the current page | [`org.apache.sling.api.resource.ValueMap`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/org/apache/sling/api/resource/ValueMap.html)|
|`inheritedPageProperties`|List of inherited page properties of the current page | [`org.apache.sling.api.resource.ValueMap`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/org/apache/sling/api/resource/ValueMap.html)|

## Java-backed Objects {#java-backed-objects}

Each of the following objects is backed by the corresponding Java object.

The most useful variables in the table below are highlighted in bold.

| Variable Name |Description |
|---|---|
| **`component`** | `com.day.cq.wcm.api.components.Component` |
| `componentContext` | `com.day.cq.wcm.api.components.ComponentContext` |
| **`currentDesign`** | `com.day.cq.wcm.api.designer.Design` |
| `currentNode` | `javax.jcr.Node` |
| **`currentPage`** | `com.day.cq.wcm.api.Page` |
| `currentSession` | `javax.servlet.http.HttpSession` |
| `currentStyle` | `com.day.cq.wcm.api.designer.Style` |
| `designer` | `com.day.cq.wcm.api.designer.Designer` |
| `editContext` | `com.day.cq.wcm.api.components.EditContext` |
| `log` | `org.slf4j.Logger` |
| `out` | `java.io.PrintWriter` |
| `pageManager` | `com.day.cq.wcm.api.PageManager` |
| `reader` | `java.io.BufferedReader` |
| **`request`** | `org.apache.sling.api.SlingHttpServletRequest` |
| `resolver` | `org.apache.sling.api.resource.ResourceResolver` |
| **`resource`** | `org.apache.sling.api.resource.Resource` |
| **`resourceDesign`** | `com.day.cq.wcm.api.designer.Design` |
| **`resourcePage`** | `com.day.cq.wcm.api.Page` |
| `response` | `org.apache.sling.api.SlingHttpServletResponse` |
| `sling` | `org.apache.sling.api.scripting.SlingScriptHelper` |
| `slyWcmHelper` | `com.adobe.cq.sightly.WCMScriptHelper` |
| **`wcmmode`** | `com.adobe.cq.sightly.SightlyWCMMode` |
| `xssAPI` | `com.adobe.granite.xss.XSSAPI` |

## JavaScript-backed Objects {#javascript-backed-objects}

It is possible to back HTL logic with JavaScript. However the preferred or recommended method is by using [Sling Models.](https://sling.apache.org/documentation/bundles/models.html)

## Additional Resources {#additional-resources}

If you want to dive in to HTL directly consider checking out:

* [The WKND tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) - Use HTL to implement an simple AEM project from scratch
* [The HTL specification](htl-specification.md) - If you have specific questions about HTL syntax
