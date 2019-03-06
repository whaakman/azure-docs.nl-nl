---
title: Azure AD v2.0 gebruiken voor aanmelding bij gebruikers op apparaten zonder browser | Microsoft Docs
description: Ingesloten en browser zonder verificatie stromen met behulp van het apparaat verlenen.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 780eec4d-7ee1-48b7-b29f-cd0b8cb41ed3
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1f34796a98acb3ed4241edff57e506990479d1a
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57443263"
---
# <a name="azure-active-directory-v20-and-the-oauth-20-device-code-flow"></a>Azure Active Directory v2.0 en de stroom voor OAuth 2.0-apparaat code

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Azure AD ondersteunt de [apparaat verlenen](https://tools.ietf.org/html/draft-ietf-oauth-device-flow-12), waarmee gebruikers zich aanmelden bij de invoer beperkte apparaten zoals een smart-tv's, IoT-apparaat of een printer.  Om in te schakelen met deze stroom, heeft het apparaat de gebruiker gaat u naar een webpagina in een browser op een ander apparaat te melden.  Als de gebruiker zich aangemeld heeft, wordt het apparaat kunnen toegangstokens ophalen en vernieuwingstokens zo nodig is.  

> [!Important] 
> Op dit moment ondersteunt het v2.0-eindpunt alleen de apparaat-stroom voor Azure AD-tenants, maar geen persoonlijke accounts.  Dit betekent dat u een eindpunt dat is ingesteld als een tenant, of het eindpunt organisaties moet gebruiken.  
>
> Persoonlijke accounts die worden uitgenodigd voor een Azure AD-tenant zich de toekenning van de stroom apparaat gebruiken, maar alleen in de context van de tenant.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).
>

## <a name="protocol-diagram"></a>Protocol-diagram

De stroom van het hele apparaat lijkt op het volgende diagram. We beschrijven elk van de stappen verderop in dit artikel.

![De stroom van apparaat](media/v2-oauth2-device-flow/v2-oauth-device-flow.png)

## <a name="device-authorization-request"></a>Autorisatie-aanvraag voor apparaat

De client moet eerst contact op met de authentication-server voor een apparaat- en -code, gebruikt voor het starten van verificatie.  De client worden verzameld van deze aanvraag uit de `/devicecode` eindpunt. In deze aanvraag moet de client ook de machtigingen die nodig zijn om te verkrijgen van de gebruiker bevatten.  Vanaf het moment dat deze aanvraag wordt verzonden, heeft de gebruiker slechts 15 minuten aan te melden bij (de gebruikelijke waarde voor `expires_in`), zodat alleen deze aanvraag maken wanneer de gebruiker heeft aangegeven dat ze klaar om aan te melden bij.

```
// Line breaks are for legibility only.

POST https://login.microsoftonline.com/{tenant}/devicecode
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
scope=user.read%20openid%20profile

```

| Parameter | Voorwaarde | Description |
| --- | --- | --- |
| tenant |Vereist |De directory-tenant die u wilt toestemming van aanvragen. Dit kan zijn in de beschrijvende naamindeling of GUID.  |
| client_id |Vereist |De aanvraag-ID die de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| scope | Aanbevolen | Een door spaties gescheiden lijst van [scopes](v2-permissions-and-consent.md) dat u wilt dat de gebruiker toe te staan.  |

### <a name="device-authorization-response"></a>De reactie van de apparaat-autorisatie

Een geslaagde reactie is een JSON-object met de vereiste gegevens zodat de gebruiker zich aanmeldt.  

