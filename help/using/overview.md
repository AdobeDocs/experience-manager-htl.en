---
title: HTL overview
description: Discover how AEM supports HTL (HTML Template Language) to provide a productive enterprise-level web framework that enhances security. This framework lets HTML developers without Java knowledge to participate better in AEM Projects.
exl-id: 5d06ff25-d681-4b95-8375-c28a8364eb7e
---

# Overview {#overview}

>[!TIP]
>
>**Have you considered Edge Delivery Services for AEM?**
>
>You can continue using the methods described in this document for existing projects. However, for new projects, Adobe recommends leveraging [Edge Delivery Services.](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/overview)

HTML Template Language (HTL), supported by Adobe Experience Manager (AEM), aims to provide a highly productive enterprise-level web framework that enhances security. It also enables HTML developers without Java knowledge to participate better in AEM Projects.

[Introduced in AEM 6.0](history.md), the HTML Template Language is the preferred and recommended server-side template system for HTML in AEM. For web developers who need to build robust enterprise websites, the HTML Template Language helps to achieve increased security and development efficiency. 

## Increased Security {#increased-security}

HTML Template Language (HTL) enhances site security by automatically applying context-aware escaping to all output variables, making it more secure than most other template systems. HTL makes this approach possible because it understands the HTML syntax, and uses that knowledge to adjust the required escaping for expressions, based on their position in the markup. This method can result in expressions placed in `href` or `src` attributes to be escaped differently from expressions placed in other attributes, or elsewhere.

While the same result can be achieved with template languages like JSP, there the developer must manually ensure that the proper escaping is applied to each variable. As a single omission or mistake in the applied escaping is potentially sufficient to cause a cross-site scripting (XSS) vulnerability, Adobe decided to automate this task with HTL. If needed, developers can still specify a different escaping on the expressions, but with HTL the default behavior is much more likely to correspond to the desired behavior, reducing the likelihood of errors.

## Simplified Development {#simplified-development}

The HTML Template Language is easy to learn and its features are purposely limited to ensure that it stays simple and straight-forward. It also has powerful mechanisms for structuring the markup and invoking logic, while always enforcing strict separation of concerns between markup and logic. HTL is standard HTML5, using expressions and data attributes to annotate the markup with dynamic behavior. This approach maintains the validity and readability of the markup. The evaluation of the expressions and data attributes is done entirely server-side and is not visible on the client-side, where any desired JavaScript framework can be used without interfering.

These capabilities let HTML developers without Java knowledge to edit HTL templates, integrate into the development team, and streamline collaboration with full-stack Java developers. And vice versa, it lets Java developers to focus on the back-end code without worrying about HTML.

## Reduced Costs {#reduced-costs}

Increased security, simplified development, and improved team collaboration, translates for AEM Projects in reduced effort, faster time to market (TTM), and lower total cost of ownership (TCO).

Re-implementing the Adobe.com site with HTML Template Language has shown that project costs and duration are reduced up to approximately 25%.

![Efficiently increase and cost decrease](assets/chlimage_1.png)

The diagram above shows the following improvements in efficiency potentially made possible by HTL:

* **HTML / CSS / JS:** HTML developers can directly edit HTL templates, allowing front-end designs to be implemented directly on AEM components, eliminating the need for separate implementation. This approach reduces painful iterations with the full-stack Java developers.
* **JSP / HTL:** Because HTL itself does not require any Java knowledge and is straight-forward to write, any developer with HTML expertise is empowered to edit the templates.
* **Java:** Thanks to the clear and simple to use Use-API provided by HTL, the interface with the business logic is clarified, which also benefits Java development overall.

## Video Introduction {#video}

The following video from an [AEM Gems session](https://experienceleague.adobe.com/en/docs/events/experience-manager-gems-recordings/gems2014/aem-introduction-to-htl), gives an overview of the purpose of HTL as well as implementation examples.

>[!VIDEO](https://video.tv.adobe.com/v/19504/?quality=9)

Please note that the video refers to HTL by [its former name, Sightly](history.md).

## Next Steps {#next-steps}

Now that you know the objectives and advantages of HTL, you can get started with the language. See [Getting Started with the HTML Template Language](getting-started.md).
