---
title: Overzicht van een Oracle-noodherstelscenario in uw Azure-omgeving | Microsoft Docs
description: Een noodherstelscenario voor een Oracle-Database 12c-database in uw Azure-omgeving
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/2/2017
ms.author: rclaus
ms.openlocfilehash: f17ebb2b74cd7ad872f88483ed7cdb4f239ee069
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a>Herstel na noodgevallen voor een Oracle-Database 12c-database in een Azure-omgeving

## <a name="assumptions"></a>Veronderstellingen

- U hebt een goed begrip van Oracle Data Guard ontwerp en de Azure-omgevingen.


## <a name="goals"></a>Doelstellingen
- Ontwerp van de topologie en de configuratie die voldoen aan de vereisten van uw disaster recovery (DR).

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a>Scenario 1: Primaire en DR-sites op Azure

Een klant heeft een Oracle-database set up op de primaire site. Er is een DR-site in een andere regio. De klant maakt gebruik van Oracle Data Guard voor snel herstel tussen deze sites. De primaire site heeft ook een secundaire database voor rapportage en andere toepassingen. 

### <a name="topology"></a>Topologie

Hier volgt een samenvatting van de Azure-instellingen:

- Twee sites (een primaire site en een DR-site)
- Twee virtuele netwerken
- Twee Oracle-databases met Data Guard (primaire en stand-by)
- Twee Oracle-databases met Golden Gate of Data Guard (alleen de primaire site)
- Twee toepassingsservices: een primaire en een op de DR-site
- Een *beschikbaarheidsset* die wordt gebruikt voor de database en de toepassing service op de primaire site
- Een jumpbox op elke site die de toegang beperkt tot het particuliere netwerk en kunt alleen aanmelden door een beheerder
- Een jumpbox, de toepassingsservice, de database en de VPN-gateway op afzonderlijke subnetten
- NSG afgedwongen voor de toepassing en database subnetten

![Schermafbeelding van de pagina DR-topologie](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a>Scenario 2: De primaire site on-premises en DR-site op Azure

Een klant heeft een lokale installatie voor Oracle-database (primaire site). Er is een DR-site op Azure. Oracle Data Guard wordt gebruikt voor snel herstel tussen deze sites. De primaire site heeft ook een secundaire database voor rapportage en andere toepassingen. 

Er zijn twee manieren om deze installatie.

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-the-firewall"></a>Methode 1: Rechtstreekse verbindingen tussen on-premises en Azure, open TCP-poorten op de firewall vereisen 

Directe verbindingen wordt niet aanbevolen omdat ze de TCP-poorten voor de buitenwereld openbaren.

#### <a name="topology"></a>Topologie

Hier volgt een samenvatting van de installatie van de Azure:

- Een DR-site 
- Een virtueel netwerk
- Een Oracle-database met Data Guard (actief)
- De service één toepassing op de DR-site
- Een jumpbox, waardoor de toegang beperkt tot het particuliere netwerk en kunt alleen aanmelden door een beheerder
- Een jumpbox, de toepassingsservice, de database en de VPN-gateway op afzonderlijke subnetten
- NSG afgedwongen voor de toepassing en database subnetten
- Een NSG/beleidsregel waarmee binnenkomende TCP-poort 1521 (of een door de gebruiker gedefinieerde poort)
- Een NSG/beleidsregel te beperken van het IP-adres adres adressen lokale (DB of toepassing) toegang tot het virtuele netwerk

![Schermafbeelding van de pagina DR-topologie](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a>Methode 2: Site-naar-site VPN
Site-naar-site VPN is een betere benadering. Zie voor meer informatie over het instellen van een VPN [een virtueel netwerk maken met een Site-naar-Site VPN-verbinding met CLI](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli).

#### <a name="topology"></a>Topologie

Hier volgt een samenvatting van de installatie van de Azure:

- Een DR-site 
- Een virtueel netwerk 
- Een Oracle-database met Data Guard (actief)
- De service één toepassing op de DR-site
- Een jumpbox, waardoor de toegang beperkt tot het particuliere netwerk en kunt alleen aanmelden door een beheerder
- Een jumpbox, de toepassingsservice, de database en de VPN-gateway worden op afzonderlijke subnetten
- NSG afgedwongen voor de toepassing en database subnetten
- Site-naar-site VPN-verbinding tussen on-premises en Azure

![Schermafbeelding van de pagina DR-topologie](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a>Aanvullende bronnen

- [Ontwerpen en implementeren van een Oracle-database in Azure](oracle-design.md)
- [Oracle Data Guard configureren](configure-oracle-dataguard.md)
- [Oracle Golden Gate configureren](configure-oracle-golden-gate.md)
- [Oracle back-up en herstel](oracle-backup-recovery.md)


## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Een maximaal beschikbare virtuele machines maken](../../linux/create-cli-complete.md)
- [VM-implementatie Azure CLI voorbeelden verkennen](../../linux/cli-samples.md)
