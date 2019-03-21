---
title: Omgevingsvariabelen - Azure Batch-rekenknooppunt | Microsoft Docs
description: Verwijzing naar een knooppunt omgeving variabele berekenen voor Azure Batch-analyses.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/07/2019
ms.author: lahugh
ms.openlocfilehash: 9902f38ddfd3035adcce697c2eb5b77bdc1d8c9c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57874758"
---
# <a name="azure-batch-compute-node-environment-variables"></a>Azure Batch compute omgevingsvariabelen

De [Azure Batch-service](https://azure.microsoft.com/services/batch/) Hiermee stelt u de volgende omgevingsvariabelen op rekenknooppunten. U kunt verwijzen naar deze omgevingsvariabelen in opdrachtregels en in de programma's en scripts uitvoeren met de opdrachtregel.

Zie voor meer informatie over het gebruik van omgevingsvariabelen met Batch [omgevingsinstellingen voor taken](https://docs.microsoft.com/azure/batch/batch-api-basics#environment-settings-for-tasks).

## <a name="environment-variable-visibility"></a>Omgeving variabele zichtbaarheid

Deze omgevingsvariabelen zijn alleen zichtbaar in de context van de **taakgebruiker**, het gebruikersaccount op het knooppunt waaronder een taak wordt uitgevoerd. U ziet deze *niet* als u [extern verbinding maakt](https://azure.microsoft.com/documentation/articles/batch-api-basics/#connecting-to-compute-nodes) met een rekenknooppunt via Remote Desktop Protocol (RDP) of Secure Shell (SSH) en de omgevingsvariabelen weergeeft. Dit komt doordat het gebruikersaccount dat voor de externe verbinding wordt gebruikt, niet hetzelfde is als het account dat door de taak wordt gebruikt.

## <a name="command-line-expansion-of-environment-variables"></a>Opdrachtregel uitbreiding van omgevingsvariabelen

De opdrachtregels uitgevoerd door taken op compute-knooppunten kan niet worden uitgevoerd onder een shell. Daarom de regels van deze opdracht kan niet op systeemeigen wijze profiteren van shell-functies zoals omgevingsvariabele-uitbreiding (dit omvat de `PATH`). Als u wilt profiteren van dergelijke functies, moet u **start u de shell** vanaf de opdrachtregel. Bijvoorbeeld: launch `cmd.exe` op Windows-rekenknooppunten of `/bin/sh` op Linux-knooppunten:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Omgevingsvariabelen

| Naam van de variabele                     | Description                                                              | Beschikbaarheid | Voorbeeld |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | De naam van het Batch-account dat de taak behoort.                  | Alle taken.   | mybatchaccount |
| AZ_BATCH_ACCOUNT_URL            | De URL van het Batch-account. | Alle taken. | `https://myaccount.westus.batch.azure.com` |
| AZ_BATCH_APP_PACKAGE            | Een voorvoegsel van alle app-pakket variabelen voor de omgeving. Bijvoorbeeld, als de toepassing "Foo" "1" versie is geïnstalleerd op een groep, is de omgevingsvariabele AZ_BATCH_APP_PACKAGE_FOO_1. AZ_BATCH_APP_PACKAGE_FOO_1 verwijst naar de locatie die het pakket is gedownload (een map). | Een taak met een bijbehorende app-pakket. Ook beschikbaar voor alle taken als het knooppunt zelf toepassingspakketten heeft. | AZ_BATCH_APP_PACKAGE_FOO_1 |
| AZ_BATCH_AUTHENTICATION_TOKEN   | Een verificatietoken die toegang tot een beperkt aantal bewerkingen voor Batch-service verleent. Deze omgevingsvariabele is alleen aanwezig als de [authenticationTokenSettings](/rest/api/batchservice/task/add#authenticationtokensettings) zijn ingesteld wanneer de [taak wordt toegevoegd](/rest/api/batchservice/task/add#request-body). De waarde voor het token wordt gebruikt in de Batch-API's als referenties te maken van een Batch-client, zoals de [BatchClient.Open() .NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient.open#Microsoft_Azure_Batch_BatchClient_Open_Microsoft_Azure_Batch_Auth_BatchTokenCredentials_). | Alle taken. | OAuth2-toegangstoken |
| AZ_BATCH_CERTIFICATES_DIR       | Een map in de [werkmap taak] [ files_dirs] op welke certificaten zijn opgeslagen voor Linux-rekenknooppunten. Houd er rekening mee dat deze omgevingsvariabele is niet van toepassing op Windows-rekenknooppunten.                                                  | Alle taken.   |  /mnt/batch/Tasks/workitems/batchjob001/Job-1/task001/certs |
| AZ_BATCH_HOST_LIST              | De lijst met knooppunten die zijn toegewezen aan een [taken met meerdere instanties] [ multi_instance] in de indeling `nodeIP,nodeIP`. | Meerdere exemplaren primaire en subtaken. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Geeft aan of het huidige knooppunt het hoofdknooppunt voor een [taken met meerdere instanties][multi_instance]. Mogelijke waarden zijn `true` en `false`.| Meerdere exemplaren primaire en subtaken. | `true` |
| AZ_BATCH_JOB_ID                 | De ID van de job waartoe de taak behoort. | Alle taken, behalve de taak te starten. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | Het volledige pad van de taakvoorbereidingstaak [taakmap] [ files_dirs] op het knooppunt. | Alle taken met uitzondering van de begintaak en jobvoorbereidingstaak. Is alleen beschikbaar als de taak is geconfigureerd met een jobvoorbereidingstaak. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | Het volledige pad van de taakvoorbereidingstaak [werkmap taak] [ files_dirs] op het knooppunt. | Alle taken met uitzondering van de begintaak en jobvoorbereidingstaak. Is alleen beschikbaar als de taak is geconfigureerd met een jobvoorbereidingstaak. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_MASTER_NODE            | De IP-adres en poort van de compute-knooppunt waarop de primaire taak van een [taken met meerdere instanties] [ multi_instance] wordt uitgevoerd. | Meerdere exemplaren primaire en subtaken. | `10.0.0.4:6000` |
| AZ_BATCH_NODE_ID                | De ID van het knooppunt dat de taak is toegewezen. | Alle taken. | tvm-1219235766_3-20160919t172711z |
| AZ_BATCH_NODE_IS_DEDICATED      | Als `true`, het huidige knooppunt is een toegewezen knooppunt. Als `false`, is het een [knooppunt met lage prioriteit](batch-low-pri-vms.md). | Alle taken. | `true` |
| AZ_BATCH_NODE_LIST              | De lijst met knooppunten die zijn toegewezen aan een [taken met meerdere instanties] [ multi_instance] in de indeling `nodeIP;nodeIP`. | Meerdere exemplaren primaire en subtaken. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_NODE_ROOT_DIR          | Het volledige pad van de hoofdmap van alle [Batch-mappen] [ files_dirs] op het knooppunt. | Alle taken. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | Het volledige pad van de [gedeelde map] [ files_dirs] op het knooppunt. Alle taken die worden uitgevoerd op een knooppunt hebt lezen/schrijven toegang tot deze map. Taken die worden uitgevoerd op andere knooppunten geen externe toegang tot deze map (dit is niet een netwerkmap 'gedeeld'). | Alle taken. | C:\user\tasks\shared |
| AZ_BATCH_NODE_STARTUP_DIR       | Het volledige pad van de [start taakmap] [ files_dirs] op het knooppunt. | Alle taken. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | De ID van de pool waarin de taak wordt uitgevoerd. | Alle taken. | batchpool001 |
| AZ_BATCH_TASK_DIR               | Het volledige pad van de [taakmap] [ files_dirs] op het knooppunt. Deze map bevat de `stdout.txt` en `stderr.txt` voor de taak en de AZ_BATCH_TASK_WORKING_DIR. | Alle taken. | C:\user\tasks\workitems\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | De id van de huidige taak. | Alle taken, behalve de taak te starten. | task001 |
| AZ_BATCH_TASK_SHARED_DIR | Een mappad die identiek zijn voor de primaire taak en elke subtaak van een [taken met meerdere instanties][multi_instance]. Het pad bestaat op elk knooppunt waarop de meerdere exemplaren taak wordt uitgevoerd en is toegankelijk voor de taak-opdrachten op het knooppunt uitgevoerd voor lezen/schrijven (zowel de [coördinatie opdracht] [ coord_cmd] en de [ toepassingsopdracht][app_cmd]). Subtaken of een primaire taak die worden uitgevoerd op andere knooppunten geen externe toegang tot deze map (dit is niet een netwerkmap 'gedeeld'). | Meerdere exemplaren primaire en subtaken. | C:\user\tasks\workitems\multiinstancesamplejob\job-1\multiinstancesampletask |
| AZ_BATCH_TASK_WORKING_DIR       | Het volledige pad van de [werkmap taak] [ files_dirs] op het knooppunt. De huidige actieve taak heeft lees-/ schrijftoegang tot deze map. | Alle taken. | C:\user\tasks\workitems\batchjob001\job-1\task001\wd |
| CCP_NODES                       | De lijst met knooppunten en het aantal kerngeheugens per knooppunt dat wordt toegewezen aan een [taken met meerdere instanties][multi_instance]. Knooppunten en kernen worden vermeld in de indeling `numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, waarbij het aantal knooppunten wordt gevolgd door een of meer IP-adressen en het aantal kernen voor elk. |  Meerdere exemplaren primaire en subtaken. |`2 10.0.0.4 1 10.0.0.5 1` |

[files_dirs]: https://azure.microsoft.com/documentation/articles/batch-api-basics/#files-and-directories
[multi_instance]: https://azure.microsoft.com/documentation/articles/batch-mpi/
[coord_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#coordination-command
[app_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#application-command
