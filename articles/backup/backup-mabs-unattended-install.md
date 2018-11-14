---
title: Installatie op de achtergrond van Azure Backup Server V2
description: Gebruik een PowerShell-script op de achtergrond installeert Azure Backup Server V2. Dit soort installatie is een afkorting voor een installatie zonder toezicht.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: markgal
ms.openlocfilehash: 8f7bda070e613e5495d3733dd1610cb291e12c36
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612692"
---
# <a name="run-an-unattended-installation-of-azure-backup-server"></a>Voer een installatie zonder toezicht van Azure Backup Server

Leer hoe u een installatie zonder toezicht van Azure Backup Server uitvoert.

Deze stappen niet van toepassing als u Azure Backup Server V1 installeert.

## <a name="install-backup-server"></a>Back-Server installeren

1. Op de server die als host fungeert voor Azure Backup-Server versie 2 of hoger, maak een tekstbestand. (U kunt het bestand maken in Kladblok of een andere teksteditor.) Sla het bestand als MABSSetup.ini.

2. Plak de volgende code in het bestand MABSSetup.ini. Vervang de tekst tussen de vierkante haken (\< \>) met waarden van uw omgeving. De volgende tekst is een voorbeeld:

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

3. Sla het bestand op. Klik vervolgens op een opdrachtprompt met verhoogde bevoegdheid op de server voor installatie, voer de volgende opdracht:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

U kunt deze vlaggen gebruiken voor de installatie:</br>
**/f**: pad naar INI-bestand</br>
**/ l**: logboekpad</br>
**/i**: installation path</br>
**/x**: pad verwijderen</br>

## <a name="next-steps"></a>Volgende stappen
Nadat u de back-up-Server hebt geïnstalleerd, informatie over het voorbereiden van uw server, of gaat een werkbelasting beveiligen.

- [Back-up-Server-workloads voorbereiden](backup-azure-microsoft-azure-backup.md)
- [Backup Server gebruiken voor back-up van een VMware-server](backup-azure-backup-server-vmware.md)
- [Backup Server gebruiken voor back-up van SQL Server](backup-azure-sql-mabs.md)
- [Modern Backup Storage toevoegen naar back-upserver](backup-mabs-add-storage.md)
