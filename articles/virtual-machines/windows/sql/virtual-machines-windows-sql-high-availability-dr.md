---
title: Hoge beschikbaarheid en herstel na noodgevallen voor SQL Server | Microsoft Docs
description: "Een bespreking van de verschillende typen HADR strategieën voor het SQL Server wordt uitgevoerd in Azure Virtual Machines."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: mikeray
ms.openlocfilehash: a81b956107ef82f40ad5304808068a7573ca7d27
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Maximale beschikbaarheid en herstel na noodgeval voor SQL Server in SQL Server Virtual Machine

Microsoft Azure virtuele machines (VM's) met SQL Server kunt lagere kosten voor een hoge beschikbaarheid en noodherstel (HADR) database hersteloplossing. De meeste HADR van SQL Server-oplossingen worden ondersteund in Azure virtuele machines, als Azure alleen-lezen en als de hybride-oplossingen. In een Azure-only-oplossing, wordt het hele HADR systeem uitgevoerd in Azure. In een hybride-configuratie is onderdeel van de oplossing wordt uitgevoerd in Azure en de andere onderdeel voert on-premises in uw organisatie. De flexibiliteit van de Azure-omgeving kunt u geheel of gedeeltelijk naar Azure verplaatst om te voldoen aan het budget en HADR vereisten van uw SQL Server database-systemen.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="understanding-the-need-for-an-hadr-solution"></a>Wat is de noodzaak van een HADR-oplossing?
Het is aan u om ervoor te zorgen dat uw databasesysteem beschikt over de HADR mogelijkheden die de service level agreement (SLA) vereist. Het feit dat Azure biedt mechanismen voor hoge beschikbaarheid, zoals service herstel voor cloudservices en herstel foutdetectie voor virtuele Machines, garandeert niet zelf dat kunt u de gewenste SLA voldoen. Deze mechanismen beveiligen hoge beschikbaarheid van de virtuele machines, maar niet de maximale beschikbaarheid van SQL Server wordt uitgevoerd in de virtuele machines. Het is mogelijk voor de SQL Server-exemplaar terwijl de virtuele machine online is en goed mislukken. Bovendien moeten zelfs de hoge beschikbaarheid mechanismen die zijn verstrekt door Azure toestaan uitvaltijd van de virtuele machines als gevolg van gebeurtenissen, zoals het herstellen van de software- of hardwarestoringen en updates van besturingssystemen.

Bovendien geografisch redundante opslag (GRS) in Azure, dit wordt geïmplementeerd met een functie geo-replicatie, mogelijk niet een voldoende noodherstel voor uw databases. Omdat geo-replicatie worden gegevens asynchroon verzendt, recente updates worden in het geval van noodherstel verloren. Meer informatie over de beperkingen van geo-replicatie worden behandeld in de [Geo-replicatie wordt niet ondersteund voor gegevens en logboekbestanden op afzonderlijke schijven](#geo-replication-support) sectie.

## <a name="hadr-deployment-architectures"></a>HADR implementatie architecturen
SQL Server HADR technologieën die worden ondersteund in Azure omvatten:

* [Altijd op beschikbaarheidsgroepen](https://technet.microsoft.com/library/hh510230.aspx)
* [Altijd op de failovercluster-exemplaren](https://technet.microsoft.com/library/ms189134.aspx)
* [Back-upfunctie voor logboekbestanden](https://technet.microsoft.com/library/ms187103.aspx)
* [SQL Server-back-up en herstel met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx)
* [Databasespiegeling](https://technet.microsoft.com/library/ms189852.aspx) - in SQL Server 2016 afgeschaft

Het is mogelijk de technologieën voor het implementeren van een SQL Server-oplossing met hoge beschikbaarheid en -mogelijkheden voor herstel na noodgevallen samen combineren. Afhankelijk van de technologie die u gebruikt, moet een hybride implementatie mogelijk een VPN-tunnel met het Azure-netwerk. De volgende secties ziet u enkele van de voorbeeld-implementatie-architecturen.

## <a name="azure-only-high-availability-solutions"></a>Azure alleen-lezen: oplossingen voor hoge beschikbaarheid

U kunt een oplossing voor hoge beschikbaarheid hebben voor SQL Server op het databaseniveau van een met AlwaysOn-beschikbaarheidsgroepen - beschikbaarheidsgroepen aangeroepen. U kunt ook maken een oplossing voor hoge beschikbaarheid op het niveau van een exemplaar met altijd op Failover Cluster Instances - failover cluster-exemplaren. Voor aanvullende redundantie, kunt u redundantie op beide niveaus maken door het maken van beschikbaarheidsgroepen op failover cluster-exemplaren. 

| Technologie | Voorbeeld architecturen |
| --- | --- |
| **Beschikbaarheidsgroepen** |Beschikbaarheidsreplica's uitgevoerd in Azure Virtual machines in dezelfde regio bieden hoge beschikbaarheid. U moet configureren van een domeincontroller VM, omdat Windows failover clustering een Active Directory-domein vereist.<br/> ![Beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Zie voor meer informatie [beschikbaarheidsgroepen configureren in Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Failover cluster-exemplaren** |Failover Cluster exemplaren (FCI), die gedeelde opslag is vereist, kunnen op verschillende manieren 3 worden gemaakt.<br/><br/>1. Een failovercluster met twee knooppunten met behulp van gekoppelde opslag in Azure VM's met [Windows Server 2016 opslagruimten Direct \(S2D\) ](virtual-machines-windows-portal-sql-create-failover-cluster.md) voor een op software gebaseerde virtueel SAN.<br/><br/>2. Twee knooppunten failovercluster uitgevoerd in Azure VM's met opslag ondersteund door een clustering oplossing van derden. Zie voor een specifieke voorbeeld die gebruikmaakt van SIOS DataKeeper [hoge beschikbaarheid voor een bestandsshare met behulp van failover clustering en 3e softwareleveranciers SIOS DataKeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>3. Een failovercluster met twee knooppunten uitgevoerd in Azure VM's met externe iSCSI-doel gedeelde blokopslag via ExpressRoute. NetApp persoonlijke opslag (NPS) wordt bijvoorbeeld een iSCSI-doel via ExpressRoute met Equinix virtuele Azure-machines.<br/><br/>Voor de gedeelde opslag van derden en oplossingen voor replicatie van gegevens, moet u contact op met de leverancier van eventuele problemen met toegang tot gegevens op failover.<br/><br/>Houd er rekening mee dat als u met FCI boven [Azure File storage](https://azure.microsoft.com/services/storage/files/) wordt nog niet ondersteund, omdat deze oplossing geen gebruik van Premium-opslag. We werken ter ondersteuning hiervan binnenkort. |

## <a name="azure-only-disaster-recovery-solutions"></a>Azure alleen-lezen: oplossingen voor herstel na noodgevallen
U kunt een noodherstel voor uw SQL Server-databases in Azure met behulp van beschikbaarheidsgroepen, databasespiegeling of back-up en herstellen met de storage-blobs.

| Technologie | Voorbeeld architecturen |
| --- | --- |
| **Beschikbaarheidsgroepen** |De beschikbaarheidsreplica is uitgevoerd in meerdere datacenters in Azure VM's voor herstel na noodgevallen. Deze oplossing regio-overschrijdende beschermd tegen storing van de volledige site. <br/> ![Beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Alle replica's moet een regio in dezelfde cloudservice en hetzelfde VNet. Omdat elke regio een afzonderlijk VNet, vereist deze oplossingen VNet naar VNet-connectiviteit. Zie voor meer informatie [configureren van een VNet-naar-VNet-verbinding met de Azure portal](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Zie voor gedetailleerde instructies [een SQL Server-beschikbaarheidsgroep configureren op Azure Virtual Machines in verschillende regio's](virtual-machines-windows-portal-sql-availability-group-dr.md).|
| **Databasespiegeling** |Principal en gespiegelde en servers die in verschillende datacenters voor herstel na noodgevallen. U moet implementeren met behulp van servercertificaten omdat active directory-domein kan niet meerdere datacenters omvatten.<br/>![Databasespiegeling](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Back-up en herstel met Azure Blob Storage-Service** |Productiedatabases back-ups rechtstreeks naar blob storage in een ander datacenter voor herstel na noodgevallen.<br/>![Back-up en herstel](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Zie voor meer informatie [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |
| **Replicatie en failover-SQL-Server naar Azure met Azure Site Recovery** |Productie SQL Server van een Azure-datacenter gerepliceerd rechtstreeks naar Azure Storage van andere Azure-datacenter voor herstel na noodgevallen.<br/>![Repliceren met Azure Site Recovery](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_standalone_sqlserver-asr.png)<br/>Zie voor meer informatie [SQL Server beveiligen met behulp van SQL Server-noodherstel en Azure Site Recovery](../../../site-recovery/site-recovery-sql.md). |


## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybride IT: Noodhersteloplossingen
Als u beschikt over van een noodherstel voor uw SQL Server-databases in een hybride IT-omgeving met beschikbaarheidsgroepen, databasespiegeling-upfunctie voor logboekbestanden en back-up en herstellen met de opslag van de Azure-blog.

| Technologie | Voorbeeld architecturen |
| --- | --- |
| **Beschikbaarheidsgroepen** |Sommige beschikbaarheidsreplica's uitgevoerd in de Azure VM's en andere replica's met on-premises voor herstel na noodgevallen voor cross-site. De productiesite mag on-premises of in een Azure-datacenter.<br/>![Beschikbaarheidsgroepen](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Omdat alle beschikbaarheidsreplica's in hetzelfde failovercluster zijn moeten, moet het cluster beide netwerken (een meerdere subnetten failover-cluster) omvatten. Deze configuratie is vereist voor een VPN-verbinding tussen Azure en de on-premises netwerk.<br/><br/>Voor geslaagde noodherstel van uw database, moet u ook een replicadomeincontroller op het herstel na noodgevallen site installeren.<br/><br/>Het is mogelijk gebruik van de Wizard Replica toevoegen in SSMS een Azure replica toevoegen aan een bestaande AlwaysOn-beschikbaarheidsgroep. Zie voor meer informatie, zelfstudie: uw AlwaysOn-beschikbaarheidsgroep uitbreiden naar Azure. |
| **Databasespiegeling** |Een partner wordt uitgevoerd in een virtuele machine van Azure en de andere actieve on-premises voor noodherstel van cross-site met behulp van servercertificaten. Partners hoeft niet in hetzelfde Active Directory-domein en er is geen VPN-verbinding vereist.<br/>![Databasespiegeling](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Een andere database mirroring scenario omvat één partner uitgevoerd in een virtuele machine van Azure en de andere actieve on-premises in hetzelfde Active Directory-domein voor herstel na noodgevallen voor cross-site. Een [VPN-verbinding tussen de virtuele Azure-netwerk en de on-premises netwerk](../../../vpn-gateway/vpn-gateway-site-to-site-create.md) is vereist.<br/><br/>Voor geslaagde noodherstel van uw database, moet u ook een replicadomeincontroller op het herstel na noodgevallen site installeren. |
| **Back-upfunctie voor logboekbestanden** |Één server worden uitgevoerd in een virtuele machine van Azure en de andere actieve on-premises voor herstel na noodgevallen voor cross-site. Back-upfunctie voor logboekbestanden hangt af van het Windows-bestanden delen, zodat een VPN-verbinding tussen de virtuele Azure-netwerk en de on-premises netwerk is vereist.<br/>![Back-upfunctie voor logboekbestanden](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Voor geslaagde noodherstel van uw database, moet u ook een replicadomeincontroller op het herstel na noodgevallen site installeren. |
| **Back-up en herstel met Azure Blob Storage-Service** |Een on-premises productiedatabases back-ups rechtstreeks naar Azure blob storage voor herstel na noodgevallen.<br/>![Back-up en herstel](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Zie voor meer informatie [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |
| **Replicatie en failover-SQL-Server naar Azure met Azure Site Recovery** |Lokale SQL Server gerepliceerd rechtstreeks naar Azure Storage voor herstel na noodgevallen voor productie.<br/>![Repliceren met Azure Site Recovery](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_standalone_sqlserver-asr.png)<br/>Zie voor meer informatie [SQL Server beveiligen met behulp van SQL Server-noodherstel en Azure Site Recovery](../../../site-recovery/site-recovery-sql.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Belangrijke aandachtspunten voor SQL Server HADR in Azure
Azure virtuele machines, opslag en netwerken hebben andere operationele kenmerken dan een on-premises, niet-gevirtualiseerde IT-infrastructuur. Een succesvolle implementatie van een oplossing voor HADR SQL Server in Azure, moet u inzicht in deze verschillen en ontwerp van uw oplossing deze kunnen verwerken.

### <a name="high-availability-nodes-in-an-availability-set"></a>Knooppunten voor hoge beschikbaarheid in een beschikbaarheidsset
Beschikbaarheidssets in Azure kunnen u de knooppunten voor hoge beschikbaarheid in afzonderlijke domeinen met fouten (FDs) en Update-domeinen (UDs) plaatsen. Voor Azure virtuele machines in dezelfde beschikbaarheidsset worden geplaatst, moet u ze in dezelfde cloudservice implementeren. Alleen knooppunten in dezelfde cloudservice deel uitmaken van dezelfde beschikbaarheidsset. Zie voor meer informatie [De beschikbaarheid van virtuele machines beheren](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="failover-cluster-behavior-in-azure-networking"></a>Gedrag van de failover-cluster in Azure-netwerken
De niet-RFC-compatibele DHCP-service in Azure kan leiden tot het maken van een bepaalde failover clusterconfiguraties mislukken vanwege de clusternetwerknaam een dubbel IP-adres, zoals de hetzelfde IP-adres als een van de clusterknooppunten worden toegewezen. Dit is een probleem wanneer u beschikbaarheidsgroepen, is afhankelijk van de functie Windows failover cluster implementeert.

Houd rekening met het scenario als een cluster met twee knooppunten wordt gemaakt en online is gekomen:

1. Het cluster online komt en vervolgens een dynamisch toegewezen IP-adres voor de clusternetwerknaam knooppunt1-aanvragen.
2. Er is geen IP-adres behalve knooppunt1 eigen IP-adres wordt bepaald door de DHCP-service, omdat de DHCP-service herkent dat de aanvraag afkomstig van knooppunt1 is zelf.
3. Windows detecteert dat een dubbele adres wordt toegewezen zowel knooppunt1 als de netwerknaam van de failover-cluster en de standaardgroep voor het cluster kan niet online.
4. De standaardgroep voor het cluster wordt verplaatst naar knooppunt2, die wordt beschouwd knooppunt1 van IP-adres als de IP-adres van het cluster en de standaard online clustergroep brengt.
5. Wanneer Knooppunt2 probeert om connectiviteit met knooppunt1, laat pakketten die gericht zijn op knooppunt1 nooit Knooppunt2 omdat hiermee het knooppunt1 van IP-adres aan zichzelf wordt opgelost. Knooppunt2 kan geen verbinding maken met knooppunt1, vervolgens quorum verliest en het cluster wordt afgesloten.
6. In de tussentijd knooppunt1 kunt verzenden van pakketten naar knooppunt2, maar Knooppunt2 kan niet antwoorden. Knooppunt1 quorum verliest en het cluster wordt afgesloten.

Dit scenario kunt u voorkomen door een niet-gebruikte statische IP-adres, zoals een link-local IP-adres zoals 169.254.1.1, toewijzen aan de clusternetwerknaam om de netwerknaam van de cluster online brengen. Als u wilt vereenvoudigen, Zie [Windows configureren van failover-cluster in Azure voor beschikbaarheidsgroepen](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Zie voor meer informatie [beschikbaarheidsgroepen configureren in Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Beschikbaarheid van groep listener-ondersteuning
Beschikbaarheid beschikbaarheidsgroeplisteners worden ondersteund op Azure VM's met Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016. Deze ondersteuning wordt mogelijk gemaakt door het gebruik van netwerktaakverdeling-eindpunten die zijn ingeschakeld op de Azure VM's die beschikbaarheid groepsknooppunten. U moet stappen speciale configuratie voor de listeners werkt voor zowel clienttoepassingen die worden uitgevoerd in Azure, evenals die lokaal wordt uitgevoerd.

Er zijn twee manieren voor het instellen van de listener: externe (openbaar) of interne. De externe (openbare)-listener maakt gebruik van de load balancer van een internetgericht en is gekoppeld met een openbare virtuele IP-adres (VIP) die toegankelijk is via het internet. Een interne listener maakt gebruik van een interne load balancer en biedt alleen ondersteuning voor clients binnen hetzelfde virtuele netwerk. Voor een load balancer type, moet u Direct Server Return inschakelen. 

Als de beschikbaarheidsgroep omvat meerdere Azure subnetten (zoals een implementatie die over Azure-regio's), de verbindingsreeks van de client moet bevatten '**MultisubnetFailover = True**'. Dit resulteert in parallelle verbindingspogingen met de replica's in verschillende subnetten. Zie voor instructies over het instellen van een listener

* [Een ILB-listener voor beschikbaarheidsgroepen configureren in Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
* [Een externe-listener voor beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-ext-listener.md).

U kunt nog steeds verbinding maken met elke beschikbaarheidsreplica afzonderlijk door rechtstreeks verbinding maken met het service-exemplaar. Bovendien aangezien beschikbaarheidsgroepen achterwaarts compatibel met databasespiegeling clients, kunt u verbinden met de beschikbaarheidsreplica's zoals partners voor databasespiegeling, zolang de replica's zijn vergelijkbaar met het spiegelen van databases geconfigureerd:

* Eén primaire replica en één secundaire replica
* De secundaire replica is geconfigureerd als niet-leesbare (**leesbare secundaire** optie ingesteld op **Nee**)

Een voorbeeld van de client verbinding tekenreeks die overeenkomt met deze configuratie van het database mirroring-achtige met ADO.NET of SQL Server Native Client lager is dan:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Zie voor meer informatie over clientverbindingen controleren:

* [Met behulp van Connection String trefwoorden met SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
* [Clients verbinden met een Databasespiegelingssessie (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
* [Verbinding maken met beschikbaarheidsgroep-Listener in hybride IT](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
* [Beschikbaarheidsgroep-Listeners, clientconnectiviteit en Failover van de toepassing (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
* [Met behulp van verbindingsreeksen Database Mirroring met beschikbaarheidsgroepen](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Netwerklatentie in hybride IT
U moet uw oplossing HADR met de veronderstelling dat er perioden met hoge netwerklatentie tussen uw on-premises netwerk en Azure implementeren. Als u replica's implementeert naar Azure, moet u asynchrone doorvoer in plaats van synchrone doorvoer voor de Synchronisatiemodus. Wanneer database mirroring servers implementeren zowel on-premises en in Azure, gebruikt de modus hoge prestaties in plaats van de modus hoge veiligheid.

### <a name="geo-replication-support"></a>Geo-replicatie-ondersteuning
Geo-replicatie in Azure-schijven biedt geen ondersteuning voor het gegevensbestand en logboekbestand van dezelfde database worden opgeslagen op afzonderlijke schijven. GRS repliceert wijzigingen op elke schijf afzonderlijk en asynchroon. Dit mechanisme zorgt ervoor dat de volgorde schrijven binnen één schijf in de kopie geogerepliceerde, maar niet tussen geogerepliceerde kopieën van meerdere schijven. Als u een database voor het opslaan van het gegevensbestand en een logboekbestand op afzonderlijke schijven configureert, bevatten de herstelde schijven na een noodgeval een meer recente kopie van het gegevensbestand dan het logboekbestand dat het logboek schrijven-ahead in SQL Server en de ACID-eigenschappen van transacties wordt verbroken. Als u niet de optie voor het uitschakelen van geo-replicatie in de storage-account hebt, moet u alle gegevens en logboekbestanden bestanden voor een bepaalde database op dezelfde schijf behouden. Als u meer dan één schijf als gevolg van de grootte van de database gebruiken moet, moet u een van de noodhersteloplossingen die hierboven worden genoemd om ervoor te zorgen gegevensredundantie implementeren.

## <a name="next-steps"></a>Volgende stappen
Als u maken van een virtuele machine van Azure met SQL Server wilt, Zie [inrichten van een virtuele Machine van SQL Server op Azure](virtual-machines-windows-portal-sql-server-provision.md).

Als u de beste prestaties van SQL Server wordt uitgevoerd op een virtuele machine in Azure, Zie de informatie in [Best Practices prestaties for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Zie voor andere onderwerpen die betrekking hebben op SQL Server wordt uitgevoerd in Azure VM's [SQL Server op Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Meer informatie
* [Een nieuw Active Directory-forest installeren in Azure](../../../active-directory/active-directory-new-forest-virtual-machine.md)
* [Failover-Cluster maken voor beschikbaarheidsgroepen in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)

