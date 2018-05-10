---
title: Met behulp van Azure Blockchain Workbench REST API
description: Scenario's voor het gebruik van de Azure Blockchain Workbench REST-API
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/2/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 27ed94b3ce14c57e369b0c80d4c53b72a5ae40a8
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="using-the-azure-blockchain-workbench-rest-api"></a>Met behulp van de REST-API van Azure Blockchain Workbench 

Azure Blockchain Workbench REST-API biedt ontwikkelaars en IT-medewerkers een manier voor het bouwen van uitgebreide integraties blockchain van toepassingen. Dit document begeleidt u bij verschillende belangrijke methoden van de Workbench REST-API. Overweeg een scenario waarbij een ontwikkelaar wil maken van een aangepaste blockchain-client, die kan zien en gebruiken met hun toepassingen toegewezen blockchain aangemelde gebruikers. De client kan gebruikers contract exemplaren weergeven en er acties op smartcard contracten. De client gebruikt de Workbench REST-API in de context van de aangemelde gebruiker het volgende doen:

* Lijst met toepassingen
* Lijst met werkstromen voor een toepassing
* Lijst met slim contract exemplaren voor een werkstroom
* Lijst met beschikbare acties voor een contract
* Een actie voor een contract uitvoeren

## <a name="list-applications"></a>Lijst met toepassingen

Wanneer een gebruiker is aangemeld bij de client blockchain, is de eerste taak voor het ophalen van alle Blockchain Workbench blockchain toepassingen voor de gebruiker. In dit scenario wordt heeft de gebruiker toegang tot twee toepassingen:

1.  Asset-overdracht
2.  Gekoeld vervoer

Gebruik de [toepassingen GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget):

``` http
GET /api/v1/applications 
Authorization : Bearer {access token}
```

Antwoord geeft een lijst van alle blockchain toepassingen waartoe een gebruiker toegang in Blockchain Workbench heeft. Blockchain Workbench beheerders krijgen alle blockchain toepassingen, terwijl niet-Workbench beheerders krijgen alle blockchains waarvoor ze ten minste één toepassing gekoppeld aan functie of de functie van een gekoppelde smart contract-exemplaar hebben.

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

## <a name="list-workflows-for-an-application"></a>Lijst met werkstromen voor een toepassing

Wanneer een gebruiker de betreffende blockchain-toepassing in dit geval Asset overdraagt selecteert, haalt de client blockchain alle werkstromen van de specifieke blockchain-toepassing. Gebruikers kunnen vervolgens de toepasselijke werkstroom selecteren voordat u alle exemplaren van de smartcard contract voor de werkstroom wordt weergegeven. Elke toepassing blockchain heeft een of meer werkstromen en elke werkstroom heeft nul of een smartcard contract exemplaren. Wanneer u blockchain clienttoepassingen, verdient het de gebruikerservaring stroom zodat gebruikers kunnen de juiste werkstroom selecteren wanneer er slechts één werkstroom voor de toepassing blockchain overslaan. In dit geval heeft Asset Transfer slechts één werkstroom, ook wel Asset overdragen.

Gebruik de [toepassingen werkstromen GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget):

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

Antwoord geeft een lijst van alle werkstromen van de opgegeven blockchain toepassing waartoe een gebruiker toegang in Blockchain Workbench heeft. Blockchain Workbench beheerders krijgen alle blockchain werkstromen, terwijl niet-Workbench beheerders krijgen alle werkstromen waarvoor ze ten minste één rol van de bijbehorende toepassing hebben of is gekoppeld aan een rol smart contract-exemplaar.

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

## <a name="list-smart-contract-instances-for-a-workflow"></a>Lijst met slim contract exemplaren voor een werkstroom

Wanneer een gebruiker de toepasselijke workflow, in dit geval Asset overdraagt selecteert, wordt de client blockchain alle exemplaren van de smartcard contract voor de opgegeven werkstroom ophalen. Deze informatie kunt u alle exemplaren van de smartcard contract voor de workflow weergeven en gebruikers kunnen worden diepgaand in een van de exemplaren weergegeven smart contract. In dit voorbeeld kunt u dat een gebruiker wil communiceren met een van de smartcard contract exemplaren om actie te ondernemen.

Gebruik de [contracten GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractsget):

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

Antwoord geeft een lijst van alle exemplaren van de smartcard contract van de opgegeven workflow. Workbench beheerders ophalen van alle exemplaren van de smartcard contract, terwijl niet-Workbench beheerders alle contract-exemplaren waarvoor ze ten minste één rol van de bijbehorende toepassing hebben van smartcard of is gekoppeld aan een rol smart contract-exemplaar.

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

## <a name="list-available-actions-for-a-contract"></a>Lijst met beschikbare acties voor een contract

Zodra een gebruiker heeft besloten te diepgaand in een van de contracten, kan de client blockchain vervolgens alle beschikbare acties weergeven voor de gebruiker krijgt de status van het contract. In dit voorbeeld is de gebruiker op zoek op alle beschikbare acties voor een nieuw smart contract die ze gemaakt:

* Wijzig: Kan de gebruiker om de beschrijving en de prijs van een activum te wijzigen.
* Beëindigen: Kan de gebruiker het contract van de activa is beëindigd.

Gebruik de [Contract actie GET API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionget):

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

Antwoord geeft een lijst van alle acties die een gebruiker kan duren gegeven van de huidige status van het exemplaar van de opgegeven smartcard contract. Gebruikers krijgen alle toepasselijke acties als de gebruiker een bijbehorende toepassingsrol heeft of aan een rol voor het exemplaar van slimme contract voor de huidige status van het exemplaar van de opgegeven smartcard contract gekoppeld is.

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

Een gebruiker kan vervolgens besluit actie voor het exemplaar van de opgegeven smartcard contract te ondernemen. In dit geval Overweeg het scenario waarbij een gebruiker wijzigen wilt, beschrijving en de prijs van een activum met het volgende:

* Beschrijving: 'Mijn bijgewerkte auto'
* Prijs: 54321

Gebruik de [Contract actie POST API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/contracts/contractactionpost):

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

Gebruikers kunnen alleen de actie gegeven van de huidige status van het exemplaar van de opgegeven smartcard contract en rol van de bijbehorende toepassing van de gebruiker of rol smart contract-exemplaren worden uitgevoerd. Als het bericht geslaagd is, wordt er een HTTP 200 OK-antwoord geretourneerd met geen antwoordtekst.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Volgende stappen

* [Naslaginformatie over Azure Blockchain Workbench REST-API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)