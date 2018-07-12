---
title: Maak een back-up van Azure Stack | Microsoft Docs
description: Een on-demand back-up uitvoeren op Azure Stack met back-up op locatie.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/08/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 2570423a3cae2a15f0cfd294f1d91e6730748f68
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972605"
---
# <a name="back-up-azure-stack"></a>Back-up van Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Een on-demand back-up uitvoeren op Azure Stack met back-up op locatie. Zie voor instructies over het configureren van de PowerShell-omgeving [PowerShell installeren voor Azure Stack ](azure-stack-powershell-install.md). Als u wilt aanmelden bij Azure Stack, Zie [configureert u de operator-omgeving en aanmelden bij Azure Stack](azure-stack-powershell-configure-admin.md).

## <a name="start-azure-stack-backup"></a>Start Azure Stack-back-up

Start AzSBackup gebruiken om te starten van een nieuwe back-up met de variabele is - AsJob voortgang bijhouden. 

```powershell
    $backupjob = Start-AzsBackup -Force -AsJob
    "Start time: " + $backupjob.PSBeginTime;While($backupjob.State -eq "Running"){("Job is currently: " + $backupjob.State+" ;Duration: " + (New-TimeSpan -Start ($backupjob.PSBeginTime) -End (Get-Date)).Minutes);Start-Sleep -Seconds 30};$backupjob.Output
```

## <a name="confirm-backup-completed-via-powershell"></a>Controleer of de back-up voltooid via PowerShell

```powershell
    if($backupjob.State -eq "Completed"){Get-AzsBackup | where {$_.BackupId -eq $backupjob.Output.BackupId}}
```

- Het resultaat ziet er als de volgende uitvoer:

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

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Back-up is voltooid in de beheerportal bevestigen

1. Open de Azure Stack-beheerportal bij [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Selecteer **meer services** > **infrastructuur back-up**. Kies **configuratie** in de **infrastructuur back-up** blade.
3. Zoek de **naam** en **datum voltooid** van de back-up in **beschikbare back-ups** lijst.
4. Controleer of de **status** is **geslaagd**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de werkstroom voor het herstellen van het gegevensverlies. Zie [herstel na onherstelbare gegevensverlies](azure-stack-backup-recover-data.md).
