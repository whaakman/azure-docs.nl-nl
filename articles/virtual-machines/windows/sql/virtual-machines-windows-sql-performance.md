---
title: Prestatierichtlijnen voor SQL Server in Azure | Microsoft Docs
description: Bevat richtlijnen voor het optimaliseren van prestaties van SQL Server in Microsoft Azure-VM's.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/24/2018
ms.author: jroth
ms.openlocfilehash: 3a61c20b922b60e3135d9f9e53928462887a602e
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886182"
---
# <a name="performance-guidelines-for-sql-server-in-azure-virtual-machines"></a>Prestatierichtlijnen voor SQL Server in Azure Virtual Machines

## <a name="overview"></a>Overzicht

Dit artikel bevat richtlijnen voor het optimaliseren van prestaties van SQL Server in Microsoft Azure Virtual machines. Tijdens het uitvoeren van SQL Server in Azure Virtual Machines, is het raadzaam dat u doorgaan met behulp van dezelfde databaseprestaties afstemmen van de opties die van toepassing op SQL Server in on-premises server-omgeving. De prestaties van een relationele database in een openbare cloud is echter afhankelijk van veel factoren zoals de grootte van een virtuele machine en de configuratie van de gegevensschijven.

[SQL Server-installatiekopieën die zijn ingericht in Azure portal](quickstart-sql-vm-create-portal.md) opslag configureren met aanbevolen procedures. Zie voor meer informatie over hoe opslag wordt geconfigureerd, [opslagconfiguratie voor SQL Server-VM's](virtual-machines-windows-sql-server-storage-configuration.md). Na het inrichten, toe te passen van andere optimalisaties die in dit artikel worden besproken. Uw keuzes baseren op uw werkbelasting en controleer of door middel van testen.

> [!TIP]
> In dit artikel is gericht op het ophalen van de *aanbevolen* prestaties voor SQL Server op Azure Virtual machines. Als uw werkbelasting minder zwaar worden belast is, kan u niet alle hieronder vermelde optimalisatie nodig. Houd rekening met uw prestatiebehoeften en patronen van werkbelasting kijkt u bij het evalueren van deze aanbevelingen.

## <a name="quick-check-list"></a>Lijst met snelle controle

Hier volgt een lijst met snelle controle voor optimale prestaties van SQL Server op Azure Virtual Machines:

| Oppervlakte | Optimalisaties |
| --- | --- |
| [VM-grootte](#vm-size-guidance) |[DS3_v2](../sizes-general.md) of hoger voor SQL Enterprise edition.<br/><br/>[DS2_v2](../sizes-general.md) of hoger voor SQL Standard- en Web-edities. |
| [Storage](#storage-guidance) |Gebruik [Premium Storage](../premium-storage.md). Standard-opslag wordt alleen aanbevolen voor ontwikkelen en testen.<br/><br/>Houd de [opslagaccount](../../../storage/common/storage-create-storage-account.md) en SQL Server-VM in dezelfde regio.<br/><br/>Azure uitschakelen [geografisch redundante opslag](../../../storage/common/storage-redundancy.md) (geo-replicatie) op het storage-account. |
| [Schijven](#disks-guidance) |Minimaal 2 gebruiken [P30 schijven](../premium-storage.md#scalability-and-performance-targets) (1 voor logboekbestanden en 1 voor gegevensbestanden en TempDB; of stripe twee of meer schijven en alle bestanden in een enkel volume store).<br/><br/>Vermijd het gebruik van besturingssysteem of tijdelijke schijven voor de database-opslag of logboekregistratie.<br/><br/>Schakel de leescache in op de schijven die als host fungeert voor de gegevens en TempDB-gegevensbestanden.<br/><br/>Schakel niet opslaan in cache op een of meer schijven die als host fungeert voor het logboekbestand.<br/><br/>Belangrijk: Stop de SQL Server-service bij het wijzigen van de cache-instellingen voor een Azure VM-schijf.<br/><br/>Stripe meerdere Azure-gegevensschijven om op te halen van hogere i/o-doorvoer.<br/><br/>Opmaken met gedocumenteerde toewijzing grootten. |
| [I/O](#io-guidance) |Database pagina compressie inschakelen.<br/><br/>Bestanden onmiddellijk de initialisatie van de gegevensbestanden inschakelen.<br/><br/>Limiet systeemverwerking op de database.<br/><br/>Schakel Automatische verkleining van de database.<br/><br/>Verplaats alle databases naar gegevensschijven, met inbegrip van systeemdatabases.<br/><br/>SQL Server-fout logboek- en traceringsbestanden bestandsmappen naar gegevensschijven verplaatsen.<br/><br/>Setup standaardbestandslocaties voor back-up en -database.<br/><br/>Vergrendelde pagina's inschakelen.<br/><br/>Oplossingen voor SQL Server-prestaties van toepassing. |
| [Functiespecifieke](#feature-specific-guidance) |Back-up rechtstreeks naar de blob-opslag. |

Voor meer informatie over *hoe* en *waarom* zodat deze optimalisaties Controleer de details en de richtlijnen van de volgende secties.

## <a name="vm-size-guidance"></a>Richtlijnen voor VM-grootte

Voor gevoelige toepassingen met prestaties, wordt aanbevolen dat u het volgende [grootte van virtuele machines](../sizes.md):

* **SQL Server Enterprise Edition**: DS3_v2 of hoger
* **SQL Server Standard en Web-edities**: DS2_v2 of hoger

[DSv2-serie](../sizes-general.md#dsv2-series) VMs ondersteuning voor premium storage, die wordt aanbevolen voor de beste prestaties. De grootte aanbevolen basislijnen hier zijn, maar de grootte van de werkelijke machine die u selecteert, is afhankelijk van uw werklastvraag. DSv2-serie VM's zijn algemeen gebruik VM's die geschikt voor verschillende workloads, zijn terwijl andere machines-grootten zijn geoptimaliseerd voor specifieke werkbelasting typen. Bijvoorbeeld, de [M-serie](../sizes-memory.md#m-series) biedt het hoogste aantal vCPU's en geheugen voor de grootste SQL Server-workloads. De [GS-serie](../sizes-memory.md#gs-series) en [DSv2-serie 11-15](../sizes-memory.md#dsv2-series-11-15) zijn geoptimaliseerd voor grote geheugenvereisten. Beide van deze serie zijn ook beschikbaar in [constrained core grootten](../../windows/constrained-vcpu.md), die voor workloads met lagere eisen bespaart u geld. De [Ls-serie](../sizes-storage.md) machines zijn geoptimaliseerd voor snelle doorvoer van schijfgegevens en IO. Het is belangrijk rekening houden met uw specifieke SQL Server-werkbelasting en dit toepassen op uw selectie van een VM-serie en de grootte.

## <a name="storage-guidance"></a>Richtlijnen voor opslag

DS-serie (samen met DSv2-serie en GS-serie) virtuele machines ondersteuning [Premium Storage](../premium-storage.md). Premium-opslag wordt aanbevolen voor alle werkbelastingen voor productie.

> [!WARNING]
> Standard-opslag heeft verschillende latentie en bandbreedte en wordt alleen aanbevolen voor werkbelastingen voor ontwikkelen/testen. Werkbelastingen voor productie moeten gebruikmaken van Premium Storage.

Bovendien is het raadzaam dat u uw Azure storage-account maken in hetzelfde Datacenter als uw virtuele machines van SQL Server om te beperken van vertragingen van overdracht. Bij het maken van een storage-account, uitschakelen geo-replicatie als consistente schrijven volgorde over meerdere schijven kan niet worden gegarandeerd. Overweeg in plaats daarvan de configuratie van een SQL Server disaster recovery-technologie tussen twee Azure-datacenters. Zie voor meer informatie, [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Richtlijnen voor schijven

Er zijn drie schijftypen van de belangrijkste op een Azure-VM:

* **Besturingssysteemschijf**: wanneer u een virtuele Azure-Machine maakt, ten minste één schijf zal worden gekoppeld aan het platform (met het label de **C** station) naar de virtuele machine voor de schijf van uw besturingssysteem. Deze schijf is een VHD die is opgeslagen als pagina-blob in de opslag.
* **Tijdelijke schijf**: virtuele machines van Azure bevatten een andere schijf met de naam de tijdelijke schijf (met het label de **D**: station). Dit is een schijf op het knooppunt dat kan worden gebruikt voor scratchruimte.
* **Gegevensschijven**: U kunt ook extra schijven koppelen aan uw virtuele machine als gegevensschijven, en deze in de opslag worden opgeslagen als pagina-blobs.

De volgende secties beschrijven de aanbevelingen voor het gebruik van deze verschillende schijven.

### <a name="operating-system-disk"></a>Besturingssysteemschijf

Een besturingssysteemschijf is een VHD die u kunt opstarten en koppelen als een versie van een besturingssysteem als het label **C** station.

Standaardcache-beleid op de besturingssysteemschijf is **lezen/schrijven**. Voor gevoelige toepassingen met prestaties, raden wij aan dat u gegevensschijven in plaats van de besturingssysteemschijf. Zie de sectie op gegevensschijven hieronder.

### <a name="temporary-disk"></a>Tijdelijke schijf

Het station voor tijdelijke opslag, met het label de **D**: station, niet naar Azure blob-opslag worden opgeslagen. Sla niet uw databasebestanden van de gebruiker of de gebruiker transactielogboekbestanden op de **D**: station.

Voor de D-serie, uit de Dv2-serie en G-serie VM's is het tijdelijke station op deze virtuele machines op basis van SSD. Als uw workload intensief gebruikgemaakt van TempDB (zoals tijdelijke objecten of complexe joins wordt) TempDB opslaan op de **D** station kan leiden tot hogere TempDB-doorvoer en lagere latentie voor TempDB.

Voor virtuele machines die ondersteuning bieden voor Premium Storage (DS-serie, DSv2-serie en GS-serie), wordt u aangeraden TempDB opslaan op een schijf die ondersteuning biedt voor Premium-opslag met lees-caching ingeschakeld. Er is een uitzondering op deze aanbeveling; Als uw verbruik TempDB schrijven-intensieve is, u betere prestaties bereiken door TempDB opslaan op de lokale **D** station, die ook SSD-gebaseerde op deze machinegrootten.

### <a name="data-disks"></a>Gegevensschijven

* **Gegevensschijven gebruiken voor gegevens en logboekbestanden**: als u niet striping van de schijf gebruikt, gebruikt u twee Premium Storage [P30 schijven](../premium-storage.md#scalability-and-performance-targets) waar één schijf bevat de logboekbestanden en de andere bevat de gegevens en het TempDB-bestanden. Elke schijf Premium-opslag biedt een aantal IOPs en bandbreedte (MB/s), afhankelijk van de grootte, zoals beschreven in het artikel [met behulp van Premium Storage voor schijven](../premium-storage.md). Als u van een schijf striping techniek, zoals opslagruimten gebruikmaakt, raden wij u plaatst alle gegevens en logboekbestanden op hetzelfde station bevinden.

   > [!NOTE]
   > Wanneer u een SQL Server-VM in de portal inricht, hebt u de mogelijkheid om de opslagconfiguratie bewerken. Afhankelijk van uw configuratie configureert Azure een of meer schijven. Meerdere schijven zijn gecombineerd tot een enkel opslaggroep met gesegmenteerd te verdelen. De gegevens en logboekbestanden bestanden bevinden zich samen in deze configuratie. Zie voor meer informatie, [opslagconfiguratie voor SQL Server-VM's](virtual-machines-windows-sql-server-storage-configuration.md).

* **Schijfsegmentering**: meer doorvoer, kunt u extra gegevensschijven toevoegen en gebruiken van Striping van de schijf. Om te bepalen het aantal gegevensschijven, moet u het aantal IOPS en bandbreedte die vereist is voor uw logboekbestanden en uw gegevens en het TempDB-bestanden te analyseren. U ziet dat andere VM-grootten hebben andere limieten voor het aantal IOPs en bandbreedte die wordt ondersteund, raadpleegt u de tabellen over IOP's per [VM-grootte](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Gebruik de volgende richtlijnen:

  * Voor Windows 8/Windows Server 2012 of hoger, gebruikt u [opslagruimten](https://technet.microsoft.com/library/hh831739.aspx) met de volgende richtlijnen:

      1. Stel de interleave (Streepgrootte) op 64 KB (65536 bytes) voor OLTP-workloads en 256 KB (262144 bytes) voor magazijnbeheer data-workloads om te voorkomen dat invloed op de prestaties vanwege databasetransactielogboeken. Dit moet worden ingesteld met PowerShell.
      1. Het aantal kolommen ingesteld = aantal fysieke schijven. PowerShell gebruiken bij het configureren van meer dan 8 schijven (geen Server Manager UI). 

    De volgende PowerShell wordt bijvoorbeeld een nieuwe opslaggroep gemaakt met de interleave-grootte aan 64 KB en het aantal kolommen in 2:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Voor Windows 2008 R2 of eerder, kunt u dynamische schijven (OS striped volumes) en de Streepgrootte altijd gelijk is aan 64 KB. Houd er rekening mee dat deze optie is afgeschaft vanaf Windows 8/Windows Server 2012. Zie voor meer informatie, de instructie ondersteuning op [Virtual Disk Service is over te stappen op Windows Storage Management API](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * Als u [Storage Spaces Direct (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) met een scenario, zoals [SQL Server Failover Cluster Instances](virtual-machines-windows-portal-sql-create-failover-cluster.md), moet u één groep configureren. Houd er rekening mee dat hoewel verschillende volumes kunnen worden gemaakt op die één groep, alle dezelfde kenmerken, zoals het dezelfde cachebeleid delen.

  * Bepaalt het aantal schijven die zijn gekoppeld aan uw opslaggroep op basis van de load-verwachtingen. Houd er rekening mee dat andere VM-grootten verschillende aantallen gekoppelde gegevensschijven toestaan. Zie voor meer informatie, [grootten voor virtuele Machines](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Als u Premium Storage (scenario's voor ontwikkelen en testen) niet gebruikt, wordt de aanbeveling is het toevoegen van het maximum aantal gegevensschijven dat wordt ondersteund door uw [VM-grootte](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en gebruik schijfsegmentering.

* **Beleid voor caching**: Houd er rekening mee de volgende aanbevelingen voor het beleid, afhankelijk van de opslagconfiguratie voor caching.

  * Als u van afzonderlijke schijven voor gegevens en logboekbestanden gebruikmaakt, schakel de leescache in op de gegevensschijven die als host fungeert voor uw gegevens en TempDB-gegevensbestanden. Dit kan resulteren in een aanzienlijke prestatievoordelen. Schakel niet opslaan in cache op de schijf het logboekbestand houden terwijl dit zorgt ervoor een kleine afname in de prestaties dat.

  * Als u van striping van de schijf gebruikmaakt, profiteren de meeste workloads van lees-caching. Vanwege de prestatieverbetering te bereiken met schijfsegmentering geldt deze aanbeveling, zelfs wanneer het logboekbestand op hetzelfde station bevinden. In bepaalde workloads hoge kan betere prestaties worden behaald met geen caching. Dit kan alleen worden bepaald door middel van testen.

  * De bovenstaande aanbevelingen gelden voor Premium Storage-schijven. Als u Premium Storage niet gebruikt, Schakel geen eventuele opslaan in cache op eventuele gegevensschijven.

  * Zie de volgende artikelen voor instructies over het configureren van de opslaan in schijfcache. Zie voor het klassieke implementatiemodel (ASM): [Set AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) en [Set AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx). Zie voor het Azure Resource Manager-implementatiemodel: [Set AzureRMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk?view=azurermps-4.4.1) en [Set-AzureRMVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdatadisk?view=azurermps-4.4.1).

     > [!WARNING]
     > Stop de SQL Server-service bij het wijzigen van de cache-instelling van Azure VM-schijven om te voorkomen dat de kans op een beschadiging van de database.

* **Grootte van NTFS-toewijzingseenheid**: bij het opmaken van de gegevensschijf, is het aanbevolen dat u een clustergrootte van 64 KB voor gegevens en logboekbestanden, evenals TempDB gebruiken.

* **Best practices voor schijf**: wanneer een gegevensschijf verwijderen of wijzigen van een cachetype, stopt de SQL Server-service tijdens de wijziging. Wanneer de cache-instellingen worden gewijzigd op de besturingssysteemschijf, Azure de virtuele machine stopt, wijzigt u het cachetype, en wordt de VM opnieuw opgestart. Wanneer de cache-instellingen van een gegevensschijf zijn gewijzigd, wordt de virtuele machine is niet gestopt, maar de gegevensschijf is losgekoppeld van de virtuele machine tijdens de wijziging en klik vervolgens opnieuw.

  > [!WARNING]
  > Fout bij stoppen van de SQL Server-service tijdens deze bewerkingen kan leiden tot beschadiging van de database.

## <a name="io-guidance"></a>I/o-richtlijnen

* De beste resultaten met Premium-opslag zijn bereikt wanneer u uw toepassing en aanvragen parallel. Premium Storage is ontworpen voor scenario's waarin de i/o-wachtrijdiepte groter is dan 1, ziet u weinig of geen prestatieverbeteringen voor één thread seriële aanvragen (zelfs als ze zich intensieve opslag). Bijvoorbeeld, kan dit gevolgen voor de testresultaten prestaties analyseprogramma's, zoals SQLIO in één thread.

* Overweeg het gebruik van [database-pagina, compressie](https://msdn.microsoft.com/library/cc280449.aspx) omdat dit kan helpen de prestaties van i/o-intensieve workloads te verbeteren. De compressie van gegevens kan echter de CPU-verbruik op de databaseserver verhogen.

* Overweeg in te schakelen van de initialisatie van de directe bestanden om de tijd die is vereist voor initiële bestandstoewijzing te verkorten. Als u wilt profiteren van de initialisatie van de bestanden onmiddellijk, u de SQL Server (MSSQLSERVER) serviceaccount met SE_MANAGE_VOLUME_NAME verlenen en toe te voegen aan de **onderhoudstaken op Volume uitvoeren** beveiligingsbeleid. Als u van een installatiekopie van SQL Server-platform voor Azure gebruikmaakt, het standaard-serviceaccount (NT Service\MSSQLSERVER) is niet toegevoegd aan de **onderhoudstaken op Volume uitvoeren** beveiligingsbeleid. Met andere woorden, is direct bestanden initialisatie niet ingeschakeld in een installatiekopie van SQL Server Azure-platform. Na het toevoegen van de SQL Server-serviceaccount voor de **onderhoudstaken op Volume uitvoeren** beveiligingsbeleid, de SQL Server-service opnieuw starten. Er zijn mogelijk beveiligingsoverwegingen voor het gebruik van deze functie. Zie voor meer informatie, [Database File Initialization](https://msdn.microsoft.com/library/ms175935.aspx).

* **autogrow** wordt beschouwd als alleen noodplan voor onverwachte groei. Beheert niet de groei van uw gegevens en logboekbestanden op dagelijks met autogrow. Als automatische groei wordt gebruikt, is vooraf uitbreiding van het bestand met behulp van de switch grootte.

* Zorg ervoor dat **Automatische verkleining** is uitgeschakeld om te voorkomen dat onnodige overhead die kan de prestaties negatief beïnvloeden.

* Verplaats alle databases naar gegevensschijven, met inbegrip van systeemdatabases. Zie voor meer informatie, [systeemdatabases verplaatsen](https://msdn.microsoft.com/library/ms345408.aspx).

* SQL Server-fout logboek- en traceringsbestanden bestandsmappen naar gegevensschijven verplaatsen. Dit is mogelijk in SQL Server Configuration Manager door met de rechtermuisknop op uw SQL Server-exemplaar en eigenschappen te selecteren. De instellingen voor foutpagina logboek- en traceringsbestanden bestand kunnen worden gewijzigd in de **opstartparameters** tabblad. De map Dump is opgegeven in de **Geavanceerd** tabblad. De volgende schermafbeelding ziet waar u wilt zoeken voor de parameter fout log opstarten.

    ![Schermafbeelding van de SQL-foutenlogboek](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* Setup standaardbestandslocaties voor back-up en -database. Gebruik de aanbevelingen in dit artikel en de wijzigingen aanbrengen in het eigenschappenvenster van de Server. Zie voor instructies [weergeven of wijzigen van de standaard locaties voor gegevens en logboekbestanden (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). De volgende schermafbeelding ziet u waar u deze wijzigingen aanbrengt.

    ![SQL Data logboek-en back-up](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* Vergrendelde pagina's vermindering van i/o- en eventuele activiteiten Paginering inschakelen. Zie voor meer informatie, [inschakelen van de vergrendeling pagina's in de optie geheugen (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

* Als u SQL Server 2012 worden uitgevoerd, installeert u Service Pack 1 Cumulative Update 10. Deze update bevat de oplossing voor slechte prestaties op i/o tijdens het uitvoeren van select into-instructie van de tijdelijke tabel in SQL Server 2012. Zie voor meer informatie, dit [knowledge base-artikel](http://support.microsoft.com/kb/2958012).

* Houd rekening met alle gegevensbestanden comprimeren bij de overdracht in/uit van Azure.

## <a name="feature-specific-guidance"></a>Functie-richtlijnen

Sommige implementaties mogelijk extra prestatievoordelen biedt met meer geavanceerde technieken voor configuratie bereiken. De volgende lijst worden enkele SQL Server-functies waarmee u betere prestaties bereiken kunnen gemarkeerd:

* **Back-up naar Azure storage**: bij het uitvoeren van back-ups voor SQL Server in virtuele machines van Azure wordt uitgevoerd, kunt u [SQL Server back-up naar URL](https://msdn.microsoft.com/library/dn435916.aspx). Deze functie is beschikbaar vanaf SQL Server 2012 SP1 CU2 en aanbevolen voor back-ups op de gekoppelde gegevensschijven. Wanneer u back-up/herstel naar/van Azure-opslag, volgt u de aanbevelingen die op [SQL Server back-up naar URL Best Practices en probleemoplossing en terugzetten van back-ups die zijn opgeslagen in Azure Storage](https://msdn.microsoft.com/library/jj919149.aspx). U kunt ook deze back-ups automatiseren [automatische back-up voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

    Voordat u SQL Server 2012, kunt u [SQL Server back-up naar Azure-hulpprogramma](https://www.microsoft.com/download/details.aspx?id=40740). Dit hulpprogramma kan helpen om back-doorvoer met behulp van meerdere back-up stripe-doelen te vergroten.

* **SQL Server-gegevensbestanden in Azure**: deze nieuwe functie [SQL Server-gegevensbestanden in Azure](https://msdn.microsoft.com/library/dn385720.aspx), is beschikbaar vanaf SQL Server 2014. Gegevensbestanden in Azure SQL-Server uitgevoerd, ziet u vergelijkbare prestatiekenmerken als het gebruik van Azure-gegevensschijven.

## <a name="next-steps"></a>Volgende stappen

Zie voor aanbevolen procedures voor beveiliging, [veiligheidsoverwegingen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-security.md).

Raadpleeg de artikelen van andere virtuele Machine van SQL Server op [SQL Server op Azure Virtual Machines Overview](virtual-machines-windows-sql-server-iaas-overview.md). Als u vragen hebt over virtuele machines met SQL Server, raadpleegt u [Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md).
