---
title: Azure Batch compute knooppunt omgevingsvariabelen | Microsoft Docs
description: Knooppunt omgeving variabeleverwijzing berekenen voor Azure Batch Analytics.
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/05/2017
ms.author: tamram
ms.openlocfilehash: 29f642754430957e77ef68946f721f8e15dba065
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-batch-compute-node-environment-variables"></a>Azure Batch compute knooppunt-omgevingsvariabelen
De [Azure Batch-service](https://azure.microsoft.com/services/batch/) stelt u de volgende omgevingsvariabelen op rekenknooppunten. U kunt verwijzen naar deze omgevingsvariabelen in taak opdrachtregels, en de programma's en scripts worden uitgevoerd door de opdracht-regels.

Zie voor meer informatie over het gebruik van omgevingsvariabelen met Batch [omgevingsinstellingen voor taken](https://docs.microsoft.com/azure/batch/batch-api-basics#environment-settings-for-tasks).

## <a name="environment-variable-visibility"></a>De variabele zichtbaarheid omgeving

Deze omgevingsvariabelen zijn alleen zichtbaar in de context van de **taak gebruiker**, het gebruikersaccount op het knooppunt waarin een taak wordt uitgevoerd. U ziet deze *niet* als u [extern verbinding maakt](https://azure.microsoft.com/documentation/articles/batch-api-basics/#connecting-to-compute-nodes) met een rekenknooppunt via Remote Desktop Protocol (RDP) of Secure Shell (SSH) en de omgevingsvariabelen weergeeft. Dit komt doordat het gebruikersaccount dat voor de externe verbinding wordt gebruikt, niet hetzelfde is als het account dat door de taak wordt gebruikt.

## <a name="command-line-expansion-of-environment-variables"></a>Opdrachtregelprogramma uitbreiding van omgevingsvariabelen

De opdrachtregels uitgevoerd door de taken op rekenknooppunten knooppunten komen niet uitgevoerd onder een shell. Daarom deze opdrachtregels niet systeemeigen profiteren van de shellfuncties zoals omgevingsvariabele-uitbreiding (dit omvat de `PATH`). Om te profiteren van deze functies, moet u **aanroepen van de shell** op de opdrachtregel. Bijvoorbeeld starten `cmd.exe` op Windows-rekenknooppunten of `/bin/sh` op Linux-knooppunten:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Omgevingsvariabelen

| Naam variabele                     | Beschrijving                                                              | Beschikbaarheid | Voorbeeld |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | De naam van het Batch-account dat de taak behoort.                  | Alle taken.   | mybatchaccount |
| AZ_BATCH_CERTIFICATES_DIR       | Een map in de [taak werkmap] [ files_dirs] welke certificaten zijn opgeslagen voor Linux-rekenknooppunten. Merk op dat deze omgevingsvariabele niet van toepassing op Windows-rekenknooppunten.                                                  | Alle taken.   |  /mnt/batch/Tasks/workitems/batchjob001/Job-1/task001/certs |
| AZ_BATCH_JOB_ID                 | De ID van de job waartoe de taak behoort. | Alle taken, behalve de taak starten. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | Het volledige pad van de taakvoorbereidingstaak [taakmap] [ files_dirs] op het knooppunt. | Alle taken met uitzondering van begintaak en jobvoorbereidingstaak. Alleen beschikbaar als de taak is geconfigureerd met een jobvoorbereidingstaak. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | Het volledige pad van de taakvoorbereidingstaak [taak werkmap] [ files_dirs] op het knooppunt. | Alle taken met uitzondering van begintaak en jobvoorbereidingstaak. Alleen beschikbaar als de taak is geconfigureerd met een jobvoorbereidingstaak. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_NODE_ID                | De ID van het knooppunt dat de taak is toegewezen. | Alle taken. | TVM-1219235766_3-20160919t172711z |
| AZ_BATCH_NODE_ROOT_DIR          | Het volledige pad van de hoofdmap van alle [Batch-mappen] [ files_dirs] op het knooppunt. | Alle taken. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | Het volledige pad van de [gedeelde map] [ files_dirs] op het knooppunt. Alle taken die worden uitgevoerd op een knooppunt hebt lezen/schrijven-toegang tot deze map. Taken die worden uitgevoerd op andere knooppunten hoeft geen externe toegang tot deze map (dit is niet een netwerkmap 'gedeeld'). | Alle taken. | C:\user\tasks\shared |
| AZ_BATCH_NODE_STARTUP_DIR       | Het volledige pad van de [start taakmap] [ files_dirs] op het knooppunt. | Alle taken. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | De ID van de pool waarin de taak wordt uitgevoerd. | Alle taken. | batchpool001 |
| AZ_BATCH_TASK_DIR               | Het volledige pad van de [taakmap] [ files_dirs] op het knooppunt. Deze map bevat de `stdout.txt` en `stderr.txt` voor de taak en de AZ_BATCH_TASK_WORKING_DIR. | Alle taken. | C:\user\tasks\workitems\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | De id van de huidige taak. | Alle taken, behalve de taak starten. | task001 |
| AZ_BATCH_TASK_WORKING_DIR       | Het volledige pad van de [taak werkmap] [ files_dirs] op het knooppunt. De actieve taak heeft lees-/ schrijftoegang tot deze map. | Alle taken. | C:\user\tasks\workitems\batchjob001\job-1\task001\wd |
| CCP_NODES                       | De lijst met knooppunten en het aantal kernen per knooppunt dat wordt toegewezen aan een [taak met meerdere instanties][multi_instance]. Knooppunten en kernen worden vermeld in de notatie`numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, waarbij het aantal knooppunten wordt gevolgd door een of meer IP-adressen en het aantal kernen voor elk. |  Meerdere exemplaren primaire en subtaken zijn. |`2 10.0.0.4 1 10.0.0.5 1` |
| AZ_BATCH_NODE_LIST              | De lijst met knooppunten die zijn toegewezen aan een [taak met meerdere instanties] [ multi_instance] in de notatie `nodeIP;nodeIP`. | Meerdere exemplaren primaire en subtaken zijn. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_HOST_LIST              | De lijst met knooppunten die zijn toegewezen aan een [taak met meerdere instanties] [ multi_instance] in de notatie `nodeIP,nodeIP`. | Meerdere exemplaren primaire en subtaken zijn. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_MASTER_NODE            | De IP-adres en poort van het rekenknooppunt waarop de primaire taak van een [taak met meerdere instanties] [ multi_instance] wordt uitgevoerd. | Meerdere exemplaren primaire en subtaken zijn. | `10.0.0.4:6000`|
| AZ_BATCH_TASK_SHARED_DIR | Een mappad die identiek is voor de primaire taak en elke subtaak van een [taak met meerdere instanties][multi_instance]. Het pad bestaat op elk knooppunt waarop meerdere exemplaren van de taak wordt uitgevoerd en is toegankelijk voor de taak opdrachten uitgevoerd op dat knooppunt lezen/schrijven (zowel de [coördinatie opdracht] [ coord_cmd] en de [ toepassingsopdracht][app_cmd]). Subtaken of een primaire taak die worden uitgevoerd op andere knooppunten geen externe toegang tot deze map (dit is niet een netwerkmap 'gedeeld'). | Meerdere exemplaren primaire en subtaken zijn. | C:\user\tasks\workitems\multiinstancesamplejob\job-1\multiinstancesampletask |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Hiermee bepaalt u of het huidige knooppunt het hoofdknooppunt voor een [taak met meerdere instanties][multi_instance]. Mogelijke waarden zijn `true` en `false`.| Meerdere exemplaren primaire en subtaken zijn. | `true` |
| AZ_BATCH_NODE_IS_DEDICATED | Als `true`, is een speciale knooppunt van het huidige knooppunt. Als `false`, is een [prioriteit Laag knooppunt](batch-low-pri-vms.md). | Alle taken. | `true` |

[files_dirs]: https://azure.microsoft.com/documentation/articles/batch-api-basics/#files-and-directories
[multi_instance]: https://azure.microsoft.com/documentation/articles/batch-mpi/
[coord_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#coordination-command
[app_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#application-command
