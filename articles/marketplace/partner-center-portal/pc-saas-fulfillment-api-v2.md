---
title: SaaS vervulling API v2 | Azure Marketplace
description: Uitleg over het maken en beheren van een SaaS-aanbod voor de AppSource en de Azure Marketplace met behulp van de bijbehorende uitvoering v2 API's.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 05/23/2019
ms.author: evansma
ms.openlocfilehash: da23b90e44869dcbd21acf9b2c4e04f30153ae09
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66751772"
---
# <a name="saas-fulfillment-apis-version-2"></a>SaaS vervulling API's versie 2 

Dit artikel vindt u de API waarmee u onafhankelijke softwareleveranciers (ISV's) om hun SaaS-toepassingen te verkopen in de Azure Marketplace en AppSource. Deze API is dat een vereiste voor transactable SaaS biedt op de Azure Marketplace en AppSource.

## <a name="managing-the-saas-subscription-lifecycle"></a>De levenscyclus van de SaaS-abonnementen beheren

Microsoft SaaS-Service beheert de gehele levenscyclus van de aankoop van een SaaS-abonnement en de vervulling-API gebruikt een mechanisme voor het station van de daadwerkelijke uitvoering wijzigingen te plannen en verwijderen van het abonnement met de ISV. De klant, wordt in rekening gebracht op basis van de status van de SaaS-abonnement die Microsoft onderhoudt. Het volgende diagram ziet u de Staten en de bewerkingen die de wijzigingen tussen de Staten station.

![Levenscyclus voor SaaS abonnementsstatussen](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Status van een SaaS-abonnement

De volgende tabel bevat de inrichting statussen voor een SaaS-abonnement, met inbegrip van een diagram beschrijving en volgorde voor elk (indien van toepassing). 

#### <a name="provisioning"></a>Inrichten

Wanneer een klant een aankoop initieert, ontvangt de ISV deze informatie in een autorisatiecode op klant interactieve webpagina's met behulp van een URL-parameter. Bijvoorbeeld: `https://contoso.com/signup?token=..`, waarbij de provider landingspagina pagina-URL in partnercentrum `https://contoso.com/signup`. De autorisatiecode kan worden gevalideerd en uitgewisseld voor de details van wat er worden ingericht moet door het aanroepen van de API oplossen.  Wanneer de SaaS-service is voltooid wordt ingericht, stuurt een aanroep activeren om aan te geven dat de uitvoering voltooid is en de klant kan worden gefactureerd.  Het volgende diagram toont de volgorde van de API-aanroepen voor een scenario voor het inrichten.  

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

In deze sectie worden de SaaS *abonnement API* en *Operations API*.  De waarde van de `api-version` parameter voor versie 2-API's is `2018-08-31`.  


### <a name="parameter-and-entity-definitions"></a>Definities van parameter en de entiteit

De volgende tabel bevat de definities voor de algemene parameters en entiteiten die worden gebruikt door uitvoering API's.

|     Entity/Parameter     |     Definitie                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | GUID-id voor een SaaS-resource  |
| `name`                   | Beschrijvende naam voor deze resource is opgegeven door de klant |
| `publisherId`            | De unieke tekenreeks-id voor elke uitgever, bijvoorbeeld 'contoso' |
| `offerId`                | De unieke tekenreeks-id voor elke aanbieding, bijvoorbeeld "offer1"  |
| `planId`                 | De unieke tekenreeks-id voor elk abonnement of welke sku, bijvoorbeeld 'silver' |
| `operationId`            | GUID-id voor een bepaalde bewerking  |
|  `action`                | De actie wordt uitgevoerd op een resource, ofwel `subscribe`, `unsubscribe`, `suspend`, `reinstate`, of `changePlan`, `changeQuantity`, `transfer`  |
|   |   |

Unieke id's ([GUID's](https://en.wikipedia.org/wiki/Universally_unique_identifier)) 128-bits (32 hexadecimale) getallen die worden doorgaans automatisch gegenereerd. 

#### <a name="resolve-a-subscription"></a>Een abonnement op te lossen 

Het eindpunt oplossen kunt de uitgever een marketplace-token omzetten in een permanente Resource-ID. De Resource-ID is de unieke id voor SAAS-abonnement.  Wanneer een gebruiker wordt omgeleid naar de website van een ISV, bevat de URL een token in de queryparameters. De ISV wordt verwacht voor gebruik van dit token, en maken van een aanvraag om dit te verhelpen. Het antwoord bevat de unieke SAAS abonnements-ID, de naam, de aanbiedings-ID en het plan voor de resource. Dit token is alleen een uur geldig. 

**Verzenden:<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`**

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
|  Autorisatie     |  [JSON web token (JWT) bearer-token ophalen](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) |
|  x-ms-marketplace-token  |  Token queryparameter in de URL in wanneer de gebruiker wordt omgeleid naar de SaaS-ISV-website van Azure (bijvoorbeeld: `https://contoso.com/signup?token=..`). *Opmerking:* Het wordt door URL gedecodeerd waarde vanuit de browser voor het token voordat u deze gebruikt.  |

*Responscodes:*

Code: 200<br>
De ondoorzichtige token worden omgezet in een SaaS-abonnement.<br>

```json
Response body:
{
    "subscriptionId": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" 
}
```

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag. x-ms-marketplace-token is ontbrekende, ongeldige of verlopen.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

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

### <a name="subscription-api"></a>Abonnement API

Het abonnement API ondersteunt de volgende bewerkingen uit HTTPS: **Ophalen**, **Post**, **Patch**, en **verwijderen**.


#### <a name="list-subscriptions"></a>Lijst met abonnementen

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
| Autorisatie      |  [JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Responscodes:*

Code: 200 <br/>
Op basis van het auth-token, krijgen de uitgever en de bijbehorende abonnementen voor aanbiedingen van de uitgever.<br> Payload van he antwoord:<br>

```json
{
  [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```

Het vervolgtoken is alleen aanwezig als u extra "'s' van plannen om op te halen. 

Code: 403 <br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert. 

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
|  Autorisatie     |  [JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Responscodes:*

Code: 200<br>
SaaS-abonnement uit-id opgehaald<br> Payload van he antwoord:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"Contoso Cloud Solution",
        "publisherId": "contoso",
        "offerId": "offer1",
        "planId": "silver",
        "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "status": "Subscribed", // Indicates the status of the operation.
}
```

Code: 404<br>
Niet gevonden<br> 

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

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

Gebruik deze aanroep om erachter te komen of er geen persoonlijke/openbare aanbiedingen voor de huidige uitgever zijn.

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
|  Autorisatie     |  [JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) |

*Responscodes:*

Code: 200<br>
Een lijst met beschikbare abonnementen ophalen voor een klant.<br>

Hoofdtekst van antwoord:

```json
{
    "plans": [{
        "planId": "Platinum001",
        "displayName": "Private platinum plan for Contoso",
        "isPrivate": true
    }]
}
```

Code: 404<br>
Niet gevonden<br> 

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert. <br> 

Code: 500<br>
Interne serverfout<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
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
|  x-ms-correlationid  | De waarde van de unieke tekenreeks voor de bewerking op de client. Deze tekenreeks correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  Autorisatie     |  [JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) |

*Aanvraag:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Responscodes:*

Code: 200<br>
Hiermee activeert u het abonnement.<br>

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

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

#### <a name="change-the-plan-on-the-subscription"></a>Het abonnement van het abonnement wijzigen

Werk de planning van het abonnement.

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
| Autorisatie      |  [JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*De nettolading van de aanvraag:*

```json
Request Body:
{
    "planId": "gold"
}
```

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Koppelen aan een resource om op te halen van de status van de bewerking.   |

*Responscodes:*

Code: 202<br>
De aanvraag voor het abonnement wijzigen is geaccepteerd. De ISV wordt verwacht op te vragen voor de bewerking-locatie op voor een geslaagde/mislukte bepalen. <br>

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten.

>[!Note]
>Alleen een abonnement of de hoeveelheid kan worden patches in één keer, niet beide. Bewerkingen op een abonnement met **Update** zich niet in `allowedCustomerOperations`.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

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

#### <a name="change-the-quantity-on-the-subscription"></a>De hoeveelheid van het abonnement wijzigen

Werk de hoeveelheid van het abonnement.

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
| Autorisatie      |  [JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*De nettolading van de aanvraag:*

```json
Request Body:
{
    "quantity": 5
}
```

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Koppelen aan een resource om op te halen van de status van de bewerking.   |

*Responscodes:*

Code: 202<br>
Geaccepteerd. De aanvraag voor het wijzigen van de hoeveelheid is geaccepteerd. De ISV wordt verwacht op te vragen voor de bewerking-locatie op voor een geslaagde/mislukte bepalen. <br>

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten.

>[!Note]
>Alleen een abonnement of de hoeveelheid kan worden patches in één keer, niet beide. Bewerkingen op een abonnement met **Update** zich niet in `allowedCustomerOperations`.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

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
|  Autorisatie     |  [JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Responscodes:*

Code: 202<br>
ISV gestart aanroep om aan te geven zich afmelden op een SaaS-abonnement.<br>

Code: 404<br>
Niet gevonden

Code: 400<br>
Verwijderen van een abonnement met **verwijderen** niet in `allowedCustomerOperations`.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

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

#### <a name="list-outstanding-operations"></a>Lijst met openstaande bewerkingen 

Geeft een lijst van de openstaande bewerkingen voor de huidige uitgever. 

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
|  Autorisatie     |  [JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Responscodes:*

Code: 200<br> Hiermee haalt u de lijst met bewerkingen op een abonnement in behandeling.<br>
Payload van he antwoord:

```json
[{
    "id": "<guid>",  
    "activityId": "<guid>",
    "subscriptionId": "<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
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
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

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

Hiermee kunt u de uitgever om bij te houden van de status van de opgegeven geactiveerde asynchrone bewerking (abonneren / afmelden / wijzigen van plan bent / wijzigen hoeveelheid).

**Toevoegen:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`**

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking te gebruiken voor deze aanvraag.  |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`   |
|  x-ms-requestid    |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  x-ms-correlationid |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  Autorisatie     |[JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*Responscodes:* Code: 200<br> Met deze eigenschap wordt de opgegeven SaaS-bewerking in behandeling<br>
Payload van he antwoord:

```json
Response body:
{
    "id  ": "<guid>",
    "activityId": "<guid>",
    "subscriptionId":"<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}

```

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.
 
Code: 500<br> Interne serverfout

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>De status van een bewerking bijwerken

De status van een bewerking om aan te geven geslaagd/mislukt met de opgegeven waarden bijwerken.

**Patch voor:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`**

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
|  Autorisatie     |  [JSON web token (JWT) bearer-token ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app)  |

*De nettolading van de aanvraag:*

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Responscodes:*

Code: 200<br> Aanroepen voor het op de hoogte van een bewerking aan de ISV is voltooid. Deze reactie kan bijvoorbeeld duiden dat de wijziging van de seats/abonnementen.

Code: 404<br>
Niet gevonden

Code: 400<br>
Ongeldige aanvraag-validatiefouten

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven, is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

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

## <a name="webhook-on-the-saas-service"></a>Webhook in de SaaS-service

De uitgever moet een webhook in deze SaaS-service om proactief te waarschuwen gebruikers van wijzigingen in de service implementeren. De API verwacht wordt niet-geverifieerde en wordt aangeroepen door de Microsoft SaaS-service. De SaaS-service wordt verwacht voor het aanroepen van de operations-API om te verifiëren en autoriseren voordat een actie wordt ondernomen op de webhook-melding.

```json
{
  "id": "<this is a Guid operation id, you can call operations API with this to get status>",
  "activityId": "<this is a Guid correlation id>",
  "subscriptionId": "<Guid to uniquely identify this resource>",
  "publisherId": "<this is the publisher’s name>",
  "offerId": "<this is the offer name>",
  "planId": "<this is the plan id>",
  "quantity": "<the number of seats, will be null if not per-seat saas offer>",
  "timeStamp": "2019-04-15T20:17:31.7350641Z",
  "action": "Unsubscribe",
  "status": "NotStarted"  

}
```
Actie kan waar een van de volgende zijn: 
- `Subscribe`, (Wanneer de resource is geactiveerd)
- `Unsubscribe`, (Wanneer de resource is verwijderd)
- `ChangePlan`, (Wanneer de wijziging plan-bewerking is voltooid)
- `ChangeQuantity`, (Wanneer de wijziging hoeveelheid bewerking is voltooid),
- `Suspend`, (Als bron is onderbroken)
- `Reinstate`, (Als bron heeft is hersteld na onderbreking)

Waar kan status van een van de volgende zijn: <br>
        -Niet begonnen, <br>
        -Wordt uitgevoerd, <br>
        -Voltooid <br>
        -Is mislukt, <br>
        -Conflict <br>

Bruikbare statussen zijn geslaagd en mislukt in een webhook-melding. Levenscyclus van een bewerking is van NotStarted naar een definitieve status heeft, zoals geslaagd/mislukt/Conflict. Als u niet gestart ontvangt of in behandeling is, ga dan naar de status opvragen via GET-bewerking API totdat de bewerking wordt een definitieve status bereikt voordat een actie wordt ondernomen. 

## <a name="mock-api"></a>Mock API

U kunt onze mock API's gebruiken om u aan de slag met ontwikkelen, met name prototypen te helpen en testen van projecten. 

Eindpunt voor de host: `https://marketplaceapi.microsoft.com/api` <br/>
API-versie: `2018-09-15` <br/>
Geen verificatie vereist <br/>
Voorbeeld-Uri: `https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

De API-eindpunt paden hetzelfde zijn voor zowel nagebootste en echte API's, maar de API-versies zijn verschillend. De versie is 2018-09-15 voor nagebootste en 2018-08-31 voor de productieversie. 

Een van de API-aanroepen in dit artikel kan worden gemaakt met het eindpunt mock-host. U kunt verwachten mock-gegevens weer als een antwoord op te halen. In het algemeen kunt u verwachten mock-gegevens weer als een antwoord op te halen. Aanroepen van de methoden voor het abonnement van update op de mock-API retourneert altijd is 500. 

## <a name="next-steps"></a>Volgende stappen

Ontwikkelaars kunnen ook programmatisch ophalen en manipuleren van werkbelastingen, aanbiedingen en uitgever profielen met behulp van de [Cloud Partner Portal REST-API's](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
