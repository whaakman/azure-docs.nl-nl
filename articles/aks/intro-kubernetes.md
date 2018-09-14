---
title: Kennismaking met Azure Kubernetes Service
description: Met Azure Kubernetes Service kunt u eenvoudig toepassingen op basis van containers implementeren en beheren in Azure.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: overview
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 294ccd70e0339ed5ad68022f002e4864fc52b452
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286717"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Azure Kubernetes Service (AKS) maakt het eenvoudig om een ​​beheerd Kubernetes-cluster in Azure te implementeren. AKS verkleint de complexiteit en de operationele overhead die gepaard gaan met het beheer van Kubernetes door veel van deze taken naar Azure over te hevelen. Azure handelt als een gehoste Kubernetes-service cruciale taken voor u af zoals statuscontrole en onderhoud. Bovendien is de service gratis; u betaalt alleen voor de agentknooppunten binnen uw clusters, niet voor de masters.

Dit document biedt een overzicht van de functies van Azure Kubernetes Service (AKS).

## <a name="flexible-deployment-options"></a>Flexibele implementatieopties

Azure Kubernetes Service biedt portal-, opdrachtregel- en sjabloongestuurde implementatie-opties (Resource Manager-sjablonen en Terraform). Bij de implementatie van een AKS-cluster worden de Kubernetes-master en alle knooppunten voor u geïmplementeerd en geconfigureerd. Extra functies zoals geavanceerd netwerken, Azure Active Directory-integratie en bewaking kunnen ook tijdens het implementatieproces worden geconfigureerd.

Zie voor meer informatie zowel de [AKS-portal-snelstart][aks-portal] en de [AKS CLI-snelstart][aks-cli].

## <a name="identity-and-security-management"></a>Identiteits- en beveiligingsbeheer

AKS-clusters ondersteunen [RBAC (op rollen gebaseerd toegangsbeheer)][kubernetes-rbac]. Een AKS-cluster kan ook worden geconfigureerd om te worden geïntegreerd met Azure Active Directory. In deze configuratie kan Kubernetes-toegang worden geconfigureerd op basis van Azure Active Directory-identiteit en groepslidmaatschap.

Zie voor meer informatie [Azure Active Directory integreren met AKS][aks-aad].

## <a name="integrated-logging-and-monitoring"></a>Geïntegreerde logboekregistratie en bewaking

Containerstatus geeft u inzicht in prestaties door metrische van geheugen- en processorgegevens te verzamelen van containers, knooppunten en controllers. Er worden ook containerlogboeken verzameld. Deze gegevens worden opgeslagen in uw Log Analytics-werkruimte en zijn beschikbaar via Azure Portal, Azure CLI of een REST-eindpunt.

Zie [Containerstatus van Azure Kubernetes Service bewaken][container-health] voor meer informatie.

## <a name="cluster-node-scaling"></a>Clusterknooppunten schalen

Naarmate de vraag naar resources toeneemt, kunnen de knooppunten van een AKS-cluster dienovereenkomstig worden geschaald. Als de resourcevraag daalt, kunnen knooppunten worden verwijderd door het cluster in te schalen. AKS-schaalbewerkingen kunnen worden uitgevoerd met behulp van Azure Portal of de Azure-CLI.

Zie [Een Azure Kubernetes Service-cluster (AKS) schalen][aks-scale] voor meer informatie.

## <a name="cluster-node-upgrades"></a>Clusterknooppuntupgrades

Azure Kubernetes Service biedt meerdere Kubernetes versies. Wanneer nieuwe versies beschikbaar komen in AKS, kan uw cluster worden bijgewerkt met behulp van Azure Portal of Azure-CLI. Tijdens het upgradeproces worden knooppunten zorgvuldig afgebakend en geleegd om onderbreking van actieve toepassingen te minimaliseren.

Zie [Een Azure Kubernetes Service-cluster (AKS) upgraden][aks-upgrade] voor meer informatie.

## <a name="http-application-routing"></a>Routering van HTTP-toepassing

De HTTP-toepassingsrouteringsoplossing maakt het eenvoudig om toegang te krijgen tot in uw AKS-cluster geïmplementeerde toepassingen. Indien ingeschakeld, configureert de HTTP-toepassingsrouteringsoplossing een ingangscontroller in uw AKS-cluster. Wanneer er toepassingen worden geïmplementeerd, worden automatisch openbaar toegankelijke DNS-namen geconfigureerd.

