---
title: Migreren van gebruikers met sociale identiteiten in Azure Active Directory B2C | Microsoft Docs
description: Belangrijkste concepten besproken voor de migratie van gebruikers met sociale identiteiten in Azure AD B2C met Graph API.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/03/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 0ca73b8bfaca481d3e0404d068a74e1a6b0e4dcb
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54846554"
---
# <a name="azure-active-directory-b2c-migrate-users-with-social-identities"></a>Azure Active Directory B2C: Gebruikers met sociale identiteiten migreren
Wanneer u van plan bent om te migreren van uw id-provider naar Azure AD B2C, moet u mogelijk ook gebruikers met sociale identiteiten migreren. Dit artikel wordt uitgelegd hoe u voor het migreren van bestaande accounts voor sociale identiteiten, zoals: Facebook, LinkedIn, Microsoft en Google-accounts aan Azure AD B2C. In dit artikel geldt ook voor federatieve identiteiten, maar deze migraties minder gangbaar zijn.

## <a name="prerequisites"></a>Vereisten
Dit artikel is een vervolg van het artikel van de migratie van gebruiker en is gericht op sociale id-migratie. Lees voordat u begint, [gebruikersmigratie](active-directory-b2c-user-migration.md).

## <a name="social-identities-migration-introduction"></a>Inleiding voor sociale identiteiten migreren

* In Azure AD B2C, **lokale accounts** aanmelden namen (de gebruiker voor de naam of e-mailadres) worden opgeslagen in de `signInNames` verzameling in de record van de gebruiker. De `signInNames` bevat een of meer `signInName` records die de namen aanmelden voor de gebruiker opgeven. De naam van elke aanmelding moet uniek zijn in de tenant.

* **Sociale accounts** identiteiten worden opgeslagen in `userIdentities` verzameling. Hiermee geeft u op de vermelding de `issuer` (naam id-provider), zoals facebook.com en de `issuerUserId`, dit is een unieke gebruikers-id voor de verlener. De `userIdentities` kenmerk bevat een of meer UserIdentity records die het type sociaalnetwerkaccount en de unieke gebruikers-id van de sociale id-provider opgeven.

* **Lokaal account worden gecombineerd met sociale id**. Zoals gezegd, worden lokaal account aanmelden namen en sociale account-id's worden opgeslagen in verschillende kenmerken. `signInNames` wordt gebruikt voor het lokale account, terwijl `userIdentities` voor sociaal account. Één Azure AD B2C-account, kan een lokaal account alleen zijn en sociaal account alleen, of een lokaal account worden gecombineerd met sociale id in een gebruikersrecord. Dit gedrag kunt u voor het beheren van één account, terwijl een gebruiker kan zich aanmelden met de credential(s) lokaal account of met de sociale identiteiten.

* `UserIdentity` Type - informatie bevat over de identiteit van een gebruiker sociaal account in een Azure AD B2C-tenant:
    * `issuer` De tekenreeksweergave van de id-provider die de gebruikers-id, zoals facebook.com heeft uitgegeven.
    * `issuerUserId` De unieke gebruikers-id die wordt gebruikt door de sociale id-provider in Base 64-indeling.

    ```JSON
    "userIdentities": [{
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
    ```

* Afhankelijk van de id-provider, de **sociale gebruikers-ID** is een unieke waarde voor een bepaalde gebruiker per toepassing of ontwikkeling-account. Het Azure AD B2C-beleid configureren met dezelfde toepassings-ID die eerder door de sociale provider is toegewezen. Of een andere toepassing binnen hetzelfde account voor de ontwikkeling.

