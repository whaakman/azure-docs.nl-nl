---
title: Hoe u problemen vaststellen en oplossen van de Azure Time Series Insights (Preview) | Microsoft Docs
description: Informatie over hoe u problemen vaststellen en oplossen met de Azure Time Series Insights (Preview)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: a9078d2f8a738700a30d265d9cfa3cd77ad72f08
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015446"
---
# <a name="how-to-diagnose-and-troubleshoot"></a>Hoe u problemen vaststellen en oplossen

In dit artikel bevat een overzicht van enkele veelvoorkomende problemen die optreden werken met uw Azure Time Series Insights (TSI)-omgeving. Het artikel beschrijft ook mogelijke oorzaken en oplossingen voor elk.

## <a name="problem-i-cant-find-my-environment-in-the-time-series-insights-preview-explorer"></a>Probleem: kan ik mijn omgeving vinden in de Verkenner van Time Series Insights (Preview)

Dit kan gebeuren als u geen machtigingen voor toegang tot de TSI-omgeving. Gebruikers moeten 'Lezer' toegangsniveau van de rol om hun TSI-omgeving weer te geven. U kunt controleren of de huidige toegangsniveaus en aanvullende toegang verlenen door naar de sectie beleid voor gegevenstoegang in de TSI-resource in te gaan [Azure Portal](https://portal.azure.com/).

  ![environment][1]

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Probleem: Er worden geen gegevens zien is in de Verkenner van Time Series Insights (Preview)

Er zijn enkele veelvoorkomende redenen waarom u ziet mogelijk niet de gegevens in de [(Preview) van Azure TSI-Verkenner](https://insights.timeseries.azure.com/preview):

1. De bron van gebeurtenis kan niet worden ontvangen van gegevens.

    Controleer of de bron van gebeurtenis (Event Hub of IoT Hub) is ontvangt gegevens van uw tags / -exemplaren. U kunt dit doen door te navigeren naar de overzichtspagina van uw resource in Azure portal.

    ![dashboard-inzichten][2]

1. Uw gebeurtenisgegevens van de bron is niet in JSON-indeling

    Azure TSI ondersteunt alleen JSON-gegevens. Zie voor voorbeelden van JSON, [ondersteund JSON-vormen](./how-to-shape-query-json.md).

1. De sleutel van de bron gebeurtenis ontbreekt een vereiste machtiging

    * Voor een IoT-Hub moet u de sleutel met de service ontbreekt de connect machtiging opgeven.

    ![configuratie][3]

    * Zoals wordt weergegeven in de voorgaande afbeelding is een van de beleidsregels *iothubowner* en service zou moeten werken, omdat beide service ontbreekt de connect machtiging.
    * Voor een event hub moet u de sleutel met de machtiging luisteren opgeven.
  
    ![machtigingen][4]

    * Zoals wordt weergegeven in de voorgaande afbeelding is een van de beleidsregels **lezen** en **beheren** zou moeten werken, omdat beide **luisteren** machtiging.

1. De consumentengroep opgegeven is niet exclusief voor TSI

    Tijdens de registratie van een IoT-Hub of Event Hub, moet u de consumentengroep die moet worden gebruikt voor het lezen van de gegevens opgeven. De consumergroep moet niet worden gedeeld. Als de consumergroep wordt gedeeld, verbroken de onderliggende Event Hub automatisch een van de lezers willekeurig. Geef een unieke consumergroep voor TSI om uit te lezen.

1. Uw Time Series-ID-eigenschap die is opgegeven op het moment van de inrichting is onjuist, ontbreekt of null zijn

    Dit kan gebeuren als de **Time Series-ID** eigenschap is niet juist geconfigureerd op het moment van de omgeving inrichten. Raadpleeg de [aanbevolen procedures voor het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md). Op dit moment kunt u een bestaande Time Series Insights-update-omgeving voor het gebruik van een andere niet bijwerken **Time Series-ID**.

## <a name="problem-some-data-is-shown-but-some-is-missing"></a>Probleem: Sommige gegevens worden weergegeven, maar sommige ontbreekt

1. U mag verzenden van gegevens zonder dat de Time Series-ID

    Deze fout kan optreden bij het verzenden van gebeurtenissen zonder de Time Series-ID-veld in de nettolading. Zie [ondersteund JSON-vormen](./how-to-shape-query-json.md) voor meer informatie.

1. Dit kan gebeuren omdat uw omgeving wordt beperkt.

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

Als de **Timestamp** eigenschap niet expliciet is opgegeven, wordt gebruik van de IoT-Hub of Event Hub een gebeurtenis **Wachtrijduur** als de standaard-tijdstempel.

## <a name="problem-i-cant-edit-or-view-my-time-series-model"></a>Probleem: die ik kan bewerken of weergeven van mijn Time Series-Model

1. U maakt mogelijk gebruik van een Time Series Insights S1 of S2-omgeving

   Time Series-modellen worden alleen ondersteund in **PAYG** omgevingen. Raadpleeg dit artikel voor meer informatie over uw omgeving S1/S2 openen vanuit de Verkenner van Time Series Insights Update.

   ![toegang][5]

1. U bent niet gemachtigd om te bekijken en bewerken van het model

   Gebruikers moeten een "bijdrager" toegangsniveau te bewerken en weergeven van de Time Series-Model. U kunt controleren of de huidige toegangsniveaus en aanvullende toegang verlenen door naar de sectie beleid voor gegevenstoegang voor uw Time Series Insights-resource in Azure Portal te gaan.

## <a name="problem-all-my-instances-in-time-series-insights-preview-explorer-dont-have-a-parent"></a>Probleem: Alle exemplaren in de Verkenner van Time Series Insights (Preview) geen bovenliggend item hebben

Dit kan gebeuren als uw omgeving beschikt niet over een **Tijdreeksmodel** hiërarchie gedefinieerd. Raadpleeg dit artikel voor meer informatie over [over het werken met Time Series-modellen](./time-series-insights-update-how-to-tsm.md).

  ![TSM][6]

## <a name="next-steps"></a>Volgende stappen

Lezen [over het werken met Time Series-modellen](./time-series-insights-update-how-to-tsm.md).

Lezen [ondersteund JSON-vormen](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/environment.png
[2]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[3]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[4]: media/v2-update-diagnose-and-troubleshoot/permissions.png
[5]: media/v2-update-diagnose-and-troubleshoot/access.png
[6]: media/v2-update-diagnose-and-troubleshoot/tsm.png