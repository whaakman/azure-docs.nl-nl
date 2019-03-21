---
title: Azure brongebruik op basis van limieten controleren | Microsoft Docs
description: Informatie over het controleren van uw Azure-resource-gebruik op basis van limieten voor Azure-abonnement.
services: networking
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2018
ms.author: jdial
ms.openlocfilehash: 54050c4c20b6ebb35f198775448f51ee8cdc533b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58117639"
---
# <a name="check-resource-usage-against-limits"></a>Brongebruik op basis van limieten controleren

In dit artikel leert u hoe u om te zien van het aantal van elk resourcetype die u hebt geïmplementeerd in uw abonnement en wat uw [abonnementslimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits) zijn. De mogelijkheid om gebruik van bronnen op basis van limieten weer te geven is handig om het huidige gebruik bijhouden en plannen voor toekomstig gebruik. U kunt de [Azure Portal](#azure-portal), [PowerShell](#powershell), of de [Azure CLI](#azure-cli) gebruik bijhouden.

## <a name="azure-portal"></a>Azure-portal

1. Meld u aan bij de Azure [portal](https://portal.azure.com).
2. Selecteer in de linkerbovenhoek hoek van de Azure-portal, **alle services**.
3. Voer *abonnementen* in de **Filter** vak. Wanneer **abonnementen** wordt weergegeven in de zoekresultaten, selecteert u dit.
4. Selecteer de naam van het abonnement dat u wilt weergeven voor informatie over het gebruik.
5. Onder **instellingen**, selecteer **gebruik + quota**.
6. U kunt de volgende opties selecteren:
   - **Resourcetypen**: U kunt alle resourcetypen selecteren, of de specifieke typen resources die u wilt weergeven.
   - **Providers**: U kunt alle resourceproviders selecteren, of selecteer **Compute**, **netwerk**, of **opslag**.
   - **Locaties**: U kunt alle Azure-locaties selecteren, of specifieke locaties.
   - U kunt selecteren om weer te geven van alle resources of alleen de resources waarop ten minste één wordt geïmplementeerd.

     Het voorbeeld in de volgende afbeelding ziet u alle van de netwerkbronnen met ten minste één resource in de VS-Oost geïmplementeerd:

       ![Gebruiksgegevens weergeven](./media/check-usage-against-limits/view-usage.png)

     U kunt de kolommen sorteren op de kolomkop te selecteren. De limieten die wordt weergegeven, zijn de limieten voor uw abonnement. Als u nodig hebt om een standaardlimiet te verhogen, selecteert u **aanvraag verhogen**, voltooien en het ondersteuningsverzoek verzendt. Alle resources een maximumlimiet voor die worden vermeld in Azure hebt [limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits). Als uw huidige limiet al het maximum aantal is, kan niet de limiet worden verhoogd.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt de opdrachten die volgen in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. De Azure Cloud Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u PowerShell vanaf uw computer uitvoeren, moet u de Azure PowerShell-module, versie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` op uw computer, de geïnstalleerde versie te vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook om uit te voeren `Login-AzAccount` aanmelden bij Azure.

Uw gebruik op basis van limieten met bekijken [Get-AzNetworkUsage](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkusage). Het volgende voorbeeld wordt het gebruik van resources waarop ten minste één resource wordt geïmplementeerd op de locatie VS-Oost:

```azurepowershell-interactive
Get-AzNetworkUsage `
  -Location eastus `
  | Where-Object {$_.CurrentValue -gt 0} `
  | Format-Table ResourceType, CurrentValue, Limit
```

Ontvangt u uitvoer opgemaakt gelijk zijn aan de volgende voorbeelduitvoer:

```powershell
ResourceType            CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
```

## <a name="azure-cli"></a>Azure-CLI

Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In dit artikel gebruikmaken van Azure CLI versie 2.0.32 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` aanmelden bij Azure.

Uw gebruik op basis van limieten met bekijken [az network list-usages](/cli/azure/network?view=azure-cli-latest#az-network-list-usages). Het volgende voorbeeld wordt het gebruik van resources in de locatie VS-Oost:

```azurecli-interactive
az network list-usages \
  --location eastus \
  --out table
```

Ontvangt u uitvoer opgemaakt gelijk zijn aan de volgende voorbeelduitvoer:

```azurecli
Name                    CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
Load Balancers                     0   100
Application Gateways               0    50
```
