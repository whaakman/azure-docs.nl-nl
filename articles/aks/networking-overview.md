---
title: Netwerkconfiguratie in Azure Kubernetes Service (AKS)
description: Meer informatie over de basiseigenschappen en geavanceerde netwerkconfiguratie in Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/07/2018
ms.author: marsma
ms.openlocfilehash: 818bae2e05f6a3256ccbf0cbcc901dd337b9a260
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Netwerkconfiguratie in Azure Kubernetes Service (AKS)

Wanneer u een Azure Kubernetes Service (AKS)-cluster maakt, kunt u selecteren uit twee netwerkopties: **Basic** of **Geavanceerd**.

## <a name="basic-networking"></a>Netwerkgebruik

De **Basic** networking optie is de standaardconfiguratie voor het maken van het cluster AKS. De netwerkconfiguratie van het cluster en het gehele product volledig door Azure worden beheerd en geschikt is voor implementaties die niet te worden aangepast VNet-configuratie hoeven. U hoeft geen controle over de netwerkconfiguratie, zoals subnetten of het IP-adresbereiken die zijn toegewezen aan het cluster wanneer u basisnetwerken selecteert.

Knooppunten in een cluster AKS is geconfigureerd voor eenvoudige netwerken gebruik de [kubenet] [ kubenet] Kubernetes-invoegtoepassing.

## <a name="advanced-networking"></a>Geavanceerde netwerken

**Geavanceerde** netwerken plaatst het gehele product in een Azure-netwerk (VNet) die u configureert, meeleveren automatische verbinding met de VNet-resources en integratie met de uitgebreide set mogelijkheden die aanbieding VNets.
Geavanceerde netwerken is momenteel beschikbaar alleen wanneer het implementeren van AKS-clusters in de [Azure-portal] [ portal] of met Resource Manager-sjabloon.

Knooppunten in een cluster AKS is geconfigureerd voor gebruik van geavanceerde netwerken de [Azure Container netwerken Interface (CNI)] [ cni-networking] Kubernetes-invoegtoepassing.

