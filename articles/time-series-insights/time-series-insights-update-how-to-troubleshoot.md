---
title: Hoe u problemen vaststellen en oplossen van de Azure Time Series Insights (preview) | Microsoft Docs
description: Informatie over hoe u problemen vaststellen en oplossen met de Azure Time Series Insights (preview)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: d13d373169287a0ec5931d5437b0a3bc70ecd79a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856041"
---
# <a name="how-to-diagnose-and-troubleshoot"></a>Hoe u problemen vaststellen en oplossen

In dit artikel bevat een overzicht van enkele veelvoorkomende problemen die optreden werken met uw Azure Time Series Insights (TSI)-omgeving. Het artikel beschrijft ook mogelijke oorzaken en oplossingen voor elk.

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Probleem: Er worden geen gegevens zien is in de Time Series Insights (preview) Explorer

Er zijn enkele veelvoorkomende redenen waarom u uw gegevens in de Azure TSI-Verkenner niet kan zien:

1. De bron van gebeurtenis kan niet worden ontvangen van gegevens.

    Controleer of de bron van gebeurtenis (Event Hub of IoT Hub) is ontvangt gegevens van uw tags / -exemplaren. U kunt dit doen door te navigeren naar de overzichtspagina van uw resource in Azure portal.

    ![dashboard-inzichten][1]

1. Uw gebeurtenisgegevens van de bron is niet in JSON-indeling

    Azure TSI ondersteunt alleen JSON-gegevens. Zie voor voorbeelden van JSON, [ondersteund JSON-vormen](./how-to-shape-query-json.md).

1. De sleutel van de bron gebeurtenis ontbreekt een vereiste machtiging

    ![configuratie][2]

    * Voor een IoT-Hub moet u de sleutel met de service ontbreekt de connect machtiging opgeven.
    * Zoals wordt weergegeven in de voorgaande afbeelding is een van de beleidsregels *iothubowner* en service zou moeten werken, omdat beide service ontbreekt de connect machtiging.
    * Voor een event hub moet u de sleutel met de machtiging luisteren opgeven.
    * Zoals weergegeven in de voorgaande afbeelding, een van de beleidsregels voor lezen en beheren zou moeten werken, omdat beide Listen-machtiging hebben.

    ![machtigingen][3]

1. De consumentengroep opgegeven is niet exclusief voor TSI

    Tijdens de registratie van uur IoT-Hub of een event hub, moet u de consumentengroep die moet worden gebruikt voor het lezen van de gegevens opgeven. Deze consumergroep moet niet worden gedeeld. Als de consumergroep wordt gedeeld, verbroken de onderliggende event hub automatisch een van de lezers willekeurig. Geef een unieke consumergroep voor TSI om uit te lezen.

## <a name="problem-some-data-is-shown-but-some-is-missing"></a>Probleem: Sommige gegevens worden weergegeven, maar sommige ontbreekt

Dit kan gebeuren omdat uw omgeving wordt beperkt.

> [!NOTE]
> Op dit moment ondersteunt Azure TSI een maximale opname-tarief van 6 MBps.

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Probleem: Instelling van mijn gebeurtenisbron timestamp eigenschap name niet werkt

Zorg ervoor dat de naam en waarde aan de volgende regels voldoet:

* De **Timestamp** naam is hoofdlettergevoelig.
* De **Timestamp** eigenschapswaarde die afkomstig van de bron van gebeurtenis, als een JSON-tekenreeks, moet de indeling hebben `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Een voorbeeld van een tekenreeks is die is `“2008-04-12T12:53Z”`.

De eenvoudigste manier om ervoor te zorgen dat de naam van de tijdstempeleigenschap is vastgelegd en correct werkt, is met de TSI-Verkenner. Gebruik de grafiek om te selecteren van een bepaalde tijd nadat u de naam van de tijdstempeleigenschap opgegeven binnen de TSI-Verkenner. Met de rechtermuisknop op de selectie en kies de **gebeurtenissen onderzoeken** optie. De eerste kolom header moet uw **Timestamp** eigenschapsnaam en deze moet een `($ts)` naast het woord `Timestamp`, in plaats van:

* `(abc)`, die aangeeft dat het TSI lezen van de gegevenswaarden als tekenreeksen
* Pictogram van de agenda, die wijzen op dat TSI leest de datum/tijd-gegevenswaarde
* `#`, de gegevenswaarden die aangeeft dat het TSI wordt gelezen als een geheel getal zijn

## <a name="problem-my-time-series-id-property-is-incorrect-missing-or-null"></a>Probleem: Mijn Time Series-ID-eigenschap is onjuist, ontbreekt of null

Dit kan gebeuren als de **Time Series-ID** eigenschap is niet juist geconfigureerd op het moment van de omgeving inrichten. Zie de [aanbevolen procedures voor het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md) artikel bij het kiezen van een **Time Series-ID**. Op dit moment kunt u een bestaande omgeving TSI (preview) voor het gebruik van een andere niet bijwerken **Time Series-ID**.

## <a name="problem-all-my-instances-in-time-series-insights-preview-explorer-dont-have-a-parent"></a>Probleem: Mijn exemplaren in Time Series Insights (preview) Explorer niet beschikken over een bovenliggend item

Dit kan gebeuren als uw omgeving beschikt niet over een **Tijdreeksmodel** hiërarchie gedefinieerd. Raadpleeg dit artikel voor meer informatie over [over het werken met Time Series-modellen](./time-series-insights-update-how-to-tsm.md).

## <a name="next-steps"></a>Volgende stappen

* Lezen [over het werken met Time Series-modellen](./time-series-insights-update-how-to-tsm.md).

* Lezen [ondersteund JSON-vormen](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[2]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[3]: media/v2-update-diagnose-and-troubleshoot/permissions.png