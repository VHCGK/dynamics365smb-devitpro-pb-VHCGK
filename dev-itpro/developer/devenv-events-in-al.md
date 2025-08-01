---
title: Events in Microsoft Dynamics 365 Business Central
description: Events are a programming concept that can ease application upgrade and limit the code modifications in customized applications during platform changes. 
author: jswymer
ms.author: solsen
ms.date: 04/24/2025
ms.update-cycle: 1095-days
ms.topic: article
ms.reviewer: solsen
ms.custom: evergreen
---

# Events in AL

The use of events is a proven and established programming concept that can ease application upgrade and limit or even eliminate the need for code modifications in customized applications because of application platform changes.  

You can use events to design the application to react to specific actions or behavior that occur. Events enable you to separate customized functionality from the application business logic. By using events in the application where customizations are typically made, you can lower the cost of code modifications and upgrades to the original application.  

- Code modifications to customized functionality can be made without having to modify the original application.  
- Changes to the original application code can be made with minimal impact on the customizations.  

Events can be used for different purposes, such as generating notifications when certain behavior occurs or the state of an entity changes, distributing information, and integrating with external systems and applications. For example, in the [!INCLUDE[demolong](includes/demolong_md.md)], events are used extensively for workflow and Dynamics 365 for Sales integration.

The following table describes all the different event types:  

|Event types | Description |
|------------|-------------|
|[BusinessEvent](attributes/devenv-businessevent-attribute.md) |Specifies the method to be business type event publisher.  |
|[IntegrationEvent](attributes/devenv-integrationevent-attribute.md) |Specifies the method to be integration type event publisher. |
|[InternalEvent](attributes/devenv-internalevent-attribute.md) |Specifies the method to be an internal event publisher.|
|[Global](devenv-event-types.md#global-events) |Global events are predefined system events. |
|[Trigger](devenv-event-types.md#trigger-events) |Trigger events are published by the runtime.|

The process for implementing these events is slightly different. To learn about the different types, see [Event types](devenv-event-types.md).

> [!TIP]
> Business Central supports external business events (preview), defined with the attribute `ExternalBusinessEvent`. These events notify external systems when high-level business processes occur and behave differently from other event types. Learn more in [Business events on Business Central (preview)](business-events-overview.md).  

## How events work

The basic principle is that you program events in the application to run customized behavior when they occur. Events in AL are modeled after Microsoft .NET Framework. There are three major participants involved in events: the *event*, a *publisher*, and a *subscriber*.  

- An *event* is the declaration of the occurrence or change in the application. An event is declared by an AL method, which is referred to as an *event publisher function*. An event publisher method is composed of a signature only and doesn't execute any code.
- A *publisher* is the object that contains the event publisher method that declares the event. The publisher exposes an event in the application to subscribers, essentially providing subscribers with a hook-up point in the application.  

    Publishing an event doesn't actually do anything in the application apart from making the event available for subscription. The event must be raised for subscribers to respond. An event is raised by adding logic to the application that calls into the publisher to invoke the event (the event publisher method).  

    Partners or subsystems can then take advantage of the published event in their solutions. An ISV that delivers vertical solutions, and Microsoft itself, are the typical providers of published events.  

    Business and integration type events must be explicitly published and raised, which means that you must create event publisher functions and add them to objects manually. On the other hand, trigger events, which occur on table and page operations, are published and raised implicitly by the system at runtime. Therefore, no coding is required to publish them.  

- A *subscriber* listens for and handles a published event. A subscriber is an AL method that subscribes to a specific event publisher method and includes the logic for handling the event. When an event is raised, the subscriber method is called and its code is run. A subscriber enables partners to hook into the core application functionality without having to do traditional code modifications. Any [!INCLUDE [prod_short](includes/prod_short.md)] solution provider, which also includes Microsoft, can use event subscribers.  

There can by multiple subscribers to a single event publisher method. However, a publisher has no knowledge of subscribers, if any. Subscribers can reside in different parts of the application than publishers.  

## How to implement events

Implementing events consists of the following tasks:  

1. Publish the event.  

    For business and integration events, create and configure a method in an application object to be an event publisher method. Learn more in [Publishing events](devenv-publishing-events.md). This task isn't required for trigger events because they're automatically published by the system.

2. Raise the event.  

    Add code that calls the event publisher method. Learn more in [Raising events](devenv-raising-events.md). This task isn't required for trigger events because they're raised automatically by the system.

3. Subscribe to the event.  

    At the consumer end, add one or more subscriber methods that subscribe to published events when they're raised. Learn more in [Subscribing to events](devenv-subscribing-to-events.md).  


## Related information

[Publishing Events](devenv-publishing-events.md)  
[Raising Events](devenv-raising-events.md)  
[Subscribing to Events](devenv-subscribing-to-events.md)  
[Isolated Events](devenv-events-isolated.md)  
[Developing Extensions Using the New Development Environment](devenv-dev-overview.md)  

<!--NAV
[Debugging Events](devenv-debugging-events.md)  
[Best Practices with Microsoft Dynamics 365 Business Central](devenv-events-best-practices.md)  
 [Walkthrough: Publishing, Raising, and Subcribing to an Event in Microsoft Dynamics NAV](Walkthrough--Publishing--Raising--and-Subcribing-to-an-Event-in-Microsoft-Dynamics-NAV.md)  
[Walkthrough: Implementing New Workflow Events and Responses](Walkthrough--Implementing-New-Workflow-Events-and-Responses.md)  -->
