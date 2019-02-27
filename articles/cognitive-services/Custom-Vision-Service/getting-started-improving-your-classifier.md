---
title: Verbetering van de classificatie - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Leer hoe u de kwaliteit van de classificatie verbeteren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.openlocfilehash: 6e97001497ebdddebe9b5b726e0526a987b6c63c
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56873318"
---
# <a name="how-to-improve-your-classifier"></a>Hoe u de classificatie verbeteren

In deze handleiding leert u hoe u om de kwaliteit van uw classificatie Custom Vision Service te verbeteren. De kwaliteit van de classificatie is afhankelijk van de hoeveelheid, de kwaliteit en de verscheidenheid aan de gelabelde gegevens die u opgeeft en hoe met gelijke taakverdeling is van de algehele gegevensset. Een goede classificatie heeft een gegevensset met gelijke taakverdeling training die representatief is voor wat worden verzonden naar de classificatie. Het proces van het bouwen van een dergelijke classificatie is iteratieve; het is gebruikelijk te nemen van een paar Rondt van training, verwachte resultaten bereiken.

Hier volgt een algemeen patroon waarmee u een meer nauwkeurige classificatie:

1. Eerste-round-training
1. Meer installatiekopieën en saldo gegevens; toevoegen opnieuw trainen
1. Toevoegen van afbeeldingen met verschillende achtergrond, belichting, objectgrootte, camerahoek en stijl; opnieuw trainen
1. Nieuwe installatiekopieën gebruiken voor het testen van voorspelling
1. Bestaande trainingsgegevens op basis van voorspellingsresultaten wijzigen

## <a name="overfitting"></a>Overfitting

Een classificatie leert soms voorspellingen op basis van willekeurige kenmerken die uw installatiekopieën gemeen hebben. Als u een classificatie voor appels versus citrus maakt en u afbeeldingen van appels in handen en citrus op wit platen hebt gebruikt, kan de classificatie onnodige belang in de handen versus platen, in plaats van appels versus citrus geven.

![Afbeelding van onverwachte indeling](./media/getting-started-improving-your-classifier/unexpected.png)

Op dit probleem te corrigeren, gebruikt u de volgende richtlijnen op training met meer verschillende installatiekopieën: installatiekopieën voorzien van verschillende hoeken, achtergronden, grootte van objecten, groepen en andere verschillen.

## <a name="data-quantity"></a>De hoeveelheid gegevens

Het aantal trainingsafbeeldingen is de belangrijkste factor. U wordt aangeraden ten minste 50 afbeeldingen per label als uitgangspunt gebruiken. Er is een hogere kans te met minder installatiekopieën en terwijl de nummers van de prestaties kunnen goed worden voorgesteld, het model is het mogelijk moeilijk met echte gegevens. 

## <a name="data-balance"></a>Gegevens verdelen

Het is ook belangrijk te bedenken van de relatieve hoeveelheid uw trainingsgegevens. Bijvoorbeeld, maakt het gebruik van 500 installatiekopieën voor een label en 50 installatiekopieën voor een ander label voor een gegevensset imbalanced training. Dit zorgt ervoor dat het model worden meer nauwkeurige bij het voorspellen van een label dan het andere. U bent waarschijnlijk betere resultaten worden weergegeven als u ten minste een 1:2-verhouding tussen het label met de minste afbeeldingen en het label met de meeste afbeeldingen onderhouden. Bijvoorbeeld, als het label met de meeste afbeeldingen 500 installatiekopieën bevat, moet het label met de minste afbeeldingen minimaal 250 installatiekopieën voor training.

## <a name="data-variety"></a>Verschillende gegevens

Zorg ervoor dat installatiekopieën die representatief zijn voor wat worden verzonden naar de classificatie bij normaal gebruik gebruiken. Anders wordt de gehanteerde classificatie kan meer informatie over voorspellingen op basis van willekeurige kenmerken die uw installatiekopieën gemeen hebben. Als u een classificatie voor appels versus citrus maakt en u afbeeldingen van appels in handen en citrus op wit platen hebt gebruikt, kan de classificatie onnodige belang in de handen versus platen, in plaats van appels versus citrus geven.

![Afbeelding van onverwachte indeling](./media/getting-started-improving-your-classifier/unexpected.png)

U lost dit probleem, bevatten een verscheidenheid aan installatiekopieën om ervoor te zorgen dat uw classificatie goed kunt generaliseren. Hieronder worden enkele manieren die kunt u uw training diverser ingesteld:

* __Achtergrond:__ Installatiekopieën van het object in het zicht van verschillende achtergronden bieden. Foto's in de context van de natuurlijke zijn beter dan foto's in het zicht van neutrale achtergronden, zoals ze vindt u meer informatie voor de classificatie.

    ![Afbeelding van achtergrond-voorbeelden](./media/getting-started-improving-your-classifier/background.png)

