---
title: Instellen van een cluster met Windows RDMA MPI-toepassingen uitvoeren | Microsoft Docs
description: Informatie over het maken van een cluster met Windows HPC Pack met een grootte H16r, H16mr, A8 of A9-VM's met het netwerk van Azure RDMA MPI-apps uitvoeren.
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 7d9f5bc8-012f-48dd-b290-db81c7592215
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 19be1d693fe13af0f6c1ab0cb6f7bc829b9fad5a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Instellen van een cluster met Windows RDMA met HPC Pack MPI-toepassingen uitvoeren
Instellen van een Windows-RDMA-cluster in Azure met [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) en [hoge prestaties compute-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) parallelle Message Passing Interface (MPI)-toepassingen uit te voeren. Wanneer u RDMA-functionaliteit, Windows Server gebaseerde knooppunten in een cluster HPC Pack instelt, wordt MPI-toepassingen efficiënt communiceren via een lage latentie en hoge doorvoersnelheid netwerk in Azure die is gebaseerd op remote direct memory access (RDMA)-technologie.

Als u uitvoeren van MPI-werkbelastingen op Linux-virtuele machines die toegang tot het netwerk van Azure RDMA wilt, Zie [instellen van een cluster Linux RDMA MPI-toepassingen uitvoeren](../../linux/classic/rdma-cluster.md).

## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack cluster implementatieopties
Microsoft HPC Pack is een hulpprogramma opgegeven zonder extra kosten maken HPC-clusters lokaal of in Azure Windows of Linux HPC-toepassingen uitvoeren. HPC Pack bevat een runtime-omgeving voor de Microsoft-implementatie van het bericht wordt doorgegeven Interface voor Windows (MS-MPI). Wanneer met RDMA-compatibel is met een ondersteund besturingssysteem voor Windows Server-exemplaren gebruikt, biedt HPC Pack een efficiënte optie Windows MPI-toepassingen die toegang het netwerk van Azure RDMA tot uit te voeren. 

Dit artikel bevat twee scenario's en koppelingen naar gedetailleerde richtlijnen voor het instellen van een cluster met Windows RDMA met Microsoft HPC Pack. 

* Scenario 1. Rekenintensieve worker rolexemplaren (PaaS) implementeren
* Scenario 2. Implementeer de rekenknooppunten in rekenintensieve VM's (IaaS)

