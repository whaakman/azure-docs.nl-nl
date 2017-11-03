---
title: Azure DDoS-bescherming Standard met Azure PowerShell beheren | Microsoft Docs
description: Informatie over het beheren van Azure DDoS-bescherming Standard met Azure PowerShell.
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: kumud
ms.openlocfilehash: a1a3688d4ff215d05d2f78cdfa7d402e3fc20be2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-ddos-protection-standard-using-azure-powershell"></a>Azure DDoS-bescherming Standard met Azure PowerShell beheren

>[!IMPORTANT]
>Azure DDoS-bescherming Standard (DDoS-bescherming) is momenteel in preview. Een beperkt aantal Azure-resources ondersteuning DDoS-bescherming, en in een select aantal regio's. U moet [registreren voor de service](http://aka.ms/ddosprotection) tijdens de beperkte preview DDoS-bescherming is ingeschakeld voor uw abonnement ophalen. Contact met u door het team van Azure DDoS bij de registratie naar helpt u bij de ingeschakelde. DDoS-bescherming is beschikbaar in VS-Oost, VS-West en West-Centraal VS regio's. Tijdens de preview, er zijn niet in rekening gebracht voor het gebruik van de service.

In dit artikel laat zien hoe u met Azure PowerShell DDoS-beveiliging inschakelen, uitschakelen DDoS-bescherming en telemetrie om het risico van een aanval te gebruiken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. Als u wilt installeren of upgraden van Azure PowerShell, Zie [Installeer Azure PowerShell-module](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="enable-ddos-protection"></a>DDoS-beveiliging inschakelen

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Een nieuw virtueel netwerk maken en inschakelen van DDoS-bescherming

Als een virtueel netwerk maken met DDoS-beveiliging is ingeschakeld, moet u het volgende voorbeeld uitvoert:

```powershell
New-AzureRmResourceGroup -Name <ResourceGroupName> -Location westcentralus 
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <frontendSubnet> -AddressPrefix "10.0.1.0/24" 
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <backendSubnet> -AddressPrefix "10.0.2.0/24" 
New-AzureRmVirtualNetwork -Name <MyVirtualNetwork> -ResourceGroupName <ResourceGroupName>  -Location westcentralus  -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -DnsServer 10.0.1.5,10.0.1.6 -EnableDDoSProtection
```

In dit voorbeeld maakt een virtueel netwerk met twee subnetten en twee DNS-servers. Het effect van het opgeven van de DNS-servers op het virtuele netwerk is dat deze DNS-servers als standaardwaarden worden overgenomen door de NIC's / virtuele machines die zijn geïmplementeerd in dit virtuele netwerk. DDoS-bescherming is ingeschakeld voor de beveiligde bronnen in het virtuele netwerk.

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>DDoS-beveiliging inschakelen op een bestaand virtueel netwerk

Als u DDoS-bescherming op een bestaand virtueel netwerk, voert u het volgende voorbeeld:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $true
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName "ResourceGroupName" -ResourceName "ResourceName" -ResourceType Microsoft.Network/virtualNetworks
```

## <a name="disable-ddos-protection-on-a-virtual-network"></a>DDoS-bescherming uitschakelen op een virtueel netwerk

Als u wilt uitschakelen DDoS-bescherming op een virtueel netwerk, moet u het volgende voorbeeld uitvoert:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $false
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName <RessourceGroupName> -ResourceName <ResourceName> -ResourceType "Microsoft.Network/virtualNetworks"
```

## <a name="review-the-ddos-protection-status-of-virtual-networks"></a>Controleer de status van de DDoS-beveiliging van virtuele netwerken 

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps
```

## <a name="use-ddos-protection-telemetry"></a>DDoS-bescherming telemetrie gebruiken

Telemetrie voor een aanval is beschikbaar via de Azure-Monitor in realtime. De telemetrie is alleen beschikbaar voor de duur die een openbaar IP-adres wordt risicobeperking. Worden er geen telemetrie vóór of na een aanval is verholpen.

### <a name="configure-alerts-on-ddos-protection-metrics"></a>Meldingen configureren voor DDoS-bescherming metrische gegevens

Mogelijkheden van de configuratie van Azure-Monitor de waarschuwing, kunt u een van de beschikbare DDoS-bescherming metrische gegevens om te waarschuwen wanneer er een actieve risicobeperking tijdens een aanval.

#### <a name="configure-email-alert-rules-via-azure-powershell"></a>Configureren van e-mailbericht waarschuwingsregels via Azure PowerShell

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

3. Als u wilt een regel maakt, moet u eerst hebt van belangrijke stukjes informatie. 

    - De Resource-ID voor de resource die u wilt een waarschuwing voor het instellen.
    - De metrische definities beschikbaar zijn voor die bron. Een manier om op te halen van de Resource-ID is de Azure portal gebruiken. Ervan uitgaande dat de resource is al gemaakt, selecteert u deze in de Azure-portal. Selecteer in de volgende pagina *eigenschappen* onder de *instellingen* sectie. De **RESOURCE-ID** is een veld in de volgende pagina. Een andere manier is met de [Azure Resource Explorer](https://resources.azure.com/). Er is een voorbeeld van de Resource-ID voor een openbaar IP-adres:`/subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Network/publicIPAddresses/mypublicip`

    Het volgende voorbeeld heeft een waarschuwing op een openbaar IP-adres die zijn gekoppeld aan een virtueel netwerk ingesteld. De meetwaarde een waarschuwing wilt maken op is **onder DDoS-aanvallen of niet**. Dit is een Booleaanse waarde 1 of 0. Een **1** betekent dat u bent bij een aanval. Een **0** betekent dat u bent geen aangevallen. De waarschuwing wordt gemaakt wanneer u aangevallen binnen de laatste 5 minuten zijn.

    Voor het maken van een webhook of e-mailbericht verzenden wanneer een waarschuwing is gemaakt, moet u eerst de e-mailadres en/of webhooks maken. Vervolgens onmiddellijk de regel daarna met de - tag acties en maken zoals weergegeven in het volgende voorbeeld. Kan geen koppelt u webhook of e-mailberichten met regels met behulp van PowerShell al is gemaakt.

    ```powershell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com 
    Add-AzureRmMetricAlertRule -Name <myMetricRuleWithEmail> -Location "West Central US" -ResourceGroup <myresourcegroup> -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypublicip -MetricName "IfUnderDDoSAttack" -Operator GreaterThan -Threshold 0 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail-Description "Under DDoS Attack" 
    ```

4. Om te controleren of uw waarschuwingen correct zijn gemaakt door te kijken naar de afzonderlijke regels.

    ```powershell
    Get-AzureRmAlertRule -Name myMetricRuleWithEmail -ResourceGroup myresourcegroup -DetailedOutput 
    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

U kunt ook meer informatie over [configureren van webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md) en [logische apps](../logic-apps/logic-apps-what-are-logic-apps.md) voor het maken van waarschuwingen.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Logboekregistratie configureren voor DDoS-bescherming metrische gegevens

Raadpleeg de [PowerShell snel starten voorbeelden](../monitoring-and-diagnostics/insights-powershell-samples.md) voor hulp bij het openen en configureren van Azure Diagnostische logboekregistratie via PowerShell.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Logboeken van de Azure-opslag met logboekanalyse analyseren](../log-analytics/log-analytics-azure-storage.md)
- [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)