## <a name="use-graph-api-to-migrate-users"></a>Graph API gebruiken om gebruikers te migreren
Maken van de Azure AD B2C-gebruikersaccount via [Graph API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet). Om te communiceren met de Graph API, moet u eerst een service-account met beheerdersbevoegdheden hebt. In Azure AD registreert u een toepassing en verificatie met Azure AD. De referenties van de toepassing zijn toepassings-ID en -geheim van de toepassing. De toepassing fungeert als zelf, niet als een gebruiker, de Graph-API aan te roepen. Volg de instructies in stap 1 in [gebruikersmigratie](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-user-migration#step-1-use-graph-api-to-migrate-users) artikel.

## <a name="required-properties"></a>Vereiste eigenschappen
De volgende lijst bevat de eigenschappen die vereist zijn wanneer u een gebruiker maken.
* **accountEnabled** : true
* **displayName** -de naam om weer te geven in het adresboek voor de gebruiker.
* **passwordProfile** -wachtwoordprofiel voor de gebruiker. 

> [!NOTE]
> Voor sociale account (zonder referenties voor lokale accounts), moet u nog steeds het wachtwoord opgeven. Azure AD B2C wordt genegeerd door het wachtwoord die u voor sociale accounts opgeeft.

* **userPrincipalName** -de user principal name (someuser@contoso.com). De principal-naam van de gebruiker moet een van de gecontroleerde domeinen bevatten voor de tenant. Als u de UPN, genereren van nieuwe GUID-waarde, samenvoegen met `@` en naam van uw tenant.
* **mailNickname** -e-mailalias voor de gebruiker. Deze waarde kan dezelfde ID die u voor de userPrincipalName gebruikt zijn. 
* **signInNames** -records van een of meer SignInName die de namen aanmelden voor de gebruiker opgeven. De naam van elke aanmelding moet uniek zijn in het bedrijf/tenant. Voor sociaal account alleen kan deze eigenschap leeg zijn.
* **userIdentities** -een of meer UserIdentity records die sociale opgeeft account type en de unieke gebruikers-id van de sociale id-provider.
* [optioneel] **otherMails** - voor sociaal account alleen e-mailadressen van de gebruiker 

Zie voor meer informatie: [Graph API-verwijzing](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser)

## <a name="migrate-social-account-only"></a>Sociale account (alleen) migreren
Sociale account alleen maken zonder referenties voor lokale accounts. HTTPS-POST-aanvraag naar Graph API verzenden. Hoofdtekst van de aanvraag bevat de eigenschappen van de gebruiker sociaal account te maken. U moet ten minste de vereiste eigenschappen opgeven. 


**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

De volgende formuliergegevens verzenden: 

```JSON
{
    "objectId": null,
    "accountEnabled": true,
    "mailNickname": "c8c3d3b8-60cf-4c76-9aa7-eb3235b190c8",
    "signInNames": [],
    "creationType": null,
    "displayName": "Sara Bell",
    "givenName": "Sara",
    "surname": "Bell",
    "passwordProfile": {
        "password": "Test1234",
        "forceChangePasswordNextLogin": false
    },
    "passwordPolicies": null,
    "userIdentities": [{
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ],
    "otherMails": ["sara@live.com"],
    "userPrincipalName": "c8c3d3b8-60cf-4c76-9aa7-eb3235b190c8@tenant-name.onmicrosoft.com"
}
```
## <a name="migrate-social-account-with-local-account"></a>Sociale account met lokaal account migreren
Een gecombineerde lokaal account maken met sociale identiteiten. HTTPS-POST-aanvraag naar Graph API verzenden. Hoofdtekst van de aanvraag bevat de eigenschappen van de gebruiker sociaal account te maken. U moet ten minste de vereiste eigenschappen opgeven. 

**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

De volgende formuliergegevens verzenden: 

```JSON
{
    "objectId": null,
    "accountEnabled": true,
    "mailNickname": "5164db16-3eee-4629-bfda-dcc3326790e9",
    "signInNames": [{
            "type": "emailAddress",
            "value": "david@contoso.com"
        }
    ],
    "creationType": "LocalAccount",
    "displayName": "David Hor",
    "givenName": "David",
    "surname": "Hor",
    "passwordProfile": {
        "password": "1234567",
        "forceChangePasswordNextLogin": false
    },
    "passwordPolicies": "DisablePasswordExpiration,DisableStrongPassword",
    "userIdentities": [{
            "issuer": "contoso.com",
            "issuerUserId": "ZGF2aWRAY29udG9zby5jb20="
        }
    ],
    "otherMails": [],
    "userPrincipalName": "5164db16-3eee-4629-bfda-dcc3326790e9@tenant-name.onmicrosoft.com"
}
```

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
### <a name="how-can-i-know-the-issuer-name"></a>Hoe weet ik de naam van de uitgever?
De naam van de verlener of de naam van de id-provider is geconfigureerd in uw beleid. Als u niet weet wat de waarde die u opgeeft `issuer`, volgt u deze procedure:
1. Meld u aan met een van de sociale accounts
2. De JWT-token, Kopieer de `sub` waarde. De `sub` bevat doorgaans de object-ID van de gebruiker in Azure AD B2C. Of vanuit de Azure portal, opent u de eigenschappen van de gebruiker en kopieer de object-ID.
3. Open [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net)
4. Meld u aan met uw beheerder. N
5. Voer de volgende GET-aanvraag. De userObjectId vervangen door de gebruikers-ID die u hebt gekopieerd. **TOEVOEGEN** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId
6. Zoek de `userIdentities` -element in de JSON geretourneerd uit Azure AD B2C.
7. [Optioneel] U kunt ook moet worden gedecodeerd de `issuerUserId` waarde.

> [!NOTE]
> Gebruik een B2C-tenant administrator-account dat is lokaal op de B2C-tenant. De syntaxis van de naam van het account is admin@tenant-name.onmicrosoft.com.

### <a name="is-it-possible-to-add-social-identity-to-an-existing-local-account"></a>Is het mogelijk om sociale id toevoegen aan een bestaande lokale account?
Ja. Nadat het lokale account is gemaakt, kunt u de sociale id toevoegen. Patch uitvoeren voor HTTPS-aanvraag worden uitgevoerd. De userObjectId vervangen door de gebruikers-ID die u wilt bijwerken. 

**PATCH** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

De volgende formuliergegevens verzenden: 

```JSON
{
    "userIdentities": [
        {
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
}
```

### <a name="is-it-possible-to-add-multiple-social-identities"></a>Is het mogelijk om toe te voegen meerdere sociale identiteiten?
Ja. U kunt meerdere sociale identiteiten voor één Azure AD B2C-account toevoegen. Patch uitvoeren voor HTTPS-aanvraag worden uitgevoerd. De userObjectId vervangen door de gebruikers-ID. 

**PATCH** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

De volgende formuliergegevens verzenden: 

```JSON
{
    "userIdentities": [
        {
            "issuer": "google.com",
            "issuerUserId": "MjQzMjE2NTc4NTQ="
        },
        {
            "issuer": "facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
}
```

## <a name="optional-user-migration-application-sample"></a>[Optioneel] Voorbeeld van de toepassing gebruiker migratie
[Downloaden en uitvoeren van de voorbeeld-app V2](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). De voorbeeld-app V2 maakt gebruik van een JSON-bestand met gegevens van de dummygebruiker, met inbegrip van: lokale account, sociaalnetwerkaccount en lokale en sociale identiteiten in één account.  Als u wilt het JSON-bestand bewerken, opent u de `AADB2C.UserMigration.sln` Visual Studio-oplossing. In de `AADB2C.UserMigration` project, open de `UsersData.json` bestand. Het bestand bevat een lijst met gebruikersentiteiten. Elke gebruikersentiteit heeft de volgende eigenschappen:
* **signInName** : voor het lokale account, e-mailadres om aan te melden
* **displayName** -weergavenaam van de gebruiker
* **firstName** -voornaam van gebruiker
* **Achternaam** -achternaam van gebruiker
* **wachtwoord** voor lokale account, het wachtwoord van gebruiker (mag leeg zijn)
* **verlener** - voor sociaal account, de naam van de id-provider
* **issuerUserId** - voor sociaal account, de unieke gebruikers-id die wordt gebruikt door de sociale id-provider. De waarde moet liggen in niet-versleutelde tekst. De voorbeeld-app worden deze waarde aan de met base64 gecodeerd.
* **e-mailbericht** voor sociaal account alleen (niet gecombineerde) van de gebruiker e-mailadres

```JSON
{
  "userType": "emailAddress",
  "Users": [
    {
      // Local account only
      "signInName": "James@contoso.com",
      "displayName": "James Martin",
      "firstName": "James",
      "lastName": "Martin",
      "password": "Pass!w0rd"
    },
    {
      // Social account only
      "issuer": "Facebook.com",
      "issuerUserId": "1234567890",
      "email": "sara@contoso.com",
      "displayName": "Sara Bell",
      "firstName": "Sara",
      "lastName": "Bell"
    },
    {
      // Combine local account with social identity
      "signInName": "david@contoso.com",
      "issuer": "Facebook.com",
      "issuerUserId": "0987654321",
      "displayName": "David Hor",
      "firstName": "David",
      "lastName": "Hor",
      "password": "Pass!w0rd"
    }
  ]
}
```

> [!NOTE]
> Als u niet de UsersData.json-bestand in het voorbeeld met uw gegevens bijwerkt, u kunt zich aanmeldt met de referenties van het voorbeeld lokaal account, maar niet met de voorbeelden sociaal account. Als u wilt migreren van uw sociale accounts, echte gegevens te leveren.

Voor meer informatie over het gebruik van de voorbeeld-app, [Azure Active Directory B2C: Gebruikersmigratie](active-directory-b2c-user-migration.md)
