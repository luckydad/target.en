---
description: Overview information about designs that define how recommendations appear on a page.
keywords: recommendations design;template;create design;delivery;output
seo-description: Overview information about designs that define how recommendations appear on a page.
seo-title: Design overview
solution: Target
title: Design overview
topic: Premium
uuid: 82cc6a19-bfde-47b3-92b9-b862be70dd87
---

# Design overview{#design-overview}

Overview information about designs that define how recommendations appear on a page.

Target can deliver the complete look and feel of your recommendations as shown in the following illustration. The design can include HTML, JavaScript, and CSS.

![](assets/velocity_example.png)

Target can also send your recommendations as JSON objects that can be used in email messages, IoT (Internet of Things) devices, console, or voice use cases (Amazon Alexa or Google Home).

## JSON Example {#section_75BFB2537CFF4FBD9B560F59EB32C8DD}

The following example shows how to JSON responses can be returned when configuring an activity via the form-based editor.

1. Create a design from within Design Library or within the form-based workflow. If you attempt to do this inside the Visual Experience Composer (VEC) workflow you cannot create anything other than an HTML design, which is wrapped in a `<div>` for click tracking purposes. 
1. Ensure that the "HTML Design" option is turned off:

   ![](assets/html_design_toggle.png)

1. The following code is an example below of what you could paste into your design:

   ```
       #* 
       * "Return a simple list of recommended entity ids"   
       *#

       {   
         "notes":{   
         "purpose": "Return a simple list of recommended entity ids",   
         "use-case": "Use this approach if you prefer to do a real-time lookup of entity attribute details (such as inventory, price, rating) from another system (such as a CMS, PIM or ecommerce platform)",   
         "version": "01"   
         },   
         "recommendedItems": {   
           "key": "$key.id",   
           "slot-01": "$entity1.id",   
           "slot-02": "$entity2.id",   
           "slot-03": "$entity3.id",   
           "slot-04": "$entity4.id",   
           "slot-05": "$entity5.id",   
           "slot-06": "$entity6.id",   
           "slot-07": "$entity7.id",   
           "slot-08": "$entity8.id",   
           "slot-09": "$entity9.id",   
           "slot-10": "$entity10.id"   
         }   
       }  
   ```

1. Set up a form-based Recommendations activity that uses this design.

    1. Navigate to the Activities page. 
    1. Click **[!UICONTROL Create Activity]**. 
    1. Select **[!UICONTROL Recommendations]**. 
    1. Under **[!UICONTROL Choose Experience Composer]**, select **[!UICONTROL Form]**. 
    
    1. Under location, enter the text: "Sample_Recs_Response" 
    1. Under **[!UICONTROL Default Content]**, click the down-arrow, then click **[!UICONTROL Add Recommendation]**. 
    1. Choose a Page Type. This determines the initial filtering of the next screen. 
    1. Select a Criteria card, then click **[!UICONTROL Next]**. 
    1. Select the design you created in the previous step, then click **[!UICONTROL Save]**. 
    1. Complete the setup process. 
    1. Click the right arrow next to **[!UICONTROL Inactive]**, then select **[!UICONTROL Activate]**.

1. After your activity is set up and activated, you can set up a sample request to get back the clean JSON response.

   From the time that you save your activity, Target will need to build a model to support the selected criteria configuration. Depending on a number of factors, this could take some time. Results appear once the model has build.

   For example:

   ```
   https://[YOUR_CLIENT_CODE].tt.omtrdc.net/m2/YOUR_CLIENT_CODE/ubox/raw?mbox=[YOUR_MBOX_NAME]&mboxContentType=text/html&mboxXDomain=disabled&entity.id=[ENTITY_ID]&mboxHost=rawbox_sample&at_property=[AT_PROPERTY_TOKEN]&mboxNoRedirect=true&mboxPC=1234-4321&mboxSession=9876-7000
   ```

   where

| Parameter | Value |
|--- |--- |
|`[YOUR_CLIENT_CODE]`|Target Client code (available on  ../target/products.html#recsSettings  > Recommendations API Token > Client Code.|
|`[YOUR_MBOX_NAME]`|The name you've selected in the "locations" section of the form-based Recommendations, in this case Sample_Recs_Response.|
|`[ENTITY_ID`]|The `entity.id` of an item in your catalog.|
|`[AT_PROPERTY_TOKEN]`|(Optional) Add if you've selected a Property (part of Enterprise Permissions) during your activity setup.|

After your algorithm has run, and you have results, your response should look something like this:

![](assets/json_recommendation.png){width="575px"}

## Additional JSON Object Tips and Tricks {#section_C305673C68944749969DB239E3221DC2}

You can also just send back a simple comma delimited list of items by setting up a design with the following syntax:

```
entity1.id, $entity2.id, $entity3.id, $entity4.id, $entity5.id, 
```

Alternatively, you can send additional information in the response. The following  code file is a more complex example that returns much more than the entity ids with their associated slots (order). This Design example also returns activity details, Target Profile details (as applicable), and other `entity.attributes` associated with the items returned.

