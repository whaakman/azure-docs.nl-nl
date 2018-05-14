---
title: Compatibiliteit met Security Center-beleid met de REST API van Azure controleren | Microsoft Docs
description: Informatie over het gebruik van Azure REST API's om te controleren van de huidige compatibiliteit met Security Center-beleid.
services: security-center
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: alleonar
ms.openlocfilehash: 6c6764eec59633f0bdd0fa396c1581117a0c1e1d
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="review-security-center-policy-compliance-using-rest-apis"></a>Compatibiliteit controleren Security Center-beleid met REST API 's

Security Center valideert periodiek uw Azure-resources op basis van uw beveiligingsbeleid gedefinieerde. Security Center biedt ook een REST-API waarmee u het controleren van naleving van uw eigen toepassingen. u kunt de service rechtstreeks een query of importeren van JSON-resultaten in andere toepassingen. 

Hier kunt u meer informatie op te halen van de huidige set met aanbevelingen uit alle Azure-resources die zijn gekoppeld aan een abonnement.

Voor het ophalen van de huidige set met aanbevelingen:
``` http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/tasks?api-version={api-version}
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>De aanvraag voor het samenstellen  

De `{subscription-id}` parameter is vereist en de abonnements-ID voor het definiëren van de beleidsregels voor Azure-abonnement moet bevatten. Als u meerdere abonnementen hebt, raadpleegt u [werken met meerdere abonnementen](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions).  

De `api-version` parameter is vereist. Op dit moment deze eindpunten worden alleen ondersteund voor `api-version=2015-06-01-preview`. 

De volgende headers zijn vereist: 

|Aanvraag-header|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|  
|*Autorisatie:*|Vereist. Ingesteld op een geldige `Bearer` [toegangstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |  

## <a name="response"></a>Antwoord  

Statuscode 200 wordt (OK) geretourneerd voor een geslaagde reactie die een lijst met aanbevolen taken voor het beveiligen van uw Azure-resources bevat.

``` json
{  
  "value": [  
    {  
       "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{region}/tasks/{task-id}",
       "name": "{task_id}",
       "type": "Microsoft.Security/locations/{region}/tasks",
       "properties": {
       "state": "Active",
       "subState": "NA",
       "creationTimeUtc": "{create-time}",
       "lastStateChangeTimeUtc": "{last-state-change}",
       "securityTaskParameters": "{security-task-properties}"
    } 
  ]  
}  
```  

Elk item in **waarde** vertegenwoordigt een aanbeveling:

|Eigenschap Response|Beschrijving|
|----------------|----------|
|**Status** | Hiermee wordt aangegeven of aanbeveling `active` of `resolved`. |
|**CreationTimeUtc** | Datum en tijd in UTC, wordt weergegeven wanneer de aanbeveling is gemaakt. |
|**lastStateChangeUtc** | Datum en tijd in UTV van de laatste statuswijziging, indien van toepassing. |
|**securityTaskParameters** | Details van de aanbeveling; eigenschappen variëren afhankelijk van de onderliggende aanbeveling. |
||
  
Zie voor ondersteunde aanbevelingen [beveiligingsaanbevelingen implementeren](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Andere statuscodes duiden op fouten. In dergelijke gevallen bevat het response-object een beschrijving toe waarin wordt uitgelegd waarom de aanvraag is mislukt.

``` json
{  
  "value": [  
    {  
      "description": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="example-response"></a>Voorbeeld van een antwoord  

``` json
{  
  "value": [  
        {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{region}/tasks/{task-id}",
            "name": "{task_id}",
            "type": "Microsoft.Security/locations/{region}/tasks",
            "properties": {
                "state": "Active",
                "subState": "NA",
                "creationTimeUtc": "{create-time}",
                "lastStateChangeTimeUtc": "{last-state-change}",
                "securityTaskParameters": {
                    "vmId": "/subscriptions/{subscription-id}/resourceGroups/{resource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}",
                    "vmName": "{vm_name}",
                    "severity": "{severity}",
                    "isOsDiskEncrypted": {is_os_disk_encrypted},
                    "isDataDiskEncrypted": {is_data_disk_encrypted},
                    "name": "EncryptionOnVm",
                    "uniqueKey": "EncryptionOnVmTaskParameters_/subscriptions/{subscription-id}/resourceGroups/{resoource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}",
                    "resourceId": "/subscriptions/{subscription_id}/resourceGroups/{resource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}"
                }
            }
        },
        {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{location}/tasks/{task-id}",
            "name": "{task-id}",
            "type": "Microsoft.Security/locations/{region}/tasks",
            "properties": {
                "state": "Active",
                "subState": "NA",
                "creationTimeUtc": "{create-time}",
                "lastStateChangeTimeUtc": "{last-state-change}",
                "securityTaskParameters": {
                    "serverName": "{sql-server-name}",
                    "serverId": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}",
                    "name": "Enable auditing for the SQL server",
                    "uniqueKey": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}/auditingPolicies/Default",
                    "resourceId": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}"
                }
            }
        }  ]  
}  
```  

Dit antwoord ziet u twee aanbevelingen; elk item in de lijst overeenkomt met een specifieke aanbeveling. De eerste aanbeveelt voor het versleutelen van opslag op een virtuele Linux-machine en de tweede wordt aangeraden dat u controle inschakelen voor een SQL-server.

De aanbevelingen zijn afhankelijk van de beleidsregels die u hebt ingeschakeld. Zie voor meer informatie, met inbegrip van de momenteel beschikbare aanbevelingen [aanbevelingen voor beveiliging in Azure Security Center beheren](https://docs.microsoft.com/azure/security-center/security-center-recommendations).


## <a name="see-also"></a>Zie ook  
- [Beveiligingsbeleid instellen](https://docs.microsoft.com/azure/security-center/security-center-policies-overview)
- [Azure Security Resource provider REST-API](https://msdn.microsoft.com/library/azure/mt704034.aspx)   
- [Aan de slag met Azure REST-API](https://docs.microsoft.com/rest/api/azure/)   
- [Azure Security Center PowerShell-module](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.22)
