---
title: SaaS vervulling-API V2 - Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd hoe u een SaaS-aanbieding op Azure Marketplace, met behulp van de bijbehorende uitvoering V2 API's maken.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 02/27/2019
ms.author: pbutlerm
ms.openlocfilehash: 7e1fa82e71738ac113bfe748a7117d0c3a10b14b
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57318759"
---
# <a name="saas-fulfillment-api-version-2"></a>SaaS vervulling API-versie 2

Dit artikel worden de API waarmee u independent software vendors (ISV) hun SaaS-toepassingen integreren met de Azure Marketplace. Deze API maakt het ISV-toepassingen om deel te nemen in alle commerce ingeschakeld kanalen: direct, door partner geleide (reseller) en onder leiding van een veld.  Deze API is een vereiste voor aanbieding dat transactable SaaS-aanbiedingen op Azure Marketplace.


## <a name="managing-the-saas-subscription-lifecycle"></a>De levenscyclus van de SaaS-abonnementen beheren

Microsoft SaaS-Service beheert de gehele levenscyclus van de aankoop van een SaaS-abonnement en de vervulling-API gebruikt een mechanisme voor het station van de daadwerkelijke uitvoering wijzigingen te plannen en verwijderen van het abonnement met de ISV. De klant, wordt in rekening gebracht op basis van de status van de SaaS-abonnement die Microsoft onderhoudt. Het volgende diagram ziet u de Staten en de bewerkingen die de wijzigingen tussen de Staten station.

