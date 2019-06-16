---
title: Het oplossen van Azure Monitor grafieken met metrische gegevens
description: Problemen met het maken, aanpassen of grafieken met metrische gegevens interpreteren
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: cff1bbefc3c54f7f9c02c646bd9eef528fe28c73
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64939572"
---
# <a name="troubleshooting-metrics-charts"></a>Oplossen van problemen met grafieken met metrische gegevens

Gebruik dit artikel als u problemen ondervindt met het maken, aanpassen of interpreteren grafieken in Verkenner voor metrische gegevens van Azure. Als u niet bekend bent met metrische gegevens, meer informatie over [aan de slag met metrics explorer](metrics-getting-started.md) en [geavanceerde functies van metrics explorer](metrics-charts.md). U kunt ook zien [voorbeelden](metric-chart-samples.md) van de geconfigureerde grafieken met metrische gegevens.

## <a name="cant-find-your-resource-to-select-it"></a>Uw resource om deze te selecteren niet kunt vinden

U hebt geklikt op de **selecteert u een resource** knop, maar niet uw resource in het dialoogvenster van de datumkiezer resource wordt weergegeven.

**Oplossing:** Metrics explorer moet u abonnementen en resourcegroepen selecteren voordat de lijst van beschikbare resources. Als u niet uw resource ziet:

1. Zorg ervoor dat u hebt geselecteerd dat juiste abonnement in de **abonnement** vervolgkeuzelijst. Als uw abonnement niet wordt weergegeven, klikt u op de **map + abonnement instellingen** en een abonnement met de resource toe te voegen.

1. Zorg ervoor dat u de juiste resourcegroep hebt geselecteerd.
    > [!WARNING]
    > Voor de beste prestaties wanneer u metrics explorer voor het eerst opent de **resourcegroep** vervolgkeuzelijst heeft geen vooraf geselecteerde resourcegroepen. U moet ten minste één groep kiezen voordat u resources kunt zien.

## <a name="chart-shows-no-data"></a>Grafiek bevat geen gegevens

De grafieken kunnen soms geen gegevens worden weergegeven na het selecteren van juiste bronnen en metrische gegevens. Dit gedrag kan worden veroorzaakt door verschillende van de volgende redenen:

### <a name="microsoftinsights-resource-provider-isnt-registered-for-your-subscription"></a>De resourceprovider Microsoft.Insights is niet geregistreerd voor uw abonnement

Metrische gegevens verkent vereist *Microsoft.Insights* -resourceprovider geregistreerd in uw abonnement. In veel gevallen, is deze (dat wil zeggen, nadat u een waarschuwingsregel configureren, diagnostische instellingen voor elke resource aanpassen of een regel voor automatisch schalen configureren) automatisch geregistreerd. Als de resourceprovider Microsoft.Insights niet is geregistreerd, moet u handmatig dit registreren door de volgende stappen wordt beschreven in [Azure resourceproviders en typen](../../azure-resource-manager/resource-manager-supported-services.md).

**Oplossing:** Open **abonnementen**, **resourceproviders** tabblad en Controleer *Microsoft.Insights* voor uw abonnement is geregistreerd.

### <a name="you-dont-have-sufficient-access-rights-to-your-resource"></a>Er niet voldoende rechten voor uw resource

