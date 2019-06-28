---
title: SaaS vervulling API v2 | Azure Marketplace
description: In dit artikel wordt uitgelegd hoe u maken en beheren van een SaaS-aanbod voor de Azure Marketplace en AppSource met behulp van de bijbehorende uitvoering v2 API's.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 05/23/2019
ms.author: evansma
ms.openlocfilehash: ecee1669c29d7b298741f9e5521de03da6dd7e3b
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331640"
---
# <a name="saas-fulfillment-apis-version-2"></a>Afhandeling van SaaS-API's, versie 2 

Dit artikel worden de API's waarmee partners hun SaaS-toepassingen in de AppSource-marketplace en de Azure Marketplace verkopen. Deze API's zijn dat een vereiste voor transactable SaaS biedt op de Azure Marketplace en AppSource.

## <a name="managing-the-saas-subscription-life-cycle"></a>De levenscyclus voor SaaS-abonnement beheren

Azure SaaS beheert de volledige levenscyclus van een SaaS-abonnement kopen. Het maakt gebruik van de uitvoering van API's als een mechanisme om de werkelijke naleving te stimuleren, wijzigingen van abonnementen en verwijderen van het abonnement met de partner. De factuur van de klant is gebaseerd op de status van de SaaS-abonnement die Microsoft onderhoudt. Het volgende diagram ziet u de Staten en de bewerkingen die de wijzigingen tussen de Staten station.

