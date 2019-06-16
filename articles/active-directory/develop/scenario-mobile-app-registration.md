---
title: Mobiele app of aanroepen van web-API's - de configuratie van app-code | Microsoft identity-platform
description: Informatie over het bouwen van een mobiele app of aanroepen van web-API's (configuratie van de app-code)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2b6ebab0eeca6895e1c7a0f6008972030d81da42
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65962402"
---
# <a name="mobile-app-that-calls-web-apis---app-registration"></a>Mobiele app of aanroepen van web-API's - app-registratie

In dit artikel bevat de app-registratie-instructies voor het maken van een mobiele toepassing.

## <a name="supported-account-types"></a>Ondersteunde accounttypen

De accounttypen worden ondersteund in mobiele toepassingen, is afhankelijk van de ervaring die u wilt inschakelen en gebruikers die uw app is gericht.

## <a name="platform-configuration-and-redirect-uris"></a>Platformconfiguratie en de omleidings-URI 's  

Bij het bouwen van een mobiele app, is de meest kritieke registratiestap de omleidings-URI. Dit kan worden ingesteld via de [platformconfiguratie in de verificatie-blade](https://aka.ms/MobileAppReg).

Deze ervaring wordt uw app krijgen eenmalige aanmelding (SSO) via de Microsoft Authenticator (en de Intune-bedrijfsportal voor Android) en ondersteuning beheerbeleid voor apparaten inschakelen.

Als u liever handmatig configureren van de omleidings-URI, kunt u dit doen via het Manifest van de toepassing. De aanbevolen indeling is het volgende:

- ***iOS***: `msauth.<BUNDLE_ID>://auth`
- ***Android***: `msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - De Android-handtekening-hash kan worden gegenereerd met behulp van de sleutels release of foutopsporing via de opdracht KeyTool.

## <a name="api-permissions"></a>API-machtigingen

Mobiele toepassingen aanroepen van API's namens de aangemelde gebruiker. Uw app nodig heeft om aan te vragen van gedelegeerde machtigingen, ook wel bereiken. Afhankelijk van de gewenste ervaring, kan dit worden gedaan statisch via Azure portal of dynamisch tijdens de uitvoering. Registreren van statisch machtigingen kan beheerders eenvoudig uw app goedkeuren en wordt aanbevolen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een token verkrijgen](scenario-mobile-acquire-token.md)
