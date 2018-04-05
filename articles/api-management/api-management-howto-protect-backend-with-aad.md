---
title: Beveiligen van een API met behulp van OAuth 2.0 met Azure Active Directory en API Management | Microsoft Docs
description: Informatie over het beveiligen van een Web-API-back-end met Azure Active Directory en API Management.
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
ms.openlocfilehash: 2c05407d761a8848f9e032aa219960cd7ea6fa93
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="how-to-protect-an-api-using-oauth-20-with-azure-active-directory-and-api-management"></a>Het beveiligen van een API met behulp van OAuth 2.0 met Azure Active Directory en API Management

Deze handleiding wordt beschreven hoe u uw exemplaar van API Management (APIM) ter bescherming van een API met het OAuth 2.0-protocol met Azure Active Directory (AAD) configureren. 

## <a name="prerequisite"></a>Vereiste
Als u wilt de stappen in dit artikel, moet u het volgende hebben:
* Een exemplaar APIM
* Een API met behulp van het exemplaar APIM wordt gepubliceerd
* Een Azure AD-tenant

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u een API met OAuth 2.0 in APIM beveiligen. In dit artikel, Azure AD als de autorisatie-Server (OAuth Server) wordt gebruikt. Hieronder volgt een snel overzicht van de stappen uit:

1. Registreert een toepassing (back-end-app) in Azure AD om weer te geven van de API
2. Registreren van een andere toepassing (client-app) in Azure AD vertegenwoordigt een clienttoepassing die u moet de API aanroepen
3. In Azure AD machtigingen verlenen voor client-app aan te roepen back-end-app toestaan
4. De Developer-Console voor het gebruik van OAuth 2.0-gebruikersautorisatie configureren
5. Beleid voor het valideren van het OAuth-token voor elke inkomende aanvraag valideren jwt toevoegen

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>Een toepassing registreren in Azure AD om weer te geven van de API

Als u wilt beveiligen een API met Azure AD, worden de eerste stap is een toepassing registreren in Azure AD dat staat voor de API. 

Ga naar uw Azure AD-tenant en navigeer naar **App registraties**.

Selecteer **registratie van de nieuwe toepassing**. 

Geef een naam van de toepassing. In dit voorbeeld `backend-app` wordt gebruikt.  

Kies **Web-app / API** als de **toepassingstype**. 

Voor **aanmeldings-URL**, kunt u `https://localhost` als tijdelijke aanduiding.

Klik op **maken**.

Zodra de toepassing is gemaakt, noteert u de **toepassings-ID** voor gebruik in een latere stap. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Een andere toepassing registreren in Azure AD vertegenwoordigt een clienttoepassing

Elke clienttoepassing die u moet de API aanroepen moet worden geregistreerd als een toepassing in Azure AD ook. In deze handleiding gebruiken we de Developer-Console in de Ontwikkelaarsportal APIM als de client-voorbeeldtoepassing. 

Er moet een andere toepassing registreren in Azure AD voor de Developer-Console.

Klik op **registratie van de nieuwe toepassing** opnieuw. 

Geef een naam op van de toepassing en kies **Web-app / API** als de **toepassingstype**. In dit voorbeeld `client-app` wordt gebruikt.  

Voor **aanmeldings-URL**, kunt u `https://localhost` als een tijdelijke aanduiding of de URL aanmeldingspagina van uw exemplaar APIM gebruiken. In dit voorbeeld `https://contoso5.portal.azure-api.net/signin` wordt gebruikt.

Klik op **maken**.

Zodra de toepassing is gemaakt, noteert u de **toepassings-ID** voor gebruik in een latere stap. 

Nu moet een clientgeheim voor deze toepassing voor gebruik in een latere stap hebt gemaakt.

Klik op **instellingen** opnieuw en Ga naar **sleutels**.

Onder **wachtwoorden**, bieden een **sleutel beschrijving**, kiezen wanneer de sleutel moet vervalt, en klik op **opslaan**.

Noteer de sleutelwaarde. 

