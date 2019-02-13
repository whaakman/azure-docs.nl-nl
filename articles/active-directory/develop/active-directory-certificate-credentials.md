---
title: Referenties van het certificaat in Azure AD | Microsoft Docs
description: Dit artikel wordt beschreven voor de registratie en het gebruik van de referenties van het computercertificaat voor de verificatie van de toepassing
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: celested
ms.reviewer: nacanuma, jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c4ee1ce56723e4a2c9ab80c12456bbc1b66f6d5
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162795"
---
# <a name="certificate-credentials-for-application-authentication"></a>Referenties van het computercertificaat voor de verificatie van de toepassing

Azure Active Directory (Azure AD) kunt u een toepassing zijn eigen referenties gebruiken voor verificatie, bijvoorbeeld in de stroom voor OAuth 2.0-clientreferenties ([v1.0](v1-oauth2-client-creds-grant-flow.md), [v2.0](v2-oauth2-client-creds-grant-flow.md)) en de On-Behalf-Of stroom ([v1.0](v1-oauth2-on-behalf-of-flow.md), [v2.0](v2-oauth2-on-behalf-of-flow.md)).

Een vorm van de referentie op die een toepassing voor verificatie gebruiken kunt is een JSON Web Token(JWT) verklaring ondertekend met een certificaat die eigendom zijn van de toepassing.

## <a name="assertion-format"></a>Bewering indeling
De verklaring Reken-, kunt u een van de vele [JSON Web Token](https://jwt.ms/) bibliotheken in de taal van uw keuze. De informatie die door het token zijn als volgt:

### <a name="header"></a>Header

| Parameter |  Opmerking |
| --- | --- |
| `alg` | Moet **RS256** |
| `typ` | Moet **JWT** |
| `x5t` | Moet de vingerafdruk van het x.509-certificaat SHA-1 |

### <a name="claims-payload"></a>Claims (payload)

| Parameter |  Opmerkingen |
| --- | --- |
| `aud` | Doelgroep: Moet  **https://login.microsoftonline.com/ *tenant_Id*  /oauth2/token** |
| `exp` | De datum van afloop: de datum waarop het token verloopt. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC totdat de geldigheid van het token is verlopen.|
| `iss` | Uitgever: moet de client_id (toepassings-ID van de client-service) |
| `jti` | GUID: de JWT-ID |
| `nbf` | Niet vooraf: de datum waarvoor het token kan niet worden gebruikt. De tijd wordt weergegeven als het aantal seconden vanaf 1 januari 1970 (1970-01-01T0:0:0Z) UTC tot het moment dat het token is uitgegeven. |
| `sub` | Houder: Als voor `iss`, moet de client_id (toepassings-ID van de client-service) |

### <a name="signature"></a>Handtekening

De handtekening wordt berekend met het toepassen van het certificaat zoals is beschreven in de [JSON Web Token RFC7519-specificatie](https://tools.ietf.org/html/rfc7519)

## <a name="example-of-a-decoded-jwt-assertion"></a>Voorbeeld van een gedecodeerde JWT-verklaring

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

## <a name="example-of-an-encoded-jwt-assertion"></a>Voorbeeld van een gecodeerde JWT-bewering

De volgende tekenreeks is een voorbeeld van gecodeerde assertion. Als u zorgvuldig bekijkt, ziet u drie secties die zijn gescheiden door punten (.):
* De eerste sectie codeert de koptekst
* De tweede sectie codeert de nettolading
* Het laatste gedeelte is de handtekening die is berekend met de certificaten van de inhoud van de eerste twee secties

```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

## <a name="register-your-certificate-with-azure-ad"></a>Uw certificaat registreren bij Azure AD

U kunt de certificaatreferentie koppelen aan de clienttoepassing in Azure AD via de Azure-portal met behulp van een van de volgende methoden:

### <a name="uploading-the-certificate-file"></a>Het certificaatbestand uploaden

In de Azure-app-registratie voor de clienttoepassing:
1. Selecteer **instellingen > sleutels** en selecteer vervolgens **openbare sleutel uploaden**. 
2. Selecteer het certificaatbestand dat u wilt uploaden.
3. Selecteer **Opslaan**. 
   
   Nadat u hebt opgeslagen, wordt het certificaat is geüpload en de vingerafdruk, de begindatum en de verloopdatum waarden worden weergegeven. 

### <a name="updating-the-application-manifest"></a>Het toepassingsmanifest bijwerken

Wanneer u de blokkering van een certificaat, moet u om te berekenen:

- `$base64Thumbprint`, dit is de met base64-codering van de certificaat-hash
- `$base64Value`, dit is de met base64-codering van de onbewerkte gegevens van het certificaat

U moet ook opgeven van een GUID voor het identificeren van de sleutel in het toepassingsmanifest (`$keyId`).

In de Azure-app-registratie voor de clienttoepassing:
1. Open het toepassingsmanifest.
2. Vervang de *keyCredentials* eigenschap met de nieuwe gegevens van een certificaat met het volgende schema.

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
3. De wijzigingen opslaan in het toepassingsmanifest en upload vervolgens het manifest naar Azure AD. 

   De `keyCredentials` eigenschap heeft meerdere waarden, zodat u meerdere certificaten voor uitgebreidere Sleutelbeheer kunt uploaden.
   
## <a name="code-sample"></a>Codevoorbeeld

De voorbeeldcode op [zich verifiëren bij Azure AD in daemon-apps met certificaten](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) laat zien hoe een toepassing zijn eigen referenties gebruikt voor verificatie. U ziet ook hoe u kunt [een zelfondertekend certificaat maken](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential#create-a-self-signed-certificate) met behulp van de `New-SelfSignedCertificate` Powershell-opdracht. U kunt ook profiteren en gebruiken de [scripts voor het maken van app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/AppCreationScripts/AppCreationScripts.md) te maken van de certificaten, de vingerafdruk van het compute, enzovoort.
