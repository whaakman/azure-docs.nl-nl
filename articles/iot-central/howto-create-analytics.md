---
title: Analyseer uw apparaatgegevens in uw Azure IoT Central-toepassing | Microsoft Docs
description: Analyseer gegevens van uw apparaat in uw Azure IoT Central-toepassing.
author: lmasieri
ms.author: lmasieri
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: c170f573a473d3fdfca5c916b4be594dc93d6b27
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50156320"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Analytics gebruiken om de apparaatgegevens van uw te analyseren


*Dit artikel is van toepassing op operators, opbouwfuncties en beheerders.*


Azure IoT Central biedt uitgebreide analysemogelijkheden, zinvol zijn van grote hoeveelheden gegevens van uw apparaten. Als u wilt beginnen, gaat u naar **Analytics** in het navigatiemenu links. 

  ![Navigatie naar analytics IoT Central](media\howto-create-analytics\analytics-navigation.png)

## <a name="querying-your-data"></a>Uw gegevens opvragen

U moet kiezen een **apparaat**, toevoegen een **filter** (optioneel), en selecteer een **periode** aan de slag. Als u klaar bent, klikt u op *resultaten weergeven* om te beginnen met het visualiseren van uw gegevens.


* **Apparaat instellen:** A [apparaat](howto-use-device-sets.md) is een door de gebruiker gedefinieerde groep van uw apparaten. Bijvoorbeeld, rev alle koelkasten in Oakland of alle 2.0 wind maar.

<!---
to-do: confirm if 10 is the max number of filters
to-do: do we need to explain how fiters work?
--->

* **Filters:** desgewenst kunt u filters toevoegen aan uw zoekopdracht tot Inzoomen op uw gegevens. U kunt maximaal 10 filters tegelijk toevoegen. Bijvoorbeeld, binnen alle koelkasten in Oakland vinden die temperatuur hebben gehad gaat dan 60 graden. 
* **Periode:** standaard zult we gegevens ophalen uit de afgelopen 10 minuten. U kunt deze waarde wijzigen in een van de vooraf gedefinieerde tijdsbereik of een aangepaste periode. 

 ![Analytics-query](media\howto-create-analytics\analytics-query.png)

## <a name="visualizing-your-data"></a>Uw gegevens te visualiseren

Nadat u uw gegevens hebt opgevraagd, kunt u zult kunnen starten met het visualiseren van deze. U kunt weergeven/verbergen metingen, wijzigen de manier waarop gegevens samengevoegd en verdere zijn gegevens splitsen op basis van de eigenschappen van een ander apparaat.  

* **Delen door:** splitsen van gegevens door de eigenschappen van een apparaat kunt u verder inzoomen naar beneden in uw gegevens. Bijvoorbeeld, kunt u de resultaten op basis van apparaat-ID of locatie splitsen.
<!---
to-do: confirm if 10 is the max number of measurements
--->
* **Metingen:** u kunt maximaal 10 verschillende telemetrie-items dat wordt gerapporteerd door uw apparaten tegelijk weergeven/verbergen. Metingen zijn, zoals temperatuur en vochtigheid. 
* **Aggregatie:** standaard we gegevens aggregeren op basis van de gemiddelde, maar u kunt kiezen om te wijzigen van de gegevens worden verzameld op iets anders aan uw behoeften voldoet. 

   ![Analytics-visualisatie](media\howto-create-analytics\analytics-visualize.png) <br/><br/>
   ![Splitsen op basis van Analytics-visualisatie](media\howto-create-analytics\analytics-splitby.png)

## <a name="interacting-with-your-data"></a>Interactie met uw gegevens

Er zijn verschillende manieren waarin u de resultaten van uw query om te voldoen aan de behoeften van uw visualisatie meer kunt wijzigen. U kunt schakelen tussen de grafiekweergave van een en een rasterweergave, Inzoomen en uitzoomen, vernieuw uw gegevensset en alter hoe regels worden weergegeven.

* **Raster weergeven:** uw resultaten zijn beschikbaar in de vorm van een tabel, zodat u kunt bekijken van de specifieke waarde voor elk gegevenspunt. In deze weergave ook voldoet aan de toegankelijkheidsnormen. 
* **Grafiek weergeven:** uw resultaten worden weergegeven in de indeling van een regel eenvoudig omhoog/omlaag punten trends en afwijkingen. 

 ![De rasterweergave voor uw analyses weergeven](media\howto-create-analytics\analytics-showgrid.png)

Zoomen kunt u inzoomen op uw gegevens. Als u een bepaalde periode die u zich richten wilt op in de resultatenset kunt vinden, gebruikt u de cursor voor het gebied dat u wilt inzoomen op en gebruikt u de beschikbare besturingselementen een van de volgende acties uit te voeren:
* **Inzoomen:** wanneer u een bepaalde periode hebt geselecteerd, zoomen in wordt ingeschakeld en kunt u inzoomen op uw gegevens.
* **Uitzoomen:** dit besturingselement kunt u één niveau van de laatste zoom uitzoomen. Bijvoorbeeld, als u hebt Inzoomen op uw gegevens drie keer, uitzoomen gaat u een back-één stap tegelijk.
* **Zoomen opnieuw instellen:** wanneer u verschillende niveaus van zoomen hebt uitgevoerd, kunt u de zoomknop opnieuw instellen om terug te keren naar de oorspronkelijke resultatenset. 

 ![Zoomen op uw gegevens uitvoeren](media\howto-create-analytics\analytics-zoom.png)


U kunt de lijnstijl om te voldoen aan uw behoeften kunt wijzigen. U zijn kunt kiezen uit vier opties:
* **Regel:** een platte lijn tussen de afzonderlijke gegevenspunten worden gevormd. 
* **Vloeiend:** een kromme lijn tussen elk punt worden gevormd
* **Stap:** lijn tussen elk punt op de grafiek wordt een stap-grafiek maken
* **Spreidingsdiagrammen:** zonder regels verbindt deze met alle punten in de grafiek worden getekend. 

 ![Andere regel die beschikbaar zijn in Analytics](media\howto-create-analytics\analytics-linetypes.png)

Ten slotte kunt u instellen dat uw gegevens voor de y-as door te kiezen uit een van de drie modi:

* **Gestapeld:** een grafiek voor elke meting is gestapelde en elk van de grafieken hebben hun eigen y-as. Gestapelde diagrammen zijn nuttig wanneer u meerdere metingen geselecteerd hebt en wilt afzonderlijke weergave van deze metingen zijn.
* **Niet gestapelde:** een grafiek voor elke meting is ze worden afgezet tegen een y-as, maar de waarden voor de y-as worden gewijzigd op basis van de geselecteerde meting. Niet gestapelde diagrammen zijn handig als u wilt meerdere metingen overlay en om te zien van patronen in deze maatregelen voor dezelfde periode.
* **Gedeelde y-as:** alle grafieken delen de dezelfde y-as en de waarden voor de as niet wijzigen. Gedeelde y-as-grafieken zijn handig als u één maateenheid kijken wilt bij het segmenteren van de gegevens met een split-by.

 ![Gegevens voor de y-as rangschikken met andere visualisatie-modi](media\howto-create-analytics\analytics-yaxis.png)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over het maken van aangepaste analytics voor uw Azure IoT Central-toepassing, de voorgestelde volgende stap als volgt:

> [!div class="nextstepaction"]
> [Voorbereiden en verbinding maken met een Node.js-toepassing](howto-connect-nodejs.md)