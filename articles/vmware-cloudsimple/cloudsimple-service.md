---
title: VMware-oplossing per CloudSimple-service
description: Hierin wordt het overzicht van de CloudSimple-service en-concepten beschreven.
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a43fbebb21be82fd751778d6fec95e0ee9c346ad
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812506"
---
# <a name="cloudsimple-service-overview"></a>Overzicht van de CloudSimple-service

Met de CloudSimple-service kunt u Azure VMware-oplossing gebruiken door CloudSimple. Nadat u de service hebt gemaakt, kunt u knoop punten inrichten, knoop punten reserveren en persoonlijke clouds maken. U maakt de CloudSimple-service in elke Azure-regio waar de CloudSimple-service beschikbaar is. De service definieert het Edge-netwerk van de Azure VMware-oplossing door CloudSimple. Het Edge-netwerk ondersteunt services die VPN, Azure ExpressRoute en Internet connectiviteit bevatten voor uw persoonlijke Clouds.

## <a name="gateway-subnet"></a>Gatewaysubnet

Een gateway-subnet is vereist per CloudSimple-service en is uniek voor de regio waarin het is gemaakt. Het gateway-subnet wordt gebruikt wanneer u het Edge-netwerk maakt en vereist een/28 CIDR-blok. De adres ruimte van het gateway-subnet moet uniek zijn. Het mag niet overlappen met een netwerk dat communiceert met de CloudSimple-omgeving. De netwerken die met CloudSimple communiceren, zijn onder andere on-premises netwerken en Azure Virtual Networks. Een gateway-subnet kan niet worden verwijderd nadat het is gemaakt. Het gateway-subnet wordt verwijderd wanneer de service wordt verwijderd.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [maken van een CloudSimple-service in azure](quickstart-create-cloudsimple-service.md).
