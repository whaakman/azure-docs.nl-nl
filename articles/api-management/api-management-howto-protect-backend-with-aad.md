---
title: Een API beveiligen met behulp van OAuth 2.0 met Azure Active Directory en API Management | Microsoft Docs
description: Informatie over het beveiligen van een web API-backend met Azure Active Directory en API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2018
ms.author: apimpm
ms.openlocfilehash: f5662a4082487137dfd642cc3264a90f8ab19054
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="protect-an-api-by-using-oauth-20-with-azure-active-directory-and-api-management"></a>Een API beveiligen met behulp van OAuth 2.0 met Azure Active Directory en API Management

Deze handleiding wordt beschreven hoe u uw Azure API Management-exemplaar voor een API beveiligen met behulp van OAuth 2.0-protocol met Azure Active Directory (Azure AD) configureren. 

## <a name="prerequisites"></a>Vereisten
Als u wilt de stappen in dit artikel, moet u het volgende hebben:
* Exemplaar van API Management
* Een API die wordt gepubliceerd en die gebruikmaakt van het exemplaar van API Management
* Een Azure AD-tenant

## <a name="overview"></a>Overzicht

Hier volgt een snel overzicht van de stappen uit:

1. Registreert een toepassing (back-end-app) in Azure AD om weer te geven van de API.
2. Een andere toepassing (client-app) in Azure AD vertegenwoordigt een clienttoepassing die de API aan te roepen moet registreren.
3. Verleent machtigingen voor het toestaan van de client-app aanroepen van de back-end-app in Azure AD.
4. Configureer de Developer-Console voor het gebruik van OAuth 2.0-gebruikersautorisatie.
5. Voeg de **valideren jwt** beleid voor het valideren van het OAuth-token voor elke inkomende aanvraag.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>Een toepassing registreren in Azure AD om weer te geven van de API

Als u wilt beveiligen een API met Azure AD, worden de eerste stap is een toepassing registreren in Azure AD dat staat voor de API. 

1. Blader naar uw Azure AD-tenant en blader vervolgens naar **App registraties**.

2. Selecteer **Nieuwe toepassing registreren**. 

3. Geef een naam van de toepassing. (Voor dit voorbeeld is de naam `backend-app`.)  

4. Kies **Web-app / API** als de **toepassingstype**. 

5. Voor **aanmeldings-URL**, kunt u `https://localhost` als tijdelijke aanduiding.

6. Selecteer **Maken**.

Wanneer de toepassing wordt gemaakt, noteert u de **toepassings-ID**, voor gebruik in een latere stap. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Een andere toepassing registreren in Azure AD vertegenwoordigt een clienttoepassing

Elke clienttoepassing die de API-aanroepen moet worden geregistreerd als een toepassing in Azure AD ook. In dit voorbeeld is de voorbeeldtoepassing voor de client de Developer-Console in de API Management-portal voor ontwikkelaars. Hier volgt een andere toepassing registreren in Azure AD voor de Developer-Console.

1. Selecteer **Nieuwe toepassing registreren**. 

2. Geef een naam van de toepassing. (Voor dit voorbeeld is de naam `client-app`.)

3. Kies **Web-app / API** als de **toepassingstype**.  

4. Voor **aanmeldings-URL**, kunt u `https://localhost` als een tijdelijke aanduiding of gebruik de aanmeldingspagina URL van uw exemplaar van API Management. (Bijvoorbeeld, de URL is `https://contoso5.portal.azure-api.net/signin`.)

5. Selecteer **Maken**.

Wanneer de toepassing wordt gemaakt, noteert u de **toepassings-ID**, voor gebruik in een latere stap. 

Maak nu een clientgeheim voor deze toepassing, voor gebruik in een latere stap.

1. Selecteer **instellingen** opnieuw, en Ga naar **sleutels**.

2. Onder **wachtwoorden**, bieden een **beschrijving sleutel**. Kiezen wanneer de sleutel moet vervalt, en selecteer **opslaan**.

