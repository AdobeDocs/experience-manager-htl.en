---
title: HTL Java Use-API
description: The HTL Java Use-API enables an HTL file to access helper methods in a custom Java class.
exl-id: 9a9a2bf8-d178-4460-a3ec-cbefcfc09959
---

# HTL Java Use-API {#htl-java-use-api}

The HTL Java Use-API enables an HTL file to access helper methods in a custom Java class.

## Use Case {#use-case}

The HTL Java Use-API enables an HTL file to access helper methods in a custom Java class through `data-sly-use`. This method allows all complex business logic to be encapsulated in the Java code, while the HTL code deals only with direct markup production. 

A Java Use-API object can be a simple POJO, instantiated by a particular implementation through the POJO's default constructor.

The Use-API POJOs can also expose a public method, called init, with the following signature:

```java
    /**
     * Initializes the Use bean.
     *
     * @param bindings All bindings available to the HTL scripts.
     **/
    public void init(javax.script.Bindings bindings);
```

The `bindings` map can contain objects that provide context to the currently run HTL script that the Use-API object can use for its processing.

## A Simple Example {#a-simple-example}

This example illustrates the usage of the Use-API. 

>[!NOTE]
>
>This example is simplified just to illustrate its use. In a production environment, Adobe recommends that you use [Sling models](https://sling.apache.org/documentation/bundles/models.html).

Start with an HTL component, called `info,` that does not have a use-class. It consists of a single file, `/apps/my-example/components/info.html`

```xml
<div>
    <h1>${properties.title}</h1>
    <p>${properties.description}</p>
</div>
```

Add some content for this component to render at `/content/my-example/`:

```xml
{
    "sling:resourceType": "my-example/component/info",
    "title": "My Example",
    "description": "This Is Some Example Content."
}
```

When this content is accessed, the HTL file is run. Within the HTL code, the context object `properties` is used to access the current resource's `title` and `description` and display them. The output file `/content/my-example.html` is the following:

```html
<div>
    <h1>My Example</h1>
    <p>This Is Some Example Content.</p>
</div>
```

### Adding a Use-Class {#adding-a-use-class}

The `info` component as it stands does not need a use-class to perform its simple function. There are cases, however, where you need to do things that cannot be done in HTL and so you need a use-class. But keep in mind the following:

>[!NOTE]
>
>A use-class should only be used when something cannot be done in HTL alone.

For example, suppose that you want the `info` component to display the `title` and `description` properties of the resource, but all in lowercase. Because HTL does not have a method for lowercasing strings, you can add a Java use-class and change `/apps/my-example/component/info/info.html` as follows:

```xml
<div data-sly-use.info="Info">
    <h1>${info.lowerCaseTitle}</h1>
    <p>${info.lowerCaseDescription}</p>
</div>
```

Additionally, `/apps/my-example/component/info/Info.java` is created.

```java
package apps.my_example.components.info;

import com.adobe.cq.sightly.WCMUsePojo;

public class Info extends WCMUsePojo {
    private String lowerCaseTitle;
    private String lowerCaseDescription;

    @Override
    public void activate() throws Exception {
        lowerCaseTitle = getProperties().get("title", "").toLowerCase();
        lowerCaseDescription = getProperties().get("description", "").toLowerCase();
    }

    public String getLowerCaseTitle() {
        return lowerCaseTitle;
    }

    public String getLowerCaseDescription() {
        return lowerCaseDescription;
    }
}
```

Please see the [Java docs for `com.adobe.cq.sightly.WCMUsePojo`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html) for more details.

Now, let's look at the different parts of the code.

### Local vs. Bundle Java Class {#local-vs-bundle-java-class}

The Java use-class can be installed in two ways:

* **Local** - In a local install, the Java source file is placed alongside the HTL file, in the same repository folder. The source is automatically compiled on demand. No separate compilation or packaging step is required.
* **Bundle** - In a bundle install, the Java class must be compiled and deployed within an OSGi bundle using the standard AEM bundle deployment mechanism (see the section [Bundled Java Class](#bundled-java-class)).

To know which method to use when, keep these two points in mind:

* A **local Java use-class** is recommended when the use-class is specific to the component in question.
* A **bundle Java use-class** is recommended when the Java code implements a service that is accessed from multiple HTL components.

This example uses a local install.

### Java Package is Repository Path {#java-package-is-repository-path}

When using a local install, the package name of the use-class must match the repository folder location. Underscores in the package name replace any hyphens in the path.

In this case `Info.java` is located at `/apps/my-example/components/info` so the package is `apps.my_example.components.info`:

```java
package apps.my_example.components.info;

import com.adobe.cq.sightly.WCMUsePojo;

public class Info extends WCMUsePojo {

   ...

}
```

>[!NOTE]
>
>Using hyphens in the names of repository items is a recommended practice in AEM development. However, hyphens are illegal within Java package names. For this reason, **all hyphens in the repository path must be converted to underscores in the package name**.

### Extending `WCMUsePojo` {#extending-wcmusepojo}

While there are a number of ways of incorporating a Java class with HTL, the simplest is to extend the `WCMUsePojo` class. For this example `/apps/my-example/component/info/Info.java`:

```java
package apps.my_example.components.info;

import com.adobe.cq.sightly.WCMUsePojo;

public class Info extends WCMUsePojo

    ...
}
```

### Initializing the Class {#initializing-the-class}

When the use-class is extended from `WCMUsePojo`, initialization is performed by overriding the `activate` method, in this case in `/apps/my-example/component/info/Info.java`

```java
...

public class Info extends WCMUsePojo {
    private String lowerCaseTitle;
    private String lowerCaseDescription;

    @Override
    public void activate() throws Exception {
        lowerCaseTitle = getProperties().get("title", "").toLowerCase();
        lowerCaseDescription = getProperties().get("description", "").toLowerCase();
    }

...

}
```

### Context {#context}

Typically, the [activate](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html) method is used to precompute and store (in member variables) the values needed in your HTL code, based on the current context (the current request and resource, for example).

The `WCMUsePojo` class provides access to the same set of context objects as are available within an HTL file (see the document [Global Objects](global-objects.md).)

In a class extending `WCMUsePojo`, you can access context objects using their names:

[`<T> T get(String name, Class<T> type)`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html)

Alternatively, you can access commonly used context objects directly using the appropriate convenience method listed in this table.

|Object|Convenience Method|
|---|---|
| [`PageManager`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/PageManager.html) | [`getPageManager()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getPageManager--) |
| [`Page`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/Page.html) | [`getCurrentPage()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getCurrentPage--) |
| [`Page`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/Page.html) | [`getResourcePage()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getResourcePage--) |
| [`ValueMap`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ValueMap.html) | [`getPageProperties()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getPageProperties--) |
| [`ValueMap`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ValueMap.html) | [`getProperties()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getProperties--) |
| [`Designer`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/designer/Designer.html) | [`getDesigner()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getDesigner--) |
| [`Design`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/designer/Design.html) | [`getCurrentDesign()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getCurrentDesign--) |
| [`Style`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/designer/Style.html) | [`getCurrentStyle()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getCurrentStyle--) |
| [`Component`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/wcm/api/components/Component.html) | [`getComponent()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getComponent--) |
| [`ValueMap`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ValueMap.html) | [`getInheritedProperties()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getInheritedPageProperties--) |
| [`Resource`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/Resource.html) | [`getResource()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getResource--) |
| [`ResourceResolver`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/resource/ResourceResolver.html) | [`getResourceResolver()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getResourceResolver--) |
| [`SlingHttpServletRequest`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/SlingHttpServletRequest.html) | [`getRequest()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getRequest--) |
| [`SlingHttpServletResponse`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/SlingHttpServletResponse.html) | [`getResponse()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getResponse--) |
| [`SlingScriptHelper`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/org/apache/sling/api/scripting/SlingScriptHelper.html) | [`getSlingScriptHelper()`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/cq/sightly/WCMUsePojo.html#getSlingScriptHelper--) |

### Getter Methods {#getter-methods}

Once the use-class has initialized, the HTL file is run. During this stage, HTL typically pulls in the state of various member variables of the use-class and renders them for presentation.

To provide access to these values from within the HTL file, you must define custom getter methods in the use-class according to the following naming convention:

* A method of the form `getXyz` exposes within the HTL file that an object property called `xyz`.

In the following example file `/apps/my-example/component/info/Info.java`, the methods `getTitle` and `getDescription` result in the object properties `title` and `description` becoming accessible within the context of the HTL file.

```java
...

public class Info extends WCMUsePojo {

    ...

    public String getLowerCaseTitle() {
        return lowerCaseTitle;
    }

    public String getLowerCaseDescription() {
        return lowerCaseDescription;
    }
}
```

### `data-sly-use` Attribute {#data-sly-use-attribute}

The `data-sly-use` attribute is used to initialize the use-class within your HTL code. In the example, the `data-sly-use` attribute declares that the class `Info` is used. You can use just the local name of the class because you are using a local install (having placed the Java source file in the same folder as the HTL file). If you were using a bundle install, you would have to specify the fully qualified class name.

Note the usage in this `/apps/my-example/component/info/info.html` example.

```xml
<div data-sly-use.info="Info">
    <h1>${info.lowerCaseTitle}</h1>
    <p>${info.lowerCaseDescription}</p>
</div>
```

### Local Identifier {#local-identifier}

The identifier `info` (after the dot in `data-sly-use.info`) is used within the HTL file to identify the class. The scope of this identifier is global within the file, after it has been declared. It is not limited to the element that contains the `data-sly-use` statement.

Note the usage in this `/apps/my-example/component/info/info.html` example.

```xml
<div data-sly-use.info="Info">
    <h1>${info.lowerCaseTitle}</h1>
    <p>${info.lowerCaseDescription}</p>
</div>
```

### Getting Properties {#getting-properties}

The identifier `info` is then used to access the object properties `title` and `description` that were exposed through the getter methods `Info.getTitle` and `Info.getDescription`.

Note the usage in this `/apps/my-example/component/info/info.html` example.

```xml
<div data-sly-use.info="Info">
    <h1>${info.lowerCaseTitle}</h1>
    <p>${info.lowerCaseDescription}</p>
</div>
```

### Output {#output}

Now, when `/content/my-example.html` is accessed, it returns the following `/content/my-example.html` file.

```xml
<div>
    <h1>my example</h1>
    <p>this is some example content.</p>
</div>
```

>[!NOTE]
>
>This example was simplified just to illustrate its use. In a production environment, Adobe recommends that you use [Sling models](https://sling.apache.org/documentation/bundles/models.html).

## Beyond the Basics {#beyond-the-basics}

This section introduces some additional features that go beyond the example described previously.

* Passing parameters to a use-class
* Bundled Java use-class

### Passing Parameters {#passing-parameters}

Parameters can be passed to a use-class upon initialization.

For details, please refer to the Sling [HTL Scripting Engine documentation](https://sling.apache.org/documentation/bundles/scripting/scripting-htl.html#passing-parameters-to-java-use-objects).

### Bundled Java Class {#bundled-java-class}

With a bundled use-class the class must be compiled, packaged, and deployed in AEM using the standard OSGi bundle deployment mechanism. In contrast with a local install, the use-class package declaration should be named normally as in this `/apps/my-example/component/info/Info.java` example.

```java
package org.example.app.components;

import com.adobe.cq.sightly.WCMUsePojo;

public class Info extends WCMUsePojo {
    ...
}
```

And the `data-sly-use` statement must reference the fully qualified class name, as opposed to just the local class name as in this `/apps/my-example/component/info/info.html` example.

```xml
<div data-sly-use.info="org.example.app.components.info.Info">
  <h1>${info.title}</h1>
  <p>${info.description}</p>
</div>
```
