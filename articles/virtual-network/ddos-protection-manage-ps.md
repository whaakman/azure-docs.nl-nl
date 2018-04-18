---
title: Azure DDoS-bescherming Standard met Azure PowerShell beheren | Microsoft Docs
description: Informatie over het beheren van Azure DDoS-bescherming Standard met Azure PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: 33ff6cfcacd1632dc49b448e70361e1cb2ce1176
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-azure-ddos-protection-standard-using-azure-powershell"></a>Azure DDoS-bescherming Standard met Azure PowerShell beheren

Informatie over het inschakelen en uitschakelen van gedistribueerde denial-of service (DDoS) beveiliging en telemetrie om het risico van een DDoS-aanval met Azure DDoS-bescherming Standard te gebruiken. DDoS-bescherming standaard beveiligt Azure-resources zoals virtuele machines en netwerktaakverdelers Toepassingsgateways waarvoor een Azure [openbaar IP-adres](virtual-network-public-ip-address.md) toegewezen. Zie voor meer informatie over DDoS-bescherming standaard en de mogelijkheden ervan, [DDoS-bescherming standaard overzicht](ddos-protection-overview.md). 

>[!IMPORTANT]
>Azure DDoS-bescherming Standard (DDoS-bescherming) is momenteel in preview. Een beperkt aantal Azure-resources DDoS-beveiliging ondersteunen en het is alleen beschikbaar in een select aantal regio's. Zie voor een lijst met beschikbare regio's, [DDoS-bescherming standaard overzicht](ddos-protection-overview.md). U moet [registreren voor de service](http://aka.ms/ddosprotection) tijdens de beperkte preview DDoS-bescherming is ingeschakeld voor uw abonnement ophalen. Na de registratie contact met u door het Azure DDoS-team dat u door het proces inschakelen leidt.


## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. Als u wilt installeren of upgraden van Azure PowerShell, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps).

```powershell
Login-AzureRmAccount
```

## <a name="enable-ddos-protection-standard---new-virtual-network"></a>Standaard in DDoS-bescherming - nieuw virtueel netwerk inschakelen

Als een virtueel netwerk maken met DDoS-beveiliging is ingeschakeld, moet u het volgende voorbeeld uitvoert:

```powershell
New-AzureRmResourceGroup -Name <ResourceGroupName> -Location westcentralus 
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <frontendSubnet> -AddressPrefix "10.0.1.0/24" 
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <backendSubnet> -AddressPrefix "10.0.2.0/24" 
New-AzureRmVirtualNetwork -Name <MyVirtualNetwork> -ResourceGroupName <ResourceGroupName>  -Location westcentralus  -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -EnableDDoSProtection
```

In dit voorbeeld maakt een virtueel netwerk met twee subnetten en twee DNS-servers. Het effect van het opgeven van de DNS-servers op het virtuele netwerk is dat deze DNS-servers als standaardwaarden worden overgenomen door de NIC's / virtuele machines die zijn geïmplementeerd in dit virtuele netwerk. DDoS-bescherming is ingeschakeld voor de beveiligde bronnen in het virtuele netwerk.

> [!WARNING]
> Als u een regio, kiest u een ondersteunde regio in de lijst in [overzicht van Azure DDoS-bescherming Standard](ddos-protection-overview.md).

## <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>DDoS-beveiliging inschakelen op een bestaand virtueel netwerk

Als u DDoS-bescherming op een bestaand virtueel netwerk, voert u het volgende voorbeeld:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $true
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName "ResourceGroupName" -ResourceName "ResourceName" -ResourceType Microsoft.Network/virtualNetworks -Force
```

> [!WARNING]
> Het virtuele netwerk moet zich in een ondersteunde regio. Zie voor een lijst met ondersteunde regio's, [overzicht van Azure DDoS-bescherming Standard](ddos-protection-overview.md).

## <a name="disable-ddos-protection-on-a-virtual-network"></a>DDoS-bescherming uitschakelen op een virtueel netwerk

Als u wilt uitschakelen DDoS-bescherming op een virtueel netwerk, moet u het volgende voorbeeld uitvoert:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $false
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName <RessourceGroupName> -ResourceName <ResourceName> -ResourceType "Microsoft.Network/virtualNetworks" -Force
```

## <a name="review-the-ddos-protection-status-of-a-virtual-network"></a>Controleer de status van de DDoS-bescherming van een virtueel netwerk 

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps
```

## <a name="use-ddos-protection-telemetry"></a>DDoS-bescherming telemetrie gebruiken

Telemetrie voor een aanval is beschikbaar via de Azure-Monitor in realtime. De telemetrie is alleen beschikbaar voor de duur die een openbaar IP-adres wordt risicobeperking. Worden er geen telemetrie vóór of na een aanval is verholpen.

### <a name="configure-alerts-on-ddos-protection-metrics"></a>Meldingen configureren voor DDoS-bescherming metrische gegevens

Mogelijkheden van de configuratie van Azure-Monitor de waarschuwing, kunt u een van de beschikbare DDoS-bescherming metrische gegevens om te waarschuwen wanneer er een actieve risicobeperking tijdens een aanval.

#### <a name="configure-email-alert-rules-via-azure"></a>E-mailbericht waarschuwingsregels via Azure configureren

1. Een lijst van de abonnementen er beschikbaar zijn. Controleer of u werkt met het juiste abonnement. Als dat niet het geval is, moet u deze instellen op de juiste is met behulp van de uitvoer van Get-AzureRmSubscription. 

    ```powershell
    Get-AzureRmSubscription 
    Get-AzureRmContext 
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

2. U kunt bestaande regels voor een resourcegroep gebruiken de volgende opdracht: 

    ```powershell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

3. Als u wilt een regel maakt, moet u eerst de volgende informatie hebt: 

    - De Resource-ID voor de resource die u wilt een waarschuwing voor het instellen.
    - De metrische definities beschikbaar zijn voor die bron. Een manier om op te halen van de Resource-ID is de Azure portal gebruiken. Ervan uitgaande dat de resource is al gemaakt, selecteert u deze in de Azure-portal. Selecteer in de volgende pagina *eigenschappen* onder de *instellingen* sectie. De **RESOURCE-ID** is een veld in de volgende pagina. Een andere manier is met de [Azure Resource Explorer](https://resources.azure.com/). Er is een voorbeeld van de Resource-ID voor een openbaar IP-adres:`/subscriptions/<Id>/resourceGroups/myresourcegroupname/providers/Microsoft.Network/publicIPAddresses/mypublicip`

    Het volgende voorbeeld wordt een waarschuwing voor een openbaar IP-adres die zijn gekoppeld aan een resource in een virtueel netwerk. De meetwaarde een waarschuwing wilt maken op is **onder DDoS-aanvallen of niet**. Dit is een Booleaanse waarde 1 of 0. Een **1** betekent dat u bent bij een aanval. Een **0** betekent dat u bent geen aangevallen. De waarschuwing wordt gemaakt wanneer een aanval binnen de laatste vijf minuten gestart.

    Voor het maken van een webhook of e-mailbericht verzenden wanneer een waarschuwing is gemaakt, moet u eerst de e-mailadres en/of -webhook maken. Nadat de e-mailadres of de webhook is gemaakt, de regel met direct maken de `-Actions` labels, zoals wordt weergegeven in het volgende voorbeeld. U kunt webhook of e-mailberichten niet koppelen aan bestaande regels met behulp van PowerShell.

    ```powershell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com 
    Add-AzureRmMetricAlertRule -Name <myMetricRuleWithEmail> -Location "West Central US" -ResourceGroup <myresourcegroup> -TargetResourceId /subscriptions/<Id>/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypublicip -MetricName "IfUnderDDoSAttack" -Operator GreaterThan -Threshold 0 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail -Description "Under DDoS Attack"
    ```

4. Controleer of de waarschuwing juist gemaakt door te kijken in de regel:

    ```powershell
    Get-AzureRmAlertRule -Name myMetricRuleWithEmail -ResourceGroup myresourcegroup -DetailedOutput 
    ```

U kunt ook meer informatie over [configureren van webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [logische apps](../logic-apps/logic-apps-overview.md) voor het maken van waarschuwingen.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Logboekregistratie configureren voor DDoS-bescherming metrische gegevens

Raadpleeg de [PowerShell snel starten voorbeelden](../monitoring-and-diagnostics/insights-powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor hulp bij het openen en configureren van Azure Diagnostische logboekregistratie via PowerShell.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Logboeken van de Azure-opslag met logboekanalyse analyseren](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md?toc=%2fazure%2fvirtual-network%2ftoc.json)