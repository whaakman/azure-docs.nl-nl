---
title: Azure Backup voor SQL Server wordt uitgevoerd in Azure VM
description: Informatie over het registreren van Azure SQL-Server voor back-up uitgevoerd in Azure VM
services: backup
author: swatisachdeva
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: swatisachdeva
ms.openlocfilehash: 8710242e04156c8af6e5882a3cb4d42cc31e3677
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607609"
---
# <a name="azure-backup-for-sql-server-running-in-azure-vm"></a>Azure Backup voor SQL Server wordt uitgevoerd in Azure VM

Azure Backup, onder andere oplossingen biedt ondersteuning voor back-ups van werkbelastingen zoals SQL Server die wordt uitgevoerd in virtuele Azure-machines. Omdat SQL-toepassing wordt uitgevoerd binnen een Azure-VM, Backup-service heeft toestemming nodig voor toegang tot de toepassing en de benodigde informatie ophalen.
Om dit te doen, Azure Backup installeert de **AzureBackupWindowsWorkload** -extensie op de virtuele machine, waarin de SQL-Server wordt uitgevoerd, tijdens het registratieproces geactiveerd door de gebruiker.

## <a name="prerequisites"></a>Vereisten

Voor de lijst met ondersteunde scenario's, raadpleegt u de [supportability matrix](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#scenario-support) ondersteund door Azure Backup.

## <a name="network-connectivity"></a>Verbinding met het netwerk

