---
title: Toepassingspagina niet correct weergegeven voor een toepassing toepassingsproxy | Microsoft Docs
description: "Richtlijnen wanneer de pagina is niet correct in een toepassing Proxy-toepassing weergegeven hebt geïntegreerd met Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: cac4c333e59ef9a0f28a2f93a7afee22eeafd54e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Toepassingspagina weergegeven niet correct voor een toepassing toepassingsproxy

In dit artikel helpt u bij het oplossen van problemen met Azure Active Directory-toepassingsproxy toepassingen wanneer u naar de pagina navigeren, maar er op de pagina ziet er niet juist.

## <a name="overview"></a>Overzicht
Wanneer u een toepassingsproxy-app publiceert, zijn alleen pagina's in uw hoofdmap toegankelijk bij het openen van de toepassing. Als de pagina is niet correct weergegeven, kan de interne basis-URL voor de toepassing gebruikt enkele pagina resources ontbreken. Om op te lossen, zorg ervoor dat u hebt gepubliceerd *alle* de resources voor de pagina als onderdeel van uw toepassing.

U kunt controleren of dit het probleem is door het openen van het beheer van uw netwerk (zoals Fiddler of F12 extra in Internet Explorer/Edge), de pagina te laden en zoekt 404-fouten. Die aangeeft dat de pagina's die momenteel is niet gevonden en kunnen nog steeds moeten worden gepubliceerd.

Als een voorbeeld van deze aanvraag, wordt ervan uitgegaan hebt u een toepassing kosten op basis van een interne URL van gepubliceerd <http://myapps/expenses>, maar de app gebruikmaakt van het opmaakmodel <http://myapps/style.css>. Het opmaakmodel is in dit geval niet gepubliceerd in uw toepassing, zodat u een 404-fout tijdens het laden van style.css bij het laden van de app uitgaven genereert. In dit voorbeeld zou het probleem worden opgelost door de toepassing met een interne URL publiceert <http://myapp/> in plaats daarvan.

## <a name="problems-with-publishing-as-one-application"></a>Problemen met de publicatie als één toepassing

Als het niet mogelijk om alle bronnen binnen dezelfde toepassing te publiceren, moet u meerdere toepassingen publiceren en koppelingen tussen deze twee inschakelen.

Om dit te doen, wordt u aangeraden de [aangepaste domeinen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) oplossing. Deze oplossing vereist echter dat de eigenaar van het certificaat voor uw domein en uw toepassingen volledig gekwalificeerde domeinnamen (FQDN's) gebruiken. Zie voor andere opties, de [verbroken koppelingen documentatie oplossen](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).

## <a name="next-steps"></a>Volgende stappen
[Toepassingen publiceren met Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md)
