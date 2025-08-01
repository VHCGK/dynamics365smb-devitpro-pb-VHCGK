---
title: Influence Copilot and agents without extending them 
description: Learn how to influence Copilot and agents without extending them in Business Central.
author: jswymer
ms.author: jswymer
ms.reviewer: jswymer
ms.topic: article
ms.collection: bap-ai-copilot
ms.date: 07/17/2025
ms.update-cycle: 180-days
ms.custom: bap-template
---

# Influence Copilot and agents without extending them

Many Business Central features are directly extensible using AL and offer documented mechanisms to modify or enhance their behavior and functionality. This article explains Copilot and agent capabilities that aren't directly extensible but can work with custom code and content from your extensions. All extensions installed in an environment can influence the output of Copilot and agents.

## Approach to extensibility

This table describes how each Copilot and agent capability offers direct or indirect extensibility.

|Capability|Direct AL extensibility|Can work with your extensions|Details|
|-|-|-|-|
|Analyze list|No |Yes |Copilot automatically works with fields you add to existing pages and new list or worksheet page objects in your extension. No action is required for Copilot to work with your extensions.|
|Autofill|No|Yes |Copilot can autofill fields you add to existing pages, and fields in new card or document page objects from your extension. No action is required for Copilot to work with your extensions. Autofill isn't available on setup pages where the [UsageCategory property](properties/devenv-usagecategory-property.md) is set to `Administration`.<br><br>Learn more about limitations in [Autofill availability](/dynamics365/business-central/autofill-fields-with-copilot#availability).|
|Chat|No |Yes|Copilot can query new pages and tables in your extension. It can include, display, or filter on fields you add to existing pages. No action is required for Copilot to work with your extensions.<br><br>Copilot doesn't support fields without a tooltip, fields with the AL property `AllowInCustomizations` set to `false`, and the following field types: <ul><li>Blob</li><li>RecordId</li><li>InStream</li><li>OutStream</li><li>Media</li><li> MediaSet</li><li>Notification</li><li>JsonArray</li><li>JsonObject</li><li>JsonToken</li><li>JsonValue</li><li>Variant</li><li>SecretText</li></ul>Copilot can provide guidance or answer questions based on documentation for your extensions. Learn more in [Chat with Copilot to learn how to use installed add-on apps](/dynamics365/release-plan/2025wave1/smb/dynamics365-business-central/chat-copilot-learn-use-installed-add-on-apps) and [Prepare your app Help for Copilot (preview)](ai-prepare-app-help-copilot.md).|
|Summarize|No |Yes |Copilot generates record summaries that can include data from:<ul><li>Visible fields on the page and fields on ListPart rows, including those you add to custom card or document pages, or fields you add by extending a page.</li><li>Visible FactBoxes including fields on custom FactBoxes or extensions to FactBoxes. Copilot only considers FactBoxes that don't have the [Provider property](properties/devenv-provider-property.md) defined.</li><li>Related statistics pages. Copilot only considers related pages linked to from an action on the page, where the action uses the [RunObject property](properties/devenv-runobject-property.md) and the page name includes the string "Statistics" or "Stats".</li></ul> No action is required for Copilot to work with your extensions, if your extension already includes any of the above.|
|Sales Order Agent|No |Yes|Similar to other users, the Sales Order Agent interacts with Business Central using the UI and reacts to the presence of new fields and actions that are made available on pages.<br><br>The agent uses an out of the box profile (role) called **Sales Order Agent (Copilot)** that defines the UI elements the agent can interact with. Using profiles reduces the UI surface that is available to users and agents alike. For example, if the **Purchase Orders** page isn't included with the profile, the agent can't navigate to it and can't modify the discounts on a sales order because these fields are hidden for this profile. If you want the agent to consider or interact with a field on the **Sales Order** page added by your extension, customize the **Sales Order Agent (Copilot)** profile so the field is visible to the agent.|
|Bank account reconciliation|No |Yes |When users ask Copilot to **Post Difference to G/L Account**, Copilot relies on data from the **Text-to-Account Mapping** table for a list of specific texts that don’t need to be matched using AI. You can add to this list to reduce cases processed by AI.|
|Create product information|No |No | |
|E-document matching assistance|No |No ||
|Marketing text suggestions|No |No | |
|No. Series Suggestions|No |Yes |Copilot can work with any No. Series setting fields added by your extension, if the field follows typical No. Series patterns. Copilot only looks for No. Series fields in Setup tables that include the term "Setup" in the table name. |
|Sales lines suggestions|No |Yes |When users choose to keep any sales lines suggested by Copilot, the lines are copied to the sales document, where Business Central automatically triggers field validation and the Copy Document Codeunit. If your extension customizes validation or this codeunit using events, your logic is executed to validate the inserted sales lines. |

## Improve Copilot and agent output for your extensions

In most cases, Copilot and agents use generative AI to view, learn, and use Business Central like any user. These tasks rely on the quality of the text displayed in Business Central captions, notifications, and messages, and the quality of your online documentation to explain new concepts and business processes introduced in your Business Central extensions.

By improving how your functionality presents itself, you improve the experience both for end users and for these AI-powered features.

Here are some practical examples to improve the quality of Copilot output:

- Ensure your captions, tooltips, and teaching tips are concise, informative, and unambiguous. Explain critical nuances such as any special meaning of a default value or the purpose of a page.

  Learn more in [Add tooltips to table and page fields](devenv-adding-tooltips.md) and [Teaching tips and in-app tours for onboarding users](../administration/onboarding-teaching-tips-tours.md).
- Define alternate keywords for your pages and reports that represent common or industry-specific terms.

  Learn more in [AdditionalSearchTerms](properties/devenv-additionalsearchterms-property.md).
- Use role-tailoring and other standard Business Central mechanisms to reduce unnecessary page content.

  Learn more in [Customize the user interface for user roles](devenv-role-customization.md).

- Define an appropriate brick field group for your tables to ensure records are represented in a compact form.

  Learn more in [Display lists as tiles using brick field group](devenv-lists-as-tiles.md).
- Apply access control using permissions so users, Copilot, and agents have a smaller data landscape to work with and only the minimum necessary to carry out their tasks.

  Learn more in [Entitlements and permission sets overview](devenv-entitlements-and-permissionsets-overview.md).

> [!NOTE]
>
> - Microsoft provides multiple layers of safety for all AI-powered features in Business Central. Some of these safeguards are designed to protect users from harmful content that can be output by generative AI features and might trigger in sensitive business domains such as medical, drugs, legal, or weapons. Customers might experience lower quality output in these domains.
> - You can't extend a Prompt Dialog page When you use the developer tools for Copilot in Business Central. This behavior reduces the chances of others overriding or negatively impacting the AI safeguards built into your experience.

## Related information

[Integrating AI using Developer Tools for Copilot](ai-integration-landing-page.yml)  
[Developing generative AI experiences overview](ai-build-experience-overview.md)  
[Build Copilot user experience](ai-build-experience.md)  
