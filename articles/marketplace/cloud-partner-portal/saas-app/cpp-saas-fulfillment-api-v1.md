---
title: SaaS vervulling-API V1 - Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd hoe u een SaaS-aanbieding op Azure Marketplace, met behulp van de bijbehorende uitvoering V1 API's maken.
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
ms.topic: reference
ms.date: 03/28/2019
ms.author: pbutlerm
ROBOTS: NOINDEX
ms.openlocfilehash: 4908233280c69a37ea470eed2ef077cb220a7930
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "59009731"
---
# <a name="saas-fulfillment-apis-version-1--deprecated"></a>SaaS vervulling API's versie 1 (afgeschaft)

In dit artikel wordt uitgelegd hoe u een SaaS-aanbieding maken met API's. De API's bestaan uit REST methoden en -eindpunten zijn nodig voor het toestaan van abonnementen voor uw SaaS-aanbod als u hebt verkopen via Azure geselecteerd.  

> [!WARNING]
> Deze eerste versie van de vervulling SaaS API is afgeschaft; in plaats daarvan gebruik [SaaS vervulling-API V2](./cpp-saas-fulfillment-api-v2.md).  Deze API is momenteel alleen voor de service aan bestaande uitgevers bijgehouden. 

De volgende API's zijn bedoeld om u uw SaaS-service te integreren met Azure:

-   Oplossen
-   Abonneer u nu
-   Converteren
-   Afmelden


## <a name="api-methods-and-endpoints"></a>API-methoden en eindpunten

De volgende secties beschrijven de API-methoden en eindpunten beschikbaar voor het inschakelen van abonnementen voor een SaaS-aanbod.


### <a name="marketplace-api-endpoint-and-api-version"></a>Marketplace API-eindpunt en de API-versie

Het eindpunt voor de API van Azure Marketplace is `https://marketplaceapi.microsoft.com`.

