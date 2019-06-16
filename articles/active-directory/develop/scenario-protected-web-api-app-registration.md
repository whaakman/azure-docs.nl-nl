---
title: Beveiligde web-API - app-registratie | Azure
description: Meer informatie over het bouwen van een beveiligde Web-API en de informatie die u nodig hebt om de app te registreren.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22fe71c38678ae789a93ecbc956f24f0b0ebeb01
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111133"
---
# <a name="protected-web-api---app-registration"></a>Beveiligde web-API - app-registratie

In dit artikel wordt uitgelegd dat de gegevens van de app-registratie voor een beveiligde web-API.

Zie [Quickstart: Een toepassing registreren met het Microsoft identity-platform](quickstart-register-app.md) voor de algemene stappen over het registreren van de toepassing.

## <a name="accepted-token-version"></a>Token versie hebben geaccepteerd

Het eindpunt van de Microsoft identity platform kan twee soorten tokens uitgeven: v1.0 tokens en v2.0-tokens. U kunt meer informatie over deze tokens in [toegangstokens](access-tokens.md). De geaccepteerde token versie is afhankelijk van de **ondersteund accounttypen** u hebt gekozen tijdens het maken van uw toepassing:

- Als de waarde van **ondersteund accounttypen** is **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts (zoals Skype, Xbox, Outlook.com)** , de geaccepteerde token versie v2.0 zal zijn.
- Anders worden de geaccepteerde token versie v1.0.

Als u de toepassing hebt gemaakt, kunt u de versie van de geaccepteerde token wijzigen door de volgende stappen:

1. Selecteer uw app in de Azure-portal en selecteer vervolgens de **Manifest** voor uw app.
2. Zoek in het manifest **"accessTokenAcceptedVersion"** , en om te zien die de waarde ervan **2**. Deze eigenschap kunt Azure AD weten dat de web-API v2.0 tokens accepteert. Als het **null**, de geaccepteerde token versie v1.0 zal zijn.
3. Selecteer **Opslaan**.

> [!NOTE]
> Het is aan de web-API om te bepalen welke token versie (v1.0 of v2.0) dat wordt geaccepteerd. Wanneer clients aanvraag een token voor uw web-API met behulp van de Microsoft identity platform v2.0-eindpunt, krijgt deze een token dat wordt aangegeven welke versie wordt geaccepteerd door de web-API.

## <a name="no-redirect-uri"></a>Er is geen omleidings-URI

Web-API's hoeft te registreren van een omleidings-URI als er geen gebruiker aangemeld interactief is.

## <a name="expose-an-api"></a>Een API beschikbaar maakt

Een andere instelling die specifiek zijn voor web-API's is de daarvoor beschikbare API en de beschikbaar gemaakte bereiken.

### <a name="resource-uri-and-scopes"></a>Resource-URI en bereiken

Bereiken zijn meestal van het formulier `resourceURI/scopeName`. Voor Microsoft Graph, hebben de scopes snelkoppelingen zoals `User.Read`, maar deze tekenreeks is slechts een snelkoppeling voor `https://graph.microsoft.com/user.read`.

Tijdens de appregistratie moet u voor het definiëren van de volgende parameters:

- De resource-URI - standaard de portal voor appregistratie raadt u `api://{clientId}`. Deze resource-URI is uniek, maar het is niet mens leesbaar is. U kunt dit wijzigen, maar zorg ervoor dat deze uniek is.
- Een of meer **scopes** (voor clienttoepassingen, worden deze weergegeven als **overgedragen machtigingen** voor uw Web-API)
- Een of meer **app-rollen** (voor clienttoepassingen, worden deze weergegeven als **Toepassingsmachtigingen** voor uw Web-API)

De bereiken worden ook weergegeven op het instemmingsscherm dat wordt weergegeven voor eindgebruikers die uw toepassing gebruiken. Daarom moet u de bijbehorende tekenreeksen die worden beschreven van het bereik opgeven:

- Zoals gezien door de eindgebruiker
- Zoals gezien door de tenantbeheerder, die toestemming van een beheerder kan verlenen

### <a name="how-to-expose-delegated-permissions-scopes"></a>Het blootstellen van gedelegeerde machtigingen (bereiken)

