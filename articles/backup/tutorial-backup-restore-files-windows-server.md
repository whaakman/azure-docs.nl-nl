---
title: Bestanden herstellen van Azure met een Windows Server | Microsoft Docs
description: Deze zelfstudie details herstellende items van Azure met een Windows Server.
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
keywords: WindowsServer back-up; herstellen van bestanden WindowsServer. een back-up en herstel na noodgevallen
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2017
ms.author: saurabhsensharma;markgal;
ms.custom: mvc
ms.openlocfilehash: b5f77ec04ef6d267583a6dc6a4476f118a4d0f74
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="recover-files-from-azure-to-a-windows-server"></a>Bestanden herstellen van Azure met een Windows Server

Azure Backup kunt het herstel van afzonderlijke items uit de back-ups van uw Windows-Server. Herstel van afzonderlijke bestanden is handig als u snel bestanden die per ongeluk zijn verwijderd moet herstellen. Deze zelfstudie bevat informatie over hoe u de agent van Microsoft Azure Recovery Services Agent (MARS) kunt gebruiken om items te herstellen vanuit back-ups die u al hebt uitgevoerd in Azure. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Herstel van afzonderlijke items initiëren 
> * Selecteer een herstelpunt 
> * Items herstellen vanaf een herstelpunt

Deze zelfstudie wordt ervan uitgegaan dat u de stappen voor het al hebt uitgevoerd [Back-up van een Windows-Server naar Azure](backup-configure-vault.md) en er ten minste één back-up van uw Windows Server-bestanden in Azure.

## <a name="initiate-recovery-of-individual-items"></a>Herstel van afzonderlijke items initiëren

Nuttige wizard voor de gebruikersinterface met de naam Microsoft Azure Backup is met de Microsoft Azure Recovery Services (MARS)-agent geïnstalleerd. De wizard Microsoft Azure Backup werkt met de Microsoft Azure Recovery Services (MARS) agent back-gegevens ophalen van herstelpunten die zijn opgeslagen in Azure. Gebruik de wizard Microsoft Azure Backup voor het identificeren van de bestanden of mappen die u wilt herstellen naar Windows Server. 

1. Open de **Microsoft Azure Backup** -module. U vindt deze door te zoeken naar **Microsoft Azure Backup** op uw machine.

    ![Back-up in behandeling](./media/tutorial-backup-restore-files-windows-server/mars.png)

2. Klik in de wizard op **gegevens herstellen** in de **actiedeelvenster** van de agent-console starten de **gegevens herstellen** wizard.

    ![Back-up in behandeling](./media/tutorial-backup-restore-files-windows-server/mars-recover-data.png)

3. Op de **aan de slag** pagina **deze server (server name)** en klik op **volgende**.

4. Op de **herstelmodus Selecteer** pagina **afzonderlijke bestanden en mappen** en klik vervolgens op **volgende** om te beginnen met het herstelproces voor de selectie van punt.
 
5. Op de **Volume selecteert en datum** pagina, selecteert u het volume met de bestanden of mappen die u wilt herstellen en klik op **koppelen**. Selecteer een datum en selecteer een tijd in de vervolgkeuzelijst die overeenkomt met een herstelpunt wordt gemaakt. Datums **vet** geven de beschikbaarheid van ten minste één herstelpunt op die dag.

    ![Back-up in behandeling](./media/tutorial-backup-restore-files-windows-server/mars-select-date.png)
 
    Wanneer u klikt op **koppelen**, maakt Azure Backup van het herstelpunt beschikbaar als een schijf. Bladeren en het herstellen van bestanden van de schijf.

## <a name="restore-items-from-a-recovery-point"></a>Items herstellen vanaf een herstelpunt

1. Nadat het herstel volume is gekoppeld, klikt u op **Bladeren** Windows Verkenner openen en zoeken van de bestanden en mappen die u wilt herstellen. 

    ![Back-up in behandeling](./media/tutorial-backup-restore-files-windows-server/mars-browse-recover.png)

    U kunt de bestanden rechtstreeks openen vanuit het volume herstel en controleer of de bestanden.

2. Kopieer de bestanden en/of de mappen die u wilt herstellen en plak ze op elke gewenste locatie op de server in Windows Verkenner.

    ![Back-up in behandeling](./media/tutorial-backup-restore-files-windows-server/mars-final.png)

3. Wanneer u klaar voor het herstellen van de bestanden en/of mappen bent, op de **bladeren en herstelbestanden** pagina van de **gegevens herstellen** wizard, klikt u op **ontkoppelen**. 

    ![Back-up in behandeling](./media/tutorial-backup-restore-files-windows-server/unmount-and-confirm.png)

4.  Klik op **Ja** om te bevestigen dat u wilt ontkoppelen van het volume.

    Nadat de momentopname ontkoppeld is, **taak voltooid** wordt weergegeven in de **taken** deelvenster in de console van de agent.

## <a name="next-steps"></a>Volgende stappen

Dit is voltooid voor de zelfstudies voor back-up en herstellen van Windows Server-gegevens naar Azure. Zie voor meer informatie over Azure Backup, de PowerShell-voorbeeld voor een back-up versleutelde virtuele machines.

> [!div class="nextstepaction"]
> [Back-up van versleutelde VM](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