Noteer de sleutelwaarde. 

## <a name="grant-permissions-in-azure-ad"></a>Machtigingen toekennen in Azure AD

Nu u twee toepassingen voor de API en de Developer-Console hebt geregistreerd, moet u machtigingen toe zodat de client-app aanroepen van de back-end-app.  

1. Blader naar **toepassing registraties**. 

2. Selecteer `client-app`, en Ga naar **instellingen**.

3. Selecteer **vereist machtigingen** > **toevoegen**.

4. Selecteer **selecteert u een API**, en zoek naar `backend-app`.

5. Onder **gedelegeerde machtigingen**, selecteer `Access backend-app`. 

6. Selecteer **Selecteer**, en selecteer vervolgens **gedaan**. 

> [!NOTE]
> Als **Azure Active Directory** niet wordt vermeld bij de machtigingen voor andere toepassingen, selecteer **toevoegen** toe te voegen in de lijst.
> 
> 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>OAuth 2.0-gebruikersautorisatie in de Console voor ontwikkelaars inschakelen

Nu u uw toepassingen in Azure AD hebt gemaakt en juiste machtigingen toe zodat de client-app aanroepen van de back-end-app hebt verleend. 

In dit voorbeeld is de Developer-Console de client-app. De volgende stappen beschrijven het inschakelen van OAuth 2.0-gebruikersautorisatie in de Developer-Console. 

1. Blader naar uw API Management-exemplaar.

2. Selecteer **OAuth 2.0** > **toevoegen**.

3. Geef een **weergavenaam** en **beschrijving**.

4. Voor de **Client de registratie van URL**, voer een aanduidingswaarde van de tijdelijke, zoals `http://localhost`. De **Client de registratie van URL** verwijst naar de pagina die gebruikers gebruiken kunnen voor het maken en hun eigen accounts configureren voor OAuth 2.0-providers die deze ondersteunen. In dit voorbeeld gebruikers niet maken en hun eigen accounts configureren zodat u een tijdelijke aanduiding in plaats daarvan gebruiken.

5. Voor **autorisatie verlenen typen**, selecteer **autorisatiecode**.

6. Geef de **autorisatie eindpunt-URL** en **Token eindpunt-URL**. Ophalen van deze waarden uit de **eindpunten** pagina in uw Azure AD-tenant. Blader naar de **App registraties** pagina opnieuw en selecteer **eindpunten**.

7. Kopieer de **OAuth 2.0-autorisatie eindpunt**, en plak deze in de **autorisatie eindpunt-URL** in het tekstvak.

8. Kopieer de **OAuth 2.0-Tokeneindpunt**, en plak deze in de **Token eindpunt-URL** in het tekstvak. Naast plakken in het token eindpunt toevoegen hoofdtekst parameter met de naam **resource**. Gebruik voor de waarde van deze parameter de **toepassings-ID** voor de back-end-app.

9. Geef vervolgens de referenties van de client. Dit zijn de referenties voor de client-app.

10. Voor **Client-ID**, gebruiken de **toepassings-ID** voor de client-app.

11. Voor **clientgeheim**, voor het gebruik van de sleutel die u hebt gemaakt voor de client-app is het eerder. 

12. Onmiddellijk na het clientgeheim is de **redirect_url** voor de autorisatie code type verlenen. Noteer deze URL.

13. Selecteer **Maken**.

14. Ga terug naar de **instellingen** pagina van uw client-app.

15. Selecteer **antwoord-URL's**, en plak de **redirect_url** in de eerste rij. In dit voorbeeld vervangen u `https://localhost` met de URL in de eerste rij.  

Nu dat u een OAuth 2.0-autorisatie-server hebt geconfigureerd, kunnen de Developer-Console toegangstokens verkrijgen bij Azure AD. 

De volgende stap is het inschakelen van OAuth 2.0-gebruikersautorisatie voor uw API. Hierdoor kunnen de Developer-Console om te weten dat deze verkrijgen van een toegangstoken namens de gebruiker moeten voordat u uw API aanroepen.

