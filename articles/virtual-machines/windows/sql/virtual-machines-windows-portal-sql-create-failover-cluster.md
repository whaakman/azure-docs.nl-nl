---
title: SQL Server FCI - virtuele Machines van Azure | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u SQL Server Failover Cluster Instance maken op Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/11/2018
ms.author: mikeray
ms.openlocfilehash: 3bb829e7cc99ee0d6e2d02f7ed3880d6c0226123
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486315"
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>Configureren van SQL Server-failovercluster-exemplaar op Azure Virtual Machines

In dit artikel wordt uitgelegd hoe u een SQL Server Failover Cluster exemplaar (FCI) maken op Azure virtual machines in Resource Manager-model. Deze oplossing maakt gebruik van [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\) ](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) als een op software gebaseerde virtuele SAN waarmee de opslag (gegevensschijven) worden gesynchroniseerd tussen de knooppunten (virtuele Azure-machines) in een Windows-Cluster. S2D is er nieuw in Windows Server 2016.

Het volgende diagram toont de volledige oplossing op Azure virtual machines:

![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

Het vorige diagram toont:

- Twee virtuele machines van Azure in een Windows-failovercluster. Wanneer een virtuele machine in een failovercluster wordt ook wel een *clusterknooppunt*, of *knooppunten*.
- Elke virtuele machine heeft twee of meer gegevensschijven.
- S2D synchroniseert de gegevens op de gegevensschijf en geeft de gesynchroniseerde opslag als een opslaggroep.
- De opslaggroep geeft een gedeeld clustervolume (CSV) aan het failovercluster.
- De rol van FCI van SQL Server-cluster maakt gebruik van de CSV voor de gegevensstations.
- Een Azure load balancer voor het opslaan van het IP-adres voor de SQL Server-FCI.
- Een Azure-beschikbaarheidsset bevat alle resources.

   >[!NOTE]
   >Alle Azure-resources worden in het diagram worden in dezelfde resourcegroep bevinden.

Zie voor meer informatie over S2D [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

S2D ondersteunt twee soorten architecturen - geconvergeerde en hyper-geconvergeerd. De architectuur in dit document is hyper-geconvergeerd. Een hyper-geconvergeerde infrastructuur plaatst de opslag op de dezelfde servers die als host van de geclusterde toepassing. In deze architectuur is de opslag op elk knooppunt FCI van SQL Server.

## <a name="licensing-and-pricing"></a>Licenties en prijzen

Op Azure Virtual Machines die u kunt SQL Server betalen (betalen per gebruik) met behulp van licentie of hun eigen licentie mee (BYOL) VM-installatiekopieën. Het type van de afbeelding die u kiest is van invloed op hoe u in rekening worden gebracht.

Met betalen per gebruik-licentieverlening is een failover clusterexemplaar (FCI) van SQL Server op Azure Virtual Machines leidt tot kosten in rekening gebracht voor alle knooppunten van FCI, met inbegrip van de passieve knooppunten. Zie voor meer informatie, [prijzen van SQL Server Enterprise Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/). 

Klanten met Enterprise-overeenkomst met Software Assurance heeft het recht op het gebruik van één gratis passieve FCI-knooppunt voor elke actieve knooppunt. BYOL-VM-installatiekopieën gebruiken om te profiteren van dit voordeel In Azure, en gebruik vervolgens de licentie op de actieve en passieve knooppunten van de FCI. Zie voor meer informatie, [Enterprise Agreement](https://www.microsoft.com/en-us/Licensing/licensing-programs/enterprise.aspx).

Betalen per gebruik en BYOL vergelijken licentiëring voor SQL Server op Azure Virtual Machines Zie [aan de slag met SQL-VM's](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms).

Zie voor meer informatie over licentieverlening SQL Server [prijzen](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

### <a name="example-azure-template"></a>Voorbeeld van de Azure-sjabloon

U kunt de gehele oplossing in Azure maken met een sjabloon. Een voorbeeld van een sjabloon is beschikbaar in de GitHub [Azure-Snelstartsjablonen](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad). In dit voorbeeld is niet ontworpen of getest voor een specifieke werkbelasting. U kunt de sjabloon voor het maken van een SQL Server-FCI met S2D-opslag is verbonden met uw domein uitvoeren. U kunt evalueren van de sjabloon en deze te wijzigen voor uw doeleinden.

## <a name="before-you-begin"></a>Voordat u begint

Er zijn enkele dingen die u nodig hebt en een aantal dingen die u nodig hebt voldaan voordat u doorgaat.

### <a name="what-to-know"></a>Wat u moet weten
U hebt een operationeel inzicht in de volgende technologieën:

- [Windows-clustertechnologieën](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server-failovercluster-exemplaren](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server).

Een belangrijk verschil is dat op een failovercluster voor Azure IaaS VM-Gast wordt aangeraden één NIC per server (clusterknooppunt) en één subnet. Azure-netwerken heeft fysieke redundantie extra NIC's en subnetten onnodige maakt op een Azure IaaS VM-gastcluster. Hoewel het clustervalidatierapport wordt een waarschuwing dat de knooppunten alleen bereikbaar is op één netwerk zijn uitgeeft, kan deze waarschuwing veilig worden genegeerd op Azure IaaS VM-gastclusters voor failover. 

Bovendien hebt u een algemeen begrip van de volgende technologieën:

- [Hypergeconvergeerde oplossing met opslagruimten Direct in Windows Server 2016](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [Azure-resourcegroepen](../../../azure-resource-manager/manage-resource-groups-portal.md)

> [!IMPORTANT]
> Op dit moment de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md) wordt niet ondersteund voor FCI van SQL Server op Azure. Het is raadzaam dat u de extensie van virtuele machines die deel uitmaken van de FCI verwijderen. Deze uitbreiding biedt ondersteuning voor functies, zoals geautomatiseerde back-ups en Patching en sommige portal functies voor SQL. Deze functies werken niet voor SQL-VM's nadat de agent is verwijderd.

### <a name="what-to-have"></a>Wat hebben

Voordat u de instructies in dit artikel, hebt u al:

- Een Microsoft Azure-abonnement.
- Een Windows-domein op Azure virtual machines.
- Een account met machtigingen voor het maken van objecten in de virtuele machine van Azure.
- Een Azure-netwerk en subnet met voldoende IP-adresruimte voor de volgende onderdelen:
   - Beide virtuele machines.
   - De failover-cluster IP-adres.
   - Een IP-adres voor elke FCI.
- DNS is geconfigureerd op het Azure-netwerk, die verwijst naar de domeincontrollers.

Met deze voorwaarden is voldaan, kunt u doorgaan met het ontwikkelen van uw failover-cluster. De eerste stap is het maken van de virtuele machines.

## <a name="step-1-create-virtual-machines"></a>Stap 1: Virtuele machines maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met uw abonnement.

1. [Maken van een Azure-beschikbaarheidsset](../tutorial-availability-sets.md).

   De beschikbaarheid van groepen virtuele machines instellen over foutdomeinen en updatedomeinen. De beschikbaarheidsset zorgt ervoor dat uw toepassing wordt niet beïnvloed door een single point of failure, zoals de netwerkswitch of de voedingseenheid van een rek met servers.

   Als u hebt niet de resourcegroep voor uw virtuele machines gemaakt, moet u het doet wanneer u een Azure-beschikbaarheidsset. Als u de Azure-portal gebruikt om te maken van de beschikbaarheidsset, voert u de volgende stappen uit:

   - Klik in de Azure-portal op **+** openen van de Azure Marketplace. Zoeken naar **beschikbaarheidsset**.
   - Klik op **beschikbaarheidsset**.
   - Klik op **Create**.
   - Op de **beschikbaarheidsset maken** blade, stel de volgende waarden:
      - **Naam**: Een naam voor de beschikbaarheidsset.
      - **Abonnement**: Uw Azure-abonnement.
      - **Resourcegroep**: Als u gebruiken van een bestaande groep wilt, klikt u op **gebruik bestaande** en selecteer de groep in de vervolgkeuzelijst. Kies anders **nieuw** en typ een naam voor de groep.
      - **Locatie**: Stel de locatie waar u van plan bent om uw virtuele machines te maken.
      - **Foutdomeinen**: Gebruik de standaardwaarde (3).
      - **Updatedomeinen**: Gebruik de standaardwaarde (5).
   - Klik op **maken** om te maken van de beschikbaarheid.

1. Maak de virtuele machines in de beschikbaarheidsset.

   Twee virtuele machines met SQL Server in de Azure-beschikbaarheidsset inrichten. Zie voor instructies [een SQL Server-machine inrichten in Azure portal](virtual-machines-windows-portal-sql-server-provision.md).

   Plaats beide virtuele machines:

   - In dezelfde Azure is resourcegroep die uw beschikbaarheidsset.
   - Op hetzelfde netwerk bevinden als uw domeincontroller.
   - In een subnet met voldoende IP-adresruimte voor zowel virtuele machines en alle Failoverclusterinstanties die u uiteindelijk kunt gebruiken voor dit cluster.
   - In de Azure-beschikbaarheidsset.   

      >[!IMPORTANT]
      >U kunt geen instellen of wijzigen van de beschikbaarheidsset nadat een virtuele machine is gemaakt.

   Kies een installatiekopie in de Azure Marketplace. U kunt een Marketplace-installatiekopie met die Windows Server en SQL Server of de Windows-Server bevat. Zie voor meer informatie, [overzicht van SQL Server op Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md)

   De officiële SQL Server-installatiekopieën in de Azure-galerie omvatten een geïnstalleerde SQL Server-exemplaar, plus de software van de installatie van SQL Server en de benodigde sleutel is.

   Kies de juiste installatiekopie op basis van hoe u wilt betalen voor de SQL Server-licentie:

   - **Betalen per gebruik-licentieverlening**: De kosten per seconde van deze installatiekopieën bevat de SQL Server-licentie:
      - **SQL Server 2016 Enterprise op Windows Server Datacenter 2016**
      - **SQL Server 2016 Standard op Windows Server Datacenter 2016**
      - **SQL Server 2016 Developer op Windows Server Datacenter 2016**

   - **Bring-your-own-license (BYOL)**

      - **{BYOL} SQL Server 2016 Enterprise op Windows Server Datacenter 2016**
      - **{BYOL} SQL Server 2016 Standard op Windows Server Datacenter 2016**

   >[!IMPORTANT]
   >Nadat u de virtuele machine hebt gemaakt, verwijdert u de vooraf geïnstalleerde zelfstandige SQL Server-exemplaar. U gebruikt de vooraf geïnstalleerde SQL Server-media te maken van de SQL Server-FCI nadat u de failover-cluster en S2D configureren.

   U kunt ook kunt u Azure Marketplace-installatiekopieën met alleen het besturingssysteem. Kies een **Windows Server 2016 Datacenter** installatiekopie en het installeren van de SQL Server-FCI nadat u de failover-cluster en S2D configureren. Deze installatiekopie bevat geen SQL Server-installatiemedium. Plaats het installatiemedium in een locatie waar u de installatie van SQL Server voor elke server kunt uitvoeren.

1. Nadat Azure uw virtuele machines maakt, moet u een verbinding maken met elke virtuele machine met RDP.

   Als u eerst verbinding met een virtuele machine met RDP, vraagt de computer als u wilt toestaan dat deze PC om te worden gedetecteerd op het netwerk. Klik op **Ja**.

1. Als u van een van de virtuele machine op basis van SQL Server-installatiekopieën gebruikmaakt, verwijdert u de SQL Server-exemplaar.

   - In **programma's en onderdelen**, met de rechtermuisknop op **Microsoft SQL Server 2016 (64-bits)** en klikt u op **verwijderen/wijzigen**.
   - Klik op **Verwijderen**.
   - Selecteer het standaardexemplaar plaatst.
   - Verwijder alle onderdelen onder **Database-Engineservices**. Verwijder niet **gedeelde onderdelen**. Zie de volgende afbeelding:

      ![Onderdelen verwijderen](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - Klik op **volgende**, en klik vervolgens op **verwijderen**.

1. <a name="ports"></a>De firewallpoorten openen.

   Op elke virtuele machine, de volgende poorten op de Windows Firewall te openen.

   | Doel | TCP-poort | Opmerkingen
   | ------ | ------ | ------
   | SQL Server | 1433 | Normale poort voor het standaardexemplaren van SQL Server. Als u een installatiekopie uit de galerie gebruikt, worden deze poort wordt automatisch geopend.
   | Statustest | 59999 | Alle open TCP-poort. Configureer de load balancer in een latere stap [statustest](#probe) en het cluster moet deze poort gebruiken.  

1. Opslag toevoegen aan de virtuele machine. Zie voor gedetailleerde informatie [opslag toevoegen](../disks-types.md).

   Beide virtuele machines moet ten minste twee schijven.

   Koppel onbewerkte schijven - geen NTFS-geformatteerde schijven.
      >[!NOTE]
      >Als u NTFS-geformatteerde schijven koppelt, kunt u alleen S2D inschakelen met geen schijf in aanmerking te komen controleren.  

   Minimaal twee premium SSD's koppelen aan elke virtuele machine. Het is raadzaam ten minste P30 (1 TB)-schijven.

   Set hostcache in op **alleen-lezen**.

   De opslagcapaciteit die u in productieomgevingen gebruikt, is afhankelijk van uw workload. De waarden die in dit artikel wordt beschreven, zijn voor demonstratie- en testdoeleinden.

1. [De virtuele machines toevoegen aan uw bestaande domein](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Nadat de virtuele machines worden gemaakt en geconfigureerd, kunt u het failovercluster configureren.

## <a name="step-2-configure-the-windows-failover-cluster-with-s2d"></a>Stap 2: Het Windows-failovercluster configureren met S2D

De volgende stap is het configureren van het failover-cluster met S2D. In deze stap maakt doet u de volgende substappen:

1. Windows Failover Clustering-functie toevoegen
1. Het cluster valideren
1. De failover-cluster maken
1. Maken van de cloud-witness
1. Opslag toevoegen

### <a name="add-windows-failover-clustering-feature"></a>Windows Failover Clustering-functie toevoegen

1. Als u wilt beginnen, verbinden met de eerste virtuele machine met RDP met behulp van een domeinaccount dat lid is van de lokale groep administrators en machtigingen heeft voor het maken van objecten in Active Directory. Dit account gebruiken voor de rest van de configuratie.

1. [Functie Failover Clustering toevoegen aan elke virtuele machine](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   Voor het onderdeel Failover Clustering installeren vanuit de gebruikersinterface, moet u de volgende stappen uit op beide virtuele machines.
   - In **Serverbeheer**, klikt u op **beheren**, en klik vervolgens op **functies en onderdelen toevoegen**.
   - In **functies Wizard Functies toevoegen en**, klikt u op **volgende** totdat u **onderdelen selecteren**.
   - In **onderdelen selecteren**, klikt u op **Failover Clustering**. Alle vereiste onderdelen en de beheerprogramma's opnemen. Klik op **functies toevoegen**.
   - Klik op **volgende** en klik vervolgens op **voltooien** om de functies te installeren.

   Als u wilt de functie Failover Clustering installeren met PowerShell, voer het volgende script uit een PowerShell-sessie op een van de virtuele machines.

   ```powershell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

Voor een verwijzing naar de volgende stappen volgt u de instructies onder de stap 3 van [hypergeconvergeerde oplossing met opslagruimten Direct in Windows Server 2016](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).

### <a name="validate-the-cluster"></a>Het cluster valideren

Deze handleiding verwijst naar de instructies onder [cluster valideren](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).

Valideren van het cluster in de gebruikersinterface of met PowerShell.

Voor het valideren van het cluster met de gebruikersinterface, moet u de volgende stappen uitvoeren vanaf een van de virtuele machines.

1. In **Serverbeheer**, klikt u op **extra**, klikt u vervolgens op **Failoverclusterbeheer**.
1. In **Failoverclusterbeheer**, klikt u op **actie**, klikt u vervolgens op **configuratie valideren...** .
1. Klik op **volgende**.
1. Op **Servers selecteren of een Cluster**, typ de naam van beide virtuele machines.
1. Op **testopties**, kiest u **alleen mij geselecteerde tests uitvoeren**. Klik op **volgende**.
1. Op **testen selectie**, bevatten alle tests uit behalve **opslag**. Zie de volgende afbeelding:

   ![Valideren van Tests](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. Klik op **volgende**.
1. Op **bevestiging**, klikt u op **volgende**.

De **Wizard een configuratie valideren** de validatietests wordt uitgevoerd.

Voor het valideren van het cluster met PowerShell, voer het volgende script uit een PowerShell-sessie op een van de virtuele machines.

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

Nadat u het cluster valideren, moet u het failovercluster maakt.

### <a name="create-the-failover-cluster"></a>De failover-cluster maken

Deze handleiding verwijst naar [het failovercluster maakt](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).

Als u wilt het failovercluster maakt, hebt u het volgende nodig:
- De namen van de virtuele machines die de clusterknooppunten.
- Een naam voor de failovercluster
- Een IP-adres voor de failover-cluster. U kunt een IP-adres dat niet wordt gebruikt op de dezelfde Azure-netwerk en subnet als de clusterknooppunten.

De volgende PowerShell maakt u een failover-cluster. Het script met de namen van de knooppunten (de namen van de virtuele machine) en een beschikbaar IP-adres van de Azure-VNET-update:

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a>Maken van een cloud-witness

Cloud-Witness is een nieuw type clusterquorum-witness die zijn opgeslagen in een Azure Storage-Blob. Hiermee verwijdert u de noodzaak van een afzonderlijke virtuele machine die als host fungeert voor een witness-share.

1. [Maken van een cloud-witness voor het failovercluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Een blob-container maken.

1. Sla de toegangssleutel en de URL van de container.

1. De failover clusterquorum-witness configureren. Zie, [de quorum-witness configureren in de gebruikersinterface](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) in de gebruikersinterface.

### <a name="add-storage"></a>Opslag toevoegen

De schijven voor S2D moeten worden leeggemaakt en alle partities of andere gegevens. Om op te schonen schijven Volg [de stappen in deze handleiding](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).

1. [Schakel Store opslagruimten Direct \(S2D\)](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   Met behulp van de volgende PowerShell kunt opslagruimten direct.  

   ```powershell
   Enable-ClusterS2D
   ```

   In **Failoverclusterbeheer**, ziet u nu de opslaggroep.

1. [Maak een volume](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   Een van de functies van S2D is dat deze automatisch wordt een opslaggroep gemaakt wanneer u deze inschakelt. U bent nu klaar om te maken van een volume. De PowerShell-commandlet `New-Volume` automatiseert het aanmaakproces volume, met inbegrip van opmaak, toe te voegen aan het cluster en het maken van een gedeeld clustervolume (CSV). Het volgende voorbeeld wordt een 800 GB (Gigabyte) CSV.

   ```powershell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   Nadat u deze opdracht is voltooid, wordt een 800 GB-volume is gekoppeld als een cluster-bron. Het volume is op `C:\ClusterStorage\Volume1\`.

   Het volgende diagram toont een gedeeld clustervolume met S2D:

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>Stap 3: Failover clusterfailover testen

In Failoverclusterbeheer, Controleer of dat u de storage-resource naar het andere clusterknooppunt verplaatsen kunt. Als u verbinding met het failover-cluster met maken kunt **Failoverclusterbeheer** en de opslag verplaatsen van het ene knooppunt naar het andere, u bent klaar om te configureren van de FCI.

## <a name="step-4-create-sql-server-fci"></a>Stap 4: Maken van SQL Server FCI

Nadat u de failover-cluster en alle clusteronderdelen van de, inclusief opslag hebt geconfigureerd, kunt u de SQL Server-FCI maken.

1. Verbinding maken met de eerste virtuele machine met RDP.

1. In **Failoverclusterbeheer**, zorg ervoor dat alle clusterkernresources zijn op de eerste virtuele machine. Indien nodig, moet u alle resources verplaatsen naar deze virtuele machine.

1. Ga naar de installatiemedia. Als de virtuele machine gebruikmaakt van een van de Azure Marketplace-installatiekopieën, de media bevindt zich in `C:\SQLServer_<version number>_Full`. Klik op **Setup**.

1. In de **Installatiecentrum van SQL Server**, klikt u op **installatie**.

1. Klik op **New SQL Server failover cluster installation**. Volg de instructies in de wizard voor het installeren van de SQL Server-FCI.

   De gegevensmappen FCI moeten op geclusterde opslag. Met S2D is het niet een gedeelde schijf, maar een koppelpunt op een volume op elke server. S2D synchroniseert het volume tussen beide knooppunten. Het volume wordt gepresenteerd aan het cluster een cluster shared volume. Gebruik het CSV-koppelpunt voor de gegevensmappen.

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. Nadat u de wizard hebt voltooid, zal Setup de FCI van een SQL-Server installeren op het eerste knooppunt.

1. Nadat Setup is de FCI is geïnstalleerd op het eerste knooppunt, moet u een verbinding maken met het tweede knooppunt met RDP.

1. Open de **Installatiecentrum van SQL Server**. Klik op **installatie**.

1. Klik op **knooppunt toevoegen aan een SQL Server-failovercluster**. Volg de instructies in de wizard installatie van SQL server en deze server toevoegen aan de FCI.

   >[!NOTE]
   >Als u de installatiekopie van een Azure Marketplace-galerie met SQL Server gebruikt, is SQL Server-hulpprogramma's zijn opgenomen in de afbeelding. Als u deze installatiekopie niet hebt gebruikt, installeert u de SQL Server-hulpprogramma's afzonderlijk. Zie [downloaden van SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-azure-load-balancer"></a>Stap 5: Azure-load balancer maken

Op Azure virtual machines, clusters een load balancer gebruiken voor het opslaan van een IP-adres dat moet worden op één clusterknooppunt tegelijk. In deze oplossing bevat de load balancer het IP-adres voor de SQL Server-FCI.

[Maken en configureren van een Azure load balancer](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>De load balancer maken in Azure portal

De load balancer maken:

1. In de Azure-portal, gaat u naar de resourcegroep met de virtuele machines.

1. Klik op **+ Toevoegen**. Zoek in Marketplace naar **Load Balancer**. Klik op **Load Balancer**.

1. Klik op **Create**.

1. Configureer de load balancer met:

   - **Naam**: Een naam ter identificatie van de load balancer.
   - **Type**: De load balancer kan worden openbaar of privé zijn. Een privéadres voor de load balancer kunt toegankelijk vanaf binnen hetzelfde VNET. De meeste Azure-toepassingen gebruiken een privéadres voor de load balancer. Als uw toepassing toegang tot SQL Server rechtstreeks via Internet moet, gebruikt u een openbare load balancer.
   - **Virtueel netwerk**: Hetzelfde netwerk bevinden als de virtuele machines.
   - **Subnet**: Hetzelfde subnet bevindt als de virtuele machines.
   - **Privé IP-adres**: Hetzelfde IP-adres dat u hebt toegewezen aan de netwerkbron van FCI van SQL Server-cluster.
   - **Abonnement**: Uw Azure-abonnement.
   - **Resourcegroep**: Gebruik dezelfde resourcegroep bevinden als uw virtuele machines.
   - **Locatie**: Gebruik dezelfde Azure-locatie als uw virtuele machines.
   Zie de volgende afbeelding:

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-the-load-balancer-backend-pool"></a>De back-endpool van de load balancer configureren

1. Ga terug naar de Azure-resourcegroep met de virtuele machines en zoek de nieuwe load balancer. U moet Vernieuw de weergave voor de resourcegroep. Klik op de load balancer.

1. Klik op **back-endpools** en klikt u op **+ toevoegen** om toe te voegen een back-endpool.

1. Back-end-pool koppelen aan de beschikbaarheidsset met de virtuele machines.

1. Onder **doel-IP-configuraties van**, Controleer **virtuele MACHINE** en kiest u de virtuele machines die wordt opgenomen als clusterknooppunten. Moet u alle virtuele machines die als voor de FCI host opnemen. 

1. Klik op **OK** om de back endpool te maken.

### <a name="configure-a-load-balancer-health-probe"></a>Een load balancer-statustest configureren

1. Klik op de blade van de load balancer **statuscontroles**.

1. Klik op **+ Toevoegen**.

1. Op de **test toevoegen** blade <a name="probe"> </a>de status test parameters instellen:

   - **Naam**: Een naam voor de statustest.
   - **Protocol**: TCP.
   - **Poort**: Ingesteld op een beschikbare TCP-poort. Deze poort is een open firewallpoort vereist. Gebruik de [dezelfde poort](#ports) u op de firewall instellen voor de statustest.
   - **Interval**: 5 seconden.
   - **Drempelwaarde voor onjuiste status**: 2 achtereenvolgende mislukte pogingen.

1. Klik op OK.

### <a name="set-load-balancing-rules"></a>Instellen van de load balancer-regels

1. Klik op de blade van de load balancer **Taakverdelingsregels**.

1. Klik op **+ Toevoegen**.

1. De load balancer-regels parameters instellen:

   - **Naam**: Een naam voor de load balancer-regels.
   - **Frontend-IP-adres**: Gebruik het IP-adres voor de netwerkbron van FCI van SQL Server-cluster.
   - **Poort**: Voor de SQL Server FCI TCP-poort ingesteld. De exemplaar-standaardpoort is 1433.
   - **Back-endpoort**: Deze waarde gebruikt dezelfde poort als de **poort** wanneer u de waarde **zwevend IP (direct server return)**.
   - **Back-end-pool**: Gebruik de naam van de back-end-toepassingen die u eerder hebt geconfigureerd.
   - **Statustest**: Gebruik de statustest die u eerder hebt geconfigureerd.
   - **Sessiepersistentie**: Geen.
   - **Time-out (minuten)**: 4.
   - **Zwevend IP (direct server return)**: Ingeschakeld

1. Klik op **OK**.

## <a name="step-6-configure-cluster-for-probe"></a>Stap 6: Voor test-cluster configureren

Stel de parameter van cluster test poort in PowerShell.

Update variabelen in het volgende script met waarden van uw omgeving om in te stellen de parameter cluster test poort. Verwijder de punthaken `<>` van het script. 

   ```powershell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

In het vorige script, stel de waarden voor uw omgeving. De volgende lijst beschrijft de waarden:

   - `<Cluster Network Name>`: Windows Server Failover Cluster-naam voor het netwerk. In **Failoverclusterbeheer** > **netwerken**, met de rechtermuisknop op het netwerk en klikt u op **eigenschappen**. De juiste waarde ligt onder de **naam** op de **algemene** tabblad. 

   - `<SQL Server FCI IP Address Resource Name>`: Naam voor de resource van SQL Server FCI IP-adres. In **Failoverclusterbeheer** > **rollen**, onder de functie SQL Server FCI onder **servernaam**, klik met de rechtermuisknop op de resource van het IP-adres en klikt u op **Eigenschappen**. De juiste waarde ligt onder de **naam** op de **algemene** tabblad. 

   - `<ILBIP>`: Het ILB IP-adres. Dit adres is geconfigureerd in Azure portal als de front-end-ILB-adres. Dit is ook het SQL Server FCI IP-adres. U vindt deze in **Failoverclusterbeheer** op dezelfde Eigenschappenpagina waar u zich de `<SQL Server FCI IP Address Resource Name>`.  

   - `<nnnnn>`: Is de testpoort die u hebt geconfigureerd in de load balancer-statustest. Een niet-gebruikte TCP-poort is ongeldig. 

>[!IMPORTANT]
>Het subnetmasker voor de cluster-parameter moet het TCP IP broadcast-adres: `255.255.255.255`.

Na het instellen van de cluster-test ziet u alle van de clusterparameters in PowerShell. Voer het volgende script uit:

   ```powershell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter 
  ```

## <a name="step-7-test-fci-failover"></a>Stap 7: Testfailover FCI

Test-failover van de FCI clusterfunctionaliteit valideren. Voer de volgende stappen uit:

1. Verbinding maken met een van de SQL Server FCI-clusterknooppunten met RDP.

1. Open **Failoverclusterbeheer**. Klik op **rollen**. U ziet het welk knooppunt eigenaar is van de SQL Server FCI-rol.

1. Met de rechtermuisknop op de SQL Server FCI-rol.

1. Klik op **verplaatsen** en klikt u op **Best mogelijke knooppunt**.

**Failover-clusterbeheer** ziet u de functie en de daarbij behorende bronnen offline gaan. Verplaats vervolgens de resources en deze op het andere knooppunt online is gekomen.

### <a name="test-connectivity"></a>Connectiviteit testen

Connectiviteit wilt testen, moet u zich aanmelden bij een andere virtuele machine in hetzelfde virtuele netwerk. Open **SQL Server Management Studio** en maak verbinding met de naam van de SQL Server FCI.

>[!NOTE]
>Zo nodig u kunt [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Beperkingen

Azure virtuele Machines ondersteunen Microsoft Distributed Transaction Coordinator (MSDTC) op Windows Server 2019 met opslag op geclusterde gedeelde volumes (CSV) en een [standaardversie van load balancer](../../../load-balancer/load-balancer-standard-overview.md).

Op Azure virtual machines, wordt MSDTC niet ondersteund op Windows Server 2016 en eerdere versies omdat:

- De geclusterde MSDTC-bron kan niet worden geconfigureerd voor het gebruik van gedeelde opslag. Met Windows Server 2016 als u een MSDTC-resource, maakt weergegeven deze niet gedeelde opslag beschikbaar voor gebruik, zelfs als de opslag er is. Dit probleem is verholpen in Windows Server 2019.
- De basisversie van load balancer verwerkt RPC-poorten niet.

## <a name="see-also"></a>Zie ook

[S2D instellen met extern bureaublad (Azure)](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

[Hypergeconvergeerde oplossing met opslagruimten direct](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).

[Directe overzicht](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[SQL Server-ondersteuning voor S2D](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
