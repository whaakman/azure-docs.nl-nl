---
title: Problemen vaststellen en oplossen van de Preview Azure Time Series Insights | Microsoft Docs
description: Lees hoe u problemen vaststellen en oplossen met de Azure Time Series Insights-Preview.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: eb1c302bf1954492ba2a7a78d16fc697fdf4b687
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58080434"
---
# <a name="diagnose-and-troubleshoot"></a>Problemen vaststellen en oplossen

In dit artikel bevat een overzicht van enkele veelvoorkomende problemen die optreden mogelijk wanneer u met uw Azure Time Series Insights Preview-omgeving werkt. Het artikel wordt ook beschreven mogelijke oorzaken en oplossingen voor elk probleem.

## <a name="problem-i-cant-find-my-environment-in-the-time-series-insights-preview-explorer"></a>Probleem: Ik kan mijn omgeving niet vinden in de Verkenner van Time Series Insights Preview

Dit probleem kan optreden als u geen machtigingen hebt voor toegang tot de Time Series Insights-omgeving. Gebruikers moeten een rol toegang reader-niveau om hun Time Series Insights-omgeving weer te geven. Als u wilt controleren of de huidige toegangsniveaus en aanvullende toegang verlenen, gaat u naar de sectie beleid voor gegevenstoegang op de Time Series Insights-resource in de [Azure-portal](https://portal.azure.com/).

  ![Omgeving][1]

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Probleem: Er zijn geen gegevens zien is in de Verkenner van Time Series Insights Preview

Er zijn enkele veelvoorkomende redenen waarom u ziet mogelijk niet de gegevens in de [Azure Time Series Insights Preview explorer](https://insights.timeseries.azure.com/preview).

- Uw gebeurtenisbron, mogelijk niet ontvangen van gegevens.

    Controleer of dat uw gebeurtenisbron, die een event hub of een IoT-hub, gegevens van uw labels of instanties ontvangt. Om te controleren, gaat u naar de overzichtspagina van uw resource in Azure portal.

    ![Dashboard-insights][2]

- Uw gebeurtenisgegevens van de bron is niet in JSON-indeling.

    Time Series Insights ondersteunt alleen JSON-gegevens. Zie voor voorbeelden van JSON, [ondersteund JSON-vormen](./how-to-shape-query-json.md).

- De sleutel van de bron gebeurtenis ontbreekt een vereiste machtiging.

  * Voor een IoT-hub, moet u de sleutel die is opgeven **service verbinding maken met** machtiging.

    ![Configuratie][3]

  * Zoals wordt weergegeven in de voorgaande afbeelding, beide van de beleidsregels **iothubowner** en **service** werken omdat ze hebben **service verbinding maken met** machtiging.
  * Voor een event hub, moet u de sleutel die is opgeven **luisteren** machtiging.
  
    ![Machtigingen][4]

  * Zoals wordt weergegeven in de voorgaande afbeelding, beide van de **lezen** en **beheren** -beleid werkt omdat ze hebben **luisteren** machtiging.

- De consumentengroep opgegeven is niet exclusief voor Time Series Insights.

    Tijdens de registratie van een IoT-hub of event hub, moet u de consumentengroep die wordt gebruikt om de gegevens te lezen opgeven. Deel geen de consumergroep. Als de consumergroep wordt gedeeld, verbroken de onderliggende event hub automatisch een van de lezers in willekeurige volgorde. Geef een unieke consumergroep voor Time Series Insights om uit te lezen.

- Uw Time Series-ID-eigenschap die is opgegeven op het moment van de inrichting is onjuist, ontbreekt of null zijn.

    Dit probleem kan optreden als de eigenschap Time Series-ID is onjuist geconfigureerd op het moment van de omgeving inrichten. Zie voor meer informatie, [aanbevolen procedures voor het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md). Op dit moment kunt bijwerken u een bestaande Time Series Insights-omgeving voor het gebruik van een andere tijd reeks id niet

## <a name="problem-some-data-shows-but-some-is-missing"></a>Probleem: Sommige gegevens bevat, maar sommige ontbreekt

Verzendt u mogelijk gegevens zonder dat de Time Series-ID.

- Dit probleem kan optreden wanneer u gebeurtenissen zonder de Time Series-ID-veld in de nettolading verzenden. Zie voor meer informatie, [ondersteund JSON-vormen](./how-to-shape-query-json.md).

- Dit probleem kan optreden omdat uw omgeving wordt beperkt.

    > [!NOTE]
    > Op dit moment ondersteunt Time Series Insights een maximale opname-tarief van 6 Mbps.

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Probleem: Mijn gebeurtenisbron Timestamp eigenschap de naam van instelling werkt niet

Zorg ervoor dat de naam en waarde aan de volgende regels voldoet:

* Naam van de Tijdstempeleigenschap is hoofdlettergevoelig.
* De waarde van de Timestamp eigenschap die afkomstig zijn uit uw gebeurtenisbron, als een JSON-tekenreeks, heeft de indeling `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Een voorbeeld van een tekenreeks is die is `“2008-04-12T12:53Z”`.

De eenvoudigste manier om ervoor te zorgen dat de naam van de Tijdstempeleigenschap is vastgelegd en correct werkt, is met de Verkenner van Time Series Insights Preview. Gebruik de grafiek om te selecteren van een bepaalde tijd nadat u de naam van de Tijdstempeleigenschap opgegeven binnen de Verkenner van Time Series Insights Preview. Met de rechtermuisknop op de selectie en selecteer de **gebeurtenissen onderzoeken** optie. De koptekst van de eerste kolom is de naam van de Tijdstempeleigenschap. Deze moet hebben `($ts)` naast het woord `Timestamp`, in plaats van:

* `(abc)`, waarmee wordt aangegeven dat Time Series Insights de gegevenswaarden als tekenreeksen leest.
* Pictogram Agenda, waarmee wordt aangegeven dat Time Series Insights de datum/tijd-gegevenswaarde leest.
* `#`, waarmee wordt aangegeven dat Time Series Insights de gegevenswaarden worden gelezen als een geheel getal zijn.

Als de eigenschap Timestamp niet expliciet is opgegeven, worden de IoT-hub of event hub Wachtrijduur van een gebeurtenis wordt gebruikt als de standaard-tijdstempel.

## <a name="problem-i-cant-edit-or-view-my-time-series-model"></a>Probleem: Ik kan bewerken of weergeven van mijn Time Series-Model

- U kunt toegang tot een Time Series Insights S1 of S2-omgeving.

   Time Series-modellen worden alleen ondersteund in omgevingen met betalen per gebruik. Zie voor meer informatie over hoe u toegang tot uw S1/S2-omgeving vanuit de Verkenner van Time Series Insights Preview [visualiseren van gegevens in de Verkenner](./time-series-insights-update-explorer.md).

   ![Access][5]

- U mogelijk niet gemachtigd om te bekijken en bewerken van het model.

   Gebruikers moeten toegang op Inzender-niveau bewerken en weergeven van de Time Series-Model. Controleer of de huidige toegangsniveaus en aanvullende toegang verlenen, gaat u naar de sectie beleid voor gegevenstoegang voor uw Time Series Insights-resource in Azure portal.

## <a name="problem-all-my-instances-in-the-time-series-insights-preview-explorer-dont-have-a-parent"></a>Probleem: Alle exemplaren in de Verkenner van Time Series Insights Preview hebben geen bovenliggend item

Dit probleem kan optreden als uw omgeving beschikt niet over een hiërarchie Time Series-Model is gedefinieerd. Zie voor meer informatie, [werken met Time Series-modellen](./time-series-insights-update-how-to-tsm.md).

  ![Time Series-modellen][6]

## <a name="next-steps"></a>Volgende stappen

- Lezen [werken met Time Series-modellen](./time-series-insights-update-how-to-tsm.md).
- Lezen [ondersteund JSON-vormen](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/environment.png
[2]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[3]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[4]: media/v2-update-diagnose-and-troubleshoot/permissions.png
[5]: media/v2-update-diagnose-and-troubleshoot/access.png
[6]: media/v2-update-diagnose-and-troubleshoot/tsm.png