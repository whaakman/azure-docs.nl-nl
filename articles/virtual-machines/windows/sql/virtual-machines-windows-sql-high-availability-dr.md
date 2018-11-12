---
title: Hoge beschikbaarheid en herstel na noodgevallen voor SQL Server | Microsoft Docs
description: Een bespreking van de verschillende typen HADR strategieën voor het SQL Server wordt uitgevoerd in Azure Virtual Machines.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: mikeray
ms.openlocfilehash: 463ef5f4a655617074915078fb4ced9e596f8957
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51257711"
---
# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Maximale beschikbaarheid en herstel na noodgeval voor SQL Server in SQL Server Virtual Machine

Microsoft Azure-machines (VM's) met SQL Server kan helpen de kosten van een hoge beschikbaarheid en noodherstel recovery (HADR) databaseoplossing verlagen. De meeste SQL Server HADR oplossingen worden ondersteund in Azure virtuele machines, zowel als alleen-Azure en hybride oplossingen. In een Azure-only-oplossing, wordt het hele HADR systeem uitgevoerd in Azure. In een hybride configuratie, is onderdeel van de oplossing wordt uitgevoerd in Azure en het andere onderdeel wordt uitgevoerd on-premises in uw organisatie. De flexibiliteit van de Azure-omgeving kunt u geheel of gedeeltelijk naar Azure te verplaatsen om te voldoen aan het budget en HADR vereisten van uw SQL Server-database-systemen.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="understanding-the-need-for-an-hadr-solution"></a>Informatie over de noodzaak voor een oplossing voor HADR
Het is aan u om ervoor te zorgen dat uw databasesysteem beschikt over de HADR-mogelijkheden die de service level agreement (SLA) is vereist. Het feit dat Azure biedt mechanismen voor hoge beschikbaarheid, zoals service healing voor cloudservices en herstel van foutdetectie voor virtuele Machines, garandeert niet zelf u kunt voldoen aan de gewenste SLA. Deze mechanismen beveiligen de hoge beschikbaarheid van de virtuele machines, maar niet de hoge beschikbaarheid van SQL Server die worden uitgevoerd binnen de virtuele machines. Het is mogelijk dat de SQL Server-exemplaar mislukken, terwijl de virtuele machine online en in orde is is. Bovendien, zelfs de hoge beschikbaarheid door Azure geleverde rapportagemethoden toestaan voor downtime van de virtuele machines vanwege gebeurtenissen zoals het herstellen van de software of problemen met de hardware en upgrades van besturingssystemen.

Geografisch redundante opslag (GRS) in Azure, die wordt geïmplementeerd met een functie met de naam van geo-replicatie, mag bovendien niet een oplossing voor noodherstel voldoende voor uw databases. Omdat geo-replicatie worden gegevens asynchroon verzonden, kunnen recente updates in het geval van noodherstel kunnen verloren. Meer informatie over de beperkingen van geo-replicatie worden behandeld in de [Geo-replicatie wordt niet ondersteund voor gegevens en logboekbestanden op afzonderlijke schijven](#geo-replication-support) sectie.

## <a name="hadr-deployment-architectures"></a>HADR implementatie-architecturen
SQL Server HADR-technologieën die worden ondersteund in Azure zijn onder andere:

* [Always On Availability Groups](https://technet.microsoft.com/library/hh510230.aspx)
* [Always On Failover Cluster Instances](https://technet.microsoft.com/library/ms189134.aspx)
* [Back-upfunctie](https://technet.microsoft.com/library/ms187103.aspx)
* [SQL Server-back-up en herstel met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx)
* [Database Mirroring](https://technet.microsoft.com/library/ms189852.aspx) : afgeschaft in SQL Server 2016

Het is mogelijk het combineren van de technologieën bij elkaar voor het implementeren van een SQL Server-oplossing met hoge beschikbaarheid- en herstelfunctionaliteit bij noodgevallen. Een hybride implementatie mogelijk, afhankelijk van de technologie die u gebruikt, een VPN-tunnel met het Azure-netwerk. De onderstaande secties ziet u enkele van de voorbeeld-implementatie-architecturen.

## <a name="azure-only-high-availability-solutions"></a>Azure alleen-lezen: oplossingen voor hoge beschikbaarheid

U kunt een oplossing voor hoge beschikbaarheid voor SQL Server op het databaseniveau van een met Always On Availability Groups - beschikbaarheidsgroepen met de naam hebben. U kunt ook maken een oplossing voor hoge beschikbaarheid op het exemplaarniveau van een met Always On Failover Cluster Instances - failover-clusterexemplaren. Voor aanvullende redundantie, kunt u redundantie op beide niveaus maken door te maken van beschikbaarheidsgroepen voor failover-clusterexemplaren. 

| Technologie | Voorbeeldarchitecturen |
| --- | --- |
| **Beschikbaarheidsgroepen** |Beschikbare replica's die worden uitgevoerd in virtuele Azure-machines in dezelfde regio bieden hoge beschikbaarheid. U moet configureren van een domeincontroller-VM, omdat Windows failover clustering is vereist voor Active Directory-domein.<br/> ![Beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Zie voor meer informatie, [beschikbaarheidsgroepen configureren in Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Failover-clusterexemplaren** |Failover Cluster Instances (FCI), waarvoor u gedeelde opslag, kunnen worden gemaakt op 3 verschillende manieren.<br/><br/>1. Een failovercluster met twee knooppunten met behulp van gekoppelde opslag in Azure VM's met [Windows Server 2016 Storage Spaces Direct \(S2D\) ](virtual-machines-windows-portal-sql-create-failover-cluster.md) voor een op software gebaseerde virtuele SAN.<br/><br/>2. Een twee knooppunten failover-cluster in Azure VM's met opslag ondersteund door een clustering oplossing van derden. Zie voor een specifiek voorbeeld met SIOS DataKeeper [hoge beschikbaarheid voor een bestandsshare met behulp van failover clustering en 3e partij software SIOS DataKeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>3. Een failovercluster met twee knooppunten externe iSCSI-doel in virtuele Azure-machines met gedeelde blokopslag via ExpressRoute. Bijvoorbeeld, NetApp Private Storage (NPS) wordt aangegeven dat een iSCSI-doel via ExpressRoute met Equinix virtuele Azure-machines.<br/><br/>Voor de gedeelde opslag van derden en oplossingen voor replicatie van gegevens, neemt u contact op met de leverancier voor eventuele problemen met betrekking tot het openen van gegevens bij failover.<br/><br/>Houd er rekening mee dat als u met FCI boven [Azure File storage](https://azure.microsoft.com/services/storage/files/) wordt nog niet ondersteund, omdat deze oplossing niet van Premium-opslag gebruikmaken biedt. We werken ter ondersteuning hiervan binnenkort. |

## <a name="azure-only-disaster-recovery-solutions"></a>Azure alleen-lezen: oplossingen voor herstel na noodgevallen
U kunt een oplossing voor noodherstel voor SQL Server-databases in Azure met behulp van de van beschikbaarheidsgroepen, databasespiegeling of back-up en herstellen met de storage-blobs.

| Technologie | Voorbeeldarchitecturen |
| --- | --- |
| **Beschikbaarheidsgroepen** |De beschikbaarheidsreplica is uitgevoerd in meerdere datacenters in Azure VM's voor herstel na noodgevallen. Deze oplossing regio-overschrijdende beveiligt tegen uitval van het volledige site. <br/> ![Beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Binnen een regio moet alle replica's binnen dezelfde cloudservice en hetzelfde VNet. Omdat elke regio een apart VNet, vereisen deze oplossingen een VNet naar VNet-connectiviteit. Zie voor meer informatie, [configureren van een VNet-naar-VNet-verbinding met de Azure portal](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Zie voor gedetailleerde instructies [configureren van een beschikbaarheidsgroep van SQL Server op Azure Virtual Machines in verschillende regio's](virtual-machines-windows-portal-sql-availability-group-dr.md).|
| **Databasespiegeling** |Principal en gespiegelde en servers die worden uitgevoerd in verschillende datacenters voor herstel na noodgevallen. U moet implementeren met behulp van servercertificaten omdat het active directory-domein kan niet meerdere datacenters omvatten.<br/>![Databasespiegeling](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Back-up en herstel met Azure Blob Storage-Service** |Productiedatabases back-ups rechtstreeks naar de blob-opslag in een ander datacenter voor herstel na noodgevallen.<br/>![Back-up en herstel](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Zie voor meer informatie, [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |
| **Repliceren en failover-SQL-Server naar Azure met Azure Site Recovery** |Productie SQL Server van een Azure-datacenter gerepliceerd rechtstreeks naar Azure Storage van andere Azure-datacenter voor herstel na noodgevallen.<br/>![Repliceren met Azure Site Recovery](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_standalone_sqlserver-asr.png)<br/>Zie voor meer informatie, [SQL Server beveiligen met behulp van SQL Server-noodherstel en Azure Site Recovery](../../../site-recovery/site-recovery-sql.md). |


## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybride IT: Noodhersteloplossingen
U kunt een oplossing voor noodherstel voor SQL Server-databases in een hybride IT-omgeving met beschikbaarheidsgroepen, databasespiegeling, back-upfunctie en back-up en herstellen met Azure BLOB-opslag.

| Technologie | Voorbeeldarchitecturen |
| --- | --- |
| **Beschikbaarheidsgroepen** |Sommige beschikbaarheidsreplica's die worden uitgevoerd in Azure VM's en andere replica's met on-premises voor herstel na noodgevallen voor cross-site. De productiesite mag on-premises of in een Azure-datacenter.<br/>![Beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Omdat alle beschikbare replica's moeten zich in hetzelfde failovercluster, moet het cluster beide netwerken (een meerdere subnetten failover-cluster) omvatten. Deze configuratie moet een VPN-verbinding tussen Azure en de on-premises netwerk.<br/><br/>Voor succesvolle noodherstel van uw databases, moet u ook een replicadomeincontroller op de site voor noodherstel installeren.<br/><br/>Het is mogelijk een Azure-replica toevoegen aan een bestaande AlwaysOn-beschikbaarheidsgroep met de Wizard Replica toevoegen in SSMS. Zie voor meer informatie de zelfstudie: uw AlwaysOn-beschikbaarheidsgroep uitbreiden naar Azure. |
| **Databasespiegeling** |Een partner die worden uitgevoerd in een Azure-VM en de andere actieve on-premises voor cross-site-noodherstel met behulp van certificaten. Partners hoeft niet in hetzelfde Active Directory-domein en er is geen VPN-verbinding is vereist.<br/>![Databasespiegeling](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Een andere database mirroring scenario omvat een partner die worden uitgevoerd in een Azure-VM en de andere actieve on-premises in hetzelfde Active Directory-domein voor herstel na noodgevallen voor cross-site. Een [VPN-verbinding tussen de Azure-netwerk en de on-premises netwerk](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) is vereist.<br/><br/>Voor succesvolle noodherstel van uw databases, moet u ook een replicadomeincontroller op de site voor noodherstel installeren. |
| **Back-upfunctie** |Een server die wordt uitgevoerd in een Azure-VM en de andere actieve on-premises voor herstel na noodgevallen voor cross-site. Back-upfunctie hangt af van het Windows delen van bestanden, zodat een VPN-verbinding tussen de Azure-netwerk en de on-premises-netwerk vereist is.<br/>![Back-upfunctie](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Voor succesvolle noodherstel van uw databases, moet u ook een replicadomeincontroller op de site voor noodherstel installeren. |
| **Back-up en herstel met Azure Blob Storage-Service** |On-premises productiedatabases back-ups rechtstreeks op de Azure blob-opslag voor herstel na noodgevallen.<br/>![Back-up en herstel](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Zie voor meer informatie, [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |
| **Repliceren en failover-SQL-Server naar Azure met Azure Site Recovery** |On-premises productie SQL-Server gerepliceerd rechtstreeks naar Azure Storage voor herstel na noodgevallen.<br/>![Repliceren met Azure Site Recovery](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_standalone_sqlserver-asr.png)<br/>Zie voor meer informatie, [SQL Server beveiligen met behulp van SQL Server-noodherstel en Azure Site Recovery](../../../site-recovery/site-recovery-sql.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Belangrijke overwegingen voor SQL Server HADR in Azure
Azure-machines, opslag en netwerken hebben verschillende operationele kenmerken dan een on-premises, niet-gevirtualiseerde IT-infrastructuur. Een succesvolle implementatie van een oplossing HADR SQL Server in Azure vereist dat u inzicht in deze verschillen en ontwerpen van uw oplossing deze kunnen verwerken.

### <a name="high-availability-nodes-in-an-availability-set"></a>Hoge beschikbaarheid-knooppunten in een beschikbaarheidsset
Beschikbaarheidssets in Azure kunnen u de knooppunten voor hoge beschikbaarheid in afzonderlijke Foutdomeinen (FD's) en Update-domeinen (ud's) te plaatsen. Voor Azure VM's in dezelfde beschikbaarheidsset worden geplaatst, moet u deze implementeren in dezelfde cloudservice. Alleen de knooppunten in dezelfde cloudservice kunnen deelnemen in dezelfde beschikbaarheidsset. Zie voor meer informatie [De beschikbaarheid van virtuele machines beheren](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="failover-cluster-behavior-in-azure-networking"></a>Gedrag bij een failover-cluster in Azure-netwerken
De niet-RFC-compatibele DHCP-service in Azure kan leiden tot het maken van bepaalde failover clusterconfiguraties mislukken vanwege de clusternetwerknaam een dubbel IP-adres, zoals het hetzelfde IP-adres als een van de clusterknooppunten worden toegewezen. Dit is een probleem wanneer u beschikbaarheidsgroepen, afhankelijk van de functie Windows failover cluster implementeren.

Houd rekening met het scenario als een cluster met twee knooppunten wordt gemaakt en online gezet:

1. Het cluster online komt, en vervolgens knooppunt1 vraagt een dynamisch toegewezen IP-adres voor de naam van het cluster-netwerk.
2. Er is geen IP-adres behalve knooppunt1 eigen IP-adres wordt bepaald door de DHCP-service, omdat de DHCP-service herkent dat de aanvraag afkomstig van knooppunt1 is zelf.
3. Windows detecteert dat een dubbel adres is toegewezen knooppunt1 en de netwerknaam van de failover-cluster en de standaardgroep voor het cluster niet online is gekomen.
4. De standaardgroep voor het cluster worden verplaatst naar knooppunt2, behandelt knooppunt1 van IP-adres als de cluster-IP-adres en de standaard-clustergroep online brengt.
5. Wanneer Knooppunt2 probeert te maken van verbinding met knooppunt1, laat pakketten die gericht zijn op knooppunt1 nooit Knooppunt2 omdat deze IP-adres van de knooppunt1 wordt omgezet naar zichzelf. Knooppunt2 kan geen verbinding met knooppunt1, vervolgens quorum verliest en het cluster wordt afgesloten.
6. In de tussentijd knooppunt1 pakketten kunnen worden verzonden naar knooppunt2, maar Knooppunt2 niet beantwoorden. Knooppunt1 quorum verliest en het cluster wordt afgesloten.

In dit scenario kan worden vermeden door een niet-gebruikte statische IP-adres, zoals een link-local-IP-adres, zoals 169.254.1.1, toewijzen aan de clusternetwerknaam te brengen van de clusternetwerknaam online. Ter vereenvoudiging van dit proces, Zie [Windows configureren van failover-cluster in Azure voor beschikbaarheidsgroepen](https://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Zie voor meer informatie, [-beschikbaarheidsgroepen configureren in Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>-Listener-ondersteuning voor groepen van beschikbaarheid
Beschikbaarheid van groep listeners worden ondersteund op virtuele Azure-machines waarop Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016 wordt uitgevoerd. Deze ondersteuning wordt mogelijk gemaakt door het gebruik van taakverdeling eindpunten die zijn ingeschakeld op de Azure-VM's die zijn van de beschikbaarheid van groepsknooppunten. U moet stappen speciale configuratie voor de listeners werkt voor zowel clienttoepassingen die worden uitgevoerd in Azure, evenals die on-premises uitgevoerd.

Er zijn twee manieren voor het instellen van de listener: externe (openbaar) of interne. De externe (openbare)-listener maakt gebruik van een internetgerichte load balancer en is gekoppeld met een openbare VIP (Virtual IP) die toegankelijk is via internet. Een interne listener maakt gebruik van een interne load balancer en biedt alleen ondersteuning voor clients binnen hetzelfde Virtueelnetwerk. Voor een type load balancer, moet u Direct Server Return inschakelen. 

Als de beschikbaarheidsgroep omvat meerdere Azure-subnetten (zoals een implementatie die meerdere Azure-regio's), de verbindingsreeks van de client moet bevatten "**MultisubnetFailover = True**'. Dit resulteert in parallelle verbindingspogingen met de replica's in de verschillende subnetten. Zie voor instructies over het instellen van een listener

* [Een ILB-listener voor beschikbaarheidsgroepen configureren in Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
* [Een externe listener configureren voor beschikbaarheidsgroepen in Azure](../sqlclassic/virtual-machines-windows-classic-ps-sql-ext-listener.md).

U kunt nog steeds verbinding maken met elke beschikbaarheidsreplica afzonderlijk door directe verbinding met het service-exemplaar. Ook, aangezien beschikbaarheidsgroepen achterwaarts compatibel met clients voor databasespiegeling, u kunt verbinding maken met de beschikbaarheid van replica's, zoals partners voor databasespiegeling, zolang de replica's die vergelijkbaar is met het spiegelen van de database zijn geconfigureerd:

* Een van de primaire replica en een secundaire replica
* De secundaire replica is geconfigureerd als niet-leesbare (**leesbare secundaire** optie ingesteld op **Nee**)

Een voorbeeld van de client-verbindingsreeks die overeenkomt met deze database mirroring-achtige configuratie met behulp van ADO.NET of SQL Server Native Client lager is dan:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Zie voor meer informatie over client-connectiviteit:

* [Met behulp van Connection String trefwoorden met SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
* [Clients verbinden met een Databasespiegelingssessie (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
* [Verbinding maken met beschikbaarheidsgroep-Listener in hybride IT](https://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
* [Beschikbaarheidsgroep Listeners, clientconnectiviteit en Failover van de toepassing (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
* [Met behulp van verbindingsreeksen Database Mirroring met beschikbaarheidsgroepen](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>De netwerklatentie in hybride IT
U moet uw oplossing HADR met de veronderstelling dat er perioden met hoge netwerklatentie tussen uw on-premises netwerk en Azure implementeren. Bij het implementeren van replica's naar Azure, moet u asynchrone doorvoer gebruiken in plaats van synchrone doorvoer voor de Synchronisatiemodus. Wanneer database mirroring servers implementeren zowel on-premises en in Azure, gebruikt de modus voor hoge prestaties in plaats van de hoge veiligheidsmodus.

### <a name="geo-replication-support"></a>Ondersteuning voor geo-replicatie
Geo-replicatie in Azure-schijven biedt geen ondersteuning voor het gegevensbestand en logboekbestand van dezelfde database worden opgeslagen op verschillende schijven. GRS repliceert wijzigingen op elke schijf afzonderlijk en asynchroon uitgevoerd. Dit mechanisme garandeert de volgorde schrijven binnen één schijf in de kopie geo-replicatie, maar ook niet voor geo-gerepliceerde kopieën van meerdere schijven. Als u een database voor het opslaan van het gegevensbestand en een logboekbestand op afzonderlijke schijven configureert, kunnen de herstelde schijven na een noodgeval een meer recente kopie van het gegevensbestand dan het logboekbestand; waarmee de write-ahead van logboek in SQL Server en de ACID-eigenschappen van transa bevatten ctions. Als u de optie voor het uitschakelen van geo-replicatie in de storage-account hebt, moet u alle gegevens en logboekbestanden bestanden voor een bepaalde database op dezelfde schijf behouden. Als u meer dan één schijf vanwege de grootte van de database gebruiken moet, moet u een van de noodhersteloplossingen bovenstaande om te controleren of de gegevensredundantie implementeren.

## <a name="next-steps"></a>Volgende stappen
Als u maken van een virtuele Azure-machine waarop SQL Server wilt, raadpleegt u [inrichten van een SQL Server-Machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).

Zie de informatie in voor de beste prestaties van SQL Server wordt uitgevoerd op een Azure VM, [aanbevolen procedures voor prestaties voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Zie voor andere onderwerpen met betrekking tot SQL Server wordt uitgevoerd in Azure VM's, [SQL Server op Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Meer informatie
* [Een nieuwe Active Directory-forest installeren in Azure](../../../active-directory/active-directory-new-forest-virtual-machine.md)
* [Failover-Cluster voor beschikbaarheidsgroepen in virtuele Azure-machine maken](https://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)

