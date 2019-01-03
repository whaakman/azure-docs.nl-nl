---
title: Geïntegreerde waarschuwingen en -bewaking in Azure Monitor vervangt klassieke waarschuwingen en -bewaking
description: Overzicht van buiten gebruik stellen van klassieke bewakingsservices en functionaliteit en eerder wordt weergegeven in Azure portal onder waarschuwingen (klassiek). Klassieke waarschuwingen en -bewaking bevat klassieke metrische waarschuwingen voor Azure-resources, klassieke metrische waarschuwingen voor Application Insights, klassieke webtest waarschuwingen voor Application Insights, klassieke aangepaste metrische gegevens op basis van waarschuwingen voor Application Insights en het klassieke model waarschuwingen voor Application Insights SmartDetection v1
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: eae0dc1d38b06a451091d79946bc95ed722ac861
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994811"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Geïntegreerde waarschuwingen en -bewaking in Azure Monitor vervangt klassieke waarschuwingen en -bewaking

Azure Monitor is nu een uniforme volledige stack monitoring-service, die nu ondersteuning biedt voor één metrische gegevens en 'Een waarschuwingen' voor resources; geworden. Zie voor meer informatie onze [blogbericht over nieuwe Azure Monitor](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/). De nieuwe Azure bewaking en waarschuwingen platforms is gemaakt om te worden sneller, slimmer en uitbreidbare: keeping hoog tempo aan de groeiende expanse van cloud computing en in-regel met de intelligente Cloud Microsoft filosofie. 

