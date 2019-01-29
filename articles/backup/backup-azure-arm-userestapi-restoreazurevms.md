---
title: 'Azure Backup: Herstellen van virtuele Azure-machines met behulp van REST-API'
description: bewerkingen voor het herstellen van Azure VM Backup met behulp van REST-API beheren
services: backup
author: pvrk
manager: shivamg
keywords: REST-API; Azure VM back-up. Azure VM-herstel.
ms.service: backup
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: pullabhk
ms.assetid: b8487516-7ac5-4435-9680-674d9ecf5642
ms.openlocfilehash: 4a65e8a855b9be797c1ceeacf4b74fea74697d00
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55100193"
---
# <a name="restore-azure-virtual-machines-using-rest-api"></a>Herstellen van Azure Virtual machines met behulp van REST-API

Zodra de back-up van een virtueel Azure-machine met Azure Backup is voltooid, kunt een hele Azure Virtual machines, schijven of bestanden herstellen met de dezelfde back-up. Dit artikel wordt beschreven hoe u een Azure-VM of schijven met behulp van REST-API te herstellen.

Voor een bewerking voor het herstellen, een voor het identificeren van de relevante herstelpunt eerst heeft.

## <a name="select-recovery-point"></a>Herstelpunt selecteren

De beschikbare herstelpunten van een back-upitem dat kunnen worden weergegeven met behulp van de [lijst herstelpunt REST-API](https://docs.microsoft.com/rest/api/backup/recoverypoints/list). Er is een eenvoudige *ophalen* bewerking opnieuw uit met de relevante waarden.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints?api-version=2016-12-01
```

De `{containerName}` en `{protectedItemName}` zijn samengesteld [hier](backup-azure-arm-userestapi-backupazurevms.md#example-responses-1). `{fabricName}` 'Azure' is.

De *ophalen* URI heeft de vereiste parameters. Er is niet nodig voor een aanvullende aanvraagtekst

### <a name="responses"></a>Antwoorden

|Name  |Type  |Beschrijving  |
|---------|---------|---------|
|200 OK     |   [RecoveryPointResourceList](https://docs.microsoft.com/rest/api/backup/recoverypoints/list#recoverypointresourcelist)      |       OK  |

#### <a name="example-response"></a>Voorbeeld van een antwoord

Zodra de *ophalen* URI wordt ingediend, een 200 (OK) antwoord wordt geretourneerd.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-client-request-id: c48f4436-ce3f-42da-b537-12710d4d1a24; c48f4436-ce3f-42da-b537-12710d4d1a24
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14998
x-ms-correlation-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-routing-request-id: SOUTHINDIA:20180604T071851Z:03453538-2f8d-46de-8374-143ccdf60f40
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 07:18:51 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/20982486783671",
      "name": "20982486783671",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "AppConsistent",
        "recoveryPointTime": "2018-06-04T06:06:00.5121087Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/23358112038108",
      "name": "23358112038108",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "CrashConsistent",
        "recoveryPointTime": "2018-06-03T06:20:56.3043878Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
......
```

Het herstelpunt dat wordt geïdentificeerd met de `{name}` veld in het bovenstaande antwoord.

## <a name="restore-disks"></a>Schijven herstellen

Als er een nodig om aan te passen van het maken van een virtuele machine van de back-upgegevens, kan een alleen schijven herstellen naar een gekozen storage-account en een virtuele machine maken van deze schijven aan de hand van hun behoeften. Het opslagaccount moet zich in dezelfde regio als de recovery services-kluis en mag geen zone-redundant. De schijven, evenals de configuratie van de back-ups van virtuele machine ("vmconfig.json") worden opgeslagen in het opgegeven opslagaccount.

U activeert op deze manier is een *POST* aanvraag. Raadpleeg voor meer informatie over het terugzetten van schijven, de [REST-API 'terugzetbewerking kan worden geactiveerd'](https://docs.microsoft.com/rest/api/backup/restores/trigger).

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2016-12-01
```

De `{containerName}` en `{protectedItemName}` zijn samengesteld [hier](backup-azure-arm-userestapi-backupazurevms.md#example-responses-1). `{fabricName}` 'Azure' is en de `{recoveryPointId}` is de `{name}` veld van het herstelpunt vermeld [hierboven](#example-response).

### <a name="create-request-body"></a>Hoofdtekst van de aanvraag maken

Hier volgen voor het activeren van een schijf herstellen vanaf een back-up van virtuele Azure-machine, de onderdelen van de aanvraagtekst.

|Name  |Type  |Beschrijving  |
|---------|---------|---------|
|properties     | [IaaSVMRestoreRequest](https://docs.microsoft.com/rest/api/backup/restores/trigger#iaasvmrestorerequest)        |    RestoreRequestResourceProperties     |

Raadpleeg voor de volledige lijst met definities van de aanvraagtekst en andere details [herstellen REST API-document activeren](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body).

#### <a name="example-request"></a>Voorbeeldaanvraag

De hoofdtekst van de volgende aanvraag definieert de eigenschappen die zijn vereist voor het activeren van een schijf herstellen.

```json
{
  "properties": {
    "objectType": "IaasVMRestoreRequest",
    "recoveryPointId": "20982486783671",
    "recoveryType": "RestoreDisks",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testAccount",
    "region": "westus",
    "createNewCloudService": false,
    "originalStorageAccountOption": false,
    "encryptionDetails": {
      "encryptionEnabled": false
    }
  }
}
```

### <a name="response"></a>Antwoord

Het activeren van een schijf terugzetten is een [asynchrone bewerking](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Dit betekent dat deze bewerking wordt gemaakt van een andere bewerking waardoor moet afzonderlijk worden bijgehouden.

Deze retourneert twee antwoorden: 202 (aanvaard) wanneer een andere bewerking wordt gemaakt en vervolgens 200 (OK) wanneer deze bewerking is voltooid.

|Name  |Type  |Beschrijving  |
|---------|---------|---------|
|202 geaccepteerd     |         |     Geaccepteerd    |

#### <a name="example-responses"></a>Van de voorbeeldantwoorden

Zodra u de *POST* URI voor het activeren van schijven herstellen, is de eerste reactie 202 (aanvaard) met een location-header of de Azure-async-header.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2016-12-01
X-Content-Type-Options: nosniff
x-ms-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-client-request-id: a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b; a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1198
x-ms-correlation-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-routing-request-id: SOUTHINDIA:20180604T130003Z:893fe372-8d6c-4c56-b589-45a95eeef95f
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:00:03 GMT
Location: https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2016-12-01
X-Powered-By: ASP.NET
```

De resulterende bewerking met de location-header of de Azure-AsyncOperation koptekst met een eenvoudige vervolgens bijhouden *ophalen* opdracht.

```http
GET https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2016-12-01
```

Nadat de bewerking is voltooid, wordt 200 (OK) geretourneerd met de ID van de resulterende hersteltaak in de hoofdtekst van de reactie.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-client-request-id: a7f3a144-ed80-41ee-bffe-ae6a90c35a2f; a7f3a144-ed80-41ee-bffe-ae6a90c35a2f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14973
x-ms-correlation-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-routing-request-id: SOUTHINDIA:20180604T130917Z:ea2a8011-eb83-4a4b-9ed2-e694070a966a
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:09:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "name": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "status": "Succeeded",
  "startTime": "2018-06-04T13:00:03.8068176Z",
  "endTime": "2018-06-04T13:00:03.8068176Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "3021262a-fb3a-4538-9b37-e3e97a386093"
  }
}
```

Aangezien de back-uptaak een langdurige bewerking is, deze moet worden bijgehouden, zoals wordt beschreven de [bewaken van taken met behulp van REST-API-document](backup-azure-arm-userestapi-managejobs.md#tracking-the-job).

Zodra de langlopende taak voltooid is, zijn de schijven en de configuratie van de back-ups van virtuele machine ("VMConfig.json") worden gebruikt in het opgegeven opslagaccount.

## <a name="restore-as-another-virtual-machine"></a>Als een andere virtuele machine herstellen

[Selecteer het herstelpunt dat](#select-recovery-point) en hoofdtekst van de aanvraag opgegeven hieronder om een andere Azure-virtuele machine maken met de gegevens van het herstelpunt te maken.

De hoofdtekst van de volgende aanvraag definieert de eigenschappen die zijn vereist voor het activeren van een virtuele machine herstellen.

```json
{
  "parameters": {
        "subscriptionId": "00000000-0000-0000-0000-000000000000",
        "resourceGroupName": "testVaultRG",
        "vaultName": "testVault",
        "fabricName": "Azure",
        "containerName": "IaasVMContainer;iaasvmcontainerv2;testRG;testVM",
        "protectedItemName": "VM;iaasvmcontainerv2;testRG;testVM",
        "recoveryPointId": "348916168024334",
        "api-version": "2016-12-01",
      "parameters": {
        "properties": {
          "objectType":  "IaasVMRestoreRequest",
          "recoveryPointId": "348916168024334",
          "recoveryType": "AlternateLocation",
          "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
          "targetVirtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Compute/virtualmachines/targetVM",
          "targetResourceGroupId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG",
          "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testingAccount",
          "virtualNetworkId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet",
          "subnetId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet/subnets/default",
          "region": "westus",
          "createNewCloudService": false,
          "originalStorageAccountOption": false,
          "encryptionDetails": {
            "encryptionEnabled": false
          }
        }
      }
    }
}
```

Het antwoord moet worden verwerkt op dezelfde manier als [hierboven is uitgelegd voor het herstellen van schijven](#response).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor meer informatie over de REST API's van Azure back-up:

- [Azure Recovery Services-provider REST-API](/rest/api/recoveryservices/)
- [Aan de slag met REST API van Azure](/rest/api/azure/)