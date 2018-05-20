---
title: Naslaginformatie over Azure Blockchain Workbench configuratie
description: Blockchain Workbench toepassing configuratie overzicht van Azure.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 178c2c1d4f727241338d6d933cd5eecbbffe65bb
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="azure-blockchain-workbench-configuration-reference"></a>Naslaginformatie over Azure Blockchain Workbench configuratie

 Azure Blockchain Workbench-toepassingen zijn meerdere partijen werkstromen die zijn gedefinieerd door de metagegevens van de configuratie en slimme Contractcode. Metagegevens van de configuratie definieert de werkstromen op hoog niveau en model van de interactie van de toepassing blockchain. Smart contracten definiëren de zakelijke logica van de toepassing blockchain. Workbench maakt gebruik van configuratie en slimme Contractcode blockchain toepassing gebruikerservaringen genereren.

Metagegevens van de configuratie bevat de volgende informatie voor elke toepassing blockchain: 

* Naam en beschrijving van de toepassing blockchain
* Unieke functies voor gebruikers die u kunnen uitvoeren of in de toepassing blockchain deelnemen
* Een of meer werkstromen. Elke werkstroom fungeert als een statusmachine om te bepalen van de stroom van de zakelijke logica. Werkstromen kunnen onafhankelijk of met elkaar communiceren.

Elke gedefinieerde workflow bevat het volgende:

* Naam en beschrijving van de werkstroom
* De status van de werkstroom.  Elke status is een fase in de zakelijke logica Controlestroom. 
* Bewerkingen voor de overgang naar de volgende status
* Gebruikersrollen die over toegangsrechten voor het initiëren van elke actie
* Smart contracten die bedrijfslogica in codebestanden vertegenwoordigen

## <a name="application"></a>Toepassing

