---
title: HTL Use-API
description: HTL encourages separation of concerns by not allowing business logic to mix with markup and instead implementing such logic through the Use-API. 
exl-id: 8f95707e-952c-4efe-a44e-9a1ad501605e
---

# HTL Use-API {#htl-use-api}

HTL encourages separation of concerns by not allowing business logic to mix with markup. Business logic can be implemented through the Use-API. 

The following table gives an overview of the advantages and disadvantages of each API.

||**Java Use-API**|**JavaScript Use-API**|
|--- |--- |--- |
|**Advantages**|<ul><li>Faster</li><li>Can be inspected with a debugger</li><li>Easy to unit-test</li></ul>|<ul><li>Can be modified by front-end developers</li><li>Is located within the component, keeping the view logic of a component close to its corresponding template</li></ul>|
|**Disadvantages**|<ul><li>Cannot be modified by front-end developers</li></ul>|<ul><li>Slower</li><li>No debugger (yet)</li><li>Harder to unit-test</li></ul>|

For page components, it is recommended to use a mixed model, where all model logic is located in Java, providing clear APIs that are agnostic to anything that happens in the view (i.e. within the components). AEM comes with great default models like the Page or the Resource API that should be able to cover most cases.

All view logic that is specific to a component should be placed within that component as JavaScript, because it belongs to that component.

## Additional Resources {#additional-resources}

* [HTL Java-Use API](use-api-java.md) - The HTL Java Use-API enables a HTL file to access helper methods in a custom Java class.
* [HTL JavaScript-Use API](use-api-javascript.md) - Learn how the HTML template language (HTL) JavaScript use-API enables a HTL file to access helper code written in JavaScript.