In Azure, toegang tot metrische gegevens wordt beheerd door [op rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/overview.md). U moet lid zijn van [controlelezer](../../role-based-access-control/built-in-roles.md#monitoring-reader), [controlebijdrager](../../role-based-access-control/built-in-roles.md#monitoring-contributor), of [Inzender](../../role-based-access-control/built-in-roles.md#contributor) verkennen van metrische gegevens voor elke resource.

**Oplossing:** Zorg ervoor dat u hebt onvoldoende machtigingen voor de bron van waaruit u metrische gegevens verkent.

### <a name="your-resource-didnt-emit-metrics-during-the-selected-time-range"></a>Uw resource is niet metrische gegevens verzenden tijdens het geselecteerde tijdsbereik

Sommige resources verzenden niet voortdurend hun metrische gegevens. Azure wordt bijvoorbeeld niet verzamelen van metrische gegevens voor gestopte virtuele machines. Andere bronnen mogelijk hun metrische gegevens verzenden alleen als bepaalde voordoet. Bijvoorbeeld: heeft een metrische waarde verwerkingstijd van een transactie met ten minste één transactie. Als er geen transacties in de geselecteerde periode, kan de grafiek op een natuurlijke manier niet leeg zijn. Bovendien de meeste van de metrische gegevens in Azure, elke minuut worden verzameld, maar er zijn enkele die minder vaak worden verzameld. Zie de documentatie van de metrische gegevens voor meer informatie over de metrische gegevens die u probeert te verkennen.

**Oplossing:** Wijzigen van de tijd van de grafiek naar een breder bereik. U kunt starten van "Afgelopen 30 dagen" met behulp van een grotere tijdgranulatie (of afhankelijk zijn van de optie 'Automatische tijdgranulatie').

### <a name="you-picked-a-time-range-greater-than-30-days"></a>U hebt geselecteerd een tijdsbereik groter is dan 30 dagen

[De meeste metrische gegevens in Azure worden opgeslagen voor 93 dagen](data-platform-metrics.md#retention-of-metrics). U kunt echter alleen query niet meer dan 30 dagen waard is om van gegevens op één grafiek. Deze beperking geldt niet voor [logboek op basis van metrische gegevens](../app/pre-aggregated-metrics-log-metrics.md#log-based-metrics).

**Oplossing:** Als u een leeg diagram ziet of de grafiek alleen deel uitmaken van de metrische gegevens worden weergegeven, controleert u of dat het verschil tussen de begin - en end-datums in de tijdkiezer niet groter is dan het interval van 30 dagen.

### <a name="all-metric-values-were-outside-of-the-locked-y-axis-range"></a>Alle metrische waarden zijn buiten het bereik vergrendelde y-as

Door [vergrendelen van de grenzen van de grafiek y-as](metrics-charts.md#lock-boundaries-of-chart-y-axis), kunt u per ongeluk maken het grafiekgebied van de weergave de regel van de grafiek niet weergeven. Bijvoorbeeld, als de y-as is gekoppeld aan een bereik tussen 0 en 50% en de metriek een constante waarde van 100 is %, wordt de regel wordt altijd buiten het zichtbaar gebied, waardoor de grafiek leeg weergegeven.

**Oplossing:** Controleer of dat de grenzen van de y-as van de grafiek niet vergrendeld buiten het bereik van de metrische waarden. Als de grenzen van de y-as zijn vergrendeld, kunt u tijdelijk opnieuw instellen om ervoor te zorgen dat de metrische waarden buiten het bereik van de grafiek niet vallen. Vergrendeling van het bereik van y-as wordt niet aanbevolen met automatische granulariteit voor de grafieken met **som**, **min**, en **max** aggregatie omdat de waarden worden gewijzigd met granulatie door vergroten of verkleinen browservenster of als een schermresolutie gaan. Switch granulariteit mogelijk het weergavegebied van uw grafiek leeg laten.

### <a name="you-are-looking-at-a-guest-os-metric-but-didnt-enable-azure-diagnostic-extension"></a>U bekijkt een Gastbesturingssysteem metrische gegevens, maar diagnostische Azure-extensie niet hebt ingeschakeld

Verzameling van **Guest OS** metrische gegevens is vereist voor het configureren van de Azure Diagnostics-extensie of inschakelen met behulp van de **diagnostische instellingen** Configuratiescherm voor uw resource.

**Oplossing:** Als Azure Diagnostics-extensie is ingeschakeld, maar u nog steeds niet voor de metrische gegevens, volgt u de stappen die worden beschreven [probleemoplossingsgids voor Azure Diagnostics-extensie](diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal). Zie ook de stappen voor probleemoplossing voor [Guest OS-naamruimte en metrische gegevens kan niet worden opgenomen](metrics-troubleshoot.md#cannot-pick-guest-os-namespace-and-metrics)

## <a name="error-retrieving-data-message-on-dashboard"></a>'Fout bij het ophalen van gegevens'-bericht op dashboard

Dit probleem is gebruikelijk wanneer uw dashboard is gemaakt met een metrische waarde die hoger is afgeschaft en verwijderd uit Azure. Om te bevestigen dat dit het geval is, opent u de **metrische gegevens** tabblad van uw resource en controleer de beschikbare metrische gegevens in de kiezer voor metrische gegevens. Als de metriek niet wordt weergegeven, is de metrische gegevens uit Azure verwijderd. Meestal wanneer een statistiek is afgeschaft, is er een betere nieuwe metrische gegevens die met een vergelijkbare perspectief op de resourcestatus biedt.

**Oplossing:** De tegel mislukt bijwerken door het verzamelen van een alternatieve metrische gegevens voor de grafiek op het dashboard. U kunt [Bekijk een lijst met beschikbare metrische gegevens voor Azure-services](metrics-supported.md).

## <a name="chart-shows-dashed-line"></a>Grafiek wordt een gestreepte lijn

Onderbroken lijnstijl grafieken met metrische gegevens van Azure gebruiken om aan te geven dat er een ontbrekende waarde (ook wel bekend als ' null-waarde') tussen twee bekende-tijdseenheid gegevenspunten. Bijvoorbeeld, als in de selectie van de tijd die u hebt geselecteerd tijdgranulatie 'minuut', maar de metriek is gerapporteerd op 07:26, 07:27 07:29 en 07:30 (Houd er rekening mee een minuut hiaat tussen de tweede en derde gegevenspunten), een gestreepte lijn 07:27 en 07:29 verbinding maken en een ononderbroken lijn verbinding alle andere gegevenspunten. De stippellijn val tot nul wanneer de metriek gebruikt **aantal** en **som** aggregatie. Voor de **avg**, **min** of **max** aggregaties, de stippellijn verbindt twee dichtstbijzijnde bekende gegevenspunten. Ook als de gegevens op de meest rechtse of uiterst rechts van de grafiek ontbreekt, breidt de stippellijn in de richting van het gegevenspunt ontbreekt.
  ![de installatiekopie van de metrische gegevens](./media/metrics-troubleshoot/missing-data-point-line-chart.png)

**Oplossing:** Dit gedrag is standaard. Dit is handig voor het identificeren van ontbrekende gegevenspunten. Het lijndiagram is een uitstekende keuze voor het visualiseren van trends van hoge dichtheid metrische gegevens, maar mogelijk moeilijk te interpreteren voor de metrische gegevens met sparse waarden, met name wanneer corelating waarden met tijdsinterval belangrijk is. De stippellijn vergemakkelijkt het lezen van deze grafieken, maar als uw grafiek nog steeds niet duidelijk is, kunt u overwegen uw metrische gegevens weergeven met een ander grafiektype. Bijvoorbeeld, een grafiek verspreid plot voor de dezelfde metrische gegevens geeft duidelijk aan elk tijdsinterval door alleen een punt visualiseren wanneer er een waarde en overslaan van de gegevens kan worden overgeslagen wanneer de waarde ontbreekt: ![metrische installatiekopie](./media/metrics-troubleshoot/missing-data-point-scatter-chart.png)

   > [!NOTE]
   > Als u nog steeds een lijndiagram liever voor uw metrische gegevens, kan verplaatsen van de muis boven de grafiek helpen om de tijdgranulatie door het markeren van het gegevenspunt op de locatie van de muisaanwijzer vast te stellen.

## <a name="chart-shows-unexpected-drop-in-values"></a>Grafiek bevat onverwachte afname van waarden

In veel gevallen is de waargenomen afname van de metrische waarden een misverstand van de gegevens in de grafiek wordt weergegeven. U kunt worden misleiden door een afname in optellingen of telt wanneer de grafiek toont de meest recente minuten omdat de laatste punten van de metrische gegevens niet zijn ontvangen of verwerkt door Azure nog. Afhankelijk van de service kan de latentie van de verwerking van metrische gegevens zijn binnen het bereik van een paar minuten. Voor grafieken met een recente tijdsbereik met een granulariteit van 1 of 5 minuten, een afname van de waarde in de afgelopen paar minuten wordt opvallender: ![metrische installatiekopie](./media/metrics-troubleshoot/drop-in-values.png)

**Oplossing:** Dit gedrag is standaard. Wij vinden dat gegevens worden weergegeven zodra we ontvangen is het nuttig, zelfs wanneer de gegevens *gedeeltelijke* of *onvolledig*. In dat geval kunt u belangrijke Conclusie sneller en meteen onderzoek starten. Bijvoorbeeld, voor een metrische waarde waarin het aantal fouten worden weergegeven, ziet een gedeeltelijke waarde X ziet u dat er ten minste X fouten op een opgegeven minuut. U kunt onderzoek het probleem direct uit te voeren, plaats wacht om te zien van de exacte telling van fouten die zich op deze minuut, die misschien niet zo belangrijk voorgedaan. De grafiek wordt bijgewerkt zodra we de volledige set gegevens ontvangen, maar op dat moment kan het ook nieuwe onvolledige gegevenspunten van meer recente minuten weergeven.

## <a name="cannot-pick-guest-os-namespace-and-metrics"></a>Gast-OS-naamruimte en metrische gegevens kan niet worden opgenomen

Virtuele machines en schaalsets voor virtuele machines hebt twee soorten metrische gegevens: **Virtuele-Machinehost** metrische gegevens die door de Azure hosting-omgeving, worden verzameld en **Guest OS** metrische gegevens die worden verzameld door de [bewakingsagent](agents-overview.md) die worden uitgevoerd op uw virtuele machines. De installatie controleren door in te schakelen [diagnostische Azure-extensie](diagnostics-extension-overview.md).

Standaard Gastbesturingssysteem metrische gegevens worden opgeslagen in Azure Storage-account, die u hebt gekozen in de **diagnostische instellingen** tab van uw resource. Als het Gastbesturingssysteem metrische gegevens worden niet verzameld of metrics explorer geen toegang tot deze, ziet u alleen de **virtuele-Machinehost** metrische naamruimte:

![de installatiekopie van de metrische gegevens](./media/metrics-troubleshoot/cannot-pick-guest-os-namespace.png)

**Oplossing:** Als er geen **Guest OS** naamruimte en metrische gegevens in metrics explorer:

1. Bevestig dat [diagnostische Azure-extensie](diagnostics-extension-overview.md) is ingeschakeld en geconfigureerd voor het verzamelen van metrische gegevens.
    > [!WARNING]
    > U kunt geen gebruiken [Log Analytics-agent](agents-overview.md#log-analytics-agent) (ook wel de Microsoft Monitoring Agent, of 'MMA' genoemd) voor het verzenden van **Guest OS** in een storage-account.

1. Controleer of dat de storage-account is niet beveiligd door de firewall.

1. Gebruik de [Azure storage explorer](https://azure.microsoft.com/features/storage-explorer/) om te valideren of de metrische gegevens over in de storage-account stromen. Als u metrische gegevens worden niet verzameld, voert u de [probleemoplossingsgids voor Azure Diagnostics-extensie](diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal).

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over aan de slag met Metric Explorer](metrics-getting-started.md)
* [Meer informatie over geavanceerde functies van Metric Explorer](metrics-charts.md)
* [Een lijst met beschikbare metrische gegevens voor Azure-services](metrics-supported.md)
* [Zie de voorbeelden van geconfigureerde grafieken](metric-chart-samples.md)