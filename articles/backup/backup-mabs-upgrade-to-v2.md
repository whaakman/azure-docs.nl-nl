---
title: Azure Backup Server v2 installeren
description: Azure Backup Server v2 biedt u verbeterde back-mogelijkheden voor het beveiligen van virtuele machines, bestanden en mappen en werkbelastingen. Informatie over het installeren of upgraden naar Azure Backup Server v2.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: adigan
ms.openlocfilehash: fdf69003566f704354a17335b1f46fc3077aedbc
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38598387"
---
# <a name="install-azure-backup-server-v2"></a>Azure Backup Server v2 installeren

Azure Backup Server beschermt uw virtuele machines (VM's), werkbelastingen, bestanden en mappen en meer. Azure Backup Server v2 is gebaseerd op Azure Backup Server v1, en biedt u de nieuwe functies die niet beschikbaar in v1. Zie voor een vergelijking van functies tussen v1 en v2 [beveiligingsmatrix voor Azure Backup Server](backup-mabs-protection-matrix.md). 

De aanvullende functies in de back-upserver van v2 zijn een upgrade van de back-upserver van v1. Back-upserver van v1 is echter niet een vereiste voor het installeren van de back-upserver van v2. Als u wilt upgraden van back-upserver van v1 naar back-upserver van v2, installeert u de back-upserver van v2 op de Server van de back-up protection-server. Uw bestaande back-up-serverinstellingen blijven intact.

U kunt back-upserver van v2 installeren op Windows Server 2012 R2 of Windows Server 2016. Als u wilt profiteren van nieuwe functies zoals System Center 2016 Data Protection Manager Modern Backup Storage, moet u back-upserver van v2 installeren op Windows Server 2016. Voordat u een upgrade naar uitvoert of back-upserver van v2 installeren, meer informatie over de [installatievereisten](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).

> [!NOTE]
> Azure Backup-Server heeft de dezelfde codebasis als System Center Data Protection Manager. Back-up Server v1 is gelijk aan Data Protection Manager 2012 R2 en back-upserver van v2 is gelijk aan Data Protection Manager 2016. In dit artikel wordt soms verwezen naar de Data Protection Manager-documentatie.
>
>

## <a name="upgrade-backup-server-to-v2"></a>Backup Server upgraden naar v2
Upgrade uitvoeren van back-upserver van v1 naar back-upserver van v2, zorg ervoor dat de installatie van heeft de vereiste updates:

- [Bijwerken van de beveiligingsagents](backup-mabs-upgrade-to-v2.md#update-the-data-protection-manager-protection-agent) op de beveiligde servers.
- Windows Server 2012 R2 upgraden naar WindowsServer 2016.
- Azure Backup Server Administrator op afstand bijwerken op alle productieservers.
- Zorg ervoor dat de back-ups zijn ingesteld om door te gaan zonder de productieserver opnieuw te starten.


### <a name="upgrade-steps-for-backup-server-v2"></a>Upgradestappen voor back-upserver van v2

1. In het Downloadcentrum [download het installatieprogramma van de upgrade](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Nadat u de wizard setup hebt uitgepakt, zorg ervoor dat **setup.exe uitvoeren** is geselecteerd en selecteer vervolgens **voltooien**.

  ![Installatieprogramma van de Setup - setup uitvoeren](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. In de wizard Microsoft Azure Backup Server onder **installeren**, selecteer **Microsoft Azure Backup Server**.

   ![Setup-installatieprogramma - Kies installeren](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. Op de **Welkom** pagina, de waarschuwingen bekijken en selecteer vervolgens **volgende**.

   ![Setup-installatieprogramma - welkomstpagina](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. De wizard setup voert controles om ervoor te zorgen dat uw omgeving kunt upgraden. Op de **Vereistencontroles** weergeeft, schakelt **controleren**.

   ![Installer - pagina Vereistencontroles instellen](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. Uw omgeving moet slagen voor de vereiste controles. Als uw omgeving niet de controles doorgeeft, houd er rekening mee de problemen en corrigeren. Selecteer **nogmaals controleren**. Nadat u de vereiste controles worden gehaald, selecteert u **volgende**.

  ![Setup-installatieprogramma - opnieuw controleren](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. Op de **SQL-instellingen** pagina, selecteer de relevante optie voor uw SQL-installatie en selecteer vervolgens **controleren en installeren**.

   ![Setup-installatieprogramma - pagina van de SQL-instellingen](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  De controles kunnen enkele minuten duren. Wanneer de controles zijn voltooid, selecteert u **volgende**.

   ![Installatieprogramma van de Setup - SQL-instellingen controleren en installatie-knop](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and-fix-settings.png)

8. Op de **installatie-instellingen** pagina, het aanbrengen van wijzigingen naar de locatie waar de back-up-Server is geïnstalleerd, of naar de Scratch-locatie. Selecteer **Volgende**.

  ![Setup-installatieprogramma - pagina installatie-instellingen](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. Selecteer voor het voltooien van de wizard setup **voltooien**.

  ![Installatieprogramma van de Setup - voltooien](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a>Opslag toevoegen voor Modern Backup Storage

Als u wilt de back-upopslag-efficiëntie verbeteren, back-upserver v2 voegt ondersteuning toe voor volumes. Back-upserver van v2 biedt ondersteuning voor schijven, zoals back-upserver van v1.

### <a name="add-volumes-and-disks"></a>Volumes en schijven toevoegen
Als u back-upserver v2 op Windows Server 2016 uitvoert, kunt u volumes gebruiken voor het opslaan van back-upgegevens. Volumes bieden opslagprestaties en snellere back-ups. Omdat de volumes niet bekend bent met back-up-Server, moet u ze toevoegen. 

Wanneer u een volume met back-up-Server toevoegt, kunt u het volume een beschrijvende naam geven. Klik op de **beschrijvende naam** kolom van het volume dat u wilt een naam. U kunt de naam van de later, indien nodig wijzigen. U kunt ook PowerShell gebruiken toevoegen of wijzigen van de beschrijvende namen voor de volumes.

Een volume toevoegen in de Administrator-Console:

1. Selecteer in de Azure Backup Server Administrator-Console **Management** > **schijfopslag** > **toevoegen**.

    ![Open de wizard schijfopslag toevoegen](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    Hiermee opent u de wizard schijfopslag toevoegen.

2. Op de **schijfopslag toevoegen** pagina, in de **beschikbare volumes** vak, selecteert u een volume en selecteer vervolgens **toevoegen**.
3. In de **volumes geselecteerd** vak, voer een beschrijvende naam voor het volume en selecteer vervolgens **OK**.

      ![Wizard schijfopslag toevoegen - volume toevoegen](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Als u toevoegen van een schijf wilt, wordt de schijf moet behoren tot een beveiligingsgroep met verouderde opslag. Deze schijven kunnen alleen worden gebruikt voor deze beveiligingsgroepen. Als back-up-Server geen bronnen met verouderde beveiliging heeft, wordt de schijf wordt niet weergegeven.

  Zie voor meer informatie over het toevoegen van schijven [schijven om te verhogen verouderde opslag toe te voegen](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). U kunt een schijf een beschrijvende naam geven.


### <a name="assign-workloads-to-volumes"></a>Workloads toewijzen aan volumes

In de Backup-Server, kunt u opgeven welke werkbelastingen zijn toegewezen aan bepaalde volumes. U kunt bijvoorbeeld dure volumes die ondersteuning bieden voor een groot aantal i/o-bewerkingen per seconde (IOP's) voor het opslaan van alleen de workloads waarvoor frequente, hoge volumes back-ups instellen. Een voorbeeld is SQL Server met transactielogboeken.

#### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

Gebruik de PowerShell-cmdlet Update-DPMDiskStorage voor het bijwerken van de eigenschappen van een volume in de opslaggroep in de back-up-Server.

Syntaxis:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

Alle wijzigingen die u maakt met behulp van PowerShell zijn doorgevoerd in de gebruikersinterface.


## <a name="protect-data-sources"></a>Gegevensbronnen beveiligen
Als u wilt die gegevensbronnen beveiligt, moet u een beveiligingsgroep maakt. De volgende stappen uit markeert wijzigingen in of toevoegingen aan de wizard nieuwe beveiligingsgroep.

Een beveiligingsgroep maken:

1. Selecteer in de back-up Server Administrator-Console **Protection**.

2. Selecteer op het lint **nieuw**.

    Hiermee opent u de wizard nieuwe beveiligingsgroep maken.

  ![Wizard nieuwe beveiligingsgroep maken](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. Op de pagina **Welkom** selecteert u **Volgende**.
4. Op de **Type beveiligingsgroep selecteren** pagina, selecteert u het type van de beveiligingsgroep die u wilt maken, en selecteer vervolgens **volgende**.

  ![De pagina Type beveiligingsgroep selecteren](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. Op de **groepsleden selecteren** pagina, in de **beschikbare leden** deelvenster, de leden met protection agents worden weergegeven. In dit voorbeeld selecteert u volume D:\ en E:\ en deze toevoegen aan de **geselecteerde leden** deelvenster. Selecteer **Volgende**.

  ![Pagina voor leden van groep selecteren](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. Op de **methode voor gegevensbeveiliging selecteren** pagina een **beveiligingsgroepnaam**, selecteer de methode voor gegevensbeveiliging en selecteer vervolgens **volgende**. Als u beveiliging op korte termijn wilt, moet u de **schijf** back-up van methode.

  ![De pagina methode voor gegevensbeveiliging selecteren](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. Op de **Kortetermijndoelen opgeven** pagina, selecteert u de details voor **bewaartermijn** en **Synchronisatiefrequentie**. Selecteer vervolgens **Volgende**. Selecteer optioneel, als u wilt wijzigen van de planning voor wanneer herstelpunten zijn gemaakt, **wijzigen**.

  ![Pagina Kortetermijndoelen opgeven](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. Op de **toewijzing van schijfopslag controleren** pagina controleren om details over de gegevensbronnen die u hebt geselecteerd, de grootte en waarden voor de ruimte die moet worden ingericht en het doelvolume voor opslag.

  ![Pagina van de toewijzing van schijfopslag controleren](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Opslagvolumes die zijn gebaseerd op de workloadvolumes (ingesteld met behulp van PowerShell) en de beschikbare opslag. U kunt de opslagvolumes wijzigen door het selecteren van andere volumes in de vervolgkeuzelijst. Als u de waarde voor **Doelopslag**, de waarde voor **beschikbare schijfopslag** dynamisch gewijzigd in overeenstemming met waarden onder **vrije ruimte** en  **Te weinig ingerichte ruimte**.

  Als de gegevensbronnen groeien zoals verwacht, de waarde voor de **te weinig ingerichte ruimte** kolom in **beschikbare schijfopslag** weergegeven hoeveel extra opslag die nodig is. Gebruik deze waarde om u te helpen uw opslagbehoeften voor goede back-ups plannen. Als de waarde nul is, zijn er geen problemen met de opslag in de nabije toekomst. Als de waarde een ander getal dan nul is, u hoeft niet voldoende opslag toegewezen (op basis van het beveiligingsbeleid en de gegevensgrootte van uw beveiligde leden).

  ![Schijfopslag](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   Voltooi de wizard voor het voltooien van het maken van de beveiligingsgroep.

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Verouderde opslag migreren naar Modern Backup Storage
Na de upgrade of installatie van back-upserver van v2 en het besturingssysteem upgraden naar Windows Server 2016, werken uw beveiligingsgroepen voor het gebruik van Modern Backup Storage. Beveiligingsgroepen worden standaard niet gewijzigd. Ze blijven werken als ze in eerste instantie zijn ingesteld. 

Bijwerken van beveiligingsgroepen voor het gebruik van Modern Backup Storage is optioneel. Voor het bijwerken van de beveiligingsgroep, stop de beveiliging van alle gegevensbronnen met behulp van de optie voor het behouden van gegevens. De gegevensbronnen vervolgens toevoegen aan een nieuwe beveiligingsgroep.

1. Selecteer in de Administrator-Console de **Protection** functie. In de **Beveiligingsgroepslid** lijst, met de rechtermuisknop op het lid en selecteer vervolgens **beveiliging van lid stoppen**.

  ![Beveiliging van lid stoppen](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. In de **verwijderen uit groep** dialoogvenster vak, controleert u de gebruikte schijfruimte en de beschikbare vrije ruimte voor de opslaggroep. De standaardwaarde is te laat u de herstelpunten op de schijf en ze verlopen volgens het bijbehorende bewaarbeleid. Klik op **OK**.

  Als u de gebruikte schijfruimte onmiddellijk geretourneerd aan de vrije opslaggroep wilt, selecteert u de **replica op schijf verwijderen** selectievakje in om de back-upgegevens (en herstelpunten) te verwijderen die zijn gekoppeld aan dit lid.

  ![Dialoogvenster groep verwijderen](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Maak een beveiligingsgroep die gebruikmaakt van Modern Backup Storage. De niet-beveiligde gegevensbronnen opnemen.


## <a name="add-disks-to-increase-legacy-storage"></a>Schijven om te verhogen verouderde opslag toevoegen

Als u gebruiken van verouderde opslag met Backup Server wilt, moet u mogelijk schijven om te verhogen verouderde opslag toevoegen. 

Schijfopslag toevoegen:

1. Selecteer in de Administrator-Console **Management** > **schijfopslag** > **toevoegen**.

    ![Dialoogvenster schijfopslag toevoegen](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. In de **schijfopslag toevoegen** dialoogvenster, selecteer **schijven toevoegen**.

5. Selecteer de schijven die u wilt toevoegen, selecteert u in de lijst met beschikbare schijven, **toevoegen**, en selecteer vervolgens **OK**.

## <a name="update-the-data-protection-manager-protection-agent"></a>De Data Protection Manager protection agent bijwerken

Back-upserver maakt gebruik van de System Center Data Protection Manager protection agent voor updates. Als u een van een beveiligingsagent die niet is verbonden met het netwerk upgrade, kunt u de Data Protection Manager Administrator-Console niet gebruiken om de upgrade van een verbonden agent te voltooien. U moet een upgrade uitvoeren van de beveiligingsagent in een niet-actieve domeinomgeving. Tot de clientcomputer is verbonden met het netwerk, ziet u de Data Protection Manager Administrator-Console dat de update van de beveiligingsagent in behandeling is.

De volgende secties wordt beschreven hoe u bijwerken van beveiligingsagents voor clientcomputers die zijn verbonden en client-computers die niet zijn verbonden.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Een beveiligingsagent voor een verbonden clientcomputer bijwerken

1. Selecteer in de back-up Server Administrator-Console **Management** > **Agents**.

2. Selecteer in het weergavepaneel de clientcomputers waarvoor u wilt bijwerken van de beveiligingsagent.

  > [!NOTE]
  > De **agentupdates** kolom geeft aan wanneer een update van de beveiligingsagent beschikbaar is voor elke beveiligde computer. In de **acties** in het deelvenster de **Update** actie is alleen beschikbaar wanneer een beveiligde computer is geselecteerd en er updates beschikbaar zijn.
  >
  >

3. Voor het installeren van de bijgewerkte beveiligingsagents op de geselecteerde computers in de **acties** venster **Update**.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Bijwerken van een beveiligingsagent op een clientcomputer die niet is verbonden

1. Selecteer in de back-up Server Administrator-Console **Management** > **Agents**.

2. Selecteer in het weergavepaneel de clientcomputers waarvoor u wilt bijwerken van de beveiligingsagent.

  > [!NOTE]
   > De **agentupdates** kolom geeft aan wanneer een update van de beveiligingsagent beschikbaar is voor elke beveiligde computer. In de **acties** in het deelvenster de **Update** actie is niet beschikbaar als u een beveiligde computer hebt geselecteerd, tenzij er updates beschikbaar zijn.
  >
  >

3. Bijgewerkt om beveiligingsagents te installeren op de geselecteerde computers, selecteert u **Update**.

4. Voor een clientcomputer die niet is verbonden met het netwerk, totdat de computer is verbonden met het netwerk, de **agentstatus** kolom ziet u de status van **Update in behandeling**.

  Nadat een clientcomputer is verbonden met het netwerk, de **agentupdates** kolom voor de clientcomputer wordt de status van **bijwerken**.
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-the-new-version-with-azure"></a>Verouderde beveiligingsgroepen verplaatsen van de oude versie en synchroniseren van de nieuwe versie met Azure

Als beide Azure Backup Server en het besturingssysteem zijn bijgewerkt, bent u klaar om nieuwe gegevensbronnen met behulp van Modern Backup Storage te beveiligen. Blijft echter al beveiligde gegevensbronnen worden beveiligd in de oude manier zoals ze in Azure Backup Server maar alle nieuwe bescherming waren Modern Backup Storage gebruikt.

Onderstaande stappen zijn voor het migreren van gegevensbronnen van legacy-modus van de beveiliging naar Modern backup storage.

• De nieuwe volumes toevoegen aan de DPM-opslaggroep en beschrijvende namen en bron-labels toewijzen als gewenst.
• Voor elke gegevensbron die zich in de legacy-modus, stop de beveiliging van de gegevensbronnen en 'Beveiligde gegevens behouden'.  Hierdoor kunnen herstellen van de oude herstelpunten na de migratie.

• Maken van een nieuwe pagina en selecteer de gegevensbronnen die moeten worden opgeslagen met behulp van de nieuwe indeling.
• DPM wordt een kopie van de replica van de oudere back-upopslag doen in het volume van Modern Backup Storage lokaal.
Opmerking: Deze wordt gezien als een bewerking na herstel taak • vervolgens alle nieuwe synchronisatie en herstelpunten worden opgeslagen in Modern Backup Storage.
• Oude herstelpunten worden verwijderd uit als ze verlopen en uiteindelijk de schijfruimte vrij.
• Als de oude volumes zijn verwijderd uit de oude opslag, de schijf kan worden verwijderd uit Azure back-up en het systeem.
• Maak een back-up van de Azure-DPMDB.

Deel 2:-belangrijke items > de nieuwe server moet dezelfde naam als de oorspronkelijke Azure Backup-server. U kunt de naam van de nieuwe Azure Backup-server niet wijzigen als u wilt dat oude opslaggroep en DPMDB gebruiken om te behouden herstelpunten - moet hebben, is de back-up van DPMDB als moet worden hersteld

1) Afsluiten van de oorspronkelijke Azure back-upserver of probeer het uit de kabel.
2) Opnieuw instellen van het computeraccount in active directory.
3) Server 2016 installeren op nieuwe computer en noem het de dezelfde computernaam als de oorspronkelijke Azure Backup-server.
4) Lid worden van het domein
5) Azure Backup server V2 installeren (verplaatsen DPM-opslaggroepschijven uit oude server en importeer)
6) Herstel de DPMDB afkomstig uit het einde van deel 2
7) De opslag van de oorspronkelijke back-upserver koppelen aan de nieuwe server.
8) Herstel de DPMDB van SQL
9) Installeren vanaf de opdrachtregel admin op cd van de nieuwe server met Microsoft Azure Backup locatie en de bin-map

Voorbeeld van pad: C:\windows\system32 > cd ' c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\
naar Azure back-up Voer DPMSYNC-SYNC

10) Voer DPMSYNC-SYNC-Opmerking Als u nieuwe schijven hebt toegevoegd aan de DPM-opslaggroep in plaats van verplaatsen van de oude versie, voer DPMSYNC - reallocatereplica uit

## <a name="new-powershell-cmdlets-in-v2"></a>Nieuwe PowerShell-cmdlets in v2

Wanneer u Azure Backup Server v2 installeert, zijn twee nieuwe cmdlets beschikbaar: 
* [Mount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Dismount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Volgende stappen

Informatie over het voorbereiden van de server of beginnen met een werkbelasting beveiligen:
- [Back-up-Server-workloads voorbereiden](backup-azure-microsoft-azure-backup.md)
- [Backup Server gebruiken voor back-up van een VMware-server](backup-azure-backup-server-vmware.md)
- [Backup Server gebruiken voor back-up van SQL Server](backup-azure-sql-mabs.md)
- [Modern Backup Storage gebruiken met back-upserver](backup-mabs-add-storage.md)

