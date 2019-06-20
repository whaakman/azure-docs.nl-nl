---
title: VMware-oplossing door CloudSimple-service
description: Hierin wordt beschreven in het overzicht van CloudSimple service en concepten.
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6a4c0bc6070d372a279b74f81ac1f84f565559c3
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165245"
---
# <a name="cloudsimple-service-overview"></a>Overzicht van de service CloudSimple

U kunt met de service CloudSimple Azure VMware-oplossing door CloudSimple gebruiken. Nadat u de service hebt gemaakt, kunt u knooppunten in te richten, knooppunten reserveren en persoonlijke clouds te maken. U maakt de CloudSimple-service in elke Azure-regio waar de service CloudSimple beschikbaar is. De service definieert het edge-netwerk van Azure VMware-oplossing door CloudSimple. Het edge-netwerk biedt ondersteuning voor services, zoals VPN, Azure ExpressRoute en internetverbinding met uw persoonlijke clouds.

## <a name="gateway-subnet"></a>Gatewaysubnet

Een gateway-subnet is vereist volgens CloudSimple service en is uniek is voor de regio waarin deze gemaakt. Het gatewaysubnet wordt gebruikt bij het maken van het edge-netwerk en vereist een/28 CIDR-blok. De adresruimte van de gateway-subnet moet uniek zijn. Deze mag niet overlappen met een netwerk die met de CloudSimple-omgeving communiceert. De netwerken die met CloudSimple communiceren zijn on-premises netwerken en virtuele netwerken van Azure. Een gateway-subnet kan niet worden verwijderd nadat deze gemaakt. Het gatewaysubnet wordt verwijderd wanneer de service is verwijderd.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [een CloudSimple-service in Azure maken](quickstart-create-cloudsimple-service.md).
