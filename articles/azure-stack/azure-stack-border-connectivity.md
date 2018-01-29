---
title: "Aandachtspunten voor de integratie van connectiviteit netwerk voor Azure-Stack geïntegreerd systemen rand | Microsoft Docs"
description: Meer informatie over wat u kunt doen om te plannen voor datacenter rand netwerkverbinding met meerdere knooppunten Azure Stack.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: c1a5496f3ab9a625d7d97c3096ae89100b7c5592
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="border-connectivity"></a>Border-connectiviteit 
Integratie met netwerkplanning is een belangrijk vereist voor een geslaagde implementatie van Azure-Stack geïntegreerd systemen, bewerking en beheer. Rand connectiviteit planning begint door te kiezen of u gebruik van dynamische routering met border gateway protocol (BGP) of niet. Hiervoor moet een 16-bits autonoom systeemnummer BGP (openbare of persoonlijke) toe te wijzen of met statische routering, waarbij een standaard statische route is toegewezen aan de rand-apparaten.

> [!IMPORTANT]
> De bovenkant van de switches (TOR) rack vereisen Layer 3-uplinks met Point-to-Point IP-adressen (/ 30 netwerken) geconfigureerd op de fysieke interfaces. Dit wordt niet ondersteund voor het gebruik van Layer 2-uplinks met ondersteuning van Azure-Stack operations TOR-switches. 

## <a name="bgp-routing"></a>BGP-routering
Met behulp van een dynamisch routeringsprotocol zoals BGP zorgt ervoor dat uw systeem altijd op de hoogte van wijzigingen in het netwerk is en vereenvoudigt het beheer. 

Zoals u in het volgende diagram, is adverteren van het particuliere IP-adres ruimte op de TOR-switch beperkt met een voorvoegsel-lijst. De lijsten voorvoegsel weigert de privé IP-subnetten en deze toe te passen als een route-kaart op de verbinding tussen de TOR en de rand.

De Software Load Balancer (SLB) uitgevoerd binnen de Azure-Stack oplossing peers op de TOR-apparaten zodat deze de VIP-adressen dynamisch kunt adverteren.

Om ervoor te zorgen dat gebruikersverkeer onmiddellijk en transparant vanuit fout herstelt, wordt de VPC of MLAG geconfigureerd tussen de TOR-apparaten kunt het gebruik van meerdere chassis koppeling netwerk aggregatie op de hosts en HSRP of VRRP die biedt redundantie voor de IP-netwerken.

![BGP-routering](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>statische routering
Gebruik van statische routes, voegt meer vaste configuratie toe aan de rand en TOR-apparaten. Grondige analyse te maken voordat u wijzigingen vereist. Problemen veroorzaakt door een configuratiefout kunnen meer tijd vergen terug te draaien, afhankelijk van de wijzigingen zijn aangebracht. Het is niet de aanbevolen methode om routering, maar wordt ondersteund.

Als u wilt integreren Azure-Stack in uw netwerkomgeving met deze methode, moet het apparaat van rand worden geconfigureerd met statische routes die verwijst naar de TOR-apparaten voor verkeer dat is bestemd voor een extern netwerk, openbare VIP's.

De TOR-apparaten moeten worden geconfigureerd met een statische standaardroute alle verkeer verzenden naar de apparaten van de rand. De enige uitzondering verkeer op deze regel is voor de persoonlijke ruimte die wordt geblokkeerd met behulp van een toegangsbeheerlijst toegepast op de TOR op rand verbinding.

Alle andere moet hetzelfde zijn als de eerste methode. BGP dynamische routering wordt nog steeds worden gebruikt binnen het rack omdat het van essentieel voor de SLB en andere onderdelen en kan niet worden uitgeschakeld of verwijderd.

![statische routering](media/azure-stack-border-connectivity/static-routing.png)

## <a name="transparent-proxy"></a>Transparentproxy
Als uw datacenter nodig al het verkeer naar een proxy gebruikt, moet u een *transparentproxy* voor het verwerken van al het verkeer van het rack verwerkt; het volgens het beleid, scheiden van verkeer tussen de zones in uw netwerk.

> [!IMPORTANT]
> De Azure-Stack-oplossing biedt geen ondersteuning voor normale webproxy.  

Een transparentproxy (ook wel bekend als een onderschept, inline of geforceerde proxy) onderschept normale communicatie op de netwerklaag zonder speciale clientconfiguratie. Clients moeten niet op de hoogte van de aanwezigheid van de proxy.

![Transparentproxy](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>Volgende stappen
[DNS-integratie](azure-stack-integrate-dns.md)