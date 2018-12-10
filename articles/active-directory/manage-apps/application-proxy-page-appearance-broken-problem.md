---
title: Toepassingspagina wordt niet correct weergegeven voor een toepassingsproxy-toepassing | Microsoft Docs
description: Richtlijnen voor wanneer de pagina wordt niet correct in een toepassing voor een toepassingsproxy weergegeven hebt geïntegreerd met Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 6354b4b0224effb1eef063779dba736cb6263286
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53141625"
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Toepassingspagina wordt niet correct weergegeven voor een toepassingsproxy-toepassing

Dit artikel helpt u problemen met Azure Active Directory-toepassingsproxy-toepassingen oplossen wanneer u gaat u naar de pagina, maar iets op de pagina ziet er niet juist.

## <a name="overview"></a>Overzicht
Wanneer u een Application Proxy-app publiceert, zijn alleen pagina's in de hoofdmap van uw toegankelijk bij het openen van de toepassing. Als de pagina wordt niet correct weergegeven, kan de interne basis-URL die wordt gebruikt voor de toepassing enkele pagina resources ontbreken. Als u wilt oplossen, zorg ervoor dat u hebt gepubliceerd *alle* de resources voor de pagina als onderdeel van uw toepassing.

U kunt controleren als het probleem ontbrekende resources is door het openen van het beheer van uw netwerk (zoals Fiddler of F12 tools in Internet Explorer/Edge), het laden van de pagina en zoek naar 404-fouten. Dat geeft aan de pagina's op dit moment kunnen niet worden gevonden en dat u wilt publiceren.

Als een voorbeeld van deze aanvraag, wordt ervan uitgegaan een onkosten-toepassing met behulp van de interne URL publiceert http://myapps/expenses, maar de app gebruikmaakt van het opmaakmodel http://myapps/style.css. Het opmaakmodel is in dit geval niet gepubliceerd in uw toepassing, zodat het laden van de app onkosten genereert een 404-fout tijdens het laden van style.css. In dit voorbeeld wordt het probleem is opgelost door het publiceren van de toepassing met een interne URL http://myapp/.

## <a name="problems-with-publishing-as-one-application"></a>Problemen met het publiceren als een toepassing

Als het niet mogelijk om alle resources binnen dezelfde toepassing te publiceren, moet u meerdere toepassingen publiceren en koppelingen tussen hen inschakelen.

Om dit te doen, wordt u aangeraden de [aangepaste domeinen](application-proxy-configure-custom-domain.md) oplossing. Deze oplossing is echter vereist dat u daar het certificaat voor uw domein en uw toepassingen volledig gekwalificeerde domeinnamen (FQDN's) gebruiken. Zie voor andere opties, de [oplossen verbroken koppelingen de documentatie bij](application-proxy-page-links-broken-problem.md).

## <a name="next-steps"></a>Volgende stappen
[Toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](application-proxy-add-on-premises-application.md)
