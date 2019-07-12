---
title: Kennismaking met Azure Kubernetes Service
description: Ontdek de functies en voordelen van Azure Kubernetes Service voor het implementeren en beheren van toepassingen op basis van containers in Azure.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: overview
ms.date: 05/06/2019
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: 5515951a9bde596935f4c603ffd9e088f74dee45
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67615166"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Azure Kubernetes Service (AKS) maakt het eenvoudig om een ​​beheerd Kubernetes-cluster in Azure te implementeren. AKS verkleint de complexiteit en de operationele overhead die gepaard gaan met het beheer van Kubernetes door veel van deze taken naar Azure over te hevelen. Azure handelt als een gehoste Kubernetes-service cruciale taken voor u af zoals statuscontrole en onderhoud. De Kubernetes-modellen worden beheerd door Azure. U beheert en onderhoudt alleen de agentknooppunten. Als een beheerde Kubernetes-service is AKS gratis. U betaalt alleen voor de agentknooppunten binnen uw clusters, niet voor de masters.

U kunt een AKS-cluster maken in de Azure Portal, met de Azure CLI, of met de sjabloongestuurde implementatieopties, zoals Resource Manager-sjablonen en Terraform. Wanneer u een AKS-cluster implementeert, worden de Kubernetes-master en alle knooppunten voor u geïmplementeerd en geconfigureerd. Extra functies zoals geavanceerd netwerken, Azure Active Directory-integratie en bewaking kunnen ook tijdens het implementatieproces worden geconfigureerd. Ondersteuning voor Windows Server-containers is momenteel in preview in AKS.

Zie voor meer informatie over de basisbeginselen voor Kubernetes, [Kubernetes-concepten voor AKS core][concepts-clusters-workloads].

Om te beginnen, voer de Quick Start voor AKS [in Azure portal][aks-portal] or [with the Azure CLI][aks-cli].

## <a name="access-security-and-monitoring"></a>Toegang, beveiliging en bewaking

Voor verbeterde beveiliging en verbeterd beheer kunt u AKS integreren met Azure Active Directory en gebruik maken van op Kubernetes-rollen gebaseerd toegangsbeheer. U kunt ook de status van uw cluster en resources bewaken.

### <a name="identity-and-security-management"></a>Identiteits- en beveiligingsbeheer

Om toegang te beperken tot clusterresources, AKS ondersteunt [Kubernetes op rollen gebaseerd toegangsbeheer (RBAC)][kubernetes-rbac]. Met RBAC kunt u de toegang instellen tot Kubernetes-resources en -naamruimten en kunt u de machtigingen voor deze bronnen instellen. U kunt een AKS-cluster ook configureren om te worden geïntegreerd met Azure Active Directory (AD). Met Azure AD-integratie kan Kubernetes-toegang worden geconfigureerd op basis van bestaande identiteiten en groepslidmaatschap. Aan uw bestaande Azure AD-gebruikers en -groepen kan toegang tot AKS-resources worden verleend met een geïntegreerde ervaring voor eenmalige aanmelding.

Zie voor meer informatie over identiteit [opties voor toegang en identiteit voor AKS][concepts-identity].

Zie voor het beveiligen van uw AKS-clusters, [Integreer Azure Active Directory met AKS][aks-aad].

### <a name="integrated-logging-and-monitoring"></a>Geïntegreerde logboekregistratie en bewaking

Om inzicht te krijgen in hoe uw AKS-cluster en geïmplementeerde toepassingen functioneren, verzamelt Azure Monitor voor containerstatus metrische geheugen- en processorgegevens van containers, knooppunten en domeincontrollers. Logboeken voor containers zijn beschikbaar en u kunt ook [Bekijk de logboeken van Kubernetes-master][aks-master-logs]. Deze bewakingsgegevens worden opgeslagen in een Log Analytics-werkruimte van Azure en zijn beschikbaar via Azure Portal, Azure CLI of een REST-eindpunt.

