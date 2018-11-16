---
title: REST-API van Azure Blockchain Workbench gebruiken
description: Scenario's voor het gebruik van de REST-API van Azure Blockchain Workbench
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 11/14/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: c1a9b526f08f330d62c30dd1d676e95460aee6c2
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712344"
---
# <a name="using-the-azure-blockchain-workbench-rest-api"></a>REST-API van Azure Blockchain Workbench gebruiken

De REST-API van Azure Blockchain Workbench biedt ontwikkelaars en IT-medewerkers een manier voor het bouwen van uitgebreide integraties voor blockchain-toepassingen. In dit document worden verschillende belangrijke methoden van de REST-API van Workbench beschreven. Stel bijvoorbeeld dat een ontwikkelaar wil maken van een aangepaste blockchain-client. Deze blockchain-client kan aangemelde gebruikers kunnen zien en gebruiken met hun toegewezen blockchaintoepassingen. De client stelt gebruikers in de gelegenheid exemplaren van contracten te bekijken en actie te ondernemen op slimme contracten. De client gebruikt de Workbench REST-API in de context van de gebruiker is aangemeld op de volgende acties uitvoeren:

* Lijst met toepassingen weergeven
* Lijst met werkstromen voor een toepassing weergeven
* Lijst met exemplaren van slimme contracten voor een werkstroom weergeven
* Lijst met beschikbare acties voor een contract weergeven
* Een actie voor een contract uitvoeren

De voorbeeld blockchain-toepassingen die in de scenario's kunnen worden [gedownload uit GitHub](https://github.com/Azure-Samples/blockchain).

## <a name="list-applications"></a>Lijst met toepassingen weergeven

Wanneer een gebruiker is aangemeld bij de blockchain-client, is de eerste taak om op te halen van alle Blockchain Workbench-toepassingen voor de gebruiker. In dit scenario heeft de gebruiker toegang tot twee toepassingen:

1. [Asset-overdracht](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer/readme.md)
2. [Gekoeld transport](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/refrigerated-transportation/readme.md)

Gebruik de [GET-API voor toepassingen](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget):

``` http
GET /api/v1/applications
Authorization : Bearer {access token}
```

Het antwoord geeft een lijst van alle blockchain-toepassingen waartoe een gebruiker toegang in Blockchain Workbench heeft. Blockchain Workbench-beheerders krijgen alle blockchain-toepassingen. Niet-Workbench beheerders ophalen alle blockchains waarvoor ze ten minste één rol van de bijbehorende toepassing of een rol gekoppeld slimme contract exemplaren hebben.

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

Wanneer een gebruiker de van toepassing blockchain-toepassingen selecteert (zoals **Asset Transfer**), de blockchain-client haalt alle werkstromen van de specifieke blockchain-toepassing. Gebruikers kunnen vervolgens de toepasselijke werkstroom selecteren voordat alle exemplaren van de slimme contracten voor de werkstroom worden weergegeven. Elke blockchain-toepassing heeft een of meer werkstromen en elke werkstroom heeft nul of meer exemplaren van slimme contracten. Voor een clienttoepassing blockchain met slechts één werkstroom, is het beste ervaring met de gebruikersstroom waarmee gebruikers selecteert u de juiste werkstroom wordt overgeslagen. In dit geval **Asset Transfer** heeft slechts één werkstroom, ook wel genoemd **Asset Transfer**.

Gebruik de [GET-API voor werkstromen van toepassingen](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget):

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

Het antwoord bestaat uit een lijst met alle werkstromen van de opgegeven blockchain-toepassing waartoe een gebruiker toegang heeft in Blockchain Workbench. Blockchain Workbench beheerders ophalen elke werkstroom blockchain. Niet-Workbench-beheerders krijgen alle werkstromen die ze hebben van ten minste één rol van de bijbehorende toepassing of is gekoppeld aan een rol met slimme contract exemplaren.

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

Wanneer een gebruiker de van toepassing werkstroom, dit geval selecteert **Asset Transfer**, de blockchain-client worden alle exemplaren van slimme contract voor de opgegeven werkstroom opgehaald. U kunt deze informatie gebruiken om weer te geven van alle exemplaren van slimme contract voor de werkstroom. Of u kunt toestaan dat gebruikers om u te verdiepen in een van de exemplaren weergegeven slimme contract. Stel dat een gebruiker in dit voorbeeld interactie wil hebben met een van de exemplaren van een slim contract om er vervolgens een bepaalde actie op uit te voeren.

Gebruik de [GET-API voor contracten](/rest/api/azure-blockchain-workbench/contractsv2/contractsget):

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

Het antwoord bestaat uit een lijst met alle exemplaren van slimme contracten van de opgegeven werkstroom. Workbench-beheerders krijgen alle exemplaren van slimme contract. Niet-Workbench beheerders ophalen elke slimme contract-exemplaar waarvoor ze ten minste één rol van de bijbehorende toepassing of is gekoppeld aan een rol met slimme contract exemplaren.

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

Wanneer een gebruiker om u te verdiepen in een contract besluit, kan de blockchain-client de beschikbare acties krijgt de status van het contract wordt weergegeven. In dit voorbeeld kijkt de gebruiker naar alle beschikbare acties voor een nieuw slim contract die hij of zij heeft gemaakt:

* Modify: hiermee kan de gebruiker de beschrijving en prijs van een asset aanpassen.
* Beëindigen: Kan de gebruiker aan het einde van het contract van de asset.

Gebruik de [GET-API voor contractacties](/rest/api/azure-blockchain-workbench/contractsv2/contractactionget):

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

Een gebruiker kan vervolgens besluiten om actie te ondernemen voor het opgegeven exemplaar van een slim contract. In dit geval, houd rekening met het scenario waarin een gebruiker wilt wijzigen van de beschrijving en prijs van een asset naar de volgende actie:

* Beschrijving: 'My updated car'
* Prijs: 54321

Gebruik de [POST-API voor contractacties](/rest/api/azure-blockchain-workbench/contractsv2/contractactionpost):

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