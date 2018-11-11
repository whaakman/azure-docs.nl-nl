---
title: Linux VM's in een HPC Pack-cluster compute | Microsoft Docs
description: Meer informatie over het maken en gebruiken van een HPC Pack-cluster in Azure voor Linux van de high performance computing (HPC)-workloads
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 4156071c36b06be586b05ee98e9eeb0a9138e4bb
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246851"
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Aan de slag met Linux-rekenknooppunten in een HPC Pack-cluster in Azure
Instellen van een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) -cluster in Azure met een hoofdknooppunt met Windows Server en diverse compute-knooppunten met een ondersteunde Linux-distributie. Verken opties voor het verplaatsen van gegevens tussen de Linux-knooppunten en het Windows-hoofdknooppunt van het cluster. Informatie over het verzenden van Linux HPC-taken naar het cluster.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Het volgende diagram toont de HPC Pack-cluster op hoog niveau, u maakt en gebruikt.

![HPC Pack-cluster met Linux-knooppunten][scenario]

Zie voor andere opties voor het uitvoeren van Linux HPC workloads in Azure, [technische bronnen voor batchverwerking en high performance computing](../../../batch/big-compute-resources.md).

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a>Een HPC Pack-cluster met Linux-rekenknooppunten implementeren
Dit artikel ziet u twee opties voor het implementeren van een HPC Pack-cluster in Azure met Linux-rekenknooppunten. Beide methoden voor het gebruik van een Marketplace-installatiekopie van Windows Server met HPC Pack te maken van het hoofdknooppunt. 

* **Azure Resource Manager-sjabloon** -gebruik van een sjabloon van de Azure Marketplace of een quickstart-sjabloon uit de community voor het automatiseren van het maken van het cluster in het Resource Manager-implementatiemodel. Bijvoorbeeld, de [HPC Pack-cluster voor Linux-workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) sjabloon in de Azure Marketplace maakt u een volledige infrastructuur voor HPC Pack-cluster voor Linux HPC werkbelastingen.
* **PowerShell-script** -gebruik de [Microsoft HPC Pack IaaS-implementatiescript](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) om de Clusterimplementatie van een volledig in het klassieke implementatiemodel te automatiseren. Dit Azure PowerShell-script maakt gebruik van een HPC Pack VM-installatiekopie in de Azure Marketplace voor snelle implementatie en biedt een uitgebreide set parameters voor de configuratie voor het implementeren van Linux-rekenknooppunten.

