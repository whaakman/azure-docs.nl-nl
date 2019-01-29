---
title: Het toevoegen van app-rollen in uw Azure Active Directory-geregistreerde toepassing en deze ontvangen in het token
description: Informatie over het toevoegen van app-rollen in een toepassing die is geregistreerd bij Azure Active Directory, gebruikers en groepen toewijzen aan deze rollen en ontvangen ze op in de `roles` claim in het token.
services: active-directory
documentationcenter: ''
author: kkrishna
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: ''
ms.custom: aaddev
ms.openlocfilehash: b44aa35765cba33a85519fcd53f0df365c26be6b
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093411"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Procedure: App-rollen in uw toepassing toevoegen en deze ontvangen in het token

Op rollen gebaseerd toegangsbeheer (RBAC) is een populaire mechanisme om af te dwingen autorisatie in toepassingen. Wanneer u RBAC gebruikt, verleent een beheerder machtigingen aan rollen, en niet naar afzonderlijke gebruikers of groepen. De beheerder kan vervolgens rollen toewijzen aan verschillende gebruikers en groepen om te bepalen wie toegang tot welke inhoud en functionaliteit heeft.

Met RBAC met toepassingsrollen en de rol van Claims, kunnen ontwikkelaars veilig afdwingen autorisatie in hun apps zonder veel moeite op te geven.

Een andere benadering is het gebruik van Azure AD-groepen en Claims van de groep, zoals wordt weergegeven in [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet). Azure AD-groepen en rollen van toepassing zijn geen sluiten elkaar wederzijds uit; ze kunnen worden gebruikt in combinatie voor nog betere korrelig toegangsbeheer.

## <a name="declare-roles-for-an-application"></a>Functies voor een toepassing declareren

De rollen voor deze toepassing zijn gedefinieerd in de [Azure-portal](https://portal.azure.com) in manifest van de registratie van de toepassing.  Wanneer een gebruiker zich bij de App aanmeldt, Azure AD verzendt een `roles` claim voor elke rol die de gebruiker afzonderlijk zijn verleend aan de gebruiker en hun groepslidmaatschap.  Toewijzing van gebruikers en groepen aan rollen kan worden gedaan via de gebruikersinterface van de portal of programmatisch met [Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/azuread-identity-access-management-concept-overview).

### <a name="declare-app-roles-using-azure-portal"></a>App-rollen met behulp van Azure portal declareren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Op de bovenste balk, selecteert u uw account en vervolgens **schakelen tussen mappen**.
1. Zodra de **map + abonnement** deelvenster wordt geopend, kies de Active Directory-tenant waar u wilt registreren van uw toepassing uit de **Favorieten** of **alle mappen**lijst.
1. Selecteer **alle services** in het navigatievenster aan de linkerkant, en kies **Azure Active Directory**.
1. In de **Azure Active Directory** venster **App-registraties** om een lijst met al uw toepassingen weer te geven.

     Als u de toepassing die u wilt dat hier weergegeven niet ziet, gebruikt u de verschillende filters aan de bovenkant van de **App-registraties** lijst om te beperken van de lijst of blader naar uw toepassing in de lijst.

1. Selecteer de toepassing die u wilt definiëren, app-rollen in.
1. Selecteer in de blade voor uw toepassing, **Manifest**.
1. Het app-manifest bewerken door te zoeken op de `appRoles` instellen en het toevoegen van alle rollen in uw toepassing.

     > [!NOTE]
     > De roldefinitie van elke in deze manifest moet hebben een andere geldige **Guid** voor de eigenschap 'Id'. De `"value"` eigenschap van elke rol moet exact overeenkomen met de tekenreeksen worden gebruikt in de code in de toepassing.
     
1. Sla het manifest.

### <a name="examples"></a>Voorbeelden

Het volgende voorbeeld wordt de `appRoles` die u kunt toewijzen aan `users`.

> [!NOTE]
>  De `id` moet een unieke GUID.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-98f8-45fd-aa4a-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

U kunt app-rollen definiëren met doel- `users`, `applications`, of beide. Wanneer deze beschikbaar op `applications`, app-rollen worden weergegeven als de machtigingen van de toepassing in de **vereiste machtigingen** blade. Het volgende voorbeeld ziet u een app-rol die is gericht op een `Application`.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "Consumer Apps",
      "id": "47fbb575-859a-4941-89c9-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

### <a name="assign-users-and-groups-to-roles"></a>Gebruikers en groepen toewijzen aan rollen

Wanneer u app-rollen in uw toepassing hebt toegevoegd, kunt u gebruikers en groepen toewijzen aan deze rollen.

1. In de **Azure Active Directory** venster **bedrijfstoepassingen** uit de **Azure Active Directory** navigatiemenu links.
1. Selecteer **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

     Als u de toepassing die u wilt dat hier weergegeven niet ziet, gebruikt u de verschillende filters aan de bovenkant van de **alle toepassingen** lijst om te beperken van de lijst of blader naar uw toepassing in de lijst.

1. Selecteer de toepassing waarin u wilt dat gebruikers of beveiligingsgroep toewijzen aan rollen.
1. Selecteer de **gebruikers en groepen** deelvenster in het navigatiemenu aan de van de toepassing.
1. Aan de bovenkant van de **gebruikers en groepen** in de lijst met de **gebruiker toevoegen** te openen de **toevoegen toewijzing** deelvenster.
1. Selecteer de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

     Een lijst met gebruikers en beveiligingsgroepen worden weergegeven, samen met een tekstvak Zoeken naar en zoeken naar een bepaalde gebruiker of groep. Dit scherm kunt u meerdere gebruikers en groepen selecteren in een go.

1. Wanneer u klaar bent de gebruikers en groepen selecteren, drukt u op de **Selecteer** knop onderaan om te verplaatsen naar het volgende gedeelte.
1. Kies de **rol selecteren** selector vanaf de **toewijzing toevoegen** deelvenster. De rollen die zijn gedeclareerd als eerder in het app-manifest worden weergegeven.
1. Kies een rol en druk op de **Selecteer** knop.
1. Druk op de **toewijzen** knop onder aan het voltooien van de toewijzingen van gebruikers en groepen naar de app.
1. Bevestig dat de gebruikers en groepen die u hebt toegevoegd worden weergegeven in de bijgewerkte **gebruikers en groepen** lijst.

## <a name="more-information"></a>Meer informatie

- [Autorisatie in een web-app met behulp van Azure AD-toepassingsrollen &amp; rolclaims (voorbeeld)](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapp-roleclaims/)
- [Met behulp van beveiligingsgroepen en toepassingsrollen in uw apps (Video)](https://www.youtube.com/watch?v=V8VUPixLSiM)
- [Azure Active Directory, nu met groepclaims en toepassingsrollen](https://cloudblogs.microsoft.com/enterprisemobility/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles)
- [Azure Active Directory-app-manifest](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)
- [AAD-toegangstokens](access-tokens.md)
- [AAD `id_tokens`](id-tokens.md)
