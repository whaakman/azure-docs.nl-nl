---
title: Azure Networking Analytics-oplossing in Log Analytics | Microsoft Docs
description: U kunt de Azure-netwerken Analytics-oplossing in Log Analytics gebruiken om te controleren van de groep beveiligingslogboeken Azure-netwerk en Azure Application Gateway-Logboeken.
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 06b67322b3812a668a515ecc357171ede1d85441
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a>Azure netwerken bewakingsoplossingen in Log Analytics

Log Analytics biedt de volgende oplossingen voor het bewaken van uw netwerken:
* Netwerk Prestatiemeter (NPM)
 * De status van uw netwerk
* Azure Application Gateway analytics om te controleren
 * Azure Application Gateway-Logboeken
 * Azure Application Gateway metrische gegevens
* Netwerkbeveiligingsgroep Azure analytics om te controleren
 * De Netwerkbeveiligingsgroep voor Azure-Logboeken

## <a name="network-performance-monitor-npm"></a>Netwerk-Prestatiemeter (NPM)

De [netwerk Prestatiemeter](log-analytics-network-performance-monitor.md) management-oplossing is een netwerk bewakingsoplossing die de status, beschikbaarheid en bereikbaarheid van netwerken bewaakt.  Het wordt gebruikt voor het bewaken van de verbinding tussen:

* openbare cloud en on-premises
* datacenters en gebruikerslocaties (filialen)
* de subnetten die als host fungeert voor verschillende lagen van een toepassing met meerdere lagen.

Zie voor meer informatie [netwerk Prestatiemeter](log-analytics-network-performance-monitor.md).

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Azure Application Gateway en de Netwerkbeveiligingsgroep analyses
De oplossingen gebruiken:
1. Toevoegen van de oplossing voor beheer met Log Analytics en
2. Diagnostische gegevens om te leiden van de diagnostische gegevens naar een werkruimte voor logboekanalyse inschakelen. Het is niet nodig zijn voor het schrijven van de logboeken naar Azure Blob-opslag.

U kunt diagnostische gegevens en de bijbehorende oplossing inschakelen voor een of beide van de toepassingsgateway en beveiligingsgroepen netwerken.

Als u Diagnostische logboekregistratie voor een bepaald brontype niet is ingeschakeld, maar de oplossing te installeren, wordt de blades dashboard voor die bron leeg zijn en een foutbericht weergegeven.

