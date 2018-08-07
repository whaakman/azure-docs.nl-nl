---
title: Een API beveiligen met behulp van OAuth 2.0 met Azure Active Directory en API Management | Microsoft Docs
description: Informatie over het beveiligen van een web-API-back-end met Azure Active Directory en API Management.
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
ms.openlocfilehash: 06350d30999cb056babbd001f98a6c3a5fdbac6c
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576991"
---
# <a name="protect-an-api-by-using-oauth-20-with-azure-active-directory-and-api-management"></a>Een API beveiligen met behulp van OAuth 2.0 met Azure Active Directory en API Management

Deze handleiding laat zien hoe het configureren van uw Azure API Management-exemplaar voor het beveiligen van een API met behulp van de OAuth 2.0-protocol met Azure Active Directory (Azure AD). 

## <a name="prerequisites"></a>Vereisten
Als u wilt volgen de stappen in dit artikel, moet u het volgende hebben:
* Exemplaar van API Management
* Een API die wordt gepubliceerd en die gebruikmaakt van de API Management-exemplaar
* Een Azure AD-tenant

## <a name="overview"></a>Overzicht

Hier volgt een kort overzicht van de stappen uit:

1. Registreert een toepassing (back-end-app) in Azure AD om weer te geven van de API.
2. Een andere toepassing (client-app) registreren in Azure AD om weer te geven van een clienttoepassing die u moet de API aan te roepen.
3. Verleent machtigingen om de client-app voor het aanroepen van de back-end-app in Azure AD.
4. Configureer de Developer-Console voor het gebruik van gebruikersautorisatie OAuth 2.0.
5. Voeg de **valideren jwt** beleid voor het valideren van het OAuth-token voor elke inkomende aanvraag.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>Een toepassing registreren in Azure AD om weer te geven van de API

Ter bescherming van een API met Azure AD, wordt de eerste stap is het registreren van een toepassing in Azure AD die staat voor de API. 

1. Blader naar uw Azure AD-tenant en blader vervolgens naar **App-registraties**.

2. Selecteer **Nieuwe toepassing registreren**. 

3. Geef een naam op van de toepassing. (Voor dit voorbeeld is de naam `backend-app`.)  

4. Kies **Web-app / API** als de **toepassingstype**. 

5. Voor **aanmeldings-URL**, kunt u `https://localhost` als tijdelijke aanduiding.

6. Selecteer **Maken**.

Wanneer de toepassing wordt gemaakt, neem notitie van de **toepassings-ID**, voor gebruik in een latere stap. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Een andere toepassing registreren in Azure AD om weer te geven van een clienttoepassing

Elke clienttoepassing die de API-aanroepen moet worden geregistreerd als een toepassing in Azure AD ook. In dit voorbeeld is de voorbeeldtoepassing van de client de Developer-Console in de API Management-portal voor ontwikkelaars. Hier volgt een andere toepassing registreren in Azure AD om weer te geven van de Developer-Console.

1. Selecteer **Nieuwe toepassing registreren**. 

2. Geef een naam op van de toepassing. (Voor dit voorbeeld is de naam `client-app`.)

3. Kies **Web-app / API** als de **toepassingstype**.  

4. Voor **aanmeldings-URL**, kunt u `https://localhost` als een tijdelijke aanduiding, of gebruik de aanmelding-URL van uw exemplaar van API Management. (In dit voorbeeld is de URL `https://contoso5.portal.azure-api.net/signin`.)

5. Selecteer **Maken**.

Wanneer de toepassing wordt gemaakt, neem notitie van de **toepassings-ID**, voor gebruik in een latere stap. 

Maak nu een clientgeheim voor deze toepassing, voor gebruik in een latere stap.

1. Selecteer **instellingen** opnieuw, en Ga naar **sleutels**.

2. Onder **wachtwoorden**, bieden een **Sleutelbeschrijving**. Kies wanneer de sleutel moet vervalt, en selecteer **opslaan**.