![Levenscyclus voor SaaS abonnementsstatussen](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Status van een SaaS-abonnement

De volgende tabel bevat de inrichting statussen voor een SaaS-abonnement, met inbegrip van een diagram beschrijving en volgorde voor elk (indien van toepassing). 

#### <a name="provisioning"></a>Inrichten

Wanneer een klant een aankoop initieert, ontvangt de ISV deze informatie in een AuthCode op klant interactieve webpagina's met behulp van een URL-parameter. De AuthCode kan worden gevalideerd en die worden uitgewisseld voor de details van wat er moet worden ingericht.  Wanneer de SaaS-service is voltooid wordt ingericht, stuurt een aanroep activeren om aan te geven dat de uitvoering voltooid is en de klant kan worden gefactureerd.  Het volgende diagram toont de volgorde van de API-aanroepen voor een scenario voor het inrichten.  

![API-aanroepen voor het inrichten van een SaaS-service.](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Ingericht

Deze status is de actieve status van een ingerichte service.

#### <a name="provisioning-for-update"></a>Inrichting voor update
(van marketplace) 

Deze status vertegenwoordigt een update aan een bestaande service is in behandeling. Deze update kan worden gestart door de klant op de marketplace, of op de SaaS-service (alleen voor het rechtstreeks naar de klanttransacties.) Het volgende diagram toont de acties wanneer een update wordt gestart vanuit de marketplace.

![API-aanroepen wanneer de update wordt gestart vanuit de marketplace.](./media/saas-update-api-v2-calls-from-marketplace-a.png)

#### <a name="provisioning-for-update"></a>Inrichting voor update  
(van SaaS-service) 

Het volgende diagram toont de acties wanneer een update wordt geïnitieerd door de SaaS-service. (De webhook-aanroep wordt vervangen door een update aan voor het abonnement dat is gestart door de SaaS-Service. 

![API-aanroepen wanneer de update wordt geïnitieerd door de SaaS-service.](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Suspended

Deze status geeft aan dat de betaling van een klant is niet ontvangen. Door het beleid bieden we de klant een respijtperiode voor het abonnement unfulfilling. Een abonnement heeft wanneer deze status: 

- Als een ISV besluiten kunt u te verlagen of te blokkeren van de gebruiker toegang tot de service. 
- Het abonnement moet worden bewaard in een herstelbare staat die u kunt de volledige functionaliteit zonder verlies van gegevens of -instellingen herstellen. 
- U kunt verwachten dat voor het verkrijgen van een aanvraag reactiveren voor dit abonnement via de vervulling API of een aanvraag voor ongedaan maken inrichting aan het einde van de respijtperiode. 

#### <a name="unsubscribed"></a>Afgemeld 

Abonnementen contact opnemen met deze status bij een expliciete klantaanvraag of als reactie op niet-betaling van bijdragen. De verwachting van de ISV is dat de gegevens van de klant voor herstel op aanvraag voor een minimum van X dagen behouden en vervolgens verwijderd. 

## <a name="api-reference"></a>API-verwijzing

In deze sectie worden de SaaS *abonnement API* en *Operations API*.


### <a name="subscription-api"></a>Abonnement API

Het abonnement API ondersteunt de volgende bewerkingen uit HTTPS: **Ophalen**, **Post**, **Patch**, en **verwijderen**.

#### <a name="list-subscriptions"></a>Een lijst met abonnementen maken

Geeft een lijst van de SaaS-abonnementen voor een uitgever.

**Toevoegen:<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`**

*Queryparameters:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  De versie van de bewerking te gebruiken voor deze aanvraag.  |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
| Content-Type       |  `application/json`  |
| x-ms-requestid     |  De waarde van de unieke tekenreeks voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| x-ms-correlationid |  De waarde van de unieke tekenreeks voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
| Autorisatie      |  De JSON web token (JWT) bearer-token.  |

*Responscodes:*

Code: 200<br>
Op basis van het token get auth de uitgever en de bijbehorende abonnementen voor aanbiedingen van de uitgever.<br> Payload van he antwoord:<br>

```json
{
  "subscriptions": [
      {
          "id": "",
          "name": "CloudEndure for Production use",
          "publisherId": "cloudendure",
          "offerId": "ce-dr-tier2",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "cc906b16-1991-4b6d-a5a4-34c66a5202d7"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "0396833b-87bf-4f31-b81c-c67f88973512"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "status": "Subscribed" // Indicates the status of the operation. [Provisioning, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```

Code: 403 <br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken. 

Code: 500 Interne serverfout

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="get-subscription"></a>Abonnement nemen

Hiermee haalt u het opgegeven SaaS-abonnement. Gebruik deze aanroep voor het ophalen van de licentie-informatie en informatie van plan bent.

**Toevoegen:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId?api-version=<ApiVersion>`**

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   De unieke id van de SaaS-abonnement dat verkregen na het oplossen van het token via API oplossen   |
|  ApiVersion        |   Versie van de bewerking te gebruiken voor deze aanvraag   |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`  |
|  x-ms-requestid    |  De waarde van de unieke tekenreeks voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  x-ms-correlationid |  De waarde van de unieke tekenreeks voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  Autorisatie     |  JSON web token (JWT) bearer-token  |

*Responscodes:*

Code: 200<br>
Saas-abonnement uit-id opgehaald<br> Payload van he antwoord:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"CloudEndure for Production use",
        "publisherId": "cloudendure",
        "offerId": "ce-dr-tier2",
        "planId": "silver",
        "quantity": "10"",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "cc906b16-1991-4b6d-a5a4-34c66a5202d7"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "0396833b-87bf-4f31-b81c-c67f88973512"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "status": "Subscribed", // Indicates the status of the operation.
}
```

Code: 404<br>
Niet gevonden<br> 

Code: 403<br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken.

Code: 500<br>
Interne serverfout<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Lijst met beschikbare abonnementen

Gebruik deze aanroep om erachter te komen of er geen persoonlijke/openbare aanbiedingen voor de huidige gebruiker zijn.

**Toevoegen:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`**

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   Versie van de bewerking te gebruiken voor deze aanvraag  |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|   x-ms-requestid   |   De waarde van de unieke tekenreeks voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  x-ms-correlationid  | De waarde van de unieke tekenreeks voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  Autorisatie     |  JSON web token (JWT) bearer-token |

*Responscodes:*

Code: 200<br>
Een lijst met beschikbare abonnementen ophalen voor een klant.<br>

```json
Response Body:
[{
    "planId": "silver",
    "displayName": "Silver",
    "isPrivate": false
},
{
    "planId": "silver-private",
    "displayName": "Silver-private",
    "isPrivate": true
}]
```

Code: 404<br>
Niet gevonden<br> 

Code: 403<br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken. <br> 

Code: 500<br>
Interne serverfout<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="resolve-a-subscription"></a>Een abonnement op te lossen 

Het eindpunt oplossen kan gebruikers een marketplace-token worden omgezet in een permanente Resource-ID. De Resource-ID is de unieke id voor SAAS-abonnement.  Wanneer een gebruiker wordt omgeleid naar de website van een ISV, bevat de URL een token in de queryparameters. De ISV wordt verwacht voor gebruik van dit token, en maken van een aanvraag om dit te verhelpen. Het antwoord bevat de unieke SAAS abonnements-ID, de naam, de aanbiedings-ID en het plan voor de resource. Dit token is alleen een uur geldig. 

**Verzenden:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`**

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versie van de bewerking te gebruiken voor deze aanvraag  |

*Aanvraagheaders:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |  De waarde van de unieke tekenreeks voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  x-ms-correlationid |  De waarde van de unieke tekenreeks voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  Autorisatie     |  JSON web token (JWT) bearer-token  |
|  x-ms-marketplace-token  |  Token queryparameter in de URL in wanneer de gebruiker wordt omgeleid naar de SaaS-ISV-website van Azure. *Opmerking:* Het wordt door URL gedecodeerd waarde vanuit de browser voor het token voordat u deze gebruikt. |

*Responscodes:*

Code: 200<br>
De ondoorzichtige token worden omgezet in een SaaS-abonnement.<br>

```json
Response body:
{
    "subscriptionId": "cd9c6a3a-7576-49f2-b27e-1e5136e57f45",  
    "subscriptionName": "My Saas application",
    "offerId": "ce-dr-tier2",
    "planId": "silver",
    "quantity": "20",
    "operationId": " be750acb-00aa-4a02-86bc-476cbe66d7fa"  
}
```

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten

Code: 403<br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken.

Code: 500<br>
Interne serverfout

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="activate-a-subscription"></a>Een abonnement activeren

**Verzenden:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`**

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versie van de bewerking te gebruiken voor deze aanvraag  |
| subscriptionId     | De unieke id van het SaaS-abonnement dat verkregen na het oplossen van het token met behulp van de API oplossen  |

*Aanvraagheaders:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json`  |
|  x-ms-requestid    | De waarde van de unieke tekenreeks voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  x-ms-correlationid  | De waarde van de unieke tekenreeks voor de bewerking op de client. Dit komt overeen met alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  Autorisatie     |  JSON web token (JWT) bearer-token |

*Aanvraag:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Responscodes:*

Code: 202<br>
Hiermee activeert u het abonnement.<br>

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten

Code: 403<br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken.

Code: 500<br>
Interne serverfout

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="update-a-subscription"></a>Een abonnement bijwerken

Bijwerken of wijzigen van een abonnement wilt met de opgegeven waarden.

**Patch voor:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`**

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking te gebruiken voor deze aanvraag.  |
| subscriptionId     | De unieke id van de SaaS-abonnement dat na het oplossen van het token met behulp van de API kunt oplossen door wordt verkregen.  |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  x-ms-correlationid  |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.    |
| Autorisatie      |  De JSON web token (JWT) bearer-token.  |

*De nettolading van de aanvraag:*

```json
Request Body:
{
    "planId": "gold",
    "quantity": ""
}
```

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Koppelen aan een resource om op te halen van de status van de bewerking.   |

*Responscodes:*

Code: 202<br>
ISV initieert een wijzigingsplan of een hoeveelheid wijzigen. <br>

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten.

>[!Note]
>Alleen een abonnement of de hoeveelheid kan worden patches in één keer, niet beide. Bewerkingen op een abonnement met **Update** zich niet in `allowedCustomerOperations`.

Code: 403<br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken.

Code: 500<br>
Interne serverfout

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="delete-a-subscription"></a>Een abonnement verwijderen

Afmelden en verwijderen van het opgegeven abonnement.

**Verwijderen:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId> ?api-version=<ApiVersion>`**

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking te gebruiken voor deze aanvraag.  |
| subscriptionId     | De unieke id van de SaaS-abonnement dat na het oplossen van het token met behulp van de API kunt oplossen door wordt verkregen.  |

*Aanvraagheaders:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.   |
|  x-ms-correlationid  |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.   |
|  Autorisatie     |  De JSON web token (JWT) bearer-token.   |

*Responscodes:*

Code: 200<br>
ISV gestart aanroep om aan te geven zich afmelden op een SaaS-abonnement.<br>

Code: 404<br>
Niet gevonden

Code: 400<br>
Verwijderen van een abonnement met **verwijderen** niet in `allowedCustomerOperations`.

Code: 403<br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken.

Code: 500<br>
Interne serverfout

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```


### <a name="operations-api"></a>API-bewerkingen

De operations-API ondersteunt de volgende Patch en Get-bewerkingen.


#### <a name="update-a-subscription"></a>Een abonnement bijwerken

Een abonnement bijwerken met de opgegeven waarden.

**Patch voor:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operation/<operationId>?api-version=<ApiVersion>`**

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  De versie van de bewerking te gebruiken voor deze aanvraag.  |
| subscriptionId     | De unieke id van de SaaS-abonnement dat na het oplossen van het token met behulp van de API kunt oplossen door wordt verkregen.  |
|  operationId       | De bewerking die wordt uitgevoerd. |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     | `application/json`   |
|   x-ms-requestid   |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  x-ms-correlationid |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  Autorisatie     |  De JSON web token (JWT) bearer-token.  |

*De nettolading van de aanvraag:*

```json
{
    "planId": "",
    "quantity": "",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}
```

*Responscodes:*

Code: 200<br> Aanroepen voor het op de hoogte van een bewerking aan de ISV is voltooid. Dit kan bijvoorbeeld worden gewijzigd van seats/abonnementen.

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten

Code: 403<br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken.

Code: 409<br>
Een conflict. Bijvoorbeeld, is een nieuwe transactie al voldaan

Code: 500<br> Interne serverfout

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="list-outstanding-operations"></a>Lijst met openstaande bewerkingen 

Geeft een lijst van de openstaande bewerkingen voor de huidige gebruiker. 

**Toevoegen:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`**

*Queryparameters:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   De versie van de bewerking te gebruiken voor deze aanvraag.                |
| subscriptionId     | De unieke id van de SaaS-abonnement dat na het oplossen van het token met behulp van de API kunt oplossen door wordt verkregen.  |

*Aanvraagheaders:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |  Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  x-ms-correlationid |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  Autorisatie     |  De JSON web token (JWT) bearer-token.  |

*Responscodes:*

Code: 200<br> Hiermee haalt u de lijst met bewerkingen op een abonnement in behandeling.<br>
Payload van he antwoord:

```json
[{
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",  
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId": "cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "ce-dr-tier2",
    "publisherId": "cloudendure",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",  
    "status": "NotStarted"  
}]
```

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten

Code: 403<br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken.

Code: 500<br>
Interne serverfout

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>Bewerkingsstatus ophalen

Kan de gebruiker voor het bijhouden van de status van een geactiveerde asynchrone bewerking (aanmelden/afmelden/wijzigen-plan).

**Toevoegen:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`**

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking te gebruiken voor deze aanvraag.  |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  ` application/json`   |
|  x-ms-requestid    |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  x-ms-correlationid |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  Autorisatie     | De JSON web token (JWT) bearer-token.  |

*Responscodes:* Code: 200<br> Hiermee haalt u de lijst van alle in behandeling SaaS-bewerkingen<br>
Payload van he antwoord:

```json
Response body:
[{
    "id  ": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "ce-dr-tier2",
    "publisherId": "cloudendure",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}]

```

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten

Code: 403<br>
Niet gemachtigd. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag is het openen van een verzameling die geen deel van de huidige gebruiker uitmaken.
 
Code: 500<br> Interne serverfout

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="webhook-on-the-saas-service"></a>Webhook in de SaaS-service

De uitgever moet een webhook in deze SaaS-service om proactief te waarschuwen gebruikers van wijzigingen in de service implementeren. De API verwacht wordt niet-geverifieerde en wordt aangeroepen door de Microsoft SaaS-service. De SaaS-service wordt verwacht voor het aanroepen van de operations-API om te verifiëren en autoriseren voordat een actie wordt ondernomen op de webhook-melding.

```json
{
    "operationId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "offerId": "ce-dr-tier2",
    "publisherId": "cloudendure",
    "planId": "silver",
    "quantity": "20"  ,
    "action": "Activate",   // Activate/Delete/Suspend/Reinstate/Change[new]  
    "timeStamp": "2018-12-01T00:00:00"
}

```

<!-- Review following, might not be needed when this publishes -->


## <a name="mock-api"></a>Mock API

U kunt onze mock API's gebruiken om u aan de slag met ontwikkelen, met name ontwikkelen van prototypen en testen projecten te helpen. 

Eindpunt voor de host: https://marketplaceapi.microsoft.com/api API-versie: 2018-09-15 geen verificatie vereist voorbeeld Uri: https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15

Een van de API-aanroepen in dit artikel kan worden gemaakt met het eindpunt mock-host. U kunt verwachten mock-gegevens weer als een antwoord op te halen.


## <a name="next-steps"></a>Volgende stappen

Ontwikkelaars kunnen ook programmatisch ophalen en manipuleren van werkbelastingen, aanbiedingen en uitgever profielen met behulp van de [Cloud Partner Portal REST-API's](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
