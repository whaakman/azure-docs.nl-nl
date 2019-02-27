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
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 09/05/2018
ms.openlocfilehash: 685183f61c1574d8c533efad8a7e0c46b1e2d23c
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56870309"
---
# <a name="back-up-azure-stack"></a>Back-up van Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Een on-demand back-up uitvoeren op Azure Stack. Zie voor instructies over het configureren van de PowerShell-omgeving [PowerShell installeren voor Azure Stack](azure-stack-powershell-install.md). Als u wilt aanmelden bij Azure Stack, Zie [met behulp van de beheerdersportal in Azure Stack](azure-stack-manage-portals.md).

## <a name="start-azure-stack-backup"></a>Start Azure Stack-back-up

### <a name="start-a-new-backup-without-job-progress-tracking"></a>Een nieuwe back-up starten zonder de voortgang van de taak volgen
Start-AzSBackup gebruiken een nieuwe back-up direct starten met geen taakvoortgang bijhouden.

```powershell
   Start-AzsBackup -Force
```

### <a name="start-azure-stack-backup-with-job-progress-tracking"></a>Azure Stack back-up starten met de voortgang van de taak volgen
Start-AzSBackup gebruiken voor het starten van een nieuwe back-up met de **is - AsJob** parameter en sla deze op als een variabele voor het bijhouden van back-uptaak wordt uitgevoerd.

> [!NOTE]
> Back-uptaak wordt weergegeven, zoals is uitgevoerd in de beheerportal van ongeveer 10-15 minuten voordat de taak wordt voltooid.
>
> De huidige status is daarom beter waargenomen via de onderstaande code.

> [!IMPORTANT]
> De eerste 1 milliseconde vertraging wordt geïntroduceerd, omdat de code te snel voor het registreren van de taak correct is en deze weer zonder **PSBeginTime** op zijn beurt zonder **status** van de taak.

```powershell
    $BackupJob = Start-AzsBackup -Force -AsJob
    While (!$BackupJob.PSBeginTime) {
        Start-Sleep -Milliseconds 1
    }
    Write-Host "Start time: $($BackupJob.PSBeginTime)"
    While ($BackupJob.State -eq "Running") {
        Write-Host "Job is currently: $($BackupJob.State) - Duration: $((New-TimeSpan -Start ($BackupJob.PSBeginTime) -End (Get-Date)).ToString().Split(".")[0])"
        Start-Sleep -Seconds 30
    }

    If ($BackupJob.State -eq "Completed") {
        Get-AzsBackup | Where-Object {$_.BackupId -eq $BackupJob.Output.BackupId}
        $Duration = $BackupJob.Output.TimeTakenToCreate
        $Pattern = '^P?T?((?<Years>\d+)Y)?((?<Months>\d+)M)?((?<Weeks>\d+)W)?((?<Days>\d+)D)?(T((?<Hours>\d+)H)?((?<Minutes>\d+)M)?((?<Seconds>\d*(\.)?\d*)S)?)$'
        If ($Duration -match $Pattern) {
            If (!$Matches.ContainsKey("Hours")) {
                $Hours = ""
            } 
            Else {
                $Hours = ($Matches.Hours).ToString + 'h '
            }
            $Minutes = ($Matches.Minutes)
            $Seconds = [math]::round(($Matches.Seconds))
            $Runtime = '{0}{1:00}m {2:00}s' -f $Hours, $Minutes, $Seconds
        }
        Write-Host "BackupJob: $($BackupJob.Output.BackupId) - Completed with Status: $($BackupJob.Output.Status) - It took: $($Runtime) to run" -ForegroundColor Green
    }
    ElseIf ($BackupJob.State -ne "Completed") {
        $BackupJob
        $BackupJob.Output
    }
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

Meer informatie over de werkstroom voor [herstellen vanaf een gegevensverlies](azure-stack-backup-recover-data.md).
