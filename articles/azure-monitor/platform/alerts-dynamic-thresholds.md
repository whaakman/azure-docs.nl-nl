---
title: Het maken van waarschuwingen met dynamische drempelwaarden in Azure Monitor
description: Waarschuwingen maken met machine learning op basis van dynamische drempelwaarden
author: yanivlavi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: yalavi
ms.reviewer: mbullwin
ms.openlocfilehash: 4024ecddde4b0d020e2c657214a4a258ea0b2ea5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449007"
---
# <a name="metric-alerts-with-dynamic-thresholds-in-azure-monitor-public-preview"></a>Metrische waarschuwingen met dynamische drempelwaarden in Azure Monitor (Preview-versie)

Waarschuwing voor metrische gegevens met dynamische drempelwaarden detectie maakt gebruik van geavanceerde machine learning (ML) meer historische gedrag van de metrische gegevens, patronen en afwijkingen die wijzen op problemen met de mogelijke service kunt identificeren. Deze biedt ondersteuning van zowel een eenvoudige gebruikersinterface en bewerkingen op schaal door toe te staan gebruikers waarschuwingsregels via de Azure Resource Manager-API op een volledig geautomatiseerde manier configureren.

Zodra een waarschuwingsregel is gemaakt, er volgt een waarschuwing alleen wanneer de bewaakte metrische gegevens niet wordt gedragen zich zoals verwacht, op basis van de op maat gemaakte drempelwaarden.

Wij willen graag uw feedback horen, bewaart u deze binnenkort op azurealertsfeedback@microsoft.com.

## <a name="why-and-when-is-using-dynamic-condition-type-recommended"></a>Waarom en wanneer is met behulp van dynamische voorwaardetype aanbevolen?

1. **Schaalbare waarschuwen** : dynamische drempelwaarden voor waarschuwingen, regels kunnen maken die zijn afgestemd drempelwaarden voor honderden metrische reeksen tegelijkertijd. Het leveren van nog net zo gemakkelijk van het definiëren van een waarschuwingsregel op één metrische waarde. Met behulp van de UI of de API van Azure Resource Manager-resultaten in minder waarschuwingsregels voor het beheren van. De schaalbare aanpak is vooral nuttig wanneer omgaan met metrische dimensies of toe te passen op meerdere resources, zoals alle resources van het abonnement. Dit wordt omgezet in een aanzienlijke tijd bespaart op beheer en het maken van regels voor waarschuwingen. [Meer informatie over het configureren van waarschuwingen voor metrische gegevens met dynamische drempelwaarden met behulp van sjablonen](alerts-metric-create-templates.md).

1. **Slimme metriek patroonherkenning** – met behulp van onze unieke ML-technologie, kunnen we automatisch detecteren van metrische patronen en gedurende een periode, die vaak seizoensgebondenheid (per uur / dagelijks / wekelijks) kan worden aangepast aan metrische wijzigingen. Aanpassing aan gedrag de metrische gegevens voor tijd en waarschuwingen op basis van de afwijkingen ten opzichte van het patroon ervan neemt de belasting van de wetenschap dat het 'recht' drempelwaarde voor elke metrische gegevens. Het ML-algoritme dat wordt gebruikt in dynamische drempelwaarden is ontworpen om te voorkomen dat ruis (geringe precisie) of hele (laag intrekken)-drempelwaarden die een verwacht patroon geen hebt.

1. **Configuratie van intuïtieve** : dynamische drempelwaarden toestaan instellen metrische waarschuwingen met behulp van geavanceerde concepten, moet uitgebreide kennis van over de metrische gegevens zijn te verlichten.

## <a name="how-to-configure-alerts-rules-with-dynamic-thresholds"></a>Hoe kunt u regels voor waarschuwingen configureren met dynamische drempelwaarden?

Waarschuwingen met dynamische drempelwaarden kunnen worden geconfigureerd via Metric Alerts in Azure Monitor. [Meer informatie over het configureren van waarschuwingen voor metrische gegevens](alerts-metric.md).

## <a name="how-are-the-thresholds-calculated"></a>Hoe worden de drempelwaarden berekend?

Dynamische drempel continu de gegevens van de metrische gegevens uit de serie leert en probeert om modellen te maken met behulp van een set algoritmen en -methoden. en probeert om modellen te maken met behulp van een set algoritmen en methoden. Deze patronen in gegevens zoals seizoensgebondenheid (per uur / dagelijks / wekelijks) detecteert en ruis metrische gegevens (zoals machine CPU of geheugen) en metrische gegevens met lage spreiding (zoals beschikbaarheid en fout-tarief) overweg kan.

De drempelwaarden zijn geselecteerd in zodanig dat een afwijking van deze drempels geeft aan een anomalie in het metrische gedrag dat.

