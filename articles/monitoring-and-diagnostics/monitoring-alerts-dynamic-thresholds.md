---
title: Maken van waarschuwingen met dynamische drempelwaarden in Azure Monitor | Microsoft Docs
description: Waarschuwingen met machine learning gebaseerde dynamische drempelwaarden maken
author: antonfrMSFT
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: antonfr;mbullwin
ms.openlocfilehash: c847052134b1d83cd606e0e2b51b63b580f7917c
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="alerts-with-dynamic-thresholds-in-azure-monitor-limited-public-preview"></a>Waarschuwingen met dynamische drempelwaarden in de Azure-Monitor (beperkte openbare Preview)

Waarschuwingen met dynamische drempelwaarden zijn een uitbreiding van Azure metriek waarschuwingen in de Azure-Monitor die gebruikmaken van geavanceerde mogelijkheden van Machine Learning (ML) voor meer informatie over de metrische gegevens historische gedrag automatisch berekent basislijnen en ze gebruiken als de drempelwaarden voor waarschuwingen.

De voordelen van het gebruik van dynamische drempelwaarden zijn:

- Sla de moeite die zijn gekoppeld aan de grens van een vooraf gedefinieerde rigid instellen als de monitor automatisch de historische prestaties van de metrische gegevens leert en geldt ML algoritmen om te bepalen van de drempelwaarden voor waarschuwingen.
- Ze kunnen seizoensgebonden gedrag en waarschuwing alleen op afwijkingen van het verwachte gedrag voor het seizoen identificeren. Als uw service regelmatig niet actief in het weekend is en vervolgens elke maandag bereikt activeren metrische waarschuwingen met dynamische drempelwaarden niet. Op dit moment ondersteund: elk uur, dagelijkse en wekelijkse seizoensgebonden.
- Continu leert de metrische prestaties en kan worden aangepast voor metrische wijzigingen.