## <a name="grant-permissions-in-aad"></a>Machtigingen in AAD

We hebben nu twee toepassingen voor de API (dat wil zeggen, back-end-app) en de Ontwikkelaarsconsole (dat wil zeggen, client-app) geregistreerd, moeten we machtigingen verlenen voor het toestaan van de client-app aanroepen van de back-end-app.  

Navigeer naar **toepassing registraties** opnieuw. 

Klik op `client-app` en Ga naar **instellingen**.

Klik op **vereist machtigingen** en vervolgens **toevoegen**.

Klik op **selecteert u een API** en zoek naar `backend-app`.

Controleer `Access backend-app` onder **overgedragen machtigingen**. 

Klik op **Selecteer** en vervolgens **gedaan**. 

> [!NOTE]
> Als **Windows** **Azure Active Directory** wordt niet vermeld in de machtigingen voor andere toepassingen, klikt u op **toevoegen** en toe te voegen in de lijst.
> 
> 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>OAuth 2.0-gebruikersautorisatie in de Ontwikkelaarsconsole inschakelen

Op dit moment we onze toepassingen in Azure AD hebt gemaakt en juiste machtigingen toe zodat de client-app aanroepen van de back-end-app hebt verleend. 

In deze handleiding gebruiken we de Developer-Console als de client-app. Onderstaande stappen wordt beschreven hoe u OAuth 2.0-gebruikersautorisatie in de Console voor ontwikkelaars inschakelen 

Navigeer naar uw APIM-exemplaar.

Klik op **OAuth 2.0** en vervolgens **toevoegen**.

Geef een **weergavenaam** en **beschrijving**.

Pagina-URL voor de registratie van de Client ** Voer een aanduidingswaarde van de tijdelijke zoals `http://localhost`.  De **Client de registratie van URL** verwijst naar de pagina die gebruikers gebruiken kunnen voor het maken en hun eigen accounts configureren voor OAuth 2.0-providers die ondersteuning bieden voor gebruikersbeheer van accounts. In dit voorbeeld gebruikers niet maken en hun eigen accounts configureren zodat een tijdelijke aanduiding wordt gebruikt.

Controleer **autorisatiecode** als de **autorisatie verlenen typen**.

Geef vervolgens **autorisatie eindpunt-URL** en **Token eindpunt-URL**.

Deze waarden kunnen worden opgehaald uit de **eindpunten** pagina in uw Azure AD-tenant. Voor toegang tot de eindpunten, gaat u naar de **App registraties** pagina opnieuw en klik op **eindpunten**.

Kopieer de **OAuth 2.0-autorisatie eindpunt** en plak deze in de **autorisatie eindpunt-URL** textbox.

Kopieer de **OAuth 2.0-Tokeneindpunt** en plak deze in de **Token eindpunt-URL** textbox.

Naast plakken in het token eindpunt toevoegen een extra inhoudparameter met de naam **resource** en voor het gebruik van de waarde de **toepassings-ID** voor de back-end-app.

Geef vervolgens de referenties van de client. Dit zijn de referenties voor de client-app.

Voor **Client-Id**, gebruiken de **toepassings-ID** voor de client-app.

Voor **clientgeheim**, voor het gebruik van de sleutel die u hebt gemaakt voor de client-app is het eerder. 

Onmiddellijk na het clientgeheim is de **redirect_url** voor de autorisatie code type verlenen.

Noteer deze URL.

Klik op **maken**.

Ga terug naar de **instellingen** pagina van uw client-app.

Klik op **antwoord-URL's** en plak de **redirect_url** in de eerste rij. In dit voorbeeld vervangen we `https://localhost` met de URL in de eerste rij.  

Nu we een OAuth 2.0-autorisatie-Server hebt geconfigureerd, is de Ontwikkelaarsconsole moet kunnen toegangstokens te verkrijgen van Azure AD. 

De volgende stap is het inschakelen van OAuth 2.0-gebruikersautorisatie voor onze API, zodat de Ontwikkelaarsconsole weet dat deze moet aan het verkrijgen van een toegangstoken namens de gebruiker voordat u de API aanroepen.

