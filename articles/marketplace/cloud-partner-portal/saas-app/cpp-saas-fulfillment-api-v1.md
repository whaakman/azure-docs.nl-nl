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
ms.date: 02/27/2019
ms.author: pbutlerm
ms.openlocfilehash: 299c06fc043391fc9b765c95ec6d29da3d440719
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57318871"
---
# <a name="saas-fulfillment-apis-version-1"></a>SaaS vervulling API's versie 1

In dit artikel wordt uitgelegd hoe u een SaaS-aanbieding maken met API's. De API's die nodig zijn voor het toestaan van abonnementen voor uw SaaS-aanbieding hebt u verkopen via Azure geselecteerd.  

> [!WARNING]
> Deze eerste versie van de vervulling SaaS API is afgeschaft; in plaats daarvan gebruik [SaaS vervulling-API V2](./cpp-saas-fulfillment-api-v2.md).


In dit artikel is onderverdeeld in twee secties:

-   Service-naar-Serviceverificatie tussen een SaaS-Service en de Azure Marketplace
-   API-methoden en eindpunten

De volgende API's zijn bedoeld om u uw SaaS-service te integreren met Azure:

-   Oplossen
-   Abonneer u nu
-   Converteren
-   Afmelden

Het volgende diagram toont de stroom van het abonnement van een nieuwe klant en wanneer deze API's worden gebruikt:

![SaaS biedt een API-stroom](./media/saas-offer-publish-api-flow-v1.png)


## <a name="service-to-service-authentication-between-saas-service-and-azure-marketplace"></a>Service-verificatie tussen SaaS-service en Azure marketplace-service

Azure legt geen beperkingen op de verificatie die de SaaS-service beschikbaar aan de eindgebruikers stelt. Echter, als het gaat om de SaaS-service communiceert met de Azure Marketplace-API's, de verificatie wordt uitgevoerd in de context van een Azure Active Directory (Azure AD)-toepassing.

De volgende sectie wordt beschreven hoe u een Azure AD-toepassing maken.


### <a name="register-an-azure-ad-application"></a>Een Azure AD-toepassing registreren

Elke toepassing die de mogelijkheden van Azure Active Directory wil gebruiken, moet eerst in een Azure Active Directory-tenant worden geregistreerd. Dit registratieproces bestaat uit Azure AD-details over uw toepassing, zoals de URL waar deze zich, de URL om te antwoorden verzenden nadat een gebruiker is geverifieerd, geeft de URI die u de app, enzovoort identificeert.

