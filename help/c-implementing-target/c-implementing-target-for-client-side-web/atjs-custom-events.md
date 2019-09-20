---
description: Information about custom events for at.js. 
keywords: custom events;at.js;request failed;request succeeded;content rendering failed;content rendering succeeded;library loaded;request start;content rendering start;content rendering no offers;content rendering rediret
seo-description: Information about custom events for the Adobe Target at.js JavaScript library.
seo-title: Information about custom events for the Adobe Target at.js JavaScript library.
solution: Target
subtopic: Getting Started
title: at.js custom events
topic: Standard
---

# at.js custom events

Information about `at.js custom events`, which lets you know when an mbox request or offer fails or succeeds.

Historically, mbox.js didn't let other JavaScript code that runs on the page know what happens behind the scenes. With the advancement of at.js, we had a unique opportunity to fix this issue.

According to our customers there are several scenarios that they would like to be notified of, including:

* An mbox request failed due to timeout, wrong status code, JSON parse error, etc. 
* An mbox request succeeded. 
* Offer rendering failed due to wrapping mbox element missing, selector can not be found, etc. 
* Offer rendering succeeded. DOM changes have been applied.

Pre-defined events have a structure that allows you to extract the required data, based on event type.

To make sure that events can be used in different scenarios, the custom events have a payload object that is assigned to the detail property of the event object (that is passed to the handler). Also to avoid passing strings as event names, the events are exposed as constants using `adobe.target.event` namespace.

## Structure {#section_0E5C9A13DE234A5DAECBCBF9F23F94FE}

| Key | Type | Description |
|--- |--- |--- |
|type|String|There are several scenarios in which you would like to be notified to help in tracing, debugging, and customizing interaction with at.js.<br>Each custom event listed below has two formats: a "constant" and a "string value."<ul><li>**Constants**: Prepended with `adobe.target.event.`, presented in all caps, and contain underscore characters. To subscribe to custom events *after* at.js loads but *before* the mbox response has been received, use the constant.</li><li>**String Values**: Lowercase and contain dashes. To subscribe to custom events *before* at.js loads, use the string value.</li></ul>**Request Failed**<br>Constant: `adobe.target.event.REQUEST_FAILED`<br>String value: `at-request-failed`<br>Description: An mbox request failed due to timeout, wrong status code, JSON parse error, etc.<br>**Request Succeeded**<br>Constant: `adobe.target.event.REQUEST_SUCCEEDED`<br>String Value: `at-request-succeeded`<br>Description: An mbox request was successful.<br>**Content Rendering Failed**<br>Constant: `adobe.target.event.CONTENT_RENDERING_FAILED`<br>String Value: `at-content-rendering-failed`<br>Description: Offer rendering failed due to wrapping mbox element missing, selector can not be found, etc.<br>**Content Rendering Succeeded**<br>Constant: `adobe.target.event.CONTENT_RENDERING_SUCCEEDED`<br>String Value: `at-content-rendering-succeeded`<br>Description: Offer rendering was successful. DOM changes have been applied.<br>**Library Loaded**<br>Constant: `adobe.target.event.LIBRARY_LOADED`<br>String Value: `at-library-loaded`<br>Description: This event is ideal to track when at.js has been fully loaded. You can use this event to customize global mbox execution. You can also use this event to disable the global mbox and then listen for this event to fire the global mbox later.<br>**Request Start**<br>Constant: `adobe.target.event.REQUEST_START`<br>String Value: `at-request-start`<br>Description: This event is fired before an HTTP request is executed. You can use this event for performance measurements using the Resource Timing API.<br>**Content Rendering Start**<br>Constant: `adobe.target.event.CONTENT_RENDERING_START`<br>String Value: `at-content-rendering-start`<br>Description: This event is fired before selector polling is started and content is rendered to the page. You can use this event to track the content rendering progress.<br>**Content Rendering No Offers**<br>Constant: `adobe.target.event.CONTENT_RENDERING_NO_OFFERS`<br>String Value: `at-content-rendering-no-offers`<br>Description: This event is fired when there are no offers returned.<br>**Content Rendering Redirect**<br>Constant: `adobe.target.event.CONTENT_RENDERING_REDIRECT`<br>String Value: `at-content-rendering-redirect`<br>Description: This event is fired when an offer is a redirect and Target will redirect to a different URL.|
|mbox|String|mbox name|
|message|String|Contains human-readable description, such as what happened, the error message, etc.|
|tracking|Object|Contains the `sessionId` and `deviceId`. In some cases, `deviceId` could be missing because [!DNL Target] couldn't retrieve it from edge server.|

## Usage {#section_0500FF09D3A04450B5DC8F85C6F793E0}

```
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(event) { 
  console.log('Event', event); 
});
```

## Training Video: Response Tokens and the at.js Custom Events {#section_ED304A7137DC42A4BDCD6D57C989F1FA}

Watch the following video to learn how to use Response Tokens and at.js Custom Events to share profile information from Target to third-party systems.

>[!VIDEO](https://video.tv.adobe.com/v/23253/)