Zie voor meer informatie over opties voor de implementatie van HPC Pack-cluster in Azure, [opties voor het maken en beheren van een high performance computing (HPC)-cluster in Azure met Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="prerequisites"></a>Vereisten
* **Azure-abonnement** -kunt u een abonnement in de globale Azure of Azure China-service. Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.
* **Quotum voor kerngeheugens** -moet u mogelijk vergroot het quotum voor kernen, met name als u wilt implementeren meerdere clusterknooppunten met multicore VM-grootten. Open een online ondersteuningsverzoek zonder kosten voor het verhogen van een quotum.
* **Linux-distributies** -momenteel HPC Pack biedt ondersteuning voor de volgende Linux-distributies voor compute-knooppunten. U kunt gebruiken van Marketplace-versies van deze distributies indien beschikbaar, of geef uw eigen.
  
  * **Op basis van centOS**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC
  * **Red Hat Enterprise Linux**: 6.7, 6,8, 7.2
  * **SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 voor HPC, SLES 12 voor HPC (Premium)
  * **Ubuntu-Server**: 14.04 LTS, 16.04 LTS
    
    > [!TIP]
    > Geef voor het gebruik van het Azure-RDMA-netwerk met een van de RDMA-compatibele VM-grootten, een installatiekopie van het SUSE Linux Enterprise Server 12 HPC- of op basis van CentOS HPC vanuit Azure Marketplace. Zie voor meer informatie, [High performance computing-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

Aanvullende vereisten voor het implementeren van het cluster met behulp van het implementatiescript HPC Pack IaaS:

* **Clientcomputer** -moet u een Windows-clientcomputer om uit te voeren een script voor de implementatie van het cluster.
* **Azure PowerShell** - [installeren en configureren van Azure PowerShell](/powershell/azure/overview) (versie 0.8.10 of hoger) op de clientcomputer.
* **HPC Pack IaaS-implementatiescript** : downloaden en uitpakken van de meest recente versie van het script uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). U kunt de versie van het script controleren door te voeren `.\New-HPCIaaSCluster.ps1 –Version`. In dit artikel is gebaseerd op versie 4.4.1 of later van het script.

### <a name="deployment-option-1-use-a-resource-manager-template"></a>Implementatieoptie 1. Een Resource Manager-sjabloon gebruiken
1. Ga naar de [HPC Pack-cluster voor Linux-workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) sjabloon in de Azure Marketplace, en klik op **implementeren**.
1. Lees de informatie in de Azure-portal en klik vervolgens op **maken**.
   
    ![Maken van de portal][portal]
1. Op de **basisbeginselen** blade, voer een naam voor het cluster, die ook-namen van de virtuele machine in het hoofdknooppunt. U kunt een bestaande resourcegroep kiezen of een groep maken voor de implementatie op een locatie die voor u beschikbaar is. De locatie is van invloed op de beschikbaarheid van bepaalde VM-grootten en andere Azure-services (Zie [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/)).
1. Op de **Head knooppunt instellingen** blade voor een eerste implementatie, kunt u in het algemeen de standaardinstellingen accepteren. 
   
   > [!NOTE]
   > De **post-configuratiescript URL** is een optionele instelling om op te geven van een openbaar beschikbare Windows PowerShell-script dat u uitvoeren op het hoofdknooppunt virtuele machine wilt nadat deze is uitgevoerd. 
   > 
   > 
1. Op de **Compute-knooppunt instellingen** blade, selecteer een naming patroon voor de knooppunten, het aantal en de grootte van de knooppunten en de Linux-distributie te implementeren.
1. Op de **infrastructuurinstellingen** blade, geef de namen voor het virtuele netwerk en Active Directory-domein, domein en VM-beheerdersreferenties en naamgevingspatroon uit een van de storage-accounts.
   
   > [!NOTE]
   > HPC Pack maakt gebruik van het Active Directory-domein om gebruikers van de cluster te verifiëren. 
   > 
   > 
1. Nadat de validatietests worden uitgevoerd en u de gebruiksvoorwaarden bekijken, klikt u op **aankoop**.

### <a name="deployment-option-2-use-the-iaas-deployment-script"></a>Implementatieoptie 2. Gebruik het IaaS-implementatiescript
Hieronder vindt u aanvullende vereisten voor het implementeren van het cluster met behulp van het implementatiescript HPC Pack IaaS:

* **Clientcomputer** -moet u een Windows-clientcomputer om uit te voeren een script voor de implementatie van het cluster.
* **Azure PowerShell** - [installeren en configureren van Azure PowerShell](/powershell/azure/overview) (versie 0.8.10 of hoger) op de clientcomputer.
* **HPC Pack IaaS-implementatiescript** : downloaden en uitpakken van de meest recente versie van het script uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). U kunt de versie van het script controleren door te voeren `.\New-HPCIaaSCluster.ps1 –Version`. In dit artikel is gebaseerd op versie 4.4.1 of later van het script.

**XML-configuratiebestand**

Het HPC Pack IaaS-implementatiescript maakt gebruik van een XML-configuratiebestand als invoer om te beschrijven het HPC-cluster. Het volgende voorbeeld-configuratiebestand bevat een klein cluster dat bestaat uit een HPC Pack-hoofdknooppunt en twee grootte A7 CentOS 7.0 Linux-rekenknooppunten. 

