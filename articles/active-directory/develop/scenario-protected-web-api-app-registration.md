---
title: Beveiligde web-API - app-registratie | Azure
description: Meer informatie over het bouwen van een beveiligde web-API en de informatie die u nodig hebt om de app te registreren.
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
ms.openlocfilehash: e4622cffedc159ce85166eafe571ccb26c2c1b4d
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536867"
---
# <a name="protected-web-api-app-registration"></a>Beveiligde web-API: App-registratie

In dit artikel wordt uitgelegd van de details van app-registratie voor een beveiligde web-API.

Zie [Quickstart: Een toepassing registreren met het Microsoft identity-platform](quickstart-register-app.md) voor de algemene stappen voor het registreren van een app.

## <a name="accepted-token-version"></a>Token versie hebben geaccepteerd

Het eindpunt van de Microsoft identity platform kan twee soorten tokens uitgeven: v1.0 tokens en v2.0-tokens. Zie voor meer informatie over deze tokens [toegangstokens](access-tokens.md). De geaccepteerde token versie is afhankelijk van de **ondersteund accounttypen** u hebt gekozen tijdens het maken van uw toepassing:

- Als de waarde van **ondersteund accounttypen** is **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts (zoals Skype, Xbox, Outlook.com)** , de geaccepteerde token versie v2.0 zal zijn.
- Anders worden de geaccepteerde token versie v1.0.

Nadat u de toepassing hebt gemaakt, kunt u bepalen of de versie van de geaccepteerde token wijzigen door de volgende stappen:

1. Selecteer uw app in de Azure-portal en selecteer vervolgens de **Manifest** voor uw app.
2. Zoek in het manifest **"accessTokenAcceptedVersion"** . Merk op dat de waarde ervan **2**. Deze eigenschap geeft u aan Azure Active Directory (Azure AD) dat de web-API v2.0 tokens accepteert. Als de waarde **null**, de versie van de geaccepteerde token v1.0 is.
3. Als u de token versie hebt gewijzigd, selecteert u **opslaan**.

> [!NOTE]
> De web-API geeft aan welke token versie (v1.0 of v2.0) dat wordt geaccepteerd. Wanneer clients een token voor uw web-API van het Microsoft identity platform v2.0-eindpunt aanvraagt, krijgt deze een token dat wordt aangegeven welke versie wordt geaccepteerd door de web-API.

## <a name="no-redirect-uri"></a>Er is geen omleidings-URI

Web-API's hoeft te registreren, een omleidings-URI omdat er geen gebruiker is aangemeld interactief.

## <a name="expose-an-api"></a>Een API beschikbaar maakt

Een andere instelling die specifiek zijn voor web-API's is de daarvoor beschikbare API en de beschikbaar gemaakte bereiken.

### <a name="resource-uri-and-scopes"></a>Resource-URI en bereiken

Bereiken zijn meestal in de vorm `resourceURI/scopeName`. Voor Microsoft Graph, hebben de scopes snelkoppelingen zoals `User.Read`. Deze tekenreeks vormt een snelkoppeling naar `https://graph.microsoft.com/user.read`.

Tijdens de appregistratie moet u deze parameters definiëren:

- De resource-URI. Standaard wordt de portal voor appregistratie aanbevolen die u gebruiken `api://{clientId}`. Deze resource-URI is uniek, maar het is niet mens leesbaar is. U kunt dit wijzigen, maar zorg ervoor dat de nieuwe waarde is uniek.
- Een of meer *scopes*. (Voor clienttoepassingen, ze wordt weergegeven als *overgedragen machtigingen* voor uw web-API.)
- Een of meer *app-rollen*. (Voor clienttoepassingen, ze wordt weergegeven als *Toepassingsmachtigingen* voor uw web-API.)

De bereiken worden ook weergegeven op het instemmingsscherm dat wordt weergegeven voor eindgebruikers van uw app. U moet dus de overeenkomstige tekenreeksen die worden beschreven van het bereik opgeven:

- Zoals gezien door de eindgebruiker.
- Zoals gezien door de tenantbeheerder, die toestemming van een beheerder kan verlenen.

### <a name="exposing-delegated-permissions-scopes"></a>Gedelegeerde machtigingen (bereiken) weergeven

1. Selecteer de **beschikbaar maken van een API** sectie in de registratie van de toepassing.
1. Selecteer **Een bereik toevoegen**.
1. Als u wordt gevraagd, accepteert u de URI van de voorgestelde toepassings-ID (`api://{clientId}`) door te selecteren **opslaan en doorgaan**.
1. Deze parameters opgeven:
      - Voor **scopenaam**, gebruikt u **access_as_user**.
      - Voor **wie kan instemmen**, zorg ervoor dat **beheerders en gebruikers** is geselecteerd.
      - In **beheerder toestemming weergavenaam**, voer **TodoListService als een gebruiker toegang**.
      - In **beschrijving van de beheerderstoestemming**, voer **toegang heeft tot de TodoListService Web-API als een gebruiker**.
      - In **weergavenaam van de gebruiker toestemming**, voer **TodoListService als een gebruiker toegang**.
      - In **beschrijving van de gebruikerstoestemming**, voer **toegang heeft tot de TodoListService Web-API als een gebruiker**.
      - Houd **status** ingesteld op **ingeschakeld**.
      - Selecteer **bereik toevoegen**.

