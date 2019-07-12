---
title: Security Center-integratie met Azure Security-producten | Microsoft Docs
description: In dit onderwerp geeft Azure Security-producten die zijn geïntegreerd met Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 805c770f1a7e9bb4e0619b27ac937a2451421dc6
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571736"
---
# <a name="security-center-integration-with-azure-security-products-in-asc"></a>Security Center-integratie met Azure Security-producten in ASC

Security Center biedt klanten met aanvullende Microsoft-licenties voor de Onboarding van hun bevindingen met Security Center en ze weergeven in een geconsolideerde manier.

* [Azure WAF](#azure-waf)
* [Azure DDoS](#azure-ddos)

## Azure WAF <a name="azure-waf"></a>

Azure Application Gateway biedt de functie Web Application Firewall (WAF) voor de gecentraliseerde beveiliging van uw webtoepassingen tegen bekende aanvallen en beveiligingsproblemen.

Webtoepassingen worden steeds vaker het doel van aanvallen die gebruikmaken van bekende beveiligingsproblemen. De Application Gateway WAF is gebaseerd op Core regel ingesteld (CRS) 3.0 of 2.2.9 van van de Open Web Application Security Project (OWASP). De WAF wordt automatisch bijgewerkt ter bescherming tegen nieuwe beveiligingsproblemen, zonder aanvullende configuratie nodig. Waarschuwingen die zijn gegenereerd met WAF worden gestreamd naar Security Center. Zie voor meer informatie over de waarschuwingen die worden gegenereerd door de WAF [artikel](https://docs.microsoft.com/azure/application-gateway/application-gateway-crs-rulegroups-rules?tabs=owasp3#crs911).

## Azure DDoS <a name="azure-ddos"></a>

Distributed denial of service (DDoS)-aanvallen bekend is dat het gemakkelijk om uit te voeren. Daarom komen ze een uitstekende beveiligingsprobleem voor klanten die hun toepassingen naar de cloud wilt verplaatsen. 

Met een DDoS-aanval wordt geprobeerd de resources van een toepassing uit te putten, waardoor de toepassing niet meer beschikbaar is voor legitieme gebruikers. DDoS-aanvallen kunnen alle eindpunten die kan worden bereikt via het Internet zijn gericht.

Azure DDoS protection, gecombineerd met aanbevolen procedures voor de toepassing-ontwerp, bieden een beveiliging tegen DDoS-aanvallen. Azure DDoS protection biedt verschillende Servicelagen. Zie voor meer informatie, [overzicht van Azure DDoS Protection](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

DDoS Protection standaard kan het risico van de volgende typen aanvallen:

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Breng-aanval gedetecteerd**|Van deze aanval doel is om de netwerklaag met een aanzienlijke hoeveelheid schijnbaar legitieme verkeer overspoelen. Het bevat UDP-overstromingen, versterking overstromingen en andere overstromingen vervalst pakket. DDoS Protection standaard vermindert deze potentiële meerdere gigabyte aanvallen door opvangen en scrubbing, schaal van de wereldwijde netwerk van Azure, automatisch.|
|**Protocol-aanval gedetecteerd**|Deze aanvallen weer een doel niet toegankelijk is, door gebruik te maken van een zwak punt in de laag 3 en een layer 4-protocolstack. Hiertoe behoren, SYN overstroming aanvallen, reflectie-aanvallen en andere aanvallen protocol. DDoS Protection standaard vermindert deze aanvallen verschillen tussen schadelijke en legitieme, door interactie met de client en het blokkeren van schadelijk verkeer.|
|**Resource (toepassing) layer-aanval gedetecteerd**|Deze aanvallen als doel web application pakketten, als u wilt de overdracht van gegevens tussen hosts te verstoren. De aanvallen zijn HTTP-protocolschendingen, SQL-injectie, cross-site scripting en andere aanvallen laag 7. Gebruik de Azure Application Gateway web application firewall, met DDoS Protection standaard, om u te beschermen tegen deze aanvallen. Er zijn ook van derden web application firewall aanbiedingen beschikbaar in de Azure Marketplace.|