![Diagram van twee knooppunten met bruggen verbinding maken met elk op één Azure-VNet][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Geavanceerde functies voor netwerkbeheer

Geavanceerde netwerkvoorzieningen bieden de volgende voordelen:

* Uw cluster AKS in een bestaand VNet implementeren of maak een nieuw VNet en een subnet voor uw cluster.
* Elke schil in het cluster een IP-adres in het VNet is toegewezen en rechtstreeks kan communiceren met andere gehele product in het cluster en andere virtuele machines in het VNet.
* Een schil kan verbinding maken met andere services in een peered VNet en on-premises netwerken via ExpressRoute en site-naar-site (S2S) VPN-verbindingen. Er zijn ook gehele product bereikbaar van on-premises.
* Tonen een service Kubernetes intern of extern via de Azure Load Balancer. Ook een functie van basisnetwerken.
* Gehele product in een subnet die ingeschakeld voor service-eindpunten zijn kunnen veilig verbinding maken met Azure-services, bijvoorbeeld Azure Storage en SQL-database.
* Gebruik de gebruiker gedefinieerde routes (UDR) om verkeer te leiden van gehele product aan een virtueel netwerkapparaat.
* Gehele product hebben toegang tot bronnen op het openbare Internet. Ook een functie van basisnetwerken.

> [!IMPORTANT]
> Elk knooppunt in een cluster AKS is geconfigureerd voor geavanceerde netwerken kan maximaal hosten **30 gehele product**. Elke VNet ingericht voor gebruik met de Azure-CNI-invoegtoepassing beperkt tot is **4096 IP-adressen** (/ 20).

## <a name="configure-advanced-networking"></a>Geavanceerde netwerk configureren

Wanneer u [maken van een cluster AKS](kubernetes-walkthrough-portal.md) in de Azure portal, de volgende parameters worden geconfigureerd voor geavanceerde netwerken:

**Virtueel netwerk**: de VNet waarin u wilt implementeren van het cluster Kubernetes. Als u maken van een nieuw VNet voor uw cluster wilt, selecteert u *nieuw* en volg de stappen in de *virtueel netwerk maken* sectie.

**Subnet**: het subnet binnen het VNet waar u wilt implementeren van het cluster. Als u maken van een nieuw subnet in het VNet voor uw cluster wilt, selecteert u *nieuw* en volg de stappen in de *subnet maken* sectie.

**Kubernetes service adresbereik**: het IP-adresbereik voor de service van het cluster Kubernetes IP-adressen. Dit bereik moet niet binnen het VNet-IP-adresbereik van het cluster.

**Kubernetes DNS-service IP-adres**: het IP-adres voor de DNS-service van het cluster. Dit adres moet binnen het *Kubernetes service adresbereik*.

**Docker Bridge adres**: het IP-adres en IP-masker toewijzen aan de Docker-brug. Dit IP-adres moet niet binnen het VNet-IP-adresbereik van het cluster.

De volgende schermafbeelding van de Azure-portal toont een voorbeeld van het configureren van deze instellingen tijdens het maken van de cluster AKS:

![Geavanceerde netwerkconfiguratie in de Azure portal][portal-01-networking-advanced]

## <a name="plan-ip-addressing-for-your-cluster"></a>IP-adressen voor uw cluster plannen

Clusters die zijn geconfigureerd met geavanceerde netwerken vereisen aanvullende planning. De grootte van uw VNet en een subnet moet geschikt zijn voor het nummer van het gehele product die u van plan bent tegelijkertijd worden uitgevoerd in het cluster, evenals de vereisten van uw vergroten/verkleinen.

IP-adressen voor het gehele product en de knooppunten van het cluster toegewezen uit het opgegeven subnet binnen het VNet. Elk knooppunt is geconfigureerd met een primaire IP-adres, het IP-adres van het knooppunt zelf en 30 extra IP-adressen vooraf geconfigureerd door Azure CNI die zijn toegewezen aan het gehele product gepland naar het knooppunt. Wanneer u uw cluster hebt uitgebreid, wordt elk knooppunt op dezelfde manier geconfigureerd met IP-adressen van het subnet.

Zoals gezegd, elke VNet ingericht voor gebruik met de Azure-CNI-invoegtoepassing beperkt tot is **4096 IP-adressen** (/ 20). Elk knooppunt in een cluster is geconfigureerd voor geavanceerde netwerken kan maximaal hosten **30 gehele product**.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

De volgende vragen en antwoorden van toepassing op de **Geavanceerd** netwerkconfiguratie.

* *Kan ik geavanceerde netwerken configureren met de Azure CLI*

  Nee. Geavanceerde netwerken wordt momenteel beschikbaar is, alleen wanneer het implementeren van AKS clusters in de Azure portal of met Resource Manager-sjabloon.

* *Kan ik virtuele machines implementeren in mijn subnet van het cluster?*

  Nee. Implementatie van virtuele machines in het subnet dat wordt gebruikt door uw cluster Kubernetes wordt niet ondersteund. Virtuele machines kunnen worden geïmplementeerd in hetzelfde VNet, maar in een ander subnet.

* *Kan ik per schil netwerkbeleid configureren?*

  Nee. Per schil netwerkbeleid worden momenteel niet ondersteund.

* *Is het maximum aantal gehele product worden geïmplementeerd op een knooppunt kunnen worden geconfigureerd?*

  Elk knooppunt kan maximaal 30 gehele product host standaard. Momenteel kunt u de maximale waarde wijzigen alleen door het wijzigen van de `maxPods` eigenschap bij het implementeren van een cluster met Resource Manager-sjabloon.

* *Hoe kan ik extra eigenschappen voor het subnet dat ik heb tijdens het maken van de cluster AKS gemaakt configureren? Bijvoorbeeld, service-eindpunten.*

  De volledige lijst met eigenschappen voor het VNet en subnetten die u tijdens het maken van AKS cluster maakt kan worden geconfigureerd in de standaard pagina VNet-configuratie in de Azure portal.

## <a name="next-steps"></a>Volgende stappen

### <a name="networking-in-aks"></a>Netwerken in AKS

Meer informatie over netwerken in AKS in de volgende artikelen:

[Gebruik een statisch IP-adres met de Azure Kubernetes Service (AKS) load balancer](static-ip.md)

[HTTPS, inkomend op Azure Container Service (AKS)](ingress.md)

[Een interne load balancer gebruiken met Azure Container Service (AKS)](internal-lb.md)

### <a name="acs-engine"></a>ACS-Engine

[Azure Container Service Engine (ACS-Engine)] [ acs-engine] is een open source-project dat Azure Resource Manager-sjablonen die u gebruiken genereert kunt voor het implementeren van clusters Docker-functionaliteit in Azure. Kubernetes, DC/OS Swarm-modus en Swarm orchestrators kunnen worden geïmplementeerd met de ACS-Engine.

Kubernetes clusters die zijn gemaakt met de ACS-Engine ondersteuning voor zowel de [kubenet] [ kubenet] en [Azure CNI] [ cni-networking] invoegtoepassingen gebruikt. Als zodanig worden basiseigenschappen en geavanceerde scenario's voor netwerken ondersteund door de ACS-Engine.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[aks-ssh]: aks-ssh.md
