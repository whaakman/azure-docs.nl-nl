---
title: Problemen met virtuele netwerkgateway en verbindingen met behulp van Azure-netwerk-Watcher - REST | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u problemen met virtuele netwerkgateways en verbindingen met Azure met behulp van REST netwerk-Watcher
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: e4d5f195-b839-4394-94ef-a04192766e55
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 086a853d0849ee22f992c9d3265f6988bcc7bd83
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a>Problemen met virtuele netwerkgateway en verbindingen met behulp van Azure-netwerk-Watcher

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Netwerk-Watcher biedt veel mogelijkheden met betrekking tot het begrijpen van uw netwerkbronnen in Azure. Een van deze mogelijkheden resource is het oplossen van problemen. Het oplossen van bron kan worden aangeroepen via de portal, PowerShell, CLI of REST-API. Als deze wordt aangeroepen, netwerk-Watcher inspecteert de status van de Gateway van een virtueel netwerk of een verbinding en retourneert de bevindingen zijn.

In dit artikel leert u de verschillende beheertaken die momenteel beschikbaar voor het oplossen van resource zijn.

- [**Problemen met een virtuele netwerkgateway**](#troubleshoot-a-virtual-network-gateway)
- [**Problemen met een verbinding**](#troubleshoot-connections)

## <a name="before-you-begin"></a>Voordat u begint

ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

Voor een lijst met ondersteunde gateway typen bezoek, [ondersteund gatewaytypen](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Overzicht

Netwerk-Watcher Probleemoplossing biedt de mogelijkheid oplossen van problemen die zich bij het virtuele netwerk gateways en verbindingen voordoen. Wanneer een aanvraag wordt gedaan aan de resource die het oplossen van problemen, worden de logboeken query wordt uitgevoerd en gecontroleerd. Als controle voltooid is, worden de resultaten worden geretourneerd. De problemen met API-aanvragen zijn lang aanvragen die een resultaat te retourneren van meerdere minuten duren voordat het kunnen worden uitgevoerd. Logboeken worden opgeslagen in een container voor een opslagaccount.

## <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

```PowerShell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a>Problemen met een virtuele netwerkgateway


### <a name="post-the-troubleshoot-request"></a>NA de aanvraag oplossen

Het volgende voorbeeld wordt de status van een virtuele netwerkgateway opgevraagd.

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$vnetGatewayName = "ContosoVNETGateway"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @"
{
'TargetResourceId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/virtualNetworkGateways/${vnetGatewayName}',
'Properties': {
'StorageId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}',
'StoragePath': 'https://${storageAccountName}.blob.core.windows.net/${containerName}'
}
}
"@


armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30" -verbose
```

Omdat deze bewerking lang is uitgevoerd, de URI voor het opvragen van de bewerking en de URI voor het resultaat wordt geretourneerd als de antwoordheader zoals weergegeven in het volgende antwoord:

**Belangrijke waarden**

* **Azure-asynchrone bewerking** -deze eigenschap bevat de URI moet een query de asynchrone bewerking oplossen
* **Locatie** -deze eigenschap bevat de URI waar de resultaten zijn wanneer de bewerking voltooid is

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-the-async-operation-for-completion"></a>Query uitvoeren op de asynchrone bewerking voor voltooiing

Gebruik de bewerkingen URI query voor de voortgang van de bewerking zoals te zien is in het volgende voorbeeld:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

Terwijl de bewerking uitgevoerd wordt, ziet u het antwoord **InProgress** zoals te zien is in het volgende voorbeeld:

```json
{
  "status": "InProgress"
}
```

Als de bewerking voltooid is de status gewijzigd in **geslaagd**.

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-the-results"></a>De resultaten ophalen

Zodra de status wordt geretourneerd is **geslaagd**, een GET-methode aanroepen voor de operationResult URI voor het ophalen van de resultaten.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

De volgende antwoorden zijn voorbeelden van een typische verslechterde antwoord geretourneerd bij het opvragen van de resultaten van het oplossen van een gateway. Zie [inzicht in de resultaten](#understanding-the-results) ophalen van informatie over de betekenis van de eigenschappen in het antwoord.

```json
{
  "startTime": "2017-01-12T10:31:41.562646-08:00",
  "endTime": "2017-01-12T18:31:48.677Z",
  "code": "Degraded",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN Gateway"
        },
        {
          "actionText": "If your VPN gateway isn't up and running by the expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN gateway is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```


## <a name="troubleshoot-connections"></a>Problemen met verbindingen oplossen

Het volgende voorbeeld wordt de status van een verbinding opgevraagd.

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$connectionName = "VNET2toVNET1Connection"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @{
"TargetResourceId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/connections/${connectionName}",
"Properties": {
"StorageId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}",
"StoragePath": "https://${storageAccountName}.blob.core.windows.net/${containerName}"
}

}
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 "
```

> [!NOTE]
> De bewerking oplossen kan niet op een verbinding en de bijbehorende gateways parallel worden uitgevoerd. De bewerking moet voltooien voordat u deze uitvoert op de vorige resource.

Omdat dit een langdurige transactie, in de kop response, wordt de URI voor query's in de bewerking en de URI voor het resultaat zoals weergegeven in het volgende antwoord geretourneerd:

**Belangrijke waarden**

* **Azure-asynchrone bewerking** -deze eigenschap bevat de URI moet een query de asynchrone bewerking oplossen
* **Locatie** -deze eigenschap bevat de URI waar de resultaten zijn wanneer de bewerking voltooid is

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-the-async-operation-for-completion"></a>Query uitvoeren op de asynchrone bewerking voor voltooiing

Gebruik de bewerkingen URI query voor de voortgang van de bewerking zoals te zien is in het volgende voorbeeld:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

Terwijl de bewerking uitgevoerd wordt, ziet u het antwoord **InProgress** zoals te zien is in het volgende voorbeeld:

```json
{
  "status": "InProgress"
}
```

Wanneer de bewerking voltooid is, wordt de status gewijzigd in **geslaagd**.

```json
{
  "status": "Succeeded"
}
```

De volgende antwoorden zijn voorbeelden van een typische antwoord geretourneerd bij het opvragen van de resultaten van het oplossen van een verbinding.

### <a name="retrieve-the-results"></a>De resultaten ophalen

Zodra de status wordt geretourneerd is **geslaagd**, een GET-methode aanroepen voor de operationResult URI voor het ophalen van de resultaten.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

De volgende antwoorden zijn voorbeelden van een typische antwoord geretourneerd bij het opvragen van de resultaten van het oplossen van een verbinding.

```json
{
  "startTime": "2017-01-12T14:09:19.1215346-08:00",
  "endTime": "2017-01-12T22:09:23.747Z",
  "code": "UnHealthy",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This 
is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN gateway"
        },
        {
          "actionText": "If your VPN Connection isn't up and running by the expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN Connection is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```

## <a name="understanding-the-results"></a>Inzicht in de resultaten

De actietekst bevat algemene richtlijnen over het oplossen van het probleem. Als een actie kan worden uitgevoerd voor de uitgifte, wordt een koppeling geboden met aanvullende richtlijnen. In het geval wanneer er geen aanvullende richtlijnen, het antwoord geeft de url om een ondersteuningsaanvraag te openen.  Voor meer informatie over de eigenschappen van het antwoord en wat is opgenomen, gaat u naar [overzicht van netwerk-Watcher oplossen](network-watcher-troubleshoot-overview.md)

Zie voor instructies over het downloaden van bestanden van azure storage-accounts, [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een ander hulpprogramma dat kan worden gebruikt, is Storage Explorer. Meer informatie over Opslagverkenner vindt u hier op de volgende koppeling: [Opslagverkenner](http://storageexplorer.com/)

## <a name="next-steps"></a>Volgende stappen

Als de instellingen zijn gewijzigd die stop VPN-verbindingen, raadpleegt u [Netwerkbeveiligingsgroepen beheren](../virtual-network/virtual-network-manage-nsg-arm-portal.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die betrokken zijn.
