---
title: Overzicht van metrische gegevens in Microsoft Azure
description: Overzicht van metrische gegevens en het gebruik ervan in Microsoft Azure
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/05/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: d61ac48aa7c51bc4b215a7d56b1bbedfdc613f9f
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287553"
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Overzicht van metrische gegevens in Microsoft Azure
Dit artikel wordt beschreven wat metrische gegevens zijn in Microsoft Azure, hun voordelen en over het gebruik worden genomen.  

## <a name="what-are-metrics"></a>Wat zijn metrische gegevens?
Azure Monitor kunt u telemetrie voor inzicht in de prestaties en status van uw werklasten op Azure gebruiken. De belangrijkste type Azure telemetrische gegevens is de metrische gegevens die (ook wel prestatiemeteritems) gegenereerd door de meeste Azure-resources. Azure Monitor biedt verschillende manieren om te configureren en gebruiken van deze metrische gegevens voor controle en probleemoplossing.

## <a name="what-are-the-characteristics-of-metrics"></a>Wat zijn de kenmerken van metrische gegevens?
Metrische gegevens hebben de volgende kenmerken:

* Alle metrische gegevens zijn **frequentie van één minuut** (tenzij anders opgegeven in de definitie van een metrische waarde). U ontvangt een metrische waarde elke minuut van uw resource, waardoor u in vrijwel realtime inzicht in de status en de status van uw resource.
* Metrische gegevens zijn **beschikbaar onmiddellijk**. U hoeft niet te melden voor beveiligingsmeldingen of u aanvullende diagnostische gegevens instelt.
* U hebt toegang tot **93 dagen** voor elke metrische gegevens. U kunt snel de recente en maandelijkse trends in de prestaties of de status van uw resource kijken.
* Sommige metrische gegevens kan de naam / waarde-paar kenmerken met de naam hebben **dimensies**. Hiermee kunt u verder segmenteren en metrische gegevens op een meer betekenisvolle manier verkennen.

## <a name="what-can-you-do-with-metrics"></a>Wat kunt u doen met metrische gegevens?
Metrische gegevens kunt u de volgende taken uitvoeren:


