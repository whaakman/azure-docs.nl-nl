---
title: Over Netwerkbewaking in Log Analytics | Microsoft Docs
description: Overzicht van oplossingen, inclusief NPM, voor het beheren van netwerken in cloud, on-premises en hybride omgevingen voor netwerkbeheer.
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: 
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ajaycode
ms.openlocfilehash: 6793bd8d2c561554213e9fe645aab018dba7a925
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="network-monitoring-solutions"></a>Oplossingen voor netwerkbeheer 

Azure biedt een groot aantal oplossingen voor het bewaken van uw netwerk activa. Azure heeft oplossingen en hulpprogramma's om te controleren van de netwerkverbinding, de status van ExpressRoute-circuits en analyseren van netwerkverkeer in de cloud.

## <a name="network-performance-monitor-npm"></a>Netwerk-Prestatiemeter (NPM)

Netwerk Performance Monitor (NPM) is een suite met mogelijkheden, die gericht is op de status van uw netwerk, verbinding met het netwerk voor uw toepassingen bewaken en biedt inzicht in de prestaties van uw netwerk. NPM is cloud-gebaseerde en biedt een hybride netwerk bewakingsoplossing die connectiviteit tussen bewaakt:
 
* Cloud-implementaties en on-premises locaties
* Meerdere datacenters en filialen
* Missie kritieke meerdere lagen toepassingen/micro-services
* Gebruikerslocaties en webtoepassingen (HTTP/HTTPs) 

## <a name="performance-monitor"></a>Prestatiemeter

Prestatiemeter maakt deel uit van de NPM en netwerkbewaking is voor de cloud en hybride on-premises omgevingen. U kunt de netwerkverbinding tussen externe vertakking en veld kantoren, store locaties, datacenters en clouds bewaken. Voordat u uw gebruikers klagen, kunt u netwerkproblemen detecteren. De belangrijkste voordelen zijn:

* Bewaken en netwerklatentie over verschillende subnetten en waarschuwingen instellen
* Bewaken alle paden (inclusief redundante paden) op het netwerk
* Problemen met tijdelijke en punt in tijd netwerken, die moeilijk te repliceren
* Het specifieke segment in het netwerk, die verantwoordelijk is voor de prestaties verslechteren bepalen
* De status van het netwerk, zonder de noodzaak van SNMP

Voor meer informatie raadpleegt u de volgende artikelen:

* [Configureren van een oplossing voor Network Monitor in Log Analytics](../log-analytics/log-analytics-network-performance-monitor.md) 
* [Gebruiksvoorbeelden](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/)
*  Productupdates: [februari 2017](https://blogs.technet.microsoft.com/msoms/2017/02/27/oms-network-performance-monitor-is-now-generally-available/), [augustus 2017](https://blogs.technet.microsoft.com/msoms/2017/08/14/improvements-to-oms-network-performance-monitor/)

## <a name="expressroute-monitor"></a>ExpressRoute-Monitor

NPM voor ExpressRoute biedt uitgebreide ExpressRoute bewakingsmogelijkheden voor persoonlijke peering verbindingen. U kunt E2E connectiviteit en prestaties tussen uw filialen en Azure bewaken via ExpressRoute. De belangrijkste mogelijkheden zijn:

* Automatische detectie van ER circuits die zijn gekoppeld aan uw abonnement
* Detectie van de netwerktopologie van on-premises voor uw cloudtoepassingen
* Capaciteitsplanning, analyse van gebruik
* Bewaking en waarschuwingen op de primaire en secundaire paden
* Vermindering van de verbinding met de VNets detecteren

Raadpleeg voor meer informatie de volgende artikelen:

* [Netwerkprestatiemeter configureren voor ExpressRoute](../expressroute/how-to-npm.md)
* [blogbericht](https://aka.ms/NPMExRmonitorGA)

## <a name="next-steps"></a>Volgende stappen

* [Prestatiemeter netwerk configureren](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor)
* [Netwerkprestatiemeter configureren voor ExpressRoute](../expressroute/how-to-npm.md)