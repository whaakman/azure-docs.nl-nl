---
title: Naslaginformatie over Azure Blockchain Workbench-configuratie
description: Azure Blockchain Workbench toepassing configuratie overzicht.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 7/12/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 60a84609c6ec8c1733f0938c69ab683f01ecb975
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224531"
---
# <a name="azure-blockchain-workbench-configuration-reference"></a>Naslaginformatie over Azure Blockchain Workbench-configuratie

 Azure Blockchain Workbench-toepassingen zijn meerdere partijen werkstromen die zijn gedefinieerd door de metagegevens van de configuratie en slimme Contractcode. Metagegevens van de configuratie definieert de werkstromen op hoog niveau en het model van de interactie van de blockchain-toepassing. Slimme contracten definiëren de bedrijfslogica van de blockchain-toepassing. Workbench maakt gebruik van configuratie- en slimme Contractcode voor het genereren van gebruikerservaringen blockchain-toepassingen.

Metagegevens van de configuratie bevat de volgende informatie voor elke toepassing blockchain: 

* Naam en beschrijving van de blockchain-toepassing
* De unieke functies voor gebruikers die kunnen optreden of nemen in de blockchain-toepassing
* Een of meer werkstromen. Elke werkstroom fungeert als een toestandsmachine voor het beheren van de stroom van de bedrijfslogica. Werkstromen kunnen onafhankelijk of communiceren met elkaar.

Elke gedefinieerde werkstroom bevat het volgende:

* Naam en beschrijving van de werkstroom
* Status van de werkstroom.  Elke status is een fase in de Controlestroom van de bedrijfslogica. 
* Overgang naar de status van de volgende acties
* Gebruikersrollen die zijn toegestaan voor elke actie starten
* Slimme contracten die staan voor zakelijke logica in codebestanden

## <a name="application"></a>Toepassing