* __Verlichting:__ Bieden van afbeeldingen met gevarieerde belichting (die wordt uitgevoerd met flash, hoge blootstelling), enzovoort, met name als de afbeeldingen die worden gebruikt voor voorspelling verschillende belichting hebben. Het is ook handig om te gebruiken installatiekopieën met verschillende verzadiging, hue en helderheid.

    ![Afbeelding van verlichting voorbeelden](./media/getting-started-improving-your-classifier/lighting.png)

* __Objectgrootte van:__ Bieden van installatiekopieën waarin de objecten in grootte en het aantal variëren (bijvoorbeeld een foto van boeket worden gepresenteerd bananen en een Close-up van een enkele bananen). Verschillende sizing helpt de classificatie beter te generaliseren.

    ![Afbeelding van de grootte van voorbeelden](./media/getting-started-improving-your-classifier/size.png)

* __Camerahoek:__ Afbeeldingen die met verschillende camerahoeken bieden. U kunt ook als al uw foto's moeten worden genomen met vaste camera's (zoals surveillancecamera's), moet u een ander label toewijzen aan elk regelmatig voordoet object om te voorkomen dat te&mdash;interpretatie van niet-gerelateerde objecten (zoals lampposts) Als de belangrijkste functie.

    ![Afbeelding van de hoek-voorbeelden](./media/getting-started-improving-your-classifier/angle.png)

* __Stijl:__ Installatiekopieën van verschillende stijlen van dezelfde klasse (bijvoorbeeld verschillende soorten van de dezelfde vruchten) bevatten. Echter, als u objecten aanzienlijk verschillen stijlen (zoals Mickey muis vergeleken met een muis concrete) hebt, raden wij dat u ze een label als afzonderlijke klassen beter vertegenwoordigen de afzonderlijke functies.

    ![Afbeelding van de stijl van voorbeelden](./media/getting-started-improving-your-classifier/style.png)

## <a name="use-prediction-images-for-further-training"></a>Voorspelling installatiekopieën gebruiken voor het trainen van verdere

Wanneer u of de classificatie van de installatiekopie testen door in te dienen installatiekopieën naar het eindpunt voor de voorspelling, slaat de Custom Vision service de afbeeldingen. U kunt deze vervolgens gebruiken voor het verbeteren van het model.

1. Voor installatiekopieën die worden verzonden naar de classificatie opent de [Custom Vision webpagina](https://customvision.ai), gaat u naar uw project en selecteer de __voorspellingen__ tabblad. De standaardweergave ziet u afbeeldingen van de huidige iteratie. U kunt de __iteratie__ vervolgkeuzemenu om installatiekopieën verzonden tijdens de vorige iteraties weer te geven.

    ![Schermafbeelding van de voorspellingen tabblad met de installatiekopieën in de weergave](./media/getting-started-improving-your-classifier/predictions.png)

2. Beweeg de muisaanwijzer over een afbeelding om te zien van de labels die door de classificatie zijn voorspeld. Afbeeldingen worden gesorteerd zodat degene die de meeste verbeteringen in de classificatie kunnen brengen worden weergegeven boven. Voor het gebruik van een andere sortering, maak een selectie in de __sorteren__ sectie. 

    Als u wilt een afbeelding toevoegen aan uw bestaande trainingsgegevens, selecteer de installatiekopie, stelt u de juiste tag(s) en klikt u op __opslaan en sluiten__. De installatiekopie wordt verwijderd uit __voorspellingen__ en toegevoegd aan de set trainingsafbeeldingen. U kunt deze weergeven door de __Trainingsafbeeldingen__ tabblad.

    ![Afbeelding van de labels pagina](./media/getting-started-improving-your-classifier/tag.png)

3. Gebruik vervolgens de __Train__ knop de classificatie te trainen.

## <a name="visually-inspect-predictions"></a>Visueel inspecteren voorspellingen

Als u wilt controleren voorspellingen van de installatiekopie, gaat u naar de __Trainingsafbeeldingen__ tabblad, selecteert u uw vorige iteratie training in de **iteratie** vervolgkeuzelijst, en schakel een of meer labels onder de **Tags** sectie. De weergave wordt nu een rood kader rond elk van de installatiekopieën waarvoor het model kan niet correct voorspellen van het opgegeven label weergegeven.

![Afbeelding van de geschiedenis herhaling](./media/getting-started-improving-your-classifier/iteration.png)

Een visueel inspecteren kunt soms patronen die u verhelpen kunt door meer trainingsgegevens toe te voegen of te wijzigen van bestaande trainingsgegevens identificeren. Een classificatie voor appels versus lijm kan bijvoorbeeld onjuist alle groen appels labelen als lijm. U kunt dit probleem verhelpen door toe te voegen en het trainen van gegevens met gelabelde afbeeldingen van groen appels leveren.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u verschillende technieken zodat uw aangepaste installatiekopie classificeringsmodel nauwkeurigere geleerd. Hierna leert u hoe u kunt installatiekopieën via een programma te testen door indienen bij de Voorspellings-API.

> [!div class="nextstepaction"]
[De voorspellings-API gebruiken](use-prediction-api.md)