- Configureren van een metrische waarde **Waarschuwing regel verzendt een melding en wordt automatische actie** wanneer de metriek de drempelwaarde die u hebt ingesteld. Acties worden beheerd via [actiegroepen](monitoring-action-groups.md). Voorbeeld van de acties omvatten e-mailadres, telefoonnummer en SMS-berichten, aanroepen van een webhook, starten van een runbook en nog veel meer. **Automatisch schalen** is een speciale geautomatiseerde actie waarmee u schaalt u bent een resource omhoog en omlaag te verwerken, maar houd de kosten lager wanneer deze niet onder belasting. U kunt een regel van de instelling voor automatisch schalen kunt in- of uitschalen configureren op basis van een metriek een drempelwaarde overschrijden.
- **Route** alle metrische gegevens moeten worden *Application Insights* of *Log Analytics* om in te schakelen via directe analyses, zoeken en aangepaste waarschuwingen voor metrische gegevens van uw resources. U kunt ook metrische gegevens om te streamen een *Event Hub*, zodat u kunt vervolgens deze route naar Azure Stream Analytics of aangepaste apps voor bijna realtime analyse. Instellen van Event Hub streamen met de diagnostische instellingen.
- **Archief** de geschiedenis van prestaties of de status van uw resource voor naleving, controle- of offline rapportagedoeleinden.  Wanneer u diagnostische instellingen voor uw resource configureren, kunt u uw metrische gegevens doorsturen naar Azure Blob-opslag.
- Gebruik de **Azure-portal** om te detecteren, openen en weergeven van alle metrische gegevens wanneer u een resource selecteren en de metrische gegevens in een grafiek te tekenen. U kunt de prestaties van uw resource (zoals een virtuele machine, de website of de logische app) bijhouden door grafiek aan uw dashboard vast te maken.  
- **Geavanceerde analyses uitvoeren** of rapporten op trends die grote prestaties of het gebruik van uw resource.
- **Query** metrische gegevens met behulp van de PowerShell-cmdlets of de REST-API voor Cross-Platform.
- **Verbruiken** de metrische gegevens via de nieuwe Azure Monitor REST-API's.

  ![Routering van metrische gegevens in Azure Monitor](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-the-portal"></a>Toegang tot metrische gegevens via de portal
Hieronder volgt een snel overzicht van het maken van een grafiek met metrische gegevens met behulp van de Azure-portal.

### <a name="to-view-metrics-after-creating-a-resource"></a>Om metrische gegevens na het maken van een resource weer te geven
1. Open Azure Portal.
2. Maak een Azure App Service-website.
3. Nadat u een website maakt, gaat u naar de **overzicht** blade van de website.
4. Vindt u nieuwe metrische gegevens als een **bewaking** tegel. U kunt vervolgens de tegel bewerken en meer metrische gegevens selecteren.

   ![Metrische gegevens over een resource in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="to-access-all-metrics-in-a-single-place"></a>Voor toegang tot alle metrische gegevens op één plek
1. Open Azure Portal.
2. Navigeer naar de nieuwe **Monitor** tabblad, en vervolgens en selecteer de **metrische gegevens** optie eronder.
3. Selecteer uw abonnement, resourcegroep en de naam van de resource in de vervolgkeuzelijst.
4. De lijst met beschikbare metrische gegevens weergeven. Selecteer vervolgens de metriek u geïnteresseerd bent in en het tekenen.
5. U kunt deze vastmaken aan het dashboard door te klikken op de pincode in de rechterbovenhoek.

   ![Toegang tot alle metrische gegevens op één plek in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> U kunt toegang tot hostniveau metrische gegevens van virtuele machines (op een Azure Resource Manager gebaseerde) en virtuele-machineschaalsets zonder eventuele extra diagnostische instellingen. Deze nieuwe hostniveau metrische gegevens zijn beschikbaar voor Windows en Linux-instanties. Deze metrische gegevens zijn niet te verwarren met de metrische gegevens over de Gast-OS-niveau dat u toegang hebt tot wanneer u Azure diagnostische gegevens op uw virtuele machines of virtual machine scale sets inschakelen. Zie voor meer informatie over het configureren van diagnostische gegevens, [wat is Microsoft Azure Diagnostics](../azure-diagnostics.md).
>
>

Azure Monitor biedt ook een nieuwe metrische gegevens voor grafieken beschikbaar is in preview. Deze ervaring kan gebruikers overlay metrische gegevens van meerdere resources in een grafiek. Gebruikers kunnen ook worden getekend, segment, en multi-dimensionale metrische gegevens met behulp van deze nieuwe metrische gegevens voor grafieken ervaring filteren. Voor meer informatie [Klik hier](https://aka.ms/azuremonitor/new-metrics-charts)

## <a name="access-metrics-via-the-rest-api"></a>Toegang tot metrische gegevens via de REST-API
Metrische gegevens van Azure kunnen worden geopend via de Azure Monitor-API's. Er zijn twee API's die u helpen detecteren en toegang tot metrische gegevens:

* Gebruik de [definities voor metrische gegevens van Azure Monitor REST-API](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) voor toegang tot de lijst met metrische gegevens en dimensies, die beschikbaar zijn voor een service.
* Gebruik de [REST-API van Azure Monitor Metrics](https://docs.microsoft.com/rest/api/monitor/metrics) segmenteren, filteren en toegang tot de werkelijke metrische gegevens.

> [!NOTE]
> In dit artikel bevat informatie over de metrische gegevens via de [nieuwe API voor metrische gegevens](https://docs.microsoft.com/rest/api/monitor/) voor Azure-resources. De API-versie voor de metrische definities van de nieuwe en metrische gegevens API's is 2018-01-01. De metrische definities van verouderde en metrische gegevens kunnen worden geopend met de API-versie 2014-04-01.
>
>

Zie voor een meer gedetailleerd overzicht met de REST-API van Azure Monitor, [Azure Monitor REST API-overzicht](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Metrische gegevens exporteren
Gaat u naar de **diagnostische instellingen** blade onder de **Monitor** tabblad en de opties voor exporteren voor metrische gegevens weer te geven. Kunt u metrische gegevens (en diagnostische logboeken) naar Blob-opslag, worden doorgestuurd naar Azure Event Hubs of Log Analytics voor use cases die eerder in dit artikel zijn vermeld.

 ![Opties voor exporteren voor metrische gegevens in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)

U kunt dit configureren via Resource Manager-sjablonen, [PowerShell](insights-powershell-samples.md), [Azure CLI](insights-cli-samples.md), of [REST-API's](https://msdn.microsoft.com/library/dn931943.aspx).

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de meetwaarde 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

## <a name="take-action-on-metrics"></a>Actie ondernemen voor metrische gegevens
Voor het ontvangen van meldingen of geautomatiseerde acties uitvoeren op de metrische gegevens, kunt u regels voor waarschuwingen of instellingen voor automatisch schalen configureren.

### <a name="configure-alert-rules"></a>Regels voor waarschuwingen configureren
U kunt regels voor waarschuwingen over metrische gegevens configureren. Deze regels voor waarschuwingen kunnen controleren als een metriek een bepaalde drempelwaarde is overschreden. Er zijn twee metrische waarschuwingen mogelijkheden die worden aangeboden door Azure Monitor.

Metrische waarschuwingen: ze vervolgens een melding via e-mail of een webhook die kan worden gebruikt om uit te voeren van een aangepast script worden geactiveerd. U kunt ook de webhook configureren integraties product van derden gebruiken.

 ![Metrische gegevens en regels voor waarschuwingen in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

Nieuwere metrische waarschuwingen hebben de mogelijkheid om te controleren van meerdere metrische gegevens en drempelwaarden voor een resource op de hoogte gesteld via een [actiegroep](monitoring-action-groups.md). Meer informatie over [nieuwere waarschuwingen hier](https://aka.ms/azuremonitor/near-real-time-alerts).


### <a name="autoscale-your-azure-resources"></a>Automatisch schalen van uw Azure-resources
Sommige Azure-resources ondersteuning voor het schalen of van meerdere exemplaren voor het afhandelen van uw workloads. Automatisch schalen is van toepassing op App Service (Web-Apps), virtuele-machineschaalsets en klassieke Azure-Cloudservices. U kunt de regels voor automatisch schalen om te schalen of bij een bepaalde gevolgen heeft voor uw workload metriek een drempelwaarde overschrijdt die u opgeeft. Zie voor meer informatie, [overzicht van automatisch schalen](monitoring-overview-autoscale.md).

 ![Metrische gegevens en voor automatisch schalen in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Meer informatie over ondersteunde services en metrische gegevens
U vindt een gedetailleerde lijst met alle ondersteunde services en de metrische gegevens in [Azure Monitor metrics--ondersteunde metrische gegevens per resourcetype](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de koppelingen in dit artikel. Bovendien leert u over:  

* [Algemene metrische gegevens voor automatisch schalen](insights-autoscale-common-metrics.md)
* [Over het maken van regels voor waarschuwingen](insights-alerts-portal.md)
* [Logboeken van Azure storage met Log Analytics analyseren](../log-analytics/log-analytics-azure-storage.md)
