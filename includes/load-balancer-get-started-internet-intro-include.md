---
author: kumudD
ms.service: load-balancer
ms.topic: include
ms.date: 11/09/2018
ms.author: kumud
ms.openlocfilehash: 459c99d1b45af9c98cc1a6d0d7dd7a9a04c824ec
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66122103"
---
Azure Load Balancer is een Layer-4 (TCP, UDP) load balancer. De load balancer biedt hoge beschikbaarheid bij het verdelen van inkomend verkeer over gezonde service-exemplaren in cloudservices of virtuele machines in een load balancer-set. Azure Load Balancer kan deze services ook toepassen op meerdere poorten, meerdere IP-adressen of allebei.

U kunt een load balancer configureren voor:

* Takenverdeling voor inkomend internetverkeer op virtuele machines (VM's). In dit scenario verwijzen we naar een load balancer als een [internetgerichte load balancer](../articles/load-balancer/load-balancer-internet-overview.md).
* Taakverdeling voor verkeer tussen VM's in een virtueel netwerk (VNet), tussen VM's in cloudservices of tussen on-premises computers en VM's in een cross-premises virtueel netwerk. In dit scenario verwijzen we naar een load balancer als een [interne load balancer (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Doorsturen van extern verkeer naar een specifiek VM-exemplaar.
