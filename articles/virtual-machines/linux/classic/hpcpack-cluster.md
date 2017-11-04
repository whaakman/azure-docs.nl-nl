---
title: Linux VM's in een cluster HPC Pack compute | Microsoft Docs
description: Meer informatie over het maken en gebruiken van een HPC Pack-cluster in Azure voor Linux van de high performance computing (HPC)-workloads
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 809d3944311badf265117d353b65642e044d900c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Aan de slag met Linux-rekenknooppunten in een HPC Pack-cluster in Azure
Instellen van een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) cluster in Azure met een hoofdknooppunt met Windows Server en meerdere rekenknooppunten met een ondersteunde Linux-distributie. Gebruik de opties voor het verplaatsen van gegevens tussen de Linux-knooppunten en het Windows-hoofdknooppunt van het cluster. Informatie over het verzenden van Linux HPC-taken voor het cluster.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Op een hoog niveau ziet het volgende diagram u het cluster HPC Pack u maken en bewerken.

![HPC Pack cluster met Linux-knooppunten][scenario]

Zie voor andere opties voor het gebruik van de Linux HPC workloads in Azure, [technische bronnen voor batchverwerking en high performance computing](../../../batch/big-compute-resources.md).

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a>Een HPC Pack cluster met Linux-rekenknooppunten implementeren
Dit artikel ziet u twee opties voor het implementeren van een HPC Pack-cluster in Azure met Linux-rekenknooppunten. Beide methoden voor het gebruik van een Marketplace-installatiekopie van Windows Server met HPC Pack te maken van het hoofdknooppunt. 

* **Azure Resource Manager-sjabloon** -een sjabloon uit Azure Marketplace, of een sjabloon Quick Start van de community gebruiken voor het automatiseren van het maken van het cluster in het Resource Manager-implementatiemodel. Bijvoorbeeld, de [HPC Pack cluster voor Linux-werkbelastingen](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) sjabloon in Azure Marketplace maakt een complete HPC Pack clusterinfrastructuur voor Linux HPC werkbelastingen.
* **PowerShell-script** -gebruik de [Microsoft HPC Pack IaaS-implementatiescript](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**nieuw HpcIaaSCluster.ps1**) voor het automatiseren van een implementatie van het volledige cluster in het klassieke implementatiemodel. Deze Azure PowerShell-script maakt gebruik van een installatiekopie van een HPC Pack VM in Azure Marketplace voor snelle implementatie en biedt een uitgebreide set parameters voor de configuratie voor het implementeren van Linux-rekenknooppunten.

