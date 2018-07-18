---
title: Problemen oplossen met Azure Virtual Network Gateway en verbindingen - Azure CLI 2.0 | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u de Azure Network Watcher voor het oplossen van Azure CLI 2.0
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 5f843b42a108968e2fbefacddcd22f331a04691e
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091098"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a>Problemen oplossen met de Gateway van virtueel netwerk en verbindingen met behulp van Azure Network Watcher Azure CLI 2.0

> [!div class="op_single_selector"]
> - [Portal](diagnose-communication-problem-between-networks.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [Azure-CLI](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Network Watcher biedt veel mogelijkheden met betrekking tot het begrijpen van de netwerkresources van uw in Azure. Een van deze mogelijkheden is resource-oplossen van problemen. Het oplossen van resource kan worden aangeroepen via de portal, PowerShell, CLI of REST-API. Indien aangeroepen, wordt Network Watcher inspecteert de status van de Gateway van een virtueel netwerk of een verbinding en retourneert de bevindingen.

In dit artikel wordt onze CLI van de volgende generatie voor het implementatiemodel resource management, Azure CLI 2.0 die beschikbaar is voor Windows, Mac en Linux.

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een Network Watcher](network-watcher-create.md) te maken van een Network Watcher.

Voor een lijst van ondersteunde gateway typen bezoek, [ondersteund gatewaytypen](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Overzicht

Het oplossen van resource biedt de mogelijkheid voor het oplossen van problemen die met Gateways van virtueel netwerk en verbindingen optreden. Wanneer een aanvraag wordt gedaan voor het oplossen van de resource, logboeken worden opgevraagd en geïnspecteerd. Als controle voltooid is, worden de resultaten worden geretourneerd. Aanvragen voor het oplossen van problemen worden langlopende resource aanvraagt, die meerdere minuten kan duren om een resultaat te retourneren. De logboeken van het oplossen van problemen worden opgeslagen in een container op een storage-account dat is opgegeven.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Een virtuele Netwerkgatewayverbinding ophalen

In dit voorbeeld is het oplossen van resource wordt uitgevoerd op een verbinding. U kunt ook deze doorgeven een virtuele netwerkgateway. De volgende cmdlet geeft een lijst van de vpn-verbindingen in een resourcegroep.

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

Zodra u de naam van de verbinding hebt, kunt u deze opdracht voor het ophalen van de resource-Id kunt uitvoeren:

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a>Create a storage account

Het oplossen van resource geeft gegevens over de status van de resource, Bovendien bespaart u logboeken naar een opslagaccount om te worden gecontroleerd. In deze stap maken we een storage-account, als een bestaand opslagaccount bestaat kunt u deze kunt gebruiken.

1. Het opslagaccount maken

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

## <a name="run-network-watcher-resource-troubleshooting"></a>Network Watcher-resource voor probleemoplossing uitvoert

Oplossen van problemen met resources met de `az network watcher troubleshooting` cmdlet. We geven de cmdlet de resourcegroep, de naam van de netwerk-Watcher, de Id van de verbinding van de Id van het opslagaccount en het pad naar de blob voor het opslaan van de problemen met resulteert in.

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

Zodra u de cmdlet uitvoert, beoordelingen Network Watcher van de resource om te controleren of de status. Deze retourneert de resultaten naar de shell en logboeken van de resultaten opgeslagen in het opslagaccount dat is opgegeven.

## <a name="understanding-the-results"></a>Inzicht krijgen in de resultaten

De actietekst bevat algemene richtlijnen over het probleem op te lossen. Als een actie kan worden ondernomen voor het probleem, wordt een koppeling volgen met aanvullende richtlijnen. In het geval waarbij er geen aanvullende richtlijnen, het antwoord geeft de url om een ondersteuningsaanvraag openen.  Voor meer informatie over de eigenschappen van het antwoord en wat is opgenomen, gaat u naar [overzicht van Network Watcher oplossen](network-watcher-troubleshoot-overview.md)

Zie voor instructies over het downloaden van bestanden vanuit azure storage-accounts, [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een ander hulpmiddel dat kan worden gebruikt, is Storage Explorer. Meer informatie over Storage Explorer hier kan worden gevonden op de volgende koppeling: [Storage Explorer](http://storageexplorer.com/)

## <a name="next-steps"></a>Volgende stappen

Als de instellingen zijn gewijzigd dat stop VPN-connectiviteit, raadpleegt u [Netwerkbeveiligingsgroepen beheren](../virtual-network/manage-network-security-group.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die mogelijk in kwestie.