Zie voor meer informatie, [containerstatus van Monitor Azure Kubernetes Service][container-health].

## <a name="clusters-and-nodes"></a>Clusters en knooppunten

AKS-knooppunten worden uitgevoerd op virtuele machines in Azure. U kunt opslag met knooppunten en pods verbinden, clusteronderdelen bijwerken en GPU's gebruiken. AKS biedt ondersteuning voor Kubernetes-clusters met meerdere knooppuntgroepen ter ondersteuning van verschillende besturingssystemen en Windows Server-containers (momenteel in Preview-versie). Een aangepaste installatiekopie van het besturingssysteem van Ubuntu Linux-knooppunten worden uitgevoerd en Windows Server-knooppunten een aangepaste installatiekopie van het Windows Server 2019 besturingssysteem worden uitgevoerd.

### <a name="cluster-node-and-pod-scaling"></a>Clusterknooppunten en pods schalen

Naarmate de vraag naar resources verandert, kan het aantal clusterknooppunten of -pods waarop uw services worden uitgevoerd, automatisch omhoog of omlaag worden geschaald. U kunt zowel de horizontale automatische schaalaanpassing voor pods als de automatische schaalaanpassing voor clusters gebruiken. Met deze schalingsmethode kan het AKS-cluster automatisch worden aangepast aan de eisen, zodat alleen de benodigde resources worden uitgevoerd.

Zie voor meer informatie, [een Azure Kubernetes Service (AKS)-cluster schalen][aks-scale].

### <a name="cluster-node-upgrades"></a>Clusterknooppuntupgrades

Azure Kubernetes Service biedt meerdere Kubernetes versies. Wanneer nieuwe versies beschikbaar komen in AKS, kan uw cluster worden bijgewerkt met behulp van Azure Portal of Azure-CLI. Tijdens het upgradeproces worden knooppunten zorgvuldig afgebakend en geleegd om onderbreking van actieve toepassingen te minimaliseren.

Zie voor meer informatie over de levenscyclus van versies, [ondersteund Kubernetes-versies in AKS][aks-supported versions]. For steps on how to upgrade, see [Upgrade an Azure Kubernetes Service (AKS) cluster][aks-upgrade].

### <a name="gpu-enabled-nodes"></a>GPU-knooppunten

AKS ondersteunt het maken van GPU-knooppuntpools. Azure biedt momenteel VM’s met een of meerdere GPU’s. GPU-VM's zijn ontworpen voor rekenintensieve, grafisch-intensieve en visualisatiewerkbelastingen.

