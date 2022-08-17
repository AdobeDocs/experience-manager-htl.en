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

It is recommended for new AEM projects to use the HTML Template Language, as it offers multiple benefits compared to JSP. For existing projects though, a migration only makes sense if it is estimated to be less effort than maintaining the existing JSPs for the coming years.

But moving to HTL is not necessarily an all-or-nothing choice, because components written in HTL are compatible with components written in JSP or ESP. Meaning that existing projects can without a problem use HTL for new components, while keeping JSP for existing components.

Even within the same component, HTL files can be used alongside JSPs and ESPs. Following example shows on **line 1** how to include an HTL file from a JSP file, and on **line 2** how a JSP file can be included from an HTL file:

```xml
<cq:include script="template.html"/>
<sly data-sly-include="template.jsp"/>
```

## Frequently Asked Questions {#frequently-asked-questions}

These are some quesitons commonly asked by exerpienced AEM developers new to HTL.

### Does HTL have any limitations that JSP doesn't? {#limitations}

HTL doesn't really have limitations compared to JSP in the sense that what can be done with JSP should also be achievable with HTL. However, HTL is by design stricter than JSP in several aspects, and what can be achieved all in a single JSP file, might need to be separated into a Java class or a JavaScript file to be achievable in HTL. But this is generally desired to ensure a good separation of concerns between the logic and the markup.

### Does HTL support JSP Tag Libraries? {#tag-libraries}

No, but as shown in the [Loading Client Libraries](#loading-client-libraries) section, the [template & call](block-statements.md#template-call) statements offer a similar pattern.

### Can the HTL features be extended on an AEM project? {#extended}

No, they cannot. HTL has powerful extension mechanisms for reuse of logic (the [Use-API](#use-api-for-accessing-logic)) and of markup (the [template & call](block-statements.md#template-call) statements), which can be used to modularize the code of projects.

### What are the main benefits of HTL over JSP? {#benefits}

Security and project efficiency are the main benefits, which are detailed on the [Overview.](overview.md)

### Will JSP eventually go away? {#go-away}

There are no plans along these lines.

## What's in a name? {#what-is-in-a-name}

In AEM 6.0 and 6.1, HTL was referred to as **Sightly**. Adobe renamed it to **HTML Template Language** or **HTL** to clarify what the specification is for and to align with Adobe’s naming guidelines in general. This naming change was effective as of August 2016 and applies to AEM version 6.0 and forward.

>[!NOTE]
>
>This naming change does not impact code or the API, therefore compatibility is not affected. For more information please [refer to this announcement video.](https://helpx.adobe.com/experience-manager/how-to/announce-htl.html)

To find out more about HTL and a great place to begin is our official [Getting Started with HTML Templating Language (HTL) Guide.](overview.md)
