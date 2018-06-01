---
title: Best practices prestaties for SQL Server in Azure Stack van virtuele Machines
description: Bevat de aanbevolen procedures voor het optimaliseren van de prestaties van SQL Server in Microsoft Azure Stack van virtuele Machines.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: mabrigg
ms.reviewer: anajod
ms.openlocfilehash: 00c67503f5b9e0027cbb62520e392f56420a75e6
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34701378"
---
# <a name="optimize-sql-server-performance"></a>SQL Server optimaliseren

In dit artikel bevat richtlijnen voor het optimaliseren van de prestaties van SQL Server in Microsoft Azure-Stack virtuele machines. Wanneer SQL Server wordt uitgevoerd in de Stack Azure virtuele machines, gebruikt u dezelfde database-prestaties optimaliseren opties van toepassing op SQL Server in een server on-premises omgeving. De prestaties van een relationele database in een Azure-Stack-cloud, hangt af van veel factoren. Factoren zijn de familie grootte van een virtuele machine en de configuratie van de gegevensschijven.

Bij het maken van SQL Server-installatiekopieën [Houd rekening met het inrichten van uw virtuele machines in de Stack van Azure-portal](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). De SQL-IaaS-uitbreiding downloaden uit het beheer van de Marketplace in het beheerportal van de Azure-Stack en downloaden van uw keuze SQL virtuele machine virtuele harde schijven (VHD's). Het gaat hierbij om SQL2014SP2 SQL2016SP1 en SQL2017.

> [!NOTE]  
> Terwijl het artikel wordt beschreven hoe een SQL Server-machine met de globale Azure portal inrichten, de richtlijn geldt ook voor Azure-Stack met de volgende verschillen: SSD is niet beschikbaar voor de besturingssysteemschijf, beheerde schijven zijn niet beschikbaar is, en Er zijn kleine verschillen in de configuratie van de opslag.

Ophalen van de *aanbevolen* prestaties voor SQL Server op virtuele machines van Azure-Stack is de focus van dit artikel. Als uw werkbelasting minder zwaar worden belast, kunt u elke aanbevolen optimalisatie mogelijk niet nodig. Houd rekening met uw prestatievereisten past en werkbelasting patronen als u deze aanbevelingen evalueren.

> [!NOTE]  
> Raadpleeg voor de prestaties van de richtlijnen voor het SQL Server in Azure virtuele machines, [in dit artikel](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance).

## <a name="before-you-begin"></a>Voordat u begint
De volgende controlelijst is voor optimale prestaties van SQL Server op Azure-Stack virtuele machines:


|Onderwerp|Optimalisaties|
|-----|-----|
|Grootte virtuele machine |[DS3](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) of hoger voor SQL Server Enterprise edition.<br><br>[DS2](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) of hoger voor SQL Server Standard edition en Web edition.|
|Storage |Gebruik een virtuele machine-familie die ondersteuning biedt voor [Premium-opslag](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-acs-differences).|
|Disks |Gebruik een minimum van twee gegevensschijven (één voor logboekbestanden) en één voor het gegevensbestand en TempDB en kies de schijfgrootte van de op basis van de capaciteitsbehoeften van uw. De standaardwaarde instellen bestandslocaties gegevens op deze schijven tijdens de installatie van SQL Server.<br><br>Vermijd het gebruik van besturingssysteem of tijdelijke schijven voor database-opslag of registratie.<br>Als u verbeterde i/o-doorvoer met behulp van opslagruimten meerdere Azure gegevensschijven stripe.<br><br>Formatteren met grootten gedocumenteerde toewijzing.|
|I/O 'S|Schakel directe bestand de initialisatie van de gegevensbestanden worden opgeslagen.<br><br>Autogrow van de databases met redelijk klein vaste intervallen (64 MB - 256 MB) beperken.<br><br>Autoshrink op de database niet uitschakelen.<br><br>Back-up en database standaardbestandslocaties op gegevensschijven, niet de besturingssysteemschijf instellen.<br><br>Schakel vergrendelde pagina's.<br><br>SQL Server-servicepacks en cumulatieve updates toepassen.|
|Functiespecifieke|Back-up rechtstreeks naar de blob-opslag (indien ondersteund door de versie van SQL Server gebruikt).|
|||

Voor meer informatie over *hoe* en *waarom* zodat deze optimalisaties Controleer de details en de richtlijnen in de volgende secties.

## <a name="virtual-machine-size-guidance"></a>Richtlijnen voor VM-grootte

Voor de volgende prestatie-intensieve toepassingen [grootten van virtuele machines](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) worden aanbevolen:

- **SQL Server Enterprise edition:** DS3 of hoger

- **SQL Server Standard edition en Web edition:** DS2 of hoger

Met de Azure-Stack is er geen prestatieverschil tussen de DS- en DS_v2 virtuele machine familie reeksen.

## <a name="storage-guidance"></a>Richtlijnen voor opslag

DS-serie (samen met DSv2-serie) virtuele machines in Azure-Stack bieden de maximumversie besturingssysteem schijf- en schijfdoorvoer (IOPS). Een virtuele machine van de DS- of DSv2 reeks biedt maximaal 1000 IOP's voor de besturingssysteemschijf en maximaal 2300 IOP's per gegevensschijf, ongeacht het type of de grootte van de gekozen schijf.

De doorvoercapaciteit van de schijf wordt bepaald uniek op basis van de virtuele machine familie reeks. U kunt [Raadpleeg dit artikel](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) voor het identificeren van de schijf gegevensdoorvoer per virtuele machine familie reeks.

> [!NOTE]  
> Voor productieworkloads, selecteert u een DS-serie of DSv2-serie virtuele machine voor de maximale IOPS mogelijk in het besturingssysteem schijf- en gegevensschijven.

Bij het maken van een opslagaccount in Azure-Stack, heeft de geo-replicatie-optie geen effect, omdat deze mogelijkheid niet beschikbaar in Azure-Stack is.

## <a name="disks-guidance"></a>Schijven richtlijnen

Er zijn drie belangrijkste schijftypen op een virtuele machine van Azure-Stack:

- **Schijf van besturingssysteem:** wanneer u een virtuele machine van Azure-Stack maakt, het platform ten minste één schijf koppelt (aangeduid als de **C** station) op de virtuele machine voor de schijf van uw besturingssysteem. Deze schijf is een VHD die is opgeslagen als een pagina-blob in de opslag.

- **Tijdelijke schijf:** Stack Azure virtuele machines bevatten een andere schijf met de naam van de tijdelijke schijf (aangeduid als de **D** station). Dit is een schijf op het knooppunt dat kan worden gebruikt voor scratchruimte.

- **Gegevensschijven:** kunt u extra schijven aan de virtuele machine als gegevensschijven koppelen en deze schijven worden opgeslagen in de opslag als pagina-blobs.

De volgende secties beschrijven de aanbevelingen voor het gebruik van deze andere schijven.

### <a name="operating-system-disk"></a>Besturingssysteemschijf

De schijf van een besturingssysteem is een VHD die u kunt opstarten en koppelen als een versie van een besturingssysteem en wordt aangeduid als **C** station.

### <a name="temporary-disk"></a>Tijdelijke schijf

Het station voor tijdelijke opslag, aangeduid als de **D** schijf, is niet persistent. Sla alle gegevens die u niet eens verliest, zoals uw Gebruikersdatabasebestanden of gebruiker transactielogboekbestanden, op bent niet de **D** station.

U wordt aangeraden de TempDB op een gegevensschijf opslaan als elke gegevensschijf een maximum van maximaal 2300 IOP's per gegevensschijf biedt.

### <a name="data-disks"></a>Gegevensschijven

- **Gebruik de gegevensschijven voor gegevens en logboekbestanden.** Als u geen striping van de schijf gebruikt, gebruikt u twee gegevensschijven van een virtuele machine die ondersteuning biedt voor Premium-opslag, waarbij één schijf bevat de logboekbestanden en de andere bevat de gegevens en bestanden van TempDB. Elke gegevensschijf biedt een aantal IOPS en bandbreedte (MB/s), afhankelijk van de virtuele machine-familie, zoals beschreven in [grootten van virtuele machines in Azure-Stack ondersteund](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes). Als u van een techniek schijf striping zoals opslagruimten gebruikmaakt, plaatst u alle gegevens en logboekbestanden bestanden op hetzelfde station (inclusief TempDB). Deze configuratie geeft het maximumaantal IOPS beschikbaar voor SQL Server te gebruiken, ongeacht welk bestand moet op een willekeurig moment.

> [!NOTE]  
> Wanneer u een virtuele machine van SQL Server in de portal inricht, hebt u de optie voor het bewerken van de opslagconfiguratie van. Afhankelijk van uw configuratie Azure Stack Hiermee configureert u een of meer schijven. Meerdere schijven worden gecombineerd tot een enkel opslaggroep. De bestanden voor gegevens en logboekbestanden zich bevinden samen in deze configuratie.

- **Striping van de schijf:** voor een meer doorvoer, kunt u extra gegevensschijven toevoegen en gebruiken van striping van de schijf. Om te bepalen het aantal gegevensschijven die u nodig hebt, het aantal IOPS en bandbreedte die is vereist voor uw logboekbestanden en voor uw gegevens en bestanden TempDB analyseren. Merk op dat limieten IOP's per gegevensschijf op basis van de virtuele machine reeks familie en niet op basis van de grootte van de virtuele machine. Netwerk-bandbreedtelimieten echter zijn gebaseerd op de grootte van de virtuele machine. Zie de tabellen op [de grootte van virtuele machine in Azure-Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) voor meer informatie. Gebruik de volgende richtlijnen:

    - Voor Windows Server 2012 of later gebruiken [opslagruimten](https://technet.microsoft.com/library/hh831739.aspx) met de volgende richtlijnen:

        1.  De interleave (stripe-grootte) op 64 KB (65.536 bytes) voor online transactieverwerking (OLTP-systemen)-workloads en 256 KB (262.144 bytes) voor magazijnbeheer werkbelastingen gegevens om te voorkomen dat van invloed op de prestaties als gevolg van onjuiste uitlijning partitie instellen. Dit moet worden ingesteld met PowerShell.

        2.  Aantal kolommen ingesteld = het aantal fysieke schijven. PowerShell gebruiken bij het configureren van meer dan acht schijven (geen Server Manager UI).

            De volgende PowerShell wordt een nieuwe opslaggroep gemaakt met de interleave-grootte die is ingesteld op 64 KB en het aantal kolommen in 2:

          ```PowerShell  
          $PoolCount = Get-PhysicalDisk -CanPool $True
          $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

          New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false
          ```

- Het aantal schijven die zijn gekoppeld aan uw opslaggroep op basis van de load-verwachtingen bepalen. Houd er rekening mee dat andere virtuele machine grootten toestaan verschillend aantal gegevensschijven gekoppeld. Zie voor meer informatie [grootten van virtuele machines in Azure-Stack ondersteund](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes).
- Om de maximale IOPS mogelijk voor gegevensschijven aangeraden wordt om toe te voegen, het maximum aantal gegevensschijven dat wordt ondersteund door uw [grootte van virtuele machine](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) en schijf-striping gebruiken.
- **Clustergrootte van NTFS:** bij het opmaken van de gegevensschijf wordt aanbevolen dat u een clustergrootte van 64 KB voor gegevens en logboekbestanden, evenals TempDB.
- **Schijf beheeractiviteiten:** wanneer u een gegevensschijf verwijdert, stopt u de SQL Server-service tijdens het wijzigen van de. Ook niet wijzigen cache-instellingen op de schijven omdat het niet mogelijk om een verbeterde prestaties.

> [!WARNING]  
> De SQL-Service tijdens deze bewerkingen stoppen is mislukt, kan leiden tot beschadiging van de database.

## <a name="io-guidance"></a>I/o-richtlijnen

- Overweeg in te schakelen instant file-initialisatie in minder tijd die nodig is voor de toewijzing van het eerste bestand. Om te profiteren van de initialisatie van de directe bestanden, u de SQL Server (MSSQLSERVER) serviceaccount met verlenen **SE_MANAGE_VOLUME_NAME** en toe te voegen aan de **volumeonderhoudstaken uitvoeren** beveiliging het beleid. Als u gebruikmaakt van een SQL Server-platforminstallatiekopie voor Azure, de standaard-serviceaccount (**NT Service\MSSQLSERVER**) is niet toegevoegd aan de **volumeonderhoudstaken uitvoeren** beveiligingsbeleid. Initialisatie van de directe bestanden met andere woorden, niet is ingeschakeld in een installatiekopie van SQL Server-Azure-platform. Na het toevoegen van de SQL Server-serviceaccount de **volumeonderhoudstaken uitvoeren** beveiligingsbeleid, de SQL Server-service opnieuw starten. Er zijn beveiligingsoverwegingen voor het gebruik van deze functie. Zie voor meer informatie [initialiseren van de Database-bestand](https://msdn.microsoft.com/library/ms175935.aspx).
- **Autogrow** geval van nood voor onverwachte groei is. Beheert de groei van uw gegevens en logboekbestanden op dagelijks met autogrow niet. Als autogrow wordt gebruikt, vooraf groter dan het bestand met de **grootte** overschakelen.
- Zorg ervoor dat **autoshrink** is uitgeschakeld om te voorkomen dat onnodige overhead die u kunt de prestaties negatief beïnvloeden.
- Het instellen van standaardbestandslocaties voor back-up en -database. Gebruik de aanbevelingen in dit artikel en de wijzigingen aanbrengen in het eigenschappenvenster van de Server. Zie voor instructies [weergeven of wijzigen van de standaard locaties voor gegevens en logboekbestanden (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). De volgende Schermafbeelding toont waar u deze wijzigingen aanbrengen:

    > ![Weergeven of wijzigen van de standaardlocaties bevinden](./media/sql-server-vm-considerations/image1.png)

- Vergrendelde pagina's te verminderen, IO en eventuele activiteiten Paginering inschakelen. Zie voor meer informatie [inschakelen van de vergrendeling pagina's in de optie geheugen (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Overweeg het comprimeren van gegevensbestanden bij de overdracht / out van de Azure-Stack, waaronder back-ups.

## <a name="feature-specific-guidance"></a>Functie-specifieke richtlijnen

Sommige implementaties mogelijk extra prestatievoordelen met behulp van meer geavanceerde configuratie technieken bereiken. De volgende lijst worden gemarkeerd voor sommige SQL Server-functies waarmee u betere prestaties bereiken:

- **Back-up naar Azure** **opslag.** Bij het uitvoeren van back-ups voor SQL Server op Azure-Stack virtuele machines, kunt u SQL Server back-up naar URL. Deze functie is beschikbaar vanaf SQL Server 2012 SP1 CU2 en aanbevolen voor back-ups op de schijven bijgesloten gegevens.

    Als uw back-up of herstellen met behulp van Azure-opslag, volg de aanbevelingen in [SQL Server back-up naar URL aanbevolen procedures en probleemoplossing](https://msdn.microsoft.com/library/jj919149.aspx) en [terugzetten van back-ups opgeslagen in Microsoft Azure](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure?view=sql-server-2017). U kunt ook deze back-ups met automatiseren [automatische back-up voor SQL Server in Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup).

-   **Back-up naar Azure Stack-opslag.** U kunt back-up naar Azure Stack-opslag op een vergelijkbare manier als bij de back-ups op Azure Storage. Wanneer u een back-up in SQL Server Management Studio (SSMS) maakt, moet u de configuratie-informatie handmatig invoeren. U kunt SSMS niet gebruiken om de storage-container of de Shared Access Signature te maken. SSMS maakt alleen verbinding met Azure-abonnementen niet Stack Azure-abonnementen. In plaats daarvan moet u de storage-account, een container en een Shared Access Signature maken in de Stack van Azure-portal of met PowerShell.

    Wanneer u de informatie in het dialoogvenster back-up van SQL Server plaatsen:

    ![SQL Server-back-up](./media/sql-server-vm-considerations/image3.png)

    > [!NOTE]  
    > De Shared Access Signature is het SAS-token van de Stack van Azure-portal, zonder de voorloopspaties '?' in de tekenreeks. Als u de functie kopiëren vanuit de portal gebruikt, moet u de voorloopspaties verwijderen '?' voor de token werken vanuit SQL Server.

    Eenmaal hebt u de back-updoel ingesteld en geconfigureerd in SQL Server, u kunt een back-up naar de Stack van Azure blob-opslag.

## <a name="next-steps"></a>Volgende stappen

[Met services of het ontwikkelen van apps voor Azure-Stack](azure-stack-considerations.md)