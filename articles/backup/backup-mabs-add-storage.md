---
title: Modern Backup Storage gebruiken met Azure Backup Server
description: Meer informatie over de nieuwe functies in Azure Backup Server. In dit artikel wordt beschreven hoe u uw back-up-Server-installatie een upgrade uitvoert.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: markgal; adigan; kasinh
ms.openlocfilehash: daa7d6ee13cf55703b71bea321e65d2518a59979
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578515"
---
# <a name="add-storage-to-azure-backup-server"></a>Opslag toevoegen aan Azure Backup Server

Azure Backup Server V2 en later wordt geleverd met System Center 2016 Data Protection Manager Modern Backup Storage. Modern Backup Storage biedt besparing in opslag van 50 procent, back-ups die drie keer sneller en efficiënter opslag. Het biedt ook werkbelastingbewuste opslag.

> [!NOTE]
> Voor het gebruik van Modern Backup Storage, moet u back-up Server V2 of V3 uitvoeren op Windows Server 2016 of V3 op Windows Server 2019.
> Als u back-up Server V2 op een eerdere versie van Windows Server uitvoert, kan niet Azure Backup Server profiteren van Modern Backup Storage. In plaats daarvan beveiligt het werkbelastingen als met back-up Server V1. Zie voor meer informatie, de versie van de back-upserver [beveiligingsmatrix](backup-mabs-protection-matrix.md).

## <a name="volumes-in-backup-server"></a>Volumes in back-upserver

Back-up-Server versie 2 of later accepteert opslagvolumes. Wanneer u een volume toevoegt, maakt back-up-Server op het volume Resilient File System (ReFS), waarvoor Modern Backup Storage is vereist. Een volume toevoegen, en vouwt u het later opnieuw als u wilt, het is raadzaam dat u deze werkstroom:

1.  Instellen van back-up-Server op een virtuele machine.
2.  Maak een volume op een virtuele schijf in een opslaggroep:
    1.  Een schijf toevoegen aan een opslaggroep en maak een virtuele schijf met een eenvoudige indeling.
    2.  Eventuele extra schijven toevoegen en uitbreiden van de virtuele schijf.
    3.  Maak volumes op de virtuele schijf.
3.  De volumes aan back-up-Server toevoegen.
4.  Werkbelastingbewuste opslag configureren.

## <a name="create-a-volume-for-modern-backup-storage"></a>Maak een volume voor Modern Backup Storage

Met behulp van back-up-Server versie 2 of hoger met volumes als schijfopslag kunt u de controle behoudt over de opslag. Een volume kan één schijf zijn. Echter, als u uitbreiden opslag in de toekomst wilt, een volume van een schijf die is gemaakt met behulp van opslagruimten maken. Dit kan helpen als u wilt uitbreiden van het volume voor back-upopslag. Deze sectie vindt u aanbevolen procedures voor het maken van een volume met deze instellingen.

1. Selecteer in Serverbeheer **File and Storage Services** > **Volumes** > **opslaggroepen**. Onder **fysieke schijven**, selecteer **nieuwe opslaggroep**.

    ![Een nieuwe opslaggroep maken](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. In de **taken** vervolgkeuzelijst, selecteer **nieuwe virtuele schijf**.

    ![Een virtuele schijf toevoegen](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Selecteer de opslaggroep en selecteer vervolgens **fysieke schijf toevoegen**.

    ![Een fysieke schijf toevoegen](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Selecteer de fysieke schijf en selecteer vervolgens **virtuele schijf uitbreiden**.

    ![De virtuele schijf uitbreiden](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Selecteer de virtuele schijf en selecteer vervolgens **NieuwVolume**.

    ![Maak een nieuw volume](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. In de **Selecteer de server en schijf** dialoogvenster, selecteer de server en de nieuwe schijf. Selecteer vervolgens **Volgende**.

    ![Selecteer de server en schijf](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a>Volumes aan back-upserver van schijfopslag toevoegen

Een volume toevoegen aan back-up-Server, in de **Management** deelvenster opnieuw scannen van de opslag, en selecteer vervolgens **toevoegen**. Er wordt een lijst weergegeven van alle volumes die beschikbaar zijn voor de opslag van back-up-Server worden toegevoegd. Nadat beschikbare volumes zijn toegevoegd aan de lijst met geselecteerde volumes, kunt u ze een beschrijvende naam voor het beheren van deze geven. Als u wilt deze volumes de ReFS-indeling, zodat back-upserver van de voordelen van Modern Backup Storage kunnen gebruiken, selecteert u **OK**.

![Beschikbare Volumes toevoegen](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>Werkbelastingbewuste opslag instellen

Met werkbelastingbewuste opslag kunt u de volumes die bij voorkeur bepaalde soorten werkbelastingen opslaat. U kunt bijvoorbeeld dure volumes die ondersteuning bieden voor een groot aantal i/o-bewerkingen per seconde (IOP's) voor het opslaan van alleen de workloads waarvoor frequente, hoge volumes back-ups instellen. Een voorbeeld is SQL Server met transactielogboeken. Andere werkbelastingen zijn back-ups van minder vaak, zoals virtuele machines, kunnen worden back-ups op goedkope volumes.

### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

U kunt de werkbelastingbewuste opslag instellen met behulp van de PowerShell-cmdlet Update-DPMDiskStorage, waarbij de eigenschappen van een volume in de opslaggroep op een Data Protection Manager-server.

Syntaxis:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
De volgende schermafbeelding ziet u de cmdlet Update-DPMDiskStorage in het PowerShell-venster.

![Met de opdracht Update-DPMDiskStorage in het PowerShell-venster](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

De wijzigingen die u aanbrengt met behulp van PowerShell zijn doorgevoerd in de back-up Server Administrator-Console.

![Schijven en volumes in de Administrator-Console](./media/backup-mabs-add-storage/mabs-add-storage-9.png)


## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Verouderde opslag migreren naar Modern Backup Storage
Na de upgrade of installatie V2 voor back-up-Server en het besturingssysteem upgraden naar Windows Server 2016, werken uw beveiligingsgroepen voor het gebruik van Modern Backup Storage. Beveiligingsgroepen worden standaard niet gewijzigd. Ze blijven werken als ze in eerste instantie zijn ingesteld.

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

## <a name="next-steps"></a>Volgende stappen
Nadat u de back-up-Server hebt geïnstalleerd, informatie over het voorbereiden van uw server, of gaat een werkbelasting beveiligen.

- [Back-up-Server-workloads voorbereiden](backup-azure-microsoft-azure-backup.md)
- [Backup Server gebruiken voor back-up van een VMware-server](backup-azure-backup-server-vmware.md)
- [Backup Server gebruiken voor back-up van SQL Server](backup-azure-sql-mabs.md)
