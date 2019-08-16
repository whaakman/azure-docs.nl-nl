---
title: Virtueel Azure-netwerk verbinden met CloudSimple met behulp van ExpressRoute
description: Hierin wordt beschreven hoe u peering-informatie kunt verkrijgen voor een verbinding tussen het virtuele Azure-netwerk en uw CloudSimple-omgeving
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8ba2d2fd9c943fe55e82956d022f6ba9840a550f
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69536597"
---
# <a name="connect-azure-virtual-network-to-cloudsimple-using-expressroute"></a>Virtueel Azure-netwerk verbinden met CloudSimple met behulp van ExpressRoute

U kunt uw particuliere cloud netwerk uitbreiden naar uw virtuele Azure-netwerk en Azure-resources. Met een ExpressRoute-verbinding kunt u vanuit uw Privécloud toegang krijgen tot resources die worden uitgevoerd in uw Azure-abonnement.

## <a name="request-authorization-key"></a>Aanvraag autorisatie sleutel

Een autorisatie sleutel is vereist voor de ExpressRoute-verbinding tussen uw Privécloud en het virtuele Azure-netwerk. Als u een sleutel wilt verkrijgen, moet u een ticket indienen met <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">ondersteuning</a>.  Gebruik de volgende informatie in de aanvraag:

* Type probleem: **Documentatie**
* Abonnement: Selecteer het abonnement waarin de CloudSimple-service is geïmplementeerd * *
* Service: **VMware-oplossing per CloudSimple**
* Probleem type: **Service aanvraag**
* Subtype van probleem: **Autorisatie sleutel voor Azure VNET-verbinding**
* Houder: **Aanvraag voor autorisatie sleutel voor Azure VNET-verbinding**

## <a name="obtain-peering-information-for-azure-virtual-network-to-cloudsimple-connection"></a>Informatie over peering voor Azure Virtual Network verkrijgen voor CloudSimple-verbinding

Als u de verbinding wilt instellen, moet u een koppeling tot stand brengen tussen het virtuele netwerk van Azure en uw CloudSimple-omgeving.  Als onderdeel van de procedure moet u de URI en autorisatie sleutel van het peer circuit opgeven. Haal de URI en autorisatie sleutel op uit de [CloudSimple-Portal](access-cloudsimple-portal.md).  Selecteer **netwerk** in het menu aan de zijkant en selecteer vervolgens **Azure-netwerk verbinding**. Of selecteer **account** in het menu aan de zijkant en selecteer vervolgens **Azure-netwerk verbinding**.

Let op de Kopieer pictogrammen voor de URI van het peer circuit en voor de autorisatie sleutel voor elk van de regio's. Voor elke Privécloud die u wilt verbinden:

* Klik op **kopiëren** om de URI te kopiëren. Plak deze in een bestand waar het kan worden toegevoegd aan de Azure Portal.  
* Klik op **kopiëren** om de autorisatie sleutel te kopiëren en deze ook in het bestand te plakken.

![Virtual Network verbindings pagina](media/network-virt-conn-page.png)

Zie [uw CloudSimple Private Cloud Environment verbinden met het virtuele Azure-netwerk met behulp van ExpressRoute](azure-expressroute-connection.md)voor meer informatie over het instellen van het virtuele netwerk van Azure op CloudSimple.
