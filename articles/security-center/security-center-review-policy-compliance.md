---
title: Controleren van naleving van het beleid met REST API van Azure Security Center | Microsoft Docs
description: Informatie over het gebruik van Azure REST API's om te controleren van de huidige compatibiliteit met Security Center-beleid.
services: security-center
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: alleonar
ms.openlocfilehash: 1443486590859aac5591aff2ab0551bed9228d7b
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301746"
---
# <a name="review-security-center-policy-compliance-using-rest-apis"></a>Naleving van de beoordeling Security Center-beleid met behulp van REST-API 's

Security Center valideert periodiek uw Azure-resources op basis van het gedefinieerde beveiligingsbeleid. Security Center biedt ook een REST-API waarmee u Controleer naleving van uw eigen toepassingen. u kunt de service rechtstreeks query of JSON-resultaten importeren in andere toepassingen. 

Hier leert u de huidige set met aanbevelingen uit alle Azure-resources die zijn gekoppeld aan een abonnement ophalen.

De huidige set met aanbevelingen ophalen:
``` http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/tasks?api-version={api-version}
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>De aanvraag voor het samenstellen  

De `{subscription-id}` -parameter is vereist en de abonnements-ID voor het definiëren van het beleid Azure-abonnement moet bevatten. Als u meerdere abonnementen hebt, raadpleegt u [werken met meerdere abonnementen](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions).  

De `api-version` parameter is vereist. Op dit moment is deze eindpunten worden alleen ondersteund voor `api-version=2015-06-01-preview`. 

De volgende headers zijn vereist: 

|Aanvraagheader|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|  
|*Autorisatie:*|Vereist. Ingesteld op een geldige `Bearer` [toegangstoken](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |  

## <a name="response"></a>Reactie  

Statuscode 200 wordt (OK) geretourneerd voor een geslaagde respons, die een lijst met aanbevolen taken voor het beveiligen van uw Azure-resources bevat.

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

|Antwoord-eigenschap|Beschrijving|
|----------------|----------|
|**status** | Geeft aan of de aanbeveling `active` of `resolved`. |
|**CreationTimeUtc** | De datum en tijd in UTC, weergegeven waarop de aanbeveling is gemaakt. |
|**lastStateChangeUtc** | De datum en tijd in UTV van de laatste statuswijziging, indien van toepassing. |
|**securityTaskParameters** | Details van de aanbeveling; eigenschappen, afhankelijk van de onderliggende aanbeveling. |
||
  
Zie voor de momenteel ondersteunde aanbevelingen [aanbevelingen voor beveiliging implementeren](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Andere statuscodes duiden op fouten. In dergelijke gevallen bevat het antwoordobject voor een beschrijving waarin wordt uitgelegd waarom de aanvraag is mislukt.

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

Dit antwoord bevat twee aanbevelingen; elk item in de lijst komt overeen met een specifieke aanbeveling. De eerste raadt aan om opslag op een virtuele Linux-machine versleutelen en de tweede kan erop wijzen dat u controle inschakelen voor een SQL-server.

De aanbevelingen afhankelijk van de beleidsregels die u hebt ingeschakeld. Zie voor meer informatie, met inbegrip van de momenteel beschikbare aanbevelingen [aanbevelingen voor beveiliging in Azure Security Center beheren](https://docs.microsoft.com/azure/security-center/security-center-recommendations).


## <a name="see-also"></a>Zie ook  
- [Beveiligingsbeleid instellen](https://docs.microsoft.com/azure/security-center/security-center-policies-overview)
- [Azure Security Resource provider REST-API](https://msdn.microsoft.com/library/azure/mt704034.aspx)   
- [Aan de slag met REST API van Azure](https://docs.microsoft.com/rest/api/azure/)   
- [PowerShell-module voor Azure Security Center](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.22)