Noteer de sleutelwaarde. 

## <a name="grant-permissions-in-azure-ad"></a>Machtigingen verlenen in Azure AD

Nu u twee toepassingen voor de Developer-Console en de API hebt geregistreerd, moet u machtigingen om de client-app voor het aanroepen van de back-end-app.  

1. Blader naar **toepassingsregistraties**. 

2. Selecteer `client-app`, en Ga naar **instellingen**.

3. Selecteer **vereiste machtigingen** > **toevoegen**.

4. Selecteer **Select an API**, en zoek naar de `backend-app`.

5. Onder **gedelegeerde machtigingen**, selecteer `Access backend-app`. 

6. Selecteer **Selecteer**, en selecteer vervolgens **gedaan**. 

> [!NOTE]
> Als **Azure Active Directory** niet wordt vermeld onder de machtigingen voor andere toepassingen, selecteer **toevoegen** toe te voegen in de lijst.
> 
> 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>De gebruikersautorisatie OAuth 2.0 in de Developer-Console inschakelen

Op dit moment u uw toepassingen in Azure AD hebt gemaakt en juiste machtigingen hebt om de client-app voor het aanroepen van de back-end-app. 

In dit voorbeeld is de Developer-Console de client-app. De volgende stappen wordt beschreven hoe u de gebruikersautorisatie OAuth 2.0 in de Developer-Console inschakelen. 

1. Blader naar de API Management-instantie.

2. Selecteer **OAuth 2.0** > **toevoegen**.

3. Geef een **weergavenaam** en **beschrijving**.

4. Voor de **URL voor de clientregistratiepgina**, voer een tijdelijke aanduiding invoegen, zoals `http://localhost`. De **URL voor de clientregistratiepgina** verwijst naar de pagina waarmee gebruikers kunnen maken en configureren van hun eigen accounts voor OAuth 2.0-providers die dit ondersteunen. In dit voorbeeld gebruikers niet maken en configureren van hun eigen accounts, zodat u een tijdelijke aanduiding in plaats daarvan gebruiken.

5. Voor **toekenningstypen**, selecteer **autorisatiecode**.

6. Geef de **autorisatie-eindpunt-URL** en **Token-eindpunt-URL**. Ophalen van deze waarden uit de **eindpunten** pagina in uw Azure AD-tenant. Blader naar de **App-registraties** pagina opnieuw en selecteer **eindpunten**.

7. Kopieer de **OAuth 2.0-autorisatie-eindpunt**, en plak deze in de **autorisatie-eindpunt-URL** in het tekstvak.

8. Kopieer de **OAuth 2.0 Token Endpoint**, en plak deze in de **Token-eindpunt-URL** in het tekstvak. Naast het plakken in het token-eindpunt toevoegen een van de hoofdtekstparameter met de naam **resource**. Gebruik voor de waarde van deze parameter, de **toepassings-ID** voor de back-end-app.

9. Geef vervolgens de referenties van de client. Dit zijn de referenties voor de client-app.

10. Voor **Client-ID**, gebruikt u de **toepassings-ID** voor de client-app.

11. Voor **clientgeheim**, eerder gebruikt u de sleutel die u hebt gemaakt voor de client-app. 

12. Onmiddellijk na het clientgeheim is de **redirect_url** voor de autorisatie code toekenningstype. Noteer deze URL.

13. Selecteer **Maken**.

14. Ga terug naar de **instellingen** pagina van uw client-app.

15. Selecteer **antwoord-URL's**, en plak de **redirect_url** in de eerste rij. In dit voorbeeld vervangen u `https://localhost` met de URL in de eerste rij.  

Nu dat u een OAuth 2.0-autorisatie-server hebt geconfigureerd, kunnen de Ontwikkelaarsconsole access tokens verkrijgen van Azure AD. 

De volgende stap is om in te schakelen gebruikersautorisatie OAuth 2.0 voor uw API. Hierdoor kunnen de Developer-Console om te weten dat nodig om op te halen van een toegangstoken namens de gebruiker voordat u uw API-aanroepen.

