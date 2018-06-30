---
title: Network security groep gebeurtenis en regel teller Azure diagnostische logboeken | Microsoft Docs
description: Informatie over het inschakelen van de gebeurtenis en regel teller diagnostische logboeken voor de beveiligingsgroep van een Azure-netwerk.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2018
ms.author: jdial
ms.openlocfilehash: 81809660bdda957eb4502e02799b9f7f5538ae51
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114020"
---
# <a name="diagnostic-logging-for-a-network-security-group"></a>Diagnostische logboekregistratie voor een netwerkbeveiligingsgroep

Een netwerkbeveiligingsgroep (NSG) bevat regels die verkeer naar een virtueel netwerksubnet en/of de netwerkinterface toestaan of weigeren. Wanneer u Diagnostische logboekregistratie voor een NSG inschakelt, kunt u zich aanmelden met de volgende categorieën van informatie:

* **Gebeurtenis:** vermeldingen worden geregistreerd voor welke NSG regels worden toegepast op virtuele machines, op basis van MAC-adres. De status voor deze regels worden verzameld om de 60 seconden.
* **Regelteller:** bevat vermeldingen voor hoe vaak elke NSG regel wordt toegepast om te weigeren of verkeer toestaan.

Diagnostische logboeken zijn alleen beschikbaar voor het nsg's geïmplementeerd via het Azure Resource Manager-implementatiemodel. U kunt Diagnostische logboekregistratie voor het nsg's geïmplementeerd via het klassieke implementatiemodel niet inschakelen. Zie voor een beter begrip van de twee modellen, [wat Azure-implementatiemodellen](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Diagnostische logboekregistratie afzonderlijk is ingeschakeld voor *elke* NSG die u wenst te verzamelen van diagnostische gegevens voor. Als u geïnteresseerd bent in operationeel is, of activiteit, in plaats daarvan registreert, Zie Azure [activiteitenregistratie](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="enable-logging"></a>Logboekregistratie inschakelen

U kunt de [Azure Portal](#azure-portal), [PowerShell](#powershell), of de [Azure CLI](#azure-cli) Diagnostische logboekregistratie in te schakelen.

### <a name="azure-portal"></a>Azure-portal

1. Meld u aan bij de [portal](https://portal.azure.com).
2. Selecteer **alle services**, typ *netwerkbeveiligingsgroepen*. Wanneer **Netwerkbeveiligingsgroepen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
3. Selecteer het NSG u logboekregistratie wilt inschakelen voor.
4. Onder **bewaking**, selecteer **diagnostische logboeken**, en selecteer vervolgens **diagnostische gegevens inschakelen**, zoals wordt weergegeven in de volgende afbeelding:
 
    ![Diagnostische gegevens inschakelen](./media/virtual-network-nsg-manage-log/turn-on-diagnostics.png)

5. Onder **diagnostische instellingen**invoert, of Selecteer de volgende informatie en selecteer vervolgens **opslaan**:

    | Instelling                                                                                     | Waarde                                                          |
    | ---------                                                                                   |---------                                                       |
    | Naam                                                                                        | De naam van uw keuze.  Bijvoorbeeld: *myNsgDiagnostics*      |
    | **Archiveren naar een opslagaccount**, **Stream naar een event hub**, en **verzenden met Log Analytics** | U kunt zoveel bestemmingen als u wilt selecteren. Zie voor meer informatie over elk [Meld bestemmingen](#log-destinations).                                                                                                                                           |
    | LOGBOEK                                                                                         | Selecteer een of beide logboek-categorieën. Zie voor meer informatie over de gegevens die zijn geregistreerd voor elke categorie, [Meld categorieën](#log-categories).                                                                                                                                             |
6. Weergeven en analyseren van Logboeken. Zie voor meer informatie [weergeven en analyseren van logboeken](#view-and-analyze-logs).

### <a name="powershell"></a>PowerShell

U kunt de volgende opdrachten in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. De Azure-Cloud-Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u PowerShell vanaf uw computer uitvoeren, moet u de *AzureRM* PowerShell-module, versie 6.1.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` op uw computer, de geïnstalleerde versie vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook uitvoeren `Login-AzureRmAccount` aan te melden bij Azure met een account met de [noodzakelijke machtigingen](virtual-network-network-interface.md#permissions)].

Als u wilt vastleggen van diagnostische gegevens inschakelen, moet u de Id van een bestaande NSG. Als u een bestaande NSG hebt, kunt u een met [nieuw AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).

Ophalen van de netwerkbeveiligingsgroep die u in staat stellen Diagnostische logboekregistratie wilt voor met [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup). Bijvoorbeeld, een NSG ophalen met de naam *myNsg* die zich in een resourcegroep met de naam *myResourceGroup*, voer de volgende opdracht:

```azurepowershell-interactive
$Nsg=Get-AzureRmNetworkSecurityGroup `
  -Name myNsg `
  -ResourceGroupName myResourceGroup
```

U kunt de logboeken met diagnostische gegevens schrijven naar drie typen van de bestemming. Zie voor meer informatie [Meld bestemmingen](#log-destinations). In dit artikel logboeken worden verzonden naar de *logboekanalyse* bestemming als voorbeeld. Ophalen van een bestaande werkruimte voor logboekanalyse met [Get-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace). Bijvoorbeeld, een bestaande werkruimte ophalen met de naam *myWorkspace* in een resourcegroep met de naam *myWorkspaces*, voer de volgende opdracht:

```azurepowershell-interactive
$Oms=Get-AzureRmOperationalInsightsWorkspace `
  -ResourceGroupName myWorkspaces `
  -Name myWorkspace
```

Als u een bestaande werkruimte geen hebt, kunt u een met [nieuw AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace).

Er zijn twee categorieën van logboekregistratie die kunt u Logboeken voor inschakelen. Zie voor meer informatie [Meld categorieën](#log-categories). Diagnostische logboekregistratie inschakelen voor de NSG met [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting). Het volgende voorbeeld registreert zowel gebeurtenissen en prestatiemeteritems categoriegegevens naar de werkruimte voor een NSG, met de id's voor de NSG en de werkruimte die u eerder hebt opgehaald:

```azurepowershell-interactive
Set-AzureRmDiagnosticSetting `
  -ResourceId $Nsg.Id `
  -WorkspaceId $Oms.ResourceId `
  -Enabled $true
```

Als u alleen wilt om gegevens in één categorie of de andere, in plaats van beide te registreren, voegt de `-Categories` optie naar de vorige opdracht, gevolgd door *NetworkSecurityGroupEvent* of *NetworkSecurityGroupRuleCounter*. Als u zich wilt aanmelden met een andere [bestemming](#log-destinations) dan een werkruimte voor logboekanalyse, gebruikt u de juiste parameters voor een Azure [opslagaccount](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Event Hub](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Weergeven en analyseren van Logboeken. Zie voor meer informatie [weergeven en analyseren van logboeken](#view-and-analyze-logs).

### <a name="azure-cli"></a>Azure-CLI

U kunt de volgende opdrachten in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de Azure CLI vanaf uw computer. De Azure-Cloud-Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u de CLI vanaf uw computer uitvoert, moet u versie 2.0.38 of hoger. Voer `az --version` op uw computer, de geïnstalleerde versie vinden. Als u upgraden wilt, Zie [Azure CLI installeren](/cli/azure/install-azure-cli?view=azure-cli-latest). Als u de CLI lokaal uitvoert, moet u ook uitvoeren `az login` aan te melden bij Azure met een account met de [noodzakelijke machtigingen](virtual-network-network-interface.md#permissions).

Als u wilt vastleggen van diagnostische gegevens inschakelen, moet u de Id van een bestaande NSG. Als u een bestaande NSG hebt, kunt u een met [az netwerk nsg maken](/cli/azure/network/nsg#az-network-nsg-create).

Ophalen van de netwerkbeveiligingsgroep die u in staat stellen Diagnostische logboekregistratie wilt voor met [az netwerk nsg weergeven](/cli/azure/network/nsg#az-network-nsg-show). Bijvoorbeeld, een NSG ophalen met de naam *myNsg* die zich in een resourcegroep met de naam *myResourceGroup*, voer de volgende opdracht:

```azurecli-interactive
nsgId=$(az network nsg show \
  --name myNsg \
  --resource-group myResourceGroup \
  --query id \
  --output tsv)
```

U kunt de logboeken met diagnostische gegevens schrijven naar drie typen van de bestemming. Zie voor meer informatie [Meld bestemmingen](#log-destinations). In dit artikel logboeken worden verzonden naar de *logboekanalyse* bestemming als voorbeeld. Zie voor meer informatie [Meld categorieën](#log-categories). 

Diagnostische logboekregistratie inschakelen voor de NSG met [az monitor diagnose-instellingen maken](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create). Het volgende voorbeeld zowel gebeurtenissen en prestatiemeteritems categoriegegevens geregistreerd met een bestaande werkruimte met de naam *myWorkspace*, die in een resourcegroep met de naam bestaat *myWorkspaces*, en de ID van de NSG die u hebt opgehaald eerder:

```azurecli-interactive
az monitor diagnostic-settings create \
  --name myNsgDiagnostics \
  --resource $nsgId \
  --logs '[ { "category": "NetworkSecurityGroupEvent", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "NetworkSecurityGroupRuleCounter", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --workspace myWorkspace \
  --resource-group myWorkspaces
```

Als u een bestaande werkruimte geen hebt, kunt u één die gebruikmaakt van de [Azure-portal](../log-analytics/log-analytics-quick-create-workspace.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [PowerShell](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace). Er zijn twee categorieën van logboekregistratie die kunt u Logboeken voor inschakelen. 

Als u alleen wilt om gegevens in één categorie of de andere te registreren, moet u de categorie die u niet wilt vastleggen van gegevens voor in de voorgaande opdracht verwijderen. Als u zich wilt aanmelden met een andere [bestemming](#log-destinations) dan een werkruimte voor logboekanalyse, gebruikt u de juiste parameters voor een Azure [opslagaccount](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Event Hub](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Weergeven en analyseren van Logboeken. Zie voor meer informatie [weergeven en analyseren van logboeken](#view-and-analyze-logs).

## <a name="log-destinations"></a>Logboek bestemmingen

Diagnostische gegevens kunnen worden:
- [Naar een Azure Storage-account geschreven](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), voor inspectie controle of handmatig. U kunt de retentietijd (in dagen) met behulp van de diagnostische instellingen resource opgeven.
- [Gestreamd naar een Event hub](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor opname door een service van derden of aangepaste analytics-oplossing, zoals Power BI.
- [Geschreven naar Azure Log Analytics](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-diagnostics-direct-to-log-analytics).

## <a name="log-categories"></a>Logboekcategorieën

Gegevens in JSON-indeling wordt voor de volgende categorieën van logboek geschreven:

### <a name="event"></a>Gebeurtenis

Het logboek bevat informatie over welke NSG-regels worden toegepast op virtuele machines, op basis van MAC-adres. Het volgende voorbeeldgegevens worden geregistreerd voor elke gebeurtenis:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY RULE NAME]",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"[PORT RANGE]",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter"></a>Regelteller

Het logboek van de teller regel bevat informatie over elke regel toegepast op resources. Het volgende voorbeeldgegevens worden geregistreerd telkens wanneer die een regel wordt toegepast:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY RULE NAME]",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

> [!NOTE]
> Het IP-bronadres voor de communicatie wordt niet geregistreerd. U kunt inschakelen [NSG stroom logboekregistratie](../network-watcher/network-watcher-nsg-flow-logging-portal.md) voor een NSG echter die logbestanden met alle van de teller regelgegevens, evenals het IP-bronadres die de communicatie wordt gestart. NSG-stroomlogboekgegevens worden naar een Azure Storage-account geschreven. U kunt de gegevens analyseren met de [traffic analytics](../network-watcher/traffic-analytics.md) mogelijkheden van Azure-netwerk-Watcher.

## <a name="view-and-analyze-logs"></a>Weergeven en analyseren van Logboeken

Zie voor meer informatie over het weergeven van diagnostische logboekgegevens, [Azure diagnostische logboeken overzicht](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u diagnostische gegevens verzenden:
- **Meld u Analytics**: kunt u de [netwerk groep beveiligingsanalyse](../log-analytics/log-analytics-azure-networking-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-security-group-analytics-solution-in-log-analytics
) oplossing voor verbeterde insights. De oplossing biedt visualisaties voor NSG-regels die verkeer per MAC-adres van de netwerkinterface op een virtuele machine toestaan of weigeren.
- **Azure Storage-account**: gegevens worden geschreven naar een bestand PT1H.json. U vindt de:
    - Gebeurtenislogboek in het volgende pad: `insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`
    - Regel teller log in het volgende pad: `insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [activiteitenregistratie](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), voorheen bekend als audit- of operationele Logboeken. Activiteit-logboekregistratie is standaard ingeschakeld voor het nsg's die zijn gemaakt via de Azure-implementatiemodel. Om te bepalen welke bewerkingen zijn voltooid op het nsg's in het activiteitenlogboek, zoeken naar gegevens die de volgende resourcetypen bevatten:
    - Microsoft.ClassicNetwork/networkSecurityGroups
    - Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
    - Microsoft.Network/networkSecurityGroups
    - Microsoft.Network/networkSecurityGroups/securityRules
- Zie voor meer informatie over het registreren van diagnostische gegevens, zodat het bron-IP-adres voor elke verkeersstroom [NSG stroom logboekregistratie](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