Azure Backup biedt ondersteuning voor NSG-Tags en implementeren van een proxyserver of IP-adresbereiken; vermeld Raadpleeg voor meer informatie over elk van de methoden, dit [artikel](https://docs.microsoft.com/azure/backup/backup-sql-server-database-azure-vms#establish-network-connectivity).

## <a name="extension-schema"></a>Extensieschema

De extensie schema en de eigenschap waarden zijn de configuratiewaarden (runtime-instellingen) die de service wordt doorgegeven naar CRP-API. Deze configuratiewaarden worden gebruikt tijdens de registratie en upgrade. **AzureBackupWindowsWorkload** uitbreiding voor dit schema ook gebruiken. Het schema is vooraf ingesteld; een nieuwe parameter kan worden toegevoegd in het veld objectStr

  ```json
      "runtimeSettings": [{
      "handlerSettings": {
      "protectedSettingsCertThumbprint": "",
      "protectedSettings": {
      "objectStr": "",
      "logsBlobUri": "",
      "statusBlobUri": ""
      }
      },
      "publicSettings": {
      "locale": "en-us",
      "taskId": "1c0ae461-9d3b-418c-a505-bb31dfe2095d",
      "objectStr": "",
      "commandStartTimeUTCTicks": "636295005824665976",
      "vmType": "vmType"
      }
      }]
      }
  ```

De volgende JSON ziet u het schema voor de extensie WorkloadBackup.  

  ```json
  {
    "type": "extensions",
    "name": "WorkloadBackup",
    "location":"<myLocation>",
    "properties": {
      "publisher": "Microsoft.RecoveryServices",
      "type": "AzureBackupWindowsWorkload",
      "typeHandlerVersion": "1.1",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "locale":"<location>",
        "taskId":"<TaskId used by Azure Backup service to communicate with extension>",

        "objectStr": "<The configuration passed by Azure Backup service to extension>",

        "commandStartTimeUTCTicks": "<Scheduled start time of registration or upgrade task>",
        "vmType": "<Type of VM where registration got triggered Eg. Compute or ClassicCompute>"
      },
      "protectedSettings": {
        "objectStr": "<The sensitive configuration passed by Azure Backup service to extension>",
        "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
        "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
      }
    }
  }
  ```

### <a name="property-values"></a>Waarden van eigenschappen

Name | Voorbeeld van een waarde / | Gegevenstype
 --- | --- | ---
Landinstelling | NL-ons  |  string
taak-id | "1c0ae461-9d3b-418c-a505-bb31dfe2095d"  | string
objectStr <br/> (publicSettings)  | "eyJjb250YWluZXJQcm9wZXJ0aWVzIjp7IkNvbnRhaW5lcklEIjoiMzVjMjQxYTItOGRjNy00ZGE5LWI4NTMtMjdjYTJhNDZlM2ZkIiwiSWRNZ210Q29udGFpbmVySWQiOjM0NTY3ODg5LCJSZXNvdXJjZUlkIjoiMDU5NWIwOGEtYzI4Zi00ZmFlLWE5ODItOTkwOWMyMGVjNjVhIiwiU3Vic2NyaXB0aW9uSWQiOiJkNGEzOTliNy1iYjAyLTQ2MWMtODdmYS1jNTM5O DI3ZTgzNTQiLCJVbmlxdWVDb250YWluZXJOYW1lIjoiODM4MDZjODUtNTQ4OS00NmNhLWEyZTctNWMzNzNhYjg3OTcyIn0sInN0YW1wTGlzdCI6W3siU2VydmljZU5hbWUiOjUsIlNlcnZpY2VTdGFtcFVybCI6Imh0dHA6XC9cL015V0xGYWJTdmMuY29tIn1dfQ == " | string
commandStartTimeUTCTicks | "636967192566036845"  | string
vmType  | "microsoft.compute/virtualmachines"  | string
objectStr <br/> (protectedSettings) | "eyJjb250YWluZXJQcm9wZXJ0aWVzIjp7IkNvbnRhaW5lcklEIjoiMzVjMjQxYTItOGRjNy00ZGE5LWI4NTMtMjdjYTJhNDZlM2ZkIiwiSWRNZ210Q29udGFpbmVySWQiOjM0NTY3ODg5LCJSZXNvdXJjZUlkIjoiMDU5NWIwOGEtYzI4Zi00ZmFlLWE5ODItOTkwOWMyMGVjNjVhIiwiU3Vic2NyaXB0aW9uSWQiOiJkNGEzOTliNy1iYjAyLTQ2MWMtODdmYS1jNTM5O DI3ZTgzNTQiLCJVbmlxdWVDb250YWluZXJOYW1lIjoiODM4MDZjODUtNTQ4OS00NmNhLWEyZTctNWMzNzNhYjg3OTcyIn0sInN0YW1wTGlzdCI6W3siU2VydmljZU5hbWUiOjUsIlNlcnZpY2VTdGFtcFVybCI6Imh0dHA6XC9cL015V0xGYWJTdmMuY29tIn1dfQ == " | string
logsBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | string
statusBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | string


## <a name="template-deployment"></a>Sjabloonimplementatie

Het is raadzaam om toe te voegen AzureBackupWindowsWorkload uitbreiding van een virtuele machine is door in te schakelen van SQL Server back-up op de virtuele machine. Dit kan worden bereikt via de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vm-workload-backup) ontworpen voor het automatiseren van back-up op een virtuele machine van SQL Server.


## <a name="powershell-deployment"></a>PowerShell-implementatie

U moet de Azure-VM met de SQL-toepassing met een Recovery services-kluis registreren. Tijdens de registratie wordt op de virtuele machine AzureBackupWindowsWorkload-extensie geïnstalleerd. Gebruik [registreren AzRecoveryServicesBackupContainerPS](https://docs.microsoft.com/powershell/module/az.recoveryservices/Register-AzRecoveryServicesBackupContainer?view=azps-1.5.0) cmdlet voor het registreren van de virtuele machine.
 
```powershell
$myVM = Get-AzVM -ResourceGroupName <VMRG Name> -Name <VMName>
Register-AzRecoveryServicesBackupContainer -ResourceId $myVM.ID -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetVault.ID -Force
```
 
De opdracht retourneert een **back-upcontainer** van deze resource en de status worden **geregistreerd**.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](https://docs.microsoft.com/azure/backup/backup-sql-server-azure-troubleshoot) probleemoplossing richtlijnen over Azure SQL Server-VM back-up
- [Veelgestelde vragen over](https://docs.microsoft.com/azure/backup/faq-backup-sql-server) over back-ups van SQL Server-databases die worden uitgevoerd op Azure virtual machines (VM's) en die gebruikmaken van de Azure Backup-service.
