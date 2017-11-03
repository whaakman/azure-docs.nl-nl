---
title: Installatie op de achtergrond van Azure Backup-Server v2 | Microsoft Docs
description: Gebruik een PowerShell-script op de achtergrond installeert v2 voor Azure Backup-Server. Dit soort installatie is een afkorting van een installatie zonder toezicht.
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/30/2017
ms.author: markgal;masaran
ms.openlocfilehash: 91778a67f9ef523aa87b7918197e0d0ded0f5702
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a>Voer een installatie zonder toezicht van Azure Backup-Server v2

Informatie over het uitvoeren van een installatie zonder toezicht van v2 voor Azure Backup-Server. 

Deze stappen zijn niet van toepassing als u v1 Azure Backup-Server installeert.

## <a name="install-backup-server-v2"></a>Back-upserver v2 installeren

1. Op de server die als host fungeert voor Azure Backup-Server v2, maak een tekstbestand. (U kunt het bestand maken in Kladblok of in een andere teksteditor.) Sla het bestand als MABSSetup.ini. 

2. Plak de volgende code in het bestand MABSSetup.ini. Vervang de tekst tussen de vierkante haken (\< \>) met waarden uit uw omgeving. De volgende tekst is een voorbeeld:

  ```
  [OPTIONS]
  UserName=administrator
  CompanyName=<Microsoft Corporation>
  SQLMachineName=localhost
  SQLInstanceName=<SQL instance name>
  SQLMachineUserName=administrator
  SQLMachinePassword=<admin password>
  SQLMachineDomainName=<machine domain>
  ReportingMachineName=localhost
  ReportingInstanceName=<reporting instance name>
  SqlAccountPassword=<admin password>
  ReportingMachineUserName=<username>
  ReportingMachinePassword=<reporting admin password>
  ReportingMachineDomainName=<domain>
  VaultCredentialFilePath=<vault credential full path and complete name>
  SecurityPassphrase=<passphrase>
  PassphraseSaveLocation=<passphrase save location>
  UseExistingSQL=<1/0 use or do not use existing SQL>
  ```

3. Sla het bestand op. Voer vervolgens bij een opdrachtprompt met verhoogde bevoegdheid op de server voor installatie met deze opdracht:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

U kunt deze vlaggen gebruiken voor de installatie:</br>
**/f**: pad naar INI-bestand</br>
**/ l**: logboekpad</br>
**/i**: installatiepad</br>
**/x**: pad verwijderen</br>

## <a name="next-steps"></a>Volgende stappen
Nadat u de back-up-Server hebt geïnstalleerd, informatie over het voorbereiden van uw server of met het beveiligen van een werkbelasting beginnen.

- [Back-upserver van werkbelastingen voorbereiden](backup-azure-microsoft-azure-backup.md)
- [Back-up-Server gebruiken om back-up van een VMware-server](backup-azure-backup-server-vmware.md)
- [Back-up-Server gebruiken om back-up van SQL Server](backup-azure-sql-mabs.md)
- [Moderne back-up naar back-upserver toevoegen](backup-mabs-add-storage.md)