Dynamische waarschuwingen op basis van drempelwaarden zijn beschikbaar voor alle Azure monitor op basis van metrische bronnen die worden vermeld in deze [artikel](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts#what-resources-can-i-create-near-real-time-metric-alerts-for).

## <a name="sign-up-to-access-the-preview"></a>Aanmelden voor toegang tot de Preview-versie

Te ondernemen deze mogelijkheid voor een spin [aanmelden voor de preview](http://aka.ms/DynamicThresholdMetricAlerts). Altijd, we horen graag uw feedback, houd deze binnenkort op [azurealertsfeedback@microsoft.com](mailto:azurealertsfeedback@microsoft.com)

## <a name="how-to-configure-alerts-with-dynamic-thresholds"></a>Waarschuwingen configureren met dynamische drempelwaarden

Waarschuwingen met dynamische drempelwaarden kunnen worden geconfigureerd via de waarschuwingen in de Azure-Monitor

![Waarschuwingen-preview](./media/monitoring-alerts-dynamic-thresholds/0001.png)

## <a name="creating-an-alert-rule-with-dynamic-thresholds"></a>Een waarschuwingsregel maken met dynamische drempelwaarden

1. Selecteer in het deelvenster waarschuwingen onder Monitor de **nieuwe waarschuwingsregel** knop een nieuwe waarschuwing maken in Azure.

   ![Nieuwe waarschuwingsregel](./media/monitoring-alerts-dynamic-thresholds/002.png)

2. De sectie van de regel voor maken wordt weergegeven met de drie onderdelen die bestaan uit: _definiëren meldingsvoorwaarde_, _definiëren Waarschuwingsdetails_, en _definiëren actiegroep_. Eerst beginnen met de _definiëren meldingsvoorwaarde_ sectie gebruik de **Select Target** koppeling naar het doel met een bron opgeven. Nadat een juiste resource hebt gekozen, klikt u op de knop Gereed.

   ![Doel selecteren](./media/monitoring-alerts-dynamic-thresholds/0003.png)

3. Vervolgens gebruikt de **criteria toevoegen** om een lijst weergeven met signaal opties die beschikbaar zijn voor de bron- en uit de lijst signaal kiest u een geschikte **metriek** optie. (Bijvoorbeeld Percentage CPU.)

   ![Criteria toevoegen](./media/monitoring-alerts-dynamic-thresholds/004.png)

4. Op het scherm configureren signaal logica in de sectie waarschuwingen logica hebt u de optie de voorwaarde overschakelen naar een type dynamisch is, die de dynamische drempelwaarden (rode lijnen) naast de metriek (blauw regel) automatisch gegenereerd.

   ![Dynamisch](./media/monitoring-alerts-dynamic-thresholds/005.png)

5. De drempelwaarden die voorkomen in de grafiek worden berekend op basis van op de afgelopen zeven dagen van historische gegevens zodra een waarschuwing is gemaakt, de dynamische drempelwaarden verkrijgt aanvullende historische gegevens die beschikbaar is en leert continu op basis van nieuwe gegevens om de drempelwaarden nauwkeuriger.

6. Aanvullende waarschuwing logica instellingen:
   - Voorwaarde: U kunt de waarschuwing worden geactiveerd op een van de volgende drie voorwaarden kiezen:
       - Groter dan de drempelwaarde van de bovenste of lager is dan de laagste drempel (standaard)
       - Groter dan de hoogste drempelwaarde
       - Lager dan de lagere drempelwaarde.
   - Tijd aggregatie: gemiddelde (standaard), sum, min, max.
   - Waarschuwing gevoeligheid:
       - Hoog – meer waarschuwingen, zoals de waarschuwing wordt geactiveerd bij het kleinste afwijking.
       - Med – minder gevoelig dan hoog, minder waarschuwingen dan met hoge gevoeligheid (standaard)
       - Laag – de minst gevoelig drempelwaarde.

    ![Instellingen van de logica voor waarschuwingen](./media/monitoring-alerts-dynamic-thresholds/00007.png)

7. Geëvalueerd op basis van:
    -  Wat tijd, de waarschuwing moet worden gezocht naar de opgegeven voorwaarde door te kiezen uit de **periode**.

    ![Geëvalueerd op basis van](./media/monitoring-alerts-dynamic-thresholds/007.png)

   > [!NOTE]
   > Ondersteunde periode waarden: 5 minuten, 10 minuten, 30 minuten en 1 uur.

   Als u waarschuwing veroorzaakt door tijdelijke pieken, wordt u aangeraden de instellingen 'Number schendingen van de waarschuwing activeren'. Deze functionaliteit kunt u ontvangt een waarschuwing alleen als de drempelwaarde is geschonden X keer achter elkaar of Y tijden buiten het laatste Z perioden. Bijvoorbeeld:

    Waarschuwing wordt gegeven als het probleem doorlopend gedurende 15 minuten, 3 opeenvolgende keren in een bepaalde periode van 5 minuten, gebruik de volgende instellingen:

   ![Geëvalueerd op basis van](./media/monitoring-alerts-dynamic-thresholds/0008.png)

    Als u wilt een waarschuwing wordt geactiveerd wanneer er een schending van een dynamische drempelwaarde in 15 minuten buiten de laatste 30 minuten met periode van 5 minuten is, gebruik de volgende instellingen:

   ![Geëvalueerd op basis van](./media/monitoring-alerts-dynamic-thresholds/0009.png)

8. Gebruikers kunnen momenteel waarschuwingen met de dynamische drempelwaarde criteria als één criterium hebben.

   ![Regel maken](./media/monitoring-alerts-dynamic-thresholds/010.png)

## <a name="q--a"></a>Vragen en antwoorden

- V: als de metriek wordt langzaam na verloop van tijd gewijzigd, wordt dit een waarschuwing activeren met dynamische drempelwaarden?

- A: waarschijnlijk niet. Dynamische drempelwaarden zijn geschikt is voor het detecteren van belangrijke afwijkingen in plaats van langzaam problemen in ontwikkeling.

- V: kan ik drempels dynamische via een API?

- A: we proberen erop.
