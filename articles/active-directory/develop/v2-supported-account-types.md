---
title: Ondersteunde gebruikersaccounts in-apps (doelgroep) - Microsoft identity-platform
description: Algemene documentatie over de doelgroepen en ondersteunde accounttypen in toepassingen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 908ba764dfee7c164c3e6f0ff24d2bbf6a0df287
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544846"
---
# <a name="supported-account-types"></a>Ondersteunde accounttypen

Dit artikel wordt uitgelegd welke accounts (soms doelgroepen genoemd) worden ondersteund in toepassingen

<!-- This section can be in an include for many of the scenarios (SPA, Web App signing-in users, protecting a Web API, Desktop (depending on the flows), Mobile -->

## <a name="supported-accounts-types-in-microsoft-identity-platform-applications"></a>Typen ondersteunde accounts in Microsoft Identity-platform-toepassingen

In de openbare Cloud van Microsoft Azure, kunnen de meeste typen apps gebruikers aanmelden met een doelgroep:

- Als u een regel van Business (LOB)-toepassing schrijft, kunt u gebruikers zich in uw eigen organisatie. Een dergelijke toepassing heet soms **één tenant**.
- Als u een ISV bent, kunt u een toepassing schrijven welke gebruikers zich aanmeldt:

  - In elke organisatie. Een dergelijke toepassing is met de naam een **multitenant** webtoepassing. U zult deze soms lezen gebruikers zich aanmeldt met hun werk- of schoolaccount-accounts.
  - Met hun werk- of school of persoonlijk Microsoft-account.
  - Met alleen persoonlijke Microsoft-account.
    > [!NOTE]
    > Op dit moment het Microsoft identity-platform biedt ondersteuning voor persoonlijke Microsoft-accounts alleen door het registreren van een app voor **werk of school of persoonlijk Microsoft-account**, en vervolgens beperken aanmelden in de code voor de toepassing door op te geven een Azure AD-instantie, bij het bouwen van de toepassing, zoals `https://login.onmicrosoftonline.com/consumers`.

- Als u een bedrijf aan consumenten-toepassing schrijft, kunt u ook gebruikers met hun sociale identiteiten met behulp van Azure AD B2C aanmelden.

## <a name="certain-authentication-flows-dont-support-all-the-account-types"></a>Bepaalde verificatiestromen bieden geen ondersteuning voor alle accounttypen

Sommige accounttypen kunnen niet worden gebruikt met bepaalde verificatiestromen. Bijvoorbeeld, in desktop, UWP-toepassingen of toepassingen die daemon:

- Daemon voor toepassingen kunnen alleen worden gebruikt met Azure Active Directory-organisaties. Het zin geen om te proberen voor het gebruik van daemon-toepassingen voor het bewerken van persoonlijke Microsoft-accounts (de toestemming van een beheerder wordt nooit worden verleend).  
- U kunt de stroom van geïntegreerde Windows-verificatie alleen gebruiken met werk of school-accounts (in uw organisatie of elke organisatie). Inderdaad, geïntegreerde Windows-verificatie werkt met domeinaccounts en de machines worden toegevoegd aan een domein of Azure AD-domein is vereist. Deze stroom zinvol niet zijn voor persoonlijke Microsoft-Accounts.
- De [Resource-eigenaar wachtwoord verlenen](./v2-oauth-ropc.md) (gebruikersnaam en wachtwoord), kan niet worden gebruikt met persoonlijke Microsoft-accounts. Inderdaad, persoonlijke Microsoft-accounts vereist dat de gebruiker toestemming heeft voor toegang tot persoonlijke resources in elke sessie aanmelden. Waarom, wordt dit gedrag is niet compatibel met niet-interactieve stromen.
- De stroom van apparaat nog werkt niet met persoonlijke Microsoft-accounts.

## <a name="supported-account-types-in-national-clouds"></a>Ondersteunde typen in nationale clouds

 Apps kunnen ook aanmelden van gebruikers in [nationale clouds](authentication-national-cloud.md). Echter, persoonlijke Microsoft-accounts worden niet ondersteund in deze clouds (per definitie van deze clouds). Daarom is de ondersteunde typen zijn beperkt, voor deze clouds, naar uw organisatie (enkele tenant) of alle organisaties (toepassingen met meerdere tenants).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Tenants in Azure Active Directory](./single-and-multi-tenant-apps.md)
- Meer informatie over [nationale Clouds](./authentication-national-cloud.md)