1. Blader naar uw exemplaar van API Management en gaat u naar **API's**.

2. Selecteer de API die u wilt beveiligen. In dit voorbeeld gebruikt u de `Echo API`.

3. Ga naar **instellingen**.

4. Onder **beveiliging**, kies **OAuth 2.0**, en selecteer de OAuth 2.0-server die u eerder hebt geconfigureerd. 

5. Selecteer **Opslaan**.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Is de API aanroepen vanuit de portal voor ontwikkelaars

Nu dat het OAuth 2.0-gebruikersautorisatie is ingeschakeld op de `Echo API`, Developer-Console een toegangstoken namens de gebruiker krijgt voordat de API aanroept.

1. Bladeren naar eventuele bewerking onder de `Echo API` in het developer portal en selecteer **Try it**. Hiermee wordt u naar de Developer-Console.

2. Houd er rekening mee een nieuw item in de **autorisatie** sectie overeenkomt met de autorisatie-server die u zojuist hebt toegevoegd.

3. Selecteer **autorisatiecode** van de autorisatie vervolgkeuzelijst en u wordt gevraagd aan te melden bij de Azure AD-tenant. Als u bent al aangemeld bij het account, kunt u niet meer worden gevraagd.

4. Na een geslaagde aanmelden, een `Authorization` header is toegevoegd aan de aanvraag met een toegangstoken van Azure AD. Hier volgt een voorbeeldtoken (Base64-gecodeerd):

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

5. Selecteer **verzenden**, en u kunt de API is aanroepen.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Configureer een beleid voor de validatie van JWT vooraf om aanvragen te verifiëren

Wanneer een gebruiker probeert te maken van een aanroep van de Developer-Console, wordt op dit punt wordt de gebruiker gevraagd aan te melden. De Ontwikkelaarsconsole verkrijgt een toegangstoken namens de gebruiker.

Maar wat gebeurt er als iemand anders uw API zonder een token of met een ongeldig token aanroepen? Bijvoorbeeld, u kunt nog steeds de API aanroepen zelfs als u verwijdert de `Authorization` header. De reden is dat API Management wordt niet gevalideerd het toegangstoken op dit moment. Dit gewoon wordt doorgegeven de `Authorization` header voor de back-end-API.

U kunt de [JWT valideren](api-management-access-restriction-policies.md#ValidateJWT) beleid voor het autoriseren vooraf aanvragen in API Management door het valideren van de toegangstokens voor elke inkomende aanvraag. Als een aanvraag geen geldig token heeft, wordt het door de API Management blokkeert. U kunt bijvoorbeeld het volgende beleid toevoegen de `<inbound>` beleidssectie van de `Echo API`. Deze controleert de claim doelgroep in een toegangstoken en wordt een foutbericht geretourneerd als het token niet geldig is. Zie voor meer informatie over het configureren van beleid [instellen of bewerken van beleid](set-edit-policies.md).

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```

## <a name="build-an-application-to-call-the-api"></a>Maken van een toepassing de API aan te roepen

In deze handleiding, gebruikt u de Developer-Console in API Management als de voorbeeldtoepassing voor de client aan te roepen de `Echo API` beveiligd met OAuth 2.0. Zie voor meer informatie over het ontwikkelen van een toepassing en implementeren van OAuth 2.0, [Azure Active Directory-codevoorbeelden](../active-directory/develop/active-directory-code-samples.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Azure Active Directory en OAuth2.0](../active-directory/develop/active-directory-authentication-scenarios.md).
* Bekijk meer [video's](https://azure.microsoft.com/documentation/videos/index/?services=api-management) over API Management.
* Zie voor andere manieren voor het beveiligen van uw back-endservice [wederzijdse certificaatverificatie](api-management-howto-mutual-certificates.md).

* [Maken van een service-exemplaar van API Management](get-started-create-service-instance.md).

* [Uw eerste API beheren](import-and-publish.md).
