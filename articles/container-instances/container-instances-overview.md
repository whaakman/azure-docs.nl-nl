---
title: Wat is Azure Container Instances?
description: De Azure Container Instances-service is de snelste en eenvoudigste methode voor het uitvoeren van geïsoleerde containers in Azure. Hierbij hoeft u geen virtuele machines te beheren of een orchestrator op een hoger niveau te gebruiken.
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: overview
ms.date: 11/30/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 8362ae5e9647c023ff950a363f9ba7bfde37fdb6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57863318"
---
# <a name="what-is-azure-container-instances"></a>Wat is Azure Container Instances?

Containers worden de voorkeursmethode voor het verpakken, implementeren en beheren van cloudtoepassingen. Azure Container Instances is de snelste en meest eenvoudige methode voor het uitvoeren van een container in Azure, waarbij het niet nodig is om virtuele machines te beheren of het serviceniveau te verhogen.

Azure Container Instances is een ideale oplossing voor elk scenario dat kan werken in geïsoleerde containers, met inbegrip van eenvoudige toepassingen, taakautomatisering en het bouwen van taken. Voor scenario's waarin u een volledige indeling met containers nodig hebt, waaronder servicedetectie in meerdere containers, automatisch schalen en gecoördineerde toepassingsupgrades, raden wij [Azure Kubernetes Service (AKS)](../aks/index.yml) aan.

## <a name="fast-startup-times"></a>Snel opstarten

Containers bieden aanzienlijke opstartvoordelen ten opzichte van virtuele machines (VM’s). Met Azure Container Instances kunnen containers in Azure in enkele seconden worden gestart, zonder dat u VM’s hoeft in te richten en te beheren.

## <a name="public-ip-connectivity-and-dns-name"></a>Openbare IP-verbinding en DNS-naam

Met Azure Container Instances kunt u uw containers direct weergeven op internet met een IP-adres en een volledig gekwalificeerde domeinnaam. Wanneer u een containerexemplaar maakt, kunt u een aangepast DNS-naamlabel opgeven, zodat uw toepassing bereikbaar is via *aangepastlabel*.*azureregio*.azurecontainer.io.

## <a name="hypervisor-level-security"></a>Beveiliging op hypervisorniveau

In het verleden boden containers toepassingsafhankelijke isolatie en resourcebesturing, maar werden ze niet voldoende veilig beschouwd voor onveilig multitenant gebruik. Met Azure Container Instances is uw toepassing gegarandeerd even geïsoleerd in een container als deze op een VM zou zijn.

## <a name="custom-sizes"></a>Aangepaste grootten

Containers zijn meestal geoptimaliseerd voor het uitvoeren van één toepassing, maar de exacte behoeften van deze toepassingen kunnen aanzienlijk verschillen. Azure Container Instances biedt een optimaal gebruik door exacte specificaties van CPU-kernen en -geheugen toe te staan. U betaalt op basis van wat u nodig hebt en wordt per seconde gefactureerd, zodat u uw uitgaven kunt verfijnen op basis van werkelijke behoefte.

Voor rekenintensieve taken zoals machine learning kunnen met Azure Container Instances Linux-containers worden ingesteld om [GPU-resources](container-instances-gpu.md) (preview) van NVIDIA Tesla te gebruiken.

## <a name="persistent-storage"></a>Permanente opslag

Voor het ophalen en persisteren van statuswaarden met exemplaren van Azure Container Instances, bieden we direct [koppelen van Azure Files-shares](container-instances-mounting-azure-files-volume.md).

## <a name="linux-and-windows-containers"></a>Linux- en Windows-containers

Met Azure Container Instances kunt u Windows- en Linux-containers met dezelfde API plannen. Geef gewoon het type besturingssysteem op wanneer u de [containergroepen](container-instances-container-groups.md) maakt.

Sommige functies zijn momenteel beperkt tot Linux-containers:

* Meerdere containers per containergroep
* Volume koppelen ([Azure Files](container-instances-volume-azure-files.md), [emptyDir](container-instances-volume-emptydir.md), [GitRepo](container-instances-volume-gitrepo.md), [geheim](container-instances-volume-secret.md))
* [Virtueel netwerkimplementatie](container-instances-vnet.md) (preview)
* [GPU-resources](container-instances-gpu.md) (preview)

Azure Container Instances biedt ondersteuning voor Windows Server 2016-installatiekopieën op basis van Long-Term Servicing-kanaal (LTSC)-versies. SAC-releases (Semi-Annual-kanaal) van Windows, zoals 1709 en 1803 worden niet ondersteund.

## <a name="co-scheduled-groups"></a>Samen geplande groepen

Azure Container Instances biedt ondersteuning voor planning van [meerdere containergroepen](container-instances-container-groups.md) die een hostmachine, lokaal netwerk, opslag en levenscyclus delen. Hierdoor kunt u uw hoofdtoepassingscontainer combineren met andere ondersteunende rolcontainers, zoals het in een logboek vastleggen van sidecars.

## <a name="virtual-network-deployment-preview"></a>Virtueel netwerk implementeren (preview)

Met deze functie van Azure Container Instances, momenteel in preview, kunt u [containerinstanties in Azure Virtual Network implementeren](container-instances-vnet.md). Als u containerinstanties in een subnet binnen uw virtuele netwerk implementeert, kunnen ze veilig communiceren met andere resources in het virtuele netwerk, met inbegrip van on-premises resources (via [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) of [ ExpressRoute](../expressroute/expressroute-introduction.md)).

> [!IMPORTANT]
> Van bepaalde functies van Azure Container Instances is een previewversie beschikbaar waarvoor een aantal [beperkingen gelden](container-instances-vnet.md#preview-limitations). Preview-versies worden beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

## <a name="next-steps"></a>Volgende stappen

Probeer met behulp van onze snelstart een container te implementeren in Azure met een enkele opdracht:

> [!div class="nextstepaction"]
> [Snelstart voor Azure Container Instances](container-instances-quickstart.md)

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
