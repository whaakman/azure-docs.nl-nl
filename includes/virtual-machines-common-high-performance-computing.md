---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux, virtual-machines-windows
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 01/15/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: a1a6b31c1500c51dbbc32683d9e0e911b60dcae4
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246824"
---
Organisaties hebben grootschalige computerbehoeften. Deze Big Compute-workloads omvatten technisch ontwerp en -analyse, berekeningen van financiële risico's, rendering van afbeeldingen, complexe modellering, Monte Carlo-simulaties en meer. 

Gebruik de Azure-cloud efficiënt rekenintensieve Linux- en Windows workloads, uitvoeren van parallelle batch-taken op traditionele HPC-simulaties. Voer uw HPC en batch-workloads op Azure-infrastructuur, met uw eigen keuze aan compute services, raster managers, Marketplace-oplossingen en leverancier gehoste (SaaS)-toepassingen. Azure biedt flexibele oplossingen voor het werk verdelen en schalen naar duizenden virtuele machines of kerngeheugens en daarna geschaald verkleinen wanneer u minder bronnen nodig hebt. 

## <a name="solution-options"></a>Oplossingsopties

* **Doe oplossingen**
    * Instellen van de clusteromgeving van uw eigen in Azure virtuele machines of [virtuele-machineschaalsets](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 
    * Optillen en verplaatsen van een on-premises cluster, of implementeren van een nieuw cluster in Azure voor extra capaciteit. 
    * Azure Resource Manager-sjablonen gebruiken om te implementeren van toonaangevende [werkbelasting managers](#workload-managers), infrastructuur en [toepassingen](#hpc-applications). 
    * Kies [HPC en GPU VM-grootten](#hpc-and-gpu-vm-sizes) die gespecialiseerde hardware- en netwerkverbindingen voor MPI- of GPU workloads bevatten. 
    * Voeg [opslag met hoge prestaties](#hpc-storage) voor I/O-intensieve workloads.
* **Hybride oplossingen**
    * Uw on-premises oplossing voor het offloaden van piekbelastingen te verwerken ('burst') naar Azure-infrastructuur uitbreiden
    * Gebruik van cloud computing in de aanvraag met uw bestaande [van workload manager](#workload-managers).
    * Profiteer van [HPC en GPU VM-grootten](#hpc-and-gpu-vm-sizes) voor MPI- of GPU-werkbelastingen.
* **BIG Compute-oplossingen als een service**
    * Ontwikkel aangepaste Big Compute-oplossingen en werkstromen die gebruikmaken van [Azure CycleCloud](#azure-cyclecloud), [Azure Batch](#azure-batch), en de bijbehorende [Azure-services](#related-azure-services).
    * Uitvoeren van Azure engineering en simulatie oplossingen van leveranciers zoals [Altair](http://www.altair.com/), [oorspronkelijke](https://www.rescale.com/azure/), en [Cycle Computing](https://cyclecomputing.com/) (nu [samengevoegd met Microsoft](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/)).
    * Gebruik een [Cray-supercomputer](https://www.cray.com/solutions/supercomputing-as-a-service/cray-in-azure) als een service die wordt gehost in Azure.
* **Marketplace-oplossingen**
    * Gebruik de schaal van [HPC-toepassingen](#hpc-applications) en [oplossingen](#marketplace-solutions) aangeboden in de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). 
    
De volgende secties bevatten meer informatie over de ondersteunende technologieën en koppelingen naar instructies.

## <a name="marketplace-solutions"></a>Marketplace-oplossingen

Ga naar de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) voor Linux en Windows-VM-installatiekopieën en is ontworpen voor HPC-oplossingen. Voorbeelden zijn:

* [RogueWave op basis van CentOS HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased74HPC?tab=Overview)
* [SUSE Linux Enterprise Server voor HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
*  [TIBCO Grid serverengine](https://azuremarketplace.microsoft.com/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
* [Azure Data Science VM voor Windows en Linux](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md)
* [D3View](https://azuremarketplace.microsoft.com/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
* [UberCloud](https://azure.microsoft.com/search/marketplace/?q=ubercloud)

## <a name="hpc-applications"></a>HPC-toepassingen

Aangepaste of commerciële HPC-toepassingen uitvoeren in Azure. Enkele voorbeelden in deze sectie zijn benchmarked efficiënt schalen met aanvullende virtuele machines of compute-resources. Ga naar de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace) voor ready-to-deploy-oplossingen.

> [!NOTE]
> Neem contact op met de leverancier van een commerciële toepassing voor licentieverlening of andere beperkingen voor het uitvoeren in de cloud. Niet alle leveranciers bieden licenties waarbij u betaalt naar gebruik. U mogelijk nodig hebt voor uw oplossing een licentieserver in de cloud of verbinding maken met een on-premises licentieserver.

### <a name="engineering-applications"></a>Engineering-toepassingen

* [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
* [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
* [MATLAB Distributed Computing Server](../articles/virtual-machines/windows/matlab-mdcs-cluster.md)
* [StarCCM+](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
* [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

### <a name="graphics-and-rendering"></a>Grafische afbeeldingen en weergaven

* [Rendering van toepassingen](../articles/batch/batch-rendering-service.md) op Azure Batch, met inbegrip van Autodesk Maya, 3ds Max en Arnold, Chaos Group V-Ray en Blender

### <a name="ai-and-deep-learning"></a>AI- en deep learning

* [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/cntk-on-azure)
* [Deep Learning VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
* [Batch scheepswerf recepten voor deep learning](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)

## <a name="hpc-and-gpu-vm-sizes"></a>HPC en GPU VM-grootten

Azure biedt een scala aan grootten voor [Linux](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [Windows](../articles/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuele machines, inclusief de grootte die is ontworpen voor rekenintensieve workloads. Bijvoorbeeld, kunt H16r en H16mr VM's verbinden met een hoge doorvoer back-end RDMA-netwerk. Deze cloudnetwerk kunt verbeteren de prestaties van nauw gekoppelde parallelle toepassingen die worden uitgevoerd onder [Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) of Intel MPI. 

Virtuele machines uit de N-serie zijn voorzien van NVIDIA GPU's die zijn ontworpen voor rekenintensieve of grafisch-intensieve toepassingen, met inbegrip van kunstmatige intelligentie (AI) learning en visualisatie. 

Meer informatie:

* High performance computing-grootten voor [Linux](../articles/virtual-machines/linux/sizes-hpc.md) en [Windows](../articles/virtual-machines/windows/sizes-hpc.md) VM's 
* Met GPU grootten voor [Linux](../articles/virtual-machines/linux/sizes-gpu.md) en [Windows](../articles/virtual-machines/windows/sizes-gpu.md) VM's 

## <a name="azure-cyclecloud"></a>Azure CycleCloud

Veelvoorkomende werkbelastingen effectief te beheren met gemak tijdens het maken van en het optimaliseren van HPC-clusters op Azure Virtual machines met [Azure CycleCloud](https://docs.microsoft.com/azure/cyclecloud/overview).

Leer hoe u het volgende doet:

* [Installeren en CycleCloud met Resource Manager-sjabloon instellen](https://docs.microsoft.com/azure/cyclecloud/quickstart-install-cyclecloud)

* [CycleCloud handmatig instellen](https://docs.microsoft.com/azure/cyclecloud/installation)

## <a name="azure-batch"></a>Azure Batch

[Batch](../articles/batch/batch-technical-overview.md) is een platform service voor het uitvoeren van grootschalige parallelle en high performance computing (HPC)-toepassingen efficiënt in de cloud. Azure Batch plant rekenintensieve werken om uit te voeren op een beheerde pool van virtuele machines, en kan automatisch de schaal van rekenkracht resources om te voldoen aan de behoeften van uw taken. 

SaaS-providers of ontwikkelaars kunnen de Batch-SDK's en hulpprogramma's gebruiken voor integratie van HPC-toepassingen of workloads voor containers met Azure, gegevens te Faseren naar Azure, en bouwen van pijplijnen voor het uitvoeren van taak. 

Leer hoe u het volgende doet:

* [Aan de slag met ontwikkelen met Batch](../articles/batch/quick-run-dotnet.md)
* [Gebruik van Azure Batch-codevoorbeelden](https://github.com/Azure/azure-batch-samples)
* [Virtuele machines met lage prioriteit gebruiken met Batch](../articles/batch/batch-low-pri-vms.md)
* [Beperkte HPC-workloads uitvoeren met Batch scheepswerf](https://github.com/Azure/batch-shipyard)
* [Parallelle R-workloads uitvoert op Batch](https://github.com/Azure/doAzureParallel)
* [Op aanvraag Spark-taken uitvoeren in Batch](https://github.com/Azure/aztk)

## <a name="workload-managers"></a>Workload managers

Hier volgen enkele voorbeelden van het cluster en de werkbelasting managers die kunnen worden uitgevoerd in Azure-infrastructuur. Zelfstandige clusters maken in Azure-VM's of burst naar Azure VM's uit een on-premises cluster. 
* [Alces Flight Compute](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview)
* [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/) 
* [Bright Cluster Manager](http://www.brightcomputing.com/technology-partners/microsoft)
* [IBM Spectrum Symphony en Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
* [PBS Pro](http://pbspro.org)
* [Microsoft HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) 

## <a name="hpc-storage"></a>HPC-opslag

Grootschalige Batch- en HPC-werkbelastingen hebben aanvragen voor opslag van gegevens- en toegangsbeheer die groter zijn dan de mogelijkheden van traditionele cloud-bestandssystemen. 

Meer informatie:

* [Avere vFXT voor Azure](../articles/avere-vfxt/avere-vfxt-overview.md) 
* [Parallelle virtuele bestandssystemen op Azure](https://azure.microsoft.com/resources/parallel-virtual-file-systems-on-microsoft-azure/) inclusief [Lustre](http://lustre.org/) en [BeeGFS](http://www.beegfs.com/content/).

## <a name="related-azure-services"></a>Gerelateerde Azure Services

Virtuele Azure-machines, virtuele-machineschaalsets, Batch en gerelateerde compute-services vormen de basis van de meeste Azure-HPC-oplossingen. Echter, uw oplossing kan profiteren van veel gerelateerde Azure-services. Hier volgt een gedeeltelijke lijst:

### <a name="storage"></a>Opslag

* [BLOB, table en queue storage](../articles/storage/storage-introduction.md)
* [Bestandsopslag](../articles/storage/storage-files-introduction.md)

### <a name="data-and-analytics"></a>Gegevens en analyse

* [HDInsight](../articles/hdinsight/hadoop/apache-hadoop-introduction.md)
* [Data Factory](../articles/data-factory/introduction.md)
* [Data Lake Storage Gen1](../articles/data-lake-store/data-lake-store-overview.md)
* [Databricks](../articles/azure-databricks/what-is-azure-databricks.md)
* [SQL Database](../articles/sql-database/sql-database-technical-overview.md)

### <a name="ai-and-machine-learning"></a>AI + Machine Learning

* [Machine Learning Service](../articles/machine-learning/service/overview-what-is-azure-ml.md)
* [Genomics](../articles/genomics/overview-what-is-genomics.md)

### <a name="networking"></a>Netwerken

* [Virtueel netwerk](../articles/virtual-network/virtual-networks-overview.md)
* [ExpressRoute](../articles/expressroute/expressroute-introduction.md)

### <a name="containers"></a>Containers

* [Azure Kubernetes Service (AKS)](../articles/aks/intro-kubernetes.md)
* [Container Registry](../articles/container-registry/container-registry-intro.md)
* [Container Instances](../articles/container-instances/container-instances-overview.md)

## <a name="customer-stories"></a>Verhalen van klanten

Voorbeelden van klanten die zakelijke problemen met Azure HPC-oplossingen hebben opgelost:

* [ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu) 
* [AXA Global P&C](https://customers.microsoft.com/story/axa-global-p-and-c)
* [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
* [d3View](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
* [EFS](https://customers.microsoft.com/story/efs-professionalservices-azure)
* [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
* [MetLife](https://customers.microsoft.com/story/metlife-insurance-azure-cloud-services-windows-server-analytics-platform-server-en)
* [Microsoft Research](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
* [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
* [Mitsubishi UFJ Securities International](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
* [NeuroInitiative](https://customers.microsoft.com/story/neuroinitiative-health-provider-azure)
* [Schlumberger](https://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Towers Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over Azure-oplossingen voor [high performance computing](https://azure.microsoft.com/solutions/high-performance-computing/), [rendering](https://azure.microsoft.com/solutions/big-compute/rendering/), [bankwezen en kapitaalmarkten](https://finance.azure.com/), en [genomics](https://enterprise.microsoft.com/industries/health/genomics/).

* Meer informatie over [Big Compute-architecturen](https://azure.microsoft.com/solutions/architecture/?solution=high-performance-computing) in Azure.
