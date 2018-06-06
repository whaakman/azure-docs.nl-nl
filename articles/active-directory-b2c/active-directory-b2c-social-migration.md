---
title: Migreren van gebruikers met sociale identiteiten in Azure Active Directory B2C | Microsoft Docs
description: Belangrijkste concepten voor de migratie van gebruikers met sociale identiteiten in Azure AD B2C Graph API met bespreken.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/03/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7c83afba1f027771b3407aecf94fefffdc951664
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34710556"
---
# <a name="azure-active-directory-b2c-migrate-users-with-social-identities"></a>Azure Active Directory B2C: Gebruikers met sociale identiteiten migreren
Wanneer u van plan bent om uw identiteitsprovider migreren naar Azure AD B2C, kunt u wellicht ook gebruikers met sociale identiteiten migreren. In dit artikel wordt uitgelegd hoe u voor het migreren van bestaande sociale identiteiten accounts, zoals: Facebook, LinkedIn Microsoft en Google-accounts naar Azure AD B2C. In dit artikel geldt ook voor federatieve identiteiten, maar deze migraties minder gangbaar zijn.

## <a name="prerequisites"></a>Vereisten
Dit artikel is een vervolg van de gebruiker migratie artikel en legt de nadruk op sociale identiteit migratie. Lees voordat u begint, [gebruikersmigratie](active-directory-b2c-user-migration.md).

## <a name="social-identities-migration-introduction"></a>Sociale identiteiten migratie Inleiding

* In Azure AD B2C, **lokale accounts** aanmelden namen (gebruikersnaam of e-mailadres) worden opgeslagen in de `signInNames` verzameling in de record van de gebruiker. De `signInNames` bevat een of meer `signInName` records die de aanmeldingspagina namen voor de gebruiker opgeven. Elke naam moet uniek zijn in de tenant.

* **Sociale accounts** identiteiten worden opgeslagen in `userIdentities` verzameling. De vermelding wordt de `issuer` (providernaam identiteit) zoals facebook.com en de `issuerUserId`, dit is een unieke gebruikers-id voor de verlener. De `userIdentities` kenmerk bevat een of meer UserIdentity records die het accounttype van sociale en de unieke gebruikers-id van de sociale id-provider opgeven.

* **Lokaal account worden gecombineerd met sociale identiteit**. Zoals gezegd, worden lokaal aanmelden accountnamen en sociale account-id's opgeslagen in verschillende kenmerken. `signInNames` wordt gebruikt voor het lokale account, terwijl `userIdentities` voor sociale-account. Een enkel account voor Azure AD B2C, worden een lokaal account alleen, sociale account alleen, of een lokaal account worden gecombineerd met sociale identiteit in een gebruikersrecord. Dit gedrag kunt u één account beheert terwijl een gebruiker zich met het lokale account credential(s) of met de sociale identiteiten te beheren aanmelden kunt.

* `UserIdentity` Type - informatie bevat over de identiteit van een accountgebruiker sociale in een Azure AD B2C-tenant:
    * `issuer` De tekenreeksrepresentatie van de identiteitsprovider die de gebruikers-id, zoals facebook.com verleend.
    * `issuerUserId` De unieke gebruikers-id die wordt gebruikt door de identiteitsprovider van sociale in base64-indeling.

    ```JSON
    "userIdentities": [{
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
    ```

* Afhankelijk van de id-provider de **sociale gebruikers-ID** is een unieke waarde voor een bepaalde gebruiker per toepassings- of -account. Het Azure AD B2C-beleid configureren met dezelfde toepassings-ID die eerder door de provider van sociale is toegewezen. Of een andere toepassing binnen hetzelfde account ontwikkeling.

