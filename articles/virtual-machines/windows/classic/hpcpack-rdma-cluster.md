---
title: Een Windows RDMA-cluster instellen voor het uitvoeren van MPI-toepassingen | Microsoft Docs
description: Informatie over het maken van een Windows HPC Pack-cluster met een grootte H16r, H16mr, A8 of A9-VM's naar het Azure RDMA-netwerk gebruiken voor het uitvoeren van MPI-apps.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,hpc-pack
ms.assetid: 7d9f5bc8-012f-48dd-b290-db81c7592215
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 03/06/2018
ms.author: danlep
ms.openlocfilehash: 7bb49e48f3777304aa6f40cee40e0b7147994201
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49345234"
---
# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Een Windows RDMA-cluster met HPC Pack instellen voor het uitvoeren van MPI-toepassingen
Instellen van een Windows RDMA-cluster in Azure met [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) en [RDMA-compatibele HPC VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#rdma-capable-instances) parallelle Message Passing Interface (MPI)-toepassingen uit te voeren. Bij het instellen van RDMA-compatibele, op basis van Windows Server-knooppunten in een HPC Pack-cluster, wordt MPI-toepassingen efficiënt communiceren via een lage latentie en hoge doorvoer netwerk in Azure die is gebaseerd op technologie van remote direct memory access (RDMA).

## <a name="hpc-pack-cluster-deployment-options"></a>Opties voor de implementatie van HPC Pack-cluster
Microsoft HPC Pack is een hulpprogramma opgegeven zonder extra kosten voor het maken van HPC-clusters on-premises of in Azure voor het uitvoeren van Windows of Linux HPC toepassingen. HPC Pack bevat een runtime-omgeving voor de Microsoft-implementatie van de Message Passing Interface voor Windows (MS-MPI). Gebruikt in combinatie met RDMA-compatibele instanties met een ondersteund Windows Server-besturingssysteem, is HPC Pack een efficiënte Windows MPI-toepassingen die toegang hebben tot de Azure-RDMA-netwerk uit te voeren. 

Dit artikel bevat twee scenario's en koppelingen naar gedetailleerde richtlijnen voor het instellen van een Windows RDMA-cluster met Microsoft HPC Pack 2012 R2. 

* Scenario 1. Instanties van de rekenintensieve werkrol (PaaS) implementeren
* Scenario 2. Compute-knooppunten in rekenintensieve VM's (IaaS) implementeren

## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Scenario 1: Implementatie van instanties van de rekenintensieve werkrol (PaaS)
Toevoegen van een bestaand HPC Pack-cluster extra rekenresources in Azure werkrolinstanties (Azure knooppunten) die worden uitgevoerd in een cloudservice (PaaS). Deze functie, ook wel 'burst naar Azure' HPC Pack, ondersteunt een scala aan grootten voor de instanties van de werkrol. Bij het toevoegen van de Azure-knooppunten, een van de RDMA-compatibele grootte opgeven.

Hieronder vindt u overwegingen en stappen om uit te breiden naar RDMA-compatibele Azure-exemplaren van een bestaand (meestal on-premises) cluster. Gebruik vergelijkbare procedures werkrolinstanties toevoegen aan een HPC Pack-hoofdknooppunt dat in een Azure-VM is geïmplementeerd.

> [!NOTE]
> Zie voor een zelfstudie om uit te breiden naar Azure met HPC Pack [een hybride cluster met HPC Pack instellen](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Houd er rekening mee de overwegingen voor in de volgende stappen uit die van toepassing specifiek voor RDMA-compatibele zijn Azure-knooppunten.
> 
> 

![Stap over op Azure][burst]

### <a name="steps"></a>Stappen
1. **Implementeer en configureer een hoofdknooppunt van HPC Pack 2012 R2**
   
    De HPC Pack-installatie downloaden uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Zie voor vereisten en instructies voor het voorbereiden voor de implementatie van een Azure burst [stap over op Azure Worker Instances met Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).
2. **Een beheercertificaat in het Azure-abonnement configureren**
   
    Configureer een certificaat voor het beveiligen van de verbinding tussen het hoofdknooppunt en Azure. Zie voor opties en -procedures, [scenario's voor het configureren van het Azure-Beheercertificaat voor HPC Pack](http://technet.microsoft.com/library/gg481759.aspx). HPC Pack installeert voor testimplementaties, een standaard Microsoft HPC Azure-Beheercertificaat u snel naar uw Azure-abonnement uploaden kunt.
3. **Een nieuwe cloudservice en een opslagaccount maken**
   
    De Azure portal gebruiken voor een cloudservice (klassiek) en een opslagaccount (klassiek) te maken voor de implementatie. Deze resources maken in een regio waar de H-serie, A8 en A9-grootte die u wilt gebruiken beschikbaar is. Zie [Azure-producten per regio](https://azure.microsoft.com/regions/services/).

4. **Een Azure-knooppunt-sjabloon maken**
   
    Gebruik de Wizard knooppunt in HPC Cluster Manager maken. Zie voor stappen [maken van een sjabloon voor Azure-knooppunt](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) in "Stappen om te implementeren Azure knooppunten met Microsoft HPC Pack".
   
    Het is raadzaam een handmatige beschikbaarheid-beleid configureren in de sjabloon voor de eerste tests.
5. **Knooppunten toevoegen aan het cluster**
   
    Gebruik de Wizard knooppunten toevoegen in HPC Cluster Manager. Zie voor meer informatie, [Azure knooppunten toevoegen aan de Windows HPC-Cluster](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).
   
    Wanneer de grootte van de knooppunten op te geven, selecteert u een van de RDMA-compatibele exemplaargrootten.
   
   > [!NOTE]
   > In elke burst naar Azure-implementatie met de rekenintensieve instanties implementeert HPC Pack automatisch een minimum van twee RDMA-compatibele exemplaren (zoals A8) als proxy-knooppunten, naast de Azure worker-rolinstanties die u opgeeft. De proxy-knooppunten gebruiken kernen die zijn toegewezen aan het abonnement en kosten, samen met de instanties van de Azure-werkrol.
   > 
   > 
6. **(Ingericht) start de knooppunten en ze online te brengen taken uit te voeren**
   
    Selecteer de knooppunten en gebruik de **Start** actie in HPC Cluster Manager. Bij het inrichten is voltooid, selecteert u de knooppunten en gebruik de **Online brengen** actie in HPC Cluster Manager. De knooppunten zijn gereed voor het uitvoeren van taken.
7. **Verzenden van taken naar het cluster**
   
   Gebruik hulpprogramma's voor verzending van HPC Pack-taak cluster taken uit te voeren. Zie [Microsoft HPC Pack: Taakbeheer](http://technet.microsoft.com/library/jj899585.aspx).
8. **Stoppen (inrichting) de knooppunten**
   
   Wanneer u klaar bent met taken die worden uitgevoerd, offline zetten van de knooppunten en gebruik de **stoppen** actie in HPC Cluster Manager.

## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Scenario 2: Implementeren-rekenknooppunten in rekenintensieve VM's (IaaS)
In dit scenario implementeert u het hoofdknooppunt van HPC Pack en de cluster compute-knooppunten op virtuele machines in een Azure-netwerk. HPC Pack biedt verschillende [implementatie-opties in Azure VM's](../../windows/hpcpack-cluster-options.md), met inbegrip van geautomatiseerde implementatiescripts en Azure-quickstart-sjablonen. Als u bijvoorbeeld de volgende overwegingen en stappen begeleiden u gebruik van de [HPC Pack IaaS-implementatiescript](hpcpack-cluster-powershell-script.md) om de implementatie van een HPC Pack 2012 R2-cluster in Azure te automatiseren.

![Cluster in Azure VM's][iaas]

### <a name="steps"></a>Stappen
1. **Het hoofdknooppunt van een cluster maken en compute-knooppunt virtuele machines door het uitvoeren van het implementatiescript HPC Pack IaaS op een clientcomputer**
   
    De HPC Pack IaaS-implementatiescript downloaden uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).
   
    Om voor te bereiden op de clientcomputer, maken het configuratie-scriptbestand en voer het script, Zie [een HPC-Cluster maken met het implementatiescript HPC Pack IaaS](hpcpack-cluster-powershell-script.md). 
   
    Compute-knooppunten voor overwegingen bij het implementeren van RDMA-compatibele, Zie [High performance computing-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#rdma-capable-instances) en Let op het volgende:
   
   * **Virtueel netwerk**: Geef een nieuw virtueel netwerk in een regio waarin de H-serie, A8 en A9-grootte die u wilt gebruiken beschikbaar is. Zie [Azure-producten per regio](https://azure.microsoft.com/regions/services/).

   * **Windows Server-besturingssysteem**: ter ondersteuning van RDMA-connectiviteit, Geef een compatibel besturingssysteem Windows Server, zoals Windows Server 2012 R2 voor de compute-knooppunt VM's.
   * **Cloudservices**: omdat het script maakt gebruik van het klassieke implementatiemodel, de cluster virtuele machines zijn geïmplementeerd met behulp van Azure-cloudservices (`ServiceName` instellingen in het configuratiebestand). Het is raadzaam om de implementatie van het hoofdknooppunt in een service in de cloud en uw compute-knooppunten in een andere cloudservice. 
   * **De grootte van knooppunt HEAD**: voor dit scenario kunt u overwegen een grootte van ten minste A4 (Extra groot) voor het hoofdknooppunt.
   * **De extensie HpcVmDrivers**: het implementatiescript de Azure VM-Agent en de HpcVmDrivers-extensie wordt automatisch geïnstalleerd wanneer u implementeert met grootte A8 of A9 compute-knooppunten met een Windows Server-besturingssysteem. HpcVmDrivers stuurprogramma's worden geïnstalleerd op het rekenknooppunt virtuele machines, zodat ze verbinding met de RDMA-netwerk maken kunnen. Op RDMA-compatibele H-serie VM's, moet u handmatig de HpcVmDrivers-extensie installeren. Zie [High performance computing-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   * **De netwerkconfiguratie**: de HPC Pack-cluster in de topologie 5 (alle knooppunten in het bedrijfsnetwerk) het script voor implementatie automatisch ingesteld. Deze topologie is vereist voor alle implementaties van HPC Pack-cluster in virtuele machines. De netwerktopologie van het cluster niet later wijzigen.
1. **De compute-knooppunten online brengen taken uit te voeren**
   
    Selecteer de knooppunten en gebruik de **Online brengen** actie in HPC Cluster Manager. De knooppunten zijn gereed voor het uitvoeren van taken.
3. **Verzenden van taken naar het cluster**
   
    Verbinding maken met het hoofdknooppunt taken indienen of een on-premises computer instellen om dit te doen. Zie voor meer informatie, [taken verzenden naar een HPC-cluster in Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
4. **De knooppunten offline te halen en te stoppen (toewijzing ongedaan maken) ze**
   
    Wanneer u klaar bent met taken die worden uitgevoerd, worden de knooppunten offline in HPC Cluster Manager. Vervolgens gebruikt u Azure-beheerhulpprogramma's ze om af te sluiten.

## <a name="run-mpi-applications-on-the-cluster"></a>MPI-toepassingen op het cluster uitvoeren
### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Voorbeeld: Mpipingpong uitvoeren op een HPC Pack-cluster
Als u wilt controleren of een HPC Pack-implementatie van de RDMA-compatibele instanties, uitvoeren van het HPC Pack **mpipingpong** opdracht op het cluster. **mpipingpong** verzendt pakketten van gegevens tussen gekoppelde knooppunten herhaaldelijk voor het berekenen van de latentie en doorvoer metingen en statistieken voor het toepassingsnetwerk met RDMA-functionaliteit. In dit voorbeeld ziet u een doorsnee patroon voor het uitvoeren van een MPI-taak (in dit geval **mpipingpong**) met behulp van het cluster **mpiexec** opdracht.

In dit voorbeeld wordt ervan uitgegaan dat u Azure knooppunten in een 'burst naar Azure-configuratie hebt toegevoegd ([Scenario 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-\(PaaS\) in this article). Als u een HPC Pack geïmplementeerd op een cluster van virtuele Azure-machines, moet u de syntaxis van de opdracht voor het geven van de groep van een ander knooppunt en aanvullende omgevingsvariabelen om te leiden van netwerkverkeer naar de RDMA-netwerk instellen te wijzigen.

Om uit te voeren mpipingpong op het cluster:

1. Op het hoofdknooppunt of op een correct geconfigureerde client-computer, open een opdrachtprompt.
2. Typ de volgende opdracht om een taak voor het uitvoeren van mpipingpong met een kleine pakketgrootte en veel iteraties te verzenden voor een schatting van de latentie tussen knooppunten in de implementatie van een Azure burst met vier knooppunten:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```
   
    De opdracht retourneert de ID van de taak die wordt verzonden.
   
    Als u het HPC Pack-cluster geïmplementeerd op Azure VM's geïmplementeerd, Geef een knooppunt groep met compute-knooppunt VM's die zijn geïmplementeerd in een enkele cloudservice en wijzigen de **mpiexec** opdracht als volgt:
   
    ```Command
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```
3. Wanneer de taak is voltooid, als u wilt weergeven van de uitvoer (in dit geval de uitvoer van de taak 1 van de taak), typt u het volgende
   
    ```Command
    task view <JobID>.1
    ```
   
    waar &lt; *JobID* &gt; is de ID van de taak die is ingediend.
   
    De uitvoer bevat de latentie van het resultaat is vergelijkbaar met de volgende.
   
    ![Ping pong latentie][pingpong1]
4. Typ de volgende opdracht om in te dienen een taak wordt uitgevoerd voor een schatting van de doorvoer tussen Azure burstknooppunten **mpipingpong** met de grootte van een groot pakket en een aantal herhalingen:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```
   
    De opdracht retourneert de ID van de taak die wordt verzonden.
   
    Op een HPC Pack-cluster geïmplementeerd op Azure Virtual machines, wijzigt u de opdracht, zoals vermeld in stap 2.
5. Wanneer de taak is voltooid, als u wilt weergeven van de uitvoer (in dit geval de uitvoer van de taak 1 van de taak), typ het volgende:
   
    ```Command
    task view <JobID>.1
    ```
   
   De uitvoer bevat de doorvoer het resultaat is vergelijkbaar met de volgende.
   
   ![Ping pong doorvoer][pingpong2]

### <a name="mpi-application-considerations"></a>Overwegingen voor MPI-toepassingen
Hieronder vindt u overwegingen voor het uitvoeren van MPI-toepassingen met HPC Pack in Azure. Sommige gelden alleen voor implementaties van Azure-knooppunten (werkrolinstanties toegevoegd in een 'burst naar Azure-configuratie).

* Instanties van de werkrol in een cloudservice worden periodiek ingericht zonder voorafgaande kennisgeving door Azure (bijvoorbeeld voor systeemonderhoud of in geval die een exemplaar is mislukt). Als een exemplaar is ingericht, terwijl deze een MPI-taak wordt uitgevoerd, wordt het exemplaar verliest de gegevens en teruggezet naar de status wanneer deze is geïmplementeerd, waardoor de taak MPI mislukken. De meer knooppunten die u voor een enkele MPI-taak en hoe langer gebruikt de taak wordt uitgevoerd, hoe groter de kans dat een van de exemplaren is ingericht, terwijl een taak wordt uitgevoerd. Ook kunt u dit als u een enkel knooppunt in de implementatie als een bestandsserver opgeven.
* Voor MPI-opdrachten uitvoeren in Azure, hebt u geen gebruik van de RDMA-compatibele exemplaren. U kunt de grootte van elk exemplaar dat wordt ondersteund door HPC Pack kunt gebruiken. Echter, de RDMA-compatibele exemplaren worden aanbevolen voor het uitvoeren van relatief grootschalige MPI-opdrachten die gevoelig zijn voor de latentie en de bandbreedte van het netwerk die verbinding de knooppunten maakt. Als u andere grootten latentie en bandbreedte gevoelig MPI-taken uit te voeren, wordt u aangeraden de uitvoering van kleine taken, waarin een enkele taak wordt uitgevoerd op slechts een paar knooppunten.
* Toepassingen die zijn geïmplementeerd op Azure-exemplaren zijn afhankelijk van de licentievoorwaarden die zijn gekoppeld aan de toepassing. Neem contact op met de leverancier van een commerciële toepassing voor licentieverlening of andere beperkingen voor het uitvoeren in de cloud. Niet alle leveranciers bieden licenties waarbij u betaalt naar gebruik.
* Azure-exemplaren moeten aanvullende instellingen voor toegang tot on-premises knooppunten, shares en licentieservers. U kunt bijvoorbeeld een site-naar-site virtueel netwerk Azure configureren zodat de Azure-knooppunten voor toegang tot een on-premises licentieserver.
* Als u wilt uitvoeren van MPI-toepassingen op Azure-exemplaren, elke MPI toepassing registreren met Windows Firewall op de instanties door het uitvoeren van de **hpcfwutil** opdracht. Hierdoor is communicatie MPI om te worden toegepast op een poort die dynamisch door de firewall toegewezen wordt.
  
  > [!NOTE]
  > U kunt ook een firewall-uitzondering opdracht automatisch wordt uitgevoerd op alle nieuwe Azure knooppunten die zijn toegevoegd aan het cluster configureren voor burst naar Azure-implementaties. Nadat u de **hpcfwutil** opdracht en controleer of dat uw toepassing werkt, de opdracht toevoegen aan een opstartscript voor uw Azure-knooppunten. Zie voor meer informatie, [een opstartscript gebruiken voor Azure-knooppunten](https://technet.microsoft.com/library/jj899632.aspx).
  > 
  > 
* HPC Pack maakt gebruik van de omgevingsvariabele CCP_MPI_NETMASK cluster om op te geven van een bereik van acceptabele-adressen voor MPI-communicatie. Starten in HPC Pack 2012 R2, de omgevingsvariabele CCP_MPI_NETMASK cluster heeft alleen gevolgen voor MPI-communicatie tussen het domein gekoppeld cluster compute-knooppunten (on-premises of in Azure VM's). De variabele wordt genegeerd door de knooppunten in een ' burst ' toegevoegd aan Azure-configuratie.
* MPI-opdrachten kunnen niet worden uitgevoerd over de Azure-instanties die zijn geïmplementeerd in verschillende cloudservices (bijvoorbeeld in burst naar Azure-implementaties met een ander knooppunt sjablonen of Azure-VM-rekenknooppunten dat is geïmplementeerd in meerdere cloudservices). Als u meerdere Azure-knooppunt-implementaties die zijn gestart met een ander knooppunt sjablonen hebt, moet de MPI-taak uitvoeren op slechts één set Azure-knooppunten.
* Wanneer u Azure knooppunten aan uw cluster toevoegen en ze online brengt, wordt er onmiddellijk de HPC Job Scheduler-Service probeert om taken te starten op de knooppunten. Als er slechts een deel van uw workload op Azure uitvoeren kunt, zorg ervoor dat u bijwerkt of taaksjablonen om te definiëren wat taaktypen kunnen uitvoeren op Azure maken. Bijvoorbeeld, om ervoor te zorgen dat taken worden ingediend met een taaksjabloon voor de alleen uitgevoerd op Azure-knooppunten, de eigenschap knooppuntgroepen toevoegen aan de sjabloon en selecteer AzureNodes als de vereiste waarde. Gebruik de Add-HpcGroup HPC PowerShell-cmdlet voor het maken van aangepaste groepen voor uw Azure-knooppunten.

## <a name="next-steps"></a>Volgende stappen
* Als alternatief voor het gebruik van HPC Pack kunt ontwikkelen met de Azure Batch-service voor het uitvoeren van MPI-toepassingen in beheerde pools van rekenknooppunten in Azure. Zie [taken met meerdere instanties gebruiken voor het uitvoeren van Message Passing Interface (MPI) applications in Azure Batch](../../../batch/batch-mpi.md).

<!--Image references-->
[burst]:media/hpcpack-rdma-cluster/burst.png
[iaas]:media/hpcpack-rdma-cluster/iaas.png
[pingpong1]:media/hpcpack-rdma-cluster/pingpong1.png
[pingpong2]:media/hpcpack-rdma-cluster/pingpong2.png