Voor het registreren van een nieuwe toepassing met behulp van de Azure portal, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij [Azure-portal](https://portal.azure.com/).
2.  Als u via uw account toegang tot meer dan één tenant hebt, klikt u op uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
3.  Klik in het navigatiedeelvenster links op de **Azure Active Directory** service, klikt u op **App-registraties**, en klikt u op **nieuwe toepassing registreren**.

    ![SaaS AD App-registraties](./media/saas-offer-app-registration-v1.png)

4.  Voer op de pagina voor het maken, uw toepassing\'s registratie-informatie:
    -   **Naam**: Voer een nuttige naam in voor de toepassing
    -   **Toepassingstype**: 
        - Selecteer **Systeemeigen** voor [clienttoepassingen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) die lokaal op een apparaat zijn geïnstalleerd. Deze instelling wordt gebruikt voor openbare [systeemeigen clients](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#native-client) met OAuth.
        - Selecteer **Web-app / API** voor [clienttoepassingen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) en [resource/API-Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#resource-server) die op een beveiligde server worden geïnstalleerd. Deze instelling wordt gebruikt voor OAuth vertrouwelijke [web-clients](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#web-client) en openbare [gebruiker agent-gebaseerde clients](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client).
        Dezelfde toepassing kan zowel een client als resource/API beschikbaar maken.
    -   **Aanmeldings-URL**: Voor Web-app/API-Apps, geeft u de basis-URL van uw app. Bijvoorbeeld, **http://localhost:31544** mogelijk de URL voor een WebApp die wordt uitgevoerd op uw lokale computer. Gebruikers zouden deze URL vervolgens gebruiken voor het aanmelden bij een web-clienttoepassing.
    -   **Omleidings-URI**: Systeemeigen toepassing maakt, geeft u de URI die door Azure AD wordt gebruikt om tokenantwoorden te retourneren. Voer een specifieke waarde aan uw toepassing, bijvoorbeeld **http://MyFirstAADApp**.

        ![SaaS AD App-registraties](./media/saas-offer-app-registration-v1-2.png)

        Voor specifieke voorbeelden voor webtoepassingen of systeemeigen toepassingen, bekijk de Snelstartgids begeleide instellingen die beschikbaar in de sectie aan de slag van zijn de [Azure AD-handleiding voor ontwikkelaars](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).

5.  Klik op **Create** als u klaar bent. Azure AD wijst een unieke toepassings-ID toe voor uw toepassing, en u\'re genomen om uw toepassing\'pagina voor de belangrijkste registratie s. Afhankelijk van of uw toepassing een webtoepassing of systeemeigen toepassing is, worden er verschillende opties geboden om extra mogelijkheden aan uw toepassing toe te voegen.

>[!Note]
>De zojuist geregistreerde toepassing is standaard geconfigureerd dat alleen gebruikers van dezelfde tenant aanmelden bij uw toepassing.

<a name="api-methods-and-endpoints"></a>API-methoden en eindpunten
-------------------------

De volgende secties beschrijven de API-methoden en eindpunten beschikbaar voor het inschakelen van abonnementen voor een SaaS-aanbod.

### <a name="get-a-token-based-on-the-azure-ad-app"></a>Een op basis van de Azure AD-app-token verkrijgen

HTTP-methode

`GET`

*Aanvraag-URL*

**https://login.microsoftonline.com/*{tenant-id}*/oauth2/token**

*URI-parameter*

|  **Parameternaam**  | **Vereist**  | **Beschrijving**                               |
|  ------------------  | ------------- | --------------------------------------------- |
| tenantId             | Waar          | Tenant-ID van de geregistreerde AAD-toepassing   |
|  |  |  |


*Aanvraagheader*

|  **Header-naam**  | **Vereist** |  **Beschrijving**                                   |
|  --------------   | ------------ |  ------------------------------------------------- |
|  Content-Type     | Waar         | Type van de inhoud die is gekoppeld aan de aanvraag. De standaardwaarde is `application/x-www-form-urlencoded`.  |
|  |  |  |


*Aanvraagtekst*

| **Eigenschapsnaam**   | **Vereist** |  **Beschrijving**                                                          |
| -----------------   | -----------  | ------------------------------------------------------------------------- |
|  Grant_type         | Waar         | Toekenningstype. De standaardwaarde is `client_credentials`.                    |
|  Client_id          | Waar         |  Client/app-id die is gekoppeld aan de Azure AD-app.                  |
|  client_secret      | Waar         |  Het wachtwoord dat is gekoppeld aan de Azure AD-app.                               |
|  Resource           | Waar         |  De doelresource waarvoor het token is aangevraagd. De standaardwaarde is `62d94f6c-d599-489b-a797-3e10e42fbe22`. |
|  |  |  |


*Antwoord*

|  **Naam**  | **Type**       |  **Beschrijving**    |
| ---------- | -------------  | ------------------- |
| 200 OK    | TokenResponse  | De aanvraag is voltooid   |
|  |  |  |

*TokenResponse*

Het antwoordtoken voorbeeld:

``` json
  {
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "62d94f6c-d599-489b-a797-3e10e42fbe22",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }               
```

### <a name="marketplace-api-endpoint-and-api-version"></a>Marketplace API-eindpunt en de API-versie

Het eindpunt voor de API van Azure Marketplace is `https://marketplaceapi.microsoft.com`.

De huidige API-versie is `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Abonnement oplossen

Actie na de op oplossen eindpunt kan gebruikers een marketplace-token worden omgezet in een permanente Resource-ID.  De Resource-ID is de unieke id voor SAAS-abonnement. 

Wanneer een gebruiker wordt omgeleid naar de website van een ISV, bevat de URL een token in de queryparameters. De ISV wordt verwacht voor gebruik van dit token, en maken van een aanvraag om dit te verhelpen. Het antwoord bevat de unieke SAAS abonnements-ID, de naam, de aanbiedings-ID en het plan voor de resource. Dit token is alleen een uur geldig.

*Aanvraag*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Parameternaam** |     **Beschrijving**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  API-versie        |  De versie van de bewerking te gebruiken voor deze aanvraag.   |
|  |  |


*Headers*

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Dit komt overeen met alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| inhoudstype       | Ja          | `application/json`                                        |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                    |
| x-ms-marketplace-token| Ja| De token queryparameter in de URL in wanneer de gebruiker wordt omgeleid naar de website SaaS ISV's van Azure. **Opmerking:** Dit token is alleen geldig voor één uur. Bovendien decoderen URL van de token waarde vanuit de browser voordat u deze gebruikt.|
|  |  |  |
  

*De hoofdtekst van antwoord*

``` json
{
    "id": "",
    "subscriptionName": "",
    "offerId": "",
    "planId": "",
}
```

| **Parameternaam** | **Gegevenstype** | **Beschrijving**                       |
|--------------------|---------------|---------------------------------------|
| id                 | String        | ID van de SaaS-abonnement.          |
| subscriptionName| String| De naam van de SaaS-abonnement instellen door de gebruiker in Azure terwijl u zich abonneert op de SaaS-service.|
| OfferId            | String        | Aanbiedings-ID die de gebruiker een abonnement op. |
| planId             | String        | Plan-ID die de gebruiker een abonnement op.  |
|  |  |  |


*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Beschrijving**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Token is opgelost.                                                            |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of een ongeldige api-versie die is opgegeven. Kan het token niet ophalen omdat een van beide het token ongeldig of verlopen is (het token is alleen geldig voor 1 uur eenmaal gegenereerd). |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                                 |
| 429                  | `RequestThrottleId`  | Service is bezet verwerken van aanvragen, voer later opnieuw uit.                                |
| 503                  | `ServiceUnavailable` | Service is omlaag tijdelijk, probeer het later opnieuw.                                        |
|  |  |  |


*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Ja          | Aanvraag-ID ontvangen van de client.                                                                   |
| x-ms-correlationid | Ja          | Correlatie-ID is als doorgegeven door de client, anders wordt deze waarde de correlatie-id op.                   |
| x-ms-activityid    | Ja          | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Dit wordt gebruikt voor alle aansluitingen. |
| Opnieuw proberen na        | Nee           | Deze waarde wordt alleen ingesteld voor een fout 429.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Abonneer u nu

Het eindpunt abonneren kan gebruikers een abonnement om een SaaS-service voor een bepaald abonnement te starten en inschakelen van facturering in het commerce-systeem.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Parameternaam**  | **Beschrijving**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | De unieke Id van saas-abonnement dat is verkregen na het oplossen van het token via API oplossen.                              |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

|  **Header-sleutel**        | **Vereist** |  **Beschrijving**                                                  |
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

| **De naam van element** | **Gegevenstype** | **Beschrijving**                      |
|------------------|---------------|--------------------------------------|
| planId           | (Vereist) Tekenreeks        | Plan-Id van de gebruiker van de SaaS-service is geabonneerd op.  |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Beschrijving**                                                           |
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

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                        |
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

| **Parameternaam**  | **Beschrijving**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID van de SaaS-abonnement.                              |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**          | **Vereist** | **Beschrijving**                                                                                                                                                                                                                  |
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

|  **De naam van element** |  **Gegevenstype**  | **Beschrijving**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  (Vereist) Tekenreeks         | Plan-Id van de gebruiker van de SaaS-service is geabonneerd op.          |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Beschrijving**                                                           |
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

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                        |
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

| **Parameternaam**  | **Beschrijving**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID van de SaaS-abonnement.                              |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client. Het beste een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.                                                           |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als dit niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                    |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Beschrijving**                                                           |
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

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                        |
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

**https://marketplaceapi.microsoft.com/api/saas/operations/*{bewerkings-id}*?api-version=2017-04-15**

| **Parameternaam**  | **Beschrijving**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | Unieke ID voor de bewerking die wordt geactiveerd.                |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client. Het beste een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.   |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.  |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                    |
|  |  |  | 

*De hoofdtekst van antwoord*

```json
{
    "id": "",
    "status": "",
    "resourceLocation": "",
    "created": "",
    "lastModified": ""
}
```

| **Parameternaam** | **Gegevenstype** | **Beschrijving**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | String        | ID van de bewerking.                                                                      |
| status             | Enum          | Status van de bewerking, een van de volgende: `In Progress`, `Succeeded`, of `Failed`.          |
| resourceLocation   | String        | Koppelen aan het abonnement dat is gemaakt of gewijzigd. Hiermee wordt de client om op te halen van de geüpdate toestand post-bewerking. Deze waarde is niet ingesteld voor `Unsubscribe` bewerkingen. |
| gemaakt            | DateTime      | Bewerking maken van de tijd in UTC.                                                           |
| lastModified       | DateTime      | Laatste update voor de bewerking die wordt in UTC.                                                      |
|  |  |  |

*Responscodes*

| **HTTP-statuscode** | **Foutcode**     | **Beschrijving**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | De get-aanvraag is opgelost en de hoofdtekst van het antwoord bevat.    |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of er is een ongeldige api-versie opgegeven. |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                      |
| 404                  | `NotFound`           | Het abonnement niet vinden met de opgegeven ID                                     |
| 429                  | `RequestThrottleId`  | Service is bezet verwerken van aanvragen, voer later opnieuw uit.                     |
| 503                  | `ServiceUnavailable` | Service is omlaag tijdelijk, probeer het later opnieuw.                             |
|  |  |  |

*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                        |
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

| **Parameternaam**  | **Beschrijving**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID van de SaaS-abonnement.                              |
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client, bij voorkeur een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.                                                           |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                                                                    |
|  |  |  |

*De hoofdtekst van antwoord*

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
| **Parameternaam**     | **Gegevenstype** | **Beschrijving**                               |
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

| **HTTP-statuscode** | **Foutcode**     | **Beschrijving**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | De get-aanvraag is opgelost en de hoofdtekst van het antwoord bevat.    |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of er is een ongeldige api-versie opgegeven. |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                      |
| 404                  | `NotFound`           | Het abonnement niet vinden met de opgegeven ID                                     |
| 429                  | `RequestThrottleId`  | Service is bezet verwerken van aanvragen, voer later opnieuw uit.                     |
| 503                  | `ServiceUnavailable` | Service is omlaag tijdelijk, probeer het later opnieuw.                             |
|  |  |  |

*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                        |
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

| **Parameternaam**  | **Beschrijving**                                       |
|---------------------|-------------------------------------------------------|
| API-versie         | De versie van de bewerking te gebruiken voor deze aanvraag. |
|  |  |

*Headers*

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Nee           | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de client. Het beste een GUID. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders.             |
| x-ms-correlationid | Nee           | Een unieke tekenreeks-waarde voor de bewerking op de client. Deze waarde is voor het correleren van alle gebeurtenissen van clientbewerking met gebeurtenissen op de server. Als deze waarde niet is opgegeven, wordt een gegenereerd en vindt u in de antwoordheaders. |
| Autorisatie      | Ja          | De JSON web token (JWT) bearer-token.                    |
|  |  |  |

*De hoofdtekst van antwoord*

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

| **Parameternaam**     | **Gegevenstype** | **Beschrijving**                               |
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

| **HTTP-statuscode** | **Foutcode**     | **Beschrijving**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | De get-aanvraag is opgelost en de hoofdtekst van het antwoord bevat.    |
| 400                  | `BadRequest`         | Een vereiste headers ontbreken of er is een ongeldige api-versie opgegeven. |
| 403                  | `Forbidden`          | De aanroeper is niet geautoriseerd deze bewerking uit te voeren.                      |
| 404                  | `NotFound`           | Het abonnement niet vinden met de opgegeven ID                                     |
| 429                  | `RequestThrottleId`  | -Service is bezet het verwerken van aanvragen, probeer het later opnieuw.                     |
| 503                  | `ServiceUnavailable` | Service is tijdelijk niet beschikbaar. Probeer het later opnieuw.                             |
|  |  |  |

*Antwoordheaders*

| **Header-sleutel**     | **Vereist** | **Beschrijving**                                                                                        |
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
    "offerId": "sampleSaaSOffer", // Provided with "Update" action
    "publisherId": "contoso", 
    "planId": "silver",     // Provided with "Update" action
    "action": "Activate", // Activate/Delete/Suspend/Reinstate/Update
    "timeStamp": "2018-12-01T00:00:00"
  }
```

| **Parameternaam**     | **Gegevenstype** | **Beschrijving**                               |
|------------------------|---------------|-----------------------------------------------|
| id  | String       | Unieke ID voor de bewerking die wordt geactiveerd.                |
| activityId   | String        | Een unieke tekenreeks-waarde voor het bijhouden van de aanvraag van de service. Dit wordt gebruikt voor alle aansluitingen.               |
| subscriptionId                     | String        | Abonnement-resource-ID van SaaS in Azure.    |
| offerId                | String        | Aanbiedings-ID die de gebruiker een abonnement op. Alleen beschikbaar in de actie 'Update'.        |
| publisherId                | String        | Uitgevers-ID van de SaaS-aanbieding         |
| planId                 | String        | Plan-ID die de gebruiker een abonnement op. Alleen beschikbaar in de actie 'Update'.          |
| Actie                 | String        | De actie die is deze melding wordt geactiveerd. Mogelijke waarden zijn: activeren, verwijderen, onderbreken, reactiveren, bijwerken          |
| timeStamp                 | String        | De waarde van de tijdstempel in UTC waarop deze melding is geactiveerd.          |
|  |  |  |


## <a name="next-steps"></a>Volgende stappen

Ontwikkelaars kunnen ook programmatisch ophalen en manipuleren van werkbelastingen, aanbiedingen en uitgever profielen met behulp van de [Cloud Partner Portal REST-API's](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
