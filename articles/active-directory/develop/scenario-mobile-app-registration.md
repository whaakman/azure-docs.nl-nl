---
title: Mobiele app die web-Api's aanroept-code configuratie van de app | Micro soft Identity-platform
description: Meer informatie over het bouwen van een mobiele app die web-Api's aanroept (de code configuratie van de app)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
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
ms.openlocfilehash: f43ae9da51f68c9765a36d27c993d1c9935d61fa
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326127"
---
# <a name="mobile-app-that-calls-web-apis---app-registration"></a>Mobiele app die web-Api's aanroept-app-registratie

Dit artikel bevat de instructies voor het registreren van apps voor het maken van een mobiele toepassing.

## <a name="supported-account-types"></a>Ondersteunde accounttypen

De account typen die in mobiele toepassingen worden ondersteund, zijn afhankelijk van de ervaring die u wilt inschakelen en gebruikers van uw app.

## <a name="platform-configuration-and-redirect-uris"></a>Platform configuratie en omleidings-Uri's  

Bij het bouwen van een mobiele app is de meest kritieke registratie stap de omleidings-URI. Dit kan worden ingesteld via de [platform configuratie op de Blade authenticatie](https://aka.ms/MobileAppReg).

Met deze ervaring kan uw app gebruikmaken van eenmalige aanmelding (SSO) via de Microsoft Authenticator (en Intune-bedrijfsportal op Android) en wordt het beheer beleid voor apparaten ondersteund.

Als u de omleidings-URI liever hand matig wilt configureren, kunt u dit doen via het manifest van de toepassing. De aanbevolen indeling is als volgt:

- ***iOS***: `msauth.<BUNDLE_ID>://auth`
- ***Android***:`msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - De hash van de Android-hand tekening kan worden gegenereerd met de release-of fout opsporings sleutels via de opdracht van het hulp programma.

## <a name="api-permissions"></a>API-machtigingen

Mobiele toepassingen bellen Api's namens de aangemelde gebruiker. Uw app moet gedelegeerde machtigingen aanvragen, ook wel bereiken genoemd. Afhankelijk van de gewenste ervaring kan dit statisch worden gedaan via de Azure Portal of dynamisch tijdens runtime. Met statische registratie machtigingen kunnen beheerders uw app eenvoudig goed keuren en wordt aanbevolen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een token verkrijgen](scenario-mobile-acquire-token.md)
