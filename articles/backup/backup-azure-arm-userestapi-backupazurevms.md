---
title: 'Azure back-up: Back-up van virtuele Azure-machines met behulp van REST-API'
description: Beheren van back-upbewerkingen van Azure VM Backup met behulp van REST-API
services: backup
author: pvrk
manager: shivamg
keywords: REST-API; Azure VM back-up. Azure VM-herstel.
ms.service: backup
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: pullabhk
ms.assetid: b80b3a41-87bf-49ca-8ef2-68e43c04c1a3
ms.openlocfilehash: 8a47d3cf346d7961e9f8b1c4fa615a2faa6b1da0
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289783"
---
# <a name="back-up-an-azure-vm-using-azure-backup-via-rest-api"></a>Maak een back-up van een Azure-VM met behulp van Azure Backup via REST-API

In dit artikel wordt beschreven hoe u back-ups beheren voor een Azure-VM maken met Azure Backup via REST-API. Configureer beveiliging voor de eerste keer voor een eerder niet-beveiligde virtuele machine van Azure, een on-demand back-up activeren voor een beveiligde Azure-virtuele machine en back-eigenschappen van een back-ups van virtuele machine via de REST-API wijzigen, zoals hier wordt beschreven.

Raadpleeg [kluis maken](backup-azure-arm-userestapi-createorupdatevault.md) en [beleid maken](backup-azure-arm-userestapi-createorupdatepolicy.md) REST-API-zelfstudies voor het maken van nieuwe kluizen en het beleid.

Stel dat u wilt beveiligen van een virtuele machine 'testVM' onder een resource group 'testRG' naar een Recovery Services-kluis "testVault", aanwezig is in de resourcegroep 'testVaultRG', met het standaardbeleid (met de naam 'DefaultPolicy').

## <a name="configure-backup-for-an-unprotected-azure-vm-using-rest-api"></a>Back-up configureren voor een niet-beveiligde Azure-VM met behulp van REST-API

### <a name="discover-unprotected-azure-vms"></a>Niet-beveiligde Azure-VM's detecteren

