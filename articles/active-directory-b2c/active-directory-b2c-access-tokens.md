---
title: Aanvragen van tokens voor toegang in Azure Active Directory B2C | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een clienttoepassing instellen en een toegangstoken verkrijgen.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/09/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f3db56c7ce61960fca0e5347b2385bcc65a88354
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845143"
---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: Toegangstokens aanvragen

Een toegangstoken (aangeduid als **toegang\_token** in de antwoorden van Azure AD B2C) is een vorm van een beveiligingstoken dat een client gebruiken kan voor toegang tot resources die worden beveiligd door een [autorisatieserver](active-directory-b2c-reference-protocols.md), zoals een web-API. Toegangstokens worden weergegeven als [JWTs](active-directory-b2c-reference-tokens.md) en bevatten informatie over de gewenste resource-server en de toegekende machtigingen op de server. Bij het aanroepen van de resource-server, is het toegangstoken moet worden gebruikt in de HTTP-aanvraag.

In dit artikel wordt beschreven hoe u voor het configureren van een clienttoepassing en web-API voor het verkrijgen van een **toegang\_token**.

> [!NOTE]
> **Web-API-ketens (On-Behalf-Of) wordt niet ondersteund door Azure AD B2C.**
>
> Veel architecturen bevatten een web-API die u moet een andere downstream web-API, zowel beveiligd door Azure AD B2C aanroepen. In dit scenario is gemeenschappelijk in systeemeigen clients met een web-API-backend, dat op zijn beurt een Microsoft online services, zoals de Azure AD Graph API-aanroepen.
>
> In dit scenario gekoppelde web-API kan worden ondersteund met behulp van de toekenning van OAuth 2.0 JWT Bearer referentie, ook bekend als de On-Behalf-Of-stroom. De On-Behalf-Of-stroom is echter niet op dit moment geïmplementeerd in Azure AD B2C.

## <a name="register-a-web-api-and-publish-permissions"></a>Een web-API registreren en publiceren van machtigingen

Voordat u een toegangstoken aanvraagt, moet u eerst een web-API registreren en machtigingen (bereiken) die kunnen worden verleend publiceren naar de clienttoepassing.

### <a name="register-a-web-api"></a>Een web-API registreren

1. Klik in het menu Azure AD B2C-functies in Azure portal op **toepassingen**.
2. Klik op **+ toevoegen** aan de bovenkant van het menu.
3. Geef een **Naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. U kunt bijvoorbeeld 'Contoso-API' opgeven.
4. Zet de schakeloptie **Inclusief web-app/web-API** op **Ja**.
5. Voer een willekeurige waarde in voor de **antwoord-URL's**. Geef bijvoorbeeld `https://localhost:44316/` op. De waarde niet van belang omdat een API moet niet worden ontvangen van het token rechtstreeks vanuit Azure AD B2C.
6. Voer een waarde in voor **URI voor de app-id**. Dit is de id die wordt gebruikt voor uw web-API. Voer bijvoorbeeld 'notities' in het vak. De **App ID URI** worden dan `https://{tenantName}.onmicrosoft.com/notes`.
7. Klik op **Maken** om uw toepassing te registreren.
8. Klik op de toepassing die u net hebt gemaakt en noteer de globaal unieke **toepassingsclient-id** die u later in uw code gebruikt.

### <a name="publishing-permissions"></a>Machtigingen voor publiceren

Bereiken die vergelijkbaar met de machtigingen zijn, zijn nodig als uw app is een API wordt aangeroepen. Enkele voorbeelden van bereiken zijn "lezen" of 'schrijven'. Stel dat u wilt dat uw web- of 'gelezen' van een API met systeemeigen app. Uw app in Azure AD B2C aanroepen en aanvragen van een toegangstoken dat toegang tot het bereik "lezen biedt". De app moet worden gemachtigd voor de specifieke API 'lezen' in order voor Azure AD B2C om te verzenden van een toegangstoken. U doet dit door moet uw API eerst voor het publiceren van het bereik 'lezen'.

1. In de Azure AD B2C **toepassingen** menu, open de web-API-toepassing ('Contoso-API').
2. Klik op **Gepubliceerd bereiken**. Dit is waar u de machtigingen (bereiken) definieert die kunnen worden verleend aan andere toepassingen.
3. Voeg **bereikwaarden** zo nodig (bijvoorbeeld ' lezen'). Standaard wordt het bereik 'user_impersonation' gedefinieerd. Als u wilt, kunt u dit negeren. Voer een beschrijving van het bereik in de **scopenaam** kolom.
4. Klik op **Opslaan**.

> [!IMPORTANT]
> De **scopenaam** is de beschrijving van de **bereikwaarde**. Wanneer u het bereik, zorg ervoor dat u de **bereikwaarde**.

## <a name="grant-a-native-or-web-app-permissions-to-a-web-api"></a>Verlenen van een systeemeigen of web-app-machtigingen aan een web-API

