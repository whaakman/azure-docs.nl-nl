---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: b657ee32e76dd90671f7e91337ced01b925889a1
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57555041"
---
Als u eventuele problemen met het apparaat ondervindt, kunt u een ondersteuningspakket maken in het systeemlogboek in Logboeken. Microsoft Support maakt gebruik van dit pakket om de problemen op te. Volg deze stappen voor een ondersteuningspakket maken:

1. [Verbinding maken met de PowerShell-interface van uw apparaat](#connect-to-the-powershell-interface).
2. Gebruik de `Get-HcsNodeSupportPackage` opdracht een ondersteuningspakket maken. Het gebruik van de cmdlet is als volgt:

    ```powershell
    Get-HcsNodeSupportPackage [-Path] <string> [-Zip] [-ZipFileName <string>] [-Include {None | RegistryKeys | EtwLogs
            | PeriodicEtwLogs | LogFiles | DumpLog | Platform | FullDumps | MiniDumps | ClusterManagementLog | ClusterLog |
            UpdateLogs | CbsLogs | StorageCmdlets | ClusterCmdlets | ConfigurationCmdlets | KernelDump | RollbackLogs |
            Symbols | NetworkCmdlets | NetworkCmds | Fltmc | ClusterStorageLogs | UTElement | UTFlag | SmbWmiProvider |
            TimeCmds | LocalUILogs | ClusterHealthLogs | BcdeditCommand | BitLockerCommand | DirStats | ComputeRolesLogs |
            ComputeCmdlets | DeviceGuard | Manifests | MeasuredBootLogs | Stats | PeriodicStatLogs | MigrationLogs |
            RollbackSupportPackage | ArchivedLogs | Default}] [-MinimumTimestamp <datetime>] [-MaximumTimestamp <datetime>]
            [-IncludeArchived] [-IncludePeriodicStats] [-Credential <pscredential>]  [<CommonParameters>]
    ```

    De cmdlet. het verzamelt logboeken van uw apparaat en deze logboeken worden gekopieerd naar een opgegeven netwerk of een lokale share.

    De parameters die worden gebruikt zijn als volgt:

    - `-Path` -Geef op het netwerk of het lokale pad naar het ondersteuningspakket te kopiëren. (vereist)
    - `-Credential` -Geef de referenties voor toegang tot het beveiligde pad.
    - `-Zip` -Geef voor het genereren van een zip-bestand.
    - `-Include` -Geef zodanig dat de onderdelen moeten worden opgenomen in het ondersteuningspakket met. Indien niet opgegeven, `Default` wordt uitgegaan.
    - `-IncludeArchived` -Geef zodat gearchiveerde logboeken die u in het ondersteuningspakket met.
    - `-IncludePeriodicStats` -Geef om op te nemen van periodieke stat Logboeken in het ondersteuningspakket met.

    