Zie voor meer informatie [HTTP-toepassingsroutering][aks-http-routing].

## <a name="gpu-enabled-nodes"></a>GPU-knooppunten

AKS ondersteunt het maken van GPU-knooppuntpools. Azure biedt momenteel VM’s met een of meerdere GPU’s. GPU-VM's zijn ontworpen voor rekenintensieve, grafisch-intensieve en visualisatiewerkbelastingen.

Zie [GPU's gebruiken op AKS][aks-gpu] voor meer informatie.

## <a name="development-tooling-integration"></a>Integratie van ontwikkelingshulpprogramma’s

Kubernetes heeft een uitgebreid ecosysteem van hulpprogramma’s voor ontwikkeling en beheer, zoals Helm, Draft en de Kubernetes-extensie voor Visual Studio Code. Deze hulpprogramma's werken naadloos samen met Azure Kubernetes Service.

Bovendien biedt Azure Dev Spaces een snelle, iteratieve Kubernetes-ontwikkelervaring voor teams. Met minimale configuratie kunt u rechtstreeks in Azure Kubernetes Service (AKS) herhaaldelijk containers uitvoeren en fouten hierin opsporen.

Zie [Azure Dev Spaces][azure-dev-spaces] voor meer informatie.

Azure DevOps-project biedt een eenvoudige oplossing voor het overbrengen van bestaande code en Git-opslagplaatsen naar Azure. Het DevOps-project maakt automatisch Azure-resources zoals AKS, een release-pijplijn in VSTS met een builddefinitie voor CI, stelt een releasedefinitie in voor CD en maakt vervolgens een Azure Application Insights-resource voor bewaking.

Zie [Azure DevOps-project][azure-devops] voor meer informatie.

## <a name="virtual-network-integration"></a>Integratie van virtueel netwerk

Een AKS-cluster kan worden geïmplementeerd in een bestaand VNet. In deze configuratie krijgt elke pod in het cluster een IP-adres toegewezen in het VNet en kan deze direct communiceren met andere pods in het cluster en met andere knooppunten in het VNet. Pods kunnen ook verbinding maken met andere services in een peering-VNet en met lokale netwerken via ExpressRoute en site-to-site (S2S) VPN-verbindingen.

Bekijk het [AKS-netwerkoverzicht][aks-networking] voor meer informatie.

## <a name="private-container-registry"></a>Privécontainerregister

Integreer met Azure Container Registry (ACR) voor privéopslag van uw Docker-installatiekopieën.

Zie voor meer informatie [Azure Container Registry (ACR)][acr-docs].

## <a name="storage-volume-support"></a>Opslagvolumeondersteuning

Azure Kubernetes Service (AKS) ondersteunt het koppelen van opslagvolumes voor persistente gegevens. AKS-clusters worden gemaakt met ondersteuning voor Azure Files en Azure Disks.

Zie [Azure Files][azure-files] en [Azure Disks][azure-disk]voor meer informatie.

## <a name="docker-image-support"></a>Ondersteuning voor Docker-installatiekopieën

Azure Kubernetes Service (AKS) ondersteunt de bestandsindeling voor Docker-installatiekopieën.

## <a name="kubernetes-certification"></a>Kubernetes-certificering

De Azure Kubernetes Service (AKS) is door de CNCF gecertificeerd als compatibel met Kubernetes.

## <a name="regulatory-compliance"></a>Naleving van regelgeving

Azure Kubernetes Service (AKS) voldoet aan SOC, ISO en PCI DSS.

## <a name="next-steps"></a>Volgende stappen

U vindt meer informatie over het implementeren en beheren van AKS in de bijbehorende Quick Start van AKS.

> [!div class="nextstepaction"]
> [AKS-snelstart][aks-cli]

<!-- LINKS - external -->
[acs-engine]: https://github.com/Azure/acs-engine
[draft]: https://github.com/Azure/draft
[helm]: https://helm.sh/
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./aad-integration.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md
[aks-http-routing]: ./http-application-routing.md
[aks-networking]: ./networking-overview.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./scale-cluster.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: https://docs.microsoft.com/en-us/azure/dev-spaces/azure-dev-spaces
[azure-devops]: https://docs.microsoft.com/en-us/azure/devops-project/overview
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../monitoring/monitoring-container-health.md

