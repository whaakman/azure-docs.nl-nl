---
title: Bewerkingen API ophalen | Azure Marketplace
description: Hiermee haalt u alle bewerkingen op de aanbieding of het opvragen van een bepaalde bewerking voor de opgegeven bewerkings-id.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/14/2018
ms.author: pabutler
ms.openlocfilehash: 1fbcc1d50dbc4488c4123be64e85de612233ccc3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935782"
---
<a name="retrieve-operations"></a>Bewerkingen ophalen
===================

Hiermee haalt u alle bewerkingen op de aanbieding of het opvragen van een bepaalde bewerking voor de opgegeven bewerkings-id. De client kan query-parameters gebruiken om te filteren op het uitvoeren van bewerkingen.

``` https

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/submissions/?api-version=2017-10-31&status=<filteredStatus>

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/operations/<operationId>?api-version=2017-10-31

```


<a name="uri-parameters"></a>URI-parameters
--------------

|  **Naam**          |      **Beschrijving**                                                                                           | **Gegevenstype** |
|  ----------------  |     --------------------------------------------------------------------------------------------------------   |  -----------  |
|  publisherId       |  Uitgever-ID, bijvoorbeeld `Contoso`                                                                   |  String       |
|  offerId           |  Aanbiedings-id                                                                                              |  String       |
|  operationId       |  De GUID die de bewerking op de aanbieding wordt aangeduid. De bewerkings-id met behulp van deze API kan worden opgehaald en wordt ook geretourneerd als de HTTP-header van het antwoord voor langlopende bewerkingen, zoals de [aanbieding publiceren](./cloud-partner-portal-api-publish-offer.md) API.  |   Guid   |
|  filteredStatus    | Optionele query-parameter die wordt gebruikt om te filteren op status (bijvoorbeeld `running`) op de verzameling die wordt geretourneerd door deze API.  |   String |
|  api-version       | Meest recente versie van de API                                                                                           |    Date      |
|  |  |  |


<a name="header"></a>Header
------

|  **Naam**          |  **Waarde**           |
|  ---------------   | -------------------- |
|  Content-Type      | `application/json`   |
|  Autorisatie     | `Bearer YOUR_TOKEN`  |
|  |  |


<a name="body-example"></a>Voorbeeld van de hoofdtekst
------------

### <a name="response"></a>Antwoord

#### <a name="get-operations"></a>Bewerkingen ophalen

``` json
    [
        {
            "id": "5a63deb5-925b-4ee0-938b-7c86fbf287c5",
            "offerId": "56615b67-2185-49fe-80d2-c4ddf77bb2e8",
            "offerVersion": 1,
            "offerTypeId": "microsoft-azure-virtualmachines",
            "publisherId": "contoso",
            "submissionType": "publish",
            "submissionState": "running",
            "publishingVersion": 2,
            "slot": "staging",
            "version": 636576975611768314,
            "definition": {
                "metadata": {
                    "emails": "jdoe@contoso.com"
                }
            },
            "changedTime": "2018-03-26T21:46:01.179948Z"
        }
    ]
```

#### <a name="get-operation"></a>GET-bewerking

``` json
    [
        {
            "status" : "running",
            "messages" : [],
            "publishingVersion" : 2,
            "offerVersion" : 1,
            "cancellationRequestState": "canCancel",
            "steps": [
                        {
                            "estimatedTimeFrame": "< 15 min",
                            "id": "displaydummycertify",
                            "stepName": "Validate Pre-Requisites",
                            "description": "Offer settings provided are validated",
                            "status": "complete",
                            "messages": 
                            [
                                {
                                    "messageHtml": "Step completed.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:36.500052Z"
                                }
                            ],
                            "progressPercentage": 100
                        },
                        {
                            "estimatedTimeFrame": "< 5 day",
                            "id": "displaycertify",
                            "stepName": "Certification",
                            "description": "Your offer is analyzed by our certification systems for issues.",
                            "status": "blocked",
                            "messages": 
                            [
                                {
                                    "messageHtml": "No virtual machine image was found for the plan contoso.",
                                    "level": "error",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                },
                                {
                                    "messageHtml": "This step has not started yet.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                }
                            ],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 day",
                            "id": "displayprovision",
                            "stepName": "Provisioning",
                            "description": "Your virtual machine is being replicated in our production systems.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 hour",
                            "id": "displaypackage",
                            "stepName": "Packaging and Lead Generation Registration",
                            "description": "Your virtual machine is packaged for being shown to your customers. Additionally, we hookup our lead generation systems to send leads for your offer.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "id": "publisher-signoff",
                            "stepName": "Publisher signoff",
                            "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "~2-5 days",
                            "id": "live",
                            "stepName": "Live",
                            "description": "Offer is publicly visible and is available for purchase.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        }
                    ],
                "previewLinks": [],
                "liveLinks": [],
                "notificationEmails": "jondoe@contoso.com"
            } 
        }
    ]
```


### <a name="response-body-properties"></a>Eigenschappen van de hoofdtekst van reactie

|  **Naam**                    |  **Beschrijving**                                                                                  |
|  --------------------        |  ------------------------------------------------------------------------------------------------ |
|  id                          | De GUID die de bewerking wordt aangeduid                                                       |
|  submissionType              | Geeft het type van de bewerking voor de aanbieding wordt gerapporteerd, bijvoorbeeld `Publish/GGoLive`      |
|  createdDateTime             | UTC datum/tijd waarop de bewerking is gemaakt                                                       |
|  lastActionDateTime          | UTC datum/tijd wanneer de laatste update van de bewerking is uitgevoerd                                       |
|  status                      | Status van de bewerking, ofwel `not started` \| `running` \| `failed` \| `completed`. Kan slechts één bewerking status hebben `running` tegelijk. |
|  error                       | Foutbericht voor mislukte bewerkingen                                                               |
|  |  |


### <a name="response-status-codes"></a>Antwoord-statuscodes

| **Code**  |   **Beschrijving**                                                                                  |
|  -------- |   -------------------------------------------------------------------------------------------------|
|  200      | `OK` -De aanvraag is verwerkt en de bewerking(en) aangevraagd zijn geretourneerd.        |
|  400      | `Bad/Malformed request` -De antwoordtekst voor fout bevat mogelijk meer informatie.                    |
|  403      | `Forbidden` -De client heeft geen toegang tot de opgegeven naamruimte.                          |
|  404      | `Not found` -De opgegeven entiteit bestaat niet.                                                 |
|  |  |
