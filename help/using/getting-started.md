---
title: Getting Started with HTL
description: Learn about HTL, the preferred and recommended server-side template system for HTML in AEM, and understand the major concepts of the language and its fundamental constructs.
exl-id: c95eb1b3-3b96-4727-8f4f-d54e7136a8f9
---

# Getting Started with HTL {#getting-started-with-htl}

HTML Template Language (HTL) is the preferred and recommended server-side template system for HTML in Adobe Experience Manager. As in all HTML server-side templating systems, an HTL file defines the output sent to the browser by specifying the HTML itself, some basic presentation logic, and variables to be evaluated at runtime.

This document gives an overview of the purpose of HTL as well as an introduction to fundamental concepts and constructs of the language.

>[!TIP]
>
>**Have you considered Edge Delivery Services for AEM?**
>
>You can continue using the methods described in this document for existing projects. However, for new projects, Adobe recommends leveraging [Edge Delivery Services.](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/overview)

>[!TIP]
>
>This document presents the purpose of HTL and an overview of its fundamental structure and concepts. If you have questions about specific syntax, see the [HTL specification](specification.md).

## HTL Layers {#layers}

In AEM, a number of layers define HTL.

1. **[HTL Specification](specification.md)** - HTL is an open-source, platform-agnostic specification, which anyone is free to implement.
1. **[`Sling` HTL Scripting Engine](specification.md)** - The `Sling` project has created the reference implementation of HTL, which is used by AEM.
1. **[AEM Extensions](specification.md)** - AEM builds on top of the `Sling` HTL Scripting Engine to offer developers convenient features specific to AEM.

This HTL documentation focuses on using HTL to develop AEM solutions. As such, it touches all three layers, linking external resources as necessary.

## Fundamental Concepts of HTL {#fundamental-concepts-of-htl}

The HTML Template Language uses an expression language to insert pieces of content into the rendered markup, and HTML5 data attributes to define statements over blocks of markup (like conditions or iterations). As HTL gets compiled into Java Servlets, the expressions and the HTL data attributes are both evaluated entirely server-side, and nothing remains visible in the resulting HTML.

