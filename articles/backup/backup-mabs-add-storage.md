---
title: Moderne back-upopslag gebruiken met Azure Backup-Server v2 | Microsoft Docs
description: Meer informatie over de nieuwe functies in v2 voor Azure Backup-Server. In dit artikel wordt beschreven hoe een back-upserver van upgrade.
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
ms.openlocfilehash: 751b9b495fd368dff1f72429707f5f33a0ccb569
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-storage-to-azure-backup-server-v2"></a>Opslag toevoegen aan Azure Backup-Server v2

Azure Backup Server v2 wordt geleverd met System Center 2016 Data Protection Manager moderne back-up opslag. Moderne back-up Storage biedt opslagbesparing van 50 procent van de back-ups die drie keer snellere en efficiëntere opslag. Biedt ook werkbelasting-bewuste opslag. 

> [!NOTE]
> Voor het gebruik van moderne back-up-opslag, moet u back-upserver van v2 uitvoeren op Windows Server 2016. Als u een back-upserver van v2 op een eerdere versie van Windows Server uitvoert, kan niet Azure Backup-Server profiteren van moderne back-up-opslag. In plaats daarvan beveiligt het werkbelastingen zoals met back-upserver van v1. Zie voor meer informatie, de versie van de back-upserver [beveiliging matrix](backup-mabs-protection-matrix.md).

## <a name="volumes-in-backup-server-v2"></a>Volumes in de back-upserver v2

Back-Server v2 accepteert opslagvolumes. Wanneer u een volume toevoegt, indelingen back-upserver van het volume Resilient File System (ReFS), waarvoor moderne back-up-opslag is vereist. Een volume toevoegen en later uitbreiden als u wilt, het is raadzaam dat u deze werkstroom:

1.  Back-upserver van v2 op een virtuele machine instellen.
2.  Maak een volume op een virtuele schijf in een opslaggroep:
    1.  Een schijf toevoegen aan een opslaggroep en een virtuele schijf maken met eenvoudige indeling.
    2.  Eventuele extra schijven toevoegen en uitbreiden van de virtuele schijf.
    3.  Volumes op de virtuele schijf maken.
3.  De volumes toevoegen aan back-upserver.
4.  Werkbelasting-bewuste opslag configureren.

## <a name="create-a-volume-for-modern-backup-storage"></a>Een volume maken voor moderne back-up-opslag

Met behulp van back-upserver van v2 met volumes als schijfopslag kunt u controle over opslag. Een volume kan één schijf zijn. Als u opslag in de toekomst kunt uitbreiden wilt, echter een volume buiten een schijf die is gemaakt met behulp van opslagruimten maken. Dit kan helpen als u wilt uitbreiden van het volume voor back-up. Deze sectie vindt u aanbevolen procedures voor het maken van een volume met deze instellingen.

1. Selecteer in Serverbeheer **File and Storage Services** > **Volumes** > **opslaggroepen**. Onder **fysieke schijven**, selecteer **nieuwe opslaggroep**. 

    ![Een nieuwe opslaggroep maken](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. In de **taken** vervolgkeuzelijst, selecteer **nieuwe virtuele schijf**.

    ![Een virtuele schijf toevoegen](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. De opslaggroep selecteren en selecteer vervolgens **fysieke schijf toevoegen**.

    ![Toevoegen van een fysieke schijf](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Selecteer de fysieke schijf en selecteer vervolgens **virtuele schijf uitbreiden**.

    ![De virtuele schijf uitbreiden](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Selecteer de virtuele schijf en selecteer vervolgens **NieuwVolume**.

    ![Maak een nieuw volume](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. In de **server en schijf selecteren** dialoogvenster, selecteer de server en de nieuwe schijf. Selecteer **volgende**.

    ![De server en schijf selecteren](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a>Volumes toevoegen aan back-upserver van schijfopslag

Een volume toevoegen aan back-up-Server, in de **Management** deelvenster opnieuw scannen van de opslag en selecteer vervolgens **toevoegen**. Een lijst van alle volumes die kunnen worden toegevoegd voor de opslag van de back-up-Server wordt weergegeven. Nadat beschikbare volumes zijn toegevoegd aan de lijst met geselecteerde volumes, kunt u ze een beschrijvende naam voor het beheren van deze geven. Om deze volumes Refs zodat back-upserver van de voordelen van moderne back-up-opslag kunt gebruiken, selecteert u **OK**.

![Toevoegen van de beschikbare Volumes](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>Werkbelasting-bewuste opslag instellen

Met de werkbelasting-bewuste opslag, kunt u de volumes waarop bepaalde soorten belasting bij voorkeur opgeslagen. U kunt bijvoorbeeld instellen dat dure volumes die ondersteuning bieden voor een groot aantal i/o-bewerkingen per seconde (IOPS) voor het opslaan van alleen de werkbelastingen die regelmatig, hoog volume back-ups vereisen. Een voorbeeld is SQL Server met de transactielogboeken. Andere werkbelastingen zijn back-ups minder vaak, zoals virtuele machines, kunnen een back-up naar goedkope volumes.

### <a name="update-dpmdiskstorage"></a>Update DPMDiskStorage

U kunt de werkbelasting-bewuste opslag instellen met behulp van de PowerShell-cmdlet Update-DPMDiskStorage, die de eigenschappen van een volume in de opslaggroep op een server met Data Protection Manager-updates.

Syntaxis:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
De volgende schermafbeelding ziet de cmdlet Update-DPMDiskStorage in het venster PowerShell.

![De opdracht Update DPMDiskStorage in het venster PowerShell](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

Met behulp van PowerShell aangebrachte wijzigingen worden doorgevoerd in de back-up Server Administrator-Console.

![Schijven en volumes in de Administrator-Console](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a>Volgende stappen
Nadat u de back-up-Server hebt geïnstalleerd, informatie over het voorbereiden van uw server of met het beveiligen van een werkbelasting beginnen.

- [Back-upserver van werkbelastingen voorbereiden](backup-azure-microsoft-azure-backup.md)
- [Back-up-Server gebruiken om back-up van een VMware-server](backup-azure-backup-server-vmware.md)
- [Back-up-Server gebruiken om back-up van SQL Server](backup-azure-sql-mabs.md)

