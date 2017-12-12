---
title: Aanvragen van de toegangstokens - Azure AD B2C | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een clienttoepassing instellen en het verkrijgen van een toegangstoken.
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: mtillman
editor: 
ms.assetid: 1c75f17f-5ec5-493a-b906-f543b3b1ea66
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: parakhj
ms.openlocfilehash: 9d2df39118741d4254f7b7fe4c419a00ceb4ba8e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: Toegangstokens voor aanvragen

Een toegangstoken (aangeduid als **toegang\_token** in de antwoorden van Azure AD B2C) is een vorm van een beveiligingstoken dat een client gebruiken kan voor toegang tot resources die zijn beveiligd met een [autorisatie server](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-protocols#the-basics), zoals een web-API. Toegangstokens worden weergegeven als [JWTs](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#types-of-tokens) en bevatten informatie over de beoogde resource-server en de machtigingen hebben op de server. Bij het aanroepen van de resource-server, moet het toegangstoken aanwezig zijn in de HTTP-aanvraag.

In dit artikel wordt beschreven hoe u een clienttoepassing configureert en web-API ter verkrijging van een **toegang\_token**.

> [!NOTE]
> **Web-API-ketens (op-andere gebruikers-Of) wordt niet ondersteund door Azure AD B2C.**
>
> Veel architecturen bevatten een web-API die u moet een andere downstream web-API, beide beveiligd door Azure AD B2C aanroepen. Dit scenario is gemeenschappelijk in systeemeigen clients waarvoor een web-API back-end die op zijn beurt een Microsoft online services zoals de Azure AD Graph-API aanroept.
>
> Dit scenario keten web-API kan worden ondersteund door met het OAuth 2.0 JWT Bearer Credential grant, ook bekend als de On-namens-stroom. Echter, de On-namens-stroom is momenteel niet geïmplementeerd in Azure AD B2C.

## <a name="register-a-web-api-and-publish-permissions"></a>Registreren van een web-API en machtigingen publiceren

Voordat u een toegangstoken aanvraagt, moet u eerst registreren van een web-API en publiceren van machtigingen (scopes genoemd) die kunnen worden verleend aan de clienttoepassing.

### <a name="register-a-web-api"></a>Een web-API registreren

1. Klik in het menu Azure AD B2C-functies in de Azure portal op **toepassingen**.
1. Klik op **+ toevoegen** aan de bovenkant van het menu.
1. Geef een **Naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. U kunt bijvoorbeeld 'Contoso-API invoeren.
1. Zet de schakeloptie **Inclusief web-app/web-API** op **Ja**.
1. Voer een willekeurige waarde voor de **antwoord-URL's**. Geef bijvoorbeeld `https://localhost:44316/` op. De waarde niet van belang omdat een API moet niet worden ontvangen van het token rechtstreeks vanuit Azure AD B2C.
1. Voer een waarde in voor **URI voor de app-id**. Dit is de id die wordt gebruikt voor uw web-API. Bijvoorbeeld 'notities' in het vak invoeren. De **App ID URI** worden dan `https://{tenantName}.onmicrosoft.com/notes`.
1. Klik op **Maken** om uw toepassing te registreren.
1. Klik op de toepassing die u net hebt gemaakt en noteer de globaal unieke **toepassingsclient-id** die u later in uw code gebruikt.

### <a name="publishing-permissions"></a>Publishing machtigingen

Scopes zijn vergelijkbaar met machtigingen en zijn nodig als uw app een API aanroept. Enkele voorbeelden van bereiken zijn 'lezen' of 'schrijven'. Stel dat u wilt dat uw web- of systeemeigen app naar een API 'lezen'. Uw app wilt aanroepen van Azure AD B2C en aanvragen van een toegangstoken dat toegang tot het bereik 'lezen biedt'. De app moet worden gemachtigd voor 'lezen' van de specifieke API in volgorde voor Azure AD B2C uitzenden van een toegangstoken. Om dit te doen, moet uw API eerst voor het publiceren van het bereik 'lezen'.

1. In de Azure AD B2C **toepassingen** toepassing in het menu, open de web-API ('Contoso-API).
1. Klik op **Gepubliceerd bereiken**. Dit is waar u de machtigingen (bereiken) definieert die kunnen worden verleend aan andere toepassingen.
1. Voeg **waarden voor het bereik** zo nodig (bijvoorbeeld ' lezen'). Standaard wordt het bereik 'user_impersonation' gedefinieerd. U kunt deze negeren als u wenst. Voer een beschrijving van het bereik in de **scopenaam** kolom.
1. Klik op **Opslaan**.

> [!IMPORTANT]
> De **scopenaam** is de beschrijving van de **bereikwaarde**. Wanneer u het bereik, moet u de **bereikwaarde**.

## <a name="grant-a-native-or-web-app-permissions-to-a-web-api"></a>Een systeemeigen verlenen of web-app-machtigingen voor een web-API

Zodra een API is geconfigureerd voor het publiceren van bereiken, moet de clienttoepassing deze bereiken via de Azure-portal worden verleend.

1. Navigeer naar de **toepassingen** in het menu van Azure AD B2C-functies.
1. Registreren van een clienttoepassing ([web-app](active-directory-b2c-app-registration.md#register-a-web-app) of [native client](active-directory-b2c-app-registration.md#register-a-mobile-or-native-app)) als u nog niet hebt. Als u deze handleiding als startpunt volgt, moet u een clienttoepassing registreren.
1. Klik op **API-toegang**.
1. Klik op **Toevoegen**.
1. Selecteer uw web-API en de bereiken (machtigingen) die u wilt verlenen.
1. Klik op **OK**.

> [!NOTE]
> Azure AD B2C wordt niet gevraagd uw client toepassingsgebruikers om hun toestemming. In plaats daarvan wordt alle toestemming verstrekt door de beheerder, op basis van de machtigingen die zijn geconfigureerd tussen de toepassingen die hierboven worden beschreven. Als een machtiging verlenen voor een toepassing is ingetrokken, kunnen alle gebruikers die eerder konden te verkrijgen die machtiging worden niet meer doen.

## <a name="requesting-a-token"></a>Aanvragen van een token

Bij het aanvragen van een toegangstoken de clienttoepassing moet specificeren van de gewenste machtigingen in de **bereik** parameter van de aanvraag. Bijvoorbeeld, om op te geven de **bereikwaarde** 'lezen' voor de API met de **App ID URI** van `https://contoso.onmicrosoft.com/notes`, het bereik zijn `https://contoso.onmicrosoft.com/notes/read`. Hieronder volgt een voorbeeld van een aanvraag van de autorisatie code naar de `/authorize` eindpunt.

> [!NOTE]
> Aangepaste domeinen worden momenteel niet ondersteund samen met toegangstokens. U moet uw domein tenantName.onmicrosoft.com in de aanvraag-URL.

```
https://login.microsoftonline.com/<tenantName>.onmicrosoft.com/oauth2/v2.0/authorize?p=<yourPolicyId>&client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
```

Als u wilt meerdere machtigingen in dezelfde aanvraag aanschaft, kunt u meerdere vermeldingen in het enkelvoudige toevoegen **bereik** parameter, gescheiden door spaties. Bijvoorbeeld:

URL gedecodeerd:

```
scope=https://contoso.onmicrosoft.com/notes/read openid offline_access
```

URL-gecodeerd:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread%20openid%20offline_access
```

U kunt meer scopes (machtigingen) aanvragen voor een resource dan wat voor de clienttoepassing wordt verleend. Als dit het geval is, wordt de aanroep slaagt als ten minste één machtiging is verleend. De resulterende **toegang\_token** heeft de claim "scp" is gevuld met alleen de machtigingen die verleend zijn.

> [!NOTE] 
> Wij ondersteunen geen aanvragende machtigingen op basis van twee verschillende webbronnen in dezelfde aanvraag. Dit soort aanvraag zal mislukken.

### <a name="special-cases"></a>Bijzondere gevallen

De standaard OpenID Connect bevat verschillende waarden voor speciale 'bereik'. De volgende speciale scopes vertegenwoordigen de machtiging voor 'toegang tot het gebruikersprofiel':

* **openid**: dit vraagt een token ID
* **offline\_toegang**: dit vraagt een vernieuwingstoken (met behulp van [autorisatiecode loopt](active-directory-b2c-reference-oauth-code.md)).

Als de `response_type` parameter in een `/authorize` aanvraag bevat `token`, wordt de `scope` parameter moet ten minste één bron bereik bevatten (anders dan `openid` en `offline_access`) die wordt verleend. Anders wordt de `/authorize` -aanvraag wordt beëindigd met een fout.

## <a name="the-returned-token"></a>Het resulterende token

In een succes geslagen **toegang\_token** (vanuit de `/authorize` of `/token` eindpunt), de volgende claims aanwezig zijn:

| Naam | Claim | Beschrijving |
| --- | --- | --- |
|Doelgroep |`aud` |De **toepassings-ID** van de afzonderlijke resource die de token verleent toegang tot. |
|Bereik |`scp` |De machtigingen te krijgen tot de resource. Meerdere verleende machtigingen worden gescheiden door een spatie. |
|Geautoriseerde partij |`azp` |De **toepassings-ID** van de clienttoepassing die de aanvraag wordt geïnitieerd. |

Wanneer uw API ontvangt de **toegang\_token**, moet deze [valideren van het token](active-directory-b2c-reference-tokens.md) om te bewijzen dat het token authentiek is en de juiste claims heeft.

We kunnen worden altijd feedback en suggesties! Als u problemen met dit onderwerp ondervindt, boek bij Stack Overflow met behulp van de tag ['azure ad b2c'](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Voor functieaanvragen, voeg deze toe aan [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

## <a name="next-steps"></a>Volgende stappen

* Maakt een API met [.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)
* Maakt een API met [Node.JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)