>[!TIP]
>
>To run most examples provided on this page, a live execution environment called the [Read Eval Print Loop](https://github.com/adobe/aem-htl-repl) can be used.

### Blocks and Expressions {#blocks-and-expressions}

Here's a first example, which could be contained as is in a `template.html` file:

```xml
<h1 data-sly-test="${properties.jcr:title}">
    ${properties.jcr:title}
</h1>
```

Two different kinds of syntaxes can be distinguished:

* **Block Statements** - If you want to display the `<h1>` element conditionally, use a `data-sly-test` HTML5 data attribute. HTL provides multiple such attributes, which allow attaching behavior to any HTML element, and all are prefixed with `data-sly`.  
* **Expression Language** - The `${` and `}` characters delimit HTL expressions. At runtime, these expressions are evaluated and their value is injected into the outgoing HTML stream.

See the [HTL specification](specification.md) for details on both syntaxes.

### The SLY Element {#the-sly-element}

A central concept of HTL is to offer the possibility of reusing existing HTML elements to define block statements. This reuse avoids the need of inserting additional delimiters to define where the statement starts and ends. Annotating the markup unobtrusively transforms static HTML into a dynamic template without breaking HTML validity, ensuring proper display even as static files.

However, sometimes there might not be an existing element at the exact location where a block statement has to be inserted. In such cases, you can insert a special `sly` element. This element is automatically removed from the output while running the attached block statements and displaying its content accordingly.

The following example:

```xml
<sly data-sly-test="${properties.jcr:title && properties.jcr:description}">
    <h1>${properties.jcr:title}</h1>
    <p>${properties.jcr:description}</p>
</sly>
```

Outputs something like following HTML, but only if there is both a `jcr:title` and a `jcr:description` property defined, and if neither of them are empty:

```xml
<h1>MY TITLE</h1>
<p>MY DESCRIPTION</p>
```

One thing to keep in mind is only to use the `sly` element when no existing element could have been annotated with the block statement. The reason is because `sly` elements deter the value offered by the language not to alter the static HTML when making it dynamic.

For example, if the previous example would have been wrapped inside a `div` element, then the added `sly` element would be abusive:

```xml
<div>
    <sly data-sly-test="${properties.jcr:title && properties.jcr:description}">
        <h1>${properties.jcr:title}</h1>
        <p>${properties.jcr:description}</p>
    </sly>
</div>
```

And the `div` element could have been annotated with the condition:

```xml
<div data-sly-test="${properties.jcr:title && properties.jcr:description}">
    <h1>${properties.jcr:title}</h1>
    <p>${properties.jcr:description}</p>
</div>
```

### HTL Comments {#htl-comments}

The following example shows an HTL comment on the first line and an HTML comment on the second line.

```xml
<!--/* An HTL Comment */-->
<!-- An HTML Comment -->
```

HTL comments are HTML comments with an additional JavaScript-like syntax. The processor entirely ignores the whole HTL comment and anything within, removing it from the output.

The content of standard HTML comments however is passed through and expressions within the comment are evaluated.

HTML comments cannot contain HTL comments and vice versa.

### Special Contexts {#special-contexts}

To be able to make the best use of HTL, it is important to understand well the consequences of it being based on the HTML syntax.

Please refer to the [Display Context section](https://github.com/adobe/htl-spec/blob/1.4/SPECIFICATION.md#121-display-context) of the HTL specification for more details.

### Element and Attribute Names {#element-and-attribute-names}

Expressions can only be placed in HTML text or attribute values, but not within element names or attribute names, or it wouldn't be valid HTML anymore. To set element names dynamically, the `data-sly-element` statement can be used on the desired elements, and to set attribute names dynamically, even setting multiple attributes at once, the `data-sly-attribute` statement can be used.

```xml
<h1 data-sly-element="${myElementName}" data-sly-attribute="${myAttributeMap}">...</h1>
```

### Contexts Without Block Statements {#contexts-without-block-statements}

As HTL uses data attributes for defining block statements, it is not possible to define such block statements inside of the following contexts, and only expressions can be used there:

* HTML comments
* Script elements
* Style elements

The reason for it is that the content of these contexts is text and not HTML, and contained HTML elements would be considered as simple character data. So, without real HTML elements, there also cannot be `data-sly` attributes run.

This approach may sound like a significant restriction. However, it is preferred because the HTML Template Language should only generate valid HTML output. The [Use-API for Accessing Logic](#use-api-for-accessing-logic) section below introduces how additional logic can be called from the template, which can be used if it is needed to prepare complex output for these contexts. To send data from the back-end to a front-end script, generate a JSON string with the component's logic and place it in a data attribute using a simple HTL expression.

The following example illustrates the behavior for HTML comments, but in script or style elements, the same behavior would be observed:

```xml
<!--
    The title is: ${properties.jcr:title}
    <h1 data-sly-test="${properties.jcr:title}">${properties.jcr:title}</h1>
-->
```

Outputs something like the following HTML:

```xml
<!--
    The title is: MY TITLE
    <h1 data-sly-test="MY TITLE">MY TITLE</h1>
-->
```

### Explicit Contexts Required {#explicit-contexts-required}

As explained in the [Automatic Context-Aware Escaping](#automatic-context-aware-escaping) section below, one objective of HTL is to reduce the risks of introducing cross-site scripting (XSS) vulnerabilities by automatically applying context-aware escaping to all expressions. HTL detects the context of expressions in HTML markup but does not analyze inline JavaScript or CSS, so developers must specify the exact context for these expressions.

Since not applying the correct escaping results in XSS vulnerabilities, HTL does therefore remove the output of all expressions that are in script and style contexts when the context has not been declared.

Here is an example of how to set the context for expressions placed inside scripts and styles:

```xml
<script> var trackingID = "${myTrackingID @ context='scriptString'}"; </script>
<style> a { font-family: "${myFont @ context='styleString'}"; } </style>
```

For more details about how to control the escaping, refer to the [Expression Language Display Context](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#121-display-context) section of the HTL specifications.

## General Capabilities of HTL {#general-capabilities-of-htl}

This section quickly walks through the general features of the HTML Template Language.

### Use-API for Accessing Logic {#use-api-for-accessing-logic}

The HTML Template Language (HTL) Java Use-API enables an HTL file to access helper methods in a custom Java class through `data-sly-use`. This process allows all complex business logic to be encapsulated in the Java code, while the HTL code deals only with direct markup production.

See the document [HTL Java Use-API](java-use-api.md) for more details.

### Automatic Context-Aware Escaping {#automatic-context-aware-escaping}

Consider the following example:

```xml
<p data-sly-use.logic="logic.js">
    <a href="${logic.link}" title="${logic.title}">
        ${logic.text}
    </a>
</p>
```

In most template languages, this example would potentially create a cross-site scripting (XSS) vulnerability, because even when all variables are automatically HTML-escaped, the `href` attribute must still be specifically URL-escaped. This omission is one of the most common errors, because it can be easily forgotten, and it is difficult to spot in an automated manner.

To help, the HTML Template Language automatically escapes each variable accordingly to the context in which it is placed. This approach is possible thanks to the fact that HTL understands the syntax of HTML.

Assuming following `logic.js` file:

```javascript
use(function () {
    return {
        link:  "#my link's safe",
        title: "my title's safe",
        text:  "my text's safe"
    };
});
```

The initial example results in the following output:

```xml
<p>
    <a href="#my%20link%27s%20safe" title="my title&#39;s safe">
        my text&#39;s safe
    </a>
</p>
```

Notice how the two attributes got escaped differently, because HTL knows that `href` and `src` attributes must be escaped for URI context. Also, if the URI started with `javascript:`, the attribute would have been removed entirely, unless the context were explicitly changed to something else.

For more details about how to control the escaping, refer to the [Expression Language Display Context](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#121-display-context) section of the HTL specifications.

### Automatic Removal of Empty Attributes {#automatic-removal-of-empty-attributes}

Consider the following example:

```xml
<p class="${properties.class}">some text</p>
```

If the value of the `class` property happens to be empty, the HTML Template Language automatically removes the entire `class` attribute from the output.

Again, this process is possible because HTL understands the HTML syntax and can therefore conditionally show attributes with dynamic values only if their value is not empty. The reason is very convenient because it avoids adding a condition block around attributes, which would have made the markup invalid and unreadable.

Additionally, the type of the variable placed in the expression matters:

* **String:**
  * **not empty:** Sets the string as an attribute value.
  * **empty:** Removes the attribute altogether.

* **Number:** Sets the value as an attribute value.  

* **Boolean:**
  * **true:** Displays the attribute without value (as a Boolean HTML attribute
  * **false:** Removes the attribute altogether.

Here's an example of how a Boolean expression would allow control of a Boolean HTML attribute:

```xml
<input type="checkbox" checked="${properties.isChecked}"/>
```

For setting attributes, the `data-sly-attribute` statement might also be useful.

## Common Patterns with HTL {#common-patterns-with-htl}

This section introduces a few common scenarios. It explains how best to solve these scenarios with the HTML Template Language.

### Loading Client Libraries {#loading-client-libraries}

In HTL, client libraries are loaded through a helper template provided by AEM, which can be accessed through `data-sly-use`. Three templates are available in this file, which can be called through `data-sly-call`:

* **`css`** - Loads only the CSS files of the referenced client libraries.
* **`js`** - Loads only the JavaScript files of the referenced client libraries.
* **`all`** - Loads all the files of the referenced client libraries (both CSS and JavaScript).

Each helper template expects a `categories` option for referencing the desired client libraries. That option can be either an array of string values, or a string containing a comma-separated values list.

The following are two short examples.

#### Loading multiple client libraries fully at once {#loading-multiple-client-libraries-fully-at-once}

```xml
<sly data-sly-use.clientlib="/libs/granite/sightly/templates/clientlib.html"
     data-sly-call="${clientlib.all @ categories=['myCategory1', 'myCategory2']}"/>
```

#### Referencing a client library in different sections of a page {#referencing-a-client-library-in-different-sections-of-a-page}

```xml
<!doctype html>
<html data-sly-use.clientlib="/libs/granite/sightly/templates/clientlib.html">
    <head>
        <!-- HTML meta-data -->
        <sly data-sly-call="${clientlib.css @ categories='myCategory'}"/>
    </head>
    <body>
        <!-- page content -->
        <sly data-sly-call="${clientlib.js @ categories='myCategory'}"/>
    </body>
</html>
```

In this example, if the HTML `head` and `body` elements are in separate files, the `clientlib.html` template must be loaded in each file that requires it.

The section on the template &amp; call statements in the [HTL specification](specification.md) provides more details about how declaring and calling such templates work.

### Passing Data to the Client {#passing-data-to-the-client}

The best and most elegant way to pass data to the client in general, but even more so with HTL, is to use `data` attributes.

The following example demonstrates how to serialize an object to JSON (also possible in Java) for passing to the client. It can then be easily placed into a `data` attribute:

```xml
<!--/* template.html file: */-->
<div data-sly-use.logic="logic.js" data-json="${logic.json}">...</div>
```

```javascript
/* logic.js file: */
use(function () {
    var myData = {
        str: "foo",
        arr: [1, 2, 3]
    };

    return {
        json: JSON.stringify(myData)
    };
});
```

From there, it is easy to imagine how a client-side JavaScript can access that attribute and parse again the JSON. This approach would be the corresponding JavaScript to place into a client library, for instance:

```javascript
var elements = document.querySelectorAll("[data-json]");
for (var i = 0; i < elements.length; i++) {
    var obj = JSON.parse(elements[i].dataset.json);
    //console.log(obj);
}
```

### Working with Client-Side Templates {#working-with-client-side-templates}

One special case, where the technique explained in the section [Lifting Limitations of Special Contexts](#lifting-limitations-of-special-contexts) can legitimately be used, is to write client-side templates (like Handlebars for instance) that are located within `scrip` elements. The reason this technique can safely be used in that case, is because the `script` element then doesn't contain JavaScript as assumed, but further HTML elements. Here's an example of how that would work:

```xml
<!--/* template.html file: */-->
<script id="entry-section" type="text/template"
    data-sly-include="entry-section.html"></script>

<!--/* entry-section.html file: */-->
<div class="entry-section">
    <h2 data-sly-test="${properties.entrySectionTitle}">
        ${properties.entrySectionTitle}
    </h2>
    {{#each entry}}<h3><a href="{{link}}">{{title}}</a></h3>{{/each}}
</div>
```

The `script` element's markup can include HTL block statements without explicit contexts, as the Handlebars template content is isolated in its own file. Also, this example shows how server-side-run HTL (like on the `h2` element) can be mixed with a client-side-run Template Language, like Handlebars (shown on the `h3` element).

A more modern technique altogether, would however be to use the HTML `template` element instead, as the benefit would then be that it doesn't require to isolate the content of the templates into separate files.

### Lifting Limitations of Special Contexts {#lifting-limitations-of-special-contexts}

In the special cases where it is needed to bypass the restrictions of the script, style, and comment contexts, it is possible to isolate their content in a separate HTL file. HTL interprets everything in its own file as a standard HTML fragment, ignoring any limiting context from where it was included.

See the [Working with Client-Side Templates](#working-with-client-side-templates) section further down for an example.

>[!CAUTION]
>
>This technique can introduce cross-site scripting (XSS) vulnerabilities. The security aspects should be carefully studied if this approach is used. There usually are better ways to implement the same thing than to rely on this practice.