Zie voor meer informatie, [met behulp van GPU's in AKS][aks-gpu].

### <a name="storage-volume-support"></a>Opslagvolumeondersteuning

Ter ondersteuning van werkbelastingen kunt u opslagvolumes koppelen voor uw permanente gegevens. U kunt zowel statische als dynamische volumes gebruiken. Afhankelijk van het aantal verbonden pods waarmee de opslag moet worden gedeeld, kunt u opslag ondersteund door Azure Disks gebruiken voor toegang tot één pod, of opslag ondersteund door Azure Files voor toegang tot meerdere pods tegelijk.

Zie voor meer informatie, [opslagopties voor toepassingen in AKS][concepts-storage].

Aan de slag met dynamische permanente volumes met behulp van [Azure Disks][azure-disk] or [Azure Files][azure-files].

## <a name="virtual-networks-and-ingress"></a>Virtual Networks en inkomend verkeer

Een AKS-cluster kan worden geïmplementeerd in een bestaand virtueel netwerk. In deze configuratie krijgt elke pod in het cluster een IP-adres toegewezen in het virtueel netwerk. De pod kan direct communiceren met andere pods in het cluster en met andere knooppunten in het virtueel netwerk. Pods kunnen ook verbinding maken met andere services in een gekoppeld virtueel netwerk en met on-premises netwerken via ExpressRoute en S2S VPN-verbindingen (site-to-site).

Zie voor meer informatie de [concepten voor toepassingen in AKS netwerk][aks-networking].

Om te beginnen met inkomend verkeer, Zie [HTTP-toepassingsroutering][aks-http-routing].

### <a name="ingress-with-http-application-routing"></a>Ingress met HTTP-toepassingsroutering

De invoegtoepassing voor HTTP-toepassingsroutering vergemakkelijkt de toegang tot toepassingen die in uw AKS-cluster zijn geïmplementeerd. Indien ingeschakeld, configureert de HTTP-toepassingsrouteringsoplossing een ingangscontroller in uw AKS-cluster. Wanneer er toepassingen worden geïmplementeerd, worden automatisch openbaar toegankelijke DNS-namen geconfigureerd. HTTP-toepassingsroutering configureert een DNS-zone en integreert deze met het AKS-cluster. Daarna kunt u inkomende Kubernetes-resources op de gebruikelijke manier implementeren.

Om te beginnen met inkomend verkeer, Zie [HTTP-toepassingsroutering][aks-http-routing].

## <a name="development-tooling-integration"></a>Integratie van ontwikkelingshulpprogramma’s

Kubernetes heeft een uitgebreid ecosysteem van hulpprogramma’s voor ontwikkeling en beheer, zoals Helm, Draft en de Kubernetes-extensie voor Visual Studio Code. Deze hulpprogramma's werken naadloos met AKS.

Bovendien biedt Azure Dev Spaces een snelle, iteratieve Kubernetes-ontwikkelervaring voor teams. Met een minimale configuratie kunt u containers rechtstreeks in AKS uitvoeren en debuggen. Als u wilt beginnen, Zie [Azure Dev spaties][azure-dev-spaces].

Het Azure DevOps-project biedt een eenvoudige oplossing voor het overbrengen van bestaande code en Git-opslagplaatsen naar Azure. Het DevOps-project maakt automatisch Azure-resources zoals AKS, een release-pipeline in Azure DevOps Services met een build-pipeline voor CI, stelt een release-pipeline in voor CD en maakt vervolgens een Azure Application Insights-resource voor bewaking.

Zie voor meer informatie, [Azure DevOps-project][azure-devops].

## <a name="docker-image-support-and-private-container-registry"></a>Ondersteuning voor Docker-installatiekopieën en registers voor persoonlijke containers

AKS ondersteunt de Docker-installatiekopie-indeling. Voor privéopslag van uw Docker-installatiekopieën kunt u AKS integreren met Azure Container Registry (ACR).

Zie voor het maken van de installatiekopieopslag van het persoonlijke [Azure Container Registry][acr-docs].

## <a name="kubernetes-certification"></a>Kubernetes-certificering

De Azure Kubernetes Service (AKS) is door de CNCF gecertificeerd als compatibel met Kubernetes.

## <a name="regulatory-compliance"></a>Naleving van regelgeving

AKS (Azure Kubernetes Service) voldoet aan SOC, ISO, PCI DSS en HIPAA. Zie voor meer informatie, [overzicht van Microsoft Azure-naleving][compliance-doc].

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het implementeren en beheren van AKS vindt u in de snelstartgids van Azure CLI.

> [!div class="nextstepaction"]
> [Quick Start voor AKS][aks-cli]

<!-- LINKS - external -->
[aks-engine]: https://github.com/Azure/aks-engine
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[compliance-doc]: https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942

<!-- LINKS - internal -->
[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./azure-ad-integration.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md
[aks-http-routing]: ./http-application-routing.md
[aks-networking]: ./concepts-network.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/azure-dev-spaces
[azure-devops]: https://docs.microsoft.com/azure/devops-project/overview
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../monitoring/monitoring-container-health.md
[aks-master-logs]: view-master-logs.md
[aks-supported versions]: supported-kubernetes-versions.md
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[kubernetes-rbac]: concepts-identity.md#role-based-access-controls-rbac
[concepts-identity]: concepts-identity.md
[concepts-storage]: concepts-storage.md
