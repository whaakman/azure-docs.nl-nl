---
title: Azure brongebruik op basis van limieten controleren | Microsoft Docs
description: Informatie over het controleren van het gebruik van uw Azure-resource op basis van limieten van de Azure-abonnement.
services: networking
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: networking
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2018
ms.author: jdial
ms.openlocfilehash: 0c51f48576c665fbe67f2f18198d6422fe872895
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34811737"
---
# <a name="check-resource-usage-against-limits"></a>Brongebruik op basis van limieten controleren

In dit artikel leert u hoe ziet u het aantal van elk resourcetype die u hebt geïmplementeerd in uw abonnement en wat uw [abonnementen](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits) zijn. De mogelijkheid om gebruik van bronnen op basis van limieten weer te geven is handig voor het huidige gebruik bijhouden en plannen voor toekomstig gebruik. U kunt de [Azure Portal](#azure-portal), [PowerShell](#powershell), of de [Azure CLI](#azure-cli) gebruik bijhouden.

## <a name="azure-portal"></a>Azure-portal

1. Meld u aan bij de Azure [portal](https://portal.azure.com).
2. Selecteer in de linkerbovenhoek hoek van de Azure portal **alle services**.
3. Voer *abonnementen* in de **Filter** vak. Wanneer **abonnementen** wordt weergegeven in de zoekresultaten, selecteer deze.
4. Selecteer de naam van het abonnement dat u wilt weergeven voor informatie over het gebruik.
5. Onder **instellingen**, selecteer **gebruik + quotum**.
6. U kunt de volgende opties selecteren:
    - **Brontypen**: alle brontypen selecteren of Selecteer de specifieke typen resources die u wilt weergeven.
    - **Providers**: U kunt alle resourceproviders selecteren, of selecteer **Compute**, **netwerk**, of **opslag**.
    - **Locaties**: U kunt alle Azure locaties selecteren of specifieke locaties selecteren.
    - U kunt selecteren om weer te geven van alle resources of alleen de resources waarop ten minste één wordt geïmplementeerd.

    Het voorbeeld in de volgende afbeelding ziet u alle resources netwerk met ten minste één bron in de VS-Oost geïmplementeerd:

        ![View usage data](./media/check-usage-against-limits/view-usage.png)

    U kunt de kolommen sorteren op de kolomkop te selecteren. De grenzen die wordt weergegeven, zijn de limieten voor uw abonnement. Als u een standaardlimiet te verhogen wilt, selecteert u **aanvraag verhogen**, voltooien en dien de ondersteuningsaanvraag. Alle resources hebben een maximumlimiet die worden vermeld in Azure [limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits). Als de huidige limiet van al het maximum aantal, kan niet de limiet worden verhoogd.

## <a name="powershell"></a>PowerShell

U kunt de volgende opdrachten in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. De Azure-Cloud-Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u PowerShell vanaf uw computer uitvoeren, moet u de *AzureRM* PowerShell-module, versie 6.0.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` op uw computer, de geïnstalleerde versie vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u bent PowerShell die lokaal wordt uitgevoerd, moet u ook uitvoeren `Login-AzureRmAccount` zich aanmelden bij Azure.

Gebruik van uw tegen limieten met weergeven [Get-AzureRmNetworkUsage](/powershell/module/azurerm.network/powershell/module/azurerm.network/get-azurermnetworkusage). Het volgende voorbeeld wordt het gebruik van bronnen waarop ten minste één bron wordt geïmplementeerd op de locatie VS-Oost:

```azurepowershell-interactive
Get-AzureRmNetworkUsage `
  -Location eastus `
  | Where-Object {$_.CurrentValue -gt 0} `
  | Format-Table ResourceType, CurrentValue, Limit
```

U ontvangt uitvoer geformatteerd gelijk zijn aan de volgende voorbeelduitvoer:

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

Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Dit artikel is vereist voor de Azure CLI versie 2.0.32 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal hebt uitgevoerd, moet u ook uitvoeren `az login` zich aanmelden bij Azure.

Gebruik van uw tegen limieten met weergeven [az netwerk lijst-gebruik](/cli/azure/network?view=azure-cli-latest#az-network-list-usages). Het volgende voorbeeld wordt het gebruik van bronnen op de locatie VS-Oost:

```azurecli-interactive
az network list-usages \
  --location eastus \
  --out table
```

U ontvangt uitvoer geformatteerd gelijk zijn aan de volgende voorbeelduitvoer:

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
