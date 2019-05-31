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
ms.openlocfilehash: 59af4e20c7fe838f7c725b47e45968941fa85cb7
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254057"
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

- Een resource-URI - standaard de portal voor appregistratie raadt u `api://{clientId}`. Deze resource-URI is uniek, maar het is niet mens leesbaar is. U kunt dit wijzigen, maar zorg ervoor dat deze uniek is.
- Een of meer scopes

De bereiken worden ook weergegeven op het instemmingsscherm dat wordt weergegeven aan eindgebruikers die uw toepassing gebruiken. Daarom moet u de bijbehorende tekenreeksen die worden beschreven van het bereik opgeven:

- Zoals gezien door de eindgebruiker
- Zoals gezien door de tenantbeheerder, die toestemming van een beheerder kan verlenen

### <a name="how-to-expose-the-api"></a>Het beschikbaar maken van de API

1. Selecteer de **beschikbaar maken van een API** sectie in de registratie van de toepassing, en:
   1. Selecteer **Een bereik toevoegen**.
   1. Accepteer de URI van de voorgestelde toepassings-ID (api :// {clientId}) door te selecteren **opslaan en doorgaan**.
   1. Voer de volgende parameters:
      - Voor **scopenaam**, gebruikt u `access_as_user`.
      - Voor **wie kan instemmen**, zorg ervoor dat de **beheerders en gebruikers** optie is geselecteerd.
      - In **beheerder toestemming weergavenaam**, type `Access TodoListService as a user`.
      - In **beschrijving van de beheerderstoestemming**, type `Accesses the TodoListService Web API as a user`.
      - In **weergavenaam van de gebruiker toestemming**, type `Access TodoListService as a user`.
      - In **beschrijving van de gebruikerstoestemming**, type `Accesses the TodoListService Web API as a user`.
      - Houd **status** ingesteld op **ingeschakeld**.
      - Selecteer **bereik toevoegen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De configuratie van App-code](scenario-protected-web-api-app-configuration.md)