1. Blader naar de API Management-instantie en gaat u naar **API's**.

2. Selecteer de API die u wilt beveiligen. In dit voorbeeld gebruikt u de `Echo API`.

3. Ga naar **instellingen**.

4. Onder **Security**, kiest u **OAuth 2.0**, en selecteer de OAuth 2.0-server die u eerder hebt geconfigureerd. 

5. Selecteer **Opslaan**.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Is de API aanroepen vanuit de ontwikkelaarsportal

Nu dat de gebruikersautorisatie OAuth 2.0 is ingeschakeld op de `Echo API`, de Ontwikkelaarsconsole verkrijgt een toegangstoken namens de gebruiker voordat u de API aanroept.

1. Blader naar een bewerking onder de `Echo API` in het developer portal en selecteer **uitproberen**. Hiermee keert u terug naar de Developer-Console.

2. Houd er rekening mee een nieuw item in de **autorisatie** sectie, overeenkomt met de autorisatie-server die u zojuist hebt toegevoegd.

3. Selecteer **autorisatiecode** uit de autorisatie vervolgkeuzelijst, en u wordt gevraagd om aan te melden bij de Azure AD-tenant. Als u bent al aangemeld met het account, u mogelijk niet gevraagd.

4. Na geslaagde aanmelding, een `Authorization` header is toegevoegd aan de aanvraag, met een toegangstoken van Azure AD. Hier volgt een voorbeeld-token (Base64-gecodeerd):

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

5. Selecteer **verzenden**, en kunt u de API kunnen aanroepen.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Configureer een beleid van de validatie van JWT voor vooraf machtigen aanvragen

Op dit moment, wanneer een gebruiker probeert te maken van een aanroep van de Developer-Console, wordt de gebruiker gevraagd aan te melden. De Developer-Console worden opgehaald van een toegangstoken namens de gebruiker.

Maar wat gebeurt er als iemand anders uw API zonder een token of met een ongeldig token aanroepen? Bijvoorbeeld, u kunt nog steeds de API aanroept, zelfs als u verwijdert de `Authorization` header. De reden is dat API Management wordt niet gevalideerd het toegangstoken op dit moment. Het gewoon wordt doorgegeven de `Authorization` koptekst naar de back-end-API.

U kunt de [JWT valideren](api-management-access-restriction-policies.md#ValidateJWT) beleid voor vooraf machtigen aanvragen in API Management, door het valideren van de toegangstokens te geven van elke inkomende aanvraag. Als een aanvraag heeft geen geldig token, wordt het door de API Management blokkeert. U kunt bijvoorbeeld het volgende beleid toevoegen de `<inbound>` beleid-gedeelte van de `Echo API`. Deze controleert de claim doelgroep in een toegangstoken en stuurt een foutbericht als het token niet geldig is. Zie voor meer informatie over het configureren van beleid [instellen of bewerken van beleid](set-edit-policies.md).

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

## <a name="build-an-application-to-call-the-api"></a>De API aanroepen van een toepassing bouwen

In deze handleiding, gebruikt u de Developer-Console in API Management als de voorbeeldtoepassing van de client om aan te roepen de `Echo API` beveiligd met OAuth 2.0. Zie voor meer informatie over hoe u een toepassing bouwen en implementeren van OAuth 2.0, [codevoorbeelden van Azure Active Directory](../active-directory/develop/sample-v1-code.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Azure Active Directory en OAuth 2.0](../active-directory/develop/authentication-scenarios.md).
* Bekijk meer [video's](https://azure.microsoft.com/documentation/videos/index/?services=api-management) over API Management.
* Zie voor andere manieren voor het beveiligen van uw back-endservice [wederzijdse certificaatverificatie](api-management-howto-mutual-certificates.md).

* [API Management service-exemplaar maken](get-started-create-service-instance.md).

* [Uw eerste API beheren](import-and-publish.md).