## <a name="use-graph-api-to-migrate-users"></a>Graph API gebruiken om gebruikers te migreren
Maken van de Azure AD B2C-gebruikersaccount via [Graph API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet). Om te communiceren met de Graph API, moet u eerst een service-account met beheerdersbevoegdheden hebben. In Azure AD registreert u een toepassing en verificatie met Azure AD. De referenties van de toepassing zijn toepassings-ID en het Toepassingsgeheim. De toepassing fungeert als zichzelf niet als een gebruiker, de API van de grafiek aan te roepen. Volg de aanwijzingen in stap 1 van [gebruikersmigratie](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-user-migration#step-1-use-graph-api-to-migrate-users) artikel.

## <a name="required-properties"></a>Vereiste eigenschappen
De volgende lijst bevat de eigenschappen die vereist zijn wanneer u een gebruiker maken.
* **accountEnabled** : true
* **displayName** -de naam moet worden weergegeven in het adresboek voor de gebruiker.
* **passwordProfile** -profiel van het wachtwoord voor de gebruiker. 

> [!NOTE]
> Voor sociale account (zonder lokale accountreferenties), moet u nog steeds het wachtwoord opgeven. Azure AD B2C wordt het wachtwoord dat u voor sociale accounts opgeeft genegeerd.

* **userPrincipalName** -user principal name (someuser@contoso.com). De user principal name moet een van de geverifieerde domeinen bevatten voor de tenant. Als u de UPN, genereren nieuwe GUID-waarde, samenvoegen met `@` en de tenantnaam van uw.
* **mailNickname** -de mailalias voor de gebruiker. Deze waarde kan dezelfde ID die u voor de userPrincipalName gebruikt zijn. 
* **signInNames** -een of meer SignInName records die de aanmeldingspagina namen voor de gebruiker opgeven. De naam van elke aanmelden moet via de bedrijfsportal/tenant uniek zijn. Voor sociale account alleen kan deze eigenschap leeg zijn.
* **userIdentities** -een of meer UserIdentity records die de sociale opgeeft account type en de unieke gebruikers-id van de identiteitsprovider van sociale.
* [optioneel] **otherMails** : voor het sociaal-account, worden alleen e-mailadressen van de gebruiker 

Zie voor meer informatie: [Graph API-referentiemateriaal](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser)

## <a name="migrate-social-account-only"></a>(Alleen) sociale account migreren
Alleen sociale account zonder lokale accountreferenties maken. HTTPS POST-aanvraag verzenden naar Graph API. De aanvraagtekst bevat de eigenschappen van de accountgebruiker sociale-maken. U moet ten minste de vereiste eigenschappen opgeven. 


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
## <a name="migrate-social-account-with-local-account"></a>Sociaal-account met lokale account migreren
Een gecombineerde lokaal account maken met sociale identiteiten. HTTPS POST-aanvraag verzenden naar Graph API. De aanvraagtekst bevat de eigenschappen van de accountgebruiker sociale-maken. U moet ten minste de vereiste eigenschappen opgeven. 

**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

Na de gegevens van een formulier verzenden: 

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
### <a name="how-can-i-know-the-issuer-name"></a>Hoe weet ik naam van de certificaatverlener
De naam van de verlener of de naam van de id-provider is geconfigureerd in uw beleid. Als u niet dat de waarde op te geven weet `issuer`, volgt u deze procedure:
1. Meld u aan met een van de sociale accounts
2. Kopiëren van de JWT-token de `sub` waarde. De `sub` bevat doorgaans de object-ID van de gebruiker in Azure AD B2C. Of vanuit Azure-portal, opent u de eigenschappen van de gebruiker en kopieer de object-ID.
3. Open [Explorer van Azure AD-grafiek](https://graphexplorer.azurewebsites.net)
4. Aanmelden met uw beheerder. N
5. Voer na GET-aanvraag. De userObjectId vervangen door de gebruikers-ID die u hebt gekopieerd. **TOEVOEGEN** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId
6. Zoek de `userIdentities` -element in het JSON-retourtype van Azure AD B2C.
7. [Optioneel] U kunt ook tot decoderen van de `issuerUserId` waarde.

> [!NOTE]
> Gebruik een B2C-tenant administrator-account dat lokaal is voor de B2C-tenant. De syntaxis van de naam van account is admin@tenant-name.onmicrosoft.com.

### <a name="is-it-possible-to-add-social-identity-to-an-existing-local-account"></a>Is het mogelijk sociale identiteit toevoegen aan een bestaand lokaal account?
Ja. Nadat het lokale account is gemaakt, kunt u de identiteit van de sociale toevoegen. PATCH voor HTTPS-aanvraag worden uitgevoerd. De userObjectId vervangen door de gebruikers-ID die u wilt bijwerken. 

**PATCH** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Na de gegevens van een formulier verzenden: 

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

### <a name="is-it-possible-to-add-multiple-social-identities"></a>Is het mogelijk meerdere sociale identiteiten toevoegen?
Ja. U kunt meerdere sociale identiteiten voor een enkele Azure AD B2C-account toevoegen. PATCH voor HTTPS-aanvraag worden uitgevoerd. De userObjectId vervangen door de gebruikers-ID. 

**PATCH** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Na de gegevens van een formulier verzenden: 

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

## <a name="optional-user-migration-application-sample"></a>[Optioneel] Gebruiker migratie toepassing voorbeeld
[Downloaden en uitvoeren van de voorbeeld-app V2](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). De voorbeeld-app V2 maakt gebruik van een JSON-bestand met dummy-gebruikersgegevens, inclusief: lokale account, sociale-account en lokale & sociale identiteiten in één account.  Het JSON-bestand bewerken, opent u de `AADB2C.UserMigration.sln` Visual Studio-oplossing. In de `AADB2C.UserMigration` project, open de `UsersData.json` bestand. Het bestand bevat een lijst met entiteiten van de gebruiker. Elke gebruikersentiteit heeft de volgende eigenschappen:
* **signInName** : voor het lokale account, e-mailadres aanmelden
* **displayName** -weergavenaam van de gebruiker
* **Voornaam** -voornaam van gebruiker
* **Achternaam** -achternaam van gebruiker
* **wachtwoord** voor lokale account, het wachtwoord van gebruiker (kan niet leeg zijn)
* **certificaatverlener** : voor het sociaal-account, de naam van de id-provider
* **issuerUserId** : voor het sociaal-account, de unieke gebruikers-id die wordt gebruikt door de identiteitsprovider van sociale. De waarde moet in ongecodeerde tekst. De voorbeeld-app worden deze waarde naar base64 gecodeerd.
* **e-mailadres** sociale account alleen (geen gecombineerde) voor gebruikers voor e-mailadres

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
> Als u niet het bestand UsersData.json in de steekproef met uw gegevens bijwerkt, u kunt aanmelden met de referenties van de voorbeeld-lokale account, maar niet met de voorbeelden sociale-account. Geef voor het migreren van uw sociale accounts echte gegevens.

Voor meer informatie het gebruik van de voorbeeld-app, Zie [Azure Active Directory B2C: migratie van de gebruiker](active-directory-b2c-user-migration.md)
