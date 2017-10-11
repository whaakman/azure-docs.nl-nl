---
title: Diagnostische gegevens van web-app prestaties wijzigingen in Azure Application Insights van smartcard | Microsoft Docs
description: Automatische diagnoses pieken of stappen in de prestatietelemetrie van uw web-app.
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: cfreeman
ms.openlocfilehash: 5e53bc714d89bf6204681349e7890e0b8fbc7046
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a>Onverwachte wijzigingen in uw app telemetrie onderzoeken

*Deze functie is in preview.*

Diagnose plotselinge veranderingen in uw web-app prestatie- of gebruik met een enkele klik! De functie Smart Diagnostics is beschikbaar wanneer u een grafiek tijd in maakt [Analytics](app-insights-analytics.md) in [Application Insights](app-insights-overview.md). Wanneer er ook een ongebruikelijke wijziging van de trend van de resultaten, zoals een piek- of een dip identificeert Smart Diagnostics een patroon van dimensies en de bijbehorende waarden die de wijziging kunnen verklaren. Zo kunt u snel diagnose van het probleem. 

In dit voorbeeld Smart Diagnostics een patroon van eigenschapswaarden die zijn gekoppeld aan de wijziging is geïdentificeerd en markeert het verschil tussen resultaten met en zonder dit patroon:

![voorbeeld analytics diagnostics resultaat](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a>Gegevenswijzigingen onderzoeken

1.  Een query in analyses uitvoeren en deze worden weergegeven als een tijd-grafiek. 
2.  Klik op elk gewenst moment gemarkeerde piek indien aanwezig.
 
    ![piek-punt](./media/app-insights-analytics-diagnostics/peak.png)

    Diagnostische gegevens duurt een paar seconden voor het detecteren van een patroon.

3. Het tabblad diagnostische resultaten bevat een patroon dat de onderbreking van uw gegevens kan verklaren.

    ![Resultaat](./media/app-insights-analytics-diagnostics/result.png)
 
    De tekst bevat de dimensiewaarden die worden weergegeven met elkaar correleren met een verschuiving. In dit voorbeeld is deze gekoppeld aan een bepaalde aanvraag en een bepaalde browser-versie.

    U ziet ook de twee onderdelen van de grafiek, met het filter true en false. Het onderdeel false toont een trend ongewijzigd. Met andere woorden, is er geen wijziging in de resultaten telemetrie als we de problematisch combinatie van dimensies met diagnostische gegevens geconstateerd uitsluiten. Daarentegen, worden de resultaten binnen deze combinatie een indrukwekkende wijziging weergegeven binnen het gemarkeerde gebied van onderzoek. Dit betekent dat diagnostische gegevens van een combinatie van eigenschappen die de wijziging wordt is gevonden.

4.  Als het patroon complex is, moet u de muisaanwijzer op **Alles weergeven** om te zien van de dimensies.

    ![alles weergeven](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  Diagnostische gegevens vindt er geen significante patroon melding versturen over, als de pagina 'geen resultaten worden weergegeven. Op dit moment kunt u uw query wijzigen. U kunt bijvoorbeeld beperken het tijdsbereik en binning in Analytics-query voor een verdere analyse en mogelijk betere resultaten.

. Met de wetenschap dat een bepaalde pagina van uw website een probleem op een bepaalde browser heeft, u kunt nu gaat u door naar de pagina van het probleem en onderzoeken van recente wijzigingen.

## <a name="try-the-demo"></a>Probeer de demoversie

[Klik hier voor een demonstratie](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) op voorbeeldgegevens.

## <a name="how-it-works"></a>Hoe werkt het?

Smart Diagnostics maakt gebruik van een geavanceerde zonder supervisie machine learning-algoritme op basis van de [DiffPatterns](app-insights-analytics-reference.md) bewerking. Kandidaat patronen die de gewijzigde gegevens kunnen verklaren zoekt. Deze analyses van de impact van elke kandidaat voor de metrische gegevens en is het patroon dat beste met de wijziging correleert.

## <a name="no-diagnostic-points"></a>Geen diagnostische punten?

Smart Diagnostics werkt alleen wanneer de volgende criteria wordt voldaan:

 * Smart Diagnostics-instelling is ingeschakeld. Zoek in het pictogram instellingen in Analytics.
 * De optie Slimme diagnostische gegevens in de instellingen is geselecteerd. 
 * Tijdas: de x-as van de grafiek moet van het type `datetime`.
 * Line- of gebied grafiek: Diagnostics werkt alleen deze typen van grafiek. Gebruik `| render timechart` of `| render areachart` aan het einde van uw query; of Selecteer een regel of vlakdiagram van de selector vervolgkeuzelijst.
 * Onderbreking: Er moet een aanzienlijke onderbreking in de gegevens.
 * Onvoldoende punten te analyseren.
 * Niet meer dan een overzicht van de component in de query.
 * Er is geen project-component met een definitie van de naam van voordat de component samenvatten.

 
 ## <a name="related-articles"></a>Verwante artikelen:

 * [Analytics-zelfstudie](app-insights-analytics-tour.md)
 * [Detectie van smartcard](app-insights-proactive-diagnostics.md) automatisch wordt u gewezen op prestatieproblemen.