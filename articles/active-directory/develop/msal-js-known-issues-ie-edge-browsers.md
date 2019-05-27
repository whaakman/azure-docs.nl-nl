---
title: Bekende problemen in browsers (Microsoft Authentication Library voor JavaScript) | Azure
description: Meer informatie over bekende problemen bij het gebruik van de Microsoft Authentication Library voor JavaScript (MSAL.js) met Internet Explorer en Microsoft Edge-browser.
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
ms.openlocfilehash: c57ed956ec50c8bac26720a27894c07353928336
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873898"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-with-msaljs"></a>Bekende problemen in Internet Explorer en Microsoft Edge browsers met MSAL.js

## <a name="issues-due-to-security-zones"></a>Problemen als gevolg beveiligingszones
We hadden maar meerdere rapporten van problemen met verificatie in Internet Explorer en Microsoft Edge (sinds het bijwerken van de *Microsoft Edge browser-versie is aan 40.15063.0.0*). We volgen deze en hoogte hebben met betrekking tot het team van Microsoft Edge. Terwijl Microsoft Edge op een resolutie werkt, is hier een beschrijving van de voorkomende problemen en de mogelijke oplossingen die kunnen worden geïmplementeerd.

### <a name="cause"></a>Oorzaak
De oorzaak van de meeste van deze problemen is als volgt. De sessie-opslag en lokale opslag worden gepartitioneerd op basis van de beveiligingszones in de browser Microsoft Edge. In deze specifieke versie van Microsoft Edge, wanneer de toepassing wordt doorgestuurd in zones, worden de opslag van de sessie en de lokale opslag gewist. Met name de sessie-opslag is uitgeschakeld in de navigatiebalk reguliere browser en de sessie- en lokale opslag zijn uitgeschakeld in de InPrivate-modus van de browser. MSAL.js bepaalde status opgeslagen in de sessie-opslag en maakt gebruik van deze status controleren tijdens de verificatiestromen. Als de sessie-opslag is uitgeschakeld, wordt deze status is verbroken en kan daarom resulteert in verbroken ervaringen.

### <a name="issues"></a>Problemen

- **Omleidings-oneindige lus en pagina geladen tijdens de verificatie**. Wanneer gebruikers zich bij de toepassing in Microsoft Edge aanmelden, ze van de AAD-aanmeldingspagina worden omgeleid en zitten vast in een omleidings-oneindige lus leidt tot herhaalde pagina laadt. Dit is doorgaans vergezeld gaan van een `invalid_state` fout in de sessie-opslag.

- **Oneindig verkrijgen token lussen en AADSTS50058 fout**. Wanneer een toepassing die wordt uitgevoerd in Microsoft Edge probeert om een token voor een resource te verkrijgen, de toepassing mogelijk zitten in een oneindige lus van de aanschaf token aanroep samen met de volgende fout van AAD in uw netwerk-trace:

    `Error :login_required; Error description:AADSTS50058: A silent sign-in request was sent but no user is signed in. The cookies used to represent the user's session were not sent in the request to Azure AD. This can happen if the user is using Internet Explorer or Edge, and the web app sending the silent sign-in request is in different IE security zone than the Azure AD endpoint (login.microsoftonline.com)`

- **Pop-upvenster niet sluiten of loopt vast bij het aanmelden met het pop-upvenster gebruik om te verifiëren**. Bij het verifiëren via een pop-upvenster in Microsoft Edge of IE(InPrivate), na het invoeren van referenties en meldt u zich, als er meerdere domeinen voor beveiligingszones zijn betrokken in de navigatiebalk het pop-upvenster niet gesloten omdat MSAL.js de ingang naar verliest het pop-upvenster.  

    Hier vindt u koppelingen naar deze problemen in de Microsoft Edge issue Tracker waar:  
    - [Bug 13861050](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/13861050/)
    - [Bug 13861663](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/13861663/)

### <a name="update-fix-available-in-msaljs-023"></a>Update: Correctie beschikbaar in MSAL.js 0.2.3
Oplossingen voor de verificatie omleidings-lus problemen die zijn vrijgegeven [MSAL.js 0.2.3](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases). De vlag inschakelen `storeAuthStateInCookie` in de configuratie MSAL.js om te profiteren van deze oplossing. Met deze markering is standaard ingesteld op false.

Wanneer de `storeAuthStateInCookie` vlag is ingeschakeld, MSAL.js wordt de browsercookies gebruiken voor het opslaan van de aanvraagstatus voor de validatie van de stromen verificatie vereist.

> [!NOTE]
> Deze oplossing is nog niet beschikbaar voor de msal-angular en msal-angularjs wrappers. Het probleem met het pop-upvensters niet wordt behandeld in deze oplossing.

Gebruik onderstaande tijdelijke oplossingen.

#### <a name="other-workarounds"></a>Andere oplossingen
Zorg ervoor dat u testen of uw probleem alleen op de specifieke versie van Microsoft Edge-browser en werkt op de andere browsers voordoet zich alvorens deze oplossingen.  
1. Als een eerste stap om op te halen om deze problemen, zorg ervoor dat het toepassingsdomein, en andere sites die betrokken zijn bij de omleidingen van de verificatiestroom worden toegevoegd als vertrouwde sites in de beveiligingsinstellingen van de browser, zodat ze deel uitmaken van dezelfde beveiligingszone.
Volg deze stappen om dit te doen:
    - Open **Internet Explorer** en klik op de **instellingen** (tandwielpictogram) in de rechterbovenhoek
    - Selecteer **Internetopties**
    - Selecteer de **Security** tabblad
    - Onder de **vertrouwde Sites** optie, klikt u op de **sites** knop en voegt u de URL's in het dialoogvenster dat wordt geopend.

2. Zoals vermeld voordat er sinds alleen de sessie opslag tijdens de normale navigatie is uitgeschakeld, kunt u MSAL.js voor het gebruik van de lokale opslag in plaats daarvan kunt configureren. Dit kan worden ingesteld als de `cacheLocation` config-parameter bij het initialiseren van MSAL.

Opmerking: dit wordt niet oplossen van het probleem voor InPrivate-browsing omdat zowel de sessie en de lokale opslag worden gewist.

## <a name="issues-due-to-popup-blockers"></a>Toepassingsproblemen vanwege een pop-upvenster blokkeringen

Er zijn gevallen wanneer pop-ups worden geblokkeerd in Internet Explorer of Microsoft Edge, bijvoorbeeld wanneer er een tweede pop-upvenster optreedt tijdens de multi-factor authentication. U ontvangt een waarschuwing in de browser om toe te staan voor de pop-up eenmaal of altijd. Als u ervoor kiest om toe te staan, de browser wordt automatisch het pop-upvenster geopend en retourneert een `null` voor het afhandelen. Als gevolg hiervan de bibliotheek heeft geen een ingang voor het venster en er is geen manier om te sluiten van het pop-upvenster. Hetzelfde probleem niet is gebeurd in Chrome wanneer u wordt gevraagd u wilt toestaan dat pop-ups omdat deze niet automatisch een pop-upvenster geopend.

Als een **tijdelijke oplossing**, ontwikkelaars moet pop-ups in Internet Explorer en Microsoft Edge toestaan voordat ze hun app gebruiken om te voorkomen dat dit probleem.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [MSAL.js in Internet Explorer met behulp van](msal-js-use-ie-browser.md).