Raadpleeg voor algemene vereisten voor het gebruik van rekenintensieve exemplaren met Windows [hoge prestaties compute-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Scenario 1: Rekenintensieve worker rolexemplaren (PaaS) implementeren
Toevoegen van een bestaand cluster HPC Pack, extra rekenresources in Azure worker-rolexemplaren (Azure knooppunten) uitgevoerd in een cloudservice (PaaS). Deze functie wordt ook wel 'burst naar Azure' van HPC Pack ondersteunt een aantal formaten voor de worker-rolexemplaren. Wanneer u de Azure knooppunten toevoegt, Geef een van de grootte van de RDMA-functionaliteit.

Hieronder vindt u overwegingen en stappen voor het burst naar RDMA-compatibele Azure-exemplaren van een bestaand (meestal on-premises) cluster. Gebruik vergelijkbare procedures exemplaren voor worker-rol toevoegen aan een HPC Pack hoofdknooppunt dat in een Azure VM is geïmplementeerd.

> [!NOTE]
> Zie voor een zelfstudie voor burst naar Azure met HPC Pack [een hybride-cluster met HPC Pack instellen](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Let op de overwegingen in de volgende stappen die van toepassing zijn specifiek voor RDMA-compatibele Azure knooppunten.
> 
> 

![Burst naar Azure][burst]

### <a name="steps"></a>Stappen
1. **Implementeer en configureer een hoofdknooppunt HPC Pack 2012 R2**
   
    Download de meest recente installatiepakket van HPC Pack uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Zie voor vereisten en instructies voor het voorbereiden voor de implementatie van een Azure burst [Burst naar Azure Worker-exemplaren met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).
2. **Een beheercertificaat in het Azure-abonnement configureren**
   
    Een certificaat voor het beveiligen van de verbinding tussen het hoofdknooppunt en Azure configureren. Zie voor opties en -procedures, [scenario's voor het configureren van het Azure-Beheercertificaat voor HPC Pack](http://technet.microsoft.com/library/gg481759.aspx). HPC Pack installeert voor testimplementaties een standaard Microsoft HPC Azure-Beheercertificaat u snel naar uw Azure-abonnement uploaden kunt.
3. **Een nieuwe cloudservice en een opslagaccount maken**
   
    De Azure portal gebruiken voor het maken van een cloudservice en een opslagaccount voor de implementatie in een regio waar de RDMA-compatibele exemplaren beschikbaar zijn.
4. **Een op Azure-knooppuntsjabloon maken**
   
    Gebruik de Wizard knooppunt maken in HPC Cluster Manager. Zie voor stappen [maken van een knooppuntsjabloon Azure](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) in 'Stappen voor implementatie van Azure knooppunten met Microsoft HPC Pack'.
   
    Voor de eerste tests, is het raadzaam een handmatige beschikbaarheid-beleid in de sjabloon te configureren.
5. **Knooppunten toevoegen aan het cluster**
   
    Gebruik de Wizard knooppunten toevoegen in HPC Cluster Manager. Zie voor meer informatie [Azure knooppunten toevoegen aan het Windows HPC-Cluster](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).
   
    Bij het opgeven van de grootte van de knooppunten, selecteer een van de grootte van de RDMA-compatibele exemplaar.
   
   > [!NOTE]
   > In elke burst naar Azure-implementatie met de exemplaren rekenintensieve HPC Pack automatisch wordt geïmplementeerd ten minste twee exemplaren van RDMA-functionaliteit (zoals A8) als proxy-knooppunten, naast de Azure worker-rolexemplaren die u opgeeft. De proxy-knooppunten kernen die zijn toegewezen aan het abonnement en kosten samen met de exemplaren van de functie Azure worker gebruiken.
   > 
   > 
6. **(Ingericht) start de knooppunten te brengen en online uitvoeren van taken**
   
    Selecteer de knooppunten en gebruik de **Start** actie in HPC Cluster Manager. Wanneer het inrichten is voltooid, selecteert u de knooppunten en gebruik de **Online brengen** actie in HPC Cluster Manager. De knooppunten zijn gereed om uit te voeren taken.
7. **Verzenden van taken naar het cluster**
   
   HPC Pack taak verzending's gebruiken om het cluster taken uitvoeren. Zie [Microsoft HPC Pack: Taakbeheer](http://technet.microsoft.com/library/jj899585.aspx).
8. **Stoppen (deprovision) de knooppunten**
   
   Wanneer u klaar bent met het actieve taken offline nemen van de knooppunten en de **stoppen** actie in HPC Cluster Manager.

## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Scenario 2: Implementeer rekenknooppunten in rekenintensieve VM's (IaaS)
In dit scenario wordt door de HPC Pack hoofdknooppunt en cluster rekenknooppunten op virtuele machines in een Azure-netwerk op te implementeren. HPC Pack bevat diverse [implementatieopties in Azure VM's](../../linux/hpcpack-cluster-options.md), met inbegrip van geautomatiseerde implementatiescripts en Azure-snelstartsjablonen. Als u bijvoorbeeld de volgende overwegingen en stappen helpen u om te gebruiken de [HPC Pack IaaS-implementatiescript](hpcpack-cluster-powershell-script.md) voor het automatiseren van de implementatie van een HPC Pack 2012 R2-cluster in Azure.

![Cluster in virtuele machines in Azure][iaas]

### <a name="steps"></a>Stappen
1. **Maken van het hoofdknooppunt van een cluster en virtuele machines knooppunt berekenen door het implementatiescript HPC Pack IaaS uitgevoerd op een clientcomputer**
   
    De HPC Pack IaaS-implementatiescript downloaden uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922).
   
    Als u met het voorbereiden van de clientcomputer, maken het configuratie-scriptbestand en voer het script, Zie [een HPC-Cluster maken met het implementatiescript HPC Pack IaaS](hpcpack-cluster-powershell-script.md). 
   
    Houd rekening met de volgende aanvullende overwegingen voor het implementeren van RDMA-compatibele rekenknooppunten:
   
   * **Virtueel netwerk**: Geef een nieuw virtueel netwerk in een regio waarin de RDMA-compatibele exemplaargrootte die u wilt gebruiken beschikbaar is.
   * **Windows Server-besturingssysteem**: Geef een besturingssysteem Windows Server 2012 R2 of Windows Server 2012 voor het rekenknooppunt VM's ter ondersteuning van RDMA-netwerkverbinding.
   * **Cloudservices**: het is raadzaam uw hoofdknooppunt in een cloudservice en uw rekenknooppunten in een andere cloudservice implementeren.
   * **De grootte van knooppunt HEAD**: voor dit scenario kunt u een grootte van ten minste A4 (Extra groot) voor het hoofdknooppunt.
   * **De extensie HpcVmDrivers**: het implementatiescript de Azure VM-Agent en de extensie HpcVmDrivers wordt automatisch geïnstalleerd wanneer u de grootte van rekenknooppunten A8 of A9 met een Windows Server-besturingssysteem implementeert. HpcVmDrivers stuurprogramma's worden geïnstalleerd op het rekenknooppunt virtuele machines zodat ze verbinding met het netwerk RDMA maken kunnen. Op virtuele machines RDMA-compatibele H-serie, moet u de extensie HpcVmDrivers handmatig installeren. Zie [hoge prestaties compute-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   * **De netwerkconfiguratie**: het cluster HPC Pack in de topologie 5 (alle knooppunten in het bedrijfsnetwerk) script voor de implementatie automatisch ingesteld. Deze topologie is vereist voor alle implementaties van HPC Pack cluster in virtuele machines. De netwerktopologie van het cluster niet later wijzigen.
2. **De compute nodes online brengt taken uitvoeren**
   
    Selecteer de knooppunten en gebruik de **Online brengen** actie in HPC Cluster Manager. De knooppunten zijn gereed om uit te voeren taken.
3. **Verzenden van taken naar het cluster**
   
    Verbinding maken met het hoofdknooppunt om taken te verzenden of een on-premises computer instellen om dit te doen. Zie voor informatie [taken verzenden naar een HPC-cluster in Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
4. **Offline zetten van de knooppunten en gestopt (toewijzing ongedaan maken) ze**
   
    Wanneer u klaar bent met het actieve taken duren offline de knooppunten in HPC Cluster Manager. Vervolgens gebruikt u Azure management-hulpprogramma's voor ze kunt afsluiten.

## <a name="run-mpi-applications-on-the-cluster"></a>Uitvoeren van MPI-toepassingen op het cluster
### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Voorbeeld: Mpipingpong uitgevoerd op een cluster HPC Pack
Om te controleren of een HPC Pack-implementatie van de RDMA-compatibele exemplaren, voer het HPC Pack **mpipingpong** opdracht op het cluster. **mpipingpong** verzendt pakketten van gegevens tussen gekoppelde knooppunten herhaaldelijk voor het berekenen van de latentie en doorvoer metingen en statistieken voor het netwerk RDMA-toepassing. Dit voorbeeld ziet u een doorsnee patroon voor het uitvoeren van een MPI-taak (in dit geval **mpipingpong**) met behulp van het cluster **mpiexec** opdracht.

In dit voorbeeld wordt ervan uitgegaan dat u Azure knooppunten toegevoegd in een 'burst naar Azure'-configuratie ([Scenario 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-\(PaaS\) in this article). Als u HPC Pack geïmplementeerd op een cluster van virtuele Azure-machines, moet u de syntaxis van de opdracht Geef een ander knooppunt en aanvullende omgevingsvariabelen toe om netwerkverkeer toe aan het netwerk RDMA rechtstreeks worden ingesteld als u wilt wijzigen.

Mpipingpong uitvoeren op het cluster:

1. Open een opdrachtprompt op het hoofdknooppunt of op een clientcomputer juist is geconfigureerd.
2. Typ de volgende opdracht om het verzenden van een taak voor uitvoering van mpipingpong met een kleine pakketten, grootte en veel iteraties voor een schatting van de latentie tussen paren van knooppunten in de implementatie van een Azure burst met vier knooppunten:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```
   
    De opdracht retourneert de ID van de taak die wordt verzonden.
   
    Als u het cluster HPC Pack is geïmplementeerd op Azure Virtual machines hebt geïmplementeerd, Geef een knooppunt groep met compute knooppunt virtuele machines die zijn geïmplementeerd in een enkel cloudservice en wijzigt de **mpiexec** opdracht als volgt:
   
    ```Command
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```
3. Wanneer de taak is voltooid, als u wilt weergeven van de uitvoer (in dit geval de uitvoer van de taak 1 van de taak), typt u het volgende
   
    ```Command
    task view <JobID>.1
    ```
   
    waar &lt; *JobID* &gt; is de ID van de taak die is ingediend.
   
    De uitvoer bevat latentie resultaten ziet er als volgt.
   
    ![Ping pong latentie][pingpong1]
4. Typ de volgende opdracht voor het verzenden van een taak uitvoeren voor een schatting van doorvoer tussen paren van Azure burst knooppunten **mpipingpong** met een grote pakketten, grootte en een aantal iteraties:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```
   
    De opdracht retourneert de ID van de taak die wordt verzonden.
   
    Op een HPC Pack cluster wordt geïmplementeerd op Azure Virtual machines, moet u de opdracht zoals beschreven in stap 2 aanpassen.
5. Wanneer de taak is voltooid, als u wilt weergeven van de uitvoer (in dit geval de uitvoer van de taak 1 van de taak), typ het volgende:
   
    ```Command
    task view <JobID>.1
    ```
   
   De uitvoer bevat de doorvoerresultaten ziet er als volgt.
   
   ![Ping pong doorvoer][pingpong2]

### <a name="mpi-application-considerations"></a>Overwegingen met betrekking tot MPI-toepassing
Hieronder volgen enkele overwegingen voor het uitvoeren van MPI-toepassingen met HPC Pack in Azure. Sommige gelden alleen voor implementaties van Azure knooppunten (worker rolinstanties toegevoegd in de configuratie van een 'burst naar Azure').

* Werknemer-rolexemplaren in een cloudservice worden periodiek zonder voorafgaande kennisgeving ingericht door Azure (bijvoorbeeld voor systeemonderhoud of in geval die een exemplaar is mislukt). Een exemplaar is ingericht tijdens het uitvoeren van een MPI-taak, het exemplaar gegevens verliest als de status retourneren wanneer deze is geïmplementeerd, waardoor de taak MPI mislukken. De meer knooppunten die u voor een enkele MPI-taak en hoe langer gebruikt de taak wordt uitgevoerd, hoe groter de kans dat een van de exemplaren is ingericht, terwijl een taak wordt uitgevoerd. U kunt ook dit als u één knooppunt in de implementatie als een bestandsserver opgeven.
* Als u wilt uitvoeren van MPI-jobs in Azure, moet u niet de RDMA-compatibele exemplaren gebruiken. U kunt de exemplaargrootte van een dat wordt ondersteund door HPC Pack gebruiken. De RDMA-compatibele exemplaren worden echter aanbevolen om relatief grootschalige gevoelig zijn voor de latentie en de bandbreedte van het netwerk die verbinding de knooppunten maakt MPI-taken uit te voeren. Als u met een andere grootten latentiegevoelig en bandbreedte MPI-taken uitvoeren, raden wij kleine taken, waarin een enkele taak wordt uitgevoerd op slechts een paar knooppunten uitgevoerd.
* Geïmplementeerde toepassingen op Azure-exemplaren zijn onderworpen aan de licentievoorwaarden die zijn gekoppeld aan de toepassing. Neem contact op met de leverancier van een commerciële toepassing voor licentieverlening of andere beperkingen voor het uitvoeren in de cloud. Niet alle leveranciers bieden licenties waarbij u betaalt naar gebruik.
* Azure-exemplaren moeten verdere setup toegang lokale knooppunten, shares en licentieservers. U kunt bijvoorbeeld een site-naar-site virtuele Azure-netwerk configureren zodat de Azure knooppunten voor toegang tot een on-premises licentie-server.
* Als u wilt uitvoeren van MPI-toepassingen op Azure-exemplaren, elke MPI toepassing registreren met Windows Firewall op de exemplaren door het uitvoeren van de **hpcfwutil** opdracht. Hierdoor MPI-communicatie vindt plaats op een poort die dynamisch wordt toegewezen door de firewall.
  
  > [!NOTE]
  > U kunt ook een firewall-uitzondering opdracht automatisch wordt uitgevoerd op alle nieuwe Azure knooppunten die zijn toegevoegd aan het cluster configureren voor burst naar Azure-implementaties. Nadat u de **hpcfwutil** opdracht in en controleer of dat uw toepassing werkt, de opdracht toevoegen aan een opstartscript voor uw Azure knooppunten. Zie voor meer informatie [gebruik een opstartscript voor Azure knooppunten](https://technet.microsoft.com/library/jj899632.aspx).
  > 
  > 
* HPC Pack maakt gebruik van de omgevingsvariabele CCP_MPI_NETMASK cluster om op te geven van een bereik van acceptabele adressen voor MPI-communicatie. U start in HPC Pack 2012 R2, de omgevingsvariabele CCP_MPI_NETMASK cluster alleen van invloed op MPI-communicatie tussen de rekenknooppunten van de domein-cluster (on-premises of in Azure VM's). De variabele wordt genegeerd door de knooppunten in een ' burst ' toegevoegd aan Azure-configuratie.
* MPI-taken uitvoeren niet op Azure-exemplaren die in een andere cloud-services (bijvoorbeeld burst naar Azure implementaties met een ander knooppunt sjablonen of Azure VM rekenknooppunten geïmplementeerd in meerdere cloudservices) zijn geïmplementeerd. Als u meerdere Azure knooppunt implementaties die zijn gestart met een ander knooppunt sjablonen hebt, wordt de MPI-taak moet uitvoeren op slechts één set met Azure knooppunten.
* Wanneer u Azure knooppunten aan het cluster toevoegen en u ze online brengt, probeert de HPC Job Scheduler-Service onmiddellijk taken starten op de knooppunten. Als er slechts een deel van uw werkbelasting op Azure uitvoeren kunt, zorg ervoor dat u bijwerken of de taaksjablonen om te definiëren wat taaktypen kunnen uitvoeren op Azure maken. Bijvoorbeeld, om ervoor te zorgen dat taken die worden verzonden met een taaksjabloon alleen op Azure knooppunten uitvoeren, de eigenschap knooppuntgroepen toevoegen aan de taaksjabloon en AzureNodes selecteren als de vereiste waarde. Gebruik de Add-HpcGroup HPC PowerShell-cmdlet voor het maken van aangepaste groepen voor uw Azure-knooppunten.

## <a name="next-steps"></a>Volgende stappen
* Als alternatief voor het gebruik van HPC Pack ontwikkelen met de Azure Batch-service op MPI-toepassingen uitvoeren op beheerde pools van rekenknooppunten in Azure. Zie [taken met meerdere instanties gebruiken voor het uitvoeren van Message Passing Interface (MPI) applications in Azure Batch](../../../batch/batch-mpi.md).
* Als u Linux MPI toepassingen uitvoeren die toegang tot het netwerk van Azure RDMA wilt, Zie [instellen van een cluster Linux RDMA MPI-toepassingen uitvoeren](../../linux/classic/rdma-cluster.md).

<!--Image references-->
[burst]:media/hpcpack-rdma-cluster/burst.png
[iaas]:media/hpcpack-rdma-cluster/iaas.png
[pingpong1]:media/hpcpack-rdma-cluster/pingpong1.png
[pingpong2]:media/hpcpack-rdma-cluster/pingpong2.png
