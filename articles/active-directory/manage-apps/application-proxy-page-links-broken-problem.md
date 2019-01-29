---
title: Koppelingen op de pagina werken niet voor een toepassingsproxy-toepassing | Microsoft Docs
description: Het oplossen van problemen met verbroken koppelingen op toepassingsproxy-toepassingen die u hebt geïntegreerd met Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 585ffd966bc3ba985e2b5b93529cadba8ce65960
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55151852"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Koppelingen op de pagina werken niet voor een toepassingsproxy-toepassing

Dit artikel helpt u waarom koppelingen op uw Azure Active Directory Application Proxy-toepassing niet goed werken.

## <a name="overview"></a>Overzicht 
Na het publiceren van een app Application Proxy, zijn de enige koppelingen die standaard in de toepassing werken koppelingen naar bestemmingen die zijn opgenomen in de gepubliceerde basis-URL. De koppelingen in de toepassingen niet werken, de interne URL voor de toepassing bevatten waarschijnlijk niet de bestemmingen van koppelingen in de toepassing.

**Waarom gebeurt dit?** Wanneer u op een koppeling in een toepassing, probeert de Application Proxy om op te lossen van de URL als een interne URL binnen dezelfde toepassing of als de URL van een extern beschikbaar. Als de koppeling naar een interne URL die zich niet binnen dezelfde toepassing verwijst, deze niet behoren tot een van deze buckets en leiden tot een fout niet gevonden.

## <a name="ways-you-can-resolve-broken-links"></a>Manieren waarop die u het kunt oplossen verbroken koppelingen

Er zijn drie manieren om dit probleem te verhelpen. De opties hieronder worden in weergegeven in de toenemende complexiteit.

1.  Zorg ervoor dat de interne URL een hoofdmap waarin alle relevante koppelingen voor de toepassing. Hiermee worden alle koppelingen naar inhoud binnen dezelfde toepassing gepubliceerd worden omgezet.

    Als u de URL van de interne wijzigen, maar niet wilt wijzigen van de landingspagina voor gebruikers, wijzigt u de URL van startpagina naar de eerder gepubliceerde interne URL. Dit kan worden gedaan door te gaan naar 'Azure Active Directory' -&gt; App-registraties -&gt; selecteert u de toepassing -&gt; eigenschappen. Op dit tabblad Eigenschappen ziet u het veld "startpagina-URL, die u moet de startpagina van de gewenste kunt aanpassen.

2.  Als uw toepassingen volledig gekwalificeerde domeinnamen (FQDN's), gebruik [aangepaste domeinen](application-proxy-configure-custom-domain.md) om uw toepassingen te publiceren. Deze functie kunt dezelfde URL moet worden gebruikt zowel intern als extern.

    Deze optie zorgt ervoor dat de koppelingen in uw toepassing extern toegankelijk zijn via de toepassingsproxy zijn omdat de koppelingen in de toepassing naar interne URL's ook extern worden herkend. Er moet nog steeds alle koppelingen te behoren tot een gepubliceerde toepassing. Echter met deze optie de koppelingen niet hoeft te behoren tot dezelfde toepassing en kunnen deel uitmaken van meerdere toepassingen.

3.  Als geen van beide opties haalbaar is zijn, zijn er meerdere opties voor het inschakelen van de vertaling van inline-koppeling. Deze opties zijn onder andere met behulp van de Intune Managed Browser, uitbreiding van mijn Apps, of de instelling van de vertaling koppeling op uw toepassing. Zie voor meer informatie over elk van deze opties en hoe u ze wilt inschakelen, [vastgelegd koppelingen voor apps die zijn gepubliceerd met Azure AD Application Proxy omleiden](application-proxy-configure-hard-coded-link-translation.md).

## <a name="next-steps"></a>Volgende stappen
[Werken met bestaande on-premises proxy-servers](application-proxy-configure-connectors-with-proxy-servers.md)