Een toepassing blockchain bevat configuratie metagegevens, werkstromen en gebruiker functies die u kunnen uitvoeren of nemen in de toepassing.

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| ApplicationName | Unieke toepassingsnaam in. Het bijbehorende smart contract moet gebruikmaken van dezelfde **ApplicationName** voor de contractklasse die van toepassing zijn.  | Ja |
| Weergavenaam | Beschrijvende weergavenaam van de toepassing. | Ja |
| Beschrijving | Beschrijving van de toepassing. | Nee |
| ApplicationRoles | Verzameling van [ApplicationRoles](#application-roles). Gebruikersrollen die kunnen optreden of nemen in de toepassing.  | Ja |
| Werkstromen | Verzameling van [werkstromen](#workflows). Elke werkstroom fungeert als een statusmachine om te bepalen van de stroom van de zakelijke logica. | Ja |

Zie voor een voorbeeld [voorbeeldconfiguratiebestand](#configuration-file-example).

## <a name="workflows"></a>Werkstromen

De bedrijfslogica voor een toepassing kan worden gezien als een statusmachine waar een actie zorgt ervoor de stroom van de bedrijfslogica dat voor het verplaatsen van een status naar een andere. Een werkstroom is een verzameling van dergelijke statussen en bewerkingen. Elke werkstroom bestaat uit een of meer smart contracten die de zakelijke logica in codebestanden vertegenwoordigen. Een uitvoerbaar contract is een exemplaar van een werkstroom.

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | Unieke Werkstroomnaam. Het bijbehorende smart contract moet gebruikmaken van dezelfde **naam** voor de contractklasse die van toepassing zijn. | Ja |
| Weergavenaam | Beschrijvende weergavenaam van de werkstroom. | Ja |
| Beschrijving | Beschrijving van de werkstroom. | Nee |
| Initiators | Verzameling van [ApplicationRoles](#application-roles). Rollen die zijn toegewezen aan gebruikers die zijn gemachtigd voor het maken van de contracten in de werkstroom. | Ja |
| StartState | Naam van de beginstatus van de werkstroom. | Ja |
| Eigenschappen | Verzameling van [id's](#identifiers). Geeft gegevens die kunnen worden gelezen uit de keten of in een gebruiker gevisualiseerde ervaren hulpprogramma. | Ja |
| Constructor | Hiermee definieert u invoerparameters voor het maken van een exemplaar van de werkstroom. | Ja |
| Functions | Een verzameling van [functies](#functions) die kunnen worden uitgevoerd in de werkstroom. | Ja |
| statussen | Een verzameling van werkstroom [statussen](#states). | Ja |

Zie voor een voorbeeld [voorbeeldconfiguratiebestand](#configuration-file-example).

## <a name="type"></a>Type

Ondersteunde gegevenstypen.

| Type | Beschrijving |
|-------|-------------|
| Adres  | Blockchain-adrestype, zoals *contracten* of *gebruikers* |
| BOOL     | Booleaans gegevenstype |
| Contract | Adres van het type contract |
| int      | Het gegevenstype geheel getal |
| Money    | Money-gegevenstype |
| state    | Werkstroomstatus |
| tekenreeks   | Het gegevenstype String |
| Gebruiker     | Adres van het type gebruiker |
| tijd     | Tijd gegevenstype |
|`[ Application Role Name ]`| De naam opgegeven in de toepassingsrol. Gebruikers van dat Roltype worden beperkt. |

## <a name="constructor"></a>Constructor

Hiermee definieert u invoerparameters voor een exemplaar van een werkstroom.

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Parameters | Verzameling van [id's](#identifiers) vereist voor het initiëren van een smartcard contract. | Ja |

### <a name="constructor-example"></a>Voorbeeld van de constructor

``` json
{
  "Parameters": [
    {
      "Name": "description",
      "Description": "The description of this asset",
      "DisplayName": "Description",
      "Type": {
        "Name": "string"
      }
    },
    {
      "Name": "price",
      "Description": "The price of this asset",
      "DisplayName": "Price",
      "Type": {
        "Name": "money"
      }
    }
  ]
}
```

## <a name="functions"></a>Functions

Hiermee definieert u de functies die kunnen worden uitgevoerd in de werkstroom.

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | De unieke naam van de functie. Het bijbehorende smart contract moet gebruikmaken van dezelfde **naam** voor de desbetreffende functie. | Ja |
| Weergavenaam | Beschrijvende weergavenaam van de functie. | Ja |
| Beschrijving | Beschrijving van de functie | Nee |
| Parameters | Verzameling van [id's](#identifiers) overeenkomt met de parameters van de functie. | Ja |

### <a name="functions-example"></a>Voorbeeld van de functies

``` json
"Functions": [
  {
    "Name": "Modify",
    "DisplayName": "Modify",
    "Description": "Modify the description/price attributes of this asset transfer instance",
    "Parameters": [
      {
        "Name": "description",
        "Description": "The new description of the asset",
        "DisplayName": "Description",
        "Type": {
          "Name": "string"
        }
      },
      {
        "Name": "price",
        "Description": "The new price of the asset",
        "DisplayName": "Price",
        "Type": {
          "Name": "money"
        }
      }
    ]
  },
  {
    "Name": "Terminate",
    "DisplayName": "Terminate",
    "Description": "Used to cancel this particular instance of asset transfer",
    "Parameters": []
  }
]

```

## <a name="states"></a>statussen

Een verzameling van unieke statussen binnen een werkstroom. Elke status bevat een stap in de zakelijke logica Controlestroom. 

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | De unieke naam van de status. Het bijbehorende smart contract moet gebruikmaken van dezelfde **naam** voor de status van de van toepassing. | Ja |
| Weergavenaam | Beschrijvende weergavenaam van de status. | Ja |
| Beschrijving | Beschrijving van de status. | Nee |
| PercentComplete | Een geheel getal in de gebruikersinterface Blockchain Workbench om de voortgang binnen de Controlestroom voor zakelijke logica weergegeven. | Ja |
| Stijl | Visual-hint die aangeeft of de status een status geslaagd of mislukt vertegenwoordigt. Er zijn twee geldige waarden: `Success` of `Failure`. | Ja |
| Overgangen | Verzameling van beschikbare [overgangen](#transitions) van de huidige status in de volgende set van statussen. | Nee |

### <a name="states-example"></a>Voorbeeld van statussen

``` json
"States": [
    {
      "Name": "Active",
      "DisplayName": "Active",
      "Description": "The initial state of the asset transfer workflow",
      "PercentComplete": 20,
      "Style": "Success",
      "Transitions": [
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Cancels this instance of asset transfer",
          "Function": "Terminate",
          "NextStates": [ "Terminated" ],
          "DisplayName": "Terminate Offer"
        },
        {
          "AllowedRoles": [ "Buyer" ],
          "AllowedInstanceRoles": [],
          "Description": "Make an offer for this asset",
          "Function": "MakeOffer",
          "NextStates": [ "OfferPlaced" ],
          "DisplayName": "Make Offer"
        },
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Modify attributes of this asset transfer instance",
          "Function": "Modify",
          "NextStates": [ "Active" ],
          "DisplayName": "Modify"
        }
      ]
    },
    {
      "Name": "Accepted",
      "DisplayName": "Accepted",
      "Description": "Asset transfer process is complete",
      "PercentComplete": 100,
      "Style": "Success",
      "Transitions": []
    },
    {
      "Name": "Terminated",
      "DisplayName": "Terminated",
      "Description": "Asset transfer has been cancelled",
      "PercentComplete": 100,
      "Style": "Failure",
      "Transitions": []
    }
  ]
```

## <a name="transitions"></a>Overgangen

Beschikbare acties op de volgende status. Een of meer gebruikersrollen, kunnen een actie op elke status, waarbij een actie een status naar een andere staat in de werkstroom kan overgang uitvoeren. 

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| AllowedRoles | Lijst met rollen van toepassingen toegestaan voor het initiëren van de overgang. Alle gebruikers van de opgegeven rol mogelijk de actie uit te voeren. | Nee |
| AllowedInstanceRoles | Lijst met gebruikersrollen deelnemende of opgegeven in de smartcard contract mag initiëren van de overgang. Rolinstanties zijn gedefinieerd in **eigenschappen** in werkstromen. AllowedInstanceRoles vertegenwoordigen een gebruiker deel uitmaakt van een exemplaar van een smartcard contract. AllowedInstanceRoles bieden u de mogelijkheid om te beperken waarbij een actie die een gebruikersrol in een exemplaar van het contract.  U kunt bijvoorbeeld alleen zodat de gebruiker die het contract (InstanceOwner) om te beëindigen in plaats van alle gebruikers in Roltype (eigenaar) als u de rol hebt opgegeven in AllowedRoles heeft gemaakt. | Nee |
| Weergavenaam | Beschrijvende weergavenaam van de overgang. | Ja |
| Beschrijving | Beschrijving van de overgang. | Nee |
| Functie | De naam van de functie voor het initiëren van de overgang. | Ja |
| NextStates | Een verzameling van mogelijke volgende statussen na een geslaagde overgang. | Ja |

### <a name="transitions-example"></a>Voorbeeld van overgangen

``` json
"Transitions": [
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Cancels this instance of asset transfer",
    "Function": "Terminate",
    "NextStates": [ "Terminated" ],
    "DisplayName": "Terminate Offer"
  },
  {
    "AllowedRoles": [ "Buyer" ],
    "AllowedInstanceRoles": [],
    "Description": "Make an offer for this asset",
    "Function": "MakeOffer",
    "NextStates": [ "OfferPlaced" ],
    "DisplayName": "Make Offer"
  },
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Modify attributes of this asset transfer instance",
    "Function": "Modify",
    "NextStates": [ "Active" ],
    "DisplayName": "Modify"
  }
]

```

## <a name="application-roles"></a>Toepassingsrollen

Toepassingsrollen definiëren een set functies die kunnen worden toegewezen aan gebruikers die fungeren of deelnemen aan in de toepassing wilt. Toepassingsrollen kunnen worden gebruikt voor het beperken van acties en deelname binnen de blockchain toepassing en de bijbehorende workflows. 

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | De unieke naam van de toepassingsrol. Het bijbehorende smart contract moet gebruikmaken van dezelfde **naam** voor de rol van toepassing. Basistype namen zijn gereserveerd. U kunt een toepassingsrol kan geen naam met dezelfde naam als [Type](#type)| Ja |
| Beschrijving | Beschrijving van de toepassingsrol. | Nee |

### <a name="application-roles-example"></a>Voorbeeld van de toepassing-functies

``` json
"ApplicationRoles": [
  {
    "Name": "Appraiser",
    "Description": "User that signs off on the asset price"
  },
  {
    "Name": "Buyer",
    "Description": "User that places an offer on an asset"
  }
]
```
## <a name="identifiers"></a>Id's

Id's vertegenwoordigt een verzameling gegevens die worden gebruikt om te beschrijven van werkstroomeigenschappen constructor en functieparameters. 

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | De unieke naam van de eigenschap of parameter. Het bijbehorende smart contract moet gebruikmaken van dezelfde **naam** voor de toepasselijke eigenschap of parameter. | Ja |
| Weergavenaam | Beschrijvende weergavenaam voor de eigenschap of parameter. | Ja |
| Beschrijving | Beschrijving van de eigenschap of parameter. | Nee |

### <a name="identifiers-example"></a>Voorbeeld van id 's

``` json
"Properties": [
  {
    "Name": "State",
    "DisplayName": "State",
    "Description": "Holds the state of the contract",
    "Type": {
      "Name": "state"
    }
  },
  {
    "Name": "Description",
    "DisplayName": "Description",
    "Description": "Describes the asset being sold",
    "Type": {
      "Name": "string"
    }
  }
]
```

## <a name="configuration-file-example"></a>Voorbeeld van een configuratie

Overdracht van de Asset is een intelligente contract scenario voor kopen en verkopen hoogwaardige middelen waarvoor een inspector en appraiser. Verkopers kunnen hun activa weergeven door het instantiëren van een asset overdracht smart contract. Kopers aanbiedingen door middel van een actie voor de smartcard contract kunnen maken en andere partijen kunnen acties controleren of de asset beoordelen uitvoeren. Zodra de asset is gemarkeerd als beide geïnspecteerd en beoordeeld, koper en verkoper wordt Bevestig de verkoop opnieuw voordat u het contract is ingesteld om te voltooien. Alle deelnemers hebben zichtbaarheid van de status van het contract voor elk punt in het proces, wanneer deze wordt bijgewerkt. 

Zie voor meer informatie, inclusief de codebestanden [asset overdracht voorbeeld voor Azure Blockchain Workbench](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer)

Het volgende configuratiebestand is voor de overdracht asset:

``` json
{
  "ApplicationName": "AssetTransfer",
  "DisplayName": "Asset Transfer",
  "Description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
  "ApplicationRoles": [
    {
      "Name": "Appraiser",
      "Description": "User that signs off on the asset price"
    },
    {
      "Name": "Buyer",
      "Description": "User that places an offer on an asset"
    },
    {
      "Name": "Inspector",
      "Description": "User that inspects the asset and signs off on inspection"
    },
    {
      "Name": "Owner",
      "Description": "User that signs off on the asset price"
    }
  ],
  "Workflows": [
    {
      "Name": "AssetTransfer",
      "DisplayName": "Asset Transfer",
      "Description": "Handles the business logic for the asset transfer scenario",
      "Initiators": [ "Owner" ],
      "StartState":  "Active",
      "Properties": [
        {
          "Name": "State",
          "DisplayName": "State",
          "Description": "Holds the state of the contract",
          "Type": {
            "Name": "state"
          }
        },
        {
          "Name": "Description",
          "DisplayName": "Description",
          "Description": "Describes the asset being sold",
          "Type": {
            "Name": "string"
          }
        },
        {
          "Name": "AskingPrice",
          "DisplayName": "Asking Price",
          "Description": "The asking price for the asset",
          "Type": {
            "Name": "money"
          }
        },
        {
          "Name": "OfferPrice",
          "DisplayName": "Offer Price",
          "Description": "The price being offered for the asset",
          "Type": {
            "Name": "money"
          }
        },
        {
          "Name": "InstanceAppraiser",
          "DisplayName": "Instance Appraiser",
          "Description": "The user that appraises the asset",
          "Type": {
            "Name": "Appraiser"
          }
        },
        {
          "Name": "InstanceBuyer",
          "DisplayName": "Instance Buyer",
          "Description": "The user that places an offer for this asset",
          "Type": {
            "Name": "Buyer"
          }
        },
        {
          "Name": "InstanceInspector",
          "DisplayName": "Instance Inspector",
          "Description": "The user that inspects this asset",
          "Type": {
            "Name": "Inspector"
          }
        },
        {
          "Name": "InstanceOwner",
          "DisplayName": "Instance Owner",
          "Description": "The seller of this particular asset",
          "Type": {
            "Name": "Owner"
          }
        }
      ],
      "Constructor": {
        "Parameters": [
          {
            "Name": "description",
            "Description": "The description of this asset",
            "DisplayName": "Description",
            "Type": {
              "Name": "string"
            }
          },
          {
            "Name": "price",
            "Description": "The price of this asset",
            "DisplayName": "Price",
            "Type": {
              "Name": "money"
            }
          }
        ]
      },
      "Functions": [
        {
          "Name": "Modify",
          "DisplayName": "Modify",
          "Description": "Modify the description/price attributes of this asset transfer instance",
          "Parameters": [
            {
              "Name": "description",
              "Description": "The new description of the asset",
              "DisplayName": "Description",
              "Type": {
                "Name": "string"
              }
            },
            {
              "Name": "price",
              "Description": "The new price of the asset",
              "DisplayName": "Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Terminate",
          "DisplayName": "Terminate",
          "Description": "Used to cancel this particular instance of asset transfer",
          "Parameters": []
        },
        {
          "Name": "MakeOffer",
          "DisplayName": "Make Offer",
          "Description": "Place an offer for this asset",
          "Parameters": [
            {
              "Name": "inspector",
              "Description": "Specify a user to inspect this asset",
              "DisplayName": "Inspector",
              "Type": {
                "Name": "Inspector"
              }
            },
            {
              "Name": "appraiser",
              "Description": "Specify a user to appraise this asset",
              "DisplayName": "Appraiser",
              "Type": {
                "Name": "Appraiser"
              }
            },
            {
              "Name": "offerPrice",
              "Description": "Specify your offer price for this asset",
              "DisplayName": "Offer Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Reject",
          "DisplayName": "Reject",
          "Description": "Reject the user's offer",
          "Parameters": []
        },
        {
          "Name": "AcceptOffer",
          "DisplayName": "Accept Offer",
          "Description": "Accept the user's offer",
          "Parameters": []
        },
        {
          "Name": "RescindOffer",
          "DisplayName": "Rescind Offer",
          "Description": "Rescind your placed offer",
          "Parameters": []
        },
        {
          "Name": "ModifyOffer",
          "DisplayName": "Modify Offer",
          "Description": "Modify the price of your placed offer",
          "Parameters": [
            {
              "Name": "offerPrice",
              "DisplayName": "Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Accept",
          "DisplayName": "Accept",
          "Description": "Accept the inspection/appraisal results",
          "Parameters": []
        },
        {
          "Name": "MarkInspected",
          "DisplayName": "Mark Inspected",
          "Description": "Mark the asset as inspected",
          "Parameters": []
        },
        {
          "Name": "MarkAppraised",
          "DisplayName": "Mark Appraised",
          "Description": "Mark the asset as appraised",
          "Parameters": []
        }
      ],
      "States": [
        {
          "Name": "Active",
          "DisplayName": "Active",
          "Description": "The initial state of the asset transfer workflow",
          "PercentComplete": 20,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancels this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate Offer"
            },
            {
              "AllowedRoles": [ "Buyer" ],
              "AllowedInstanceRoles": [],
              "Description": "Make an offer for this asset",
              "Function": "MakeOffer",
              "NextStates": [ "OfferPlaced" ],
              "DisplayName": "Make Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Modify attributes of this asset transfer instance",
              "Function": "Modify",
              "NextStates": [ "Active" ],
              "DisplayName": "Modify"
            }
          ]
        },
        {
          "Name": "OfferPlaced",
          "DisplayName": "Offer Placed",
          "Description": "Offer has been placed for the asset",
          "PercentComplete": 30,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Accept the proposed offer for the asset",
              "Function": "AcceptOffer",
              "NextStates": [ "PendingInspection" ],
              "DisplayName": "Accept Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you previously placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Modify the price that you specified for your offer",
              "Function": "ModifyOffer",
              "NextStates": [ "OfferPlaced" ],
              "DisplayName": "Modify Offer"
            }
          ]
        },
        {
          "Name": "PendingInspection",
          "DisplayName": "Pending Inspection",
          "Description": "Asset is pending inspection",
          "PercentComplete": 40,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceInspector" ],
              "Description": "Mark this asset as inspected",
              "Function": "MarkInspected",
              "NextStates": [ "Inspected" ],
              "DisplayName": "Mark Inspected"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceAppraiser" ],
              "Description": "Mark this asset as appraised",
              "Function": "MarkAppraised",
              "NextStates": [ "Appraised" ],
              "DisplayName": "Mark Appraised"
            }
          ]
        },
        {
          "Name": "Inspected",
          "DisplayName": "Inspected",
          "PercentComplete": 45,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceAppraiser" ],
              "Description": "Mark this asset as appraised",
              "Function": "MarkAppraised",
              "NextStates": [ "NotionalAcceptance" ],
              "DisplayName": "Mark Appraised"
            }
          ]
        },
        {
          "Name": "Appraised",
          "DisplayName": "Appraised",
          "Description": "Asset has been appraised, now awaiting inspection",
          "PercentComplete": 45,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceInspector" ],
              "Description": "Mark the asset as inspected",
              "Function": "MarkInspected",
              "NextStates": [ "NotionalAcceptance" ],
              "DisplayName": "Mark Inspected"
            }
          ]
        },
        {
          "Name": "NotionalAcceptance",
          "DisplayName": "Notional Acceptance",
          "Description": "Asset has been inspected and appraised, awaiting final sign-off from buyer and seller",
          "PercentComplete": 50,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "SellerAccepted" ],
              "DisplayName": "SellerAccept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "BuyerAccepted" ],
              "DisplayName": "BuyerAccept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            }
          ]
        },
        {
          "Name": "BuyerAccepted",
          "DisplayName": "Buyer Accepted",
          "Description": "Buyer has signed-off on inspection and appraisal",
          "PercentComplete": 75,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "SellerAccepted" ],
              "DisplayName": "Accept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            }
          ]
        },
        {
          "Name": "SellerAccepted",
          "DisplayName": "Seller Accepted",
          "Description": "Seller has signed-off on inspection and appraisal",
          "PercentComplete": 75,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "Accepted" ],
              "DisplayName": "Accept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            }
          ]
        },
        {
          "Name": "Accepted",
          "DisplayName": "Accepted",
          "Description": "Asset transfer process is complete",
          "PercentComplete": 100,
          "Style": "Success",
          "Transitions": []
        },
        {
          "Name": "Terminated",
          "DisplayName": "Terminated",
          "Description": "Asset transfer has been cancelled",
          "PercentComplete": 100,
          "Style": "Failure",
          "Transitions": []
        }
      ]
    }
  ]
}
```
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over Azure Blockchain Workbench REST-API](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)