Het bestand niet wijzigen omdat die nodig zijn voor uw omgeving en de configuratie van het gewenste cluster en opslaan met een naam, bijvoorbeeld HPCDemoConfig.xml. Bijvoorbeeld, moet u de abonnementsnaam van uw en een unieke opslagaccountnaam en servicenaam in de cloud. Bovendien kunt u kiezen van een andere ondersteunde Linux-installatiekopie voor de compute-knooppunten. Zie voor meer informatie over de elementen in het configuratiebestand, het bestand Manual.rtf in de scriptmap en [een HPC-cluster maken met het implementatiescript HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

**Om uit te voeren van het implementatiescript HPC Pack IaaS**

1. Open Windows PowerShell op de clientcomputer als beheerder.
1. Wijziging naar de map waarin het script is geïnstalleerd (E:\IaaSClusterScript in dit voorbeeld).
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
1. Voer de volgende opdracht om de HPC Pack-cluster te implementeren. In dit voorbeeld wordt ervan uitgegaan dat het configuratiebestand in E:\HPCDemoConfig.xml bevinden zich
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    a. Omdat de **AdminPassword** niet is opgegeven in de voorgaande opdracht wordt u gevraagd het wachtwoord invoeren voor gebruiker *MyAdminName*.
   
    b. Het script vervolgens voor het valideren van het configuratiebestand wordt gestart. Het kan duren tot enkele minuten, afhankelijk van de netwerkverbinding.
   
    ![Validatie][validate]
   
    c. Nadat de validaties worden gehaald, is het script geeft een lijst de clusterresources te maken. Voer *Y* om door te gaan.
   
    ![Resources][resources]
   
    d. Het script begint met de implementatie van de HPC Pack-cluster en de configuratie zonder verdere handmatige stappen is voltooid. Het script kunt uitvoeren voor enkele minuten.
   
    ![Implementeren][deploy]
   
   > [!NOTE]
   > In dit voorbeeld, genereert het script een logboekbestand automatisch omdat de **- logboekbestand** parameter is niet opgegeven. De logboeken in realtime niet worden geschreven, maar worden verzameld aan het einde van de validatie en de implementatie. Als het PowerShell-proces wordt gestopt terwijl het script wordt uitgevoerd, zijn sommige logboeken verloren gaan.
   > 
   > 

## <a name="connect-to-the-head-node"></a>Verbinding maken met het hoofdknooppunt
Nadat u het HPC Pack-cluster in Azure, geïmplementeerd [verbinding maken met extern bureaublad](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) met het hoofdknooppunt VM met behulp van het domein referenties die u opgegeven bij de implementatie van het cluster (bijvoorbeeld *hpc\\clusteradmin*). U beheren het cluster vanaf het hoofdknooppunt.

Start op het hoofdknooppunt HPC Cluster Manager om te controleren of de status van de HPC Pack-cluster. U kunt beheren en bewaken die de dezelfde manier van werken met Windows-rekenknooppunten Linux-rekenknooppunten. Bijvoorbeeld, ziet u de Linux-knooppunten die worden vermeld in **resourcebeheer** (deze knooppunten worden geïmplementeerd met de **LinuxNode** sjabloon).

![Beheer van knooppunt][management]

U ziet ook de Linux-knooppunten in de **Heat Map** weergeven.

![Heatmap][heatmap]

## <a name="how-to-move-data-in-a-cluster-with-linux-nodes"></a>Het verplaatsen van gegevens in een cluster met Linux-knooppunten
U hebt verschillende mogelijkheden om gegevens tussen de Linux-knooppunten en het Windows-hoofdknooppunt van het cluster te verplaatsen. Hier volgen drie algemene methoden in de volgende secties in meer detail beschreven:

* **Azure-bestand** -wordt aangegeven dat een beheerde SMB-bestandsshare voor het opslaan van gegevensbestanden in Azure storage. Knooppunten voor Windows en Linux-knooppunten kunnen een Azure-bestandsshare koppelen als een station of een map op hetzelfde moment, zelfs als deze zijn geïmplementeerd in verschillende virtuele netwerken.
* **Hoofdknooppunt SMB delen** -koppelt een standaard gedeelde Windows-map van het hoofdknooppunt op Linux-knooppunten.
* **HEAD knooppunt NFS-server** -biedt een oplossing voor het delen van bestanden voor een gemengde omgeving van Windows en Linux.

### <a name="azure-file-storage"></a>Azure File storage
De [Azure File](https://azure.microsoft.com/services/storage/files/) service biedt bestandsshares met behulp van het standaard SMB 2.1-protocol. Azure VM's en cloudservices kunnen bestandsgegevens delen tussen toepassingsonderdelen via gekoppelde shares en on-premises toepassingen hebben toegang tot bestandsgegevens in een share via de API van File storage. 

Zie voor gedetailleerde stappen voor het maken van een Azure-bestandsshare en koppel deze op het hoofdknooppunt, [aan de slag met Azure File storage in Windows](../../../storage/files/storage-how-to-use-files-windows.md). Zie voor het koppelen van de Azure-bestandsshare op de Linux-knooppunten, [Azure File storage gebruiken met Linux](../../../storage/files/storage-how-to-use-files-linux.md). Als u permanente verbindingen instelt, Zie [Persisting verbindingen met Microsoft Azure Files](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

In het volgende voorbeeld maakt u een Azure-bestandsshare op een storage-account. Open een opdrachtprompt en voer de volgende opdrachten voor het koppelen van de bestandsshare op het hoofdknooppunt:

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

In dit voorbeeld allvhdsje is de naam van uw opslagaccount, storageaccountkey is de sleutel van uw opslagaccount en rdma is de sharenaam van de Azure-bestand. De Azure-bestandsshare is gekoppeld als Z: op het hoofdknooppunt.

Voer voor het koppelen van de Azure-bestandsshare op Linux-knooppunten, een **clusrun** opdracht op het hoofdknooppunt. **[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  een handig hulpmiddel HPC Pack administratieve taken op meerdere knooppunten uit te voeren. (Zie ook [Clusrun voor Linux-knooppunten](#Clusrun-for-Linux-nodes) in dit artikel.)

Open een Windows PowerShell-venster en voer de volgende opdrachten:

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

De eerste opdracht maakt een map met de naam /rdma op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de Azure File share allvhdsjw.file.core.windows.net/rdma naar de map /rdma met dir en bestand modusbits ingesteld op 777. In de tweede opdracht allvhdsje is de naam van uw opslagaccount en storageaccountkey is de sleutel van uw opslagaccount.

> [!NOTE]
> De "\`" symbool in de tweede opdracht is een escapeteken voor PowerShell. "\`, ' betekent dat de ',' (komma) een onderdeel van de opdracht is.
> 
> 

### <a name="head-node-share"></a>Hoofdknooppunt delen
U kunt ook een gedeelde map van het hoofdknooppunt op Linux-knooppunten koppelen. Een share biedt de eenvoudigste manier om bestanden te delen, maar het hoofdknooppunt en alle Linux-knooppunten moeten worden geïmplementeerd in hetzelfde virtuele netwerk. Hier volgen de stappen.

1. Maak een map op het hoofdknooppunt en delen voor iedereen met machtigingen voor lezen/schrijven. Bijvoorbeeld D:\OpenFOAM delen op het hoofdknooppunt als \\CentOS7RDMA HN\OpenFOAM. CentOS7RDMA-HN als volgt de hostnaam van het hoofdknooppunt.
   
    ![Machtigingen voor bestandsshares][fileshareperms]
   
    ![Bestandsdeling][filesharing]
1. Open een Windows PowerShell-venster en voer de volgende opdrachten uit:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

De eerste opdracht maakt een map met de naam /openfoam op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de gedeelde map //CentOS7RDMA-HN/OpenFOAM naar de map met dir en bestand modusbits ingesteld op 777. De gebruikersnaam en het wachtwoord in de opdracht moet de gebruikersnaam en het wachtwoord van een clustergebruiker op het hoofdknooppunt. (Zie [toevoegen of verwijderen cluster gebruikers](https://technet.microsoft.com/library/ff919330.aspx).)

> [!NOTE]
> De "\`" symbool in de tweede opdracht is een escapeteken voor PowerShell. "\`, ' betekent dat de ',' (komma) een onderdeel van de opdracht is.
> 
> 

### <a name="nfs-server"></a>NFS-server
De NFS-service kunt u het delen en migreren van bestanden tussen computers met het besturingssysteem van Windows Server 2012 met behulp van het SMB-protocol en op basis van Linux-computers met behulp van het NFS-protocol. De NFS-server en alle andere knooppunten moeten worden geïmplementeerd in hetzelfde virtuele netwerk. Het biedt betere compatibiliteit met Linux-knooppunten in vergelijking met een SMB-share. Bijvoorbeeld, ondersteunt het bestandskoppelingen.

1. Als u wilt installeren en instellen van een NFS-server, volgt u de stappen in [-Server voor Network File System-eerste Share End-to-End](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).
   
    Maak bijvoorbeeld een NFS-share met de naam nfs met de volgende eigenschappen:
   
    ![NFS-autorisatie][nfsauth]
   
    ![NFS-sharemachtigingen][nfsshare]
   
    ![NFS-NTFS-machtigingen][nfsperm]
   
    ![Eigenschappen van de NFS-beheer][nfsmanage]
1. Open een Windows PowerShell-venster en voer de volgende opdrachten uit:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   De eerste opdracht maakt een map met de naam /nfsshared op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de NFS-share CentOS7RDMA-HN: / nfs naar de map. Hier CentOS7RDMA-HN: / nfs is de externe pad van de NFS-share.

## <a name="how-to-submit-jobs"></a>Het verzenden van taken
Er zijn verschillende manieren voor het verzenden van taken naar de HPC Pack-cluster:

* HPC Cluster Manager-interface of GUI van HPC Job Manager
* HPC-webportal
* REST-API

Verzenden van taken in het cluster in Azure via HPC Pack GUI-hulpprogramma's en de HPC-webportal zijn dezelfde als die voor Windows-rekenknooppunten. Zie [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) en [hoe u taken verzenden vanuit een on-premises clientcomputer](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Als u wilt verzenden van taken via de REST-API, verwijzen naar [maken en verzenden van taken met behulp van de REST-API in Microsoft HPC Pack](https://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). Als u wilt verzenden van taken in een Linux-client, ook verwijzen naar het Python-voorbeeld in de [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).

## <a name="clusrun-for-linux-nodes"></a>Clusrun voor Linux-knooppunten
De HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) hulpprogramma kan worden gebruikt voor het uitvoeren van opdrachten op Linux-knooppunten via een opdrachtprompt of HPC Cluster Manager. Hier volgen enkele algemene voorbeelden.

* Huidige gebruikersnamen op alle knooppunten in het cluster weergeven.
  
    ```command
    clusrun whoami
    ```
* Installeer de **gdb** foutopsporingsprogramma met **yum** op alle knooppunten in de linuxnodes groeperen en start de knooppunten na 10 minuten opnieuw.
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* Maken van een shell-script weergeven van elk getal van 1 t/m 10 voor een tweede op elke Linux-knooppunt in het cluster, uitvoeren en de uitvoer van de knooppunten direct weergeven.
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> U moet mogelijk bepaalde escape-tekens in gebruiken **clusrun** opdrachten. In dit voorbeeld wordt weergegeven, gebruikt u ^ in een opdrachtprompt als escape voor de ' > ' symbool.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Probeer het cluster naar een groter aantal knooppunten omhoog of uitvoeren van een Linux-werkbelasting op het cluster. Zie voor een voorbeeld [NAMD uitvoeren met Microsoft HPC Pack op Linux-rekenknooppunten in Azure](hpcpack-cluster-namd.md).
* Een cluster met proberen [RDMA-compatibele, rekenintensieve VM's](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor het uitvoeren van MPI-workloads. Zie voor een voorbeeld [OpenFOAM uitvoeren met Microsoft HPC Pack op een Linux RDMA-cluster in Azure](hpcpack-cluster-openfoam.md).
* Als u geïnteresseerd bent in werken met Linux-knooppunten in een on-premises HPC Pack-cluster, raadpleegt u de [TechNet richtlijnen](https://technet.microsoft.com/library/mt595803.aspx).

<!--Image references-->
[scenario]:media/hpcpack-cluster/scenario.png
[portal]:media/hpcpack-cluster/portal.png
[validate]:media/hpcpack-cluster/validate.png
[resources]:media/hpcpack-cluster/resources.png
[deploy]:media/hpcpack-cluster/deploy.png
[management]:media/hpcpack-cluster/management.png
[heatmap]:media/hpcpack-cluster/heatmap.png
[fileshareperms]:media/hpcpack-cluster/fileshare1.png
[filesharing]:media/hpcpack-cluster/fileshare2.png
[nfsauth]:media/hpcpack-cluster/nfsauth.png
[nfsshare]:media/hpcpack-cluster/nfsshare.png
[nfsperm]:media/hpcpack-cluster/nfsperm.png
[nfsmanage]:media/hpcpack-cluster/nfsmanage.png
