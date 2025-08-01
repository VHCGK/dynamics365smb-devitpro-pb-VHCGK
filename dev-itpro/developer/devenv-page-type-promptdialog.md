---
title: The PromptDialog Page Type
description: The PromptDialog page type allows you to integrate copilot capabilities into your custom scenarios.
author: SusanneWindfeldPedersen
ms.author: solsen
ms.topic: overview
ms.collection:
  - bap-ai-copilot
ms.date: 06/19/2025
ms.update-cycle: 180-days
ms.reviewer: solsen
---

# The PromptDialog page type 

The PromptDialog page type is a specialized page type introduced in [!INCLUDE [prod_short](includes/prod_short.md)] runtime 12.1. It enables developers to integrate generative AI capabilities into their custom scenarios, providing a seamless Copilot experience with signature visuals and built-in safety controls. This page type is designed to facilitate user interaction with AI-driven features, offering structured input and output areas, predefined prompt guides, and system actions tailored for Copilot functionality. In this article, you'll learn about the properties, layout areas, actions, and implementation details of the PromptDialog page type, along with a comprehensive example to help you get started.

## Snippet support

Type the shortcut `tpage` and then choose the **Page of type Prompt Dialog**, which creates the basic layout for a `PromptDialog` page object when using the [!INCLUDE[d365al_ext_md](../includes/d365al_ext_md.md)] in Visual Studio Code.

## Properties of the PromptDialog page type

The `PromptDialog` page type has some specific properties that characterize the dialog and must be set for this specific experience.

