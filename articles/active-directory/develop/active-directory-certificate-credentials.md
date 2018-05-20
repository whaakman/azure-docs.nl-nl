---
title: Referenties van het certificaat in Azure AD | Microsoft Docs
description: Dit artikel wordt beschreven voor de registratie en het gebruik van referenties van het computercertificaat voor de verificatie van de toepassing
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: c782429ac2d8ee030ca8b589b4241242c7b101d6
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="certificate-credentials-for-application-authentication"></a>Referenties van het computercertificaat voor de verificatie van de toepassing

Azure Active Directory kunt een toepassing zijn eigen referenties voor verificatie. Bijvoorbeeld, in de stroom OAuth 2.0-Client referenties Grant ([v1](active-directory-protocols-oauth-service-to-service.md), [v2](active-directory-v2-protocols-oauth-client-creds.md)) en de On-namens-stroom ([v1](active-directory-protocols-oauth-on-behalf-of.md), [v2](active-directory-v2-protocols-oauth-on-behalf-of.md)).
Een vorm van de referentie die kan worden gebruikt is een JSON Web Token(JWT) assertion ondertekend met een certificaat waartoe de toepassing.

## <a name="format-of-the-assertion"></a>Indeling van de verklaring
Als u wilt de verklaring compute, wilt u waarschijnlijk een van de vele [JSON Web Token](https://jwt.ms/) bibliotheken in de taal van uw keuze. De informatie die door het token is:

#### <a name="header"></a>Koptekst

| Parameter |  Opmerking |
| --- | --- |
| `alg` | Moet **RS256** |
| `typ` | Moet **JWT** |
| `x5t` | Moet de vingerafdruk van het x.509-certificaat SHA-1 |

#### <a name="claims-payload"></a>Claims (Payload)

| Parameter |  Opmerking |
| --- | --- |
| `aud` | : Doelgroep  **https://login.microsoftonline.com/ *tenant_Id*  /oauth2/token** |
| `exp` | Vervaldatum: de datum waarop het token verloopt. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC totdat de geldigheid van het token is verlopen.|
| `iss` | Uitgever: moet de client_id (toepassings-ID van de client-service) |
| `jti` | GUID: de JWT-ID |
| `nbf` | Niet vooraf: de datum voor het token kan niet worden gebruikt. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC totdat de tijd die het token is uitgegeven. |
| `sub` | Onderwerp:-als voor `iss`, moet de client_id (toepassings-ID van de client-service) |

#### <a name="signature"></a>Handtekening

De handtekening wordt berekend met het toepassen van het certificaat, zoals beschreven in de [JSON Web Token RFC7519 specificatie](https://tools.ietf.org/html/rfc7519)

### <a name="example-of-a-decoded-jwt-assertion"></a>Voorbeeld van een gedecodeerde JWT-verklaring

```
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,  
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"

```

### <a name="example-of-an-encoded-jwt-assertion"></a>Voorbeeld van een gecodeerde JWT bewering

De volgende tekenreeks is een voorbeeld van gecodeerde verklaring. Als u zorgvuldig bekijkt, ziet u drie secties gescheiden door punten (.).
De eerste sectie codeert de kop van de tweede de nettolading en de laatste is de handtekening berekend met de certificaten van de inhoud van de eerste twee secties.
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a>Uw certificaat registreren met Azure AD

U kunt de referentie certificaat koppelen aan de clienttoepassing in Azure AD via de Azure portal met behulp van een van de volgende methoden:

**Het certificaatbestand uploaden**

Klik in de Azure-app-registratie voor de clienttoepassing, op **instellingen**, klikt u op **sleutels** en klik vervolgens op **openbare sleutel uploaden**. Selecteer het certificaatbestand dat u wilt uploaden en klik op **opslaan**. Zodra u opslaat, het certificaat is geüpload en de vingerafdruk begindatum en verloopt worden waarden weergegeven. 

**Het toepassingsmanifest bijwerken**

De blokkering van een certificaat hebt, moet u berekenen:

- `$base64Thumbprint`, namelijk het base64-codering van het certificaat-Hash
- `$base64Value`, namelijk het base64-codering van de onbewerkte gegevens van certificaat

U moet ook een GUID voor het identificeren van de sleutel in het toepassingsmanifest bieden (`$keyId`).

In de Azure-app-registratie voor de clienttoepassing, open het toepassingsmanifest en vervang de *keyCredentials* eigenschap met de nieuwe certificaatinformatie van een met het volgende schema:

```
"keyCredentials": [
    {
        "customKeyIdentifier": "$base64Thumbprint",
        "keyId": "$keyid",
        "type": "AsymmetricX509Cert",
        "usage": "Verify",
        "value":  "$base64Value"
    }
]
```

De wijzigingen opslaan in het toepassingsmanifest en uploaden naar Azure AD. De eigenschap keyCredentials heeft meerdere waarden, zodat u meerdere certificaten voor uitgebreidere Sleutelbeheer kan uploaden.
