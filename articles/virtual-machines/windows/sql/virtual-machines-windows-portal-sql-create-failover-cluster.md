---
title: SQL Server FCI - virtuele Machines in Azure | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u SQL Server Failover Cluster Instance maken op Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 09/26/2017
ms.author: mikeray
ms.openlocfilehash: ec35b4a02c04d5b6d0bbf9049927529258c3825b
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>Exemplaar van SQL Server-failovercluster configureren op virtuele Machines in Azure

In dit artikel wordt uitgelegd hoe u een SQL Server Failover Cluster Instance (FCI) maken op Azure virtuele machines in de Resource Manager-model. Deze oplossing gebruikt [Windows Server 2016 Datacenter edition opslagruimten Direct \(S2D\) ](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) als een op software gebaseerde virtueel SAN waarmee de opslag (gegevensschijven) tussen de knooppunten (virtuele Azure-machines) in een Windows-Cluster worden gesynchroniseerd. S2D is nieuw in Windows Server 2016.

Het volgende diagram ziet u de volledige oplossing op virtuele machines in Azure:

![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

Het voorgaande diagram geeft:

- Twee virtuele Azure-machines in een Windows-failovercluster. Wanneer een virtuele machine in een failovercluster wordt ook wel een *clusterknooppunt*, of *knooppunten*.
- Elke virtuele machine heeft twee of meer gegevensschijven.
- S2D synchroniseert de gegevens op de gegevensschijf en geeft de gesynchroniseerde opslag als een opslaggroep.
- De opslaggroep geeft een gedeeld clustervolume (CSV) aan het failovercluster.
- De rol FCI van SQL Server-cluster maakt gebruik van de CSV voor de gegevensstations.
- Een Azure load balancer om het IP-adres voor de SQL Server-Failoverclusterinstanties.
- Een Azure beschikbaarheidsset bevat alle resources.

   >[!NOTE]
   >Alle Azure-resources zijn in het diagram zich in dezelfde resourcegroep.

Zie voor meer informatie over S2D [Windows Server 2016 Datacenter edition opslagruimten Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

S2D ondersteunt twee soorten architecturen - geconvergeerde en hyper-geconvergeerde. De architectuur in dit document is hyper-geconvergeerde. Een hyper-geconvergeerde infrastructuur plaatst u de opslag op dezelfde servers die als host fungeren van de geclusterde toepassing. De opslag is in deze architectuur op elk knooppunt FCI van SQL Server.

### <a name="example-azure-template"></a>Voorbeeld van de Azure-sjabloon

U kunt de volledige oplossing in Azure maken van een sjabloon. Een voorbeeld van een sjabloon is beschikbaar in de GitHub [Azure-Snelstartsjablonen](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad). In dit voorbeeld is niet ontworpen of getest voor een specifieke werkbelasting. U kunt de sjabloon voor het maken van een SQL Server-FCI met S2D opslag is verbonden met uw domein kunt uitvoeren. U kunt evalueren van de sjabloon en aanpassen voor uw doeleinden.

## <a name="before-you-begin"></a>Voordat u begint

Er zijn een aantal dingen die u nodig hebt en een aantal dingen die u nodig hebt voldaan voordat u doorgaat.

### <a name="what-to-know"></a>Wat u moet weten
U hebt een operationeel inzicht in de volgende technologieën:

- [Windows cluster-technologieën](http://technet.microsoft.com/library/hh831579.aspx)
-  [Exemplaren van SQL Server-failovercluster](http://msdn.microsoft.com/library/ms189134.aspx).

Bovendien hebt u een algemeen begrip van de volgende technologieën:

- [Hyper-geconvergeerde oplossing met behulp van opslagruimten Direct in Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [Azure-resourcegroepen](../../../azure-resource-manager/resource-group-portal.md)

### <a name="what-to-have"></a>Wat u moet hebben

Voordat u de instructies in dit artikel, hebt u al:

- Een Microsoft Azure-abonnement.
- Een Windows-domein op virtuele machines in Azure.
- Een account met machtigingen om objecten te maken in de virtuele machine van Azure.
- Een virtuele Azure-netwerk en subnet met voldoende IP-adresruimte voor de volgende onderdelen:
   - Beide virtuele machines.
   - Failover-IP-adres van het cluster.
   - Een IP-adres voor elke FCI.
- DNS wordt geconfigureerd op het netwerk van Azure, die verwijst naar de domeincontrollers.

Met deze vereisten is voldaan, kunt u doorgaan met het bouwen van uw failover-cluster. De eerste stap is het maken van de virtuele machines.

## <a name="step-1-create-virtual-machines"></a>Stap 1: Virtuele machines maken

1. Meld u aan bij de [Azure-portal](http://portal.azure.com) met uw abonnement.

1. [Maken van een Azure beschikbaarheidsset](../tutorial-availability-sets.md).

   De beschikbaarheid van de virtuele machines voor groepen instellen voor domeinen met fouten en domeinen bijwerken. De beschikbaarheidsset zorgt ervoor dat uw toepassing is niet van invloed op een individuele foutpunten, zoals de netwerkswitch of de energie-eenheid van een rek met servers.

   Als u hebt niet de resourcegroep gemaakt voor uw virtuele machines, moet u het doen wanneer u een Azure beschikbaarheidsset maakt. Als u de Azure-portal voor het maken van de beschikbaarheidsset, gaat u de volgende stappen uit:

   - Klik in de Azure-portal op  **+**  openen Azure Marketplace. Zoeken naar **beschikbaarheidsset**.
   - Klik op **beschikbaarheidsset**.
   - Klik op **Create**.
   - Op de **beschikbaarheidsset maken** blade, stel de volgende waarden:
      - **Naam**: een naam voor de beschikbaarheidsset.
      - **Abonnement**: uw Azure-abonnement.
      - **Resourcegroep**: als u gebruiken van een bestaande groep wilt, klikt u op **gebruik bestaande** en selecteer de groep in de vervolgkeuzelijst. Kies anders **nieuw** en typ een naam voor de groep.
      - **Locatie**: Stel de locatie waar u van plan bent uw virtuele machines maken.
      - **Fault-domeinen**: Gebruik de standaardwaarde (3).
      - **Bijwerken van domeinen**: Gebruik de standaardwaarde (5).
   - Klik op **maken** ingesteld voor het maken van de beschikbaarheid.

1. Maak de virtuele machines in de beschikbaarheidsset.

   Twee SQL Server-virtuele machines in de Azure beschikbaarheidsset inrichten. Zie voor instructies [een SQL Server-machine inrichten in de Azure-portal](virtual-machines-windows-portal-sql-server-provision.md).

   Plaats beide virtuele machines:

   - In de dezelfde Azure is resourcegroep die uw beschikbaarheidsset.
   - Op hetzelfde netwerk bevindt als uw domeincontroller.
   - In een subnet met voldoende IP-adresruimte voor virtuele machines en alle Failoverclusterinstanties die u uiteindelijk op dit cluster kan gebruiken.
   - In de Azure beschikbaarheidsset.   

      >[!IMPORTANT]
      >U niet instellen of wijzigen van de beschikbaarheidsset nadat een virtuele machine is gemaakt.

   Een afbeelding kiezen die vanuit Azure Marketplace. U kunt een Marketplace installatiekopie met die Windows Server en SQL Server of de Windows-Server bevat. Zie voor meer informatie [overzicht van SQL Server op Azure Virtual Machines](../../virtual-machines-windows-sql-server-iaas-overview.md)

   De officiële SQL Server-afbeeldingen in de Azure-galerie omvatten een geïnstalleerde SQL Server-exemplaar, plus de SQL Server-installatie van software en de sleutel.

   Kies de juiste installatiekopie volgens hoe wilt u betalen voor de SQL Server-licentie:

   - **Betalen per gebruik licentieverlening**: de kosten per minuut van deze installatiekopieën bevat de SQL Server-licentieverlening:
      - **SQL Server 2016 Enterprise op Windows Server Datacenter 2016**
      - **SQL Server 2016 Standard op Windows Server Datacenter 2016**
      - **SQL Server 2016 Developer op Windows Server Datacenter 2016**

   - **Bring-your-eigenaar-license (BYOL)**

      - **{BYOL} SQL Server 2016 Enterprise op Windows Server Datacenter 2016**
      - **{BYOL} SQL Server 2016 Standard op Windows Server Datacenter 2016**

   >[!IMPORTANT]
   >Nadat u de virtuele machine hebt gemaakt, verwijdert u de vooraf geïnstalleerde zelfstandige SQL Server-exemplaar. De vooraf geïnstalleerde SQL Server-media gebruikt u de SQL Server-FCI maken nadat u de failover-cluster en S2D configureren.

   U kunt ook Azure Marketplace-installatiekopieën gebruiken met alleen het besturingssysteem. Kies een **Windows Server 2016 Datacenter** installatiekopie en de SQL Server-FCI installeren nadat u de failover-cluster en S2D configureren. Deze installatiekopie bevat geen SQL Server-installatiemedia. Plaats de installatiemedia in een locatie waar u de installatie van SQL Server voor elke server kunt uitvoeren.

1. Nadat uw virtuele machines is gemaakt in Azure, verbinding maken met elke virtuele machine met RDP.

   Wanneer u de eerste verbinding met een virtuele machine met RDP, vraagt de computer als u wilt toestaan dat deze PC om te worden gedetecteerd op het netwerk. Klik op **Ja**.

1. Als u van een van de virtuele machine op basis van SQL Server-installatiekopieën gebruikmaakt, verwijdert u de SQL Server-exemplaar.

   - In **programma's en onderdelen**, met de rechtermuisknop op **Microsoft SQL Server 2016 (64-bits)** en klik op **verwijderen/wijzigen**.
   - Klik op **verwijderen**.
   - Selecteer het standaardexemplaar.
   - Verwijder alle onderdelen onder **Database Engine-Services**. Verwijder niet **gedeelde onderdelen**. Zie de volgende afbeelding:

      ![Onderdelen verwijderen](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - Klik op **volgende**, en klik vervolgens op **verwijderen**.

1. <a name="ports"></a>Open de firewall-poorten.

   Open de volgende poorten op de Windows Firewall op elke virtuele machine.

   | Doel | TCP-poort | Opmerkingen
   | ------ | ------ | ------
   | SQL Server | 1433 | Normale poort voor standaardexemplaren van SQL Server. Als u een installatiekopie van het uit de galerie gebruikt, worden deze poort wordt automatisch geopend.
   | De statuscontrole | 59999 | Een open TCP-poort. Configureer de load balancer in een latere stap [health test](#probe) en het cluster om deze poort te gebruiken.  

1. Opslag toevoegen aan de virtuele machine. Zie voor gedetailleerde informatie [opslag toevoegen](../premium-storage.md).

   Beide virtuele machines nodig ten minste twee schijven.

   Koppel onbewerkte schijven - geen NTFS-geformatteerde schijven.
      >[!NOTE]
      >Als u NTFS-geformatteerde schijven koppelt, kunt u alleen S2D inschakelen met geen controle van de schijf in aanmerking komt.  

   Minimaal twee Premium-opslag (SSD-schijven) koppelen aan elke virtuele machine. Het is raadzaam ten minste P30 schijven (1 TB).

   Set-hostcaching naar **alleen-lezen**.

   De opslagcapaciteit die u in productieomgevingen, is afhankelijk van uw workload. De waarden die worden beschreven in dit artikel zijn voor de demonstratie en testen.

1. [De virtuele machines toevoegen aan uw bestaande domein](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Nadat de virtuele machines worden gemaakt en geconfigureerd, kunt u het failover-cluster configureren.

## <a name="step-2-configure-the-windows-failover-cluster-with-s2d"></a>Stap 2: De Windows-failovercluster met S2D configureren

De volgende stap is het failover-cluster met S2D configureren. In deze stap doet u het volgende substappen:

1. Windows Failover Clustering-functie toevoegen
1. Het cluster valideren
1. De failover-cluster maken
1. Maken van de cloud-witness
1. Opslag toevoegen

### <a name="add-windows-failover-clustering-feature"></a>Windows Failover Clustering-functie toevoegen

1. Om te beginnen, verbinding met de eerste virtuele machine met RDP met behulp van een domeinaccount dat lid is van de lokale groep administrators en machtigingen heeft voor het maken van objecten in Active Directory. Dit account wordt gebruikt voor de rest van de configuratie.

1. [Functie Failover Clustering toevoegen aan elke virtuele machine](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   Voor het onderdeel Failover Clustering installeren vanuit de gebruikersinterface, komen de volgende stappen uit op beide virtuele machines.
   - In **Serverbeheer**, klikt u op **beheren**, en klik vervolgens op **functies en onderdelen toevoegen**.
   - In **Wizard Functies toevoegen en onderdelen**, klikt u op **volgende** totdat u **onderdelen selecteren**.
   - In **onderdelen selecteren**, klikt u op **Failoverclustering**. Alle vereiste onderdelen en de beheerhulpprogramma's bevatten. Klik op **functies toevoegen**.
   - Klik op **volgende** en klik vervolgens op **voltooien** functies worden geïnstalleerd.

   Als u wilt de functie Failover Clustering installeren met PowerShell, voer het volgende script van een beheerder PowerShell-sessie op een van de virtuele machines.

   ```PowerShell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

Voor een verwijzing naar de volgende stappen Volg de aanwijzingen in stap 3 van [Hyper-geconvergeerde oplossing met behulp van opslagruimten Direct in Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).

### <a name="validate-the-cluster"></a>Het cluster valideren

Deze handleiding verwijst naar instructies onder [cluster valideren](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).

Valideren van het cluster in de gebruikersinterface of met PowerShell.

Voor het valideren van het cluster met de gebruikersinterface van de volgende handelingen uit een van de virtuele machines.

1. In **Serverbeheer**, klikt u op **extra**, klikt u vervolgens op **Failoverclusterbeheer**.
1. In **Failoverclusterbeheer**, klikt u op **actie**, klikt u vervolgens op **configuratie valideren...** .
1. Klik op **Volgende**.
1. Op **Servers selecteren of een Cluster**, typ de naam van beide virtuele machines.
1. Op **testopties**, kies **alleen mij geselecteerde tests uitvoeren**. Klik op **Volgende**.
1. Op **selectie testen**, nemen alle tests uit behalve **opslag**. Zie de volgende afbeelding:

   ![Valideren van Tests](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. Klik op **Volgende**.
1. Op **bevestiging**, klikt u op **volgende**.

De **Wizard een configuratie valideren** de validatietests wordt uitgevoerd.

Voer het volgende script van een beheerder PowerShell-sessie op een van de virtuele machines voor het valideren van het cluster met PowerShell.

   ```PowerShell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

Nadat u het cluster valideren, moet u het failovercluster maakt.

### <a name="create-the-failover-cluster"></a>De failover-cluster maken

Deze handleiding verwijst naar [het failovercluster maakt](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).

Voor het maken van het failover-cluster, hebt u het volgende nodig:
- De namen van de virtuele machines die de clusterknooppunten worden.
- Een naam voor de failovercluster
- Een IP-adres voor de failover-cluster. U kunt een IP-adres dat niet wordt gebruikt in hetzelfde virtuele Azure-netwerk en subnet bevindt als de clusterknooppunten.

De volgende PowerShell maakt een failover-cluster. Het script met de namen van de knooppunten (de namen van de virtuele machine) en een beschikbaar IP-adres van de Azure-VNET-update:

```PowerShell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a>Maken van een cloud-witness

Cloud-Witness is een nieuw type clusterquorum-witness is opgeslagen in een Azure Storage-Blob. Hiermee verwijdert u de noodzaak van een afzonderlijke virtuele machine die als host fungeert voor een witness-share.

1. [Maken van een cloud-witness voor het failovercluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Maak een blob-container.

1. Sla de toegangstoetsen en de URL van de container.

1. De failover-cluster clusterquorum-witness configureren. Zie, [de quorumwitness configureren in de gebruikersinterface]. (http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) in de gebruikersinterface.

### <a name="add-storage"></a>Opslag toevoegen

De schijven voor de S2D moeten leeg zijn en zonder partities of andere gegevens. Op te schonen schijven Volg [de stappen in deze handleiding](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).

1. [Store inschakelen opslagruimten Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   De volgende PowerShell kunt opslagruimten direct.  

   ```PowerShell
   Enable-ClusterS2D
   ```

   In **Failoverclusterbeheer**, ziet u nu de opslaggroep.

1. [Een volume maken](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   Een van de functies van S2D is dat er een opslaggroep automatisch gemaakt wanneer u deze functie inschakelen. U bent nu klaar om een volume te maken. De PowerShell-cmdlet `New-Volume` automatiseert het maakproces volume, met inbegrip van de opmaak, toevoegen aan het cluster en het maken van een gedeeld clustervolume (CSV). Het volgende voorbeeld wordt een 800 GB (Gigabyte) CSV.

   ```PowerShell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   Nadat u deze opdracht is voltooid, wordt een 800 GB-volume als clusterbron gekoppeld. Het volume is op `C:\ClusterStorage\Volume1\`.

   Het volgende diagram ziet u een gedeeld clustervolume met S2D:

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>Stap 3: Failover clusterfailover testen

In Failoverclusterbeheer, Controleer of dat u de opslagbronnen naar het andere clusterknooppunt verplaatsen kunt. Als u verbinding met het failover-cluster met maken kunt **Failoverclusterbeheer** en de opslag van het ene knooppunt verplaatsen naar de andere, bent u klaar voor het configureren van de FCI.

## <a name="step-4-create-sql-server-fci"></a>Stap 4: SQL Server FCI maken

Nadat u het failovercluster en alle clusteronderdelen inclusief opslag hebt geconfigureerd, kunt u de SQL Server-FCI maken.

1. Verbinding maken met de eerste virtuele machine met RDP.

1. In **Failoverclusterbeheer**, zorg ervoor dat alle clusterkernresources zijn op de eerste virtuele machine. Indien nodig, verplaatst u alle resources voor deze virtuele machine.

1. Zoek de installatiemedia. Als de virtuele machine gebruikmaakt van een van de Azure Marketplace-installatiekopieën, de media bevindt zich op `C:\SQLServer_<version number>_Full`. Klik op **Setup**.

1. In de **Installatiecentrum van SQL Server**, klikt u op **installatie**.

1. Klik op **New SQL Server failover cluster installation**. Volg de instructies in de wizard voor het installeren van de SQL Server-Failoverclusterinstanties.

   De gegevensmappen FCI moeten op geclusterde opslag. Met S2D is het niet een gedeelde schijf, maar een koppelpunt op een volume op elke server. S2D synchroniseert het volume van beide knooppunten. Het volume wordt gepresenteerd aan het cluster een cluster shared volume. Gebruik het koppelpunt CSV voor de gegevensmappen.

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. Nadat u de wizard hebt voltooid, installeert Setup een FCI SQL Server op het eerste knooppunt.

1. Nadat Setup is het FCI op het eerste knooppunt is geïnstalleerd, moet u een verbinding maken met het tweede knooppunt met RDP.

1. Open de **Installatiecentrum van SQL Server**. Klik op **installatie**.

1. Klik op **knooppunt toevoegen aan een SQL Server-failovercluster**. Volg de instructies in de wizard installatie van SQL server en deze server toevoegen aan de FCI.

   >[!NOTE]
   >Als u een installatiekopie van de galerie van Azure Marketplace met SQL Server gebruikt, zijn SQL Server-hulpprogramma's opgenomen in de afbeelding. Als u deze installatiekopie niet gebruikt, afzonderlijk de SQL Server-hulpprogramma's installeren. Zie [downloaden van SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-azure-load-balancer"></a>Stap 5: Azure load balancer maken

Op virtuele machines in Azure, clusters een load balancer gebruiken voor het opslaan van een IP-adres dat moet kunnen worden op één clusterknooppunt tegelijk. In deze oplossing bevat de load balancer het IP-adres voor de SQL Server-FCI.

[Maken en configureren van een Azure load balancer](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>De load balancer maken in de Azure portal

De load balancer maken:

1. Ga naar de resourcegroep met de virtuele machines in de Azure portal.

1. Klik op **+ toevoegen**. Zoek de Marketplace voor **Load Balancer**. Klik op **Load Balancer**.

1. Klik op **Create**.

1. Configureer de load balancer met:

   - **Naam**: een naam waarmee de load balancer.
   - **Type**: de load balancer openbare of persoonlijke kan zijn. Een persoonlijke load balancer toegankelijk zijn vanuit binnen hetzelfde VNET. De meeste Azure toepassingen kunnen gebruikmaken van een persoonlijke load balancer. Als uw toepassing toegang tot SQL Server rechtstreeks via Internet moet, gebruikt u een openbare load balancer.
   - **Virtueel netwerk**: hetzelfde netwerk bevindt als de virtuele machines.
   - **Subnet**: hetzelfde subnet als de virtuele machines.
   - **Privé IP-adres**: hetzelfde IP-adres dat u met de netwerkbron FCI van SQL Server-cluster hebt toegewezen.
   - **abonnement**: uw Azure-abonnement.
   - **Resourcegroep**: gebruik dezelfde resourcegroep als uw virtuele machines.
   - **Locatie**: dezelfde Azure-locatie gebruiken als uw virtuele machines.
   Zie de volgende afbeelding:

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-the-load-balancer-backend-pool"></a>De back-endpool van de load balancer configureren

1. Terug naar de Azure-resourcegroep met de virtuele machines en zoek de nieuwe load balancer. U moet Vernieuw de weergave voor de resourcegroep. Klik op de load balancer.

1. Klik op de load balancer-blade **back-endpools**.

1. Klik op **+ toevoegen** toevoegen van een back-endpool.

1. Typ een naam voor de back-endpool.

1. Klik op **toevoegen van een virtuele machine**.

1. Op de **virtuele machines kiezen** blade, klikt u op **een beschikbaarheidsset kiezen**.

1. Kies dat de beschikbaarheid van de set dat u de SQL Server-virtuele machines in geplaatst.

1. Op de **virtuele machines kiezen** blade, klikt u op **de virtuele machines kiezen**.

   Uw Azure-portal moet eruitzien als in de volgende afbeelding:

   ![CreateLoadBalancerBackEnd](./media/virtual-machines-windows-portal-sql-create-failover-cluster/33-load-balancer-back-end.png)

1. Klik op **Selecteer** op de **virtuele machines kiezen** blade.

1. Klik op **OK** twee keer.

### <a name="configure-a-load-balancer-health-probe"></a>Een load balancer-test health configureren

1. Klik op de load balancer-blade **statuscontroles**.

1. Klik op **+ toevoegen**.

1. Op de **toevoegen health test** blade <a name="probe"> </a>het health test parameters instellen:

   - **Naam**: een naam voor de health-test.
   - **Protocol**: TCP.
   - **Poort**: ingesteld op een beschikbare TCP-poort. Deze poort moet een firewallpoort worden geopend. Gebruik de [dezelfde poort](#ports) u ingesteld voor de health-test op de firewall.
   - **Interval**: 5 seconden.
   - **Drempelwaarde voor onjuiste status**: 2 achtereenvolgende mislukte pogingen.

1. Klik op OK.

### <a name="set-load-balancing-rules"></a>Taakverdelingsregels instellen

1. Klik op de load balancer-blade **Taakverdelingsregels**.

1. Klik op **+ toevoegen**.

1. De parameters van de regels voor taakverdeling instellen:

   - **Naam**: een naam voor de load-balancingregels.
   - **Frontend IP-adres**: het IP-adres gebruiken voor de netwerkbron FCI van SQL Server-cluster.
   - **Poort**: ingesteld voor de SQL Server FCI TCP-poort. De exemplaar-standaardpoort is 1433.
   - **Back-endpoort**: deze waarde maakt gebruik van dezelfde poort als de **poort** waarde wanneer u inschakelt **zwevend IP (direct server return)**.
   - **Back-endpool**: Gebruik de naam van de back-end-toepassingen die u eerder hebt geconfigureerd.
   - **De statuscontrole van**: Gebruik de health-test op die u eerder hebt geconfigureerd.
   - **Sessiepersistentie**: geen.
   - **Time-out (minuten)**: 4.
   - **Zwevend IP (direct server return)**: ingeschakeld

1. Klik op **OK**.

## <a name="step-6-configure-cluster-for-probe"></a>Stap 6: Configureren voor de test-cluster

Stel de parameter cluster test poort in PowerShell.

De parameter cluster test poort stelt bijwerken van variabelen in het volgende script met waarden van uw omgeving. Verwijder de punthaken `<>` uit het script. 

   ```PowerShell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

Stel de waarden voor uw omgeving in het voorgaande script. De volgende lijst beschrijft de waarden:

   - `<Cluster Network Name>`: Windows Server Failover Cluster-naam voor het netwerk. In **Failoverclusterbeheer** > **netwerken**, met de rechtermuisknop op het netwerk en klik op **eigenschappen**. De juiste waarde ligt onder de **naam** op de **algemene** tabblad. 

   - `<SQL Server FCI IP Address Resource Name>`: Bronnaam SQL Server FCI-IP-adres. In **Failoverclusterbeheer** > **rollen**, onder de functie SQL Server FCI onder **servernaam**, klik met de rechtermuisknop op de bron van het IP-adres en klik op **Eigenschappen**. De juiste waarde ligt onder de **naam** op de **algemene** tabblad. 

   - `<ILBIP>`: De ILB IP-adres. Dit adres is geconfigureerd in de Azure portal als het adres voor front-end ILB. Dit is ook het SQL Server FCI IP-adres. U vindt deze in **Failoverclusterbeheer** op dezelfde Eigenschappenpagina waar u zich de `<SQL Server FCI IP Address Resource Name>`.  

   - `<nnnnn>`: De testpoort die u hebt geconfigureerd in de load balancer-test health is. Een niet-gebruikte TCP-poort is ongeldig. 

>[!IMPORTANT]
>Het subnetmasker voor de cluster-parameter moet de broadcast TCP IP-adres: `255.255.255.255`.

Nadat u de cluster-test ziet u alle van de clusterparameters in PowerShell instellen. Voer het volgende script uit:

   ```PowerShell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter 
  ```

## <a name="step-7-test-fci-failover"></a>Stap 7: Testen FCI failover

Testfailover van de FCI clusterfunctionaliteit valideren. Voer de volgende stappen uit:

1. Verbinding maken met een van de SQL Server FCI clusterknooppunten met RDP.

1. Open **Failoverclusterbeheer**. Klik op **rollen**. De aankondiging welk knooppunt eigenaar van de FCI van SQL Server-rol.

1. Met de rechtermuisknop op de SQL Server FCI-rol.

1. Klik op **verplaatsen** en klik op **Best mogelijke knooppunt**.

**Failover Cluster Manager** ziet u de functie en de daarbij behorende bronnen offline gaan. De resources Verplaats en komt niet online op het andere knooppunt.

### <a name="test-connectivity"></a>Connectiviteit testen

Als verbindingen wilt testen, moet u zich aanmelden bij een andere virtuele machine in hetzelfde virtuele netwerk. Open **SQL Server Management Studio** en maak verbinding met de naam van de SQL Server FCI.

>[!NOTE]
>Zo nodig u kunt [SQL Server Management Studio downloaden](http://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Beperkingen
Op virtuele machines in Azure, wordt Microsoft Distributed Transaction Coordinator (DTC) niet ondersteund op Failoverclusterinstanties omdat de RPC-poort wordt niet ondersteund door de load balancer.

## <a name="see-also"></a>Zie ook

[Setup S2D met extern bureaublad (Azure)](http://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

[Hyper-geconvergeerde oplossing met opslagruimten direct](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).

[Directe opslag ruimte-overzicht](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[SQL Server-ondersteuning voor S2D](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
