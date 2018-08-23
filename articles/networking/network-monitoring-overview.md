---
title: Over de Netwerkbewaking van Log Analytics | Microsoft Docs
description: Overzicht van oplossingen, met inbegrip van NPM, voor het beheren van netwerken in cloud, on-premises en hybride omgevingen voor netwerkbeheer.
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ajaycode
ms.openlocfilehash: 0656cfcc2dcded284be1a337f797681117f3b313
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42055333"
---
# <a name="network-monitoring-solutions"></a>Oplossingen voor netwerkbeheer 

Azure biedt een groot aantal oplossingen voor het controleren van de activa van uw netwerk. Azure heeft oplossingen en hulpprogramma's voor verbinding met het netwerk, de status van ExpressRoute-circuits, controleren en analyseren van netwerkverkeer in de cloud.

## <a name="network-performance-monitor-npm"></a>Netwerkprestatiemeter (NPM)

(Netwerkprestatiemeter) is een reeks mogelijkheden, die is afgestemd op de bewaking van de status van uw netwerk, verbinding met het netwerk aan uw toepassingen, en biedt inzicht in de prestaties van uw netwerk. NPM is cloud-gebaseerde en biedt een hybride Netwerkcontrole-oplossing die connectiviteit tussen bewaakt:
 
* Cloud-implementaties en on-premises locaties
* Meerdere datacenters en filialen
* Missiekritieke kritieke toepassingen/micro-services met meerdere lagen
* Gebruikerslocaties en webtoepassingen (HTTP/HTTPs) 

Prestatiemeter, ExpressRoute-bewaking en Connectiviteitsmonitor Service mogelijkheden van NPM bewaakt en worden hieronder beschreven.

## <a name="performance-monitor"></a>Prestatiemeter

Prestatiemeter maakt deel uit van NPM en netwerkbewaking is voor de cloud, hybride en on-premises omgevingen. U kunt de netwerkconnectiviteit controleren voor externe filialen en bijkantoren, WINKELLOCATIES, datacentrums en clouds. U kunt netwerkproblemen detecteren voordat uw gebruikers klagen. De belangrijkste voordelen zijn:

* Verlies en latentie bewaken in verschillende subnetten en waarschuwingen instellen
* Alle paden (inclusief redundante paden) op het netwerk controleren
* Problemen met tijdelijke en point-in-time-netwerk, die moeilijk te repliceren
* Het specifieke segment van het netwerk, die verantwoordelijk is voor de verslechterde prestaties bepalen
* De status van het netwerk bewaken, zonder dat een SNMP nodig is

![NPM topologiekaart weer](./media/network-monitoring-overview/npm-topology-map.png) 

Voor meer informatie raadpleegt u de volgende artikelen:

