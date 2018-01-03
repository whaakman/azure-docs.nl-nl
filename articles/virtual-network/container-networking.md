---
title: Virtuele Azure-netwerk voor containers | Microsoft Docs
description: Meer informatie over de invoegtoepassing voor Kubernetes clusters, CNI waarmee containers om te communiceren met elkaar en andere resources, in een virtueel netwerk.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: jdial
ms.openlocfilehash: f70afa8ff69b6c79363313c0f2df3b6785da8d81
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="container-networking"></a>Container netwerken

Azure biedt een [Container Networking Interface (CNI) invoegtoepassing](https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md) waarmee u te implementeren en beheren van uw eigen Kubernetes-cluster met systeemeigen Azure VPN-functionaliteit. De invoegtoepassing is ingeschakeld, standaard, wanneer het implementeren van Kubernetes clusters met de [Azure Container Service-Engine](https://github.com/Azure/acs-engine) (of de ACS-engine).

## <a name="networking-capabilities"></a>Mogelijkheden van netwerken

Containers kunnen gebruikmaken van de uitgebreide set met mogelijkheden waarmee een virtueel netwerk, zoals biedt:
-   U kunt een afzonderlijke virtueel netwerk voor uw cluster maken of implementeren van uw cluster in een bestaand virtueel netwerk. 
-   Elke schil in het cluster ontvangt van een IP-adres uit in het virtuele netwerk en kan rechtstreeks communiceren met andere gehele product in het cluster en een virtuele machine in het virtuele netwerk. 
-   Een schil kan verbinding maken voor andere gehele product en virtuele machines in virtuele netwerken brengen en om on-premises netwerken via ExpressRoute en site-naar-site VPN-verbindingen. Lokale bronnen kunnen communiceren met de gehele product. 
-   U kunt een Kubernetes-service blootstelt aan Internet via de Azure Load Balancer.  
-   Gehele product in een subnet met de service-eindpunten ingeschakeld veilig verbinding kunnen maken naar Azure-services (opslag en SQL-Database, bijvoorbeeld).
-   U kunt de gebruiker gedefinieerde routes gebruiken voor het routeren van verkeer van het gehele product met een virtueel netwerkapparaat. 
-   Gehele product hebben toegang tot openbare bronnen op het Internet.
-   U kunt een schil een openbaar IP-adres, gekoppeld aan een DNS-naam worden kan toewijzen.
 
## <a name="limits"></a>Limieten
U kunt maximaal 4.000 knooppunten in een cluster Kubernetes en maximaal 250 gehele product per knooppunt met een algemene grens van 16.000 gehele product per cluster implementeren wanneer u de invoegtoepassing.

## <a name="constraints"></a>Beperkingen
- De invoegtoepassing is niet ingeschakeld bij het implementeren van een cluster Kubernetes met de [Azure Container Service (AKS)](../aks/intro-kubernetes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [ACS](../container-service/kubernetes/container-service-intro-kubernetes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cluster met Kubernetes.
- Er is geen systeemeigen ondersteuning voor Kubernetes netwerkbeleid, met inbegrip van beleid voor DNS- of toegang.
- De invoegtoepassing biedt geen ondersteuning voor het netwerkbeleid per schil.

## <a name="pricing"></a>Prijzen
Er zijn geen kosten voor het gebruik van de invoegtoepassing CNI.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [implementeren van een cluster Kubernetes](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/deploy.md) in uw [eigen virtueel netwerk](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/features.md#using-azure-integrated-networking-cni).
