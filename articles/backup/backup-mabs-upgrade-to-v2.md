---
title: Azure Backup-Server v2 installeren | Microsoft Docs
description: Azure back-upserver v2 biedt verbeterde back-mogelijkheden voor het beveiligen van virtuele machines, bestanden en mappen en werkbelastingen. Informatie over het installeren of upgraden van v2 voor Azure Backup-Server.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: 1bbb16afef7940933b4c3ae23873f212770137e0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="install-azure-backup-server-v2"></a>V2 voor Azure Backup-Server installeren

Azure Backup-Server beveiligt uw virtuele machines (VM's), werkbelastingen, bestanden en mappen en meer. Azure back-upserver van v2 bouwt voort op Azure Backup-Server v1- en biedt u de nieuwe functies die niet beschikbaar in v1. Zie voor een vergelijking van functies tussen v1- en v2 [Azure Backup-Server protection matrix](backup-mabs-protection-matrix.md). 

De aanvullende functies in v2 back-upserver van zijn een upgrade van de back-upserver van v1. Back-upserver van v1 is echter niet vereist voor de installatie van de back-upserver van v2. Als u wilt upgraden van back-upserver van v1 tot back-upserver van v2, moet u back-upserver van v2 installeren op de back-upserver protection-server. Uw bestaande back-upserver instellingen blijven behouden.

U kunt back-upserver van v2 installeren op Windows Server 2012 R2 of Windows Server 2016. Om te profiteren van nieuwe functies zoals System Center 2016 Data Protection Manager moderne back-up opslag, moet u back-upserver van v2 installeren op Windows Server 2016. Voordat u een upgrade naar uitvoert of back-upserver van v2 installeert, leest u over de [installatievereisten](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).

> [!NOTE]
> Azure Backup-Server heeft dezelfde codebasis als System Center Data Protection Manager. Back-Server v1 is gelijk aan Data Protection Manager 2012 R2 en back-upserver van v2 komt overeen met Data Protection Manager 2016. In dit artikel wordt soms verwezen naar de Data Protection Manager-documentatie.
>
>

## <a name="upgrade-backup-server-to-v2"></a>Upgrade van de back-upserver voor v2
Upgrade uitvoeren van back-upserver van v1 naar back-upserver van v2, zorg ervoor dat de installatie heeft de vereiste updates:

- [De beveiligingsagents bijwerken](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) op de beveiligde servers.
- Windows Server 2012 R2 upgraden naar WindowsServer 2016.
- Azure Backup-serverbeheerder voor extern bijwerken op alle productieservers.
- Zorg ervoor dat de back-ups zijn ingesteld om door te gaan zonder de productieserver opnieuw te starten.


### <a name="upgrade-steps-for-backup-server-v2"></a>Stappen voor de upgrade voor back-upserver van v2

1. In het Downloadcentrum [het upgrade-installatieprogramma downloaden](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Nadat u de wizard setup hebt uitgepakt, zorg ervoor dat **setup.exe uitvoeren** is geselecteerd en selecteer vervolgens **voltooien**.

  ![Installatieprogramma van de Setup - setup uitvoeren](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. In de wizard Microsoft Azure Backup-Server onder **installeren**, selecteer **Microsoft Azure Backup-Server**.

  ![Installatieprogramma van de Setup - Selecteer installeren](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. Op de **Welkom** pagina en selecteer vervolgens de waarschuwingen bekijken **volgende**.

  ![Installatieprogramma van de Setup - welkomstpagina](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. De wizard setup voert controles op om ervoor te zorgen dat uw omgeving een upgrade kunt uitvoeren. Op de **vereiste controleert** pagina **controleren**.

  ![Installatieprogramma van de Setup - vereisten controleert pagina](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. Uw omgeving moet voldoen aan de vereiste controles. Als uw omgeving, niet de controles slaagt, houd rekening met het en corrigeer deze. Selecteer **opnieuw controleren**. Nadat u de vereiste controles doorgeven, selecteert u **volgende**.

  ![Installatieprogramma van de Setup - opnieuw controleren](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. Op de **SQL-instellingen** pagina en selecteer vervolgens de relevante optie voor uw SQL-installatie **controleren en installeren**.

  ![Installatieprogramma van de Setup - pagina van de SQL-instellingen](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  De controles kunnen enkele minuten duren. Wanneer de controles zijn voltooid, selecteert u **volgende**.

  ![Installatieprogramma van de Setup - SQL-instellingen controleren en knop installeren](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. Op de **installatie-instellingen** pagina, het aanbrengen van wijzigingen naar de locatie waar back-up-Server is geïnstalleerd, of naar de scratchruimte locatie. Selecteer **volgende**.

  ![Installatieprogramma van de Setup - pagina installatie-instellingen](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. Selecteer voor het voltooien van de wizard setup **voltooien**.

  ![Installatieprogramma van de Setup - voltooien](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a>Opslag toevoegen voor moderne back-up-opslag

Ter verbetering van de back-upopslag efficiëntie back-upserver v2 voegt ondersteuning toe voor volumes. Zoals back-upserver van v1 ondersteunt v2 back-upserver schijven.

### <a name="add-volumes-and-disks"></a>Volumes en schijven toevoegen
Als u back-upserver v2 op Windows Server 2016 uitvoert, kunt u volumes gebruiken voor het opslaan van back-upgegevens. Volumes bieden opslagbesparing en snellere back-ups. Omdat de volumes zijn toegevoegd aan back-up-Server, moet u ze toevoegen. 

Wanneer u een volume met back-up-Server toevoegt, kunt u het volume een beschrijvende naam geven. Klik op de **beschrijvende naam** kolom van het volume dat u de naam wilt wijzigen. U kunt de naam van de later, indien nodig wijzigen. Ook kunt u PowerShell toevoegen of wijzigen van de beschrijvende namen voor volumes.

Toevoegen van een volume in de Administrator-Console:

1. Selecteer in de Azure Backup Server Administrator-Console **Management** > **schijfopslag** > **toevoegen**.

    ![Open de wizard schijfopslag toevoegen](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    Hiermee opent u de wizard schijfopslag toevoegen.

2. Op de **schijfopslag toevoegen** pagina in de **beschikbare volumes** vak en selecteer vervolgens selecteert u een volume **toevoegen**.
3. In de **volumes geselecteerd** vak en selecteer vervolgens een beschrijvende naam voor het volume **OK**.

      ![Schijfopslag wizard Add - volume toevoegen](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Als u wilt een schijf toevoegen, wordt de schijf moet behoren tot een beveiligingsgroep met de oude opslag. Deze schijven kunnen alleen worden gebruikt voor deze beveiligingsgroepen. Als back-upserver geen bronnen die oudere beveiliging, de schijf niet weergegeven.

  Zie voor meer informatie over het toevoegen van schijven [schijven voor een verhoging van de oude opslag toe te voegen](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). U kan een schijf een beschrijvende naam geven.


### <a name="assign-workloads-to-volumes"></a>Werkbelastingen toewijzen aan volumes

Back-up-server kunt u opgeven welke werkbelastingen zijn toegewezen aan welke volumes. U kunt bijvoorbeeld dure volumes die ondersteuning bieden voor een groot aantal i/o-bewerkingen per seconde (IOPS) voor het opslaan van alleen werkbelastingen waarvoor frequente, hoog volume back-ups instellen. Een voorbeeld is SQL Server met de transactielogboeken.

#### <a name="update-dpmdiskstorage"></a>Update DPMDiskStorage

Gebruik de PowerShell-cmdlet Update DPMDiskStorage voor het bijwerken van de eigenschappen van een volume in de opslaggroep in de back-upserver.

Syntaxis:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

Alle wijzigingen die u maakt met behulp van PowerShell worden weergegeven in de gebruikersinterface.


## <a name="protect-data-sources"></a>Gegevensbronnen beveiligen
Om te beginnen die gegevensbronnen beveiligt, moet u een beveiligingsgroep maakt. De volgende stappen, markeer de wizard nieuwe beveiligingsgroep toegevoegd of gewijzigd.

Maak een beveiligingsgroep:

1. Selecteer in de beheerdersconsole van back-up Server **beveiliging**.

2. Selecteer op het lint **nieuw**.

    Hiermee opent u de wizard nieuwe beveiligingsgroep maken.

  ![Wizard nieuwe beveiligingsgroep maken](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. Op de **Welkom** pagina **volgende**.
4. Op de **Type beveiligingsgroep selecteren** pagina, selecteer het type van de beveiligingsgroep die u wilt maken en selecteer vervolgens **volgende**.

  ![De pagina Type beveiligingsgroep selecteren](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. Op de **groepsleden selecteren** pagina in de **beschikbare leden** deelvenster, de leden met protection agents worden weergegeven. Bijvoorbeeld, selecteert u volume D:\ en E:\ en deze toevoegen aan de **leden geselecteerd** deelvenster. Selecteer **volgende**.

  ![De pagina leden groep selecteren](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. Op de **methode voor gegevensbeveiliging selecteren** pagina, voert u een **naam beveiligingsgroep**, selecteer de methode voor gegevensbeveiliging en selecteer vervolgens **volgende**. Als u beveiliging op korte termijn wilt, moet u de **schijf** back-up van methode.

  ![De pagina methode voor gegevensbeveiliging selecteren](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. Op de **Kortetermijndoelen opgeven** pagina, selecteert u de details voor **bewaartermijn** en **Synchronisatiefrequentie**. Selecteer **volgende**. Selecteer desgewenst voor het wijzigen van de planning voor wanneer de herstelpunten zijn gemaakt, **wijzigen**.

  ![Pagina Kortetermijndoelen opgeven](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. Op de **toewijzing van schijfopslag controleren** pagina controleren om details over de gegevensbronnen die u hebt geselecteerd, de grootte en waarden voor de ruimte die moet worden ingericht en het opslagvolume voor het doel.

  ![Pagina van de toewijzing van schijfopslag controleren](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Opslagvolumes zijn gebaseerd op de werkbelasting volume-toewijzing (ingesteld met behulp van PowerShell) en de beschikbare opslag. U kunt de opslagvolumes wijzigen door het selecteren van andere volumes in de vervolgkeuzelijst. Als u de waarde voor wijzigt **Doelopslag**, de waarde voor **beschikbaar schijfopslag** dynamisch wordt aangepast aan de waarden onder **vrije ruimte** en  **Ruimte underprovisioned**.

  Als de gegevensbronnen groeien zolang er gepland, de waarde voor de **Underprovisioned ruimte** kolom in **beschikbaar schijfopslag** geeft de hoeveelheid opslagruimte die nodig is. Deze waarde gebruiken om u te helpen uw opslagbehoeften voor goede back-ups plannen. Als de waarde nul is, moet u er geen mogelijke problemen met opslag in de nabije toekomst zijn. Als de waarde een getal dan nul is, u hoeft niet voldoende opslag die is toegewezen (op basis van uw beveiligingsbeleid en de gegevensgrootte van uw beveiligde leden).

  ![Onderbezette schijfopslag](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   Voltooi de wizard voor het voltooien van uw beveiligingsgroep te maken.

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Verouderde opslag migreren naar een moderne back-up-opslag
Na de upgrade of installatie van back-upserver van v2 en het besturingssysteem upgraden naar Windows Server 2016, werken uw beveiligingsgroepen voor het gebruik van moderne back-up-opslag. Standaard zijn beveiligingsgroepen niet gewijzigd. Ze blijven functioneren als ze in eerste instantie zijn ingesteld. 

Bijwerken van de beveiligingsgroepen voor het gebruik van opslag voor moderne back-up is optioneel. Stop de beveiliging van alle gegevensbronnen met de optie van de gegevens behouden voor het bijwerken van de beveiligingsgroep. De gegevensbronnen vervolgens toevoegen aan een nieuwe beveiligingsgroep.

1. Selecteer in de Administrator-Console de **beveiliging** functie. In de **Beveiligingsgroepslid** lijst, met de rechtermuisknop op het lid en selecteer vervolgens **beveiliging van lid stoppen**.

  ![Beveiliging van lid stoppen](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. In de **verwijderen uit groep** dialoogvenster Controleer de gebruikte schijfruimte en de beschikbare vrije ruimte voor de opslaggroep. De standaardwaarde is te laat u de herstelpunten op de schijf en kunnen ze verlopen per hun bijbehorende bewaarbeleid. Klik op **OK**.

  Als u retourneren onmiddellijk de gebruikte schijfruimte aan de beschikbare opslaggroep wilt, selecteer de **replica op schijf verwijderen** selectievakje in om de back-upgegevens (en herstelpunten) te verwijderen die zijn gekoppeld aan dit lid.

  ![Dialoogvenster groep verwijderen](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Maak een beveiligingsgroep die gebruik maakt van opslag voor moderne back-up. De niet-beveiligde gegevensbronnen bevatten.


## <a name="add-disks-to-increase-legacy-storage"></a>Schijven voor een verhoging van de oude opslag toevoegen

Als u verouderde storage gebruiken met Backup-Server wilt, moet u mogelijk schijven voor een verhoging van de oude opslag toevoegen. 

Schijfopslag toevoegen:

1. Selecteer in de Administrator-Console **Management** > **schijfopslag** > **toevoegen**.

    ![Schijfopslag dialoogvenster toevoegen](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. In de **schijfopslag toevoegen** dialoogvenster Selecteer **schijven toevoegen**.

5. Selecteer de schijven die u wilt toevoegen, selecteert u in de lijst met beschikbare schijven **toevoegen**, en selecteer vervolgens **OK**.

## <a name="update-the-data-protection-manager-protection-agent"></a>Werk de beveiligingsagent van Data Protection Manager

Back-upserver maakt gebruik van de System Center Data Protection Manager protection-agent voor updates. Als u een beveiligingsagent die niet is verbonden met het netwerk upgraden wilt, kunt u de Data Protection Manager Administrator-Console niet gebruiken om de upgrade van een verbonden agent te voltooien. De beveiligingsagent in een niet-actieve domeinomgeving, moet u upgraden. Tot de clientcomputer is verbonden met het netwerk, ziet u de beheerdersconsole van Data Protection Manager update van de beveiligingsagent is in behandeling.

De volgende secties wordt beschreven hoe bijwerken van beveiligingsagents voor clientcomputers die zijn verbonden en clientcomputers die niet zijn verbonden.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Een beveiligingsagent voor een verbonden clientcomputer bijwerken

1. Selecteer in de beheerdersconsole van back-up Server **Management** > **Agents**.

2. Selecteer in het weergavepaneel de clientcomputers waarvoor u wilt de beveiligingsagent bijwerkt.

  > [!NOTE]
  > De **agentupdates** kolom wordt aangegeven wanneer een update van de beveiligingsagent beschikbaar is voor elke beveiligde computer. In de **acties** deelvenster de **Update** actie is alleen beschikbaar wanneer een beveiligde computer is geselecteerd en er updates beschikbaar zijn.
  >
  >

3. Bijgewerkte om beveiligingsagents te installeren op de geselecteerde computers in de **acties** deelvenster **Update**.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Een beveiligingsagent op een clientcomputer die niet verbonden bijwerken

1. Selecteer in de beheerdersconsole van back-up Server **Management** > **Agents**.

2. Selecteer in het weergavepaneel de clientcomputers waarvoor u wilt de beveiligingsagent bijwerkt.

  > [!NOTE]
   > De **agentupdates** kolom wordt aangegeven wanneer een update van de beveiligingsagent beschikbaar is voor elke beveiligde computer. In de **acties** deelvenster de **Update** actie is niet beschikbaar wanneer een beveiligde computer is geselecteerd tenzij er updates beschikbaar zijn.
  >
  >

3. Om bijgewerkte beveiligingsagents op de geselecteerde computers installeert, selecteert u **Update**.

4. Voor een clientcomputer die niet is verbonden met het netwerk, tot de computer is verbonden met het netwerk, de **agentstatus** kolom ziet u de status van **Update in behandeling**.

  Nadat een clientcomputer is verbonden met het netwerk, de **agentupdates** kolom voor de client wordt de status van **Updating**.
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-the-new-version-with-azure"></a>Verouderde beveiligingsgroepen van de oude versie en synchroniseren van de nieuwe versie met Azure

Wanneer zowel Azure Backup-Server en het besturingssysteem worden bijgewerkt, bent u klaar om nieuwe gegevensbronnen moderne back-up-opslag te beveiligen. Blijven echter al beveiligde gegevensbronnen worden beveiligd in de verouderde manier zijn als in Azure Backup-Server maar alle nieuwe bescherming moderne back-up-opslag gebruikt.

Onderstaande stappen zijn gegevensbronnen van de legacy-modus van de beveiliging naar moderne back-upopslag migreren.

• De nieuwe volumes toevoegen aan de DPM-opslaggroep bevinden en beschrijvende namen en data source-labels toewijzen indien gewenst.
• Voor elke gegevensbron die in de legacy-modus, stop de beveiliging van de gegevensbronnen en 'Beveiligde gegevens behouden'.  Hierdoor kan herstel van de oude herstelpunten na de migratie.

• Maken van een nieuwe pagina en selecteer de gegevensbronnen die moeten worden opgeslagen met behulp van de nieuwe indeling.
• DPM wordt een kopie van de replica van de oudere back-upopslag doen in het moderne back-upopslag volume lokaal.
Opmerking: Deze wordt gezien als een na een herstelbewerking wordt uitgevoerd taak • alle nieuwe punten voor synchronisatie en herstel vervolgens worden opgeslagen in moderne back-up-opslag.
• Oude herstelpunten worden verwijderd uit als ze verlopen en uiteindelijk de schijfruimte vrij.
• Als de oude volumes zijn verwijderd uit de oude opslag, de schijf kan worden verwijderd uit Azure back-up en het systeem.
• Maak een back-up van de Azure-DPMDB.

Deel 2:-belangrijke zaken > de nieuwe server moet dezelfde naam als de oorspronkelijke Azure Backup-server. U kunt de naam van de nieuwe Azure Backup-server niet wijzigen als u oude opslaggroep en DPMDB gebruiken wilt voor het bewaren van back-up van DPMDB herstelpunten - moet hebben als moet worden hersteld

1) Afsluiten van de oorspronkelijke Azure back-upserver of weghalen de kabel.
2) De computeraccount in active directory herstellen.
3) Server 2016 op nieuwe computer installeren en noem deze de dezelfde computernaam als de oorspronkelijke Azure Backup-server.
4) Aan domein toevoegen
5) Installeer Azure Backup-server V2 (verplaatsen DPM-opslaggroepschijven van de oude server en importeer)
6) Herstel de DPMDB overgenomen uit het einde van deel 2
7) Koppel de opslag van de oorspronkelijke server back-up naar de nieuwe server.
8) Herstel de DPMDB uit SQL.
9) Installeren vanaf de opdrachtregel beheerder op de nieuwe server cd op Microsoft Azure Backup locatie en de bin-map

Voorbeeld van pad: C:\windows\system32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\
naar Azure back-up Voer DPMSYNC-SYNC

10) Voer DPMSYNC-SYNC-Opmerking Als u nieuwe schijven hebt toegevoegd aan de DPM-opslaggroep in plaats van het verplaatsen van de oude versie Voer DPMSYNC - reallocatereplica uit

## <a name="new-powershell-cmdlets-in-v2"></a>Nieuwe PowerShell-cmdlets in v2

Wanneer u v2 voor Azure Backup-Server installeert, zijn twee nieuwe cmdlets zijn beschikbaar: 
* [Koppelpunt DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Ontkoppeling DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het voorbereiden van uw server of met het beveiligen van een werkbelasting beginnen:
- [Back-upserver van werkbelastingen voorbereiden](backup-azure-microsoft-azure-backup.md)
- [Back-up-Server gebruiken om back-up van een VMware-server](backup-azure-backup-server-vmware.md)
- [Back-up-Server gebruiken om back-up van SQL Server](backup-azure-sql-mabs.md)
- [Moderne back-upopslag met back-upserver gebruiken](backup-mabs-add-storage.md)