* [Een-oplossing Netwerkprestatiemeter configureren in Log Analytics](../log-analytics/log-analytics-network-performance-monitor.md) 
* [Use-cases](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/)
*  Productupdates: [februari 2017](https://blogs.technet.microsoft.com/msoms/2017/02/27/oms-network-performance-monitor-is-now-generally-available/), [augustus 2017](https://blogs.technet.microsoft.com/msoms/2017/08/14/improvements-to-oms-network-performance-monitor/)

## <a name="expressroute-monitor"></a>ExpressRoute-bewaking

NPM voor ExpressRoute biedt uitgebreide ExpressRoute-bewaking voor persoonlijke Azure-peering en Microsoft-peeringverbindingen. U kunt E2E-connectiviteit en -prestaties tussen uw filialen en Azure bewaken via ExpressRoute. De belangrijkste mogelijkheden zijn:

* Automatische detectie van ER circuits die zijn gekoppeld aan uw abonnement
* Detectie van de netwerktopologie van on-premises voor uw cloudtoepassingen
* Capaciteitsplanning, analyse van Resourcegebruik bandbreedte
* Bewaking en waarschuwingen voor de primaire en secundaire paden
* Verbinding met Azure Monitoring services zoals Office 365, Dynamics 365, via ExpressRoute
* Vermindering van de verbinding met vnet's detecteren

![Geografische kaart weergegeven verkeer tussen regio 's](./media/network-monitoring-overview/expressroute-topology-map.png) 

Raadpleeg voor meer informatie de volgende artikelen:

* [Netwerkprestatiemeter configureren voor ExpressRoute](../expressroute/how-to-npm.md)
* [Blogbericht](https://aka.ms/NPMExRmonitorGA)

## <a name="service-connectivity-monitor"></a>Bewaking van serviceverbinding

Met Service-connectiviteit controleren, kunt u nu testen bereikbaarheid van toepassingen en knelpunten detecteren via on-premises, provider-netwerken en cloud/persoonlijk datacenters.

* Controleer de netwerkconnectiviteit voor end-to-end voor toepassingen
* Levering van toepassingen correleren met prestaties van het netwerk, precieze locatie van de degradatie langs het pad tussen de gebruiker en de toepassing te detecteren
* Test bereikbaarheid van de toepassing vanaf verschillende gebruikerslocaties over de hele wereld
* Bepalen van de latentie en pakket wegvallend netwerk voor uw line-of-business en SaaS-toepassingen
* Hotspots in het netwerk, die kan worden veroorzaakt door de prestaties slecht presteert bepalen
* Bereikbaarheid voor Office 365-toepassingen, met behulp van ingebouwde tests voor Microsoft Office 365, Dynamics 365, Skype voor bedrijven en andere Microsoft-services bewaken

Raadpleeg voor meer informatie de volgende artikelen:

* [Network Performance Monitor configureren voor het bewaken van Service-eindpunten](https://aka.ms/applicationconnectivitymonitorguide)
* [Blogbericht](https://aka.ms/svcendptmonitor)

## <a name="traffic-analytics"></a>Traffic Analytics
Traffic Analytics is een cloud-gebaseerde oplossing waarmee u inzicht in gebruikers en toepassingen in uw cloudnetwerken. Stroomlogboeken logboeken zijn geanalyseerd om te bieden van inzicht in:

* Verkeer stroomt tussen uw netwerken tussen Azure en Internet, openbare cloud-regio's, VNETs en subnetten
* Toepassingen en -protocollen op uw netwerk, zonder de noodzaak voor sniffers of toegewezen stroom collector apparaten
* Belangrijke gebruikers, intensieve toepassingen, VM-gesprekken in de cloud, verkeer hotspots
* Bronnen en bestemmingen van verkeer in vnet's, tussen relaties tussen kritische zakelijke services en toepassingen
* Beveiliging – schadelijk verkeer, poorten openen op het Internet, toepassingen of virtuele machines toegang tot het Internet proberen...
* Capaciteitsverbruik - helpt u bij het voorkomen van problemen met capaciteit in te richten of ondermaats gebruik door de bewaking van trends in het gebruik van VPN-gateways en andere services

Traffic Analytics voorziet u van bruikbare informatie die helpt u activiteit op het netwerk, veilige toepassingen en gegevens van uw organisatie controleren werkbelasting prestaties te optimaliseren en blijven voldoen.

![Geografische kaart weergegeven verkeer tussen regio 's](../network-watcher/media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png) 

Verwante koppelingen:
* [Blogbericht](https://aka.ms/trafficanalytics), [documentatie](https://aka.ms/trafficanalyticsdocs), [Veelgestelde vragen](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-faq)

## <a name="dns-analytics"></a>DNS-analyse
Gebouwd voor DNS-beheerders, deze oplossing verzamelt, analyseert en correleert DNS-logboeken voor beveiliging, bewerkingen en inzichten met betrekking tot prestaties.  Enkele van de mogelijkheden zijn:

* Identificatie van clients die proberen te worden omgezet naar schadelijke domeinen
* Identificatie van verlopen bronrecords
* Inzicht in vaak opgevraagde domeinnamen en dremepelwaarde DNS-clients
* Inzicht in de aanvraagbelasting van DNS-servers
* Bewaking van dynamische DNS-registratiefouten

![DNS Analytics-Dashboard](./media/network-monitoring-overview/dns-analytics-overview.png) 

Verwante koppelingen:
* [Blogbericht](https://blogs.technet.microsoft.com/msoms/2017/04/19/introducing-oms-dns-analytics/), [documentatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-dns)

## <a name="miscellaneous"></a>Diversen

* [Nieuwe prijzen](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor-pricing-faq)
