---
title: History of HTL
description: For long time users of AEM, this document gives the background on HTL, how it replaces JSP, and the change of name from Sightly.
exl-id: 00985b35-2130-4946-959a-0a09a34a0f05
---

# History of HTL {#history-of-htl}

For long time users of AEM, this document gives the background on HTL, how it replaces JSP, and the change of name from Sightly.

## Formerly Known As Sightly {#sightly}

HTML Template Language (HTL) is the preferred and recommended server-side template system for HTML in Adobe Experience Manager. It takes the place of JSP (JavaServer Pages) as used in previous versions of AEM.

## HTL over JSP {#htl-over-jsp}

Adobe recommends that for new AEM Projects, you use the HTML Template Language. The reason is because it offers multiple benefits compared to JSP. For existing projects though, a migration only makes sense if it is estimated to be less effort than maintaining the existing JSPs for the coming years.

Moving to HTL is not necessarily an all-or-nothing choice, because components written in HTL are compatible with components written in JSP or ESP. This approach means that existing projects can use HTL for new components without a problem, while keeping JSP for existing components.

Even within the same component, HTL files can be used alongside JSPs and ESPs. The following example shows on **line 1** how to include an HTL file from a JSP file, and on **line 2** how a JSP file can be included from an HTL file:

```xml
<cq:include script="template.html"/>
<sly data-sly-include="template.jsp"/>
```

## Frequently Asked Questions {#frequently-asked-questions}

Experienced AEM developers who are new to HTL, commonly ask the following questions:

### Does HTL have any limitations that JSP doesn't? {#limitations}

HTL does not have limitations compared to JSP in the sense that what can be done with JSP should also be achievable with HTL. However, HTL is by design stricter than JSP in several aspects. What can be achieved in a single JSP file might need to be separated into a Java class or a JavaScript file to be achievable in HTL. But this approach is generally desired to ensure a good separation of concerns between the logic and the markup.

### Does HTL support JSP Tag Libraries? {#tag-libraries}

No. However, as shown in the [Loading Client Libraries](getting-started.md#loading-client-libraries) section of the Getting Started document, the template &amp; call statements offer a similar pattern.

### Can the HTL features be extended on an AEM project? {#extended}

No. HTL has powerful extension mechanisms for reuse of logic (the [Use-API](#use-api-for-accessing-logic)) and of markup (the template &amp; call statements), which can be used to modularize the code of projects.

### What are the main benefits of HTL over JSP? {#benefits}

Security and project efficiency are the main benefits, which are detailed in the [Overview](overview.md).

### Are JavaServer Pages (JSP) going away? {#go-away}

No. There are no plans to discontinue JSP.

## What's in a name? {#what-is-in-a-name}

In AEM 6.0 and 6.1, HTL was called **Sightly**. Adobe renamed it to **HTML Template Language** or **HTL** to clarify what the specification is for and to align with Adobe's naming guidelines in general. This naming change was effective as of August 2016 and applies to AEM version 6.0 and forward.

>[!NOTE]
>
>This naming change does not impact the code or the API, therefore compatibility is not affected. 

<!-- LINK IS 404
For more information, watch [this announcement video](https://helpx.adobe.com/experience-manager/how-to/announce-htl.html). -->

To find out more about HTL, see [Getting Started with HTML Templating Language (HTL) Guide](overview.md).