|Property|Description|
|--------|-----------|
|`PageType` | The `PageType` property must be set to `PromptDialog` for this specific page type.|
|`PromptMode` |The `PromptMode` property value is by default `Prompt`, which is the starting prompt mode. The `PromptMode` property can be changed at runtime. The other options are; <br> - `Generate`, which triggers generating the output of the copilot interaction, and <br>- `Content`, which shows the output of the copilot interaction.<br> You can programmatically set this property by setting the variable `CurrPage.PromptMode` before the page is opened.|
|`IsPreview` | The `IsPreview` property adds a specific note in the UI to indicate that the feature is in preview. It's by default set to `false`.|
|`Image`| To identify and ensure consistency in the UI for a generative AI experience, the `Image` property for the action invoking it, should be set to `Sparkle`. If there are multiple copilot options to choose from in the UI, the `Image` property can be set to `Image = SparkleFilled;` to make a specific action more prominent. To view a list of images, see [Available icons](https://aka.ms/bcicons). |

To find links to the properties related to the `PromptDialog` page type, see the [Related information](devenv-page-type-promptdialog.md#related-information) section in this article.

## Areas of the PromptDialog page type

The `PromptDialog` page type has the following layout area types that characterize the dialog. To enable the full copilot experience, you must use the `Prompt`, `Content`, and `PromptOptions` areas. The page must have a `area(Prompt)` with one or more controls that accept user input to have the `PromptDialog` page start with a prompt.)

|Area |Description|
|-----|-----------|
|`Prompt` | The `Prompt` area is the input to copilot, and accepts any control, except repeater controls.|
|`Content` | The `Content` area is the output of copilot, and accepts any control, except repeater controls.|
|`PromptOptions` | The `PromptOptions` area is the input options, and only accepts option fields.|

### Actions in the PromptDialog page

Unlike other page types, `PromptDialog` pages can only specify two action areas; `SystemActions` and `PromptGuide`. The `SystemActions` area defines the Copilot experience with options for generating and accepting generated content. The `PromptGuide` adds help to the user by providing predefined text prompt "guides" that users can select to use as input to generate content, rather than having to write that up themselves. The `PromptGuide` menu is only rendered in the web client when the `PromptMode` of the `PromptDialog` page is set to `Prompt`.

|Action|Description|
|------|-----------|
|`SystemActions` | The `SystemActions` area only allows you to define a fixed set of actions called system actions, which are only supported by this page type. These system actions are `Generate`, `Regenerate`, `Attach`, `Ok` and `Cancel`.|
|`PromptGuide` | The `PromptGuide` action area represents a list of predefined text prompt "guides", which users can select to use as input to generate content, rather than creating their own prompt from scratch. The prompt guide menu is only rendered in the web client when the `PromptMode` of the `PromptDialog` page is set to `Prompt`.|

The following illustration shows an example of how the `PromptDialog` page with the `Prompt`, `Content`, `SystemActions`, and `PromptGuide` areas is implemented in [!INCLUDE [prod_short](includes/prod_short.md)] for the **Analyze Bank Account Reconciliation** prompt dialog. You identify the prompt guide by the ![Prompt guide icon that opens a prompt guide.](media/prompt-guide-icon.png "The prompt guide icon") icon of a prompt dialog page.

:::image type="content" source="media/promptdialog-analyze-bank.png" alt-text="Analyze bank account prompt dialog":::

## Example

The following example describes a page, which is a PromptDialog page, set with the `PromptDialog` option. The `Extensible = false;` is a mandatory setting, to ensure that the page isn't extended so that customers can trust the AI experience implemented.

Use the `IsPreview` property to indicate to your customers that you're using the feature in preview, and that the feature might change in the future as you gather feedback. The `IsPreview` property adds a specific note in the UI to indicate that the feature is in preview. It's by default set to `false`. 

The page is divided into two main areas: `Prompt` and `Content`. 

- In the `Prompt` area, the field `ProjectDescriptionField` is bound to the variable `InputProjectDescription` and is a multiline text field where the user can describe the project they want to create with Copilot.

- The `Content` area contains fields that display the job's short description, full details, and the customer's name. The `CustomerNameField` has two triggers: `OnAssistEdit` and `OnValidate`. The `OnAssistEdit` trigger is used to select a customer from the existing customer records. The `OnValidate` trigger is used to validate the customer name and number. There's also a part named `ProposalDetails`, which is used to display the structure of the job proposal.

The `actions` section defines actions that the user can perform on this page. There are several actions defined under the `PromptGuide` area, such as `OrganizeCampaign`, `FurnishOffice`, `SetUpConferenceRooms`, and `OrganizeWorkshop`. Each action sets the `InputProjectDescription` to a predefined text. 

The `SystemActions` area contains system actions like `Generate`, `OK`, `Cancel`, and `Regenerate`. These actions generate the project structure, save the project, discard the project, and regenerate the project respectively.

The `OnQueryClosePage` trigger saves the job proposal when the page is closed with the `OK` action.

The `RunGeneration` procedure generates the job proposal based on the user's input. It uses the `Generate Job Proposal` codeunit to generate the proposal and updates the fields in the `Content` area with the generated data.

The `FindCustomerNameAndNumber` procedure finds a customer based on the input customer name. It sets a filter on the `Search Name` field of the `Customer` record and tries to find a customer that matches the input name. If a customer is found, it updates the `CustomerName` and `CustomerNo` variables with the customer's name and number.

```al
page 54320 "Copilot Job Proposal"
{
    PageType = PromptDialog;
    Extensible = false;
    Caption = 'Draft new project with Copilot';
    DataCaptionExpression = InputProjectDescription;
    IsPreview = true;

    layout
    {
        // This is the input section that accepts user input to generate content

        area(Prompt) 
        {
            field(ProjectDescriptionField; InputProjectDescription)
            {
                ApplicationArea = All;
                ShowCaption = false;
                MultiLine = true;
                InstructionalText = 'Describe the project you want to create with Copilot';
            }
        }
        
        // This is the output section that displays the generated content

        area(Content) 
        {
            field("Job Short Description"; JobDescription)
            {
                ApplicationArea = All;
                Caption = 'Project Short Description';
            }
            field("Job Full Details"; JobFullDescription)
            {
                ApplicationArea = All;
                MultiLine = true;
                Caption = 'Details';
            }
            field(CustomerNameField; CustomerName)
            {
                ApplicationArea = All;
                Caption = 'Customer Name';
                ShowMandatory = true;

                trigger OnAssistEdit()
                var
                    Customer: Record Customer;
                begin
                    if not Customer.SelectCustomer(Customer) then
                        Clear(Customer);

                    CustomerName := Customer.Name;
                    CustomerNo := Customer."No.";
                end;

                trigger OnValidate()
                begin
                    FindCustomerNameAndNumber(CustomerName, false);
                end;
            }
            part(ProposalDetails; "Copilot Job Proposal Subpart")
            {
                Caption = 'Job structure';
                ShowFilter = false;
                ApplicationArea = All;
                Editable = true;
                Enabled = true;
            }
        }
    }
    actions
    {
        // This is the prompt guide area which contains the predefined prompts that the user can select to use as input to generate content

        area(PromptGuide)
        {
            action(OrganizeCampaign)
            {
                ApplicationArea = All;
                Caption = 'Create a campaign';

                trigger OnAction()
                begin
                    InputProjectDescription := 'Campaign on [social media] for [Customer] to [promote education].';
                end;
            }

            // Group of actions

            group(Furnishing)
            {
                action(FurnishOffice)
                {
                    ApplicationArea = All;
                    Caption = 'Furnish an office';

                    trigger OnAction()
                    begin
                        InputProjectDescription := '[Customer] needs to furnish [office] for [4 people].';
                    end;
                }

                action(SetUpConferenceRooms)
                {
                    ApplicationArea = All;
                    Caption = 'Set up work areas';

                    trigger OnAction()
                    begin
                        InputProjectDescription := 'Design and set up [work areas] for [Customer].';
                    end;
                }
            }

            action(OrganizeWorkshop)
            {
                ApplicationArea = All;
                Caption = 'Organize a workshop';

                trigger OnAction()
                begin
                    InputProjectDescription := 'Organize a [workshop] for [Customer] about [sustainability].';
                end;
            }

        }

        // This is the system actions area which contains the options for generating and accepting generated content

        area(SystemActions)
        {
            systemaction(Generate)
            {
                Caption = 'Generate';
                ToolTip = 'Generate project structure with Dynamics 365 Copilot.';

                trigger OnAction()
                begin
                    RunGeneration();
                end;
            }
            systemaction(OK)
            {
                Caption = 'Keep it';
                ToolTip = 'Save the Project proposed by Dynamics 365 Copilot.';
            }
            systemaction(Cancel)
            {
                Caption = 'Discard it';
                ToolTip = 'Discard the Project proposed by Dynamics 365 Copilot.';
            }
            systemaction(Regenerate)
            {
                Caption = 'Regenerate';
                ToolTip = 'Regenerate the Project proposed by Dynamics 365 Copilot.';

                trigger OnAction()
                begin
                    RunGeneration();
                end;
            }
        }
    }

    trigger OnQueryClosePage(CloseAction: Action): Boolean
    var
        SaveCopilotJobProposal: Codeunit "Save Copilot Job Proposal";
    begin
        if CloseAction = CloseAction::OK then
            SaveCopilotJobProposal.Save(CustomerNo, CopilotJobProposal);
    end;

    // The code triggering the copilot interaction. This is where you call the Copilot API, and get the results back. 

    local procedure RunGeneration()
    var
        GenJobProposal: Codeunit "Generate Job Proposal";
        InStr: InStream;
        ProgressDialog: Dialog;
        Attempts: Integer;
    begin
        ProgressDialog.Open(GeneratingTextDialogTxt);
        GenJobProposal.SetUserPrompt(InputProjectDescription);

        CopilotJobProposal.Reset();
        CopilotJobProposal.DeleteAll();

        for Attempts := 0 to 3 do
            if GenJobProposal.Run() then begin
                GenJobProposal.GetResult(CopilotJobProposal);

                if not CopilotJobProposal.IsEmpty() then begin
                    CopilotJobProposal.FindFirst();

                    JobDescription := CopilotJobProposal."Job Short Description";
                    CopilotJobProposal.CalcFields("Job Full Description");
                    CopilotJobProposal."Job Full Description".CreateInStream(InStr);
                    InStr.ReadText(JobFullDescription);

                    FindCustomerNameAndNumber(CopilotJobProposal."Job Customer Name", true);

                    CurrPage.ProposalDetails.Page.Load(CopilotJobProposal);
                    exit;
                end;
            end;

        if GetLastErrorText() = '' then
            Error(SomethingWentWrongErr)
        else
            Error(SomethingWentWrongWithLatestErr, GetLastErrorText());
    end;

    local procedure FindCustomerNameAndNumber(InputCustomerName: Text; Silent: Boolean)
    var
        Customer: Record Customer;
    begin
        if InputCustomerName <> '' then begin
            Customer.SetFilter("Search Name", '%1', '*' + UpperCase(InputCustomerName) + '*');

            if not Customer.FindFirst() then
                if not Silent then
                    Error(CustomerDoesNotExistErr);
        end;

        CustomerName := Customer.Name;
        CustomerNo := Customer."No.";
    end;

    var
        GeneratingTextDialogTxt: Label 'Generating with Copilot...';
        SomethingWentWrongErr: Label 'Something went wrong. Please try again.';
        SomethingWentWrongWithLatestErr: Label 'Something went wrong. Please try again. The latest error is: %1';
        CustomerDoesNotExistErr: Label 'Customer does not exist';
        CustomerNo: Code[20];
        CopilotJobProposal: Record "Copilot Job Proposal" temporary;
        JobFullDescription: Text;
        InputProjectDescription: Text;
        JobDescription: Text[100];
        CustomerName: Text[100];
}
```

To see the `Copilot Job Proposal` code example in a complete implementation, go to [aka.ms/BCTech](https://github.com/microsoft/BCTech/blob/master/samples/AzureOpenAI/Advanced_SuggestJob/DescribeJob/CopilotJobProposal.Page.al). Learn more about building an AI capability in [Build the copilot capability in AL](ai-build-capability-in-al.md).

Learn more about error handling in [Error handling in prompt dialogs](devenv-page-prompt-error-handling.md).

## Nudging users with prompt actions

With the floating action bar, you can create prompt actions to promote AI capabilities in [!INCLUDE [prod_short](includes/prod_short.md)]. A prompt action is a standard action that is rendered in a floating action bar on your pages, and it nudges users to use relevant Copilot built-in features. Learn more in [Prompting using a floating action bar](devenv-page-prompting-floating-actionbar.md).

## Related information

[Page types and layouts](devenv-page-types-and-layouts.md)  
[Page object](devenv-page-object.md)  
[PageType property](properties/devenv-pagetype-property.md)  
[PromptMode property](properties/devenv-promptmode-property.md)  
[Image property](properties/devenv-image-property.md)  
[IsPreview property](properties/devenv-ispreview-property.md)  
[SourceTable property](properties/devenv-sourcetable-property.md)  
[SourceTableTemporary property](properties/devenv-sourcetabletemporary-property.md)  
[Extensible property](properties/devenv-extensible-property.md)  
[Prompting using a floating action bar](devenv-page-prompting-floating-actionbar.md)  
[Error handling in prompt dialogs](devenv-page-prompt-error-handling.md)