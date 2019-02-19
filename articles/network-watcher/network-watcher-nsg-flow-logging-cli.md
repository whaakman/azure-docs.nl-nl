---
title: Logboeken met Azure Network Watcher - CLI van Azure Network Security Group stromen beheren | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van Network Security Group Flow Logboeken in Azure Network Watcher met Azure CLI
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 61674cc5551fb0d1be184de0ff0ed659cc56bbbe
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56340243"
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli"></a>Network Security Group Flow logboeken configureren met Azure CLI

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure-CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Stroomlogboeken van Netwerkbeveiligingsgroep zijn een functie van Network Watcher waarmee u informatie wilt weergeven over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep. Deze logboeken van de stroom zijn geschreven in json-indeling en weergeven van binnenkomende en uitgaande stromen op basis van per regel, de NIC die de stroom is van toepassing op, 5-tuple-informatie over de stroom (bron-/ doel-IP-adres, poort van de bron-/ doel, Protocol), en als het verkeer is toegestaan of geweigerd.

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (CLI)](/cli/azure/install-azure-cli).

## <a name="register-insights-provider"></a>Insights-provider registreren

Stroom logboekregistratie om te werken, zodat de **Microsoft.Insights** provider moet worden geregistreerd. Als u niet zeker weet als de **Microsoft.Insights** provider is geregistreerd, voer het volgende script.

```azurecli
az provider register --namespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a>Stroomlogboeken van Netwerkbeveiligingsgroep inschakelen

De opdracht uit om Logboeken van de stroom wordt weergegeven in het volgende voorbeeld:

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled true --nsg nsgName --storage-account storageAccountName
# Configure 
az network watcher flow-log configure --resource-group resourceGroupName --enabled true --nsg nsgName --storage-account storageAccountName  --format JSON --log-version 2
```

Het opslagaccount dat u opgeeft kan niet zijn geconfigureerd voor het netwerkregels waarmee toegang tot het netwerk wordt beperkt tot alleen Microsoft-services of specifieke virtuele netwerken. Het opslagaccount kan zich in dezelfde of een ander Azure-abonnement, dan de NSG waarmee het stroomlogboek voor. Als u verschillende abonnementen vallen, moeten ze beide zijn gekoppeld aan dezelfde Azure Active Directory-tenant. Het account dat u voor elk abonnement gebruikt moet hebben de [benodigde machtigingen](required-rbac-permissions.md). 

Als het opslagaccount zich in een andere resourcegroep of abonnement, geeft u de volledige-ID van het storage-account, in plaats van de naam dan de netwerkbeveiligingsgroep. Bijvoorbeeld, als het opslagaccount zich in een resourcegroep met de naam *RG-opslag*, in plaats van op te geven *storageAccountName* in de vorige opdracht geeft u   */subscriptions / { SubscriptionID}/resourceGroups/RG-Storage/providers/Microsoft.Storage/storageAccounts/storageAccountName*.

## <a name="disable-network-security-group-flow-logs"></a>Stroomlogboeken van Netwerkbeveiligingsgroep uitschakelen

Gebruik het volgende voorbeeld om uit te schakelen van Logboeken van de stroom:

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled false --nsg nsgName
```

## <a name="download-a-flow-log"></a>Een Flow-log Download

De opslaglocatie van het stroomlogboek van een wordt gedefinieerd bij het maken ervan. Een handig hulpmiddel voor toegang tot deze stroomlogboeken die zijn opgeslagen op een storage-account is Microsoft Azure Storage Explorer, die u kunt hier downloaden:  http://storageexplorer.com/

Als een storage-account is opgegeven, worden logboekbestanden van de stroom worden opgeslagen in een storage-account op de volgende locatie:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Ga voor informatie over de structuur van het logboek naar [Network Security Group Flow-log overzicht](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [visualiseren met Power BI uw NSG-stroomlogboeken](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Meer informatie over het [visualiseren van uw NSG-stroomlogboeken met open-sourcehulpprogramma's](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
