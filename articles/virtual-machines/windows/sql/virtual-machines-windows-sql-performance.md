---
title: Best practices prestaties for SQL Server in Azure | Microsoft Docs
description: Bevat de aanbevolen procedures voor het optimaliseren van de prestaties van SQL Server in Microsoft Azure VM's.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a0c85092-2113-4982-b73a-4e80160bac36
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: jroth
ms.openlocfilehash: 03580952800e595125fc48d169f7d4aa7846dd3f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure

## <a name="overview"></a>Overzicht

Dit onderwerp bevat aanbevolen procedures voor het optimaliseren van de prestaties van SQL Server in Microsoft Azure Virtual machines. Tijdens het uitvoeren van SQL Server in Azure Virtual Machines, is het raadzaam dat u doorgaat met de dezelfde databaseprestaties afstemmen van de opties die van toepassing op SQL Server in on-premises server-omgeving. De prestaties van een relationele database in een openbare cloud is afhankelijk van veel factoren zoals de grootte van een virtuele machine en de configuratie van de gegevensschijven.

Bij het maken van SQL Server-installatiekopieën [Houd rekening met het inrichten van uw virtuele machines in de Azure portal](virtual-machines-windows-portal-sql-server-provision.md). SQL Server-VM's in de Portal met Resource Manager ingericht en volg de aanbevolen procedures.

In dit artikel is gericht op het ophalen van de *aanbevolen* prestaties voor SQL Server op Azure Virtual machines. Als uw werkbelasting minder zwaar worden belast, kunt u elke optimalisatie onderstaande mogelijk niet nodig. Houd rekening met uw prestatievereisten past en werkbelasting patronen als u deze aanbevelingen evalueren.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Lijst met snelle controle

Hier volgt een lijst met snelle controle voor optimale prestaties van SQL Server op Azure Virtual Machines:

| Onderwerp | Optimalisaties |
| --- | --- |
| [VM-grootte](#vm-size-guidance) |[DS3](../../virtual-machines-windows-sizes-memory.md) of hoger voor SQL Enterprise edition.<br/><br/>[DS2](../../virtual-machines-windows-sizes-memory.md) of hoger voor SQL Standard- en Web-edities. |
| [Storage](#storage-guidance) |Gebruik [Premium-opslag](../premium-storage.md). Standard-opslag wordt alleen aanbevolen voor ontwikkelen en testen.<br/><br/>Houd de [opslagaccount](../../../storage/common/storage-create-storage-account.md) en SQL Server VM in dezelfde regio.<br/><br/>Schakel Azure [geografisch redundante opslag](../../../storage/common/storage-redundancy.md) (geo-replicatie) van het opslagaccount. |
| [Schijven](#disks-guidance) |Gebruik een minimum van 2 [P30 schijven](../premium-storage.md#scalability-and-performance-targets) (1 voor logboekbestanden, 1 voor gegevensbestanden en TempDB).<br/><br/>Vermijd het gebruik van besturingssysteem of tijdelijke schijven voor database-opslag of registratie.<br/><br/>Lezen inschakelen voor het opslaan in cache op de schijven die als host fungeert voor de gegevensbestanden en TempDB.<br/><br/>Schakel niet opslaan in cache op een of meer schijven die als host fungeert voor het logboekbestand.<br/><br/>Belangrijk: Stop de service SQL Server bij het wijzigen van de cache-instellingen voor een virtuele machine van Azure-schijf.<br/><br/>Als u verbeterde i/o-doorvoer meerdere Azure gegevensschijven stripe.<br/><br/>Formatteren met grootten gedocumenteerde toewijzing. |
| [I/O 'S](#io-guidance) |Database pagina compressie inschakelen.<br/><br/>Schakel directe bestand de initialisatie van de gegevensbestanden worden opgeslagen.<br/><br/>Beperk of autogrow op de database uitschakelen.<br/><br/>Autoshrink op de database niet uitschakelen.<br/><br/>Verplaats alle databases naar gegevensschijven, met inbegrip van systeemdatabases.<br/><br/>SQL Server-fout logboek- en traceringsbestanden bestandsmappen naar gegevensschijven verplaatsen.<br/><br/>Het instellen van standaardbestandslocaties voor back-up en -database.<br/><br/>Schakel vergrendelde pagina's.<br/><br/>SQL Server prestaties correcties toepassen. |
| [Specifieke functies](#feature-specific-guidance) |Back-up rechtstreeks naar de blob-opslag. |

Voor meer informatie over *hoe* en *waarom* zodat deze optimalisaties Controleer de details en de richtlijnen in de volgende secties.

## <a name="vm-size-guidance"></a>Richtlijnen voor VM-grootte

Voor gevoelige toepassingen prestaties, wordt aanbevolen dat u de volgende [grootten voor virtuele machines](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json):

* **SQL Server Enterprise Edition**: DS3 of hoger
* **SQL Server Standard en Web-edities**: DS2 of hoger

## <a name="storage-guidance"></a>Richtlijnen voor opslag

DS-serie (samen met DSv2-serie en GS-serie) VMs ondersteuning [Premium-opslag](../premium-storage.md). Premium-opslag wordt aanbevolen voor alle productieworkloads.

> [!WARNING]
> Standard-opslag heeft verschillende latenties en bandbreedte en wordt alleen aanbevolen voor ontwikkel-/ testwerkbelastingen. Productieworkloads moeten Premium-opslag gebruiken.

Bovendien is het raadzaam dat u uw Azure storage-account maken in hetzelfde Datacenter als uw virtuele machines van SQL Server om te beperken van overdracht vertragingen. Wanneer u een opslagaccount maakt, uitschakelen geo-replicatie zoals consistent schrijven volgorde over meerdere schijven kan niet worden gegarandeerd. Overweeg in plaats daarvan het configureren van een SQL Server-technologie disaster recovery tussen twee Azure-datacenters. Zie voor meer informatie [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Schijven richtlijnen

Er zijn drie belangrijkste schijftypen op een virtuele machine van Azure:

* **Besturingssysteemschijf**: wanneer u een virtuele Machine in Azure maakt, ten minste één schijf zal worden gekoppeld aan het platform (aangeduid als de **C** station) naar de virtuele machine voor de schijf van uw besturingssysteem. Deze schijf is een VHD die is opgeslagen als een pagina-blob in de opslag.
* **Tijdelijke schijf**: Azure Virtual Machines bevatten een andere schijf met de naam van de tijdelijke schijf (aangeduid als de **D**: station). Dit is een schijf op het knooppunt dat kan worden gebruikt voor scratchruimte.
* **Gegevensschijven**: U kunt ook extra schijven koppelen aan uw virtuele machine als gegevensschijven en deze in de opslag worden opgeslagen als pagina-blobs.

De volgende secties beschrijven de aanbevelingen voor het gebruik van deze andere schijven.

### <a name="operating-system-disk"></a>Besturingssysteemschijf

De schijf van een besturingssysteem is een VHD die u kunt opstarten en koppelen als een versie van een besturingssysteem en wordt aangeduid als **C** station.

Standaardwaarde cachebeleid op de schijf van het besturingssysteem is **lezen/schrijven**. Voor gevoelige toepassingen prestaties, is het raadzaam dat u gegevensschijven in plaats van de besturingssysteemschijf. Zie het gedeelte over gegevensschijven hieronder.

### <a name="temporary-disk"></a>Tijdelijke schijf

Het station voor tijdelijke opslag, aangeduid als de **D**: schijf, is niet opgeslagen in Azure blob-opslag. Sla niet uw Gebruikersdatabasebestanden of transactielogboekbestanden gebruiker op de **D**: station.

Voor D-reeks, Dv2-serie en G-serie virtuele machines is het tijdelijke station op deze virtuele machines op SSD gebaseerd. Als uw werkbelasting intensief gebruik van TempDB (bijvoorbeeld voor tijdelijke objecten of complexe joins maakt) TempDB opslaan op de **D** station kan leiden tot hogere TempDB-doorvoer en lagere latentie van TempDB.

Voor virtuele machines die ondersteuning bieden voor Premium-opslag (DS-serie, DSv2-serie en GS-serie), wordt u aangeraden TempDB opslaan op een schijf die ondersteuning biedt voor Premium-opslag met lees-caching is ingeschakeld. Er is een uitzondering op deze aanbeveling; Als het gebruik van uw TempDB intensief gebruik gemaakt schrijven wordt, kunt u betere prestaties bereiken doordat de TempDB op de lokale **D** station, dat ook SSD op basis van de grootte van deze machine.

### <a name="data-disks"></a>Gegevensschijven

* **Gegevensschijven gebruiken voor gegevens en logboekbestanden**: ten minste 2 Premium-opslag gebruiken [P30 schijven](../premium-storage.md#scalability-and-performance-targets) waar één schijf bevat de logboekbestanden en de andere bevat de gegevens en bestanden van TempDB. Elke schijf Premium-opslag biedt een aantal IOPs en bandbreedte (MB/s), afhankelijk van de grootte, zoals beschreven in het volgende artikel: [Premium-opslag voor schijven met behulp van](../premium-storage.md).

   > [!NOTE]
   > Wanneer u een virtuele SQL Server-machine in de portal inricht, hebt u de optie voor het bewerken van de opslagconfiguratie van. Afhankelijk van uw configuratie Azure Hiermee configureert u een of meer schijven. Meerdere schijven worden gecombineerd tot een enkel opslaggroep met striping. De bestanden voor gegevens en logboekbestanden zich samen in deze configuratie, in plaats van twee afzonderlijke schijven bevinden. Zie voor meer informatie [opslagconfiguratie voor SQL Server-VM's](virtual-machines-windows-sql-server-storage-configuration.md).

* **Striping schijf**: voor een meer doorvoer, kunt u extra gegevensschijven toevoegen en gebruiken van Striping van de schijf. Om te bepalen het aantal gegevensschijven, moet u het aantal IOPS en bandbreedte die is vereist voor uw logboekbestanden, en voor uw gegevens en bestanden TempDB analyseren. U ziet dat andere VM-grootten verschillende beperkingen hebben op het aantal IOPs en bandbreedte die wordt ondersteund, raadpleegt u de tabellen op IOP's per [VM-grootte](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Gebruik de volgende richtlijnen:

  * Gebruik voor Windows 8/Windows Server 2012 of later, [opslagruimten](https://technet.microsoft.com/library/hh831739.aspx) met de volgende richtlijnen:

      1. Stel de interleave (stripe-grootte) op 64 KB (65536 bytes) voor OLTP-werkbelastingen en 256 KB (262144 bytes) voor magazijnbeheer werkbelastingen gegevens om te voorkomen dat van invloed op de prestaties als gevolg van onjuiste uitlijning partitie. Dit moet worden ingesteld met PowerShell.
      1. Aantal kolommen ingesteld = het aantal fysieke schijven. PowerShell gebruiken bij het configureren van meer dan 8 schijven (geen Server Manager UI). 

    De volgende PowerShell wordt een nieuwe opslaggroep gemaakt met de grootte interleave 64 KB en het aantal kolommen in 2:

    ```powershell
    $PoolCount = Get-PhysicalDisk -CanPool $True
    $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

    New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false 
    ```

  * Voor Windows 2008 R2 of eerder, kunt u dynamische schijven (OS striped volumes) en de stripe-grootte is altijd 64 KB. Houd er rekening mee dat deze optie is afgeschaft vanaf Windows 8 of Windows Server 2012. Zie voor meer informatie, de instructie ondersteuning op [Virtual Disk-Service in een overgang naar Windows Storage Management API](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

  * Als uw werkbelasting geen logboek intensieve is en niet toegewezen IOP's hoeft, kunt u slechts één opslaggroep kunt configureren. Anders maakt u twee opslaggroepen, één voor de logboekbestanden en een andere opslaggroep voor de gegevensbestanden en TempDB. Het aantal schijven die zijn gekoppeld aan elke opslaggroep op basis van de load-verwachtingen bepalen. Houd er rekening mee dat andere VM-grootten toestaan verschillend aantal gegevensschijven gekoppeld. Zie voor meer informatie [grootten voor virtuele Machines](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

  * Als u geen Premium-opslag (ontwikkelen en testen scenario's), de aanbeveling is het toevoegen van het maximum aantal gegevensschijven dat wordt ondersteund door uw [VM-grootte](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en schijf-Striping gebruiken.

* **Cachebeleid**: gegevensschijven voor de Premium-opslag, inschakelen Lees-caching op de gegevensschijven gegevensbestanden en TempDB alleen hosten. Als u geen Premium-opslag gebruikt, niet is ingeschakeld eventuele cachegebruik op eventuele gegevensschijven. Zie de volgende onderwerpen voor instructies over het configureren van de schijfcache. Zie voor het klassieke implementatiemodel van (ASM): [Set AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) en [Set AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx). Zie voor het Azure Resource Manager-implementatiemodel: [Set AzureRMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk?view=azurermps-4.4.1) en [Set AzureRMVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdatadisk?view=azurermps-4.4.1).

  > [!WARNING]
  > Stop de service SQL Server bij het wijzigen van de cache-instelling van de virtuele machine van Azure-schijven om te voorkomen dat de mogelijkheid van een beschadiging van de database.

* **Clustergrootte van NTFS**: bij het opmaken van de gegevensschijf wordt aanbevolen dat u een clustergrootte van 64 KB voor gegevens en logboekbestanden, evenals TempDB.

* **Best practices voor schijf**: wanneer een gegevensschijf verwijderen of wijzigen van een cachetype, de SQL Server-service stoppen tijdens de wijziging. Wanneer de cache-instellingen worden gewijzigd op de schijf met het besturingssysteem, Azure stopt de virtuele machine, wijzigt u het cachetype, en de virtuele machine opnieuw is opgestart. Wanneer een gegevensschijf van de cache-instellingen worden gewijzigd, wordt de virtuele machine is niet gestopt, maar de gegevensschijf is losgekoppeld van de virtuele machine tijdens de wijziging en klik vervolgens opnieuw.

  > [!WARNING]
  > De SQL Server-service tijdens deze bewerkingen stoppen is mislukt, kan leiden tot beschadiging van de database.

## <a name="io-guidance"></a>I/o-richtlijnen

* De beste resultaten met Premium-opslag zijn bereikt wanneer u uw toepassing en aanvragen parallelize. Premium-opslag is bedoeld voor scenario's waarbij de i/o-wachtrijdiepte groter dan 1, is daarom u weinig of geen prestatieverbeteringen voor seriële aanvragen single thread ziet (zelfs als ze zich intensieve opslag). Dit kan bijvoorbeeld van de testresultaten single thread-prestaties analyseprogramma's, zoals SQLIO invloed.

* Overweeg het gebruik van [database pagina compressie](https://msdn.microsoft.com/library/cc280449.aspx) zoals kunt verbeteren de prestaties van i/o-intensieve werkbelastingen. De compressie van gegevens kan echter de CPU-verbruik op de databaseserver verhogen.

* Overweeg in te schakelen instant file-initialisatie in minder tijd die nodig is voor de toewijzing van het eerste bestand. Om te profiteren van de initialisatie van de directe bestanden, u de SQL Server (MSSQLSERVER) serviceaccount met SE_MANAGE_VOLUME_NAME verlenen en toe te voegen aan de **volumeonderhoudstaken uitvoeren** beveiligingsbeleid. Als u van een SQL Server-platforminstallatiekopie voor Azure gebruikmaakt, de standaard-serviceaccount (NT Service\MSSQLSERVER) is niet toegevoegd aan de **volumeonderhoudstaken uitvoeren** beveiligingsbeleid. Initialisatie van de directe bestanden met andere woorden, niet is ingeschakeld in een installatiekopie van SQL Server-Azure-platform. Na het toevoegen van de SQL Server-serviceaccount de **volumeonderhoudstaken uitvoeren** beveiligingsbeleid, de SQL Server-service opnieuw starten. Er zijn beveiligingsoverwegingen voor het gebruik van deze functie. Zie voor meer informatie [initialiseren van de Database-bestand](https://msdn.microsoft.com/library/ms175935.aspx).

* **autogrow** wordt beschouwd als slechts een geval van nood voor onverwachte groei. Beheert de groei van uw gegevens en logboekbestanden op dagelijks met autogrow niet. Als autogrow wordt gebruikt, is vooraf uitbreiding van het bestand met de schakeloptie grootte.

* Zorg ervoor dat **autoshrink** is uitgeschakeld om te voorkomen dat onnodige overhead die u kunt de prestaties negatief beïnvloeden.

* Verplaats alle databases naar gegevensschijven, met inbegrip van systeemdatabases. Zie voor meer informatie [systeemdatabases verplaatsen](https://msdn.microsoft.com/library/ms345408.aspx).

* SQL Server-fout logboek- en traceringsbestanden bestandsmappen naar gegevensschijven verplaatsen. Dit is mogelijk in SQL Server Configuration Manager met de rechtermuisknop op uw SQL Server-exemplaar eigenschappen te selecteren. De instellingen voor foutpagina logboek- en traceringsbestanden bestand kunnen worden gewijzigd in de **opstartparameters** tabblad. De map Dump is opgegeven in de **Geavanceerd** tabblad. De volgende schermafbeelding ziet waar de fout logboek starten van de parameter.

    ![Schermafbeelding van de SQL-foutenlogboek](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

* Het instellen van standaardbestandslocaties voor back-up en -database. Gebruik de aanbevelingen in dit onderwerp en de wijzigingen aanbrengen in het eigenschappenvenster van de Server. Zie voor instructies [weergeven of wijzigen van de standaard locaties voor gegevens en logboekbestanden (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). De volgende schermafbeelding ziet u waar u deze wijzigingen aanbrengt.

    ![Logboekbestanden voor SQL-gegevens en back-up](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)
* Vergrendelde pagina's te verminderen, IO en eventuele activiteiten Paginering inschakelen. Zie voor meer informatie [inschakelen van de vergrendeling pagina's in de optie geheugen (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

* Als u SQL Server 2012 uitvoert, installeert u Service Pack 1 Cumulative Update 10. Deze update bevat de oplossing voor slechte prestaties in i/o tijdens het uitvoeren van select into-instructie van de tijdelijke tabel in SQL Server 2012. Zie voor meer informatie, dit [knowledge base-artikel](http://support.microsoft.com/kb/2958012).

* Overweeg het comprimeren van gegevensbestanden bij de overdracht / out van Azure.

## <a name="feature-specific-guidance"></a>Functie voor specifieke aanwijzingen

Sommige implementaties mogelijk extra prestatievoordelen met behulp van meer geavanceerde configuratie technieken bereiken. De volgende lijst worden gemarkeerd voor sommige SQL Server-functies waarmee u betere prestaties bereiken:

* **Back-up naar Azure storage**: bij het uitvoeren van back-ups voor SQL Server op Azure virtuele machines, kunt u [SQL Server back-up naar URL](https://msdn.microsoft.com/library/dn435916.aspx). Deze functie is beschikbaar vanaf SQL Server 2012 SP1 CU2 en aanbevolen voor back-ups op de schijven bijgesloten gegevens. Wanneer u back-up/herstel naar/van de Azure-opslag, volgt u de aanbevelingen op [SQL Server back-up naar URL aanbevolen procedures en probleemoplossing en terugzetten van back-ups die zijn opgeslagen in Azure Storage](https://msdn.microsoft.com/library/jj919149.aspx). U kunt ook deze back-ups met automatiseren [automatische back-up voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

    Voorafgaand aan SQL Server 2012, kunt u [SQL Server back-up naar Azure hulpprogramma](https://www.microsoft.com/download/details.aspx?id=40740). Dit hulpprogramma kunt u back-doorvoer met meerdere back-stripe-doelen te verhogen.

* **SQL Server-gegevensbestanden in Azure**: deze nieuwe functie [SQL Server-gegevensbestanden in Azure](https://msdn.microsoft.com/library/dn385720.aspx), is beschikbaar vanaf SQL Server 2014. Met SQL Server met gegevensbestanden in Azure, ziet u vergelijkbare prestatiekenmerken als het gebruik van Azure gegevensschijven.

## <a name="next-steps"></a>Volgende stappen

Zie voor aanbevolen procedures voor beveiliging, [beveiligingsoverwegingen voor het SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-security.md).

Zie de andere virtuele Machine van SQL Server-onderwerpen op [SQL Server op Azure Virtual Machines Overview](virtual-machines-windows-sql-server-iaas-overview.md).
