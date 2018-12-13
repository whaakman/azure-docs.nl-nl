---
title: Inleiding tot Azure Service Fabric-netwerk | Microsoft Docs
description: Meer informatie over netwerken, gateways en intelligente verkeersroutering in Service Fabric NET.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/26/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 8d9f5c71d05477094d733fd6b798831b3ff02ce1
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52892362"
---
# <a name="introduction-to-networking-in-service-fabric-mesh-applications"></a>Inleiding tot netwerken in Service Fabric-NET-toepassingen
In dit artikel beschrijft de verschillende soorten load balancers, hoe door gateways in het netwerk verbinding maken met uw toepassingen met andere netwerken en hoe verkeer wordt gerouteerd tussen de services in uw toepassingen.

## <a name="layer-4-vs-layer-7-load-balancers"></a>Laag 4 vs layer 7 load balancers
Taakverdeling kan worden uitgevoerd op verschillende lagen in de [OSI-model](https://en.wikipedia.org/wiki/OSI_model) voor netwerken, vaak in laag 4 (N4) en layer 7 (L7).  Er zijn doorgaans twee typen van load balancers:

- Een N4 load balancer werkt op het netwerk transportlaag behandelt de levering van pakketten met geen aandacht voor de inhoud van de pakketten. Alleen pakketheaders worden gecontroleerd door de load balancer, zodat de taakverdeling criteria is beperkt tot IP-adressen en poorten. Een client maakt bijvoorbeeld een TCP-verbinding met de load balancer. De load balancer de verbinding verbreekt (door rechtstreeks naar de SYN te reageren), selecteert u een back-end en maakt een nieuwe TCP-verbinding met de back-end (er worden een nieuwe SYN verzonden). Een N4 load balancer wordt doorgaans werkt alleen op het niveau van het N4 TCP/UDP-verbinding of de sessie. Dus de taakverdeling wordt omgeleid bytes rond en zorgt ervoor dat de bytes van dezelfde sessie afsluiting van op de dezelfde back-end. De N4 load balancer is geen informatie over de details van elke toepassing van het aantal bytes dat wordt verplaatst. Het aantal bytes kunnen elk toepassingsprotocol voor de zijn.

- Een L7-taakverdeling werkt op het niveau van de toepassing, die met de inhoud van elk pakket omgaat. Pakketinhoud inspecteert deze omdat wordt begrepen protocollen zoals HTTP, HTTPS en WebSockets. Dit biedt de mogelijkheid om uit te voeren geavanceerde routering de load balancer. Een client maakt bijvoorbeeld een enkele HTTP/2-TCP-verbinding met de load balancer. De load balancer gaat deze vervolgens door twee back-end-verbindingen te maken. Wanneer de client twee HTTP/2-stromen aan de load balancer verzendt, stream een wordt verzonden naar back-end een en stream twee wordt verzonden naar back-end twee. Dus zal zelfs multiplexing clients die aanzienlijk verschillende aanvraag belastingen hebben worden verdeeld efficiënt over de back-ends. 

## <a name="networks-and-gateways"></a>Netwerken en gateways
In de [Service Fabric-Resourcemodel](service-fabric-mesh-service-fabric-resources.md), een netwerkbron is een afzonderlijk implementeerbare resource, onafhankelijk van een toepassing of Service-resource die als hun afhankelijkheid naar deze verwijzen kan. Het wordt gebruikt voor het maken van een netwerk voor uw toepassingen die is geopend op het internet. Meerdere services van verschillende toepassingen kunnen een deel uitmaken van hetzelfde netwerk. Deze particulier netwerk is gemaakt en beheerd door Service Fabric en is niet een Azure-netwerk (VNET). Toepassingen kunnen dynamisch worden toegevoegd en verwijderd uit de netwerkbron-en uitschakelen van VNET-connectiviteit. 

Een gateway wordt gebruikt om twee netwerken overbruggen. De resource van de Gateway implementeert een [Envoy proxy](https://www.envoyproxy.io/) waarmee de N4 voor elk protocol voor Routering en L7 routering voor geavanceerde HTTP (S)-toepassingsroutering. De gateway routeert verkeer in uw netwerk vanuit een extern netwerk en bepaalt welke service verkeer naar routeert.  Het externe netwerk mogelijk op een open netwerk (in feite het openbare internet) of een Azure-netwerk, zodat u kunt verbinding maken met uw andere Azure-toepassingen en resources. 

![Netwerk- en gateway][Image1]

Wanneer de netwerk-resource is gemaakt met `ingressConfig`, een openbaar IP-adres is toegewezen aan de netwerkbron. Het openbare IP-adres worden gekoppeld aan de levensduur van de netwerk-resource.

Wanneer een Mesh-toepassing wordt gemaakt, moet deze verwijzen naar een bestaande netwerk-resource. Nieuwe openbare poorten kunnen worden toegevoegd of bestaande poorten kunnen worden verwijderd uit de configuratie van de inkomende gegevens. Een verwijderd voor een netwerkbron mislukt als de bron van een toepassing verwijst naar deze. Wanneer de toepassing wordt verwijderd, worden de netwerk-resource wordt verwijderd.

## <a name="next-steps"></a>Volgende stappen 
Lees het overzicht voor meer informatie over Service Fabric NET:
- [Service Fabric-NET-overzicht](service-fabric-mesh-overview.md)

[Image1]: media/service-fabric-mesh-networks-and-gateways/NetworkAndGateway.png