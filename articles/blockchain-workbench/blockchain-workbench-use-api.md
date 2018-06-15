---
title: REST-API van Azure Blockchain Workbench gebruiken
description: Scenario's voor het gebruik van de REST-API van Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 63e87c59a2e560b5a78708482c2ed89f5f8fb127
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34257900"
---
# <a name="using-the-azure-blockchain-workbench-rest-api"></a>REST-API van Azure Blockchain Workbench gebruiken 

De REST-API van Azure Blockchain Workbench biedt ontwikkelaars en IT-medewerkers een manier voor het bouwen van uitgebreide integraties voor blockchain-toepassingen. In dit document worden verschillende belangrijke methoden van de REST-API van Workbench beschreven. We gaan een scenario uitvoeren waarin een ontwikkelaar een aangepaste blockchain-client wil maken waarmee aangemelde gebruikers de blockchain-toepassingen kunnen zien en gebruiken die aan hen zijn toegewezen. De client stelt gebruikers in de gelegenheid exemplaren van contracten te bekijken en actie te ondernemen op slimme contracten. De client gebruikt de REST-API van Workbench in de context van de aangemelde gebruiker om het volgende te doen:

* Lijst met toepassingen weergeven
* Lijst met werkstromen voor een toepassing weergeven
* Lijst met exemplaren van slimme contracten voor een werkstroom weergeven
* Lijst met beschikbare acties voor een contract weergeven
* Een actie voor een contract uitvoeren

De voorbeeld-blockchain-toepassingen die in de scenario's kunnen worden [gedownload van GitHub](https://github.com/Azure-Samples/blockchain). 

## <a name="list-applications"></a>Lijst met toepassingen weergeven

Wanneer een gebruiker is aangemeld bij de client blockchain, is de eerste taak voor het ophalen van alle Blockchain Workbench toepassingen voor de gebruiker. In dit scenario heeft de gebruiker toegang tot twee toepassingen:

1.  [Asset-overdracht](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer/readme.md)
2.  [Gekoeld vervoer](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/refrigerated-transportation/readme.md)

Gebruik de [GET-API voor toepassingen](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget):

``` http
GET /api/v1/applications 
Authorization : Bearer {access token}
```

Het antwoord geeft een lijst van alle blockchain toepassingen waartoe een gebruiker toegang in Blockchain Workbench heeft. Beheerders van Blockchain Workbench zien alle blockchain-toepassingen, terwijl andere Workbench-gebruikers alleen de blockchains te zien krijgen waaraan ze via ten minste één toepassing of een exemplaar van één gekoppeld slim contract zijn gekoppeld.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications?skip=2",
    "applications": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
            "displayName": "Asset Transfer",
            "createdByUserId": 1,
            "createdDtTm": "2018-04-28T05:59:14.4733333",
            "enabled": true,
            "applicationRoles": null
        },
        {
            "id": 2,
            "name": "RefrigeratedTransportation",
            "description": "Application to track end-to-end transportation of perishable goods.",
            "displayName": "Refrigerated Transportation",
            "createdByUserId": 7,
            "createdDtTm": "2018-04-28T18:25:38.71",
            "enabled": true,
            "applicationRoles": null
        }
    ]
}
```

## <a name="list-workflows-for-an-application"></a>Lijst met werkstromen voor een toepassing weergeven

Wanneer een gebruiker de toepassing van toepassing blockchain in dit geval selecteert **Asset Transfer**, de client blockchain haalt alle werkstromen van de specifieke blockchain-toepassing. Gebruikers kunnen vervolgens de toepasselijke werkstroom selecteren voordat alle exemplaren van de slimme contracten voor de werkstroom worden weergegeven. Elke blockchain-toepassing heeft een of meer werkstromen en elke werkstroom heeft nul of meer exemplaren van slimme contracten. Wanneer u een blockchain-clienttoepassing gaat bouwen, is het raadzaam om de interface waarin gebruikers de juiste werkstroom kunnen selecteren weg te laten als er maar één werkstroom is voor de toepassing. In dit geval **Asset Transfer** heeft slechts één werkstroom, ook wel **Asset Transfer**.

Gebruik de [GET-API voor werkstromen van toepassingen](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget):

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

Het antwoord bestaat uit een lijst met alle werkstromen van de opgegeven blockchain-toepassing waartoe een gebruiker toegang heeft in Blockchain Workbench. Beheerders van Blockchain Workbench zien alle blockchain-werkstromen, terwijl andere Workbench-gebruikers alleen de werkstromen te zien krijgen waaraan ze via ten minste één toepassing of een exemplaar van één gekoppeld slim contract zijn gekoppeld.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications/1/workflows?skip=1",
    "workflows": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Handles the business logic for the asset transfer scenario",
            "displayName": "Asset Transfer",
            "applicationId": 1,
            "constructorId": 1,
            "startStateId": 1
        }
    ]
}
```

## <a name="list-smart-contract-instances-for-a-workflow"></a>Lijst met exemplaren van slimme contracten voor een werkstroom weergeven

Wanneer een gebruiker de betreffende workflow dit geval selecteert **Asset Transfer**, de client blockchain alle exemplaren van de smartcard contract voor de opgegeven workflow wordt opgehaald. U kunt deze gegevens gebruiken om alle exemplaren van slimme contracten voor de werkstroom weer te geven, zodat gebruikers een contract in detail kunnen bekijken. Stel dat een gebruiker in dit voorbeeld interactie wil hebben met een van de exemplaren van een slim contract om er vervolgens een bepaalde actie op uit te voeren.