### <a name="if-your-web-api-is-called-by-a-daemon-app"></a>Als uw web-API wordt aangeroepen door een daemon-app

In deze sectie leert u hoe u uw beveiligde web-API registreren, zodat deze veilig kan worden aangeroepen door daemon-apps.

- u moet blootstellen *Toepassingsmachtigingen*. U kunt alleen de machtigingen van de toepassing moet declareren, omdat de daemon-apps niet communiceren met gebruikers, zodat u gedelegeerde machtigingen heeft geen zin.
- Tenantbeheerders kunnen Azure Active Directory (Azure AD) moeten probleem tokens voor uw web-API alleen voor toepassingen die zijn geregistreerd voor toegang tot een van de machtigingen van de web-API's van de toepassing.

#### <a name="exposing-application-permissions-app-roles"></a>Machtigingen van de toepassing (app-rollen) weergeven

Als u toepassingsmachtigingen beschikbaar, wilt moet u het manifest bewerken.

1. Selecteer in de toepassingsregistratie voor uw toepassing, **Manifest**.
1. Het manifest bewerken door te zoeken op de `appRoles` instellen en het toevoegen van een of meer toepassingsrollen. De roldefinitie van de is opgegeven in het volgende voorbeeld-JSON-blok. Laat de `allowedMemberTypes` ingesteld op `"Application"` alleen. Zorg ervoor dat de `id` is een unieke GUID en die `displayName` en `value` geen spaties bevatten.
1. Sla het manifest.

Het volgende voorbeeld ziet u de inhoud van `appRoles`. (De `id` kan een unieke GUID zijn.)

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

#### <a name="ensuring-that-azure-ad-issues-tokens-for-your-web-api-to-only-allowed-clients"></a>Ervoor te zorgen dat problemen met Azure AD-tokens voor uw web-API voor het alleen clients toegestaan

De web-API gecontroleerd voor de rol van de app. (Dat is de ontwikkelaar manier om machtigingen van de toepassing zichtbaar te maken.) Maar u kunt ook Azure AD voor het uitgeven van een token voor uw web-API alleen op apps die zijn goedgekeurd door de tenantbeheerder voor toegang tot uw API configureren. Deze verbeterde beveiliging toevoegen:

1. Op de app **overzicht** pagina voor de registratie van uw app, selecteer de koppeling met de naam van uw app bij **beheerde toepassing in lokale map**. De titel op voor dit veld kan worden afgekapt. U kunt bijvoorbeeld zien **beheerde toepassing in...**

   > [!NOTE]
   >
   > Wanneer u deze koppeling selecteert, gaat u naar de **Enterprise Toepassingsoverzicht** pagina die is gekoppeld aan de service-principal voor uw toepassing in de tenant waar u deze hebt gemaakt. U kunt Ga terug naar de registratiepagina van de app met behulp van de knop terug van uw browser.

1. Selecteer de **eigenschappen** pagina in de **beheren** sectie van de onderneming toepassingspagina's.
1. Als u Azure AD om toegang te verlenen aan uw web-API van alleen bepaalde clients wilt, stelt **Gebruikerstoewijzing vereist?** naar **Ja**.

   > [!IMPORTANT]
   >
   > Als u instelt **Gebruikerstoewijzing vereist?** naar **Ja**, Azure AD wordt de app-roltoewijzingen van de clients gecontroleerd bij het aanvragen van een toegangstoken voor de web-API. Als de client niet is toegewezen aan een app-rollen, Azure AD wordt geretourneerd met de fout `invalid_client: AADSTS501051: Application <application name> is not assigned to a role for the <web API>`.
   >
   > Als u wilt houden **Gebruikerstoewijzing vereist?** ingesteld op **Nee**, *Azure AD de app-roltoewijzingen wordt niet gecontroleerd wanneer een client een toegangstoken voor uw web-API vraagt*. Een daemon-client (dat wil zeggen, een client met behulp van de clientreferenties-stroom) is mogelijk een toegangstoken verkrijgen voor de API door te geven van de doelgroep. Elke toepassing zich voor toegang tot de API zonder te moeten machtigingen voor deze aanvragen. Maar uw web-API kunt altijd, zoals wordt beschreven in de vorige sectie, moet u controleren dat de toepassing de juiste rol heeft (die is gemachtigd door de tenantbeheerder). De API voert deze uit door te valideren dat het toegangstoken een rollen heeft claim en dat de waarde voor deze claim juist is. (In ons geval de waarde is `access_as_application`.)

1. Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De configuratie van App-code](scenario-protected-web-api-app-configuration.md)
