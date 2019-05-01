---
title: Registreren van een SaaS-toepassing | Azure Marketplace
description: Wordt uitgelegd over het registreren van een SaaS-toepassing met behulp van de Azure portal.
services: Azure, Marketplace, Cloud Partner Portal, Azure portal
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: pabutler
ms.openlocfilehash: 1edaf89c056918f640a905b99d01775273b2c133
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64941933"
---
# <a name="register-a-saas-application"></a>Een SaaS-toepassing registreren

In dit artikel wordt uitgelegd hoe u voor het registreren van een SaaS-toepassing met behulp van de Microsoft [Azure-portal](https://portal.azure.com/).  Na een succesvolle registratie ontvangt u een beveiligingstoken van Azure Active Directory (Azure AD) die u gebruiken kunt voor toegang tot de SaaS-API's voor uitvoering.  Zie voor meer informatie over Azure AD, [wat is verificatie?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios)


## <a name="service-to-service-authentication-flow"></a>Service-naar-serviceverificatie stroom

Het volgende diagram toont de stroom van het abonnement van een nieuwe klant en wanneer deze API's worden gebruikt:

![SaaS biedt een API-stroom](./media/saas-offer-publish-api-flow-v1.png)

Azure legt geen beperkingen op de verificatie die de SaaS-service beschikbaar aan de eindgebruikers stelt. Verificatie met de SaaS-API's voor uitvoering wordt echter uitgevoerd met een Azure AD security-token, doorgaans verkregen door het registreren van de SaaS-app via de Azure-portal. 


## <a name="register-an-azure-ad-secured-app"></a>Een Azure AD-beveiligde app registreren

Elke toepassing die de mogelijkheden van Azure Active Directory wil gebruiken, moet eerst in een Azure Active Directory-tenant worden geregistreerd. Dit registratieproces bestaat uit Azure AD-details over uw toepassing, zoals de URL waar deze zich, de URL om te antwoorden verzenden nadat een gebruiker is geverifieerd, geeft de URI die u de app, enzovoort identificeert.  Voor het registreren van een nieuwe toepassing met behulp van de Azure portal, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com/).
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

        Voor specifieke voorbeelden voor webtoepassingen of systeemeigen toepassingen, bekijk de Snelstartgids begeleide instellingen die beschikbaar zijn in de *aan de slag* sectie van de [Azure AD-handleiding voor ontwikkelaars](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).

5.  Klik op **Create** als u klaar bent. Azure AD wijst een unieke *toepassings-ID* voor uw toepassing, en u\'re genomen om uw toepassing\'pagina voor de belangrijkste registratie s. Afhankelijk van of uw toepassing een webtoepassing of systeemeigen toepassing is, worden er verschillende opties geboden om extra mogelijkheden aan uw toepassing toe te voegen.

>[!Note]
>De zojuist geregistreerde toepassing is standaard geconfigureerd dat alleen gebruikers van dezelfde tenant aanmelden bij uw toepassing.


## <a name="using-the-azure-ad-security-token"></a>Met behulp van de Azure AD security-token

Nadat u uw toepassing hebt geregistreerd, kunt u programmatisch een Azure AD security-token aanvragen.  De uitgever wordt verwacht voor gebruik van dit token, en maken van een aanvraag om dit te verhelpen.  Wanneer u de verschillende vervulling-API's gebruikt, is de queryparameter van de token in de URL wanneer de gebruiker wordt omgeleid naar SaaS-website van Azure.  Dit token is alleen geldig voor één uur.  Bovendien moet u de URL van de token waarde vanuit de browser worden ontsleuteld voordat u deze gebruikt.

Zie voor meer informatie over deze tokens [Azure Active Directory-toegangstokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens).


### <a name="get-a-token-based-on-the-azure-ad-app"></a>Een op basis van de Azure AD-app-token verkrijgen

HTTP-methode

`GET`

*Aanvraag-URL*

**https://login.microsoftonline.com/*{tenant-id}*/oauth2/token**

*URI-parameter*

|  **Parameternaam**  | **Vereist**  | **Beschrijving**                               |
|  ------------------  | ------------- | --------------------------------------------- |
| tenantId             | True          | Tenant-ID van de geregistreerde AAD-toepassing   |
|  |  |  |


*Aanvraagheader*

|  **Header-naam**  | **Vereist** |  **Beschrijving**                                   |
|  --------------   | ------------ |  ------------------------------------------------- |
|  Content-Type     | True         | Type van de inhoud die is gekoppeld aan de aanvraag. De standaardwaarde is `application/x-www-form-urlencoded`.  |
|  |  |  |


*Aanvraagtekst*

| **Eigenschapsnaam**   | **Vereist** |  **Beschrijving**                                                          |
| -----------------   | -----------  | ------------------------------------------------------------------------- |
|  Grant_type         | True         | Toekenningstype. De standaardwaarde is `client_credentials`.                    |
|  Client_id          | True         |  Client/app-id die is gekoppeld aan de Azure AD-app.                  |
|  client_secret      | True         |  Het wachtwoord dat is gekoppeld aan de Azure AD-app.                               |
|  Resource           | True         |  De doelresource waarvoor het token is aangevraagd. De standaardwaarde is `62d94f6c-d599-489b-a797-3e10e42fbe22`. |
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


## <a name="next-steps"></a>Volgende stappen

Uw Azure AD-beveiligde app kan nu gebruikmaken van de [SaaS vervulling API-versie 2](./cpp-saas-fulfillment-api-v2.md).