```
    {   
     "adobeRecommendations": {   
      "notes": {   
       "purpose": "Return a list of entity ids with their associated entity.attributes",   
       "use-case": "Use this approach to avoid looking up attribute details after receiving a response from Target",   
       "version": "01"   
      },   
      "recommendedItems": {   
       "slot-01": "$entity1.id",   
       "slot-02": "$entity2.id",   
       "slot-03": "$entity3.id",   
       "slot-04": "$entity4.id",   
       "slot-05": "$entity5.id",   
       "slot-06": "$entity6.id",   
       "slot-07": "$entity7.id",   
       "slot-08": "$entity8.id",   
       "slot-09": "$entity9.id",   
       "slot-10": "$entity10.id"   
      },   
      "activityDetails": {   
       "mbox.name": "email-mbox",   
       "campaign.name": "\${campaign.name}",   
       "campaign.id": "\${campaign.id}",   
       "campaign.recipe.name": "\${campaign.recipe.name}",   
       "campaign.recipe.id": "\${campaign.recipe.id}",   
       "offer.name": "\${offer.name}",   
       "offer.id": "\${offer.id}",   
       "criteria.title": "$criteria.title",   
       "algorithm.name": "$algorithm.name",   
       "algorithm.dayCount": "$algorithm.dayCount"   
      },   
      "visitorProfile": {   
       "profile.favorite-category": "\${profile.favorite-category}",   
       "profile.test": "\${profile.test}",   
       "user.endpoint.lastPurchasedEntity": "\${user.endpoint.lastPurchasedEntity}",   
       "user.endpoint.lastViewedEntity": "\${user.endpoint.lastViewedEntity}",   
       "user.endpoint.mostViewedEntity": "\${user.endpoint.mostViewedEntity}",   
       "user.endpoint.categoryAffinity": "\${user.endpoint.categoryAffinity}",   
       "profile.geolocation.city": "\${profile.geolocation.city}",   
       "profile.geolocation.dma": "\${profile.geolocation.dma}",   
       "profile.geolocation.state": "\${profile.geolocation.state}",   
       "profile.geolocation.country": "\${profile.geolocation.country}",   
       "profile.sessionCount": "\${profile.sessionCount}",   
       "profile.averageDaysBetweenVisits": "\${profile.averageDaysBetweenVisits}",   
       "profile.browserTime": "\${profile.browserTime}",   
       "user.activeActivities": "\${user.activeActivities}",   
       "user.pcId": "\${user.pcId}",   
       "user.isFirstSession": "\${user.isFirstSession}",   
       "user.isNewSession": "\${user.isNewSession}",   
       "user.header": "\${user.header}",   
       "user.parameter": "\${user.parameter}"   
      },   
      "recKey": {   
       "recKeyDetails": {   
        "id": "$key.id",   
        "name": "$key.name",   
        "category": "$key.category",   
        "pageUrl": "$key.pageUrl",   
        "thumbnailUrl": "$key.thumbnailUrl"   
       }   
      },   
      "recDetailedResults": {   
       "recEntity1Details": {   
        "id": "$entity1.id",   
        "name": "$entity1.name",   
        "category": "$entity1.category",   
        "pageUrl": "$entity1.pageUrl",   
        "thumbnailUrl": "$entity1.thumbnailUrl"   
       },   
       "recEntity2Details": {   
        "id": "$entity2.id",   
        "name": "$entity2.name",   
        "category": "$entity2.category",   
        "pageUrl": "$entity2.pageUrl",   
        "thumbnailUrl": "$entity2.thumbnailUrl"   
       },   
       "recEntity3Details": {   
        "id": "$entity3.id",   
        "name": "$entity3.name",   
        "category": "$entity3.category",   
        "pageUrl": "$entity3.pageUrl",   
        "thumbnailUrl": "$entity3.thumbnailUrl"   
       },   
       "recEntity4Details": {   
        "id": "$entity4.id",   
        "name": "$entity4.name",   
        "category": "$entity4.category",   
        "pageUrl": "$entity4.pageUrl",   
        "thumbnailUrl": "$entity4.thumbnailUrl"   
       },   
       "recEntity5Details": {   
        "id": "$entity5.id",   
        "name": "$entity5.name",   
        "category": "$entity5.category",   
        "pageUrl": "$entity5.pageUrl",   
        "thumbnailUrl": "$entity5.thumbnailUrl"   
       },   
       "recEntity6Details": {   
        "id": "$entity6.id",   
        "name": "$entity6.name",   
        "category": "$entity6.category",   
        "pageUrl": "$entity6.pageUrl",   
        "thumbnailUrl": "$entity6.thumbnailUrl"   
       },   
       "recEntity7Details": {   
        "id": "$entity7.id",   
        "name": "$entity7.name",   
        "category": "$entity7.category",   
        "pageUrl": "$entity7.pageUrl",   
        "thumbnailUrl": "$entity7.thumbnailUrl"   
       },   
       "recEntity8Details": {   
        "id": "$entity8.id",   
        "name": "$entity8.name",   
        "category": "$entity8.category",   
        "pageUrl": "$entity8.pageUrl",   
        "thumbnailUrl": "$entity8.thumbnailUrl"   
       },   
       "recEntity9Details": {   
        "id": "$entity9.id",   
        "name": "$entity9.name",   
        "category": "$entity9.category",   
        "pageUrl": "$entity9.pageUrl",   
        "thumbnailUrl": "$entity9.thumbnailUrl"   
       },   
       "recEntity10Details": {   
        "id": "$entity10.id",   
        "name": "$entity10.name",   
        "category": "$entity10.category",   
        "pageUrl": "$entity10.pageUrl",   
        "thumbnailUrl": "$entity10.thumbnailUrl"   
       }   
      }   
     }   
    }  
```

