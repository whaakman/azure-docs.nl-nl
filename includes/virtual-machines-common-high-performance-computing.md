Organisaties hebben grootschalige IT-behoeften. Deze Big Compute-workloads bevatten technische ontwerpen en analyses, berekeningen van financiële risico's, rendering van afbeeldingen, complexe modellering, Monte Carlo-simulaties en meer. 

Gebruik de Azure-cloud rekenintensieve Linux- en Windows workloads efficiënt vanaf parallelle batchtaken voor traditionele HPC-simulaties uitvoeren. Voer uw HPC en batch-workloads in Azure-infrastructuur, met van uw keuze compute services, raster managers Marketplace-oplossingen en toepassingen met de leverancier van host (SaaS). Azure biedt flexibele oplossingen voor het distribueren van werk en kan worden uitgebreid naar duizenden virtuele machines of kernen en daarna geschaald verkleinen wanneer u minder bronnen nodig. 



## <a name="solution-options"></a>Oplossingsopties voor de



* **Doe oplossingen**
    * Instellen van uw eigen clusteromgeving in Azure virtuele machines of [virtuele-machineschaalsets](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 
    * Lift- en verplaatsen van een on-premises-cluster of een nieuw cluster in Azure voor extra capaciteit te implementeren. 
    * Azure Resource Manager-sjablonen gebruiken voor het implementeren van voorloopspaties [werkbelasting managers](#workload-managers), infrastructuur en [toepassingen](#hpc-applications). 
    * Kies [HPC en GPU VM-grootten](#hpc-and-gpu-sizes) die gespecialiseerde hardware- en netwerkverbindingen voor MPI of GPU werkbelastingen bevatten. 
    * Voeg [krachtige opslag](#hpc-storage) voor I/O-intensieve werkbelastingen.
* **Hybride oplossingen**
    * Uw on-premises-oplossing voor het offloaden ('burst') piekworkloads naar Azure-infrastructuur uitbreiden
    * Cloud compute op de vraag te gebruiken met uw bestaande [van workload manager](#workload-manager).
    * Profiteren van [HPC en GPU VM-grootten](#hpc-and-gpu-sizes) voor MPI of GPU werkbelastingen.
* **BIG Compute-oplossingen als een service**
    * Aangepaste Big Compute-oplossingen en werkstromen die gebruikmaken van ontwikkelen [Azure Batch](#azure-batch) en verwante [Azure-services](#related-azure-services).
    * Engineering en simulatie Azure-oplossingen uitvoeren vanaf leveranciers zoals [Altair](http://www.altair.com/), [oorspronkelijke](https://www.rescale.com/azure/), en [Cycle Computing](https://cyclecomputing.com/) (nu [samengevoegd met Microsoft](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/)).
* **Marketplace-oplossingen**
    * De schaal van [HPC-toepassingen](#hpc-applications) en [oplossingen](#marketplace-solutions) aangeboden in de [Azure Marketplace](https://azuremarketplace.microsoft.com/). 
    


De volgende secties bevatten meer informatie over de ondersteunende technologieën en koppelingen naar instructies.



## <a name="marketplace-solutions"></a>Marketplace-oplossingen

Ga naar de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) voor Linux en Windows-VM-installatiekopieën en die zijn bestemd voor HPC-oplossingen. Voorbeelden zijn:

* [RogueWave op basis van CentOS HPC](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
* [SUSE Linux Enterprise Server voor HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
*  [TIBCO raster-Engine van Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
* [Azure voor Gegevenswetenschap VM voor Windows en Linux](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md)
* [D3View](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
* [UberCloud](https://azure.microsoft.com/search/marketplace/?q=ubercloud)
* [Intel Cloud Edition voor Lustre](https://azuremarketplace.microsoft.com/marketplace/apps/intel.lustre-cloud-edition-eval?tab=Overview)


 
## <a name="hpc-applications"></a>HPC-toepassingen

Aangepaste of commerciële HPC-toepassingen worden uitgevoerd in Azure. Enkele voorbeelden in deze sectie zijn benchmarked om efficiënt schalen met aanvullende virtuele machines of compute kernen. Ga naar de [Azure Marketplace](https://marketplace.azure.com) voor oplossingen gereed te implementeren.

> [!NOTE]
> Neem contact op met de leverancier van een commerciële toepassing voor licentieverlening of andere beperkingen voor het uitvoeren in de cloud. Niet alle leveranciers bieden licenties waarbij u betaalt naar gebruik. U mogelijk nodig hebt voor uw oplossing een licentieserver in de cloud of verbinding maken met een on-premises licentie-server.

### <a name="engineering-applications"></a>Engineering-toepassingen


* [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
* [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
* [MATLAB gedistribueerde Computing Server](../articles/virtual-machines/windows/matlab-mdcs-cluster.md)
* [StarCCM +](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
* [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)



### <a name="graphics-and-rendering"></a>Afbeeldingen en rendering

* [Autodesk Maya, 3ds Max en Arnold](../articles/batch/batch-rendering-service.md) op Azure Batch (preview)

### <a name="ai-and-deep-learning"></a>AI en grondige leren

* [Batch-AI](../articles/batch-ai/overview.md) training voor grondige learning-modellen
* [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/cntk-on-azure)
* [Deep Learning VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
* [Batch scheepswerf recepten voor grondige learning](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)






## <a name="hpc-and-gpu-vm-sizes"></a>HPC en GPU VM-grootten
Azure biedt een reeks grootten voor [Linux](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) en [Windows](../articles/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuele machines, inclusief de grootte die zijn ontworpen voor rekenintensieve workloads. Bijvoorbeeld, H16r en H16mr virtuele machines verbinding kunnen maken met een hoge doorvoersnelheid back-end RDMA-netwerk. Dit netwerk cloud kunt verbeteren de prestaties van nauw gekoppeld parallelle toepassingen die worden uitgevoerd onder [Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) of Intel MPI. 

N-serie VMs functie NVIDIA GPU's die zijn ontworpen voor rekenintensieve of grafisch-intensieve toepassingen, waaronder kunstmatige intelligence (AI) learning en visualisatie. 

Meer informatie:

* Hoge prestaties compute grootten voor [Linux](../articles/virtual-machines/linux/sizes-hpc.md) en [Windows](../articles/virtual-machines/windows/sizes-hpc.md) virtuele machines 
* Grootten GPU-functionaliteit voor [Linux](../articles/virtual-machines/linux/sizes-gpu.md) en [Windows](../articles/virtual-machines/windows/sizes-gpu.md) virtuele machines 

Leer hoe u het volgende doet:

* [Instellen van een cluster Linux RDMA MPI-toepassingen uitvoeren](../articles/virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Instellen van een cluster met Windows RDMA met Microsoft HPC Pack MPI-toepassingen uitvoeren](../articles/virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Rekenintensieve virtuele machines in de Batch-pools gebruiken](../articles/batch/batch-pool-compute-intensive-sizes.md)



## <a name="azure-batch"></a>Azure Batch
[Batch](../articles/batch/batch-technical-overview.md) is een platform-service voor het uitvoeren van grootschalige parallelle en high-performance computing (HPC) toepassingen efficiënt in de cloud. Azure Batch planningen rekenintensieve werken om te worden uitgevoerd op een beheerde pool van virtuele machines, en kan automatisch het aantal rekenresources om te voldoen aan de behoeften van uw taken. 

Aanbieders van SaaS- of ontwikkelaars HPC-toepassingen of werkbelastingen van de container integreren met Azure, fase gegevens naar Azure, met de Batch-SDK's en hulpprogramma's en taak uitvoering pijplijnen bouwen. 

Leer hoe u het volgende doet:

* [Te beginnen met ontwikkelen met Batch](../articles/batch/batch-dotnet-get-started.md)
* [Gebruik van Azure Batch-codevoorbeelden](https://github.com/Azure/azure-batch-samples)
* [Met lage prioriteit virtuele machines, met Batch](../articles/batch/batch-low-pri-vms.md)
* [Beperkte HPC-workloads uitvoeren met Batch scheepswerf](https://github.com/Azure/batch-shipyard)
* [Gebruik de taal R met Batch](https://github.com/Azure/doAzureParallel)

## <a name="workload-managers"></a>Werkbelasting managers

Hier volgen enkele voorbeelden van het cluster en de belasting van managers die kunnen worden uitgevoerd in Azure-infrastructuur. Zelfstandige clusters maken in Azure Virtual machines of burst virtuele Azure-machines van een on-premises-cluster. 
* [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/) 
* [Helder Clusterbeheer](http://www.brightcomputing.com/technology-partners/microsoft)
* [IBM Spectrum Symphony en Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
* [PBS Pro](http://pbspro.org)
* [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029(v=ws.11).aspx) -opties in te voeren ziet [Windows](../articles/virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [Linux](../articles/virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) virtuele machines 



## <a name="hpc-storage"></a>HPC-opslag

Grootschalige Batch- en HPC-workloads hebben vaak voor opslag van gegevens en de toegang die groter is dan de mogelijkheden van traditionele cloud-bestandssystemen. Zoals parallel file system-oplossingen in Azure implementeren [Lustre](http://lustre.org/) en [BeeGFS](http://www.beegfs.com/content/).

Meer informatie:

* [Parallelle bestandssystemen voor HPC-opslag in Azure](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/)


## <a name="related-azure-services"></a>Gerelateerde Azure Services

Azure virtuele machines, virtuele-machineschaalsets Batch en verwante compute services vormen de basis van de meeste Azure HPC-oplossingen. Echter kunt uw oplossing profiteren van veel gerelateerde Azure-services. Hier volgt een gedeeltelijke lijst:

### <a name="storage"></a>Storage

* [BLOB, table en queue storage](../articles/storage/storage-introduction.md)
* [Opslag van bestanden](../articles/storage/storage-files-introduction.md)

### <a name="data-and-analytics"></a>Gegevens en analyse
* [HDInsight](../articles/hdinsight/hadoop/apache-hadoop-introduction.md) voor Hadoop-clusters op Azure
* [Data Factory](../articles/data-factory/introduction.md)
* [Data Lake Store](../articles/data-lake-store/data-lake-store-overview.md)
* [Machine Learning](../articles/machine-learning/machine-learning-what-is-machine-learning.md)
* [SQL Database](../articles/sql-database/sql-database-technical-overview.md)

### <a name="networking"></a>Netwerken
* [Virtueel netwerk](../articles/virtual-network/virtual-networks-overview.md)
* [ExpressRoute](../articles/expressroute/expressroute-introduction.md)

### <a name="containers"></a>Containers
* [Container Service](../articles/container-service/dcos-swarm/container-service-intro.md)
* [Container Registry](../articles/container-registry/container-registry-intro.md)



## <a name="customer-stories"></a>Verhalen van klanten

Hier volgen enkele voorbeelden van klanten die zakelijke problemen met Azure HPC-oplossingen hebt opgelost:

* [ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu) 
* [AXA globale P & C](https://customers.microsoft.com/story/axa-global-p-and-c)
* [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
* [d3View](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
* [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
* [MetLife](https://enterprise.microsoft.com/en-us/customer-story/industries/insurance/metlife/)
* [Microsoft Research](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
* [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
* [Mitsubishi UFJ effecten International](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Towers Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over Big Compute-oplossingen voor [simulatie engineering](https://simulation.azure.com/), [rendering](https://simulation.azure.com/), [bank- en investering markten](https://finance.azure.com/), en [genomica](https://enterprise.microsoft.com/en-us/industries/health/genomics/) .
* Bezoek het [teamblog voor Microsoft HPC en Batch](http://blogs.technet.com/b/windowshpc/) en het [Azure-blog](https://azure.microsoft.com/blog/tag/hpc/) voor de meest recente aankondigingen.

* Gebruik de beheerde en schaalbare Azure [Batch](https://azure.microsoft.com/services/batch/) service rekenintensieve workloads uit zonder de onderliggende infrastructuur beheren [meer informatie](https://azure.microsoft.com/en-us/solutions/architecture/hpc-big-compute-saas/)



