---
title: Bekende problemen in browsers (Microsoft Authentication Library voor JavaScript) | Azure
description: Meer informatie over bekende problemen bij het gebruik van de Microsoft Authentication Library voor JavaScript (MSAL.js) met Safari-browser.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb89b1ef4dbbef234fba3152d7f85bbadfbdc64a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65873883"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>Bekende problemen in Safari-browser met MSAL.js 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Op de achtergrond vernieuwing van het token op Safari 12 en ITP 2.0

Apple iOS 12 en MacOS 10.14 besturingssystemen opgenomen een versie van de [12 Safari-browser](https://developer.apple.com/safari/whats-new/). Voor de doeleinden van beveiliging en privacy, Safari 12 bevat de [intelligente bijhouden preventie 2.0](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/). Dit is in feite zorgt ervoor dat de browser om te verwijderen van cookies van derden wordt ingesteld. ITP 2.0 behandelt ook de cookies die door de id-providers als cookies van derden instellen.

### <a name="impact-on-msaljs"></a>Gevolgen voor de MSAL.js

MSAL.js maakt gebruik van een verborgen Iframe om uit te voeren op de achtergrond ophalen van tokens en -verlenging als onderdeel van de `acquireTokenSilent` aanroepen. De op de achtergrond token aanvragen, is afhankelijk van de Iframe die toegang hebben tot de geverifieerde gebruiker-sessie wordt vertegenwoordigd door de cookies die door Azure AD instellen. Bij toegang tot deze cookies voorkomen ITP 2.0 MSAL.js mislukt op de achtergrond verkrijgen en vernieuwen van tokens en dit resulteert in `acquireTokenSilent` fouten.

Er is geen oplossing voor dit probleem op dit moment en we opties met de community standaards evalueren.

### <a name="work-around"></a>Tijdelijke oplossing

De instelling ITP is standaard ingeschakeld op de Safari-browser. U kunt deze instelling uitschakelen door te navigeren naar **voorkeuren** -> **Privacy** en apparaatverificatie de **te voorkomen dat cross-site bijhouden** optie.

![Safari-instelling](./media/msal-js-known-issue-safari-browser/safari.png)

U moet voor het afhandelen van de `acquireTokenSilent` fouten met een interactieve verkrijgen token aanroep, waarbij u wordt gevraagd de gebruiker zich aanmeldt.
Om te voorkomen dat herhaalde aanmeldingen, een methode die u kunt implementeren om af te handelen is de `acquireTokenSilent` fout en biedt gebruikers een optie voor uitschakelen van de instelling ITP in Safari voordat u doorgaat met de interactieve aanroep. Wanneer de instelling is uitgeschakeld, moeten de volgende op de achtergrond token vernieuwingen slagen.
