---
title: Maak een back-up van Azure Stack | Microsoft Docs
description: Een on-demand back-up uitvoeren op Azure Stack met back-up op locatie.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: a11de2a4580515f6a358438a706e5be3f5543e28
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44025307"
---
# <a name="back-up-azure-stack"></a>Back-up van Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Een on-demand back-up uitvoeren op Azure Stack met back-up op locatie. Zie voor instructies over het configureren van de PowerShell-omgeving [PowerShell installeren voor Azure Stack ](azure-stack-powershell-install.md). Als u wilt aanmelden bij Azure Stack, Zie [met behulp van de beheerdersportal in Azure Stack](azure-stack-manage-portals.md).

## <a name="start-azure-stack-backup"></a>Start Azure Stack-back-up

### <a name="start-a-new-backup-without-job-progress-tracking"></a>Een nieuwe back-up starten zonder de voortgang van de taak volgen
Start-AzSBackup gebruiken een nieuwe back-up direct starten met geen taakvoortgang bijhouden.

```powershell
   Start-AzsBackup -Force
```

### <a name="start-azure-stack-backup-with-job-progress-tracking"></a>Azure Stack back-up starten met de voortgang van de taak volgen
Start AzSBackup gebruiken om te starten van een nieuwe back-up met de variabele is - AsJob om bij te houden van back-uptaak wordt uitgevoerd.

```powershell
    $backupjob = Start-AzsBackup -Force -AsJob 
    "Start time: " + $backupjob.PSBeginTime;While($backupjob.State -eq "Running"){("Job is currently: " `
    + $backupjob.State+" ;Duration: " + (New-TimeSpan -Start ($backupjob.PSBeginTime) `
    -End (Get-Date)).Minutes);Start-Sleep -Seconds 30};$backupjob.Output

    if($backupjob.State -eq "Completed"){Get-AzsBackup | where {$_.BackupId -eq $backupjob.Output.BackupId}}
```

## <a name="confirm-backup-has-completed"></a>Controleer of de back-up is voltooid

### <a name="confirm-backup-has-completed-using-powershell"></a>Controleer of de back-up is voltooid met behulp van PowerShell
Gebruik de volgende PowerShell-opdrachten om ervoor te zorgen dat back-up is voltooid:

```powershell
   Get-AzsBackup
```

Het resultaat ziet er als de volgende uitvoer:

```powershell
    BackupDataVersion : 1.0.1
    BackupId          : <backup ID>
    RoleStatus        : {NRP, SRP, CRP, KeyVaultInternalControlPlane...}
    Status            : Succeeded
    CreatedDateTime   : 7/6/2018 6:46:24 AM
    TimeTakenToCreate : PT20M32.364138S
    DeploymentID      : <deployment ID>
    StampVersion      : 1.1807.0.41
    OemVersion        : 
    Id                : /subscriptions/<subscription ID>/resourceGroups/System.local/providers/Microsoft.Backup.Admin/backupLocations/local/backups/<backup ID>
    Name              : local/<local name>
    Type              : Microsoft.Backup.Admin/backupLocations/backups
    Location          : local
    Tags              : {}
```

### <a name="confirm-backup-has-completed-in-the-administration-portal"></a>Controleer of de back-up is voltooid in de beheerportal
Gebruik de Azure Stack-beheerportal om te controleren of dat deze back-up is voltooid door de volgende stappen:

1. Open de [Azure Stack-beheerportal](azure-stack-manage-portals.md).
2. Selecteer **alle services**, en klik vervolgens onder de **beheer** categorie selecteren > **infrastructuur back-up**. Kies **configuratie** in de **infrastructuur back-up** blade.
3. Zoek de **naam** en **datum voltooid** van de back-up in **beschikbare back-ups** lijst.
4. Controleer of de **status** is **geslaagd**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de werkstroom voor het herstellen van het gegevensverlies. Zie [herstel na onherstelbare gegevensverlies](azure-stack-backup-recover-data.md).
