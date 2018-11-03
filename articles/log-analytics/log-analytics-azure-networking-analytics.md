---
title: Azure Networking Analytics-oplossing in Log Analytics | Microsoft Docs
description: U kunt de oplossing Azure Networking Analytics in Log Analytics gebruiken om Logboeken van netwerkbeveiligingsgroepen Azure-netwerk en Azure Application Gateway-logboeken te controleren.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: ewinner
editor: ''
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: richrund
ms.component: ''
ms.openlocfilehash: 2ba921a45619c74af230b282c23adff502ae5b6d
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961446"
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a>Azure-netwerken bewakingsoplossingen in Log Analytics

Log Analytics biedt de volgende oplossingen voor het bewaken van uw netwerken:
* Netwerkprestatiemeter (NPM) voor
 * Controleer de status van uw netwerk
* Azure Application Gateway analytics om te controleren
 * Azure Application Gateway-Logboeken
 * Metrische gegevens van Azure Application Gateway
* Netwerkactiviteit van oplossingen bewaken en analyseren in uw cloudnetwerk
* [Traffic Analytics](https://docs.microsoft.com/azure/networking/network-monitoring-overview#traffic-analytics) 
* Analyse van Azure-netwerkbeveiligingsgroep

## <a name="network-performance-monitor-npm"></a>Netwerkprestatiemeter (NPM)

De [Network Performance Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview) management-oplossing is een Netwerkcontrole-oplossing, die de status, beschikbaarheid en bereikbaarheid van netwerken controleert.  Het wordt gebruikt voor het bewaken van verbinding tussen:

* Openbare cloud en on-premises
* -Datacenters en locaties van de gebruiker (filialen)
* Subnetten die als host fungeert voor verschillende lagen van een toepassing met meerdere lagen.

Zie voor meer informatie, [Network Performance Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview).

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Azure Application Gateway en Network Security Group analytics
De oplossingen gebruiken:
1. De oplossing toevoegen aan Log Analytics, en
2. Schakel diagnostische gegevens om te leiden van de diagnostische gegevens naar Log Analytics-werkruimte. Het is niet die nodig zijn voor het schrijven van de logboeken naar Azure Blob storage.

U kunt diagnostische gegevens en de bijbehorende oplossing inschakelen voor een of beide van Application Gateway en netwerken-beveiligingsgroepen.

Als u Diagnostische logboekregistratie voor een bepaald brontype niet is ingeschakeld, maar de oplossing installeren, wordt de dashboard-blades voor die bron leeg zijn en een foutbericht weergegeven.

> [!NOTE]
> In januari 2017, de ondersteunde manier Logboeken uit Application Gateways en Netwerkbeveiligingsgroepen te verzenden naar Log Analytics gewijzigd. Als u ziet de **Azure Networking Analytics (afgeschaft)** oplossing, verwijzen naar [migreren van de oude netwerken analyseoplossing](#migrating-from-the-old-networking-analytics-solution) voor stappen die u moet volgen.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Azure networking details van de verzameling gegevens bekijken
De Azure Application Gateway-analyse- en Network Security Group analytics-beheeroplossingen verzamelen van logboeken met diagnostische gegevens rechtstreeks vanuit Azure Toepassingsgateways en Netwerkbeveiligingsgroepen. Het is niet nodig om te schrijven van de logboeken naar Azure Blob storage en geen agent is vereist voor het verzamelen van gegevens.

De volgende tabel ziet u gegevens verzameling methoden en andere informatie over hoe gegevens worden verzameld voor Azure Application Gateway analytics en de Network Security Group analytics.

| Platform | Agent toewijzen | Systems Center Operations Manager-agent | Azure | Operations Manager vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |Wanneer u bent aangemeld |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a>Azure Application Gateway analytics-oplossing in Log Analytics

![Azure Application Gateway Analytics symbool](media/log-analytics-azure-networking-analytics/azure-analytics-symbol.png)

De volgende logboeken worden voor Toepassingsgateways ondersteund:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog
* ApplicationGatewayFirewallLog

De volgende metrische gegevens worden ondersteund voor Application Gateways: opnieuw


* doorvoer van 5 minuten

### <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende instructies om te installeren en configureren van de oplossing Azure Application Gateway analytics:

1. Inschakelen van de Azure Application Gateway analytics-oplossing van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen uit de galerie van oplossingen](../monitoring/monitoring-solutions.md).
2. Diagnostische gegevens voor logboekregistratie inschakelen de [Toepassingsgateways](../application-gateway/application-gateway-diagnostics.md) u wilt bewaken.

#### <a name="enable-azure-application-gateway-diagnostics-in-the-portal"></a>Azure Application Gateway diagnostische gegevens in de portal inschakelen

1. In de Azure-portal, gaat u naar de Application Gateway-resource om te controleren
2. Selecteer *diagnoselogboeken* om de volgende pagina te openen

   ![afbeelding van Azure Application Gateway-resource](media/log-analytics-azure-networking-analytics/log-analytics-appgateway-enable-diagnostics01.png)
3. Klik op *diagnostische gegevens inschakelen* om de volgende pagina te openen

   ![afbeelding van Azure Application Gateway-resource](media/log-analytics-azure-networking-analytics/log-analytics-appgateway-enable-diagnostics02.png)
4. Voor diagnostische gegevens inschakelen, klikt u op *op* onder *Status*
5. Klik op het selectievakje voor *verzenden naar Log Analytics*
6. Selecteer een bestaande Log Analytics-werkruimte of maak een werkruimte
7. Klik op het selectievakje onder **Log** voor elk van de typen logboeken te verzamelen
8. Klik op *opslaan* om in te schakelen van de logboekregistratie van diagnostische gegevens naar Log Analytics

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>Diagnostische gegevens over Azure-netwerk met behulp van PowerShell inschakelen

De volgende PowerShell-script geeft een voorbeeld van hoe u Diagnostische logboekregistratie voor application gateways inschakelt.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Azure Application Gateway analytics gebruiken
![afbeelding van Azure Application Gateway analytics tegel](media/log-analytics-azure-networking-analytics/log-analytics-appgateway-tile.png)

Nadat u op de **Azure Application Gateway analytics** tegel in het overzicht kunt u samenvattingen van uw logboeken weergeven en vervolgens inzoomen op details voor de volgende categorieën:

* Toegang tot toepassingen Gateway-Logboeken
  * Client- en fouten voor Application Gateway-Logboeken
  * Aanvragen per uur voor elke Application Gateway
  * Mislukte aanvragen per uur voor elke Application Gateway
  * Fouten per gebruikersagent voor Application Gateways
* Prestaties van toepassingsgateway
  * Status van de host voor Application Gateway
  * Maximum- en 95e percentiel voor Application Gateway mislukte aanvragen

![afbeelding van Azure Application Gateway analytics-dashboard](media/log-analytics-azure-networking-analytics/log-analytics-appgateway01.png)

![afbeelding van Azure Application Gateway analytics-dashboard](media/log-analytics-azure-networking-analytics/log-analytics-appgateway02.png)

Op de **Azure Application Gateway analytics** dashboard, Controleer de samenvattingsinformatie in een van de blades, en klik op een om gedetailleerde informatie op de zoekpagina logboek weer te geven.

Op een van de log search pagina's, kunt u resultaten weergeven door de tijd, gedetailleerde resultaten en uw Logboekgeschiedenis zoeken. U kunt ook filteren op facetten om de resultaten te beperken.


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a>Azure Network Security Group analytics solution in Log Analytics

![Azure Network Security Group Analytics symbool](media/log-analytics-azure-networking-analytics/azure-analytics-symbol.png)

> [!NOTE]
> De oplossing Network Security Group analytics wordt verplaatst naar ondersteuning van de community, omdat de functionaliteit is vervangen door [Traffic Analytics](../network-watcher/traffic-analytics.md).
> - De oplossing is nu beschikbaar in [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/oms-azurensg-solution/) en wordt binnenkort niet langer beschikbaar in de Azure Marketplace.
> - Voor bestaande klanten die de oplossing al toegevoegd aan de werkruimte, blijft het functioneren zonder te wijzigen.
> - Microsoft blijft voor de ondersteuning van verzonden diagnostische logboeken van de NSG aan uw werkruimte met behulp van de diagnostische instellingen.

De volgende logboeken worden voor netwerkbeveiligingsgroepen ondersteund:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

### <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende instructies om te installeren en configureren van de oplossing Azure Networking Analytics:

1. Inschakelen van de oplossing Azure Network Security Group analytics van [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen uit de galerie van oplossingen](../monitoring/monitoring-solutions.md).
2. Diagnostische gegevens voor logboekregistratie inschakelen de [Network Security Group](../virtual-network/virtual-network-nsg-manage-log.md) resources die u wilt bewaken.

### <a name="enable-azure-network-security-group-diagnostics-in-the-portal"></a>Azure network security group diagnostische gegevens in de portal inschakelen

1. In de Azure-portal, gaat u naar de resource van de Netwerkbeveiligingsgroep voor het bewaken van
2. Selecteer *diagnoselogboeken* om de volgende pagina te openen

   ![afbeelding van Azure Network Security Group-resource](media/log-analytics-azure-networking-analytics/log-analytics-nsg-enable-diagnostics01.png)
3. Klik op *diagnostische gegevens inschakelen* om de volgende pagina te openen

   ![afbeelding van Azure Network Security Group-resource](media/log-analytics-azure-networking-analytics/log-analytics-nsg-enable-diagnostics02.png)
4. Voor diagnostische gegevens inschakelen, klikt u op *op* onder *Status*
5. Klik op het selectievakje voor *verzenden naar Log Analytics*
6. Selecteer een bestaande Log Analytics-werkruimte of maak een werkruimte
7. Klik op het selectievakje onder **Log** voor elk van de typen logboeken te verzamelen
8. Klik op *opslaan* om in te schakelen van de logboekregistratie van diagnostische gegevens naar Log Analytics

### <a name="enable-azure-network-diagnostics-using-powershell"></a>Diagnostische gegevens over Azure-netwerk met behulp van PowerShell inschakelen

De volgende PowerShell-script geeft een voorbeeld van hoe u Diagnostische logboekregistratie voor netwerkbeveiligingsgroepen inschakelen
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Gebruik Azure Network Security Group analytics
Nadat u op de **Azure Network Security Group analytics** tegel in het overzicht kunt u samenvattingen van uw logboeken weergeven en vervolgens inzoomen op details voor de volgende categorieën:

* Netwerkbeveiligingsgroep geblokkeerde stromen
  * Regels voor netwerkbeveiligingsgroepen met geblokkeerde stromen
  * MAC-adressen met geblokkeerde stromen
* Netwerkbeveiligingsgroep toegestane stromen
  * Regels voor netwerkbeveiligingsgroepen met toegestane stromen
  * MAC-adressen met toegestane stromen

![afbeelding van Azure Network Security Group analytics-dashboard](media/log-analytics-azure-networking-analytics/log-analytics-nsg01.png)

![afbeelding van Azure Network Security Group analytics-dashboard](media/log-analytics-azure-networking-analytics/log-analytics-nsg02.png)

Op de **Azure Network Security Group analytics** dashboard, Controleer de samenvattingsinformatie in een van de blades, en klik op een om gedetailleerde informatie op de zoekpagina logboek weer te geven.

Op een van de log search pagina's, kunt u resultaten weergeven door de tijd, gedetailleerde resultaten en uw Logboekgeschiedenis zoeken. U kunt ook filteren op facetten om de resultaten te beperken.

## <a name="migrating-from-the-old-networking-analytics-solution"></a>Migreren van de oude Networking Analytics-oplossing
In januari 2017, de ondersteunde manier van het verzenden van Logboeken van Azure-toepassing en Azure Network Security Groups naar Log Analytics gewijzigd. Deze wijzigingen bieden de volgende voordelen:
+ Logboeken worden geschreven rechtstreeks naar Log Analytics zonder de noodzaak voor het gebruik van een storage-account
+ Minder latentie vanaf het moment waarop de logboeken worden gegenereerd voor hen beschikbaar worden gesteld in Log Analytics
+ Zijn minder configuratiestappen
+ Een algemene indeling voor alle typen Azure diagnostics

Bijgewerkte oplossingen gebruiken:

1. [Diagnostische gegevens rechtstreeks naar Log Analytics worden verzonden vanuit Azure Application Gateways configureren](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [Diagnostische gegevens worden verzonden naar Log Analytics rechtstreeks vanuit Azure Network Security Groups configureren](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. Schakel de *Azure Application Gateway Analytics* en de *Azure Network Security Group Analytics* oplossing met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de Galerie van oplossingen](../monitoring/monitoring-solutions.md)
3. Bijwerken van een opgeslagen query's, dashboards of waarschuwingen gebruik van het nieuwe gegevenstype
  + Type is het AzureDiagnostics. Het ResourceType kunt u filteren op Azure-netwerklogboeken.

    | In plaats van: | U kunt gebruiken: |
    | --- | --- |
    | NetworkApplicationgateways &#124; waar OperationName == "ApplicationGatewayAccess" | AzureDiagnostics &#124; waar ResourceType = "APPLICATIONGATEWAYS" en OperationName == "ApplicationGatewayAccess" |
    | NetworkApplicationgateways &#124; waar OperationName == "ApplicationGatewayPerformance" | AzureDiagnostics &#124; waar ResourceType == "APPLICATIONGATEWAYS" en OperationName ApplicationGatewayPerformance = |
    | NetworkSecuritygroups | AzureDiagnostics &#124; waar ResourceType == "NETWORKSECURITYGROUPS" |

   + Voor elk veld dat een achtervoegsel van \_s, \_d, of \_g in de naam wijzigen van het eerste teken in kleine letters
   + Voor elk veld dat een achtervoegsel van \_o in naam van de gegevens is opgesplitst in afzonderlijke velden op basis van de geneste veldnamen.
4. Verwijder de *Azure Networking Analytics (afgeschaft)* oplossing.
  + Als u met behulp van PowerShell, gebruikt u `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`

Gegevens die worden verzameld voordat de wijziging niet zichtbaar in de nieuwe oplossing is. U kunt zoeken naar deze gegevens met behulp van de oude Type en de veldnamen.

## <a name="troubleshooting"></a>Problemen oplossen
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten in Logboeken in Log Analytics](log-analytics-log-search.md) om weer te geven gedetailleerde gegevens van Azure diagnostics.