De kluis moet eerst, kunnen voor het identificeren van de Azure-VM. Dit wordt geactiveerd met behulp van de [vernieuwbewerking](https://docs.microsoft.com/rest/api/backup/protectioncontainers/refresh). Het is een asynchrone *POST* bewerking ervoor dat de kluis zorgt opgehaald van de meest recente lijst met alle niet-beveiligde virtuele machine in het huidige abonnement en ze ' caches. Zodra de virtuele machine 'in de cache opgeslagen is' zich Recovery services voor toegang tot de virtuele machine en deze beveiligen.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupname}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/refreshContainers?api-version=2016-12-01
```

De POST-URI heeft `{subscriptionId}`, `{vaultName}`, `{vaultresourceGroupName}`, `{fabricName}` parameters. De `{fabricName}` 'Azure' is. Volgens ons voorbeeld `{vaultName}` "testVault" is en `{vaultresourceGroupName}` 'testVaultRG' is. Als de vereiste parameters zijn opgegeven in de URI, is er niet nodig voor de hoofdtekst van de afzonderlijke aanvraag.

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/refreshContainers?api-version=2016-12-01
```

#### <a name="responses"></a>Antwoorden

De vernieuwingsbewerking is een [asynchrone bewerking](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Dit betekent dat deze bewerking wordt gemaakt van een andere bewerking waardoor moet afzonderlijk worden bijgehouden.

Deze twee antwoorden retourneert: 202 (aanvaard) wanneer een andere bewerking wordt gemaakt en klik vervolgens op 200 (OK) wanneer deze bewerking is voltooid.

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|204 geen inhoud     |         |  OK met geen inhoud geretourneerd      |
|202 geaccepteerd     |         |     Geaccepteerd    |

##### <a name="example-responses"></a>Van de voorbeeldantwoorden

Zodra de *POST* aanvraag is ingediend, een 202 (Accepted)-antwoord wordt geretourneerd.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
X-Content-Type-Options: nosniff
x-ms-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-client-request-id: 4910609f-bb9b-4c23-8527-eb6fa2d3253f; 4910609f-bb9b-4c23-8527-eb6fa2d3253f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-routing-request-id: SOUTHINDIA:20180521T105701Z:43cf550d-e463-421c-8922-37e4766db27d
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:57:00 GMT
Location: https://management.azure.com/subscriptions//00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2016-12-01
X-Powered-By: ASP.NET
```

Bijhouden van de resulterende bewerking met behulp van de 'Location'-header met een eenvoudige *ophalen* opdracht

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2016-12-01
```

Zodra alle de Azure-VM's worden gedetecteerd, retourneert de GET-opdracht een 204 (geen inhoud) antwoord. De kluis kan nu voor het detecteren van alle virtuele machines binnen het abonnement.

```http
HTTP/1.1 204 NoContent
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-client-request-id: 25bb6345-f9fc-4406-be1a-dc6db0eefafe; 25bb6345-f9fc-4406-be1a-dc6db0eefafe
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14997
x-ms-correlation-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-routing-request-id: SOUTHINDIA:20180521T105825Z:cf6cd73b-9189-4942-a61d-878fcf76b1c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:58:25 GMT
X-Powered-By: ASP.NET
```

### <a name="selecting-the-relevant-azure-vm"></a>De relevante Azure-VM selecteren

 U kunt bevestigen dat 'caching' wordt uitgevoerd door [alle beveiligbare objecten vermelden](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list) onder het abonnement en Ga naar de gewenste VM in het antwoord. [Het antwoord van deze bewerking](#example-responses-1) biedt ook informatie over de manier waarop herstelservices identificeert een virtuele machine.  Als u bekend met het patroon bent, kunt u deze stap overslaan en rechtstreeks doorgaan naar [inschakelen van beveiliging](#enabling-protection-for-the-azure-vm).

Met deze bewerking wordt een *ophalen* bewerking.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupProtectableItems?api-version=2016-12-01&$filter=backupManagementType eq 'AzureIaasVM'
```

De *ophalen* URI heeft de vereiste parameters. Er zijn geen extra aanvraagtekst nodig is.

#### <a name="responses"></a>Antwoorden

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|200 OK     | [WorkloadProtectableItemResourceList](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list#workloadprotectableitemresourcelist)        |       OK |

##### <a name="example-responses"></a>Van de voorbeeldantwoorden

Zodra de *ophalen* aanvraag is ingediend, een 200 (OK) antwoord wordt geretourneerd.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-client-request-id: 40c8601a-c217-4c68-87da-01db8dac93dd; 40c8601a-c217-4c68-87da-01db8dac93dd
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14979
x-ms-correlation-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-routing-request-id: SOUTHINDIA:20180521T071408Z:7c2cf56a-e6be-4345-96df-c27ed849fe36
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:14:08 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainerv2;testRG;testVM/protectableItems/vm;iaasvmcontainerv2;testRG;testVM",
      "name": "iaasvmcontainerv2;testRG;testVM",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectableItems",
      "properties": {
        "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
        "virtualMachineVersion": "Compute",
        "resourceGroup": "testRG",
        "backupManagementType": "AzureIaasVM",
        "protectableItemType": "Microsoft.Compute/virtualMachines",
        "friendlyName": "testVM",
        "protectionState": "NotProtected"
      }
    },……………..

```

> [!TIP]
> Het aantal waarden in een *ophalen* antwoord is beperkt tot 200 voor een 'pagina'. Het veld 'nextLink' gebruiken om op te halen van de URL voor de volgende set van antwoorden.

Het antwoord bevat de lijst met alle niet-beveiligde Azure-VM's en elk `{value}` bevat alle gegevens die zijn vereist door de Azure Recovery-Service voor het configureren van back-up. Voor het configureren van back-up, houd er rekening mee de `{name}` veld en de `{virtualMachineId}` veld `{properties}` sectie. Twee variabelen uit deze waarden opgeven, zoals hieronder wordt beschreven.

- containerName = "iaasvmcontainer;" +`{name}`
- protectedItemName = 'vm'; + `{name}`
- `{virtualMachineId}` we later tijdens [hoofdtekst van de aanvraag](#example-request-body)

In het voorbeeld wordt de bovenstaande waarden vertalen naar:

- containerName = "iaasvmcontainer; iaasvmcontainerv2; testRG; testVM"
- protectedItemName = "vm; iaasvmcontainerv2; testRG; testVM"

### <a name="enabling-protection-for-the-azure-vm"></a>Inschakelen van beveiliging voor de Azure-VM

Nadat de relevante VM "in de cache opgeslagen" en "geïdentificeerd", selecteert u het beleid om te beveiligen. Raadpleeg voor meer informatie over bestaande beleidsregels in de kluis, [lijst beleid API](https://docs.microsoft.com/rest/api/backup/backuppolicies/list). Selecteer vervolgens de [relevante beleid](https://docs.microsoft.com/rest/api/backup/protectionpolicies/get) door te verwijzen naar de naam van het beleid. Raadpleeg voor het maken van beleid, [maken van beleid zelfstudie](backup-azure-arm-userestapi-createorupdatepolicy.md). "DefaultPolicy" is geselecteerd in het onderstaande voorbeeld.

Inschakelen van de beveiliging is een asynchrone *plaatsen* bewerking die een beveiligd item' maakt.

```http
https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2016-12-01
```

De `{containerName}` en `{protectedItemName}` zijn gebouwd boven. De `{fabricName}` 'Azure' is. In ons voorbeeld, dit wordt omgezet in:

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2016-12-01
```

#### <a name="create-the-request-body"></a>Hoofdtekst van de aanvraag maken

Hier volgen voor het maken van een beveiligd item, de onderdelen van de aanvraagtekst.

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|properties     | AzureIaaSVMProtectedItem        |ProtectedItem Resource-eigenschappen         |

Raadpleeg voor de volledige lijst met definities van de aanvraagtekst en andere details [beveiligd item REST-API-document maken](https://docs.microsoft.com/rest/api/backup/protecteditems/createorupdate#request-body).

##### <a name="example-request-body"></a>Voorbeeld van de aanvraag hoofdtekst

De hoofdtekst van de volgende aanvraag definieert de eigenschappen die zijn vereist voor het maken van een beveiligd item.

```json
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy"
  }
}
```

De `{sourceResourceId}` is de `{virtualMachineId}` hierboven is vermeld in de [respons van de lijst met beveiligbare items](#example-responses-1).

#### <a name="responses"></a>Antwoorden

Het maken van een beveiligd item is een [asynchrone bewerking](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Dit betekent dat deze bewerking wordt gemaakt van een andere bewerking waardoor moet afzonderlijk worden bijgehouden.

Deze twee antwoorden retourneert: 202 (aanvaard) wanneer een andere bewerking wordt gemaakt en klik vervolgens op 200 (OK) wanneer deze bewerking is voltooid.

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|200 OK     |    [ProtectedItemResource](https://docs.microsoft.com/rest/api/backup/protecteditemoperationresults/get#protecteditemresource)     |  OK       |
|202 geaccepteerd     |         |     Geaccepteerd    |

##### <a name="example-responses"></a>Van de voorbeeldantwoorden

Zodra u de *plaatsen* van de aanvraag voor het beveiligde item maken of bijwerken, de eerste reactie is 202 (aanvaard) met een location-header of de Azure-async-header.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
X-Powered-By: ASP.NET
```

De resulterende bewerking met de location-header of de Azure-AsyncOperation koptekst met een eenvoudige vervolgens bijhouden *ophalen* opdracht.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
```

Nadat de bewerking is voltooid, wordt 200 (OK) geretourneerd met de inhoud van het beveiligde item in de hoofdtekst van de reactie.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM",
  "name": "VM;testRG;testVM",
  "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems",
  "properties": {
    "friendlyName": "testVM",
    "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "protectionStatus": "Healthy",
    "protectionState": "IRPending",
    "healthStatus": "Passed",
    "lastBackupStatus": "",
    "lastBackupTime": "2001-01-01T00:00:00Z",
    "protectedItemDataId": "17592691116891",
    "extendedInfo": {
      "recoveryPointCount": 0,
      "policyInconsistent": false
    },
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "backupManagementType": "AzureIaasVM",
    "workloadType": "VM",
    "containerName": "iaasvmcontainerv2;testRG;testVM",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy",
    "policyName": "DefaultPolicy"
  }
}
```

Hiermee bevestigt u dat de beveiliging is ingeschakeld voor de virtuele machine en de eerste back-up volgens het beleidsschema wordt geactiveerd.

## <a name="trigger-an-on-demand-backup-for-a-protected-azure-vm"></a>Trigger een on-demand back-up voor een beveiligde virtuele machine van Azure

Nadat een Azure-VM is geconfigureerd voor back-ups, back-ups plaatsvinden volgens de planning van het beleid. U kunt wachten op de eerste geplande back-up of een on-demand back-up op elk gewenst moment activeren. Bewaarperiode voor back-ups op aanvraag is gescheiden van de bewaarperiode voor back-upbeleid en kan worden opgegeven voor een bepaalde datum en tijd. Indien niet opgegeven, wordt uitgegaan van 30 dagen van de dag van de trigger van de back-up op aanvraag.

Activeren van een on-demand back-up is een *POST* bewerking.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/backup?api-version=2016-12-01
```

De `{containerName}` en `{protectedItemName}` zijn samengesteld [hierboven](#responses-1). De `{fabricName}` 'Azure' is. In ons voorbeeld, dit wordt omgezet in:

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM/backup?api-version=2016-12-01
```

### <a name="create-the-request-body"></a>Hoofdtekst van de aanvraag maken

Voor het activeren van een on-demand back-up, vindt hieronder u de onderdelen van de aanvraagtekst.

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|properties     | [IaaSVMBackupRequest](https://docs.microsoft.com/rest/api/backup/backups/trigger#iaasvmbackuprequest)        |BackupRequestResource eigenschappen         |

Raadpleeg voor de volledige lijst met definities van de aanvraagtekst en andere details [back-ups voor beveiligde items REST API-document activeren](https://docs.microsoft.com/rest/api/backup/backups/trigger#request-body).

#### <a name="example-request-body"></a>Voorbeeld van de aanvraag hoofdtekst

De hoofdtekst van de volgende aanvraag definieert de eigenschappen die zijn vereist voor het activeren van een back-up voor een beveiligd item. Als de bewaarperiode niet opgegeven is, wordt wel worden bewaard gedurende 30 dagen vanaf het moment van de trigger van de back-uptaak.

```json
{
   "properties": {
    "objectType": "IaasVMBackupRequest",
    "recoveryPointExpiryTimeInUTC": "2018-12-01T02:16:20.3156909Z"
  }
}
```

### <a name="responses"></a>Antwoorden

Activeren van een on-demand back-up is een [asynchrone bewerking](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Dit betekent dat deze bewerking wordt gemaakt van een andere bewerking waardoor moet afzonderlijk worden bijgehouden.

Deze twee antwoorden retourneert: 202 (aanvaard) wanneer een andere bewerking wordt gemaakt en klik vervolgens op 200 (OK) wanneer deze bewerking is voltooid.

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|202 geaccepteerd     |         |     Geaccepteerd    |

#### <a name="example-responses"></a>Van de voorbeeldantwoorden

Zodra u de *POST* van de aanvraag voor een on-demand back-up, het oorspronkelijke antwoord is 202 (aanvaard) met een location-header of de Azure-async-header.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2016-12-01
X-Content-Type-Options: nosniff
x-ms-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-client-request-id: 7df8e874-1d66-4f81-8e91-da2fe054811d; 7df8e874-1d66-4f81-8e91-da2fe054811d
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-routing-request-id: SOUTHINDIA:20180521T083541Z:7885ca75-c7c6-43fb-a38c-c0cc437d8810
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:35:41 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationResults/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2016-12-01
X-Powered-By: ASP.NET
```

De resulterende bewerking met de location-header of de Azure-AsyncOperation koptekst met een eenvoudige vervolgens bijhouden *ophalen* opdracht.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
```

Nadat de bewerking is voltooid, wordt 200 (OK) geretourneerd met de ID van de resulterende back-uptaak in de hoofdtekst van de reactie.

```json
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-client-request-id: 5a63209d-3708-4e69-a75f-9499f4c8977c; 5a63209d-3708-4e69-a75f-9499f4c8977c
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14995
x-ms-correlation-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-routing-request-id: SOUTHINDIA:20180521T083723Z:a8b13524-2c95-445f-b107-920806f385c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:37:22 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "00000000-0000-0000-0000-000000000000",
  "status": "Succeeded",
  "startTime": "2018-05-21T08:35:40.9488967Z",
  "endTime": "2018-05-21T08:35:40.9488967Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "7ddead57-bcb9-4269-ac31-6a1b57588700"
  }
}
```

Omdat de back-uptaak een langdurige bewerking is, deze moet worden bijgehouden, zoals wordt beschreven de [bewaken van taken met behulp van REST-API-document](backup-azure-arm-userestapi-managejobs.md#tracking-the-job).

## <a name="modify-the-backup-configuration-for-a-protected-azure-vm"></a>De back-upconfiguratie voor een beveiligde Azure-virtuele machine wijzigen

### <a name="changing-the-policy-of-protection"></a>Wijzigen van het beleid van beveiliging

Als u wilt wijzigen van het beleid waarmee de virtuele machine is beveiligd, kunt u dezelfde indeling als [inschakelen van beveiliging](#enabling-protection-for-the-azure-vm). Geeft u alleen de nieuwe beleids-ID in [hoofdtekst van de aanvraag](#example-request-body) en dien de aanvraag. Voor bijvoorbeeld: als u wilt wijzigen in het beleid van testVM van 'DefaultPolicy' naar 'ProdPolicy', geef de id 'ProdPolicy' in de aanvraagtekst.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/ProdPolicy"
  }
}
```

Het antwoord volgt dezelfde indeling zoals vermeld [voor het inschakelen van beveiliging](#responses-2)

### <a name="stop-protection-but-retain-existing-data"></a>Stop de beveiliging, maar bestaande gegevens behouden

Als u wilt beveiliging op een beveiligde virtuele machine verwijderen, maar de gegevens die al een back-up behouden, verwijder het beleid in de aanvraagtekst en dien de aanvraag. Nadat de koppeling met het beleid wordt verwijderd, back-ups niet meer worden geactiveerd en er zijn geen nieuwe herstelpunten worden gemaakt.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": ""
  }
}
```

Het antwoord volgt dezelfde indeling zoals vermeld [voor het activeren van een on-demand back-up](#example-responses-3). De resulterende taak moet worden bijgehouden, zoals wordt beschreven de [bewaken van taken met behulp van REST-API-document](backup-azure-arm-userestapi-managejobs.md#tracking-the-job).

### <a name="stop-protection-and-delete-data"></a>Beveiliging beëindigen en gegevens verwijderen

Voor het verwijderen van de beveiliging op een beveiligde virtuele machine en ook de back-upgegevens verwijderen, voeren een verwijderbewerking als gedetailleerde [hier](https://docs.microsoft.com/rest/api/backup/protecteditems/delete).

Beveiliging stoppen en verwijderen van gegevens is een *verwijderen* bewerking.

```http
DELETE https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2016-12-01
```

De `{containerName}` en `{protectedItemName}` zijn samengesteld [hierboven](#responses-1). `{fabricName}` 'Azure' is. In ons voorbeeld, dit wordt omgezet in:

```http
DELETE https://management.azure.com//Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2016-12-01
```

### <a name="responses"></a>Antwoorden

*Verwijder* beveiliging is een [asynchrone bewerking](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Dit betekent dat deze bewerking wordt gemaakt van een andere bewerking waardoor moet afzonderlijk worden bijgehouden.

Deze twee antwoorden retourneert: 202 (aanvaard) wanneer een andere bewerking wordt gemaakt en vervolgens 204 (NoContent) wanneer deze bewerking is voltooid.

|Naam  |Type  |Beschrijving  |
|---------|---------|---------|
|204 NoContent     |         |  NoContent       |
|202 geaccepteerd     |         |     Geaccepteerd    |

## <a name="next-steps"></a>Volgende stappen

[Gegevens terugzetten vanaf een virtueel Azure-machine back-up](backup-azure-arm-userestapi-restoreazurevms.md).

Raadpleeg de volgende documenten voor meer informatie over de REST API's van Azure back-up:

- [Azure Recovery Services-provider REST-API](/rest/api/recoveryservices/)
- [Aan de slag met REST API van Azure](/rest/api/azure/)