![Levenscyclus voor SaaS abonnementsstatussen](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Status van een SaaS-abonnement

De volgende tabel bevat de inrichting statussen voor een SaaS-abonnement, met inbegrip van een diagram beschrijving en volgorde voor elk (indien van toepassing). 

#### <a name="provisioning"></a>Inrichten

Wanneer een klant een aankoop initieert, ontvangt de partner deze informatie in een autorisatiecode voor een klant-interactieve webpagina die gebruikmaakt van een URL-parameter. Een voorbeeld is `https://contoso.com/signup?token=..`, terwijl de URL van de landingspagina in Partner Center `https://contoso.com/signup`. De autorisatiecode kan worden gevalideerd en die voor de details van de provisioning-service door het aanroepen van de API kunt oplossen door worden uitgewisseld.  Wanneer een SaaS-service is voltooid wordt ingericht, stuurt een aanroep activeren om aan te geven dat de uitvoering voltooid is en de klant kan worden gefactureerd. 

Het volgende diagram toont de volgorde van de API-aanroepen voor een scenario voor het inrichten.  

![API-aanroepen voor het inrichten van een SaaS-service](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Ingericht

Deze status is de actieve status van een ingerichte service.

##### <a name="provisioning-for-update"></a>Inrichting voor update 

Deze status geeft aan dat een update aan voor een bestaande service in behandeling is. Deze update kan worden gestart door de klant, vanuit de marketplace of in de SaaS-service (alleen voor directe klant transacties).

##### <a name="provisioning-for-update-when-its-initiated-from-the-marketplace"></a>Inrichting van update (wanneer deze wordt gestart vanuit de marketplace)

Het volgende diagram toont de volgorde van de acties wanneer een update wordt gestart vanuit de marketplace.

![API-aanroepen wanneer de update wordt gestart vanuit de marketplace](./media/saas-update-api-v2-calls-from-marketplace-a.png)

##### <a name="provisioning-for-update-when-its-initiated-from-the-saas-service"></a>Inrichting van update (wanneer deze wordt gestart vanuit de SaaS-service)

Het volgende diagram toont de acties wanneer een update van de SaaS-service wordt gestart. (De webhook-aanroep wordt vervangen door een update aan voor het abonnement dat is gestart door de SaaS-service.) 

![API-aanroepen wanneer de update wordt gestart vanuit de SaaS-service](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Suspended

Deze status geeft aan dat de betaling van een klant is niet ontvangen. Door het beleid bieden we de klant een respijtperiode voordat het abonnement wordt geannuleerd. Een abonnement heeft wanneer deze status: 

- Als partner besluiten kunt u te verlagen of te blokkeren van de gebruiker toegang tot de service.
- Het abonnement moet worden bewaard in een herstelbare staat die u kunt de volledige functionaliteit zonder verlies van gegevens of -instellingen herstellen. 
- Verwacht om op te halen van een aanvraag voor reactiveren voor dit abonnement via de vervulling API's of een aanvraag voor ongedaan maken inrichting aan het einde van de respijtperiode. 

#### <a name="unsubscribed"></a>Afgemeld 

Abonnementen contact opnemen met deze status in reactie op een verzoek van de expliciete klant- of betaald van bijdragen. De verwachting van de partner is dat de gegevens van de klant voor herstel op aanvraag voor een bepaald aantal dagen bewaard en vervolgens verwijderd. 


## <a name="api-reference"></a>API-verwijzing

In deze sectie worden de SaaS *abonnement API* en *Operations API*.  De waarde van de `api-version` parameter voor versie 2-API's is `2018-08-31`.  


### <a name="parameter-and-entity-definitions"></a>Definities van parameter en de entiteit

De volgende tabel bevat de definities voor de algemene parameters en entiteiten die worden gebruikt door uitvoering API's.

|     Entity/Parameter     |     Definitie                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | De GUID-id voor een SaaS-resource.  |
| `name`                   | Een beschrijvende naam voor deze resource is opgegeven door de klant. |
| `publisherId`            | Een unieke tekenreeks-id voor elke uitgever (bijvoorbeeld: "contoso"). |
| `offerId`                | Een unieke tekenreeks-id voor elke aanbieding (bijvoorbeeld: "offer1").  |
| `planId`                 | Een unieke tekenreeks-id voor elk abonnement of welke SKU (bijvoorbeeld: 'silver'). |
| `operationId`            | De GUID-id voor een bepaalde bewerking.  |
|  `action`                | De actie wordt uitgevoerd op een resource, ofwel `subscribe`, `unsubscribe`, `suspend`, `reinstate`, of `changePlan`, `changeQuantity`, `transfer`.  |
|   |   |

Unieke id's ([GUID's](https://en.wikipedia.org/wiki/Universally_unique_identifier)) 128-bits (32 hexadecimale) getallen die worden doorgaans automatisch gegenereerd. 

#### <a name="resolve-a-subscription"></a>Een abonnement op te lossen 

Het eindpunt oplossen kunt de uitgever een marketplace-token omzetten in een permanente resource-ID. De resource-ID is de unieke id voor een SaaS-abonnement. Wanneer een gebruiker wordt omgeleid naar de website van de partner, bevat de URL een token in de queryparameters. De partner wordt verwacht voor gebruik van dit token en maken van een aanvraag om dit te verhelpen. Het antwoord bevat de unieke SaaS abonnements-ID, de naam, de aanbiedings-ID en het plan voor de resource. Dit token is slechts één uur geldig. 

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsresolveapi-versionapiversion"></a>Verzenden<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking te gebruiken voor deze aanvraag.  |

*Aanvraagheaders:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |  Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  x-ms-correlationid |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  authorization     |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |
|  x-ms-marketplace-token  |  De queryparameter van de token in de URL in wanneer de gebruiker wordt omgeleid naar de website van de SaaS-partner van Azure (bijvoorbeeld: `https://contoso.com/signup?token=..`). *Opmerking:* Het wordt door URL gedecodeerd waarde vanuit de browser voor het token voordat u deze gebruikt.  |

*Responscodes:*

Code: 200<br>
De ondoorzichtige token worden omgezet in een SaaS-abonnement. Hoofdtekst van antwoord:
 

```json
{
    "id": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" 
}
```

Code: 400<br>
Ongeldige aanvraag. x-ms-marketplace-token is ontbrekende, ongeldige of verlopen.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

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

##### <a name="getbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsapi-versionapiversion"></a>Ophalen<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`

*Queryparameters:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  De versie van de bewerking te gebruiken voor deze aanvraag.  |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
| Content-Type       |  `application/json`  |
| x-ms-requestid     |  Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| x-ms-correlationid |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
| authorization      |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Responscodes:*

Code: 200 <br/>
Hiermee haalt u de uitgever en de bijbehorende abonnementen van de uitgever-aanbiedingen, op basis van het verificatietoken.
Payload van he antwoord:<br>

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
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```

Het vervolgtoken is alleen aanwezig als er zijn aanvullende "'s' van plannen om op te halen. 

Code: 403 <br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert. 

Code: 500<br>
Interne serverfout.

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

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Ophalen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId?api-version=<ApiVersion>`

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Een unieke id van de SaaS-abonnement dat verkregen na het oplossen van het token via API oplossen.   |
|  ApiVersion        |   De versie van de bewerking te gebruiken voor deze aanvraag.   |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`  |
|  x-ms-requestid    |  Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  x-ms-correlationid |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  authorization     |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*Responscodes:*

Code: 200<br>
Hiermee haalt u de SaaS-abonnement uit-id. Payload van he antwoord:<br>

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
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "status": "Subscribed", // Indicates the status of the operation.
}
```

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

Code: 404<br>
Niet gevonden.<br> 

Code: 500<br>
Interne serverfout.<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Lijst met beschikbare abonnementen

Gebruik deze aanroep om erachter te komen of er geen persoonlijke of openbare aanbiedingen voor de huidige uitgever zijn.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidlistavailableplansapi-versionapiversion"></a>Ophalen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   De versie van de bewerking te gebruiken voor deze aanvraag.  |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|   x-ms-requestid   |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  x-ms-correlationid  | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  authorization     |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*Responscodes:*

Code: 200<br>
Hiermee haalt een lijst met beschikbare abonnementen voor een klant. Hoofdtekst van antwoord:

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
Niet gevonden.<br> 

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert. <br> 

Code: 500<br>
Interne serverfout.<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="activate-a-subscription"></a>Een abonnement activeren

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidactivateapi-versionapiversion"></a>Verzenden<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking te gebruiken voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat verkregen na het oplossen van het token met behulp van de API oplossen.  |

*Aanvraagheaders:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json`  |
|  x-ms-requestid    | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  x-ms-correlationid  | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze tekenreeks correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  authorization     |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). |

*De nettolading van de aanvraag:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Responscodes:*

Code: 200<br>
Hiermee activeert u het abonnement.<br>

Code: 400<br>
Ongeldige aanvraag: validatiefouten.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

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

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking te gebruiken voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat verkregen na het oplossen van het token met behulp van de API oplossen.  |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  x-ms-correlationid  |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.    |
| authorization      |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

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
| Operation-Location | De koppeling naar een resource om op te halen van de status van de bewerking.   |

*Responscodes:*

Code: 202<br>
De aanvraag voor het abonnement wijzigen is geaccepteerd. De partner wordt verwacht op te vragen voor de bewerking-locatie om te bepalen of een geslaagd of mislukt. <br>

Code: 400<br>
Ongeldige aanvraag: validatiefouten.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Alleen een abonnement of de hoeveelheid kan worden patches in één keer, niet beide. Bewerkingen op een abonnement met **Update** zich niet in `allowedCustomerOperations`.

#### <a name="change-the-quantity-on-the-subscription"></a>De hoeveelheid van het abonnement wijzigen

Werk de hoeveelheid van het abonnement.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch voor:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking te gebruiken voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat verkregen na het oplossen van het token met behulp van de API oplossen.  |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  x-ms-correlationid  |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.    |
| authorization      |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

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
De aanvraag voor het wijzigen van de hoeveelheid is geaccepteerd. De partner wordt verwacht op te vragen voor de bewerking-locatie om te bepalen of een geslaagd of mislukt. <br>

Code: 400<br>
Ongeldige aanvraag: validatiefouten.


Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Alleen een abonnement of de hoeveelheid kan worden patches in één keer, niet beide. Bewerkingen op een abonnement met **Update** zich niet in `allowedCustomerOperations`.

#### <a name="delete-a-subscription"></a>Een abonnement verwijderen

Afmelden en verwijderen van het opgegeven abonnement.

##### <a name="deletebr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionid-api-versionapiversion"></a>Verwijderen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId> ?api-version=<ApiVersion>`

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking te gebruiken voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat verkregen na het oplossen van het token met behulp van de API oplossen.  |

*Aanvraagheaders:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.   |
|  x-ms-correlationid  |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.   |
|  authorization     |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Responscodes:*

Code: 202<br>
De partner gestart een aanroep van een SaaS-abonnement opzeggen.<br>

Code: 400<br>
Verwijderen van een abonnement met **verwijderen** niet in `allowedCustomerOperations`.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

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

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsapi-versionapiversion"></a>Ophalen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`

*Queryparameters:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   De versie van de bewerking te gebruiken voor deze aanvraag.                |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat verkregen na het oplossen van het token met behulp van de API oplossen.  |

*Aanvraagheaders:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |  Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  x-ms-correlationid |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
|  authorization     |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Responscodes:*

Code: 200<br> Hiermee haalt u de lijst met bewerkingen op een abonnement in behandeling. Payload van he antwoord:

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


Code: 400<br>
Ongeldige aanvraag: validatiefouten.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>Bewerkingsstatus ophalen

Hiermee kunt u de uitgever om bij te houden van de status van de opgegeven geactiveerde asynchrone bewerking (zoals `subscribe`, `unsubscribe`, `changePlan`, of `changeQuantity`).

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Ophalen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

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
|  authorization     |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*Responscodes:*<br>

Code: 200<br> Hiermee haalt u de opgegeven SaaS-bewerking in behandeling. Payload van he antwoord:

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

Code: 400<br>
Ongeldige aanvraag: validatiefouten.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.
 
Code: 404<br>
Niet gevonden.

Code: 500<br> Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>De status van een bewerking bijwerken

De status van een bewerking voor het slagen of mislukken met de opgegeven waarden geven bijwerken.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Queryparameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  De versie van de bewerking te gebruiken voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat verkregen na het oplossen van het token met behulp van de API oplossen.  |
|  operationId       | De bewerking die wordt uitgevoerd. |

*Aanvraagheaders:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     | `application/json`   |
|   x-ms-requestid   |   Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  x-ms-correlationid |  Een unieke tekenreeks-waarde voor de bewerking op de client. Deze parameter correleert alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
|  authorization     |  [Ophalen van JSON web token (JWT) bearer-token](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  |

*De nettolading van de aanvraag:*

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Responscodes:*

Code: 200<br> Een aanroep van de hoogte van een bewerking aan de partner is voltooid. Deze reactie kan bijvoorbeeld duiden dat de wijziging van de seats of plannen.

Code: 400<br>
Ongeldige aanvraag: validatiefouten.

Code: 403<br>
Niet-gemachtigde. Het verificatietoken is niet opgegeven of is ongeldig, of de aanvraag voor toegang tot een verzameling die geen deel van de uitgever van de huidige uitmaken probeert.

Code: 404<br>
Niet gevonden.

Code: 409<br>
Een conflict. Bijvoorbeeld, is een nieuwe transactie al voldaan.

Code: 500<br> Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="implementing-a-webhook-on-the-saas-service"></a>Implementatie van een webhook in de SaaS-service

De uitgever moet een webhook in deze SaaS-service om proactief te waarschuwen gebruikers van wijzigingen in de service implementeren. De SaaS-service wordt verwacht voor het aanroepen van de operations-API om te verifiëren en autoriseren voordat een actie wordt ondernomen op de webhook-melding.

```json
{
  "id": "<this is a GUID operation id, you can call operations API with this to get status>",
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
De actie kan waar een van de volgende zijn: 
- `subscribe` (wanneer de resource is geactiveerd)
- `unsubscribe` (wanneer de resource is verwijderd)
- `changePlan` (wanneer de wijziging plan-bewerking is voltooid)
- `changeQuantity` (wanneer de wijziging hoeveelheid-bewerking is voltooid)
- `suspend` (als bron is onderbroken)
- `reinstate` (als bron heeft is hersteld na onderbreking)

Waar kan de status van een van de volgende zijn: 
- **NotStarted** <br>
 - **InProgress** <br>
- **Geslaagd** <br>
- **Mislukt** <br>
- **Conflict** <br>

In een melding webhook bruikbare statussen zijn beide **geslaagd** en **mislukt**. De levenscyclus van een bewerking is afkomstig van **NotStarted** naar een definitieve status heeft, zoals **geslaagd**, **mislukt**, of **Conflict**. Als u ontvangt **NotStarted** of **InProgress**, blijven de status opvragen via de API ophalen totdat de bewerking wordt een definitieve status bereikt voordat actie wordt ondernomen. 

## <a name="mock-apis"></a>Mock-API 's

U kunt onze mock API's gebruiken om u aan de slag met ontwikkelen, met name prototypen, ook als testen projecten te helpen. 

Eindpunt hosten: `https://marketplaceapi.microsoft.com/api` (geen verificatie vereist)<br/>
API-versie: `2018-09-15`<br/>
URI-voorbeeld: `https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

De paden voor de API-eindpunt zijn hetzelfde voor echte zowel mock API's, maar de API-versies zijn verschillend. De versie is `2018-09-15` voor de mock-versie en `2018-08-31` voor de productieversie. 

Een van de API-aanroepen in dit artikel kan worden gemaakt met het eindpunt mock-host. In het algemeen kunt u verwachten mock-gegevens weer als een antwoord op te halen. Aanroepen van de methoden voor het abonnement van update op de mock-API retourneert altijd is 500. 

## <a name="next-steps"></a>Volgende stappen

Ontwikkelaars kunnen ook programmatisch worden opgehaald en workloads, aanbiedingen en uitgever profielen bewerken met behulp van de [Cloud Partner Portal REST-API's](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