Gebruik de [GET-API voor contracten](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractsget):

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

Het antwoord bestaat uit een lijst met alle exemplaren van slimme contracten van de opgegeven werkstroom. Beheerders van Workbench zien alle exemplaren van slimme contracten, terwijl andere Workbench-gebruikers alleen de exemplaren te zien krijgen waaraan ze via ten minste één toepassing of een exemplaar van één gekoppeld slim contract zijn gekoppeld.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts?skip=3&workflowId=1",
    "contracts": [
        {
            "id": 1,
            "provisioningStatus": 2,
            "connectionID": 1,
            "ledgerIdentifier": "0xbcb6127be062acd37818af290c0e43479a153a1c",
            "deployedByUserId": 1,
            "workflowId": 1,
            "contractCodeId": 1,
            "contractProperties": [
                {
                    "workflowPropertyId": 1,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 2,
                    "value": "My first car"
                },
                {
                    "workflowPropertyId": 3,
                    "value": "54321"
                },
                {
                    "workflowPropertyId": 4,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 5,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 6,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 7,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 8,
                    "value": "0xd882530eb3d6395e697508287900c7679dbe02d7"
                }
            ],
            "transactions": [
                {
                    "id": 1,
                    "connectionId": 1,
                    "transactionHash": "0xf3abb829884dc396e03ae9e115a770b230fcf41bb03d39457201449e077080f4",
                    "blockID": 241,
                    "from": "0xd882530eb3d6395e697508287900c7679dbe02d7",
                    "to": null,
                    "value": 0,
                    "isAppBuilderTx": true
                }
            ],
            "contractActions": [
                {
                    "id": 1,
                    "userId": 1,
                    "provisioningStatus": 2,
                    "timestamp": "2018-04-29T23:41:14.9333333",
                    "parameters": [
                        {
                            "name": "Description",
                            "value": "My first car"
                        },
                        {
                            "name": "Price",
                            "value": "54321"
                        }
                    ],
                    "workflowFunctionId": 1,
                    "transactionId": 1,
                    "workflowStateId": 1
                }
            ]
        }
    ]
}
```

## <a name="list-available-actions-for-a-contract"></a>Lijst met beschikbare acties voor een contract weergeven

Op het moment dat een gebruiker een van de contracten in meer detail gaat bekijken, kan de blockchain-client alle beschikbare acties weergeven aan de gebruiker op basis van de status van het contract. In dit voorbeeld kijkt de gebruiker naar alle beschikbare acties voor een nieuw slim contract die hij of zij heeft gemaakt:

* Modify: hiermee kan de gebruiker de beschrijving en prijs van een asset aanpassen.
* Terminate: hiermee kan de gebruiker het contract van de asset beëindigen.

Gebruik de [GET-API voor contractacties](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionget):

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

Het antwoord bestaat uit een lijst met alle acties die een gebruiker kan uitvoeren op basis van de huidige status van het opgegeven exemplaar van het slimme contract. Gebruikers zien alle toepasselijke acties als de gebruiker een bijbehorende toepassingsrol heeft of een rol voor het exemplaar van het slimme contract voor de huidige status van het opgegeven exemplaar van het slimme contract.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts/1/actions?skip=2",
    "workflowFunctions": [
        {
            "id": 2,
            "name": "Modify",
            "description": "Modify the description/price attributes of this asset transfer instance",
            "displayName": "Modify",
            "parameters": [
                {
                    "id": 1,
                    "name": "description",
                    "description": "The new description of the asset",
                    "displayName": "Description",
                    "type": {
                        "id": 2,
                        "name": "string",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                },
                {
                    "id": 2,
                    "name": "price",
                    "description": "The new price of the asset",
                    "displayName": "Price",
                    "type": {
                        "id": 3,
                        "name": "money",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                }
            ],
            "workflowId": 1
        },
        {
            "id": 3,
            "name": "Terminate",
            "description": "Used to cancel this particular instance of asset transfer",
            "displayName": "Terminate",
            "parameters": [],
            "workflowId": 1
        }
    ]
}
```

## <a name="execute-an-action-for-a-contract"></a>Een actie voor een contract uitvoeren

Een gebruiker kan vervolgens besluiten om actie te ondernemen voor het opgegeven exemplaar van een slim contract. In dit geval nemen we het scenario dat een gebruiker de beschrijving en de prijs van een asset wilt wijzigen in:

* Beschrijving: 'My updated car'
* Prijs: 54321

Gebruik de [POST-API voor contractacties](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionpost):

``` http
POST /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
actionInformation: {
    "workflowFunctionId": 2,
    "workflowActionParameters": [
        {
            "name": "description",
            "value": "My updated car"
        },
        {
            "name": "price",
            "value": "54321"
        }
    ]
}
```

Gebruikers kunnen alleen de actie uitvoeren die mogelijk is op basis van de huidige status van het opgegeven exemplaar van het slimme contract en de bijbehorende toepassingsrol van de gebruiker of de rol van het exemplaar van het slimme contract. Als het posten is gelukt, wordt dit aangegeven met een HTTP 200 OK-antwoord zonder verdere toelichting.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over de REST-API van Azure Blockchain Workbench](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)