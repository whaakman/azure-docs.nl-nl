---
title: Batch- en HPC-oplossingen in de cloud - Azure | Microsoft Docs
description: Meer informatie over batchverwerkings- en High Performance Computing-scenario's (HPC en Big Compute) en oplossingsopties in Azure
services: batch, virtual-machines, cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: aab5401d-2baf-4cf2-bf20-ad224de33888
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: d30c09d23a84200ba81df133c9de5b11910ffb61
ms.contentlocale: nl-nl
ms.lasthandoff: 08/21/2017

---
# <a name="batch-and-hpc-solutions-for-large-scale-computing-workloads"></a>Batch- en HPC-oplossingen voor grootschalige rekenworkloads

Azure biedt efficiënte, schaalbare cloudoplossingen voor batchverwerking en High Performance Computing (HPC), ook wel *Big Compute* genoemd. Ontdek hier meer over Big Compute-workloads en de Azure-services om ze te ondersteunen of ga direct naar de [oplossingscenario's](#scenarios) verderop in dit artikel. Dit artikel is hoofdzakelijk bestemd voor technische besluitvormers, IT-managers en onafhankelijke softwareleveranciers, maar ook andere IT-professionals en ontwikkelaars kunnen het gebruiken om vertrouwd te raken met deze oplossingen.

Organisaties kampen met grootschalige rekenproblemen: technische ontwerpen en analyses, opbouwen van afbeeldingen, complexe modellering, Monte Carlo-simulaties, berekeningen van financiële risico's enz. Azure helpt organisaties deze problemen op te lossen met de benodigde resources, schaal en planning. Met Azure kunnen organisaties:

* Hybride oplossingen maken, een on-premises HPC-cluster uitbreiden om piekworkloads naar de cloud te offloaden
* HPC-clusterhulpprogramma's en workloads volledig in Azure uitvoeren
* Beheerde en schaalbare Azure-services zoals [Batch](https://azure.microsoft.com/documentation/services/batch/) gebruiken om rekenintensieve workloads uit te voeren zonder rekeninfrastructuur te implementeren en te beheren

Hoewel dit buiten het bereik van dit artikel valt, biedt Azure ontwikkelaars en partners ook een volledige set met mogelijkheden, architectuurkeuzen en hulpprogramma's voor ontwikkeling, om grootschalige, aangepaste Big Compute-werkstromen te bouwen. Bovendien staat er een groeiend ecosysteem voor partners klaar om u te helpen uw Big Compute-workloads productief te maken in de Azure-cloud.

## <a name="batch-and-hpc-applications"></a>Batch- en HPC-toepassingen
In tegenstelling tot webtoepassingen en veel Line-Of-Business-toepassingen, hebben batch- en HPC-toepassingen een gedefinieerd begin en einde en kunnen ze volgens een planning of op aanvraag worden uitgevoerd, urenlang of zelfs nog langer. De meeste kunnen worden onderverdeeld in twee hoofdcategorieën: *intrinsiek parallel* (ook wel 'perfect parallel' genoemd, omdat de problemen die deze toepassingen oplossen evengoed parallel op meerdere computers of processoren kunnen worden uitgevoerd) en *nauw gekoppeld*. De volgende tabel biedt meer informatie over deze toepassingstypen. Sommige Azure-benaderingen voor oplossingen werken beter voor het ene type dan voor het andere.

> [!NOTE]
> In Batch- en HPC-oplossingen wordt een actief exemplaar van een toepassing doorgaans een *job* genoemd en kan elke job onderverdeeld worden in *taken*. De geclusterde rekenresources voor de toepassing worden vaak *rekenknooppunten* genoemd.
> 
> 

| Type | Kenmerken | Voorbeelden |
| --- | --- | --- |
| **Intrinsically parallel**<br/><br/>![Intrinsically parallel][parallel] |• Afzonderlijke computers voeren toepassingslogica onafhankelijk uit<br/><br/> • Door computers toe te voegen, kan de toepassing geschaald worden vergroot en kan de rekentijd worden ingekort<br/><br/>• Toepassing bestaat uit afzonderlijke uitvoerbare bestanden of is onderverdeeld in een groep services die worden aangeroepen door een client (een Service Oriented Architecture (SOA), toepassing) |• Modellering van financiële risico’s<br/><br/>• Rendering van afbeeldingen en beeldverwerking<br/><br/>• Mediacodering en -transcodering<br/><br/>• Monte Carlo-simulaties<br/><br/>• Softwaretests |
| **Nauw gekoppeld**<br/><br/>![Nauw gekoppeld][coupled] |• Toepassing vereist rekenknooppunten om te communiceren of tussenliggende resultaten uit te wisselen<br/><br/>• Rekenknooppunten kunnen communiceren met de Message Passing Interface (MPI), een gemeenschappelijk communicatieprotocol voor parallelle berekeningen<br/><br/>• De toepassing is gevoelig voor netwerklatentie en bandbreedte<br/><br/>• Toepassingsprestaties kunnen worden verbeterd door gebruik te maken van snelle netwerktechnologieën zoals InfiniBand en Remote Direct Memory Access (RDMA) |• Modellering van olie- en gasreservoirs<br/><br/>• Technische ontwerpen en analyses, zoals Computational Fluid Dynamics<br/><br/>• Fysieke simulaties zoals auto-ongevallen en nucleaire reacties<br/><br/>• Weersvoorspelling |

### <a name="considerations-for-running-batch-and-hpc-applications-in-the-cloud"></a>Overwegingen voor het uitvoeren van Batch- en HPC-toepassingen in de cloud
Vele toepassingen die zijn ontworpen om in on-premises HPC-clusters te worden uitgevoerd, kunt u gemakkelijk migreren naar Azure of naar een hybride (cross-premises) omgeving. Er kunnen wel enkele beperkingen of overwegingen zijn, zoals:

* **Beschikbaarheid van cloudresources**: afhankelijk van het type cloudrekenresources dat u gebruikt, kunt u mogelijk niet vertrouwen op de continue beschikbaarheid van computers wanneer een taak wordt uitgevoerd. Statusafhandeling en voortgangscontrolepunten zijn gangbare technieken om mogelijke tijdelijke fouten af te handelen, en zijn vooral nodig bij het gebruik van cloudresources.
* **Gegevenstoegang**: technieken voor gegevenstoegang die doorgaans beschikbaar zijn in Enterprise-clusters, zoals NFS, moet mogelijk een speciale configuratie in de cloud plaatsvinden. Het kan ook nodig zijn om verschillende praktijken en patronen voor gegevenstoegang in gebruik te nemen voor de cloud.
* **Gegevensverplaatsing**: voor toepassingen die grote hoeveelheden gegevens verwerken, zijn strategieën nodig om de gegevens naar de cloudopslag te verplaatsen en om resources te berekenen. Mogelijk hebt u ook snelle cross-premises netwerkmogelijkheden nodig, zoals die van [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Houd ook rekening met juridische, regelgevende of beleidsbeperkingen voor de opslag van en toegang tot die gegevens.
* **Licentieverlening** - Neem contact op met de leverancier van een commerciële toepassing, voor licentieverlening of andere beperkingen voor het uitvoeren van die toepassing in de cloud. Niet alle leveranciers bieden licenties waarbij u betaalt naar gebruik. Mogelijk moet u voor uw oplossing een licentieserver in de cloud plannen of verbinding maken met een on-premises licentieserver.

### <a name="big-compute-or-big-data"></a>Big Compute of Big Data?
De scheidingslijn tussen Big Compute- en Big Data-toepassingen is niet altijd duidelijk en sommige toepassingen kunnen kenmerken van beide typen hebben. Beide toepassingen maken gebruik van grootschalige berekeningen, die doorgaans op clusters of computers worden uitgevoerd. De benaderingen en ondersteunende hulpprogramma's van de oplossing kunnen echter verschillen.

• Bij **Big Compute** zijn doorgaans toepassingen betrokken die afhankelijk zijn van CPU-kracht en geheugen, zoals technische simulaties, modellering van financiële risico's en digitale rendering. De infrastructuur voor een Big Compute-oplossing kan computers met gespecialiseerde multicore-processoren bevatten om grote berekeningen uit te voeren en gespecialiseerde, snelle netwerkhardware om de computers te verbinden.

• **Big data**-toepassingen lossen problemen met gegevensanalyse op waarbij grote hoeveelheden gegevens betrokken zijn die niet door één computer of databasebeheersysteem kunnen worden beheerd. Dit zijn bijvoorbeeld grote volumes van weblogboeken of andere business intelligence-gegevens. Big data zijn meestal in hogere mate afhankelijk van de schijfcapaciteit en I/O-prestaties dan van de CPU-capaciteit. Er zijn ook speciale big data-hulpprogramma's zoals Apache Hadoop voor het beheer van het cluster en de partitie van de gegevens. (Zie [Hadoop](https://azure.microsoft.com/solutions/hadoop/) voor meer informatie over Azure HDInsight en andere Azure Hadoop-oplossingen.)

## <a name="compute-management-and-job-scheduling"></a>Berekeningsbeheer en jobplanning
Bij het uitvoeren van Batch- en HPC-toepassingen wordt vaak gebruikgemaakt van *clusterbeheer* en een *jobplanner* om geclusterde rekenresources te helpen beheren en ze toe te wijzen aan de toepassingen die de jobs uitvoeren. Deze functies kunnen worden uitgevoerd door verschillende hulpprogramma's of door een geïntegreerd hulpprogramma of geïntegreerde service.

* **Clusterbeheer** - Staat in voor het inrichten, vrijgeven en beheren van rekenresources (of rekenknooppunten). Clusterbeheer kan de installatie van installatiekopieën van besturingssystemen en toepassingen in rekenknooppunten automatiseren, rekenresources geschaald uitbreiden op basis van behoeften, evenals de prestaties van de knooppunten controleren.
* **Taakplanner**: geeft aan welke resources (zoals processoren of geheugen) voor een toepassing zijn vereist, evenals de voorwaarden waaronder deze wordt uitgevoerd. Een jobplanner houdt een wachtrij van taken bij en wijst er resources aan toe op basis van een toegewezen prioriteit of andere kenmerken.

Hulpprogramma's voor clustering en jobplanning voor Windows- en Linux-clusters kunnen uitstekend naar Azure migreren. Zo biedt bijvoorbeeld [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), de gratis rekenclusteroplossing van Microsoft voor HPC-workloads in Windows en Linux, verschillende opties voor het uitvoeren in Azure. U kunt ook Linux-clusters bouwen om opensourcehulpprogramma's zoals Torsie en SLURM uit te voeren. U kunt ook commerciële rasteroplossingen overbrengen naar Azure, zoals [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/), [IBM Spectrum Symphony en Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/) en [Univa Grid Engine](http://www.univa.com/products/grid-engine).

Zoals in de volgende secties is aangegeven, kunt u ook van Azure-services profiteren om rekenresources te beheren en jobs te plannen zonder (of als aanvulling op) traditionele hulpprogramma's voor clusterbeheer.

## <a name="scenarios"></a>Scenario's
Hieronder vindt u drie gangbare scenario's om Big Compute-workloads in Azure uit te voeren door gebruik te maken van bestaande HPC-clusteroplossingen, Azure-services of een combinatie hiervan. Voor de keuze van elk scenario zijn belangrijke, maar mogelijk niet alle aandachtspunten vermeld. Meer informatie over de beschikbare Azure-services die u in uw oplossing kunt gebruiken, vindt u verderop in het artikel.

| Scenario | Waarom voor dit scenario kiezen? |
| --- | --- | --- |
| **Een HPC-cluster uitbreiden naar Azure**<br/><br/>[![Clusteruitbreiding][burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Meer informatie:<br/>• [Burst to Azure worker instances with HPC Pack](https://technet.microsoft.com/library/gg481749.aspx) (Uitbreiden naar Azure-werkrolexemplaren met HPC Pack)<br/><br/>• [Set up a hybrid compute cluster with HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) (Een hybride rekencluster met HPC Pack instellen)<br/><br/>• [Burst to Azure Batch with HPC Pack](https://technet.microsoft.com/library/mt612877.aspx) (Uitbreiden naar Azure Batch met HPC Pack)<br/><br/> |• Combineer uw [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) of een ander on-premises cluster met extra Azure-resources in een hybride oplossing.<br/><br/>• Breid uw Big Compute-workloads uit voor uitvoering op Platform as a Service (PaaS) VM-instanties (momenteel alleen Windows Server).<br/><br/>• Krijg toegang tot een on-premises licentieserver of gegevensopslag met behulp van een optioneel virtueel Azure-netwerk |
| **Een HPC-cluster volledig in Azure maken**<br/><br/>[![Cluster in IaaS][iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Meer informatie:<br/>• [HPC cluster solutions in Azure](big-compute-resources.md) (HPC-clusteroplossingen in Azure)<br/><br/> |• Implementeer uw toepassingen en clusterhulpprogramma's snel en consistent op standaard of aangepaste Infrastructure as a service (IaaS) virtuele machines met Windows of Linux.<br/><br/>• Voer uiteenlopende Big Compute-workloads uit met behulp van de oplossing van uw keuze voor taakplanning.<br/><br/>• Gebruik extra Azure-services, met inbegrip van netwerken en opslag, om volledige cloudoplossingen te maken. |
| **Een parallelle toepassing geschaald uitbreiden naar Azure**<br/><br/>[![Azure Batch][batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Meer informatie:<br/>• [Basics of Azure Batch](batch-technical-overview.md) (Basisbeginselen van Azure Batch)<br/><br/>• [Aan de slag met de Azure Batch-bibliotheek voor .NET](batch-dotnet-get-started.md) |• Ontwikkel met [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) om diverse Big Compute-workloads uit te schalen voor uitvoering in pools van virtuele machines met Windows of Linux.<br/><br/>• Gebruik een Azure-platformservice voor het beheer van de implementatie en automatische geschaalde uitbreiding van virtuele machines, taakplanning, herstel na noodgevallen, gegevensverplaatsing, afhankelijkheidsbeheer en toepassingsimplementatie. |

## <a name="azure-services-for-big-compute"></a>Azure-services voor Big Compute
Hier vindt u meer informatie over de compute-, gegevens- en netwerkservices en verwante services die u kunt combineren voor Big Compute-oplossingen en -werkstromen. Raadpleeg de [documentatie](https://azure.microsoft.com/documentation/) van de Azure-services voor meer informatie over Azure-services. De [scenario's](#scenarios) eerder in dit artikel tonen slechts enkele manieren waarop u deze services kunt gebruiken.

> [!NOTE]
> Azure introduceert regelmatig nieuwe services die voor uw scenario nuttig kunnen zijn. Als u vragen hebt, neemt u contact op met een [Azure-partner](https://pinpoint.microsoft.com/en-US/search?keyword=azure) of stuurt u een e-mail naar *bigcompute@microsoft.com*.
> 
> 

### <a name="compute-services"></a>Compute services
Compute Services van Azure vormen de kern van een Big Compute-oplossing en de andere Compute Services bieden voordelen voor andere scenario's. Op basisniveau bieden deze services verschillende modi voor toepassingen om op VM-rekeninstanties te worden uitgevoerd die Azure verstrekt met behulp van Windows Server Hyper-V-technologie. Op deze instanties kunnen standaard en aangepaste Linux- en Windows-besturingssystemen en hulpprogramma's worden uitgevoerd. Azure biedt u een keuze uit [instantiegrootten](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) met verschillende configuraties van CPU-kernen, geheugen, schijfcapaciteit en andere kenmerken. Afhankelijk van uw behoeften kunt u de instanties opschalen naar duizenden kernen en daarna omlaag schalen wanneer u minder bronnen nodig hebt.

> [!NOTE]
> Gebruik de [rekenintensieve instanties zoals de H-serie](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) van Azure om de prestaties en schaalbaarheid van HPC-workloads te verbeteren. Deze instanties ondersteunen ook parallelle MPI-toepassingen die een toepassingsnetwerk met een lage latentie en hoge doorvoer vereisen. Er zijn ook virtuele machines uit de [N-serie](../virtual-machines/windows/sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) met NVIDIA-GPU's beschikbaar om het aantal reken- en visualisatiescenario's in Azure te vergroten.  
> 
> 

| Service | Beschrijving |
| --- | --- |
| **[Virtuele machines](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Bieden Infrastructure as a Service (IaaS) voor berekeningen met behulp van Microsoft Hyper-V-technologie<br/><br/>• Bieden u de mogelijkheid om permanente cloudcomputers flexibel in te richten en te beheren vanuit standaardinstallatiekopieën met Windows Server of Linux vanuit [Azure Marketplace](https://azure.microsoft.com/marketplace/) of vanuit installatiekopieën en gegevensschijven die u opgeeft<br/><br/>• Kunnen worden geïmplementeerd en beheerd als [VM-schaalsets](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) voor het bouwen van grootschalige services vanaf identieke virtuele machines, met automatisch schalen om de capaciteit automatisch te vergroten of te verkleinen<br/><br/>• Voeren on-premises rekenclusterhulpprogramma's en toepassingen volledig uit in de cloud<br/><br/> |
| **[Cloudservices](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Kunnen Big Compute-toepassingen uitvoeren in werkrolinstanties (virtuele machines met een versie van Windows Server en die volledig door Azure worden beheerd)<br/><br/>• Maken schaalbare, betrouwbare toepassingen mogelijk met lage administratieve overhead, die worden uitgevoerd in een Platform as a service (PaaS)-model<br/><br/>• Vereisen mogelijk extra hulpprogramma's of extra ontwikkeling voor de integratie met on-premises HPC-clusteroplossingen |
| **[Batch](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Voert grootschalige parallelle workloads en batchworkloads uit in een volledig beheerde service<br/><br/>• Biedt jobplanning en automatisch schalen van een beheerde pool van virtuele machines<br/><br/>• Biedt ontwikkelaars de mogelijkheid om toepassingen te bouwen en uit te voeren als een service of om bestaande toepassingen in te schakelen voor de cloud<br/> |

### <a name="storage-services"></a>Opslagservices
Een Big Compute-oplossing werkt doorgaans met een reeks invoergegevens en genereert gegevens voor de resultaten. Enkele van de Azure-opslagservices die in Big Compute-oplossingen worden gebruikt, zijn:

* [Blob Storage, Table Storage en Queue Storage](https://azure.microsoft.com/documentation/services/storage/) - Beheer respectievelijk grote hoeveelheden ongestructureerde gegevens, NoSQL-gegevens en berichten voor werkstromen en communicatie. U kunt bijvoorbeeld Blob Storage gebruiken voor grote sets met technische gegevens, of voor de installatiekopieën of mediabestanden van de invoer die uw toepassing verwerkt. U kunt wachtrijen gebruiken voor asynchrone communicatie in een oplossing. Zie [Inleiding tot Microsoft Azure Storage](../storage/common/storage-introduction.md).
* [Azure File Storage](https://azure.microsoft.com/services/storage/files/) - Deelt algemene bestanden en gegevens in Azure met behulp van het standaard SMB-protocol, dat nodig is voor een aantal HPC-clusteroplossingen.
* [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/): biedt een grootschalig Apache Hadoop Distributed File System voor de cloud, dat nuttig is voor batch-, realtime- en interactieve analyses.

### <a name="data-and-analysis-services"></a>Gegevens- en analyseservices
Bij sommige Big Compute-scenario's zijn grootschalige gegevensstromen betrokken of worden gegevens gegenereerd die verder moeten worden verwerkt of geanalyseerd. Azure biedt verschillende gegevens- en analyseservices, waaronder:

* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) - Bouwt gegevensgestuurde werkstromen (pijplijnen) die gegevens uit on-premises gegevensopslag en cloud- en internetgegevensopslag samenvoegen, aggregeren en transformeren.
* [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/) - Biedt de belangrijkste functies van een relationeel Microsoft SQL Server-databasebeheersysteem in een beheerde service.
* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) - Implementeert en richt Windows Server- of Linux-gebaseerde Apache Hadoop-clusters in de cloud in om big data te beheren en analyseren en erover te rapporteren.
* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) - Helpt u bij het maken, testen, gebruiken en beheren van voorspellende analytische oplossingen in een volledig beheerde service.

### <a name="additional-services"></a>Extra services
Uw Big Compute-oplossing heeft mogelijk andere Azure-services nodig om verbinding te maken met on-premises resources of resources in andere omgevingen. Voorbeelden zijn:

* [Virtueel netwerk](https://azure.microsoft.com/documentation/services/virtual-network/): hiermee maakt u een logisch geïsoleerde sectie in Azure om Azure-resources met elkaar of met uw on-premises datacenter te verbinden. Met een virtueel cross-premises netwerk hebben Big Compute-toepassingen toegang tot on-premises gegevens, Active Directory-services en licentieservers
* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/): hiermee maakt u een privéverbinding tussen Microsoft-datacenters en -infrastructuren on-premises of in een co-locatieomgeving. ExpressRoute maakt ze veiliger, betrouwbaarder en sneller, en geeft ze lagere latenties en een betere beveiliging dan gewone verbindingen via internet.
* [Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) - Biedt verschillende mechanismen voor toepassingen om te communiceren of gegevens uit te wisselen, ongeacht of ze zich bevinden in Azure, op een ander cloudplatform of in een datacenter.

## <a name="next-steps"></a>Volgende stappen
* Zie [Technical Resources for Batch and HPC](big-compute-resources.md) (Technische bronnen voor Batch en HPC) voor technische richtlijnen voor het bouwen van uw oplossing.
* Bespreek uw Azure-opties met partners, zoals Cycle Computing, Rescale en UberCloud.
* Lees meer over Big Compute-oplossingen voor Azure die worden geleverd door [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/), [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/) en [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).
* Bezoek het [teamblog voor Microsoft HPC en Batch](http://blogs.technet.com/b/windowshpc/) en het [Azure-blog](https://azure.microsoft.com/blog/tag/hpc/) voor de meest recente aankondigingen.

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png