Zie voor meer informatie over opties voor de implementatie van HPC Pack cluster in Azure [opties voor het maken en beheren van een high performance computing (HPC)-cluster in Azure met Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="prerequisites"></a>Vereisten
* **Azure-abonnement** -kunt u een abonnement in de globale Azure of Azure China-service. Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.
* **Quotum voor kernen** -moet u mogelijk verhogen van het quotum van kernen, vooral als u kiest voor het implementeren van verschillende clusterknooppunten multicore VM-grootte. U kunt een quotum verhogen, door een ondersteuningsaanvraag online klant kosteloos te openen.
* **Linux-distributies** -momenteel HPC Pack ondersteunt de volgende Linux-distributies voor rekenknooppunten. U kunt Marketplace-versies van deze verdelingen gebruiken indien beschikbaar, of geef uw eigen.
  
  * **Op basis van centOS**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC
  * **Red Hat Enterprise Linux**: 6.7, 6,8, 7.2
  * **SUSE Linux Enterprise Server**: SLES 12 SLES 12 (Premium) SLES 12 SP1, SLES 12 SP1 (Premium) SLES 12 voor HPC SLES 12 voor HPC (Premium)
  * **Ubuntu Server**: 14.04 TNS, 16.04 TNS
    
    > [!TIP]
    > Geef voor het gebruik van het Azure RDMA-netwerk met een van de RDMA-compatibele VM-grootten, een SUSE Linux Enterprise Server 12 HPC of op basis van CentOS HPC-installatiekopie uit Azure Marketplace. Zie voor meer informatie [hoge prestaties compute-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

Aanvullende vereisten voor het implementeren van het cluster met behulp van het implementatiescript HPC Pack IaaS:

* **Clientcomputer** -moet u een Windows-clientcomputer om uit te voeren een script voor de implementatie van het cluster.
* **Azure PowerShell** - [installeren en configureren van Azure PowerShell](/powershell/azure/overview) (versie 0.8.10 of hoger) op de clientcomputer.
* **HPC Pack IaaS-implementatiescript** - downloaden en uitpakken van de meest recente versie van het script van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). U kunt de versie van het script controleren door te voeren `.\New-HPCIaaSCluster.ps1 –Version`. In dit artikel is gebaseerd op versie 4.4.1 of hoger van het script.

### <a name="deployment-option-1-use-a-resource-manager-template"></a>Implementatieoptie 1. Een Resource Manager-sjabloon gebruiken
1. Ga naar de [HPC Pack cluster voor Linux-werkbelastingen](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) sjabloon in de Azure Marketplace en op **implementeren**.
2. Lees de informatie in de Azure portal en klik vervolgens op **maken**.
   
    ![Maken van de portal][portal]
3. Op de **basisbeginselen** blade een naam voor het cluster, die ook de namen van de VM in het hoofdknooppunt. U kunt een bestaande resourcegroep kiezen of maken van een groep voor de implementatie op een locatie die voor u beschikbaar is. De locatie is van invloed op de beschikbaarheid van bepaalde VM-grootten en andere Azure-services (Zie [producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/services/)).
4. Op de **Head knooppunt instellingen** blade voor een eerste implementatie, kunt u in het algemeen de standaardinstellingen accepteren. 
   
   > [!NOTE]
   > De **post-configuratiescript URL** is een optionele instelling om op te geven van een openbaar beschikbare Windows PowerShell-script dat u uitvoeren op het hoofdknooppunt VM wilt nadat deze wordt uitgevoerd. 
   > 
   > 
5. Op de **Compute knooppunt instellingen** blade, selecteer een naming patroon voor de knooppunten, het aantal en de grootte van de knooppunten en de Linux-distributie te implementeren.
6. Op de **infrastructuurinstellingen** blade invoeren voor het virtuele netwerk en Active Directory-domein, domein en VM-beheerdersreferenties en naamgevingspatroon uit een voor de storage-accounts.
   
   > [!NOTE]
   > HPC Pack maakt gebruik van de Active Directory-domein om gebruikers van de cluster te verifiëren. 
   > 
   > 
7. Nadat de validatietests worden uitgevoerd en u de gebruiksvoorwaarden bekijken, klikt u op **aankoop**.

### <a name="deployment-option-2-use-the-iaas-deployment-script"></a>Implementatieoptie 2. Gebruik het script voor IaaS-implementatie
Hieronder vindt u aanvullende vereisten voor het implementeren van het cluster met behulp van het implementatiescript HPC Pack IaaS:

* **Clientcomputer** -moet u een Windows-clientcomputer om uit te voeren een script voor de implementatie van het cluster.
* **Azure PowerShell** - [installeren en configureren van Azure PowerShell](/powershell/azure/overview) (versie 0.8.10 of hoger) op de clientcomputer.
* **HPC Pack IaaS-implementatiescript** - downloaden en uitpakken van de meest recente versie van het script van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). U kunt de versie van het script controleren door te voeren `.\New-HPCIaaSCluster.ps1 –Version`. In dit artikel is gebaseerd op versie 4.4.1 of hoger van het script.

**XML-configuratiebestand**

Het implementatiescript HPC Pack IaaS maakt gebruik van een XML-configuratiebestand als invoer om te beschrijven de HPC-cluster. Het volgende voorbeeld-configuratiebestand bevat een kleine cluster die bestaan uit een hoofdknooppunt HPC Pack en twee grootte A7 CentOS 7.0 Linux-rekenknooppunten. 