De huidige API-versie is `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Abonnement oplossen

Actie na de op oplossen eindpunt kan gebruikers een marketplace-token worden omgezet in een permanente Resource-ID.  De Resource-ID is de unieke id voor SAAS-abonnement. 

Wanneer een gebruiker wordt omgeleid naar de website van een ISV, bevat de URL een token in de queryparameters. De ISV wordt verwacht voor gebruik van dit token, en maken van een aanvraag om dit te verhelpen. Het antwoord bevat de unieke SAAS abonnements-ID, de naam, de aanbiedings-ID en het plan voor de resource. Dit token is alleen een uur geldig.

*Aanvraag*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Parameternaam** |     **Description**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  API-versie        |  De versie van de bewerking te gebruiken voor deze aanvraag.   |
|  |  |


*Headers*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Dit komt overeen met alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| inhoudstype       | Ja          | `application/json`                                        |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                    |
| x-ms-marketplace-token| Ja| De token queryparameter in de URL in wanneer de gebruiker wordt omgeleid naar de website SaaS ISV's van Azure. **Opmerking:** Dit token is alleen geldig voor één uur. Bovendien decoderen URL van de token waarde vanuit de browser voordat u deze gebruikt.|
|  |  |  |
  

*Hoofdtekst van de reactie*

``` json
{
    "id": "",
    "subscriptionName": "",
    "offerId": "",
    "planId": "",
}
```

| **Parameternaam** | **Gegevenstype** | **Description**                       |
|--------------------|---------------|---------------------------------------|
| id                 | String        | ID van de SaaS-abonnement.          |
| subscriptionName| String| De naam van de SaaS-abonnement instellen door de gebruiker in Azure terwijl u zich abonneert op de SaaS-service.|
| OfferId            | String        | Aanbiedings-ID die de gebruiker een abonnement op. |
| planId             | String        | Plan-ID die de gebruiker een abonnement op.  |
|  |  |  |


*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Description**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Token is opgelost.                                                            |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of een ongeldige api-versie die is opgegeven. Kan het token niet ophalen omdat een van beide het token ongeldig of verlopen is (het token is alleen geldig voor 1 uur eenmaal gegenereerd). |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                                 |
| 429                  | `RequestThrottleId`  | Service is bezet verwerken van aanvragen, voer later opnieuw uit.                                |
| 503                  | `ServiceUnavailable` | Service is omlaag tijdelijk, probeer het later opnieuw.                                        |
|  |  |  |


*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Aanvraag-ID ontvangen van de client.                                                                   |
| x-ms-correlationid | Ja          | Correlatie-ID is als doorgegeven door de client, anders wordt deze waarde de correlatie-id op.                   |
| x-ms-activityid    | Ja          | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Deze ID wordt gebruikt voor alle aansluitingen. |
| Opnieuw proberen na        | Nee           | Deze waarde wordt alleen ingesteld voor een fout 429.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Abonneer u nu

Het eindpunt abonneren kan gebruikers een abonnement om een SaaS-service voor een bepaald abonnement te starten en inschakelen van facturering in het commerce-systeem.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Parameternaam**  | **Description**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Unieke ID van de SaaS-abonnement dat is verkregen na het oplossen van het token via API oplossen.                              |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

|  **Header-sleutel**        | **Vereist** |  **Description**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   Nee         | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als dit niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| x-ms-correlationid     |   Nee         | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als dit niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| If-Match/If-None-Match |   Nee         |   Validatie van sterke ETag-waarde.                                                          |
| inhoudstype           |   Ja        |    `application/json`                                                                   |
|  Autorisatie         |   Ja        |    De JSON web token (JWT) bearer-token.                                               |
| x-ms-marketplace-session-mode| Nee | De vlag om in te schakelen testmodus terwijl u zich abonneert op een SaaS-aanbod. Als is ingesteld, het abonnement wordt niet in rekening gebracht. Dit is handig voor ISV-scenario's te testen. Stel deze in op **'is een'**|
|  |  |  |

*Hoofdtekst*

``` json
{
    "lanId": "",
}
```

| **De naam van element** | **Gegevenstype** | **Description**                      |
|------------------|---------------|--------------------------------------|
| planId           | (Vereist) Tekenreeks        | Plan-Id van de gebruiker van de SaaS-service is geabonneerd op.  |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Description**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Activering van SaaS-abonnement ontvangen voor een bepaald abonnement.                   |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of de hoofdtekst van de JSON is ongeldig. |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                   |
| 404                  | `NotFound`           | Het abonnement niet vinden met de opgegeven ID                                  |
| 409                  | `Conflict`           | Een andere bewerking wordt uitgevoerd op het abonnement.                     |
| 429                  | `RequestThrottleId`  | Service is bezet verwerken van aanvragen, voer later opnieuw uit.                  |
| 503                  | `ServiceUnavailable` | Service is omlaag tijdelijk, probeer het later opnieuw.                          |
|  |  |  |

Volgen voor een 202-antwoord op de status van de aanvraagbewerking bij de bewerking-location-header. De verificatie is hetzelfde als andere Marketplace API's.

*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Aanvraag-ID ontvangen van de client.                                                                   |
| x-ms-correlationid | Ja          | Correlatie-ID is als doorgegeven door de client, anders wordt deze waarde de correlatie-id op.                   |
| x-ms-activityid    | Ja          | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Deze waarde wordt gebruikt voor alle aansluitingen. |
| Opnieuw proberen na        | Ja          | Interval met welke client u kunt de status controleren.                                                       |
| Operation-Location | Ja          | Koppelen aan een resource om op te halen van de status van de bewerking.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Eindpunt van de planning wijzigen

Het eindpunt van de wijziging kan de gebruiker van een plan op dat moment geabonneerde converteren naar een nieuw abonnement.

**PATCH**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Parameternaam**  | **Description**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID van de SaaS-abonnement.                              |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**          | **Vereist** | **Description**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client. Het beste een GUID. Als dit niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.   |
| x-ms-correlationid      | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als dit niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| If-None-Match /If-None-Match | Nee           | Validatie van sterke ETag-waarde.                              |
| inhoudstype            | Ja          | `application/json`                                        |
| Autorisatie           | Ja          | De JSON web token (JWT) bearer-token.                    |
|  |  |  |

*Hoofdtekst*

```json
{
    "planId": ""
}
```

|  **De naam van element** |  **Gegevenstype**  | **Description**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  (Vereist) Tekenreeks         | Plan-Id van de gebruiker van de SaaS-service is geabonneerd op.          |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Description**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Activering van SaaS-abonnement ontvangen voor een bepaald abonnement.                   |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of de hoofdtekst van de JSON is ongeldig. |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                   |
| 404                  | `NotFound`           | Het abonnement niet vinden met de opgegeven ID                                  |
| 409                  | `Conflict`           | Een andere bewerking wordt uitgevoerd op het abonnement.                     |
| 429                  | `RequestThrottleId`  | Service is bezet verwerken van aanvragen, voer later opnieuw uit.                  |
| 503                  | `ServiceUnavailable` | Service is omlaag tijdelijk, probeer het later opnieuw.                          |
|  |  |  |

*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Aanvraag-ID ontvangen van de client.                                                                   |
| x-ms-correlationid | Ja          | Correlatie-ID is als doorgegeven door de client, anders wordt deze waarde de correlatie-id op.                   |
| x-ms-activityid    | Ja          | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Deze waarde wordt gebruikt voor alle aansluitingen. |
| Opnieuw proberen na        | Ja          | Interval met welke client u kunt de status controleren.                                                       |
| Operation-Location | Ja          | Koppelen aan een resource om op te halen van de status van de bewerking.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Abonnement verwijderen

De Delete-bewerking op het eindpunt abonneren kan een gebruiker verwijderen van een abonnement met een opgegeven ID.

*Aanvraag*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Parameternaam**  | **Description**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID van de SaaS-abonnement.                              |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client. Het beste een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.                                                           |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als dit niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                    |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Description**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Activering van SaaS-abonnement ontvangen voor een bepaald abonnement.                   |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of de hoofdtekst van de JSON is ongeldig. |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                   |
| 404                  | `NotFound`           | Het abonnement niet vinden met de opgegeven ID                                  |
| 429                  | `RequestThrottleId`  | -Service is bezet het verwerken van aanvragen, probeer het later opnieuw.                  |
| 503                  | `ServiceUnavailable` | Service is tijdelijk niet beschikbaar. Probeer het later opnieuw.                          |
|  |  |  |

Volgen voor een 202-antwoord op de status van de aanvraagbewerking bij de bewerking-location-header. De verificatie is hetzelfde als andere Marketplace API's.

*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Aanvraag-ID ontvangen van de client.                                                                   |
| x-ms-correlationid | Ja          | Correlatie-ID is als doorgegeven door de client, anders wordt dit de correlatie-id op.                   |
| x-ms-activityid    | Ja          | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Dit wordt gebruikt voor alle aansluitingen. |
| Opnieuw proberen na        | Ja          | Interval met welke client u kunt de status controleren.                                                       |
| Operation-Location | Ja          | Koppelen aan een resource om op te halen van de status van de bewerking.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>Bewerkingsstatus ophalen

Dit eindpunt kan een gebruiker voor het bijhouden van de status van een geactiveerde asynchrone bewerking (aanmelden/afmelden/wijzigen-plan).

*Aanvraag*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/*{operationId}*?api-version=2017-04-15**

| **Parameternaam**  | **Description**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | Unieke ID voor de bewerking die wordt geactiveerd.                |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client. Het beste een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.   |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                    |
|  |  |  | 

*Hoofdtekst van de reactie*

```json
{
    "id": "",
    "status": "",
    "resourceLocation": "",
    "created": "",
    "lastModified": ""
}
```

| **Parameternaam** | **Gegevenstype** | **Description**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | String        | ID van de bewerking.                                                                      |
| status             | Enum          | Status van de bewerking, een van de volgende: `In Progress`, `Succeeded`, of `Failed`.          |
| resourceLocation   | String        | Koppelen aan het abonnement dat is gemaakt of gewijzigd. Hiermee wordt de client om op te halen van de geüpdate toestand post-bewerking. Deze waarde is niet ingesteld voor `Unsubscribe` bewerkingen. |
| gemaakt            | DateTime      | Bewerking maken van de tijd in UTC.                                                           |
| lastModified       | DateTime      | Laatste update voor de bewerking die wordt in UTC.                                                      |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Description**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | De get-aanvraag is opgelost en de hoofdtekst van het antwoord bevat.    |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of er is een ongeldige api-versie opgegeven. |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                      |
| 404                  | `NotFound`           | Het abonnement niet vinden met de opgegeven ID                                     |
| 429                  | `RequestThrottleId`  | Service is bezet verwerken van aanvragen, voer later opnieuw uit.                     |
| 503                  | `ServiceUnavailable` | Service is omlaag tijdelijk, probeer het later opnieuw.                             |
|  |  |  |

*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Aanvraag-ID ontvangen van de client.                                                                   |
| x-ms-correlationid | Ja          | Correlatie-ID is als doorgegeven door de client, anders wordt dit de correlatie-id op.                   |
| x-ms-activityid    | Ja          | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Dit wordt gebruikt voor alle aansluitingen. |
| Opnieuw proberen na        | Ja          | Interval met welke client u kunt de status controleren.                                                       |
|  |  |  |

### <a name="get-subscription"></a>Abonnement ophalen

De Get-actie op abonneren eindpunt kan een gebruiker een abonnement met een bepaalde resource-id ophalen.

*Aanvraag*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Parameternaam**  | **Description**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID van de SaaS-abonnement.                              |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.                                                           |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                                                                    |
|  |  |  |

*Hoofdtekst van de reactie*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parameternaam**     | **Gegevenstype** | **Description**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Abonnement-resource-ID van SaaS in Azure.    |
| offerId                | String        | Aanbiedings-ID die de gebruiker een abonnement op.         |
| planId                 | String        | Plan-ID die de gebruiker een abonnement op.          |
| saasSubscriptionName   | String        | De naam van de SaaS-abonnement.                |
| saasSubscriptionStatus | Enum          | Status van de bewerking.  Een van de volgende:  <br/> - `Subscribed`: Abonnement is actief.  <br/> - `Pending`: Gebruiker maken van de resource, maar deze niet is geactiveerd door de ISV.   <br/> - `Unsubscribed`: Gebruiker heeft zich afgemeld.   <br/> - `Suspended`: Gebruiker heeft het abonnement onderbroken.   <br/> - `Deactivated`:  Azure-abonnement is onderbroken.  |
| gemaakt                | DateTime      | Abonnement maken van de waarde van het tijdstempel in UTC. |
| lastModified           | DateTime      | Abonnement gewijzigd tijdstempelwaarde in UTC. |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Description**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | De get-aanvraag is opgelost en de hoofdtekst van het antwoord bevat.    |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of er is een ongeldige api-versie opgegeven. |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                      |
| 404                  | `NotFound`           | Het abonnement niet vinden met de opgegeven ID                                     |
| 429                  | `RequestThrottleId`  | Service is bezet verwerken van aanvragen, voer later opnieuw uit.                     |
| 503                  | `ServiceUnavailable` | Service is omlaag tijdelijk, probeer het later opnieuw.                             |
|  |  |  |

*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Aanvraag-ID ontvangen van de client.                                                                   |
| x-ms-correlationid | Ja          | Correlatie-ID is als doorgegeven door de client, anders wordt dit de correlatie-id op.                   |
| x-ms-activityid    | Ja          | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Dit wordt gebruikt voor alle aansluitingen. |
| Opnieuw proberen na        | Nee           | Interval met welke client u kunt de status controleren.                                                       |
| eTag               | Ja          | Koppelen aan een resource om op te halen van de status van de bewerking.                                                        |
|  |  |  |

### <a name="get-subscriptions"></a>Abonnementen ophalen

De Get-actie op abonnementen eindpunt kan een gebruiker om op te halen van alle abonnementen voor alle aanbiedingen in de ISV.

*Aanvraag*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **Parameternaam**  | **Description**                                       |
|---------------------|-------------------------------------------------------|
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**     | **Vereist** | **Description**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client. Het beste een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.             |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                    |
|  |  |  |

*Hoofdtekst van de reactie*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Parameternaam**     | **Gegevenstype** | **Description**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Abonnement-resource-ID van SaaS in Azure.    |
| offerId                | String        | Aanbiedings-ID die de gebruiker een abonnement op.         |
| planId                 | String        | Plan-ID die de gebruiker een abonnement op.          |
| saasSubscriptionName   | String        | De naam van de SaaS-abonnement.                |
| saasSubscriptionStatus | Enum          | Status van de bewerking.  Een van de volgende:  <br/> - `Subscribed`: Abonnement is actief.  <br/> - `Pending`: Gebruiker maken van de resource, maar deze niet is geactiveerd door de ISV.   <br/> - `Unsubscribed`: Gebruiker heeft zich afgemeld.   <br/> - `Suspended`: Gebruiker heeft het abonnement onderbroken.   <br/> - `Deactivated`:  Azure-abonnement is onderbroken.  |
| gemaakt                | DateTime      | Abonnement maken van de waarde van het tijdstempel in UTC. |
| lastModified           | DateTime      | Abonnement gewijzigd tijdstempelwaarde in UTC. |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Description**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | De get-aanvraag is opgelost en de hoofdtekst van het antwoord bevat.    |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of er is een ongeldige api-versie opgegeven. |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                      |
| 404                  | `NotFound`           | Het abonnement niet vinden met de opgegeven ID                                     |
| 429                  | `RequestThrottleId`  | -Service is bezet het verwerken van aanvragen, probeer het later opnieuw.                     |
| 503                  | `ServiceUnavailable` | Service is tijdelijk niet beschikbaar. Probeer het later opnieuw.                             |
|  |  |  |

*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Description**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Aanvraag-ID ontvangen van de client.                                                                   |
| x-ms-correlationid | Ja          | Correlatie-ID is als doorgegeven door de client, anders wordt dit de correlatie-id op.                   |
| x-ms-activityid    | Ja          | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Dit wordt gebruikt voor alle aansluitingen. |
| Opnieuw proberen na        | Nee           | Interval met welke client u kunt de status controleren.                                                       |
|  |  |  |

### <a name="saas-webhook"></a>SaaS Webhook

Een SaaS-webhook wordt gebruikt voor het melden van wijzigingen proactief in de SaaS-service. Deze POST-API wordt verwacht dat deze worden niet-geverifieerde en wordt aangeroepen door de Microsoft-service. De SaaS-service wordt verwacht voor het aanroepen van de operations-API om te verifiëren en autoriseren voordat actie wordt ondernomen op de webhook-melding. 

*Hoofdtekst*

``` json
  {
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "action": "Subscribe", // Subscribe/Unsubscribe/ChangePlan
    "operationRequestSource":"Azure",
    "timeStamp":"2018-12-01T00:00:00"
  }
