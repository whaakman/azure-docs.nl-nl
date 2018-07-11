---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-linux, virtual-machines-windows
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 07/02/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 5a6c5498b4607719199363b5f6d93d3b42ccec0d
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967620"
---
Organisaties hebben grootschalige computerbehoeften. Deze Big Compute-workloads omvatten technisch ontwerp en -analyse, berekeningen van financiële risico's, rendering van afbeeldingen, complexe modellering, Monte Carlo-simulaties en meer. 

Gebruik de Azure-cloud efficiënt rekenintensieve Linux- en Windows workloads, uitvoeren van parallelle batch-taken op traditionele HPC-simulaties. Voer uw HPC en batch-workloads op Azure-infrastructuur, met uw eigen keuze aan compute services, raster managers, Marketplace-oplossingen en leverancier gehoste (SaaS)-toepassingen. Azure biedt flexibele oplossingen voor het werk verdelen en schalen naar duizenden virtuele machines of kerngeheugens en daarna geschaald verkleinen wanneer u minder bronnen nodig hebt. 



## <a name="solution-options"></a>Oplossingsopties



* **Doe oplossingen**
    * Instellen van de clusteromgeving van uw eigen in Azure virtuele machines of [virtuele-machineschaalsets](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 
    * Optillen en verplaatsen van een on-premises cluster, of implementeren van een nieuw cluster in Azure voor extra capaciteit. 
    * Azure Resource Manager-sjablonen gebruiken om te implementeren van toonaangevende [werkbelasting managers](#workload-managers), infrastructuur en [toepassingen](#hpc-applications). 
    * Kies [HPC en GPU VM-grootten](#hpc-and-gpu-sizes) die gespecialiseerde hardware- en netwerkverbindingen voor MPI- of GPU workloads bevatten. 
    * Voeg [opslag met hoge prestaties](#hpc-storage) voor I/O-intensieve workloads.
* **Hybride oplossingen**
    * Uw on-premises oplossing voor het offloaden van piekbelastingen te verwerken ('burst') naar Azure-infrastructuur uitbreiden
    * Gebruik van cloud computing in de aanvraag met uw bestaande [van workload manager](#workload-manager).
    * Profiteer van [HPC en GPU VM-grootten](#hpc-and-gpu-sizes) voor MPI- of GPU-werkbelastingen.
* **BIG Compute-oplossingen als een service**
    * Ontwikkel aangepaste Big Compute-oplossingen en werkstromen die gebruikmaken van [Azure Batch](#azure-batch) en gerelateerde [Azure-services](#related-azure-services).
    * Uitvoeren van Azure engineering en simulatie oplossingen van leveranciers zoals [Altair](http://www.altair.com/), [oorspronkelijke](https://www.rescale.com/azure/), en [Cycle Computing](https://cyclecomputing.com/) (nu [samengevoegd met Microsoft](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/)).
    * Gebruik een [Cray-supercomputer](https://www.cray.com/solutions/supercomputing-as-a-service/cray-in-azure) als een service die wordt gehost in Azure.
* **Marketplace-oplossingen**
    * Gebruik de schaal van [HPC-toepassingen](#hpc-applications) en [oplossingen](#marketplace-solutions) aangeboden in de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). 
    


De volgende secties bevatten meer informatie over de ondersteunende technologieën en koppelingen naar instructies.



## <a name="marketplace-solutions"></a>Marketplace-oplossingen

Ga naar de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) voor Linux en Windows-VM-installatiekopieën en is ontworpen voor HPC-oplossingen. Voorbeelden zijn:

* [RogueWave op basis van CentOS HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
* [SUSE Linux Enterprise Server voor HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
*  [TIBCO Grid serverengine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
* [Azure Data Science VM voor Windows en Linux](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md)
* [D3View](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
* [UberCloud](https://azure.microsoft.com/search/marketplace/?q=ubercloud)
* [Intel Cloud Edition voor Lustre](https://azuremarketplace.microsoft.com/marketplace/apps/intel.intel-cloud-edition-gs)


 
## <a name="hpc-applications"></a>HPC-toepassingen

Aangepaste of commerciële HPC-toepassingen uitvoeren in Azure. Enkele voorbeelden in deze sectie zijn benchmarked efficiënt schalen met aanvullende virtuele machines of compute-resources. Ga naar de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace) voor ready-to-deploy-oplossingen.

> [!NOTE]
> Neem contact op met de leverancier van een commerciële toepassing voor licentieverlening of andere beperkingen voor het uitvoeren in de cloud. Niet alle leveranciers bieden licenties waarbij u betaalt naar gebruik. U mogelijk nodig hebt voor uw oplossing een licentieserver in de cloud of verbinding maken met een on-premises licentieserver.

### <a name="engineering-applications"></a>Engineering-toepassingen


* [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
* [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
* [MATLAB Computing Server gedistribueerd](../articles/virtual-machines/windows/matlab-mdcs-cluster.md)
* [StarCCM+](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
* [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)



### <a name="graphics-and-rendering"></a>Grafische afbeeldingen en weergaven

* [Autodesk Maya, 3ds Max en Arnold](../articles/batch/batch-rendering-service.md) op Azure Batch 

### <a name="ai-and-deep-learning"></a>AI- en deep learning

* [Batch AI](../articles/batch-ai/overview.md) training voor deep learning-modellen
* [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/cntk-on-azure)
* [Deep Learning VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
* [Batch scheepswerf recepten voor deep learning](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)






## <a name="hpc-and-gpu-vm-sizes"></a>HPC en GPU VM-grootten
Azure biedt een scala aan grootten voor [Linux](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [Windows](../articles/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuele machines, inclusief de grootte die is ontworpen voor rekenintensieve workloads. Bijvoorbeeld, kunt H16r en H16mr VM's verbinden met een hoge doorvoer back-end RDMA-netwerk. Deze cloudnetwerk kunt verbeteren de prestaties van nauw gekoppelde parallelle toepassingen die worden uitgevoerd onder [Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) of Intel MPI. 

Virtuele machines uit de N-serie zijn voorzien van NVIDIA GPU's die zijn ontworpen voor rekenintensieve of grafisch-intensieve toepassingen, met inbegrip van kunstmatige intelligentie (AI) learning en visualisatie. 

Meer informatie:

* High performance computing-grootten voor [Linux](../articles/virtual-machines/linux/sizes-hpc.md) en [Windows](../articles/virtual-machines/windows/sizes-hpc.md) VM's 
* Met GPU grootten voor [Linux](../articles/virtual-machines/linux/sizes-gpu.md) en [Windows](../articles/virtual-machines/windows/sizes-gpu.md) VM's 

Leer hoe u het volgende doet:

* [Een Linux RDMA-cluster instellen voor het uitvoeren van MPI-toepassingen](../articles/virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Een Windows RDMA-cluster met Microsoft HPC Pack instellen voor het uitvoeren van MPI-toepassingen](../articles/virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Rekenintensieve VM's in Batch-pools gebruiken](../articles/batch/batch-pool-compute-intensive-sizes.md)



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
* [Helder Clusterbeheer](http://www.brightcomputing.com/technology-partners/microsoft)
* [IBM Spectrum Symphony en Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
* [PBS Pro](http://pbspro.org)
* [Microsoft HPC Pack](https://technet.microsoft.com/library/mt744885.aspx) -opties om uit te voeren [Windows](../articles/virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [Linux](../articles/virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM's 



## <a name="hpc-storage"></a>HPC-opslag

Grootschalige Batch- en HPC-werkbelastingen hebben aanvragen voor opslag van gegevens- en toegangsbeheer die groter zijn dan de mogelijkheden van traditionele cloud-bestandssystemen. Parallelle bestand system-oplossingen, zoals implementeren in Azure [Lustre](http://lustre.org/) en [BeeGFS](http://www.beegfs.com/content/).

Meer informatie:

* [Parallelle virtuele bestandssystemen op Azure](https://azure.microsoft.com/resources/parallel-virtual-file-systems-on-microsoft-azure/)
* Krachtige oplossingen voor cloudopslag van [Avere](http://www.averesystems.com/about-us/about-avere) (nu [samengevoegd met Microsoft](https://blogs.microsoft.com/blog/2018/01/03/microsoft-to-acquire-avere-systems-accelerating-high-performance-computing-innovation-for-media-and-entertainment-industry-and-beyond/))


## <a name="related-azure-services"></a>Gerelateerde Azure Services

Virtuele Azure-machines, virtuele-machineschaalsets, Batch en gerelateerde compute-services vormen de basis van de meeste Azure-HPC-oplossingen. Echter, uw oplossing kan profiteren van veel gerelateerde Azure-services. Hier volgt een gedeeltelijke lijst:

### <a name="storage"></a>Storage

* [BLOB, table en queue storage](../articles/storage/storage-introduction.md)
* [Bestandsopslag](../articles/storage/storage-files-introduction.md)

### <a name="data-and-analytics"></a>Gegevens en analyse
* [HDInsight](../articles/hdinsight/hadoop/apache-hadoop-introduction.md)
* [Data Factory](../articles/data-factory/introduction.md)
* [Data Lake Store](../articles/data-lake-store/data-lake-store-overview.md)
* [Databricks](../articles/azure-databricks/what-is-azure-databricks.md)
* [SQL Database](../articles/sql-database/sql-database-technical-overview.md)

### <a name="ai-and-machine-learning"></a>AI en machine learning
* [Machine Learning-Services](../articles/machine-learning/service/overview-what-is-azure-ml.md)
* [Batch AI](../articles/batch-ai/overview.md)
* [Genomics](../articles/genomics/overview-what-is-genomics.md)

### <a name="networking"></a>Netwerken
* [Virtueel netwerk](../articles/virtual-network/virtual-networks-overview.md)
* [ExpressRoute](../articles/expressroute/expressroute-introduction.md)

### <a name="containers"></a>Containers
* [Container Service](../articles/container-service/dcos-swarm/container-service-intro.md)
* [Azure Kubernetes Service (AKS)](../articles/aks/intro-kubernetes.md)
* [Container Registry](../articles/container-registry/container-registry-intro.md)



## <a name="customer-stories"></a>Verhalen van klanten

Voorbeelden van klanten die zakelijke problemen met Azure HPC-oplossingen hebben opgelost:

* [ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu) 
* [AXA globale P & C](https://customers.microsoft.com/story/axa-global-p-and-c)
* [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
* [d3View](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
* [EFS](https://customers.microsoft.com/story/efs-professionalservices-azure)
* [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
* [MetLife](https://enterprise.microsoft.com/en-us/customer-story/industries/insurance/metlife/)
* [Microsoft Research](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
* [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
* [Mitsubishi UFJ effecten International](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
* [NeuroInitiative](https://customers.microsoft.com/en-us/story/neuroinitiative-health-provider-azure)
* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Towers Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over Big Compute-oplossingen voor [technische simulatie](https://simulation.azure.com/), [rendering](https://azure.microsoft.com/solutions/big-compute/rendering/), [bankwezen en kapitaalmarkten](https://finance.azure.com/), en [genomics](https://enterprise.microsoft.com/en-us/industries/health/genomics/) .
* Bezoek het [teamblog voor Microsoft HPC en Batch](http://blogs.technet.com/b/windowshpc/) en het [Azure-blog](https://azure.microsoft.com/blog/tag/hpc/) voor de meest recente aankondigingen.

* Gebruik de Azure beheerde en schaalbare [Batch](https://azure.microsoft.com/services/batch/) service voor het uitvoeren van rekenintensieve workloads, zonder het beheren van onderliggende infrastructuur [meer informatie](https://azure.microsoft.com/solutions/architecture/hpc-big-compute-saas/)