> [!NOTE]
> In januari 2017, de ondersteunde manier van het verzenden van Logboeken vanaf Toepassingsgateways en Netwerkbeveiligingsgroepen met logboekanalyse gewijzigd. Als u ziet de **Azure Networking Analytics (afgeschaft)** oplossing Raadpleeg [migreren van de oude Networking Analytics-oplossing](#migrating-from-the-old-networking-analytics-solution) voor stappen die u moet volgen.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Azure verzameling Gegevensdetails toegang controleren
De Azure Application Gateway-analyses en het beheer van Netwerkbeveiligingsgroep analyseoplossingen verzamelen van diagnostische gegevens logboeken rechtstreeks vanuit Azure Toepassingsgateways en Netwerkbeveiligingsgroepen. Het is niet nodig om te schrijven van de logboeken naar Azure Blob-opslag en geen agent is vereist voor het verzamelen van gegevens.

De volgende tabel bevat gegevens verzameling methoden en andere informatie over hoe gegevens worden verzameld voor de analyse van Azure Application Gateway en de Netwerkbeveiligingsgroep analytics.

| Platform | Directe agent | Systems Center Operations Manager-agent | Azure | Operations Manager is vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |Wanneer het logboek geregistreerd |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a>Azure Application Gateway analytics-oplossing in Log Analytics

![Azure Application Gateway Analytics symbool](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

De volgende logboeken worden ondersteund voor Toepassingsgateways:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog
* ApplicationGatewayFirewallLog

De volgende metrische gegevens worden voor Toepassingsgateways ondersteund:

* doorvoer van 5 minuten

### <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende instructies voor het installeren en configureren van de Azure Application Gateway analytics-oplossing:

1. Inschakelen van de Azure Application Gateway analytics-oplossing van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).
2. Registratie voor diagnostische gegevens inschakelen de [Toepassingsgateways](../application-gateway/application-gateway-diagnostics.md) u wilt bewaken.

#### <a name="enable-azure-application-gateway-diagnostics-in-the-portal"></a>Azure Application Gateway diagnostische gegevens in de portal inschakelen

1. Navigeer in de Azure-portal aan de toepassingsgateway resource bewaken
2. Selecteer *diagnostische logboeken* om de volgende pagina te openen

   ![afbeelding van Azure Application Gateway resource](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. Klik op *diagnostische gegevens inschakelen* om de volgende pagina te openen

   ![afbeelding van Azure Application Gateway resource](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. Om diagnostische gegevens inschakelen, klikt u op *op* onder *Status*
5. Klik op het selectievakje voor *verzenden met Log Analytics*
6. Selecteer een bestaande werkruimte voor logboekanalyse of een werkruimte maken
7. Klik op het selectievakje onder **logboek** voor elk van de typen logboekbestanden te verzamelen
8. Klik op *opslaan* waarmee de logboekregistratie van diagnostische gegevens met Log Analytics

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>Schakel Azure netwerkdiagnose met PowerShell

De volgende PowerShell-script bevat een voorbeeld van hoe diagnostische logboekregistratie voor Toepassingsgateways inschakelen.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Azure Application Gateway analytics gebruiken
![afbeelding van Azure Application Gateway analytics tegel](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

Nadat u op de **Azure Application Gateway analytics** tegel in het overzicht kunt u samenvattingen van uw logboeken weergeven en ga vervolgens omlaag de gedetailleerde informatie voor de volgende categorieën:

* Toegang tot toepassingen Gateway-Logboeken
  * Client en server-fouten voor Application Gateway-Logboeken
  * Aanvragen per uur voor elke Application Gateway
  * Mislukte aanvragen per uur voor elke Application Gateway
  * Fouten door gebruikersagent voor Toepassingsgateways
* Gateway-toepassingsprestaties
  * Status van de host voor Application Gateway
  * Maximum- en 95e percentiel voor Application Gateway mislukte aanvragen

![afbeelding van een dashboard met analytische Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![afbeelding van een dashboard met analytische Azure Application Gateway](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

Op de **Azure Application Gateway analytics** dashboard, Controleer de overzichtsgegevens in een van de blades en klik op eentje om gedetailleerde informatie weergeven over de zoekpagina logboek.

U kunt op elk van de zoekpagina logboek kunt resultaten weergeven door de tijd, gedetailleerde resultaten en uw Logboekgeschiedenis zoeken. U kunt ook filteren op facetten om de resultaten te beperken.


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a>Netwerkbeveiligingsgroep Azure analytics-oplossing in Log Analytics

![Netwerkbeveiligingsgroep Azure Analytics symbool](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

De volgende logboeken worden ondersteund voor netwerkbeveiligingsgroepen:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

### <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende instructies voor het installeren en configureren van de Azure-netwerken Analytics-oplossing:

1. Inschakelen van de Netwerkbeveiligingsgroep Azure analytics-oplossing van [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).
2. Registratie voor diagnostische gegevens inschakelen de [Netwerkbeveiligingsgroep](../virtual-network/virtual-network-nsg-manage-log.md) resources die u wilt bewaken.

### <a name="enable-azure-network-security-group-diagnostics-in-the-portal"></a>Azure-netwerk beveiliging groep diagnostische gegevens in de portal inschakelen

1. Navigeer in de Azure-portal naar de bron van de Netwerkbeveiligingsgroep voor het bewaken van
2. Selecteer *diagnostische logboeken* om de volgende pagina te openen

   ![afbeelding van de Netwerkbeveiligingsgroep voor Azure resource](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. Klik op *diagnostische gegevens inschakelen* om de volgende pagina te openen

   ![afbeelding van de Netwerkbeveiligingsgroep voor Azure resource](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. Om diagnostische gegevens inschakelen, klikt u op *op* onder *Status*
5. Klik op het selectievakje voor *verzenden met Log Analytics*
6. Selecteer een bestaande werkruimte voor logboekanalyse of een werkruimte maken
7. Klik op het selectievakje onder **logboek** voor elk van de typen logboekbestanden te verzamelen
8. Klik op *opslaan* waarmee de logboekregistratie van diagnostische gegevens met Log Analytics

### <a name="enable-azure-network-diagnostics-using-powershell"></a>Schakel Azure netwerkdiagnose met PowerShell

De volgende PowerShell-script bevat een voorbeeld van het inschakelen van diagnostische logboekregistratie voor netwerkbeveiligingsgroepen
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Gebruik Azure Netwerkbeveiligingsgroep analytics
Nadat u op de **Netwerkbeveiligingsgroep Azure analytics** tegel in het overzicht kunt u samenvattingen van uw logboeken weergeven en ga vervolgens omlaag de gedetailleerde informatie voor de volgende categorieën:

* Netwerkbeveiligingsgroep geblokkeerd stromen
  * Netwerkbeveiligingsgroepen met geblokkeerde stromen
  * MAC-adressen met geblokkeerde stromen
* Netwerkbeveiligingsgroep stromen toegestaan
  * Netwerkbeveiligingsgroepen met toegestane stromen
  * MAC-adressen met toegestane stromen

![afbeelding van een dashboard met analytische Azure Netwerkbeveiligingsgroep](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![afbeelding van een dashboard met analytische Azure Netwerkbeveiligingsgroep](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

Op de **Netwerkbeveiligingsgroep Azure analytics** dashboard, Controleer de overzichtsgegevens in een van de blades en klik op eentje om gedetailleerde informatie weergeven over de zoekpagina logboek.

U kunt op elk van de zoekpagina logboek kunt resultaten weergeven door de tijd, gedetailleerde resultaten en uw Logboekgeschiedenis zoeken. U kunt ook filteren op facetten om de resultaten te beperken.

## <a name="migrating-from-the-old-networking-analytics-solution"></a>Migreren van de oude Networking Analytics-oplossing
In januari 2017, de ondersteunde manier van het verzenden van Logboeken vanaf Azure Toepassingsgateways en beveiligingsgroepen voor Azure-netwerk met logboekanalyse gewijzigd. Deze wijzigingen bieden de volgende voordelen:
+ Logboeken geschreven rechtstreeks met logboekanalyse zonder te hoeven gebruiken van een opslagaccount
+ Minder latentie vanaf het moment wanneer logboeken worden gegenereerd voor hen beschikbaar worden gesteld in Log Analytics
+ Minder configuratiestappen
+ Een algemene indeling voor alle typen Azure diagnostics

De bijgewerkte oplossingen gebruiken:

1. [Diagnostische gegevens worden verzonden met logboekanalyse rechtstreeks vanuit Azure Toepassingsgateways configureren](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [Diagnostische gegevens rechtstreeks met logboekanalyse worden verzonden vanuit Azure Network-beveiligingsgroepen configureren](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. Schakel de *Azure Application Gateway Analytics* en de *Azure Network Security groep Analytics* oplossing met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de Galerie met oplossingen](log-analytics-add-solutions.md)
3. Bijwerken van een opgeslagen query's, dashboards of waarschuwingen te gebruiken van het nieuwe gegevenstype
  + Type is het AzureDiagnostics. Het ResourceType kunt u filteren op Azure VPN-Logboeken.

    | In plaats van: | Gebruik: |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + Voor elk veld dat een achtervoegsel van \_s, \_d, of \_g in de naam wijzigen van het eerste teken in kleine letters
   + Voor elk veld dat een achtervoegsel van \_o in naam van de gegevens is opgesplitst in afzonderlijke velden op basis van de geneste veldnamen.
4. Verwijder de *Azure Networking Analytics (afgeschaft)* oplossing.
  + Als u met behulp van PowerShell, gebruikt u`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`

Gegevens verzameld voordat de wijziging niet zichtbaar in de nieuwe oplossing is. U kunt blijven opvragen voor deze gegevens met behulp van de oude Type en de veldnamen.

## <a name="troubleshooting"></a>Problemen oplossen
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-searches.md) gedetailleerde Azure diagnostics-gegevens om weer te geven.
