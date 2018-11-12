---
title: "Azure back-up: Back-uptaken met REST API's beheren"
description: beheren van back-up en herstellen van taken van Azure Backup met behulp van REST-API
services: backup
author: pvrk
manager: shivamg
keywords: REST-API; Azure VM back-up. Azure VM-herstel.
ms.service: backup
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: pullabhk
ms.assetid: b234533e-ac51-4482-9452-d97444f98b38
ms.openlocfilehash: eb8b7dc77d180eb56c2585e93e60a36742f6c84c
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289678"
---
# <a name="track-backup-and-restore-jobs-using-rest-api"></a>Bijhouden van back-up en herstel taken met behulp van REST-API

Azure Backup-service wordt geactiveerd voor taken die worden uitgevoerd op de achtergrond in verschillende scenario's zoals back-up activeren, bewerkingen, uitschakelen van back-up te herstellen. Deze taken kunnen worden gevolgd met behulp van de id's.

## <a name="fetch-job-information-from-operations"></a>Taakgegevens ophalen uit operations

Een bewerking, zoals back-up activeren retourneert altijd een jobID is. Voor bijvoorbeeld: het definitieve antwoord van een [back-REST-API-bewerking activeren](backup-azure-arm-userestapi-backupazurevms.md#example-responses-3) is als volgt:

```http
{
  "id": "cd153561-20d3-467a-b911-cc1de47d4763",
  "name": "cd153561-20d3-467a-b911-cc1de47d4763",
  "status": "Succeeded",
  "startTime": "2018-09-12T02:16:56.7399752Z",
  "endTime": "2018-09-12T02:16:56.7399752Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "41f3e94b-ae6b-4a20-b422-65abfcaf03e5"
  }
}
```

De Azure-VM back-uptaak wordt aangeduid met "jobId" veld en kunnen worden bijgehouden, zoals vermeld [hier](https://docs.microsoft.com/rest/api/backup/jobdetails/) met behulp van een eenvoudige *ophalen* aanvraag.

## <a name="tracking-the-job"></a>De taak volgen

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupJobs/{jobName}?api-version=2017-07-01
```

De `{jobName}` 'jobId' hierboven wordt genoemd. Het antwoord is altijd 200 OK met het veld 'status' met de huidige status van de taak. Zodra deze is 'Voltooid' of 'CompletedWithWarnings', wordt de sectie 'extendedInfo' meer informatie over de taak.

### <a name="response"></a>Antwoord

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|200 OK     | [JobResource](https://docs.microsoft.com/rest/api/backup/jobdetails/get#jobresource)        | OK        |

#### <a name="example-response"></a>Voorbeeld van een antwoord

Zodra de *ophalen* URI wordt ingediend, een 200 (OK) antwoord wordt geretourneerd.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-client-request-id: ba4dff71-1655-4c1d-a71f-c9869371b18b; ba4dff71-1655-4c1d-a71f-c9869371b18b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14989
x-ms-correlation-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-routing-request-id: SOUTHINDIA:20180521T102317Z:e9702101-9da2-4681-bdf3-a54e17329a56
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:23:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-RecoveryServices-ResourceGroup-centralindia/providers/microsoft.recoveryservices/vaults/abdemovault/backupJobs/7ddead57-bcb9-4269-ac31-6a1b57588700",
  "name": "7ddead57-bcb9-4269-ac31-6a1b57588700",
  "type": "Microsoft.RecoveryServices/vaults/backupJobs",
  "properties": {
    "jobType": "AzureIaaSVMJob",
    "duration": "00:20:23.0896697",
    "actionsInfo": [
      1
    ],
    "virtualMachineVersion": "Compute",
    "extendedInfo": {
      "tasksList": [
        {
          "taskId": "Take Snapshot",
          "duration": "00:00:00",
          "status": "Completed"
        },
        {
          "taskId": "Transfer data to vault",
          "duration": "00:00:00",
          "status": "Completed"
        }
      ],
      "propertyBag": {
        "VM Name": "uttestvmub1",
        "Backup Size": "2332 MB"
      }
    },
    "entityFriendlyName": "uttestvmub1",
    "backupManagementType": "AzureIaasVM",
    "operation": "Backup",
    "status": "Completed",
    "startTime": "2018-05-21T08:35:40.9488967Z",
    "endTime": "2018-05-21T08:56:04.0385664Z",
    "activityId": "7df8e874-1d66-4f81-8e91-da2fe054811d"
  }
}
}

```