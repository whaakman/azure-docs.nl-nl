---
title: Problemen oplossen met de Gateway van virtueel netwerk en verbindingen met behulp van Azure Network Watcher - REST | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u problemen oplossen met Gateways van virtueel netwerk en verbindingen met Azure Network Watcher met behulp van REST
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: e4d5f195-b839-4394-94ef-a04192766e55
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 2c65b595e4c56b8c69d38a6e2f30ae8e57bba3f4
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344101"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a>Problemen oplossen met de gateway van Virtueelnetwerk en verbindingen met behulp van Azure Network Watcher

> [!div class="op_single_selector"]
> - [Portal](diagnose-communication-problem-between-networks.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Network Watcher biedt veel mogelijkheden met betrekking tot het begrijpen van de netwerkresources van uw in Azure. Een van deze mogelijkheden is resource-oplossen van problemen. Het oplossen van resource kan worden aangeroepen via de portal, PowerShell, CLI of REST-API. Indien aangeroepen, wordt Network Watcher inspecteert de status van de Gateway van een virtueel netwerk of een verbinding en retourneert de bevindingen.

In dit artikel doorloopt u de verschillende beheertaken die momenteel beschikbaar voor het oplossen van de resource zijn.

- [**Oplossen van een virtuele netwerkgateway**](#troubleshoot-a-virtual-network-gateway)
- [**Problemen met een verbinding oplossen**](#troubleshoot-connections)

## <a name="before-you-begin"></a>Voordat u begint

ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

In dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een Network Watcher](network-watcher-create.md) te maken van een Network Watcher.

Voor een lijst van ondersteunde gateway typen bezoek, [ondersteund gatewaytypen](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Overzicht

Network Watcher troubleshooting biedt de mogelijkheid voor het oplossen van problemen die met Virtual Network-gateways en verbindingen optreden. Wanneer een aanvraag wordt gedaan bij het oplossen van problemen resource, worden de logboeken wilt zoeken en geïnspecteerd. Als controle voltooid is, worden de resultaten worden geretourneerd. De problemen met API-aanvragen zijn lang actieve aanvragen, die kunnen meerdere minuten om een resultaat te retourneren. Logboeken worden opgeslagen in een container op een storage-account.

## <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

```PowerShell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a>Oplossen van een virtuele netwerkgateway


### <a name="post-the-troubleshoot-request"></a>NA de aanvraag voor problemen oplossen

Het volgende voorbeeld wordt de status van een virtuele netwerkgateway opvragen.

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


armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30" $requestBody -verbose
```

Omdat deze bewerking lang is uitgevoerd, de URI voor het uitvoeren van query's de bewerking en de URI voor het resultaat wordt geretourneerd in de antwoordkop zoals wordt weergegeven in het volgende antwoord:

**Belangrijke waarden**

* **Azure-AsyncOperation** -deze eigenschap bevat de URI aan query de asynchrone bewerking oplossen
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

Gebruik de bewerkingen URI aan query voor de voortgang van de bewerking, zoals te zien is in het volgende voorbeeld:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

Terwijl de bewerking uitgevoerd wordt, ziet u het antwoord **InProgress** zoals te zien is in het volgende voorbeeld:

```json
{
  "status": "InProgress"
}
```

Wanneer de bewerking voltooid is wordt de status gewijzigd in **geslaagd**.

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-the-results"></a>De resultaten ophalen

Zodra de status wordt geretourneerd **geslaagd**, een GET-methode aanroepen op kan operationresult niet URI om de resultaten te halen.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30" -verbose
```

De volgende antwoorden zijn voorbeelden van een typische gedegradeerde antwoord geretourneerd bij het opvragen van de resultaten van het oplossen van een gateway. Zie [inzicht in de resultaten](#understanding-the-results) uitleg over de betekenis van de eigenschappen in het antwoord ophalen.

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
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
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
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
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

Het volgende voorbeeld vraagt de status van een verbinding.

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
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 $requestBody"
```

> [!NOTE]
> De problemen oplossen-bewerking kan niet parallel worden uitgevoerd op een verbinding en de bijbehorende gateways. De bewerking moet uitvoeren voordat u deze uitvoert op de vorige resource.

Aangezien dit een langlopende transactie, in de antwoordkop, wordt de URI voor het uitvoeren van query's de bewerking en de URI voor het resultaat zoals in het volgende antwoord geretourneerd:

**Belangrijke waarden**

* **Azure-AsyncOperation** -deze eigenschap bevat de URI aan query de asynchrone bewerking oplossen
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

Gebruik de bewerkingen URI aan query voor de voortgang van de bewerking, zoals te zien is in het volgende voorbeeld:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

Terwijl de bewerking uitgevoerd wordt, ziet u het antwoord **InProgress** zoals te zien is in het volgende voorbeeld:

```json
{
  "status": "InProgress"
}
```

Wanneer de bewerking voltooid is, verandert de status in **geslaagd**.

```json
{
  "status": "Succeeded"
}
```

De volgende antwoorden zijn voorbeelden van een typische antwoord geretourneerd bij het opvragen van de resultaten van het oplossen van een verbinding.

### <a name="retrieve-the-results"></a>De resultaten ophalen

Zodra de status wordt geretourneerd **geslaagd**, een GET-methode aanroepen op kan operationresult niet URI om de resultaten te halen.

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
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
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
          "actionUri": "https://azure.microsoft.com/documentation/articles/vpn-gateway-resetgw-classic/",
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

## <a name="understanding-the-results"></a>Inzicht krijgen in de resultaten

De actietekst bevat algemene richtlijnen over het probleem op te lossen. Als een actie kan worden ondernomen voor het probleem, wordt een koppeling volgen met aanvullende richtlijnen. In het geval waarbij er geen aanvullende richtlijnen, het antwoord geeft de url om een ondersteuningsaanvraag openen.  Voor meer informatie over de eigenschappen van het antwoord en wat is opgenomen, gaat u naar [overzicht van Network Watcher oplossen](network-watcher-troubleshoot-overview.md)

Zie voor instructies over het downloaden van bestanden vanuit azure storage-accounts, [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een ander hulpmiddel dat kan worden gebruikt, is Storage Explorer. Meer informatie over Storage Explorer hier kan worden gevonden op de volgende koppeling: [Storage Explorer](http://storageexplorer.com/)

## <a name="next-steps"></a>Volgende stappen

Als de instellingen zijn gewijzigd dat stop VPN-connectiviteit, raadpleegt u [Netwerkbeveiligingsgroepen beheren](../virtual-network/manage-network-security-group.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die mogelijk in kwestie.
