---
title: Ontwikkelen voor Azure NetApp-bestanden met de REST-API | Microsoft Docs
description: Beschrijft hoe u aan de slag met het gebruik van de REST-API van Azure NetApp bestanden.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: b-juche
ms.openlocfilehash: 56667b9a47411b2abae30ff159fa6bc555fec070
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58104621"
---
# <a name="develop-for-azure-netapp-files-with-rest-api"></a>Ontwikkelen voor Azure NetApp-bestanden met de REST-API 

De REST-API voor de service Azure NetApp Files definieert HTTP-bewerkingen op resources, zoals de NetApp-account, de capaciteit van toepassingen, de volumes en momentopnamen. Dit artikel helpt u aan de slag met het gebruik van de REST-API van Azure NetApp bestanden.

## <a name="access-the-azure-netapp-files-rest-api"></a>Toegang tot de bestanden Azure NetApp REST-API  

1. [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) als u nog niet hebt gedaan.
2. Maak een service-principal in uw Azure Active Directory (Azure AD):
   1. Controleer of u [voldoende machtigingen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions).

   1. Voer de volgende opdracht in Azure CLI in:  

           az ad sp create-for-rbac --name $YOURSPNAMEGOESHERE--password $YOURGENERATEDPASSWORDGOESHERE

      Uitvoer van de opdracht is vergelijkbaar met het volgende voorbeeld:  

           { 
               "appId": "appIDgoeshere", 
               "displayName": "APPNAME", 
               "name": "http://APPNAME", 
               "password": "supersecretpassword", 
               "tenant": "tenantIDgoeshere" 
           } 

      Houd de uitvoer van de opdracht.  U moet de `appId`, `password`, en `tenant` waarden. 

3. Een OAuth-toegangstoken aanvragen:

    De voorbeelden in dit artikel gebruiken we cURL.  U kunt ook verschillende API-hulpprogramma's zoals gebruiken [Postman](https://www.getpostman.com/), [Slapeloosheid](https://insomnia.rest/), en [Paw](https://paw.cloud/).  

    Vervang de variabelen in het volgende voorbeeld door de uitvoer van de opdracht in stap 2 hierboven. 

        curl -X POST -d 'grant_type=client_credentials&client_id=[APP_ID]&client_secret=[PASSWORD]&resource=https%3A%2F%2Fmanagement.azure.com%2F' https://login.microsoftonline.com/[TENANT_ID]/oauth2/token

    De uitvoer bevat een toegangstoken die vergelijkbaar is met het volgende voorbeeld:

        eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSIsImtpZCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSJ9

    Het weergegeven token is geldig 3600 seconden. Hierna moet u een nieuw token aanvragen. 
    Sla het token naar een teksteditor.  U moet deze voor de volgende stap.

4. Verzenden van een aanroep van de test en de token voor het valideren van de toegang tot de REST-API opnemen:

        curl -X GET -H "Authorization: Bearer [TOKEN]" -H "Content-Type: application/json" https://management.azure.com/subscriptions/[SUBSCRIPTION_ID]/providers/Microsoft.Web/sites?api-version=2016-08-01

## <a name="examples-using-the-api"></a>Voorbeelden van het gebruik van de API  

In dit artikel wordt de volgende URL voor de basislijn van aanvragen. Deze URL verwijst naar de hoofdmap van de naamruimte Azure NetApp bestanden. 

`https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2017-08-15`

U moet vervangen de `subID` en `resourceGroups` waarden in de volgende voorbeelden door uw eigen waarden. 

### <a name="get-request-examples"></a>Voorbeelden van de aanvraag ophalen

U een GET-aanvraag voor queryobjecten van het Azure NetApp bestanden in een abonnement, zoals de volgende voorbeelden gebruiken: 

        #get NetApp accounts 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2017-08-15

        #get capacity pools for NetApp account 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools?api-version=2017-08-15

        #get volumes in NetApp account & capacity pool 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes?api-version=2017-08-15

        #get snapshots for a volume 
        curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/VOLUMEGOESHERE/snapshots?api-version=2017-08-15

### <a name="put-request-examples"></a>Voorbeelden van de aanvraag voor plaatsen

Een PUT-aanvraag kunt u nieuwe objecten maken in Azure NetApp bestanden, zoals de volgende voorbeelden. De hoofdtekst van de PUT-aanvraag kan de gegevens van de JSON-indeling voor de wijzigingen bevatten, of deze te lezen uit een bestand kunt opgeven. 

        #create a NetApp account  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE?api-version=2017-08-15

        #create a capacity pool  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE?api-version=2017-08-15

        #create a volume  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME?api-version=2017-08-15

        #create a volume snapshot  
        curl -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME/Snapshots/SNAPNAME?api-version=2017-08-15

### <a name="json-examples"></a>JSON-voorbeelden

Het volgende voorbeeld laat zien hoe een NetApp-account maken:

    { 
        "name": "MYNETAPPACCOUNT", 
        "type": "Microsoft.NetApp/netAppAccounts", 
        "location": "westus2", 
        "properties": { 
            "name": "MYNETAPPACCOUNT" 
        }
    } 

Het volgende voorbeeld laat zien hoe een capaciteit van toepassingen maken: 

    {
        "name": "MYNETAPPACCOUNT/POOLNAME",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools",
        "location": "westus2",
        "properties": {
            "name": "POOLNAME"
            "size": "4398046511104",
            "serviceLevel": "Premium"
        }
    }

Het volgende voorbeeld laat zien hoe het maken van een nieuw volume: 

    {
        "name": "MYNEWVOLUME",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools/volumes",
        "location": "westus2",
        "properties": {
            "serviceLevel": "Premium",
            "usageThreshold": "322122547200",
            "creationToken": "MY-FILEPATH",
            "snapshotId": "",
            "subnetId": "/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.Network/virtualNetworks/VNETGOESHERE/subnets/MYDELEGATEDSUBNET.sn"
            }
    }

Het volgende voorbeeld laat zien hoe een momentopname van een volume maken: 

    {
        "name": "apitest2/apiPool01/apiVol01/snap02",
        "type": "Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots",
        "location": "westus2",
        "properties": {
            "name": "snap02",
            "fileSystemId": "0168704a-bbec-da81-2c29-503825fe7420"
        }
    }

> [!NOTE] 
> U moet opgeven `fileSystemId` voor het maken van een momentopname.  U vindt de `fileSystemId` waarde met een GET-aanvraag naar een volume. 

## <a name="next-steps"></a>Volgende stappen

[Zie de Azure NetApp Files REST API-naslaginformatie](https://docs.microsoft.com/rest/api/netapp/)
