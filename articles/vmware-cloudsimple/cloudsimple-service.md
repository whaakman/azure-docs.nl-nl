---
title: VMware-oplossingen door CloudSimple - Service
description: Hierin wordt beschreven in het overzicht van CloudSimple service en concepten.
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f7b4be0ff3997e27dd5b5321dd44b5006ae52102
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358106"
---
# <a name="cloudsimple-service-overview"></a>CloudSimple Service-overzicht

De service CloudSimple kunt u Azure VMware-oplossing door CloudSimple gebruiken.  Het maken van de service, kunt u knooppunten kopen, knooppunten reserveren en Privéclouds te maken.  U maakt de CloudSimple-service in elke Azure-regio waar de service CloudSimple beschikbaar is.  De service definieert het edge-netwerk van Azure VMware-oplossing door CloudSimple.  Het edge-netwerk biedt ondersteuning voor services, zoals VPN, ExpressRoute en internetverbinding met uw Privéclouds.

## <a name="gateway-subnet"></a>Gatewaysubnet

Een gateway-subnet is vereist volgens CloudSimple service en is uniek is voor de regio waarin deze gemaakt. Het gatewaysubnet wordt gebruikt bij het maken van het edge-netwerk en vereist een/28 CIDR-blok.  De adresruimte van de gateway-subnet moet uniek zijn. Deze mag niet overlappen met een netwerk die met de CloudSimple-omgeving communiceert. De netwerken die met CloudSimple communiceren zijn on-premises netwerken en virtuele Azure-netwerk.  Een gateway-subnet kan niet worden verwijderd zodra deze gemaakt.  Het gatewaysubnet wordt verwijderd wanneer de service is verwijderd.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [een CloudSimple-service in Azure maken](quickstart-create-cloudsimple-service.md)