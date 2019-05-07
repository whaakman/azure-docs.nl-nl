---
title: Daemon-app aanroepen van web API's (overzicht) - Microsoft identity-platform
description: Informatie over het bouwen van een daemon-app die web-API's aanroepen
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
ms.openlocfilehash: 578b7cdb38b7df3fab5885d773354a36f76a4cfb
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075878"
---
# <a name="scenario-daemon-application-that-calls-web-apis"></a>Scenario: Daemon-toepassing die aanroepen van web-API 's

Meer informatie over alles die wat u nodig om een daemontoepassing die web-API's aanroept te bouwen.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="overview"></a>Overzicht

Uw toepassing kunt verkrijgen van een token voor het aanroepen van een web-API namens zichzelf (niet namens een gebruiker). Dit scenario is handig voor toepassingen met daemon. Er wordt met behulp van de standaard OAuth 2.0 [clientreferenties](v2-oauth2-client-creds-grant-flow.md) verlenen.

![Daemon-apps](./media/scenario-daemon-app/daemon-app.svg)

Hier volgen enkele voorbeelden van use cases voor daemon-apps:

- Webtoepassingen die worden gebruikt voor het inrichten of beheren van gebruikers of batchprocessen in een map
- Bureaubladtoepassingen zoals (windows-services op Windows) of daemons processen op Linux die batch-taken of een besturingssysteem-service wordt uitgevoerd op de achtergrond uitvoeren
- Web-API's die nodig hebt voor het bewerken van mappen, geen specifieke gebruikers

Er is een andere gevallen waarbij niet-daemon-toepassingen clientreferenties gebruiken: zelfs wanneer ze namens gebruikers handelen, hebben ze nodig voor toegang tot een web-API of een resource met hun identiteit omwille van de technische. Een voorbeeld is de toegang tot geheimen in Key Vault of Azure SQL-database voor een cache.

Toepassingen die een token voor hun eigen identiteiten verkrijgen:

- zijn vertrouwelijke client-toepassingen. Deze apps, gezien het feit dat ze toegang krijgen resources, onafhankelijk van een gebruiker tot nodig hebt om hun identiteit te bewijzen. Ze zich ook in plaats van gevoelige apps die ze nodig hebben om te worden goedgekeurd door de beheerders van de tenant Azure Active Directory (Azure AD).
- Een geheim (toepassingswachtwoord of certificaat) hebt met Azure AD worden geregistreerd. Dit geheim wordt doorgegeven in tijdens de oproep aan Azure AD om een token verkrijgen.

## <a name="specifics"></a>Specifieke informatie

> [!IMPORTANT]
>
> - Tussenkomst van de gebruiker is niet mogelijk is met een daemon-toepassing. Een daemontoepassing vereist een eigen identiteit. Dit type toepassing vraagt een toegangstoken met behulp van de toepassings-id en presenteert de toepassings-ID, de referentie (wachtwoord of certificaat) en de toepassing-ID-URI naar Azure AD. Na een geslaagde authenticatie ontvangt de daemon voor een toegangstoken (en een vernieuwingstoken) van het Microsoft identity platform eindpunt, die vervolgens wordt gebruikt voor het aanroepen van de web-API (en wordt vernieuwd, indien nodig).
> - Omdat tussenkomst van de gebruiker niet mogelijk is, zijn incrementele toestemming niet mogelijk. Alle vereiste API-machtigingen moeten worden geconfigureerd tijdens de toepassingsregistratie en de code van de toepassing alleen statisch gedefinieerde machtigingen aanvraagt. Dit betekent ook dat daemon toepassingen incrementele toestemming wordt niet ondersteund.

Voor ontwikkelaars heeft de end-to-end-ervaring voor dit scenario de volgende aspecten:

- Daemon voor toepassingen kunnen uitsluitend worden gebruikt in Azure AD-tenants. Dit heeft geen zin om een daemontoepassing die toegang probeert te manipuleren van persoonlijke Microsoft-accounts te bouwen. Als u een line-of-business (LOB)-app-ontwikkelaar bent, kunt u uw daemon-app maken in uw tenant. Als u een ISV bent, is het raadzaam om een multitenant daemontoepassing te maken. Het moet worden gegeven door elke tenant-beheerder.
- Tijdens de [toepassingsregistratie](./scenario-daemon-app-registration.md), wordt de **antwoord-URI** niet nodig is. U moet voor het delen van geheimen of certificaten met Azure AD en moet u toepassingsmachtigingen aanvragen en verlenen van toestemming van een beheerder om deze app-machtigingen te gebruiken.
- De [Toepassingsconfiguratie](./scenario-daemon-app-configuration.md) moet de referenties van de client als een gedeeld met Azure AD tijdens de registratie van de toepassing opgeven.
- De [bereik](scenario-daemon-acquire-token.md#scopes-to-request) voor het verkrijgen van een token met de referenties van de client stroom nodig heeft om te worden van een statische bereik.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Daemon-app - app-registratie](./scenario-daemon-app-registration.md)