Met de nieuwe Azure-bewaking en waarschuwingen platform op locatie zijn ingesteld, we zullen worden buiten gebruik stellen van het "klassieke" bewaking en waarschuwingen platform, die worden gehost in *klassieke waarschuwingen weergeven* sectie van de Azure-waarschuwingen, **juni worden afgeschaft 2019**.

 ![Klassieke waarschuwing in Azure portal](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

We raden u aan de slag en opnieuw maken van uw waarschuwingen in het nieuwe platform. Voor klanten met een groot aantal waarschuwingen kunnen wij werken aan een geautomatiseerde manier om te verplaatsen van bestaande klassieke waarschuwingen naar de nieuwe waarschuwingensysteem zonder onderbreking of kosten toegevoegd.

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Geïntegreerde metrische gegevens en waarschuwingen in Application Insights

Azure Monitor nieuwere metrische platform wordt nu bewaking afkomstig van Application Insights voor energiebeheer. Deze verplaatsing betekent dat Application Insights wordt van een toepassing aansluiten op actiegroepen, zodat nog veel meer opties dan alleen de vorige e-mail en webhook aanroepen. Waarschuwingen kunnen nu activeren spraakoproepen, Azure Functions, Logic Apps, SMS en ITSM-hulpprogramma's zoals ServiceNow en Automation-Runbooks. Met kan het nieuwe platform in de buurt van real-time bewaking en waarschuwingen, Application Insights gebruikers gebruikmaken van de technologie waarop bewaking buiten de grenzen van andere Azure-resources en versterking van bewaking van Microsoft-producten.

De nieuwe geïntegreerde bewaking en waarschuwingen voor Application Insights bevat:

- **Metrische gegevens van Application Insights-Platform** – waarmee u populaire vooraf gemaakte metrische gegevens van Application Insights-product. Zie dit artikel voor meer informatie over het gebruik van [Platform metrische gegevens voor Application Insights op de nieuwe Azure Monitor](../../application-insights/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics).
- **Application Insights beschikbaarheid en Web-test** -waarmee u de mogelijkheid om de reactiesnelheid en beschikbaarheid van uw web-app of de server vast te stellen. Zie dit artikel voor meer informatie over het gebruik van [Beschikbaarheidstests en waarschuwingen voor Application Insights op de nieuwe Azure Monitor](../../azure-monitor/app/monitor-web-app-availability.md).
- **Metrische gegevens van Application Insights aangepaste** – Hiermee kunt u definiëren en hun eigen metrische gegevens voor controle en meldingen te verzenden. Zie dit artikel voor meer informatie over het gebruik van [aangepaste metrische gegevens voor Application Insights op de nieuwe Azure Monitor](../../application-insights/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation).
- **Application Insights-Foutafwijkingen (onderdeel van Slimme detectie)** – die automatisch ontvangt u een melding in bijna realtime als er een abnormale toename van mislukte aanvragen voor HTTP- of afhankelijkheidsaanroepen optreedt in uw web-app. Foutafwijkingen (onderdeel van Slimme detectie) als onderdeel van de nieuwe Azure Monitor, Application Insights is binnenkort beschikbaar en we zullen dit document bijwerken met koppelingen op de volgende iteratie als deze is geïmplementeerd-out in de komende maanden.

## <a name="unified-metrics--alerts-for-other-azure-resources"></a>Geïntegreerde metrische gegevens en waarschuwingen voor andere Azure-resources

Sinds maart 2018 zijn de volgende generatie van waarschuwingen en multi-dimensionale bewaking voor Azure-resources in de beschikbaarheid. Het platform voor nieuwere metrische gegevens en waarschuwingen, is nu sneller met mogelijkheden voor near-real-time. De nieuwere metrische platform waarschuwingen bieden meer granulatie, nog belangrijker, zoals de nieuwere platform biedt de mogelijkheid van dimensies, waarmee u segment te filteren op specifieke waarde combinatie, voorwaarde of bewerking. Net als alle waarschuwingen in de nieuwe Azure Monitor zijn de nieuwere metrische waarschuwingen meer uit te breiden met het gebruik van ActionGroups – zodat u verder uitgebreid dan e-mailadres of webhook tot SMS, spraak, Azure-functie, Automation-Runbook en meer.
Nieuwere metrische gegevens voor Azure-resources zijn beschikbaar als:

- **Metrische gegevens het platform van Azure Monitor standaard** – waarmee u populaire vooraf gevulde metrische gegevens uit verschillende Azure-services en producten. Zie voor meer informatie in dit artikel op [ondersteunde metrische gegevens in Azure Monitor](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported) en [metrische waarschuwingen ondersteuning op Azure Monitor](../../azure-monitor/platform/alerts-metric-overview.md#supported-resource-types-for-metric-alerts).
- **Metrische gegevens van Azure Monitor aangepaste** – waarmee metrische gegevens van de gebruiker gestuurd bronnen, met inbegrip van de Azure Diagnostics-agent. Zie voor meer informatie in dit artikel op [aangepaste metrische gegevens in Azure Monitor](../../azure-monitor/platform/metrics-custom-overview.md). Met aangepaste metrische gegevens, kunt u ook publiceren metrische gegevens die worden verzameld door [Windows Azure Diagnostics-agent](../../azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm.md) en [InfluxData Telegraf agent](../../azure-monitor/platform/collect-custom-metrics-linux-telegraf.md).

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>Buiten gebruik stellen van klassieke bewaking en waarschuwingen platform

Zoals gezegd, het klassieke bewaking en waarschuwingen platform op dit moment kan worden gebruikt vanuit de [waarschuwingen (klassiek) sectie](../../azure-monitor/platform/alerts-classic.overview.md) van Azure portal wordt beëindigd in de komende maanden gegeven ze zijn vervangen door de nieuwere systeem.
Oudere klassiek bewaking en waarschuwingen wordt buiten gebruik gesteld op 30 juni 2019; het sluiten van gerelateerde API's, de interface van de Azure portal en de Services inclusief erin. Om precies worden deze functies afgeschaft:

- Oudere (klassiek) metrische gegevens en waarschuwingen voor Azure-resources als op dit moment beschikbaar zijn via [waarschuwingen (klassiek) sectie](../../azure-monitor/platform/alerts-classic.overview.md) van Azure portal; toegankelijk als [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) resource
- Oudere (klassiek) platform en aangepaste metrische gegevens voor Application Insights, evenals waarschuwingen op deze als op dit moment beschikbaar zijn via [waarschuwingen (klassiek) sectie](../../azure-monitor/platform/alerts-classic.overview.md) van Azure portal en toegankelijk als [microsoft.insights/ alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) resource
- Oudere (klassiek) Foutafwijkingen waarschuwing momenteel beschikbaar als [Slimme detectie in Application Insights](../../application-insights/app-insights-proactive-diagnostics.md) in de Azure-portal; met waarschuwingen geconfigureerd wordt weergegeven in [waarschuwingen (klassiek) sectie](../../azure-monitor/platform/alerts-classic.overview.md) van Azure Portal

Alle klassieke voor bewaking en waarschuwingen van systemen, met inbegrip van bijbehorende [API](https://msdn.microsoft.com/library/azure/dn931945.aspx), [PowerShell](../../azure-monitor/platform/alerts-classic-portal.md), [CLI](../../azure-monitor/platform/alerts-classic-portal.md), [Azure portal-pagina](../../azure-monitor/platform/alerts-classic-portal.md), en [ Resource-sjabloon](../../azure-monitor/platform/alerts-enable-template.md) bruikbaar blijven tot het einde van juni 2019. 

Aan het einde van juni 2019, in Azure Monitor:

- Klassieke service voor bewaking en waarschuwingen worden buiten gebruik is gesteld en niet meer beschikbaar zijn voor het maken van nieuwe regels voor waarschuwingen
- Geen waarschuwingsregels die blijven bestaan in waarschuwingen (klassiek) buiten juni 2019 blijven uitvoeren en meldingen worden geactiveerd, maar niet meer beschikbaar voor de wijziging.
- Vanaf juli 2019, worden geen waarschuwingsregels in klassieke bewaking en waarschuwingen automatisch gemigreerd door Microsoft aan het equivalent hiervan in het nieuwe Azure monitor-platform. Het proces wordt naadloze zonder uitvaltijd en klanten hebben geen invloed op de bewaking van de dekking.

Hulpprogramma's kunt u uw waarschuwingen van vrijwillig migratie zal binnenkort worden geboden [waarschuwingen (klassiek) sectie](../../azure-monitor/platform/alerts-classic.overview.md) van Azure-portal naar de nieuwe Azure-waarschuwingen. Alle regels die zijn geconfigureerd in waarschuwingen (klassiek) die worden gemigreerd naar nieuwe Azure Monitor, gratis blijven en niet in rekening gebracht. Gemigreerde regels voor klassieke waarschuwingen worden kosten in rekening gebracht voor het pushen van meldingen via e-mail, webhook of LogicApp ook niet voorzien. Gebruik van de nieuwere melding of actie typen (zoals SMS, Spraakoproep, ITSM-integratie, enzovoort) is gebracht of toegevoegd aan een gemigreerde of een nieuwe waarschuwing. Zie voor meer informatie, [prijzen voor Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

Bovendien de volgende zijn factureerbaar onder de onderwerpen in [prijzen voor Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/):

- Een nieuwe (niet-gemigreerde) waarschuwingsregel meer dan gratis eenheden, op het nieuwe platform van Azure Monitor
- Alle gegevens die zijn opgenomen en behouden na door Azure Monitor inbegrepen gratis eenheden
- Alle webtests met meerdere test uitgevoerd door Application Insights
- Een aangepaste metrische gegevens die zijn opgeslagen buiten de gratis eenheden die zijn opgenomen in Azure Monitor

In dit artikel worden de details met betrekking tot de nieuwe Azure-bewaking en waarschuwingen functionaliteit, evenals de beschikbaarheid van extra gebruikers helpen bij het overstappen op het nieuwe platform van Azure Monitor & koppelingen wordt voortdurend bijgewerkt.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [nieuwe geïntegreerde Azure Monitor](../../azure-monitor/overview.md).
* Meer informatie over de nieuwe [Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md).
