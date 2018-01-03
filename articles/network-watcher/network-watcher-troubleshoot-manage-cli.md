---
title: 'Problemen oplossen met Azure Virtual Network Gateway en verbindingen: Azure CLI 2.0 | Microsoft Docs'
description: Deze pagina wordt uitgelegd hoe u de Azure-netwerk-Watcher Azure CLI 2.0 oplossen
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 6e55e0a70142c81e9543688bf699ef149f3ecff2
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a>Problemen met virtuele netwerkgateway en verbindingen met behulp van Azure-netwerk-Watcher Azure CLI 2.0

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Netwerk-Watcher biedt veel mogelijkheden met betrekking tot het begrijpen van uw netwerkbronnen in Azure. Een van deze mogelijkheden resource is het oplossen van problemen. Het oplossen van bron kan worden aangeroepen via de portal, PowerShell, CLI of REST-API. Als deze wordt aangeroepen, netwerk-Watcher inspecteert de status van de Gateway van een virtueel netwerk of een verbinding en retourneert de bevindingen zijn.

Dit artikel wordt de volgende generatie CLI gebruikt voor de resource management-implementatiemodel, Azure CLI 2.0, die beschikbaar is voor Windows, Mac en Linux.

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

Voor een lijst met ondersteunde gateway typen bezoek, [ondersteund gatewaytypen](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Overzicht

Het oplossen van resource biedt de mogelijkheid oplossen van problemen die zich bij het virtuele netwerkgateways en verbindingen voordoen. Wanneer een aanvraag wordt gedaan bij resource probleemoplossing, logboeken worden opgevraagd en geïnspecteerd. Als controle voltooid is, worden de resultaten worden geretourneerd. Resources voor probleemoplossing aanvragen zijn langlopende aanvraagt, die meerdere minuten kan duren om een resultaat te retourneren. De logboeken van het oplossen van problemen worden opgeslagen in een container voor een opslagaccount die is opgegeven.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>De gatewayverbinding van een virtueel netwerk ophalen

In dit voorbeeld is het oplossen van resource wordt uitgevoerd op een verbinding. U kunt ook gebruikmaakt van een virtuele netwerkgateway. De volgende cmdlet geeft een lijst van de vpn-verbindingen in een resourcegroep.

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

Zodra u de naam van de verbinding hebt, kunt u deze opdracht de resource-Id ophalen uitvoeren:

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken

Gegevens over de status van de resource resource probleemoplossing retourneert, Bovendien bespaart u logboeken naar een opslagaccount moeten worden gecontroleerd. In deze stap, maken we een opslagaccount, als een bestaand opslagaccount bestaat kunt u deze kunt gebruiken.

1. De storage-account maken

    ```azurecli
    az storage account create --name storageAccountName --location westcentralus --resource-group resourceGroupName --sku Standard_LRS
    ```

1. De opslagaccountsleutels ophalen

    ```azurecli
    az storage account keys list --resource-group resourcegroupName --account-name storageAccountName
    ```

1. De container maken

    ```azurecli
    az storage container create --account-name storageAccountName --account-key {storageAccountKey} --name logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Netwerk-Watcher resource voor probleemoplossing uitvoert

Oplossen van resources met de `az network watcher troubleshooting` cmdlet. We geven de cmdlet de resourcegroep, de naam van de netwerk-Watcher, de Id van de verbinding van de Id van het storage-account en het pad naar de blob voor het opslaan van de problemen met resultaten.

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

Zodra u de cmdlet uitvoert, controleert de netwerk-Watcher de bron om te controleren of de status. Het retourneert de resultaten naar de shell en logboeken van de resultaten worden opgeslagen in het opgegeven opslagaccount.

## <a name="understanding-the-results"></a>Inzicht in de resultaten

De actietekst bevat algemene richtlijnen over het oplossen van het probleem. Als een actie kan worden uitgevoerd voor de uitgifte, wordt een koppeling geboden met aanvullende richtlijnen. In het geval wanneer er geen aanvullende richtlijnen, het antwoord geeft de url om een ondersteuningsaanvraag te openen.  Voor meer informatie over de eigenschappen van het antwoord en wat is opgenomen, gaat u naar [overzicht van netwerk-Watcher oplossen](network-watcher-troubleshoot-overview.md)

Zie voor instructies over het downloaden van bestanden van azure storage-accounts, [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een ander hulpprogramma dat kan worden gebruikt, is Storage Explorer. Meer informatie over Opslagverkenner vindt u hier op de volgende koppeling: [Opslagverkenner](http://storageexplorer.com/)

## <a name="next-steps"></a>Volgende stappen

Als de instellingen zijn gewijzigd die stop VPN-verbindingen, raadpleegt u [Netwerkbeveiligingsgroepen beheren](../virtual-network/virtual-network-manage-nsg-arm-portal.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die betrokken zijn.
