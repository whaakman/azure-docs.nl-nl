---
title: Toepassingspagina niet correct weergegeven voor een toepassing toepassingsproxy | Microsoft Docs
description: Richtlijnen wanneer de pagina is niet correct in een toepassing Proxy-toepassing weergegeven hebt geïntegreerd met Azure AD
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: asteen
ms.openlocfilehash: d15c02d86dc4a61e7e92b4c896cfc13dd0218c61
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Toepassingspagina weergegeven niet correct voor een toepassing toepassingsproxy

In dit artikel helpt u bij het oplossen van problemen met Azure Active Directory-toepassingsproxy toepassingen wanneer u naar de pagina navigeren, maar er op de pagina ziet er niet juist.

## <a name="overview"></a>Overzicht
Wanneer u een toepassingsproxy-app publiceert, zijn alleen pagina's in uw hoofdmap toegankelijk bij het openen van de toepassing. Als de pagina is niet correct weergegeven, kan de interne basis-URL voor de toepassing gebruikt enkele pagina resources ontbreken. Om op te lossen, zorg ervoor dat u hebt gepubliceerd *alle* de resources voor de pagina als onderdeel van uw toepassing.

U kunt controleren als bronnen ontbreekt het probleem door het openen van het beheer van uw netwerk (zoals Fiddler of F12 extra in Internet Explorer/Edge), de pagina te laden en zoekt 404-fouten. Die aangeeft dat de pagina's die momenteel is niet gevonden en dat u wilt publiceren.

Als een voorbeeld van deze aanvraag, wordt ervan uitgegaan hebt u een toepassing kosten op basis van de interne URL gepubliceerd http://myapps/expenses, maar de app gebruikmaakt van het opmaakmodel http://myapps/style.css. Het opmaakmodel is in dit geval niet gepubliceerd in uw toepassing, zodat u een 404-fout tijdens het laden van style.css bij het laden van de app uitgaven genereert. In dit voorbeeld wordt het probleem is opgelost door het publiceren van de toepassing met een interne URL http://myapp/.

## <a name="problems-with-publishing-as-one-application"></a>Problemen met de publicatie als één toepassing

Als het niet mogelijk om alle bronnen binnen dezelfde toepassing te publiceren, moet u meerdere toepassingen publiceren en koppelingen tussen deze twee inschakelen.

Om dit te doen, wordt u aangeraden de [aangepaste domeinen](manage-apps/application-proxy-configure-custom-domain.md) oplossing. Deze oplossing vereist echter dat de eigenaar van het certificaat voor uw domein en uw toepassingen volledig gekwalificeerde domeinnamen (FQDN's) gebruiken. Zie voor andere opties, de [verbroken koppelingen documentatie oplossen](application-proxy-page-links-broken-problem.md).

## <a name="next-steps"></a>Volgende stappen
[Toepassingen publiceren met Azure AD-toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md)