```

| **Parameternaam**     | **Gegevenstype** | **Description**                               |
|------------------------|---------------|-----------------------------------------------|
| id  | String       | Unieke ID voor de bewerking die wordt geactiveerd.                |
| activityId   | String        | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Dit wordt gebruikt voor alle aansluitingen.               |
| subscriptionId                     | String        | Abonnement-resource-ID van SaaS in Azure.    |
| offerId                | String        | Aanbiedings-ID die de gebruiker een abonnement op. Alleen beschikbaar in de actie 'Update'.        |
| publisherId                | String        | Uitgevers-ID van de SaaS-aanbieding         |
| planId                 | String        | Plan-ID die de gebruiker een abonnement op. Alleen beschikbaar in de actie 'Update'.          |
| action                 | String        | De actie die is deze melding wordt geactiveerd. Mogelijke waarden zijn: activeren, verwijderen, onderbreken, reactiveren, bijwerken          |
| timeStamp                 | String        | De waarde van de tijdstempel in UTC waarop deze melding is geactiveerd.          |
|  |  |  |


## <a name="next-steps"></a>Volgende stappen

Ontwikkelaars kunnen ook programmatisch ophalen en manipuleren van werkbelastingen, aanbiedingen en uitgever profielen met behulp van de [Cloud Partner Portal REST-API's](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
