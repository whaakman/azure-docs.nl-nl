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
ms.openlocfilehash: df75ff9a359620781743732f4f12a6d3e7ec51c6
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331671"
---
# <a name="alerts-with-dynamic-thresholds-in-azure-monitor-limited-private-preview"></a>Waarschuwingen met dynamische drempelwaarden in Azure Monitor (beperkte Private Preview)

Waarschuwingen met dynamische drempelwaarden zijn een uitbreiding op Azure metrische waarschuwingen in Azure Monitor, die gebruikmaken van geavanceerde mogelijkheden voor Machine Learning (ML) voor meer informatie over metrische gegevens historische gedrag automatisch berekent basislijnen en ze als drempelwaarden gebruiken.

De voordelen van het gebruik van dynamische drempelwaarden zijn:

- Sla de problemen die zijn gekoppeld aan de grens van een vooraf gedefinieerde Star instellen als de monitor automatisch de historische prestaties van de metrische gegevens leert en van toepassing is ML-algoritmes om drempelwaarden vast te stellen.
- Ze kunnen seizoensgebonden gedrag en waarschuwingen alleen bij afwijkingen van het verwachte gedrag seizoensgebonden identificeren. Metrische waarschuwingen met dynamische drempelwaarden wordt niet geactiveerd als de service regelmatig niet actief in het weekend is en vervolgens elke maandag pieken. Op dit moment ondersteund: per uur, dagelijks en wekelijks seizoensgebondenheid.
- Voortdurend leert van de prestaties van de metrische gegevens en is adaptieve metrische wijzigingen.

Dynamische drempelwaarde gebaseerde waarschuwingen zijn beschikbaar voor alle Azure monitor op basis van metrische bronnen die worden vermeld in dit [artikel](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts#what-resources-can-i-create-near-real-time-metric-alerts-for).

## <a name="sign-up-to-access-the-preview"></a>Zich registreren voor toegang tot de Preview-versie

Om deze mogelijkheid een proefrit te [zich registreren voor de Preview-versie](https://aka.ms/DynamicThresholdMetricAlerts). Zoals altijd, we horen graag uw feedback, bewaar deze op binnenkort [azurealertsfeedback@microsoft.com](mailto:azurealertsfeedback@microsoft.com)

## <a name="how-to-configure-alerts-with-dynamic-thresholds"></a>Waarschuwingen configureren met dynamische drempelwaarden

Waarschuwingen met dynamische drempelwaarden kunnen worden geconfigureerd via de waarschuwingen in Azure Monitor

![Waarschuwingen-preview](media/alerts-dynamic-thresholds/0001.png)

## <a name="creating-an-alert-rule-with-dynamic-thresholds"></a>Het maken van een waarschuwingsregel met dynamische drempelwaarden

1. Selecteer in het deelvenster waarschuwingen van onder Monitor, de **nieuwe waarschuwingsregel** knop een nieuwe waarschuwing maken in Azure.

   ![Nieuwe waarschuwingsregel](media/alerts-dynamic-thresholds/002.png)

2. De sectie van de regel voor maken wordt weergegeven met de drie onderdelen die bestaan uit: _Waarschuwingsvoorwaarde definiëren_, _Waarschuwingsdetails definiëren_, en _actiegroep definiëren_. Begin met het _waarschuwingsvoorwaarde definiëren_ sectie de **doel selecteren** koppeling naar het doel, met een resource opgeven. Nadat een juiste resource is gekozen, klikt u op de knop Gereed.

   ![Doel selecteren](media/alerts-dynamic-thresholds/0003.png)

3. Vervolgens gebruikt de **criteria toevoegen** knop om een lijst met signaal opties beschikbaar voor de resource en in de lijst signaal weer te geven, kies een geschikte **metric** optie. (Bijvoorbeeld Percentage CPU.)

   ![Criteria toevoegen](media/alerts-dynamic-thresholds/004.png)

4. Op het scherm configureren signaal logica in de sectie Alert logic hebt u de optie om over te schakelen van de voorwaarde voor een type dynamisch is, die de dynamische drempels (rode lijnen) naast de metrische gegevens (blauwe lijn) automatisch wordt gegenereerd.

   ![Dynamisch](media/alerts-dynamic-thresholds/005.png)

5. De drempelwaarden die worden weergegeven in de grafiek worden berekend op basis van in de afgelopen zeven dagen van historische gegevens, wanneer een waarschuwing is gemaakt, de dynamische drempelwaarden krijgen extra historische gegevens die beschikbaar is en leert op basis van nieuwe gegevens om continu de drempelwaarden voor nauwkeurigere.

6. Aanvullende Alert logic-instellingen:
   - Voorwaarde - kunt u de waarschuwing wordt geactiveerd op een van de volgende drie voorwaarden:
       - Groter dan de hoogste drempelwaarde of lager dan de lagere drempelwaarde (standaard)
       - Groter dan de hoogste drempelwaarde
       - Lager dan de lagere drempelwaarde.
   - Tijdverzameling: Gemiddelde (standaard), sum, min, max.
   - Gevoeligheid van waarschuwingen:
       - Hoog: meer waarschuwingen, zoals de waarschuwing wordt geactiveerd op de kleinste afwijking.
       - Med-minder gevoelig dan hoog, minder waarschuwingen dan met hoge gevoeligheid (standaard)
       - Laag – de minst gevoelige drempelwaarde.

    ![Instellingen voor waarschuwingslogica](media/alerts-dynamic-thresholds/00007.png)

7. Geëvalueerd op basis van:
    -  Welke duur, de waarschuwing moet worden gezocht naar de opgegeven voorwaarde door te kiezen uit de **periode**.

    ![Geëvalueerd op basis van](media/alerts-dynamic-thresholds/007.png)

   > [!NOTE]
   > Ondersteunde waarden: 5 minuten, 10 minuten, 30 minuten en 1 uur.

   Om te beperken die worden gegenereerd door tijdelijke pieken waarschuwingsruis, wordt u aangeraden de instellingen 'Number schending van het activeren van de waarschuwing'. Deze functie kunt u ontvangt een waarschuwing alleen als de drempelwaarde is geschonden X keer achter elkaar of Y tijden buiten het laatste Z perioden. Bijvoorbeeld:

    Als u wilt een waarschuwing geactiveerd wanneer het probleem continue gedurende 15 minuten is, 3 opeenvolgende keren dat in een bepaalde periode van 5 minuten, gebruik de volgende instellingen:

   ![Geëvalueerd op basis van](media/alerts-dynamic-thresholds/0008.png)

    Als u wilt een waarschuwing geactiveerd wanneer er een schending van een dynamische drempel in 15 minuten buiten de laatste 30 minuten met een periode van 5 minuten is, gebruik de volgende instellingen:

   ![Geëvalueerd op basis van](media/alerts-dynamic-thresholds/0009.png)

8. Gebruikers kunnen op dit moment waarschuwingen met Dynamische drempel criteria hebben als een enkele criteria.

   ![Regel maken](media/alerts-dynamic-thresholds/010.png)

## <a name="q--a"></a>Vragen en antwoorden

- VRAAG: Als de metriek wordt langzaam na verloop van tijd gewijzigd, wordt dit een waarschuwing met dynamische drempelwaarden geactiveerd?

- A: Waarschijnlijk niet. Dynamische drempelwaarden zijn geschikt voor grote afwijkingen detecteren in plaats van langzaam problemen in ontwikkeling.

- VRAAG: Kan ik dynamische drempelwaarden via een API configureren?

- A: We zijn ermee bezig.