1. Selecteer de **beschikbaar maken van een API** sectie in de registratie van de toepassing, en:
   1. Selecteer **Een bereik toevoegen**.
   1. Als aangevraagd, accepteert u de URI van de voorgestelde toepassings-ID (api :// {clientId}) door te selecteren **opslaan en doorgaan**.
   1. Voer de volgende parameters:
      - Voor **scopenaam**, gebruikt u `access_as_user`.
      - Voor **wie kan instemmen**, zorg ervoor dat de **beheerders en gebruikers** optie is geselecteerd.
      - In **beheerder toestemming weergavenaam**, type `Access TodoListService as a user`.
      - In **beschrijving van de beheerderstoestemming**, type `Accesses the TodoListService Web API as a user`.
      - In **weergavenaam van de gebruiker toestemming**, type `Access TodoListService as a user`.
      - In **beschrijving van de gebruikerstoestemming**, type `Accesses the TodoListService Web API as a user`.
      - Houd **status** ingesteld op **ingeschakeld**.
      - Selecteer **bereik toevoegen**.

### <a name="case-where-your-web-api-is-called-by-daemon-application"></a>Aanvraag waarbij uw Web-API wordt aangeroepen door daemon-toepassing

In dit lid leert u hoe u uw beveiligde Web-API registreren zodat deze veilig kan worden aangeroepen door daemon-toepassingen:

- u moet blootstellen **Toepassingsmachtigingen**. U wordt alleen Toepassingsmachtigingen declareren daemon toepassingen werken niet met gebruikers en daarom gedelegeerde machtigingen niet zinvol is.
- tenantbeheerders kunnen mogelijk Azure AD-tokens probleem voor uw Web-App voor alleen toepassingen die zijn geregistreerd die ze willen toegang tot een van de Web-API apps-machtigingen.

#### <a name="how-to-expose-application-permissions-app-roles"></a>Het blootstellen van machtigingen van de toepassing (app-rollen)

Als u toepassingsmachtigingen beschikbaar, wilt moet u het manifest bewerken.

1. Klik in de toepassingsregistratie voor uw toepassing, op **Manifest**.
1. Het manifest bewerken door te zoeken op de `appRoles` instellen en het toevoegen van een of meer toepassingsrollen. De roldefinitie van de is opgegeven in het onderstaande voorbeeld JSON blok.  Laat de `allowedMemberTypes` naar 'Application' alleen. Zorg ervoor dat de **id** is een unieke guid en **displayName** en **waarde** geen spaties bevatten.
1. Sla het manifest.

De inhoud van `appRoles` moet de volgende (de `id` mag een unieke GUID)

```JSon
"appRoles": [
    {
    "allowedMemberTypes": [ "Application" ],
    "description": "Accesses the TodoListService-Cert as an application.",
    "displayName": "access_as_application",
    "id": "ccf784a6-fd0c-45f2-9c08-2f9d162a0628",
    "isEnabled": true,
    "lang": null,
    "origin": "Application",
    "value": "access_as_application"
    }
],
```

#### <a name="how-to-ensure-that-azure-ad-issues-tokens-for-your-web-api-only-to-allowed-clients"></a>Hoe u om ervoor te zorgen dat Azure AD geeft tokens voor uw Web-API alleen voor clients toegestaan

De Web-API gecontroleerd voor de app-rol (dat wil zeggen de ontwikkelaar manier). Maar u kunt ook Azure Active Directory voor het uitgeven van een token voor uw Web-API alleen voor toepassingen die zijn goedgekeurd door de tenantbeheerder voor toegang tot uw API configureren. Deze extra beveiliging toevoegen:

1. Op de app **overzicht** pagina voor de registratie van uw app, selecteer de hyperlink met de naam van uw toepassing in **beheerde toepassing in lokale map**. De titel op voor dit veld kan worden afgekapt. U kunt bijvoorbeeld lezen: `Managed application in ...`

   > [!NOTE]
   >
   > Wanneer u deze koppeling u naar gaat de **Enterprise Toepassingsoverzicht** pagina die is gekoppeld aan de service-principal voor uw toepassing in de tenant waar u deze hebt gemaakt. U kunt Ga terug naar de registratiepagina van de app met behulp van de knop terug van uw browser.

1. Selecteer de **eigenschappen** pagina in de **beheren** sectie van de onderneming toepassingspagina's
1. Als u AAD om af te dwingen van toegang tot uw Web-API van alleen bepaalde clients wilt, stelt **Gebruikerstoewijzing vereist?** naar **Ja**.

   > [!IMPORTANT]
   >
   > Door in te stellen **Gebruikerstoewijzing vereist?** naar **Ja**, AAD wordt de app-roltoewijzingen van de clients gecontroleerd bij het aanvragen van een toegangstoken voor de Web-API. Als de client is niet worden toegewezen aan elke AppRoles, zou AAD alleen de volgende fout geretourneerd: `invalid_client: AADSTS501051: Application xxxx is not assigned to a role for the xxxx`
   >
   > Als u wilt houden **Gebruikerstoewijzing vereist?** naar **Nee**, <span style='background-color:yellow; display:inline'>Azure AD de app-roltoewijzingen wordt niet gecontroleerd wanneer een client een toegangstoken voor uw Web-API vraagt</span>. Daarom zou een daemon-client (dat wil zeggen een willekeurige client die met behulp van clientreferenties-stroom) nog steeds kunnen verkrijgen van een toegangstoken voor de API door te geven van de doelgroep. Elke toepassing zou kunnen krijgen tot de API zonder te moeten machtigingen voor deze aanvragen. Dit is nu niet vervolgens einde van het, als uw Web-API kunt altijd, zoals wordt beschreven in de volgende sectie, Controleer of de toepassing heeft de juiste rol (die is geautoriseerd door de tenantbeheerder), door te valideren dat het toegangstoken heeft een `roles` claim en de juiste waarde voor t vordering (in ons geval `access_as_application`).

1. Selecteer **opslaan**

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De configuratie van App-code](scenario-protected-web-api-app-configuration.md)
