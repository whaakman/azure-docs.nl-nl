---
title: Problemen oplossen met Azure Virtual Network Gateway en verbindingen - PowerShell | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u de Azure Network Watcher PowerShell-cmdlet oplossen
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 51fb834c0c6a3602ed0edfee6256183eefb2026b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57889485"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Problemen oplossen met de Gateway van virtueel netwerk en verbindingen met behulp van Azure Network Watcher-PowerShell

> [!div class="op_single_selector"]
> - [Portal](diagnose-communication-problem-between-networks.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [Azure-CLI](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Network Watcher biedt veel mogelijkheden met betrekking tot het begrijpen van de netwerkresources van uw in Azure. Een van deze mogelijkheden is resource-oplossen van problemen. Het oplossen van resource kan worden aangeroepen via de portal, PowerShell, CLI of REST-API. Indien aangeroepen, wordt Network Watcher inspecteert de status van de Gateway van een virtueel netwerk of een verbinding en retourneert de bevindingen.

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een Network Watcher](network-watcher-create.md) te maken van een Network Watcher.

Voor een lijst van ondersteunde gateway typen bezoek, [ondersteund gatewaytypen](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Overzicht

Het oplossen van resource biedt de mogelijkheid voor het oplossen van problemen die met Gateways van virtueel netwerk en verbindingen optreden. Wanneer een aanvraag wordt gedaan voor het oplossen van de resource, logboeken worden opgevraagd en geïnspecteerd. Als controle voltooid is, worden de resultaten worden geretourneerd. Aanvragen voor het oplossen van problemen worden langlopende resource aanvraagt, die meerdere minuten kan duren om een resultaat te retourneren. De logboeken van het oplossen van problemen worden opgeslagen in een container op een storage-account dat is opgegeven.

## <a name="retrieve-network-watcher"></a>Retrieve Network Watcher

De eerste stap is om op te halen van de Network Watcher-exemplaar. De `$networkWatcher` variabele wordt doorgegeven aan de `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet in stap 4.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Een virtuele Netwerkgatewayverbinding ophalen

In dit voorbeeld is het oplossen van resource wordt uitgevoerd op een verbinding. U kunt ook deze doorgeven een virtuele netwerkgateway.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a>Create a storage account

Het oplossen van resource geeft gegevens over de status van de resource, Bovendien bespaart u logboeken naar een opslagaccount om te worden gecontroleerd. In deze stap maken we een storage-account, als een bestaand opslagaccount bestaat kunt u deze kunt gebruiken.

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a>Network Watcher-resource voor probleemoplossing uitvoert

Oplossen van problemen met resources met de `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet. De netwerk-Watcher-object, de Id van de verbinding of de Gateway van het virtuele netwerk, de storage-account-id en het pad voor het opslaan van de resultaten geven we de cmdlet.

> [!NOTE]
> De `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet wordt de uitvoering lang duurt, en kan een paar minuten duren.

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

Zodra u de cmdlet uitvoert, beoordelingen Network Watcher van de resource om te controleren of de status. Deze retourneert de resultaten naar de shell en logboeken van de resultaten opgeslagen in het opslagaccount dat is opgegeven.

## <a name="understanding-the-results"></a>Inzicht krijgen in de resultaten

De actietekst bevat algemene richtlijnen over het probleem op te lossen. Als een actie kan worden ondernomen voor het probleem, wordt een koppeling volgen met aanvullende richtlijnen. In het geval waarbij er geen aanvullende richtlijnen, het antwoord geeft de url om een ondersteuningsaanvraag openen.  Voor meer informatie over de eigenschappen van het antwoord en wat is opgenomen, gaat u naar [overzicht van Network Watcher oplossen](network-watcher-troubleshoot-overview.md)

Zie voor instructies over het downloaden van bestanden vanuit azure storage-accounts, [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een ander hulpmiddel dat kan worden gebruikt, is Storage Explorer. Meer informatie over Storage Explorer Hier vindt op de volgende koppeling: [Storage Explorer](https://storageexplorer.com/)

## <a name="next-steps"></a>Volgende stappen

Als de instellingen zijn gewijzigd dat stop VPN-connectiviteit, raadpleegt u [Netwerkbeveiligingsgroepen beheren](../virtual-network/manage-network-security-group.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die mogelijk in kwestie.