| Parameter | Indeling | Description |
| ---              | --- | --- |
|`device_code`     |String| Een lange tekenreeks die wordt gebruikt om te controleren of de sessie tussen de client en de autorisatie-server.  Dit wordt gebruikt door de client om aan te vragen van het toegangstoken van de autorisatie-server. |
|`user_code`       |String| Een korte tekenreeks aan de gebruiker, gebruikt voor het identificeren van de sessie op een tweede apparaat weergegeven.|
|`verification_uri`|URI| De URI die de gebruiker moet te gaan met de `user_code` om te kunnen aanmelden. |
|`verification_uri_complete`|URI| Een URI combineren de `user_code` en de `verification_uri`, die wordt gebruikt voor niet-tekstuele verzending naar de gebruiker (bijvoorbeeld via Bluetooth op een apparaat of via een QR-code).  |
|`expires_in`      |int| Het aantal seconden voordat de `device_code` en `user_code` verlopen. |
|`interval`        |int| Het aantal seconden dat de client tussen navragen aanvragen wachten moet. |
| `message`        |String| Een leesbare tekenreeks met instructies voor de gebruiker.  Dit kan worden gelokaliseerd door een **queryparameter** in de aanvraag van het formulier `?mkt=xx-XX`, vullen in de juiste taal voor de cultuur. |

## <a name="authenticating-the-user"></a>Verifiëren van de gebruiker

Na de `user_code` en `verification_uri`, toont de client aan de gebruiker, dat ze zich aanmelden met hun mobiele telefoon of PC-browser.  Daarnaast de client een QR-code of een vergelijkbaar mechanisme kunt gebruiken om weer te geven de `verfication_uri_complete`, die duurt de stap voor het invoeren van de `user_code` voor de gebruiker.

Terwijl de gebruiker is geverifieerd op de `verification_uri`, de client moet polling de `/token` eindpunt voor de aangevraagde token met de `device_code`.

``` 
POST https://login.microsoftonline.com/tenant/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

grant_type: urn:ietf:params:oauth:grant-type:device_code
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8
```

|Parameter | Vereist | Description|
| -------- | -------- | ---------- |
|`grant_type` | Vereist| Moet zijn `urn:ietf:params:oauth:grant-type:device_code`|
|`client_id`  | Vereist| Moet overeenkomen met de `client_id` wordt gebruikt in de eerste aanvraag. |
|`device_code`| Vereist| De `device_code` geretourneerd in de autorisatieaanvraag van het apparaat.  |

### <a name="expected-errors"></a>Verwachte fouten

Omdat de stroom van het apparaat een polling-protocol is, moet uw client verwacht voor het ontvangen van fouten voordat de verificatie van de gebruiker is voltooid.  

| Fout | Description | Clientactie |
|------ | ----------- | -------------|
| `authorization_pending` |  De gebruiker is nog niet voltooid verificatie, maar niet de stroom is geannuleerd. | De aanvraag opnieuw nadat ten minste `interval` seconden. |
| `authorization_declined`|  De eindgebruiker de autorisatieaanvraag geweigerd.| Stop polling en terugkeren naar een niet-geverifieerde status.  |
| `bad_verification_code`|De `device_code` verzonden naar de `/token` eindpunt is niet herkend. | Controleer of dat de client de juiste verzendt `device_code` in de aanvraag. |
| `expired_token`|  Ten minste `expires_in` seconden zijn verstreken en verificatie is niet meer mogelijk is met dit `device_code`. | Stop polling en terugkeren naar een niet-geverifieerde status. |


### <a name="successful-authentication-response"></a>Verificatie is geslaagd antwoord

Een geslaagde respons token ziet er als:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Parameter | Indeling | Description |
| --------- | ------ | ----------- |
|`token_type` | String| Altijd 'Bearer. |
|`scope` | Tekenreeksen gescheiden door spaties | Als een toegangstoken is geretourneerd, zijn dit de scopes die het toegangstoken is ongeldig voor. |
|`expires_in`| int | Aantal seconden voordat de opgenomen toegangstoken is geldig voor. |
|`access_token`| Ondoorzichtige tekenreeks | Uitgegeven voor de [scopes](v2-permissions-and-consent.md) die zijn aangevraagd.  |
|`id_token`   | JWT | Uitgegeven als de oorspronkelijke `scope` parameter opgenomen de `openid` bereik.  |
|`refresh_token` | Ondoorzichtige tekenreeks | Uitgegeven als de oorspronkelijke `scope` parameter opgenomen `offline_access`.  |

Het vernieuwingstoken dat kan worden gebruikt om nieuwe toegangstokens verkrijgen en vernieuwingstokens met behulp van dezelfde stroom uiteengezet in de [OAuth Code flow-documentatie](v2-oauth2-auth-code-flow.md#refresh-the-access-token).  