Navigeer naar uw exemplaar APIM, gaat u naar **API's**.

Klik op de API die u wilt beveiligen. In dit voorbeeld gebruiken we de `Echo API`.

Ga naar **instellingen**.

Onder **beveiliging**, kies **OAuth 2.0** en selecteer de eerder geconfigureerde OAuth 2.0-server. 

Klik op **Opslaan**.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Is de API aanroepen vanuit de portal voor ontwikkelaars

Nu dat het OAuth 2.0-gebruikersautorisatie is ingeschakeld op de `Echo API`, Developer-Console een toegangstoken namens de gebruiker zal krijgen voordat u de API aanroept.

Navigeer naar een bewerking onder de `Echo API` in de Portal voor ontwikkelaars en klik op **Try it**, die ervoor zorgt dat ons op de Developer-Console.

Houd er rekening mee een nieuw item in de **autorisatie** sectie overeenkomt met de autorisatie-server die u zojuist hebt toegevoegd.

Selecteer **autorisatiecode** van de autorisatie vervolgkeuzelijst en u wordt gevraagd aan te melden bij de Azure AD-tenant. Als u bent al aangemeld bij het account, mag u niet meer worden gevraagd.

Na een geslaagde aanmelden, een `Authorization` koptekst wordt toegevoegd aan de aanvraag met een toegangstoken van Azure AD. 

Een voorbeeldtoken lijkt erop dat hieronder Base64 is-gecodeerd.

```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
```

Klik op **verzenden** en u moet mogelijk de API-aanroep.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Configureer een beleid voor de validatie van JWT vooraf om aanvragen te verifiëren

Wanneer een gebruiker probeert te maken van een aanroep van de Developer-Console, op dit punt wordt de gebruiker wordt gevraagd aan te melden en de Ontwikkelaarsconsole namens de gebruiker een Token toegang zal krijgen. Alles werkt zoals verwacht. Echter, wat gebeurt er als iemand onze API zonder een token of met een ongeldig token aanroepen? Bijvoorbeeld, u kunt proberen te verwijderen van de `Authorization` header en vindt u kunnen nog steeds de API aan te roepen. De reden is dat APIM wordt niet gevalideerd Access Token op dit moment. Dit gewoon wordt doorgegeven de `Auhtorization` header met de back-end API.

Kunnen we gebruiken de [JWT valideren](api-management-access-restriction-policies.md#ValidateJWT) beleid voor het autoriseren van aanvragen in APIM vooraf door het valideren van de toegangstokens voor elke inkomende aanvraag. Als een aanvraag voor een geldig token niet beschikt, wordt geblokkeerd door de API Management en niet wordt doorgestuurd naar de back-end. Bijvoorbeeld: we kunt toevoegen de hieronder beleid de `<inbound>` beleidssectie van de `Echo API`. Het controleert de claim doelgroep in een toegangstoken en wordt een foutbericht geretourneerd als het token niet geldig is. Zie voor meer informatie over het configureren van beleid [instellen of bewerken van beleid](set-edit-policies.md).

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

In deze handleiding gebruikt we de Developer-Console in APIM als de voorbeeldtoepassing voor de client aan te roepen de `Echo API` beveiligd met OAuth 2.0. Raadpleeg voor meer informatie over het ontwikkelen van een toepassing en implementeren van de OAuth 2.0-stroom [Azure Active Directory-codevoorbeelden](../active-directory/develop/active-directory-code-samples.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Azure Active Directory en OAuth2.0](../active-directory/develop/active-directory-authentication-scenarios.md)
* Bekijk meer [video's](https://azure.microsoft.com/documentation/videos/index/?services=api-management) over API Management.
* Zie voor andere manieren voor het beveiligen van uw back-endservice [wederzijdse certificaatverificatie](api-management-howto-mutual-certificates.md).

[Create an API Management service instance]: get-started-create-service-instance.md
[Manage your first API]: import-and-publish.md
