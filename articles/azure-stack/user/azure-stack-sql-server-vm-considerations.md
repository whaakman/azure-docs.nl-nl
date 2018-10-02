---
title: Aanbevolen procedures voor prestaties voor SQL Server op virtuele Machines van Azure Stack
description: Bevat de aanbevolen procedures voor het optimaliseren van prestaties van SQL Server in Microsoft Azure Stack Virtual Machines.
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
ms.date: 09/28/2018
ms.author: mabrigg
ms.reviewer: anajod
ms.openlocfilehash: e784185cfc7f2c588db354bab1cfb36934b9c417
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585863"
---
# <a name="optimize-sql-server-performance"></a>SQL Server-prestaties optimaliseren

Dit artikel bevat richtlijnen voor het optimaliseren van prestaties van SQL Server in virtuele machines van Microsoft Azure Stack. Wanneer SQL Server wordt uitgevoerd in virtuele machines van Azure Stack, gebruikt u dezelfde database-prestaties optimaliseren opties van toepassing op SQL Server in een on-premises server-omgeving. De prestaties van een relationele database in een Azure Stack-cloud, is afhankelijk van veel factoren. Factoren zijn de familie grootte van een virtuele machine en de configuratie van de gegevensschijven.

Bij het maken van SQL Server-installatiekopieën, [Houd rekening met het inrichten van uw virtuele machines in de Azure Stack-portal](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). De SQL IaaS-extensie downloaden uit het beheer van de Marketplace in de Azure Stack-beheerportal en downloaden van uw eigen keuze aan SQL virtuele machine virtuele harde schijven (VHD's). Hieronder vallen SQL2014SP2, SQL2016SP1 en SQL2017.

> [!NOTE]  
> Hoewel het artikel wordt beschreven hoe u een SQL Server-machine inrichten met de globale Azure-portal, de richtlijn geldt ook voor Azure Stack met de volgende verschillen: SSD is niet beschikbaar voor de besturingssysteemschijf, beheerde schijven zijn niet beschikbaar, en Er zijn kleine verschillen in de configuratie van de opslag.

Aan de *aanbevolen* prestaties voor SQL Server op virtuele machines van Azure Stack is de focus van dit artikel. Als uw werkbelasting minder zwaar worden belast is, kan u niet elke aanbevolen optimalisatie nodig. Houd rekening met uw prestatiebehoeften en patronen van werkbelasting kijkt u bij het evalueren van deze aanbevelingen.

> [!NOTE]  
> Raadpleeg voor prestatierichtlijnen voor SQL Server in virtuele machines van Azure, [in dit artikel](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance).

## <a name="before-you-begin"></a>Voordat u begint
De volgende controlelijst is voor optimale prestaties van SQL Server op virtuele machines van Azure Stack:


|Onderwerp|Optimalisaties|
|-----|-----|
|Grootte virtuele machine |[DS3](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) of hoger voor SQL Server Enterprise edition.<br><br>[DS2](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) of hoger voor SQL Server Standard edition en Web edition.|
|Storage |Gebruik van een virtuele machine-serie die ondersteuning biedt voor [Premium storage](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-acs-differences).|
|Disks |Minimaal twee gegevensschijven (één voor de logboekbestanden) en één voor het gegevensbestand en TempDB gebruiken en de grootte van de schijf op basis van de capaciteitsbehoeften van uw kiezen. De standaardwaarde instellen locaties van gegevens naar deze schijven tijdens de installatie van SQL Server.<br><br>Vermijd het gebruik van besturingssysteem of tijdelijke schijven voor de database-opslag of logboekregistratie.<br>Stripe meerdere Azure-gegevensschijven om op te halen van hogere i/o-doorvoer met behulp van opslagruimten.<br><br>Opmaken met gedocumenteerde toewijzing grootten.|
|I/O|Bestanden onmiddellijk de initialisatie van de gegevensbestanden inschakelen.<br><br>Beperken van autogrow voor de databases met een veelvoud van redelijk klein vaste (64 MB - 256 MB).<br><br>Schakel Automatische verkleining van de database.<br><br>Instellen van back-up- en database standaardbestandslocaties op gegevensschijven, niet de besturingssysteemschijf.<br><br>Vergrendelde pagina's inschakelen.<br><br>SQL Server servicepacks en de cumulatieve updates van toepassing.|
|Functiespecifieke|Back-up rechtstreeks naar de blob-opslag (indien ondersteund door de versie van SQL Server wordt gebruikt).|
|||

Voor meer informatie over *hoe* en *waarom* zodat deze optimalisaties Raadpleeg de details en de richtlijnen van de volgende secties.

## <a name="virtual-machine-size-guidance"></a>Richtlijnen voor VM-grootte

Voor prestatie-intensieve toepassingen, de volgende [grootten van virtuele machines](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) worden aanbevolen:

- **SQL Server Enterprise edition:** DS3 of hoger

- **SQL Server Standard edition en Web edition:** DS2 of hoger

Met Azure Stack is er geen prestatieverschil tussen de DS- en DS_v2 virtuele machine-familie reeksen.

## <a name="storage-guidance"></a>Richtlijnen voor opslag

DS-serie (samen met de DSv2-serie) virtuele machines in Azure Stack bieden de hoogste besturingssysteemversie schijf en schijfdoorvoer (IOPS). Een virtuele machine uit de DS- of DSv2-serie biedt maximaal 1000 IOPS voor de besturingssysteemschijf en tot wel 2300 IOP's per gegevensschijf, ongeacht het type of de grootte van de gekozen schijf.

Doorvoer gegevensschijf wordt uniek op basis van de virtuele machine-familie reeks bepaald. U kunt [verwijzen naar dit artikel](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) voor het identificeren van de gegevens schijfdoorvoer per virtuele machine-familie reeks.

> [!NOTE]  
> Voor werkbelastingen voor productie, selecteert u een virtuele machine uit de DS-serie of DSv2-serie voor de maximale IOPS mogelijk op het besturingssysteem schijf- en gegevensschijven.

Bij het maken van een storage-account in Azure Stack, heeft de geo-replicatie-optie geen effect, omdat deze mogelijkheid niet beschikbaar in Azure Stack is.

## <a name="disks-guidance"></a>Richtlijnen voor schijven

Er zijn drie schijftypen van de belangrijkste op een virtuele machine van Azure Stack:

- **Besturingssysteemschijf:** wanneer u een virtuele machine van Azure Stack maakt, koppelt u het platform ten minste één schijf (met het label de **C** station) op de virtuele machine voor de schijf van uw besturingssysteem. Deze schijf is een VHD die is opgeslagen als pagina-blob in de opslag.

- **Tijdelijke schijf:** Azure Stack virtuele machines bevatten een andere schijf met de naam de tijdelijke schijf (met het label de **D** station). Dit is een schijf op het knooppunt dat kan worden gebruikt voor scratchruimte.

- **Gegevensschijven:** kunt u extra schijven aan uw virtuele machine als gegevensschijven koppelen en deze schijven in de opslag worden opgeslagen als pagina-blobs.

De volgende secties beschrijven de aanbevelingen voor het gebruik van deze verschillende schijven.

### <a name="operating-system-disk"></a>Besturingssysteemschijf

Een besturingssysteemschijf is een VHD die u kunt opstarten en koppelen als een versie van een besturingssysteem als het label **C** station.

### <a name="temporary-disk"></a>Tijdelijke schijf

Het station voor tijdelijke opslag, met het label de **D** station, worden niet bewaard. Sla niet alle gegevens die u wilt gaan verloren, zoals de databasebestanden van de gebruiker of de gebruiker transactielogboekbestanden, op de **D** station.

U wordt aangeraden TempDB opslaan op een gegevensschijf, omdat elke gegevensschijf een maximum van maximaal 2300 IOP's per gegevensschijf biedt.

### <a name="data-disks"></a>Gegevensschijven

- **Gegevensschijven gebruiken voor gegevens en logboekbestanden.** Als u niet striping van de schijf gebruikt, moet u twee gegevensschijven van een virtuele machine die ondersteuning biedt voor Premium-opslag, waarbij één schijf bevat de logboekbestanden en de andere bevat de gegevens en het TempDB-bestanden gebruiken. Elke gegevensschijf biedt een aantal IOPS en bandbreedte (MB/s), afhankelijk van de virtuele machine-familie, zoals beschreven in [grootten van virtuele machines worden ondersteund in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes). Als u van een techniek schijfsegmentering zoals opslagruimten gebruikmaakt, plaatst u alle gegevens en logboekbestanden bestanden op hetzelfde station (met inbegrip van TempDB). Deze configuratie geeft u het maximum aantal IOP's die beschikbaar zijn voor SQL Server om te gebruiken, ongeacht welk bestand moet op een bepaald moment.

> [!NOTE]  
> Wanneer u een SQL Server-machine in de portal inrichten, hebt u de mogelijkheid om de opslagconfiguratie bewerken. Afhankelijk van uw configuratie, configureert Azure Stack u een of meer schijven. Meerdere schijven zijn gecombineerd tot een één opslaggroep. De gegevens en logboekbestanden bestanden bevinden zich samen in deze configuratie.

- **Schijfsegmentering:** meer doorvoer, kunt u extra gegevensschijven toevoegen en gebruiken van striping van de schijf. Om te bepalen het aantal gegevensschijven die u nodig hebt, het aantal IOPS en bandbreedte die vereist is voor uw logboekbestanden en voor uw gegevens en het TempDB-bestanden te analyseren. U ziet dat de IOPS-limieten per gegevensschijf op basis van de virtuele machine-serie en niet op basis van de grootte van de virtuele machine. Netwerk-bandbreedtelimieten, echter zijn gebaseerd op de grootte van de virtuele machine. Overzicht van de tabellen op [groottes van virtuele machines in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) voor meer informatie. Gebruik de volgende richtlijnen:

    - Voor Windows Server 2012 of hoger, gebruikt u [opslagruimten](https://technet.microsoft.com/library/hh831739.aspx) met de volgende richtlijnen:

        1.  De interleave (Streepgrootte) aan 64 KB (65.536 bytes) instellen voor online transactieverwerking workloads (OLTP) en 256 KB (262.144 bytes) voor magazijnbeheer data-workloads om te voorkomen dat invloed op de prestaties vanwege databasetransactielogboeken. Dit moet worden ingesteld met PowerShell.

        2.  Het aantal kolommen ingesteld = aantal fysieke schijven. PowerShell gebruiken bij het configureren van meer dan acht schijven (geen Server Manager UI).

            De volgende PowerShell wordt bijvoorbeeld een nieuwe opslaggroep gemaakt met de interleave-grootte die is ingesteld op 64 KB en het aantal kolommen in 2:

          ```PowerShell  
          $PoolCount = Get-PhysicalDisk -CanPool $True
          $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

          New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false
          ```

- Bepaalt het aantal schijven die zijn gekoppeld aan uw opslaggroep op basis van de load-verwachtingen. Houd er rekening mee dat andere VM-grootten verschillende aantallen stellen gekoppelde gegevensschijven. Zie voor meer informatie, [grootten van virtuele machines worden ondersteund in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes).
- Om de maximale IOPS mogelijk voor gegevensschijven, is de aanbeveling is om toe te voegen van het maximum aantal gegevensschijven dat wordt ondersteund door uw [grootte van virtuele machine](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) en gebruik schijfsegmentering.
- **Grootte van NTFS-toewijzingseenheid:** bij het opmaken van de gegevensschijf, is het aanbevolen dat u een clustergrootte van 64 KB voor gegevens en logboekbestanden, evenals TempDB gebruiken.
- **Procedures voor schijf:** bij het verwijderen van een gegevensschijf, stopt u de SQL Server-service tijdens de wijziging. Ook niet wijzigen cache-instellingen op de schijven als het niet mogelijk om een verbeterde prestaties.

> [!WARNING]  
> Fout bij stoppen van de SQL-Service tijdens deze bewerkingen kan leiden tot beschadiging van de database.

## <a name="io-guidance"></a>I/o-richtlijnen

- Overweeg in te schakelen van de initialisatie van de directe bestanden om de tijd die is vereist voor initiële bestandstoewijzing te verkorten. Om te profiteren van de initialisatie van de bestanden onmiddellijk, verleent u de SQL Server (MSSQLSERVER) serviceaccount met **SE_MANAGE_VOLUME_NAME** en toe te voegen aan de **onderhoudstaken op Volume uitvoeren** beveiliging het beleid. Als u gebruikmaakt van een installatiekopie van SQL Server-platform voor Azure, de standaard-serviceaccount (**NT Service\MSSQLSERVER**) is niet toegevoegd aan de **onderhoudstaken op Volume uitvoeren** beveiligingsbeleid. Met andere woorden, is direct bestanden initialisatie niet ingeschakeld in een installatiekopie van SQL Server Azure-platform. Na het toevoegen van de SQL Server-serviceaccount voor de **onderhoudstaken op Volume uitvoeren** beveiligingsbeleid, de SQL Server-service opnieuw starten. Er zijn mogelijk beveiligingsoverwegingen voor het gebruik van deze functie. Zie voor meer informatie, [Database File Initialization](https://msdn.microsoft.com/library/ms175935.aspx).
- **Autogrow** noodplan voor onverwachte groei is. Beheert niet de groei van uw gegevens en logboekbestanden op dagelijks met autogrow. Als automatische groei wordt gebruikt, vooraf vergroten het bestand met de **grootte** overschakelen.
- Zorg ervoor dat **Automatische verkleining** is uitgeschakeld om te voorkomen dat onnodige overhead die kan de prestaties negatief beïnvloeden.
- Setup standaardbestandslocaties voor back-up en -database. Gebruik de aanbevelingen in dit artikel en breng de wijzigingen in het eigenschappenvenster van de Server. Zie voor instructies [weergeven of wijzigen van de standaard locaties voor gegevens en logboekbestanden (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). De volgende schermafbeelding ziet u waar u deze wijzigingen aanbrengen:

    > ![Weergeven of wijzigen van de standaardlocaties bevinden](./media/sql-server-vm-considerations/image1.png)

- Vergrendelde pagina's vermindering van i/o- en eventuele activiteiten Paginering inschakelen. Zie voor meer informatie, [inschakelen van de vergrendeling pagina's in de optie geheugen (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Houd rekening met alle gegevensbestanden comprimeren bij de overdracht in/uit van Azure Stack, met inbegrip van back-ups.

## <a name="feature-specific-guidance"></a>Functie-richtlijnen

Sommige implementaties mogelijk extra prestatievoordelen biedt met meer geavanceerde technieken voor configuratie bereiken. De volgende lijst worden sommige SQL Server-functies waarmee u betere prestaties bereiken gemarkeerd:

- **Back-up naar Azure** **opslag.** Bij het uitvoeren van back-ups voor SQL Server in virtuele machines van Azure Stack wordt uitgevoerd, kunt u SQL Server back-up naar URL. Deze functie is beschikbaar vanaf SQL Server 2012 SP1 CU2 en aanbevolen voor back-ups op de gekoppelde gegevensschijven.

    Als uw back-up of herstellen met behulp van Azure-opslag, volg de aanbevelingen in [SQL Server back-up naar URL Best Practices en probleemoplossing](https://msdn.microsoft.com/library/jj919149.aspx) en [terugzetten van back-ups die zijn opgeslagen in Microsoft Azure](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure?view=sql-server-2017). U kunt ook deze back-ups automatiseren [automatische back-up voor SQL Server in Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup).

-   **Back-up naar Azure Stack-opslag.** U kunt back-up naar Azure Stack-opslag op een vergelijkbare manier als u zich bij de back-ups op Azure Storage. Wanneer u een back-up in SQL Server Management Studio (SSMS) maakt, moet u de configuratie-informatie handmatig invoeren. U kunt SSMS niet gebruiken om de storage-container of de handtekening voor gedeelde toegang te maken. SSMS maakt alleen verbinding met Azure-abonnementen, niet Azure Stack-abonnementen. In plaats daarvan moet u de storage-account, container en Shared Access Signature maken in de Azure Stack-portal of met PowerShell.

    Wanneer u de informatie in het dialoogvenster SQL Server-back-up plaatsen:

    ![Back-up van SQL Server](./media/sql-server-vm-considerations/image3.png)

    > [!NOTE]  
    > De handtekening voor gedeelde toegang is het SAS-token van de Azure Stack-portal, zonder dat de voorloopspaties '?' in de tekenreeks. Als u de functie kopiëren vanuit de portal gebruikt, moet u de voorloopspaties verwijderen '?' voor het token voor het werken in SQL Server.

    Eenmaal hebt u de back-updoel instellen en in SQL Server is geconfigureerd, klikt u vervolgens een back-up naar de blobopslag van Azure Stack.

## <a name="next-steps"></a>Volgende stappen

[Met behulp van services of het bouwen van apps voor Azure Stack](azure-stack-considerations.md)