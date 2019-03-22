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
ms.openlocfilehash: 30f853bd65c83b922faf008fbb5279c28f197f68
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339003"
---
# <a name="metric-alerts-with-dynamic-thresholds-in-azure-monitor-public-preview"></a>Metrische waarschuwingen met dynamische drempelwaarden in Azure Monitor (Preview-versie)

Waarschuwing voor metrische gegevens met dynamische drempelwaarden detectie maakt gebruik van geavanceerde machine learning (ML) meer historische gedrag van de metrische gegevens, patronen en afwijkingen die wijzen op problemen met de mogelijke service kunt identificeren. Deze biedt ondersteuning van zowel een eenvoudige gebruikersinterface en bewerkingen op schaal door toe te staan gebruikers waarschuwingsregels via de Azure Resource Manager-API op een volledig geautomatiseerde manier configureren.

Zodra een waarschuwingsregel is gemaakt, er volgt een waarschuwing alleen wanneer de bewaakte metrische gegevens niet wordt gedragen zich zoals verwacht, op basis van de op maat gemaakte drempelwaarden.

Wij willen graag uw feedback horen, bewaart u deze binnenkort op <azurealertsfeedback@microsoft.com>.

## <a name="why-and-when-is-using-dynamic-condition-type-recommended"></a>Waarom en wanneer is met behulp van dynamische voorwaardetype aanbevolen?

1. **Schaalbare waarschuwen** : dynamische drempelwaarden voor waarschuwingen, regels kunnen maken die zijn afgestemd drempelwaarden voor honderden metrische reeksen tegelijkertijd. Het leveren van nog net zo gemakkelijk van het definiëren van een waarschuwingsregel op één metrische waarde. Met behulp van de UI of de API van Azure Resource Manager-resultaten in minder waarschuwingsregels voor het beheren van. De schaalbare aanpak is vooral nuttig wanneer omgaan met metrische dimensies of toe te passen op meerdere resources, zoals alle resources van het abonnement. Dit wordt omgezet in een aanzienlijke tijd bespaart op beheer en het maken van regels voor waarschuwingen. [Meer informatie over het configureren van waarschuwingen voor metrische gegevens met dynamische drempelwaarden met behulp van sjablonen](alerts-metric-create-templates.md).

1. **Slimme metriek patroonherkenning** – met behulp van onze unieke ML-technologie, kunnen we automatisch detecteren van metrische patronen en gedurende een periode, die vaak seizoensgebondenheid (per uur / dagelijks / wekelijks) kan worden aangepast aan metrische wijzigingen. Aanpassing aan gedrag de metrische gegevens voor tijd en waarschuwingen op basis van de afwijkingen ten opzichte van het patroon ervan neemt de belasting van de wetenschap dat het 'recht' drempelwaarde voor elke metrische gegevens. Het ML-algoritme dat wordt gebruikt in dynamische drempelwaarden is ontworpen om te voorkomen dat ruis (geringe precisie) of hele (laag intrekken)-drempelwaarden die een verwacht patroon geen hebt.

1. **Configuratie van intuïtieve** : dynamische drempelwaarden toestaan instellen metrische waarschuwingen met behulp van geavanceerde concepten, moet uitgebreide kennis van over de metrische gegevens zijn te verlichten.

## <a name="how-to-configure-alerts-rules-with-dynamic-thresholds"></a>Hoe kunt u regels voor waarschuwingen configureren met dynamische drempelwaarden?

Waarschuwingen met dynamische drempelwaarden kunnen worden geconfigureerd via Metric Alerts in Azure Monitor. [Meer informatie over het configureren van waarschuwingen voor metrische gegevens](alerts-metric.md).

## <a name="how-are-the-thresholds-calculated"></a>Hoe worden de drempelwaarden berekend?

Dynamische drempelwaarden continu de gegevens van de metrische gegevens uit de serie leert en probeert om modellen te maken met behulp van een set algoritmen en methoden. Deze patronen in gegevens zoals seizoensgebondenheid (per uur / dagelijks / wekelijks) detecteert en ruis metrische gegevens (zoals machine CPU of geheugen) en metrische gegevens met lage spreiding (zoals beschikbaarheid en fout-tarief) overweg kan.

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

De drempelwaarden die worden weergegeven in de grafiek, voordat een waarschuwingsregel wordt gemaakt op de metrische gegevens, worden berekend op basis van genoeg historische gegevens voor het berekenen van uur of dagelijks seizoensgebonden patronen (10 dagen). Te drukken op 'Wekelijks weergave-patroon' krijgen voldoende historische gegevens voor het berekenen van de wekelijkse seizoensgebonden patronen (28 dagen). Zodra een waarschuwingsregel is gemaakt, wordt de dynamische drempelwaarden alle benodigde historische gegevens die beschikbaar is en continu leren en vertrouwd op basis van nieuwe gegevens gebruiken om te de drempelwaarden nauwkeuriger maken.

## <a name="how-much-data-is-needed-to-trigger-an-alert"></a>Hoeveel gegevens nodig is om een waarschuwing te activeren?

Hebt u een nieuwe resource of metrische gegevens ontbreken, wordt niet dynamische drempelwaarden voor waarschuwingen activeren voordat de drie dagen aan gegevens zijn beschikbaar om te controleren of nauwkeurige drempelwaarden.

## <a name="dynamic-thresholds-best-practices"></a>Aanbevolen procedures voor dynamische drempelwaarden

Dynamische drempelwaarden kunnen worden toegepast op elk platform of aangepaste metrische gegevens in Azure Monitor en het is ook afgestemd op voor de algemene metrische gegevens van toepassingen en infrastructuur.
De volgende items zijn aanbevolen procedures voor het configureren van waarschuwingen voor sommige van deze metrische gegevens met behulp van dynamische drempelwaarden.