Nadat een API is geconfigureerd voor het publiceren van bereiken, moet de clienttoepassing deze bereiken via Azure portal worden verleend.

1. Navigeer naar de **toepassingen** in het menu van Azure AD B2C-functies.
2. Registreren van een clienttoepassing ([web-app](active-directory-b2c-app-registration.md) of [native client](active-directory-b2c-app-registration.md)) als u er nog geen hebt. Als u als eerste stap in deze handleiding volgt, moet u het registreren van een clienttoepassing.
3. Klik op **API-toegang**.
4. Klik op **Toevoegen**.
5. Selecteer uw web-API en scopes (machtigingen) u wilt verlenen.
6. Klik op **OK**.

> [!NOTE]
> Azure AD B2C wordt niet gevraagd uw client gebruikers van de toepassing om hun toestemming. In plaats daarvan wordt alle toestemming geleverd door de beheerder, op basis van de machtigingen die zijn geconfigureerd tussen de toepassingen die hierboven worden beschreven. Als een machtiging verlenen voor een toepassing is ingetrokken, kunnen alle gebruikers die eerder konden aan te schaffen die machtiging zich niet meer doen.

## <a name="requesting-a-token"></a>Aanvragen van een token

Bij het aanvragen van een toegangstoken de clienttoepassing nodig heeft om op te geven van de gewenste machtigingen in de **bereik** parameter van de aanvraag. Bijvoorbeeld, om op te geven de **bereikwaarde** "lezen" voor de API waaraan de **App ID URI** van `https://contoso.onmicrosoft.com/notes`, het bereik worden `https://contoso.onmicrosoft.com/notes/read`. Hieronder volgt een voorbeeld van een autorisatieaanvraag code naar het `/authorize` eindpunt.

> [!NOTE]
> Aangepaste domeinen worden op dit moment niet ondersteund, samen met toegangstokens. U moet uw domein tenantName.onmicrosoft.com in de aanvraag-URL.

```
https://<tenantName>.b2clogin.com/tfp/<tenantName>.onmicrosoft.com/<yourPolicyId>/oauth2/v2.0/authorize?client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
```

Als u wilt meerdere machtigingen in dezelfde aanvraag aanschaft, kunt u meerdere vermeldingen toevoegen in de ene **bereik** parameter, gescheiden door spaties. Bijvoorbeeld:

URL gedecodeerd:

```
scope=https://contoso.onmicrosoft.com/notes/read openid offline_access
```

URL-codering:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread%20openid%20offline_access
```

U kunt meer bereiken (machtigingen) aanvragen voor een resource dan wat voor uw clienttoepassing wordt verleend. Als dit het geval is, slaagt de aanroep als ten minste één machtiging is verleend. De resulterende **toegang\_token** heeft de claim "scp" is gevuld met alleen de machtigingen die verleend is zijn.

> [!NOTE] 
> We ondersteunen geen waarin machtiging wordt gevraagd op twee verschillende webresources in dezelfde aanvraag. Dit soort aanvraag mislukt.

### <a name="special-cases"></a>Speciale gevallen

De OpenID Connect-standaard bevat verschillende waarden voor het speciale 'bereik'. De volgende speciale bereiken, gelden voor de machtiging voor 'toegang tot het gebruikersprofiel':

* **openid**: Dit aanvragen een ID-token
* **offline\_access**: Hiermee vraagt een vernieuwingstoken (met behulp van [autorisatiecode stromen](active-directory-b2c-reference-oauth-code.md)).

Als de `response_type` parameter in een `/authorize` verzoek bevat `token`, wordt de `scope` parameter moet ten minste één resource bereik bevatten (andere dan `openid` en `offline_access`) die wordt verleend. Anders wordt de `/authorize` -aanvraag wordt beëindigd met een fout.

## <a name="the-returned-token"></a>Het geretourneerde token

In een is geslagen **toegang\_token** (vanuit de `/authorize` of `/token` eindpunt), de volgende claims aanwezig zijn:

| Name | Claim | Description |
| --- | --- | --- |
|Doelgroep |`aud` |De **toepassings-ID** van de afzonderlijke resource die de token verleent toegang tot. |
|Bereik |`scp` |De machtigingen verleend aan de resource. Meerdere toegekende machtigingen worden gescheiden door een spatie. |
|Geautoriseerde partij |`azp` |De **toepassings-ID** van de clienttoepassing die de aanvraag wordt gestart. |

Als uw API krijgt de **toegang\_token**, moet het [valideren van het token](active-directory-b2c-reference-tokens.md) om te bewijzen dat het token authentiek is en de juiste claims heeft.

We zijn altijd geopend zodat u kunt feedback en suggesties. Als u problemen met dit onderwerp hebt, kunt u plaatsen op Stack Overflow met het label ['azure-ad-b2c'](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Voor functieaanvragen, voeg deze toe aan [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

## <a name="next-steps"></a>Volgende stappen

* Bouw een web-API via [.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)
* Bouw een web-API via [Node.JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)