Wijzigen van het bestand nodig zijn voor uw omgeving en de configuratie van het gewenste cluster en sla het bestand met een naam, zoals HPCDemoConfig.xml. Bijvoorbeeld, moet u de abonnementsnaam van uw en de naam van een uniek account op te geven en in de cloud servicenaam. U wilt ook kiezen een andere ondersteunde Linux-afbeelding voor de rekenknooppunten. Zie het bestand Manual.rtf in de scriptmap voor meer informatie over de elementen in het configuratiebestand en [een HPC-cluster maken met het implementatiescript HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

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

**Het implementatiescript HPC Pack IaaS uitvoeren**

1. Open Windows PowerShell op de clientcomputer als beheerder.
2. Wijzig de directory naar de map waarin het script is geïnstalleerd (E:\IaaSClusterScript in dit voorbeeld).
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. Voer de volgende opdracht voor het implementeren van het cluster HPC Pack. In dit voorbeeld wordt ervan uitgegaan dat het configuratiebestand in E:\HPCDemoConfig.xml bevinden zich
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    a. Omdat de **AdminPassword** niet is opgegeven in de voorgaande opdracht u wordt gevraagd het wachtwoord invoeren voor de gebruiker *MyAdminName*.
   
    b. Het script vervolgens valideren van het configuratiebestand wordt gestart. Het kan enkele minuten duren, afhankelijk van de netwerkverbinding.
   
    ![Validatie][validate]
   
    c. Nadat de validatie slaagt, worden het script de clusterbronnen maken. Voer *Y* om door te gaan.
   
    ![Resources][resources]
   
    d. Het script begint met de implementatie van het cluster HPC Pack en de configuratie zonder verdere handmatige stappen is voltooid. Het script kunt uitvoeren voor enkele minuten.
   
    ![Implementeren][deploy]
   
   > [!NOTE]
   > In dit voorbeeld genereert het script een logboekbestand automatisch aangezien de **- LogFile** parameter is niet opgegeven. De logboeken in realtime niet worden geschreven, maar worden verzameld aan het einde van de validatie en de implementatie. Als het PowerShell-proces wordt gestopt terwijl het script wordt uitgevoerd, is enkele logboeken gaan verloren.
   > 
   > 

## <a name="connect-to-the-head-node"></a>Verbinding maken met het hoofdknooppunt
Nadat u het HPC Pack cluster in Azure worden geïmplementeerd [verbinding maken met extern bureaublad](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) met het hoofdknooppunt VM gebruikmaken van het domein referenties u opgegeven bij de implementatie van het cluster (bijvoorbeeld *hpc\\clusteradmin*). U beheren het cluster vanaf het hoofdknooppunt.

Start op het hoofdknooppunt HPC Cluster Manager om de status van het cluster HPC Pack te controleren. U kunt beheren en bewaken Linux rekenknooppunten dezelfde manier als u werkt met Windows rekenknooppunten. Bijvoorbeeld, ziet u de Linux-knooppunten die worden vermeld in **bronbeheer** (deze knooppunten worden geïmplementeerd met de **LinuxNode** sjabloon).

![Knooppunt Management][management]

U ziet ook de Linux-knooppunten in de **Heatmap** weergeven.

![Heatmap][heatmap]

## <a name="how-to-move-data-in-a-cluster-with-linux-nodes"></a>Het verplaatsen van gegevens in een cluster met Linux-knooppunten
U hebt verschillende mogelijkheden om gegevens tussen Linux-knooppunten en het Windows-hoofdknooppunt van het cluster te verplaatsen. Hier volgen drie algemene methoden uitgebreid beschreven in de volgende secties:

* **Azure File** -beschrijft een beheerde SMB-bestandsshare voor het opslaan van gegevensbestanden in Azure-opslag. Knooppunten voor Windows en Linux-knooppunten kunnen koppelen een Azure-bestandsshare als een station of een map op hetzelfde moment, zelfs als ze zijn geïmplementeerd in verschillende virtuele netwerken.
* **Hoofdknooppunt SMB-share** -koppelt een standaard gedeelde Windows-map van het hoofdknooppunt op Linux-knooppunten.
* **HEAD knooppunt NFS-server** -biedt een oplossing voor het delen van bestanden voor een gemengde omgeving van Windows en Linux.

### <a name="azure-file-storage"></a>Azure File storage
De [Azure File](https://azure.microsoft.com/services/storage/files/) -service geeft bestandsshares met behulp van het standaard SMB 2.1-protocol. Azure virtuele machines en cloudservices kunnen bestandsgegevens delen tussen toepassingsonderdelen via gekoppelde shares en on-premises toepassingen hebben toegang tot bestandsgegevens in een share via de File storage-API. 

Zie voor gedetailleerde stappen voor een Azure-bestandsshare maken en koppelen van het hoofdknooppunt van het [aan de slag met Azure File storage in Windows](../../../storage/files/storage-how-to-use-files-windows.md). De Azure-bestandsshare koppelen op de Linux-knooppunten, Zie [Azure File storage gebruiken met Linux](../../../storage/files/storage-how-to-use-files-linux.md). Zie voor het persistent maken verbindingen instellen [Persisting verbindingen met Microsoft Azure-bestanden](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

In het volgende voorbeeld wordt een Azure-bestandsshare te maken van een opslagaccount. De share op het hoofdknooppunt koppelen, open een opdrachtprompt en voer de volgende opdrachten:

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

In dit voorbeeld allvhdsje is de naam van uw opslagaccount, storageaccountkey is de sleutel van uw opslagaccount en rdma is de sharenaam van de Azure-bestand. De Azure-bestandsshare is gekoppeld als Z: op het hoofdknooppunt.

Koppelen van de Azure-bestandsshare op Linux-knooppunten, voer een **clusrun** opdracht op het hoofdknooppunt. **[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  is een nuttig hulpmiddel HPC Pack bij het uitvoeren van beheertaken op meerdere knooppunten. (Zie ook [Clusrun voor Linux-knooppunten](#Clusrun-for-Linux-nodes) in dit artikel.)

Open een Windows PowerShell-venster en voer de volgende opdrachten:

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

De eerste opdracht maakt een map met de naam /rdma op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de allvhdsjw.file.core.windows.net/rdma Azure File share naar de map /rdma met dir en de bestandsmodus ingesteld op 777. In de tweede opdracht allvhdsje is de naam van uw opslagaccount en storageaccountkey is de sleutel van uw opslagaccount.

> [!NOTE]
> De '\`' symbool in de tweede opdracht is een escapeteken voor PowerShell. "\`, ' betekent dat de","(kommateken) een onderdeel van de opdracht is.
> 
> 

### <a name="head-node-share"></a>Hoofdknooppunt share
U kunt ook een gedeelde map van het hoofdknooppunt op Linux-knooppunten koppelen. Een share biedt de eenvoudigste manier om bestanden te delen, maar het hoofdknooppunt en alle Linux-knooppunten in hetzelfde virtuele netwerk moeten worden geïmplementeerd. Hier volgen de stappen.

1. Maak een map op het hoofdknooppunt en delen voor iedereen met machtigingen voor lezen/schrijven. Bijvoorbeeld D:\OpenFOAM delen op het hoofdknooppunt als \\CentOS7RDMA HN\OpenFOAM. Hier volgt CentOS7RDMA HN de hostnaam van het hoofdknooppunt.
   
    ![Machtigingen voor de bestandsshare][fileshareperms]
   
    ![Delen van bestanden][filesharing]
2. Open een Windows PowerShell-venster en voer de volgende opdrachten:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

De eerste opdracht maakt een map met de naam /openfoam op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de gedeelde map //CentOS7RDMA-HN/OpenFOAM naar de map met dir en de bestandsmodus ingesteld op 777. De gebruikersnaam en wachtwoord in de opdracht moet de gebruikersnaam en het wachtwoord van een gebruiker van het cluster op het hoofdknooppunt. (Zie [toevoegen of verwijderen cluster gebruikers](https://technet.microsoft.com/library/ff919330.aspx).)

> [!NOTE]
> De '\`' symbool in de tweede opdracht is een escapeteken voor PowerShell. "\`, ' betekent dat de","(kommateken) een onderdeel van de opdracht is.
> 
> 

### <a name="nfs-server"></a>NFS-server
De NFS-service kunt u delen en migreren van bestanden tussen de computers waarop het besturingssysteem van Windows Server 2012 met behulp van het SMB-protocol en Linux-computers met de NFS-protocol. De NFS-server en alle andere knooppunten moeten worden geïmplementeerd in hetzelfde virtuele netwerk. Dit biedt betere compatibiliteit met Linux-knooppunten vergeleken met een SMB-share. Bijvoorbeeld: het bestandskoppelingen ondersteunt.

1. Volg de stappen in wilt installeren en instellen van een NFS-server, [Server voor Network File System-eerste Share End-to-End](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).
   
    Maak bijvoorbeeld een NFS-share met de naam nfs met de volgende eigenschappen:
   
    ![NFS-autorisatie][nfsauth]
   
    ![NFS-sharemachtigingen][nfsshare]
   
    ![NFS-NTFS-machtigingen][nfsperm]
   
    ![Eigenschappen van de NFS-beheer][nfsmanage]
2. Open een Windows PowerShell-venster en voer de volgende opdrachten:
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   De eerste opdracht maakt een map met de naam /nfsshared op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de NFS-share CentOS7RDMA HN: / nfs naar de map. Hier CentOS7RDMA HN:-nfs is de externe pad van de NFS-share.

## <a name="how-to-submit-jobs"></a>Het verzenden van taken
Er zijn verschillende manieren om taken voor het cluster HPC Pack te verzenden:

* HPC Cluster Manager of HPC Job Manager GUI
* HPC-webportal
* REST API

Verzenden van de taak aan het cluster in Azure via HPC Pack GUI-hulpprogramma's en de HPC-webportal zijn hetzelfde als voor Windows rekenknooppunten. Zie [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) en [het verzenden van taken van een on-premises clientcomputer](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Raadpleeg voor het verzenden van taken via de REST-API [maken en verzenden van taken met behulp van de REST-API in Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). Voor het verzenden van taken van een Linux-client ook verwijzen naar het Python-voorbeeld in de [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).

## <a name="clusrun-for-linux-nodes"></a>Clusrun voor Linux-knooppunten
De HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) hulpprogramma kan worden gebruikt voor het uitvoeren van opdrachten op Linux-knooppunten via een opdrachtprompt of HPC Cluster Manager. Hier volgen enkele eenvoudige voorbeelden.

* Huidige gebruikersnamen weergeven op alle knooppunten in het cluster.
  
    ```command
    clusrun whoami
    ```
* Installeer de **gdb** foutopsporingsprogramma met **yum** op alle knooppunten in de linuxnodes groep en de knooppunten na 10 minuten opnieuw opstarten.
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* Maken van een shell-script weergeven van elk getal van 1 tot en met 10 voor één seconde op elke Linux-knooppunt in het cluster, uitvoeren en uitvoer van de knooppunten onmiddellijk weergeven.
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> Mogelijk moet u bepaalde escapetekens in gebruiken **clusrun** opdrachten. Zoals u in dit voorbeeld, gebruik ^ in een opdrachtprompt als escape voor de ' > ' symbool.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Probeer schalen van het cluster naar een groter aantal knooppunten, of een werkbelasting Linux wordt uitgevoerd op het cluster. Zie voor een voorbeeld [NAMD uitvoeren met Microsoft HPC Pack op Linux-rekenknooppunten in Azure](hpcpack-cluster-namd.md).
* Probeer een cluster met [RDMA-functionaliteit, rekenintensieve VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) MPI-workloads uitvoeren. Zie voor een voorbeeld [OpenFOAM uitvoeren met Microsoft HPC Pack op een Linux RDMA-cluster in Azure](hpcpack-cluster-openfoam.md).
* Als u geïnteresseerd bent in het werken met Linux-knooppunten in een on-premises HPC Pack-cluster, raadpleegt u de [TechNet richtlijnen](https://technet.microsoft.com/library/mt595803.aspx).

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