### <a name="dynamic-thresholds-on-virtual-machine-cpu-percentage-metrics"></a>Dynamische drempelwaarden op virtuele machines CPU-percentage metrische gegevens

1. In [Azure-portal](https://portal.azure.com), klikt u op **Monitor**. De Monitor-weergave worden samengevoegd voor alle controle-instellingen en gegevens in één weergave.

2. Klik op **waarschuwingen** klikt u vervolgens op **+ nieuwe waarschuwingsregel**.

    > [!TIP]
    > De meeste resourceblades hebben ook **waarschuwingen** in hun resourcemenu onder **bewaking**, u kunt waarschuwingen maken ook.

3. Klik op **doel selecteren**, in het deelvenster context die wordt geladen, selecteert u een doelresource die u een waarschuwing wilt maken. Gebruik **abonnement** en **'Virtuele Machines' resourcetype** vervolgkeuzelijsten om te vinden van de resource die u wilt bewaken. U kunt ook de zoekbalk te vinden van uw resource.

4. Als u een doelbron hebt geselecteerd, klikt u op **voorwaarde toevoegen**.

5. Selecteer de **CPU-Percentage**.

6. (Optioneel) de metriek verfijnen door aan te passen **periode** en **aggregatie**. Het wordt afgeraden samenvoegingstype 'Maximaal' voor dit type metrische gegevens gebruiken, omdat dit minder vertegenwoordiger van het gedrag van. Voor type 'Maximale' aggregatie statische drempelwaarde mogelijk meer nodig.

7. U ziet een grafiek voor de metrische gegevens voor de afgelopen 6 uur. Definieer de parameters voor waarschuwing:
    1. **Type voorwaarde** -optie voor 'Dynamische'.
    1. **Gevoeligheid** -Kies/Laag gevoeligheid voor waarschuwingsruis te verminderen.
    1. **Operator** -groter is dan te kiezen, tenzij gedrag Hiermee geeft u het gebruik van toepassingen.
    1. **Frequentie** -overwegen verlagen op basis van impact op bedrijf van de waarschuwing.
    1. **Perioden mislukt** (geavanceerde optie) - het uiterlijk back-venster moet ten minste 15 minuten. Bijvoorbeeld, als de periode is ingesteld op vijf minuten, moet klikt u vervolgens mislukt perioden ten minste drie of meer.

8. De grafiek met metrische gegevens, wordt de berekende drempelwaarden op basis van recente gegevens weergegeven.

9. Klik op **Gereed**.

10. Vul in **Waarschuwingsdetails** zoals **waarschuwing regelnaam**, **beschrijving**, en **ernst**.

11. Een actiegroep toevoegen aan de waarschuwing door een bestaande actiegroep selecteren of door een nieuwe actiegroep maken.

12. Klik op **gedaan** om op te slaan van de waarschuwingsregel voor metrische gegevens.

> [!NOTE]
> Waarschuwing voor metrische gegevens regels die zijn gemaakt via de portal worden gemaakt in dezelfde resourcegroep bevinden als de doelresource.

### <a name="dynamic-thresholds-on-application-insights-http-request-execution-time"></a>Dynamische drempelwaarden in uitvoeringstijd van Application Insights HTTP-aanvraag

1. In [Azure-portal](https://portal.azure.com), klikt u op **Monitor**. De Monitor-weergave worden samengevoegd voor alle controle-instellingen en gegevens in één weergave.

2. Klik op **waarschuwingen** klikt u vervolgens op **+ nieuwe waarschuwingsregel**.

    > [!TIP]
    > De meeste resourceblades hebben ook **waarschuwingen** in hun resourcemenu onder **bewaking**, u kunt waarschuwingen maken ook.

3. Klik op **doel selecteren**, in het deelvenster context die wordt geladen, selecteert u een doelresource die u een waarschuwing wilt maken. Gebruik **abonnement** en **type 'Application Insights-Resource** vervolgkeuzelijsten om te vinden van de resource die u wilt bewaken. U kunt ook de zoekbalk te vinden van uw resource.

4. Als u een doelbron hebt geselecteerd, klikt u op **voorwaarde toevoegen**.

5. Selecteer de **HTTP-aanvraag uitvoeringstijd**.

6. (Optioneel) de metriek verfijnen door aan te passen **periode** en **aggregatie**. Het wordt afgeraden samenvoegingstype 'Maximaal' voor dit type metrische gegevens gebruiken, omdat dit minder vertegenwoordiger van het gedrag van. Voor type 'Maximale' aggregatie statische drempelwaarde mogelijk meer nodig.

7. U ziet een grafiek voor de metrische gegevens voor de afgelopen 6 uur. Definieer de parameters voor waarschuwing:
    1. **Type voorwaarde** -optie voor 'Dynamische'.
    1. **Operator** -Kies 'Groter dan' te verminderen voor verbetering van de duur van de geactiveerde waarschuwingen.
    1. **Frequentie** -overwegen verlagen op basis van impact op bedrijf van de waarschuwing.

8. De grafiek met metrische gegevens, wordt de berekende drempelwaarden op basis van recente gegevens weergegeven.

9. Klik op **Gereed**.

10. Vul in **Waarschuwingsdetails** zoals **waarschuwing regelnaam**, **beschrijving**, en **ernst**.

11. Een actiegroep toevoegen aan de waarschuwing door een bestaande actiegroep selecteren of door een nieuwe actiegroep maken.

12. Klik op **gedaan** om op te slaan van de waarschuwingsregel voor metrische gegevens.

> [!NOTE]
> Waarschuwing voor metrische gegevens regels die zijn gemaakt via de portal worden gemaakt in dezelfde resourcegroep bevinden als de doelresource.
