---
title: Uitvoeren van de STER-CCM + met HPC Pack op de virtuele Linux-machines | Microsoft Docs
description: Een Microsoft HPC Pack cluster in Azure implementeren en uitvoeren van een STER-taak CCM + op meerdere Linux-rekenknooppunten via een netwerk RDMA.
services: virtual-machines-linux
documentationcenter: 
author: xpillons
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 75523406-d268-4623-ac3e-811c7b74de4b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 09/13/2016
ms.author: xpillons
ms.openlocfilehash: b45fcfb981287035da02fda62eaf5f9436ec2379
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Uitvoeren van de STER-CCM + met Microsoft HPC Pack op een Linux RDMA-cluster in Azure
Dit artikel ziet u het implementeren van een cluster Microsoft HPC Pack op Azure en voer een [CD adapco STER-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) taak op meerdere Linux-rekenknooppunten die met elkaar door middel van InfiniBand verbonden zijn.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack bevat functies om uit te voeren van tal van grootschalige HPC en parallelle toepassingen, waaronder MPI-toepassingen op clusters van Microsoft Azure virtuele machines. HPC Pack ondersteunt ook waarop Linux HPC-toepassingen op Linux-rekenknooppunt virtuele machines die zijn geïmplementeerd in een cluster HPC Pack. Zie voor een inleiding tot het gebruik van Linux-rekenknooppunten met HPC Pack, [aan de slag met Linux-rekenknooppunten in een cluster HPC Pack Azure](hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Een cluster HPC Pack instellen
Download de implementatiescripts HPC Pack IaaS van de [Downloadcentrum](https://www.microsoft.com/en-us/download/details.aspx?id=44949) en pak ze lokaal.

Azure PowerShell is een vereiste. Lees het artikel als PowerShell is niet geconfigureerd op uw lokale computer, [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

Op het moment van schrijven van dit zijn de Linux-installatiekopieën vanuit Azure Marketplace (met daarin de InfiniBand-stuurprogramma's voor Azure) voor SLES 12, CentOS 6.5 en CentOS 7.1. In dit artikel is gebaseerd op het gebruik van SLES 12. Voor het ophalen van de naam van alle Linux-installatiekopieën die ondersteuning bieden voor HPC in de Marketplace, kunt u de volgende PowerShell-opdracht uitvoeren:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

De uitvoer bevat de locatie waarin deze installatiekopieën beschikbaar zijn en de naam van de installatiekopie (**ImageName**) moet later in de sjabloon voor de implementatie worden gebruikt.

Voordat u het cluster implementeert, hebt u een sjabloonbestand voor implementatie van HPC Pack bouwen. Omdat we een klein cluster ontwikkelt je, wordt het hoofdknooppunt de domeincontroller en een lokale SQL-database host.

De volgende sjabloon wordt een hoofdknooppunt implementeren, maakt u een XML-bestand met de naam **MyCluster.xml**, en vervang de waarden van **SubscriptionId**, **StorageAccount**,  **Locatie**, **VMName**, en **ServiceName** met jouw e-mailadres.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Start het hoofdknooppunt maken door het uitvoeren van de PowerShell-opdracht in een opdrachtprompt met verhoogde bevoegdheid:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Na 20-30 minuten moet het hoofdknooppunt gereed. U kunt verbinding maken met het vanuit de Azure-portal door te klikken op de **Connect** pictogram van de virtuele machine.

Mogelijk moet u uiteindelijk los van de DNS-doorstuurserver. Om dit te doen, DNS-beheer te starten.

1. Met de rechtermuisknop op de naam van de server in DNS-beheer, selecteer **eigenschappen**, en klik vervolgens op de **doorstuurservers** tabblad.
2. Klik op de **bewerken** klikken om het verwijderen van doorstuurservers en klik vervolgens op **OK**.
3. Zorg ervoor dat de **root-hints gebruiken als er geen doorstuurservers beschikbaar** selectievakje is ingeschakeld en klik vervolgens op **OK**.

## <a name="set-up-linux-compute-nodes"></a>Linux-rekenknooppunten instellen
U implementeren de Linux-rekenknooppunten met behulp van de dezelfde installatiesjabloon die u gebruikt voor het maken van het hoofdknooppunt.

Kopieer het bestand **MyCluster.xml** van uw lokale computer naar het hoofdknooppunt en update de **NodeCount** label met het aantal knooppunten dat u wilt implementeren (< = 20). Let voldoende beschikbare kernen hebben in uw Azure quotum omdat elk exemplaar A9 16 kernen in uw abonnement verbruikt. Als u wilt meer virtuele machines in hetzelfde budget gebruiken, kunt u A8 exemplaren (8 kernen) in plaats van A9.

Kopieer de implementatiescripts HPC Pack IaaS op het hoofdknooppunt.

Voer de volgende Azure PowerShell-opdrachten in een opdrachtprompt met verhoogde bevoegdheid:

1. Voer **Add-AzureAccount** verbinding maken met uw Azure-abonnement.
2. Als u meerdere abonnementen hebt, voert u **Get-AzureSubscription** om deze.
3. Een standaardabonnement ingesteld door de **Select-AzureSubscription - SubscriptionName-xxxx-standaard** opdracht.
4. Voer **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** om te implementeren Linux-rekenknooppunten.
   
   ![Hoofdknooppunt implementatie in actie][hndeploy]

Open het hulpprogramma HPC Pack Cluster Manager. Linux-rekenknooppunten wordt regelmatig na enkele minuten weergegeven in de lijst met clusterknooppunten compute. Met de klassieke implementatiemodus worden opeenvolgend IaaS VM's gemaakt. Dus als het aantal knooppunten belangrijk is, alle geïmplementeerd ophalen kan duren voordat een aanzienlijke hoeveelheid tijd.

![Linux-knooppunten in HPC Pack Cluster Manager][clustermanager]

Nu dat alle knooppunten actief in het cluster zijn, zijn er extra infrastructuurinstellingen aanbrengen.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Instellen van een bestandsshare in Azure voor Windows en Linux-knooppunten
U kunt de Azure File-service gebruiken voor het opslaan van scripts, toepassingspakketten en gegevensbestanden worden opgeslagen. Azure-bestand biedt mogelijkheden voor CIFS boven op Azure Blob-opslag als een permanente opslag. Let erop dat dit niet de meest schaalbare oplossing is, maar dit is de eenvoudigste en toegewezen virtuele machines niet nodig.

Een Azure-bestandsshare maken door de instructies in het artikel [aan de slag met Azure File storage in Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).

Houd de naam van uw opslagaccount als **saname**, de naam van bestandsshare als **sharename**, en de opslagaccountsleutel als **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>De Azure-bestandsshare op het hoofdknooppunt koppelen
Open een opdrachtprompt met verhoogde bevoegdheid en voer de volgende opdracht voor het opslaan van de referenties in de lokale computer-kluis:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Voer vervolgens de volgende opdracht om te koppelen van de Azure-bestandsshare:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>De Azure-bestandsshare op Linux-rekenknooppunten koppelen
Een nuttig hulpmiddel dat wordt geleverd met HPC Pack is het hulpprogramma clusrun. Dit opdrachtregelprogramma kunt u dezelfde opdracht tegelijkertijd worden uitgevoerd op een set van rekenknooppunten. In ons geval wordt deze gebruikt voor de Azure-bestandsshare koppelen en om te blijven na opnieuw opstarten te behouden.
Voer de volgende opdrachten in een opdrachtprompt met verhoogde bevoegdheid op het hoofdknooppunt.

De gekoppelde map maken:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Koppelen van de Azure-bestandsshare:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Om vast te leggen van de share koppelen:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Installeren van de STER-CCM +
Azure VM-instanties A8 en A9 bieden ondersteuning voor InfiniBand en RDMA-mogelijkheden. De kernel-stuurprogramma waarmee deze mogelijkheden zijn beschikbaar voor Windows Server 2012 R2, SUSE 12, CentOS 6.5 en CentOS 7.1 afbeeldingen in Azure Marketplace. Microsoft MPI en Intel MPI (versie 5.x) zijn de twee MPI-bibliotheken die ondersteuning bieden voor de stuurprogramma's in Azure.

CD adapco STER-CCM + 11.x vrijgeven en later wordt meegeleverd met Intel MPI-versie 5.x, dus InfiniBand-ondersteuning voor Azure opgenomen is.

Ophalen van de Linux64 STER-CCM + pakket van de [CD adapco portal](https://steve.cd-adapco.com). In ons geval hebben we versie 11.02.010 in gemengde precisie gebruikt.

Op het hoofdknooppunt in de **/hpcdata** Azure File delen, maakt u een shell-script met de naam **setupstarccm.sh** met de volgende inhoud. Dit script wordt uitgevoerd op elk rekenknooppunt voor het instellen van de STER-CCM + lokaal.

#### <a name="sample-setupstarcmsh-script"></a>Voorbeeldscript setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Nu voor het instellen van de STER-CCM + op alle uw Linux rekenknooppunten, open een opdrachtprompt met verhoogde bevoegdheid en voer de volgende opdracht:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Terwijl de opdracht wordt uitgevoerd, kunt u het CPU-gebruik controleren met behulp van de heatmap van Clusterbeheer. Na enkele minuten alle knooppunten moeten correct zijn ingesteld.

## <a name="run-star-ccm-jobs"></a>Uitvoeren van de STER-CCM + taken
HPC Pack wordt gebruikt voor de taak scheduler mogelijkheden om te kunnen uitvoeren STER-CCM + taken. Om dit te doen, moeten we de ondersteuning van een aantal scripts die worden gebruikt voor het starten van de taak en voeren STER-CCM +. De ingevoerde gegevens wordt opgeslagen op de Azure-bestandsshare eerste voor eenvoud.

De volgende PowerShell-script wordt gebruikt in de wachtrij een STER-CCM + taak. Het duurt drie argumenten:

* Naam van het model
* Het aantal knooppunten moet worden gebruikt
* Het aantal kernen op elk knooppunt moet worden gebruikt

Omdat STER-CCM + vult de geheugenbandbreedte, is het doorgaans beter gebruiken minder kernen per rekenknooppunten en het toevoegen van nieuwe knooppunten. Het exacte aantal kernen per knooppunt, is afhankelijk van de processorfamilie en de snelheid tussen geheugenonderdelen.

De knooppunten uitsluitend bedoeld is voor de taak worden toegewezen en kunnen niet worden gedeeld met andere taken. De taak is niet rechtstreeks als een MPI-taak gestart. De **runstarccm.sh** shell-script wordt MPI starten.

Het ingevoerde model en de **runstarccm.sh** script worden opgeslagen in de **/hpcdata** -share die eerder is gekoppeld.

Logboekbestanden zijn met de naam met de taak-ID en worden opgeslagen in de **/hpcdata share**, samen met de STER-CCM + uitvoerbestanden.

#### <a name="sample-submitstarccmjobps1-script"></a>Voorbeeldscript SubmitStarccmJob.ps1
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job     
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Vervang **runner.java** met uw voorkeur STER-CCM + Java model linksboven en logboekregistratie-code.

#### <a name="sample-runstarccmsh-script"></a>Voorbeeldscript runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

In onze test hebben we een token Power-On-Demand-licentie gebruikt. Voor die u hebt ingesteld de **$CDLMD_LICENSE_FILE** omgevingsvariabele  **1999@flex.cd-adapco.com**  en de sleutel in de **- podkey** optie van de opdrachtregel.

Na enkele initialisatie pakt het script--uit de **$CCP_NODES_CORES** omgevingsvariabelen die HPC Pack instellen--de lijst met knooppunten voor het bouwen van een hostfile die gebruikmaakt van het MPI-venster te starten. Deze hostfile bevat de lijst met namen van de compute nodes die worden gebruikt voor de taak één naam per regel.

De indeling van **$CCP_NODES_CORES** volgt dit patroon:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Waar:

* `<Number of nodes>`is het aantal knooppunten dat is toegewezen aan deze taak.
* `<Name of node_n_...>`is de naam van elk knooppunt dat is toegewezen aan deze taak.
* `<Cores of node_n_...>`is het aantal kernen op het knooppunt dat is toegewezen aan deze taak.

Het aantal kernen (**$NBCORES**) ook wordt berekend op basis van het aantal knooppunten (**$NBNODES**) en het aantal kernen per knooppunt (opgegeven als parameter **$NBCORESPERNODE**).

Voor de MPI-opties zijn de waarden die worden gebruikt met Intel MPI op Azure:

* `-mpi intel`Intel MPI opgeven.
* `-fabric UDAPL`Azure InfiniBand-woorden gebruiken.
* `-cpubind bandwidth,v`bandbreedte optimaliseren voor MPI met STER-CCM +.
* `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`om Intel MPI werken met Azure InfiniBand en instellen van het vereiste aantal kernen per knooppunt.
* `-batch`starten van de STER-CCM + in batchmodus met geen gebruikersinterface.

Ten slotte voor het starten van een taak, zorg dat uw knooppunten actief zijn en online in de Cluster-beheer zijn. Vanuit een PowerShell-opdrachtprompt voert u dit:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Knooppunten stoppen
Later op wanneer u met de tests uitgevoerd bent klaar, kunt u de volgende HPC Pack PowerShell-opdrachten om te stoppen en starten van de knooppunten:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Volgende stappen
Probeer andere werkbelastingen Linux wordt uitgevoerd. Bijvoorbeeld, Zie:

* [Voer NAMD met Microsoft HPC Pack op Linux-rekenknooppunten in Azure](hpcpack-cluster-namd.md)
* [OpenFOAM met Microsoft HPC Pack uitvoeren op een Linux RDMA-cluster in Azure](hpcpack-cluster-openfoam.md)

<!--Image references-->
[hndeploy]:media/hpcpack-cluster-starccm/hndeploy.png
[clustermanager]:media/hpcpack-cluster-starccm/ClusterManager.png
