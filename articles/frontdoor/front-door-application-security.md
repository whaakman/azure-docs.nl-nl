---
title: Azure voordeur Service - laag Toepassingsbeveiliging | Microsoft Docs
description: In dit artikel helpt u begrijpen hoe Azure voordeur Service kunt beschermen en beveiligen van uw back-ends
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: e5714f60b7fdd790f3af8e31250c41038110fd08
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047415"
---
# <a name="application-layer-security-with-front-door"></a>Toepassingsbeveiliging van laag met de voordeur
Azure voordeur Service biedt web application protection mogelijkheid voor het beveiligen van uw webtoepassingen tegen netwerkaanvallen veelvoorkomende web beveiligingslekken zoals SQL-injectie of Cross-Site Scripting (XSS). Ingeschakeld voor HTTP (s) van front-ends, van de voordeur laag Toepassingsbeveiliging is wereldwijd worden gedistribueerd en altijd op aanvallen op de Azure-netwerk moet worden gestopt edge, ver van uw back-ends. Met extra beveiliging en optimalisatie van prestaties, voordeur biedt een snelle en veilige web ervaringen aan uw eindgebruikers.

## <a name="application-protection"></a>Toepassingsbeveiliging
Beveiliging van de voordeur is geconfigureerd op elke omgeving edge over de hele wereld, in overeenstemming met toepassingen, en blokkeert automatisch niet-HTTP (s)-verkeer van uw webtoepassingen is bereikt. Onze gedistribueerde architectuur met meerdere tenants kan algemene beveiliging op schaal zonder verlies van prestaties. Voor workloads van HTTP (s), de voordeur web application protection-service biedt een uitgebreide regelengine voor het aangepaste regels, vooraf geconfigureerde ruleset tegen algemene aanvallen, en gedetailleerde logboekregistratie voor alle aanvragen die overeenkomt met een regel. Flexibele herstelacties met inbegrip van toestaan, blokkeren of logboekbestanden worden alleen ondersteund.

## <a name="custom-access-control-rules"></a>Regels voor aangepaste toegang
- **IP-zwarte en witte lijsten:** kunt u aangepaste regels voor het beheren van toegang tot uw webtoepassingen op basis van de lijst met client-IP-adressen configureren. Zowel de IP-v4-processors en de IP-v6 worden ondersteund
- **Toegangsbeheer op basis van geografische:** u kunt aangepaste regels voor het beheren van toegang tot uw webtoepassingen op basis van de landcode afkomstig van een client-IP-adres is configureren
- **HTTP-parameters filteren:** u kunt regels voor aangepaste toegang op basis van die overeenkomt met de parameters van de aanvraag HTTP (s) met inbegrip van headers, URL, configureren en query uitvoeren op tekenreeksen

## <a name="azure-managed-rules"></a>Azure beheerde regels
- Een vooraf geconfigureerde set regels op basis van veelvoorkomende bovenste OWASP beveiligingslekken is standaard ingeschakeld. Preview-versie is inclusief de set met regels sqli en xss aanvragen controleren. Aanvullende regels worden toegevoegd. U kunt beginnen met de enige actie log voor het valideren van vooraf geconfigureerde regels werken zoals verwacht voor uw toepassingen 

## <a name="rate-limiting"></a>Frequentielimieten instellen
- Een regel voor snelheid is abnormaal veel verkeer vanaf elke client-IP-beperken.  U kunt een drempel instellen op aantal webaanvragen toegestaan door een client-IP-adres tijdens een duur van één minuut.

## <a name="centralized-protection-policy"></a>Beleid voor gecentraliseerde beveiliging
- U kunt diverse beveiligingsregels definiëren en ze toevoegen aan een beleid in volgorde van prioriteit. Aangepaste regels hebben een hogere prioriteit dan de beheerde ruleset om toe te staan van uitzonderingen. Een enkele beleidsregel is gekoppeld aan uw webtoepassing.  Dezelfde web-beveiligingsbeleid wordt gerepliceerd naar alle edge-servers op alle locaties, ervoor te zorgen consistent beveiligingsbeleid in alle regio 's

## <a name="configuration"></a>Configuratie
- Tijdens de preview kunt u REST-API's, PowerShell of CLI kunt gebruiken om te maken en implementeren van de voordeur toepassing beveiligingsregels en het beleid. Portal-toegang wordt ondersteund voordat de service algemeen beschikbaar is. 


## <a name="monitoring"></a>Bewaking
Voordeur biedt de mogelijkheid voor het bewaken van webtoepassingen tegen aanvallen met realtime metrische gegevens die kunnen worden geïntegreerd met Azure Monitor om te houden van waarschuwingen en gemakkelijk trends te bewaken.

## <a name="pricing"></a>Prijzen
Beveiliging van de laag van de voordeur toepassingen is gratis tijdens de Preview-versie.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van een voordeur](quickstart-create-front-door.md).
- Informatie over [de werking van de voordeur](front-door-routing-architecture.md).
