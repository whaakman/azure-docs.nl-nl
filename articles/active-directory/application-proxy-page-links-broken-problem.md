---
title: Koppelingen op de pagina werken niet voor een toepassing toepassingsproxy | Microsoft Docs
description: Het oplossen van problemen met verbroken koppelingen op hebt geïntegreerd met Azure AD-toepassingsproxy-toepassingen
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: barbkess
ms.reviewer: harshja
ms.openlocfilehash: 3e356beda3e95748cbee64a180612dd183a7ce0e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592293"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Koppelingen op de pagina werken niet voor een toepassing toepassingsproxy

In dit artikel helpt u bij waarom koppelingen op uw toepassing met Azure Active Directory-toepassingsproxy niet goed werken.

## <a name="overview"></a>Overzicht 
Na het publiceren van een app-toepassingsproxy, zijn de enige koppelingen die standaard in de toepassing werken koppelingen naar bestemmingen die zich in de gepubliceerde basis-URL. De koppelingen in de toepassingen zijn niet werkt, de interne URL voor de toepassing bevatten waarschijnlijk niet de doelen van koppelingen binnen de toepassing.

**Waarom komt dat?** Wanneer u op een koppeling in een toepassing, probeert de toepassingsproxy omzetten van de URL als een interne URL binnen dezelfde toepassing, of als een extern beschikbare URL. Als de koppeling naar een interne URL valt niet binnen dezelfde toepassing verwijst, deze niet behoren tot een van deze buckets en resulteert in een fout niet gevonden.

## <a name="ways-you-can-resolve-broken-links"></a>Manieren waarop die u het kunt oplossen verbroken koppelingen

Er zijn drie manieren om dit probleem te verhelpen. De opties hieronder in staan in oplopende complexiteit.

1.  Zorg ervoor dat de interne URL een toegangspunt met de relevante koppelingen voor de toepassing. Hierdoor kunnen alle koppelingen naar worden omgezet naar inhoud die wordt gepubliceerd binnen dezelfde toepassing.

    Als u de URL van de interne wijzigen, maar niet wilt wijzigen van de startpagina voor gebruikers, wijzigt u de URL van de aan de eerder gepubliceerde interne URL. Dit kan worden gedaan door te gaan naar 'Azure Active Directory' -&gt; registraties van App -&gt; selecteert u de toepassing -&gt; eigenschappen. Op dit tabblad Eigenschappen ziet u het veld "startpagina URL', dat u moet de gewenste startpagina kunt aanpassen.

2.  Als uw toepassingen volledig gekwalificeerde domeinnamen (FQDN's), gebruik [aangepaste domeinen](manage-apps/application-proxy-configure-custom-domain.md) om uw toepassingen te publiceren. Deze functie kunt dezelfde URL zijn voor zowel intern en extern.

    Deze optie zorgt ervoor dat de koppelingen in uw toepassing extern toegankelijk is via toepassingsproxy omdat de koppelingen in de toepassing naar interne URL's ook extern worden herkend. Alle koppelingen moeten nog steeds te behoren tot een gepubliceerde toepassing. Met deze optie de koppelingen niet hoeft echter te behoren tot dezelfde toepassing en tot meerdere toepassingen kunt behoren.

3.  Als geen van beide opties haalbaar is, kunt u een nieuwe functie die wordt het URL-omzetting/herschrijven bekijken. Met deze functie interne URL's of koppelingen die aanwezig zijn in de hoofdtekst HTML van uw toepassingen worden vertaald of 'toegewezen', aan de gepubliceerde externe App Proxy-URL's. Deze vertaling werkt alleen op de koppelingen in de HTML-indeling of CSS, en niet als de koppeling wordt gegenereerd via JS helpen. 

Als gevolg hiervan wordt aangeraden met behulp van de [aangepaste domeinen](manage-apps/application-proxy-configure-custom-domain.md) oplossing indien mogelijk. Als u deelnemen aan de preview wilt, e- <aadapfeedback@microsoft.com> met de applicationId(s).

## <a name="next-steps"></a>Volgende stappen
[Werken met bestaande lokale proxyservers](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)

