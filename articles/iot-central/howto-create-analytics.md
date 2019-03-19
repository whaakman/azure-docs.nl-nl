---
title: Analyseer uw apparaatgegevens in uw Azure IoT Central-toepassing | Microsoft Docs
description: Analyseer gegevens van uw apparaat in uw Azure IoT Central-toepassing.
author: lmasieri
ms.author: lmasieri
ms.date: 02/20/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 05915e2059e45c002a9dfa71a538ed6391caa157
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58078326"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Analytics gebruiken om de apparaatgegevens van uw te analyseren

*Dit artikel is van toepassing op operators, opbouwfuncties en beheerders.*

Azure IoT Central biedt uitgebreide analysemogelijkheden, zinvol zijn van grote hoeveelheden gegevens van uw apparaten. Als u wilt beginnen, gaat u naar **Analytics** in het navigatiemenu links.

## <a name="querying-your-data"></a>Uw gegevens opvragen

U moet kiezen een **apparaat**, toevoegen een **filter** (optioneel), en selecteer een **periode** aan de slag. Als u klaar bent, selecteert u **resultaten weergeven** om te beginnen met het visualiseren van uw gegevens.

* **Apparaat instellen:** Een [apparaat](howto-use-device-sets.md) is een door de gebruiker gedefinieerde groep van uw apparaten. Bijvoorbeeld, rev alle koelkasten in Oakland of alle 2.0 wind maar.

* **Filters:** U kunt eventueel filters toevoegen aan uw zoekopdracht tot Inzoomen op uw gegevens. U kunt maximaal 10 filters tegelijk toevoegen. Bijvoorbeeld, binnen alle koelkasten in Oakland vinden die temperatuur hebben gehad gaat dan 60 graden.
* **Tijdsperiode:** Standaard wordt er gegevens ophalen uit de afgelopen 10 minuten. U kunt deze waarde wijzigen in een van de vooraf gedefinieerde tijdsbereik of een aangepaste periode.

  ![Analytics-query](media/howto-create-analytics/analytics-query.png)

## <a name="visualizing-your-data"></a>Uw gegevens te visualiseren

Nadat u uw gegevens hebt opgevraagd, kunt u zult kunnen starten met het visualiseren van deze. U kunt weergeven/verbergen metingen, wijzigen de manier waarop gegevens samengevoegd en verdere zijn gegevens splitsen op basis van de eigenschappen van een ander apparaat.  

* **Splitsen op basis van:** Het splitsen van gegevens door de eigenschappen van een apparaat kunt u verder inzoomen naar beneden in uw gegevens. Bijvoorbeeld, kunt u de resultaten op basis van apparaat-ID of locatie splitsen.

* **Metingen:** U kunt maximaal 10 verschillende telemetrie-items dat wordt gerapporteerd door uw apparaten tegelijk weergeven/verbergen. Metingen zijn, zoals temperatuur en vochtigheid.

* **Aggregatie:** Standaard we gegevens aggregeren op basis van de gemiddelde, maar u kunt kiezen om te wijzigen van de gegevens worden verzameld op iets anders aan uw behoeften voldoet.

   ![Splitsen op basis van Analytics-visualisatie](media/howto-create-analytics/analytics-splitby.png)

## <a name="interacting-with-your-data"></a>Interactie met uw gegevens

Er zijn verschillende manieren om te wijzigen van de resultaten van uw query om te voldoen aan de behoeften van uw visualisatie. U kunt schakelen tussen de grafiekweergave van een en een rasterweergave, in-en uitzoomen, vernieuw uw gegevensset en alter hoe regels worden weergegeven.

* **Raster weergeven:** De resultaten zijn beschikbaar in de vorm van een tabel, zodat u kunt bekijken van de specifieke waarde voor elk gegevenspunt. In deze weergave ook voldoet aan de toegankelijkheidsnormen.
* **Grafiek weergeven:** Uw resultaten worden weergegeven in de indeling van een regel om te herkennen omhoog of omlaag trends en afwijkingen.

  ![De rasterweergave voor uw analyses weergeven](media/howto-create-analytics/analytics-showgrid.png)

Zoomen kunt u naar de introductiepagina in voor uw gegevens. Als u een bepaalde periode die u zich richten wilt op in de resultatenset kunt vinden, gebruikt u de cursor voor het gebied dat u wilt inzoomen op en gebruikt u de beschikbare besturingselementen een van de volgende acties uit te voeren:

* **Zoom in:** Wanneer u een bepaalde periode hebt geselecteerd, wordt in te zoomen is ingeschakeld en kunt u inzoomen op uw gegevens.
* **Zoom uit:** Dit besturingselement kunt u één niveau van de laatste zoom uitzoomen. Bijvoorbeeld, als Inzoomen op die u hebt back in met uw gegevens drie keer uitzoomen vindt u-één stap tegelijk.
* **Zoomen opnieuw instellen:** Wanneer u verschillende niveaus van zoomen hebt uitgevoerd, kunt u de zoomknop opnieuw instellen om terug te keren naar de oorspronkelijke resultatenset.

  ![Zoomen op uw gegevens uitvoeren](media/howto-create-analytics/analytics-zoom.png)

U kunt de lijnstijl om te voldoen aan uw behoeften kunt wijzigen. Er zijn vier opties:

* **Regel:** Een platte lijn tussen de afzonderlijke gegevenspunten.
* **Vloeiend:** Een kromme lijn tussen elk punt.
* **Stap:** Lijn tussen elk punt op de grafiek is een stap.
* **Scatter:** Alle punten worden in de grafiek getekend zonder regels aan elkaar te koppelen.

  ![Andere regel die beschikbaar zijn in Analytics](media/howto-create-analytics/analytics-linetypes.png)

Ten slotte kunt u instellen dat uw gegevens voor de y-as door te kiezen uit een van drie modi:

* **Gestapeld:** Een grafiek voor elke meting is gestapelde en elk van de grafieken hebben hun eigen y-as. Gestapelde diagrammen zijn nuttig wanneer u meerdere metingen geselecteerd hebt en wilt afzonderlijke weergave van deze metingen zijn.
* **Niet gestapelde:** Een grafiek voor elke meting is ze worden afgezet tegen een y-as, maar de waarden voor de y-as worden gewijzigd op basis van de geselecteerde meting. Niet gestapelde diagrammen zijn handig als u wilt meerdere metingen overlay en om te zien van patronen in deze maatregelen voor dezelfde periode.
* **Gedeelde y-as:** Alle grafieken delen de dezelfde y-as en de waarden voor de as niet wijzigen. Gedeelde y-as-grafieken zijn handig als u één maateenheid kijken wilt bij het segmenteren van de gegevens met een split-by.

  ![Gegevens voor de y-as rangschikken met andere visualisatie-modi](media/howto-create-analytics/analytics-yaxis.png)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over het maken van aangepaste analytics voor uw Azure IoT Central-toepassing, de voorgestelde volgende stap als volgt:

> [!div class="nextstepaction"]
> [Voorbereiden en verbinding maken met een Node.js-toepassing](howto-connect-nodejs.md)