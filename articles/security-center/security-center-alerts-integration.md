---
title: Integratie met Azure-beveiligings producten Security Center | Microsoft Docs
description: Deze onderwerpen bevatten Azure-beveiligings producten die zijn geïntegreerd met Azure Security Center.
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
ms.author: v-mohabe
ms.openlocfilehash: 64a636cc4452de1ef4a2d0e94629e7d8e3a5878d
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295742"
---
# <a name="security-center-integration-with-azure-security-products-in-asc"></a>Security Center integratie met Azure-beveiligings producten in ASC

Security Center biedt klanten aanvullende micro soft-licenties om hun bevindingen op een geconsolideerde manier op te Security Center en weer te geven.

* [Azure WAF](#azure-waf)
* [Azure DDoS](#azure-ddos)

## Azure WAF <a name="azure-waf"></a>

Azure Application Gateway biedt de functie Web Application Firewall (WAF) voor de gecentraliseerde beveiliging van uw webtoepassingen tegen bekende aanvallen en beveiligingsproblemen.

Webtoepassingen worden steeds gericht op kwaad aardige aanvallen die veelvoorkomende beveiligings problemen misbruiken. De Application Gateway WAF is gebaseerd op de basisrule set (CRS) 3,0 of 2.2.9 van het open Web Application Security-project (OWASP). De WAF wordt automatisch bijgewerkt om te beschermen tegen nieuwe beveiligings problemen, zonder dat er aanvullende configuratie nodig is. Waarschuwingen die door WAF worden gegenereerd, worden gestreamd naar Security Center. Zie dit [artikel](https://docs.microsoft.com/azure/application-gateway/application-gateway-crs-rulegroups-rules?tabs=owasp3#crs911)voor meer informatie over de waarschuwingen die door WAF worden gegenereerd.

## Azure-DDoS<a name="azure-ddos"></a>

DDoS-aanvallen (Distributed Denial of service) zijn bekend om eenvoudig te worden uitgevoerd. Daarom zijn ze een geweldig beveiligings probleem voor klanten die hun toepassingen naar de Cloud verplaatsen. 

Met een DDoS-aanval wordt geprobeerd de resources van een toepassing uit te putten, waardoor de toepassing niet meer beschikbaar is voor legitieme gebruikers. DDoS-aanvallen kunnen worden gericht op elk eind punt dat kan worden bereikt via internet.

Azure DDoS Protection, in combi natie met aanbevolen procedures voor het ontwerpen van toepassingen, biedt een verdediging tegen DDoS-aanvallen. Azure DDoS Protection biedt verschillende service lagen. Zie [Azure DDoS Protection Overview](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)voor meer informatie.

DDoS Protection Standard kan de volgende typen aanvallen beperken:

> [!div class="mx-tableFixed"]

|Waarschuwing|Description|
|---|---|
|**Volumetrische aanval gedetecteerd**|Het doel van deze aanval is om de netwerklaag te laten overstappen op een aanzienlijke hoeveelheid getrouwd verkeer. Dit omvat UDP-flooden, versterking van stromen en andere vervalste pakket stromen. DDoS Protection Standard verkleint deze potentiële multi-Gigabyte-aanvallen door ze te absorberen en te reinigen, met de wereld wijde schaal van Azure automatisch.|
|**Protocol aanval gedetecteerd**|Deze aanvallen genereren een doel dat niet toegankelijk is door een zwakte te misbruiken in de Layer 3-en Layer 4-protocol stack. Dit omvat, SYN-flood-aanvallen, reflectie aanvallen en andere protocol aanvallen. DDoS Protection Standard verkleint deze aanvallen, onderscheidt zich van schadelijk en betrouwbaar verkeer door interactie met de client en het blok keren van schadelijk verkeer.|
|**Aanval van resource (toepassing)-laag gedetecteerd**|Deze aanvallen richten op webtoepassingen om de overdracht van gegevens tussen hosts te verstoren. De aanvallen omvatten schendingen van het HTTP-protocol, SQL-injectie, cross-site scripting en andere Layer 7-aanvallen. Gebruik de Azure-toepassing gateway Web Application Firewall met DDoS Protection Standard om tegen deze aanvallen te beschermen. Er zijn ook Web Application Firewall aanbiedingen van derden beschikbaar in azure Marketplace.|