## <a name="what-does-sensitivity-setting-in-dynamic-thresholds-mean"></a>Wat betekent ' gevoeligheidsinstelling ' in dynamische drempelwaarden gemiddelde?

Drempelwaarde voor waarschuwing bij gevoeligheid is een op hoog niveau concept die bepaalt de hoeveelheid afwijking van metrische gedrag vereist om een waarschuwing te activeren.
Deze optie vereist geen kennis over de metrische gegevens, zoals statische drempelwaarde van het domein. De beschikbare opties zijn:

- Hoog – de drempelwaarden worden contour en dicht bij de metrische gegevens uit de serie patroon. Waarschuwingsregel geactiveerd bij kleinste afwijking, wat resulteert in meer waarschuwingen.
- Gemiddeld: minder krachtige en meer met gelijke taakverdeling drempelwaarden, minder waarschuwingen dan met hoge gevoeligheid (standaard).
- Laag – de drempelwaarden worden losse met meer afstand van metrische gegevens uit de serie patroon. Waarschuwingsregel wordt alleen geactiveerd op grote afwijking, wat resulteert in minder waarschuwingen.

## <a name="what-are-the-operator-setting-options-in-dynamic-thresholds"></a>Wat zijn de opties van de instelling 'Operator' in dynamische drempelwaarden?

Dynamische drempelwaarden voor waarschuwingsregel kunt maken die zijn afgestemd op basis van metrische gedrag voor beide boven en ondergrenzen met behulp van de dezelfde waarschuwingsregel drempelwaarden.
U kunt de waarschuwing wordt geactiveerd op een van de volgende drie voorwaarden:

- Groter dan de hoogste drempelwaarde of lager dan de lagere drempelwaarde (standaard)
- Groter dan de hoogste drempelwaarde
- Lager dan de lagere drempelwaarde.

## <a name="what-do-the-advanced-settings-in-dynamic-thresholds-mean"></a>Wat de geavanceerde instellingen in dynamische drempelwaarden gemiddelde?

**Perioden mislukt** -dynamische drempelwaarden ook kunt u configureren 'Aantal schendingen voor het activeren van de waarschuwing', een minimum aantal afwijkingen vereist binnen een bepaalde periode voor het systeem een waarschuwing (het standaardtijdvenster is vier te activeren afwijkingen in 20 minuten). De gebruiker kan configureren mislukt punten en kiezen wat u wilt worden gewaarschuwd op door de punten van mislukken en tijdvenster te wijzigen. Deze mogelijkheid vermindert waarschuwingsruis die worden gegenereerd door tijdelijke pieken. Bijvoorbeeld:

Als u wilt een waarschuwing geactiveerd wanneer het probleem continue voor 20 minuten is, 4 opeenvolgende keren dat in een bepaalde periode groepering van 5 minuten, gebruik de volgende instellingen:

![Instellingen voor continue probleem perioden mislukt voor 20 minuten, 4 opeenvolgende keren dat in een bepaalde periode groepering van 5 minuten](media/alerts-dynamic-thresholds/0008.png)

Als u wilt een waarschuwing geactiveerd wanneer er een schending van een dynamische drempelwaarden in 20 minuten buiten de laatste 30 minuten met een periode van 5 minuten is, gebruik de volgende instellingen:

![Perioden-instellingen voor uitgifte mislukt voor 20 minuten buiten de laatste 30 minuten met periode groepering van 5 minuten](media/alerts-dynamic-thresholds/0009.png)

**Gegevens negeren vóór** -gebruikers kunnen eventueel ook een begindatum van waaruit het systeem moet beginnen met de berekening van de drempelwaarden van definiëren. Een typische gebruiksscenario's kan optreden wanneer een resource is een die wordt uitgevoerd in een modus voor testen en nu om aan te bieden van een productie-werkbelasting wordt gepromoveerd, en daarom het gedrag van een willekeurige statistiek tijdens de testfase moet worden genegeerd.

## <a name="will-slow-behavior-change-in-the-metric-trigger-an-alert"></a>Trage gedrag veranderen in de metrische trigger een waarschuwing?

Waarschijnlijk niet. Dynamische drempelwaarden zijn geschikt voor grote afwijkingen detecteren in plaats van langzaam problemen in ontwikkeling.

## <a name="how-much-data-is-used-to-preview-and-then-calculate-thresholds"></a>Hoeveel gegevens wordt gebruikt om te bekijken en vervolgens drempelwaarden te berekenen?

De drempelwaarden die worden weergegeven in de grafiek, voordat een waarschuwingsregel wordt gemaakt op de metrische gegevens, worden berekend op basis van op de laatste 10 dagen van historische gegevens, zodra een waarschuwingsregel is gemaakt, de dynamische drempelwaarden wordt verkrijgen van aanvullende historische gegevens die beschikbaar is en wordt continu leren op basis van de nieuwe gegevens om de drempelwaarden meer nauwkeurige.
