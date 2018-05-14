---
title: Naslaginformatie over Azure Blockchain Workbench configuratie
description: Blockchain Workbench toepassing configuratie overzicht van Azure.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 3/20/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 6318b6aafda972242239ce91abb0f67a15721e1e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
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
| AllowedInstanceRoles | Lijst met gebruikersrollen deelnemende of opgegeven in de smartcard contract mag initiëren van de overgang. Rolinstanties zijn gedefinieerd in **eigenschappen** in werkstromen. Deze gebruikers vertegenwoordigen de deelname van een gebruiker of opgegeven in het smart contract in plaats van alle gebruikers van het type van een rol. | Nee |
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

Het volgende voorbeeld definieert een basic aanvragen en antwoorden toepassing waarin een aanvrager verzendt dat een aanvraag- en een eindpunt een antwoord op de aanvraag te verzenden.

``` json
{
  "ApplicationName": "HelloBlockchain",
  "DisplayName": "Hello, Blockchain!",
  "Description": "A simple application to send request and get response",
  "ApplicationRoles": [
    {
      "Name": "Requestor",
      "Description": "A person sending a request."
    },
    {
      "Name": "Responder",
      "Description": "A person responding to a request"
    }
  ],
  "Workflows": [
    {
      "Name": "RequestResponse",
      "DisplayName": "Request Response",
      "Description": "A simple workflow to send a request and receive a response.",
      "Initiators": [ "Requestor" ],
      "StartState": "Request",
      "Properties": [
        {
          "Name": "State",
          "DisplayName": "State",
          "Description": "Holds the state of the contract.",
          "Type": {
            "Name": "state"
          }
        },
        {
          "Name": "Requestor",
          "DisplayName": "Requestor",
          "Description": "A person sending a request.",
          "Type": {
            "Name": "Requestor"
          }
        },
        {
          "Name": "Responder",
          "DisplayName": "Responder",
          "Description": "A person sending a response.",
          "Type": {
            "Name": "Responder"
          }
        },
        {
          "Name": "RequestMessage",
          "DisplayName": "Request Message",
          "Description": "A request message.",
          "Type": {
            "Name": "string"
          }
        },
        {
          "Name": "ResponseMessage",
          "DisplayName": "Response Message",
          "Description": "A response message.",
          "Type": {
            "Name": "string"
          }
        }
      ],
      "Constructor": {
        "Parameters": [
          {
            "Name": "message",
            "Description": "...",
            "DisplayName": "Request Message",
            "Type": {
              "Name": "string"
            }
          }
        ]
      },
      "Functions": [
        {
          "Name": "SendRequest",
          "DisplayName": "Request",
          "Description": "...",
          "Parameters": [
            {
              "Name": "requestMessage",
              "Description": "...",
              "DisplayName": "Request Message",
              "Type": {
                "Name": "string"
              }
            }
          ]
        },
        {
          "Name": "SendResponse",
          "DisplayName": "Response",
          "Description": "...",
          "Parameters": [
            {
              "Name": "responseMessage",
              "Description": "...",
              "DisplayName": "Response Message",
              "Type": {
                "Name": "string"
              }
            }
          ]
        }
      ],
      "States": [
        {
          "Name": "Request",
          "DisplayName": "Request",
          "Description": "...",
          "PercentComplete": 50,
          "Value": 0,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": ["Responder"],
              "AllowedInstanceRoles": [],
              "Description": "...",
              "Function": "SendResponse",
              "NextStates": [ "Respond" ],
              "DisplayName": "Send Response"
            }
          ]
        },
        {
          "Name": "Respond",
          "DisplayName": "Respond",
          "Description": "...",
          "PercentComplete": 90,
          "Value": 1,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": ["Requestor"],
              "Description": "...",
              "Function": "SendRequest",
              "NextStates": [ "Request" ],
              "DisplayName": "Send Request"
            }
          ]
        }
      ]
    }
  ]
}
```
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench implementeren](blockchain-workbench-deploy.md)