Een blockchain-toepassing bevat configuratie-metagegevens, werkstromen en gebruiker rollen die kunnen optreden of nemen in de toepassing.

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| ApplicationName | De unieke toepassingsnaam. Het bijbehorende slimme contract moet gebruiken dezelfde **ApplicationName** voor de contractklasse van toepassing.  | Ja |
| Weergavenaam | Beschrijvende weergavenaam van de toepassing. | Ja |
| Beschrijving | Beschrijving van de toepassing. | Nee |
| ApplicationRoles | Verzameling van [ApplicationRoles](#application-roles). Gebruikersrollen die kunnen optreden of nemen in de toepassing.  | Ja |
| Werkstromen | Verzameling van [werkstromen](#workflows). Elke werkstroom fungeert als een toestandsmachine voor het beheren van de stroom van de bedrijfslogica. | Ja |

Zie voor een voorbeeld [voorbeeldconfiguratiebestand](#configuration-file-example).

## <a name="workflows"></a>Werkstromen

De bedrijfslogica van een toepassing kan worden gezien als een toestandsmachine waar een actie zorgt ervoor dat de stroom van de bedrijfslogica van een status naar de andere verplaatsen. Een werkstroom is een verzameling van dergelijke Staten en acties. Elke werkstroom bestaat uit een of meer slimme contracten die de bedrijfslogica in codebestanden vertegenwoordigen. Een uitvoerbaar bestand contract is een exemplaar van een werkstroom.

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | Naam van de unieke werkstroom. Het bijbehorende slimme contract moet gebruiken dezelfde **naam** voor de contractklasse van toepassing. | Ja |
| Weergavenaam | Beschrijvende weergavenaam van de werkstroom. | Ja |
| Beschrijving | Beschrijving van de werkstroom. | Nee |
| Initiators | Verzameling van [ApplicationRoles](#application-roles). Functies die zijn toegewezen aan gebruikers die zijn gemachtigd voor het maken van overeenkomsten in de werkstroom. | Ja |
| StartState | De naam van de beginstatus van de werkstroom. | Ja |
| Eigenschappen | Verzameling van [id's](#identifiers). Vertegenwoordigt gegevens die kunnen worden gelezen buiten de chain of zinvoller visualiseren in een gebruiker zich hulpprogramma. | Ja |
| Constructor | Hiermee definieert u invoerparameters die zijn opgegeven voor het maken van een exemplaar van de werkstroom. | Ja |
| Functions | Een verzameling van [functies](#functions) die kan worden uitgevoerd in de werkstroom. | Ja |
| Staten | Een verzameling van werkstroom [Staten](#states). | Ja |

Zie voor een voorbeeld [voorbeeldconfiguratiebestand](#configuration-file-example).

## <a name="type"></a>Type

Ondersteunde gegevenstypen.

| Type | Beschrijving |
|-------|-------------|
| Adres  | Blockchain-adrestype, zoals *contracten* of *gebruikers* |
| BOOL     | Boole-gegevenstype |
| contract | Adres van het type contract |
| Enum     | Geïnventariseerde set benoemde waarden. Wanneer u het type enum, opgeven u ook een lijst met EnumValues. Elke waarde is beperkt tot 255 tekens. Geldige waarde tekens bevatten hoofdletters en kleine letters (A-Z, a-z) letters en getallen (0-9). |
| int      | Het gegevenstype geheel getal zijn |
| geld    | Money-gegevenstype |
| state    | Werkstroomstatus |
| tekenreeks   | Het gegevenstype String |
| Gebruiker     | Adres van het type gebruiker |
| tijd     | Tijd gegevenstype |
|`[ Application Role Name ]`| Een naam die is opgegeven in de toepassingsrol. Gebruikers van dat Roltype beperkt. |

### <a name="example-configuration-of-type-string"></a>Van de voorbeeldconfiguratie van het typetekenreeks

``` json
{
  "Name": "description",
  "Description": "Descriptive text",
  "DisplayName": "Description",
  "Type": {
    "Name": "string"
  }
}
```

### <a name="example-configuration-of-type-enum"></a>Van de voorbeeldconfiguratie van het type enum

``` json
{
  "Name": "PropertyType",
  "DisplayName": "Property Type",
  "Description": "The type of the property",
  "Type": {
    "Name": "enum",
    "EnumValues": ["House", "Townhouse", "Condo", "Land"]
  }
}
```

#### <a name="using-enumeration-type-in-solidity"></a>Met behulp van opsommingstype in Solidity

Zodra een enum-waarde is gedefinieerd in de configuratie, kunt u Opsommingstypen in Solidity. U kunt bijvoorbeeld een enum-waarde met de naam PropertyTypeEnum definiëren.

```
enum PropertyTypeEnum {House, Townhouse, Condo, Land} PropertyTypeEnum public PropertyType; 
```

De lijst met tekenreeksen moet overeenkomen met de configuratie en slimme contract moet een geldig en consistente declaraties in Blockchain Workbench.

Voorbeeld van een toewijzing:

```
PropertyType = PropertyTypeEnum.Townhouse;
```

Voorbeeld van de parameter functie: 

``` 
function AssetTransfer(string description, uint256 price, PropertyTypeEnum propertyType) public
{
    InstanceOwner = msg.sender;
    AskingPrice = price;
    Description = description;
    PropertyType = propertyType;
    State = StateType.Active;
    ContractCreated();
}

```

## <a name="constructor"></a>Constructor

Hiermee definieert u invoerparameters voor een exemplaar van een werkstroom.

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Parameters | Verzameling van [id's](#identifiers) vereist voor het initiëren van een slimme contract. | Ja |

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

Definieert de functies die kunnen worden uitgevoerd in de werkstroom.

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | De unieke naam van de functie. Het bijbehorende slimme contract moet gebruiken dezelfde **naam** voor de desbetreffende functie. | Ja |
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

## <a name="states"></a>Staten

Een verzameling van unieke statussen binnen een werkstroom. Elke status bevat een stap in de Controlestroom van de bedrijfslogica. 

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | De unieke naam van de status. Het bijbehorende slimme contract moet gebruiken dezelfde **naam** voor de status van de van toepassing. | Ja |
| Weergavenaam | Beschrijvende weergavenaam van de status. | Ja |
| Beschrijving | Beschrijving van de status. | Nee |
| PercentComplete | Een geheel getal-waarde die wordt weergegeven in de gebruikersinterface Blockchain Workbench om de voortgang in de Controlestroom van zakelijke logica weer te geven. | Ja |
| Stijl | Visuele hint die aangeeft of de status een staat slagen of mislukken vertegenwoordigt. Er zijn twee geldige waarden: `Success` of `Failure`. | Ja |
| Overgangen | Verzameling van beschikbare [overgangen](#transitions) van de huidige status van de Staten met de volgende. | Nee |

### <a name="states-example"></a>Voorbeeld van de Staten

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

Beschikbare acties op de volgende status. Een of meer gebruikersrollen kunnen een actie op elke status, waarbij een actie kan status niet laten overgaan een naar een andere staat in de werkstroom uitvoert. 

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| AllowedRoles | Lijst met toepassingen rollen mag starten van de overgang. Alle gebruikers van de opgegeven rol mogelijk de actie uit te voeren. | Nee |
| AllowedInstanceRoles | Lijst met gebruikersrollen deelnemende of zijn opgegeven in de slimme contract mag starten van de overgang. Rolinstanties zijn gedefinieerd in **eigenschappen** in werkstromen. AllowedInstanceRoles vertegenwoordigen een gebruiker die deel uitmaken van een exemplaar van een slimme contract. AllowedInstanceRoles bieden u de mogelijkheid om te beperken van een actie aan een gebruikersrol in een exemplaar van het contract.  U kunt bijvoorbeeld alleen zodat de gebruiker die het contract (InstanceOwner) kunnen in plaats van alle gebruikers in Roltype (eigenaar) is beëindigd als u de rol hebt opgegeven in AllowedRoles heeft gemaakt. | Nee |
| Weergavenaam | Beschrijvende weergavenaam van de overgang. | Ja |
| Beschrijving | Beschrijving van de overgang. | Nee |
| Functie | De naam van de functie voor het starten van de overgang. | Ja |
| NextStates | Een verzameling van mogelijke volgende statussen na een succesvolle overgang. | Ja |

### <a name="transitions-example"></a>Voorbeeld van de overgangen

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

Toepassingsrollen definiëren van een groep met de rollen die kunnen worden toegewezen aan gebruikers die u wilt uitvoeren of op te nemen in de toepassing. Toepassingsrollen kunnen worden gebruikt voor het beperken van acties en deel te nemen in de blockchain-toepassing en de bijbehorende werkstromen. 

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | De unieke naam van de toepassingsrol. Het bijbehorende slimme contract moet gebruiken dezelfde **naam** voor de rol die van toepassing. Basistype namen zijn gereserveerd. U kunt geen een toepassingsrol naam met dezelfde naam als [Type](#type)| Ja |
| Beschrijving | Beschrijving van de toepassingsrol. | Nee |

### <a name="application-roles-example"></a>Voorbeeld van de toepassing-rollen

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

Id's omvatten een verzameling van informatie die wordt gebruikt voor het beschrijven van werkstroomeigenschappen constructor en parameters van de functie. 

| Veld | Beschrijving | Vereist |
|-------|-------------|:--------:|
| Naam | De unieke naam van de eigenschap of de parameter. Het bijbehorende slimme contract moet gebruiken dezelfde **naam** voor de parameter of van toepassing op. | Ja |
| Weergavenaam | Beschrijvende weergavenaam voor de eigenschap of de parameter. | Ja |
| Beschrijving | Beschrijving van de eigenschap of de parameter. | Nee |

### <a name="identifiers-example"></a>Voorbeeld van de id 's

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

Asset-overdracht is een slimme contract scenario voor het kopen en verkopen van waardevolle activa, waarvoor een inspector en appraiser. Verkopers kunnen hun activa weergeven door het instantiëren van een asset overdracht slimme contract. Kopers kunnen aanbiedingen maken door een actie te ondernemen op de slimme contract en andere partijen kunnen ondernemen om te controleren of de asset te beoordelen. Zodra de asset is gemarkeerd als beide gecontroleerd en beoordeeld, de koper en verkoper bevestigt de verkoop opnieuw voordat het contract is ingesteld om te voltooien. Op elk punt in het proces hebben alle deelnemers inzicht in de status van het contract wanneer deze wordt bijgewerkt. 

Zie voor meer informatie, met inbegrip van de codebestanden [asset overdracht voorbeeld voor Azure Blockchain Workbench](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer)

Het volgende configuratiebestand is voor de overdracht asset voorbeeld:

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
> [Naslaginformatie over de REST-API van Azure Blockchain Workbench](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)

