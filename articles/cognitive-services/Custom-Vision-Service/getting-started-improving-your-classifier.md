---
title: De classificatie met behulp van Custom Vision Service - Azure Cognitive Services verbeteren | Microsoft Docs
description: Meer informatie over het verbeteren van de kwaliteit van uw classificatie Custom Vision Service.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 07/05/2018
ms.author: nolachar
ms.openlocfilehash: 7c6cbd996d0c35b96fde78daf391bebb36feddce
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888174"
---
# <a name="how-to-improve-your-classifier"></a>Hoe u de classificatie verbeteren

Meer informatie over het verbeteren van de kwaliteit van uw classificatie Custom Vision Service. De kwaliteit van de classificatie is afhankelijk van de hoeveelheid, de kwaliteit en de verscheidenheid aan de gelabelde gegevens die u opgeeft en hoe met gelijke taakverdeling is van de gegevensset. Een goede classificatie heeft doorgaans een gegevensset met gelijke taakverdeling training die representatief is voor wat worden verzonden naar de classificatie. Het proces van het bouwen van een dergelijke classificatie is vaak iteratieve. Het is gebruikelijk te nemen van een paar Rondt van training, verwachte resultaten bereiken.

Hier volgen de algemene stappen voor het verbeteren van een classificatie. Deze stappen zijn niet moeilijk en snelle regels, maar methodiek waarmee u een betere classificatie maken.

1. Eerste-round-training
1. Meer installatiekopieën toevoegen en gegevens in balans brengen
1. Retrain
1. Toevoegen van afbeeldingen met verschillende achtergrond, belichting, objectgrootte, camerahoek en stijl
1. Opnieuw trainen & kanaal in de afbeelding voor voorspelling
1. Bekijk de voorspellingsresultaten
1. Bestaande trainingsgegevens wijzigen

## <a name="data-quantity-and-data-balance"></a>Verdelen over de hoeveelheid en de gegevens van de gegevens

Het belangrijkste is voor het uploaden onvoldoende installatiekopieën trainen van de classificatie. Ten minste 50 afbeeldingen per label voor de trainingsset worden aanbevolen als uitgangspunt. Er is een sterke risico dat u bent overfitting met minder afbeeldingen. Terwijl de nummers van de prestaties kunnen goed worden voorgesteld, is het mogelijk moeilijk op basis van gegevens van de echte wereld werkt. De classificatie met meer installatiekopieën training, wordt de nauwkeurigheid van de voorspellingsresultaten algemeen verhoogd.

Een andere overweging is dat u moet ervoor zorgen dat uw gegevens is verdeeld. Bijvoorbeeld 500 installatiekopieën voor een label en 50 installatiekopieën voor een ander label in te stellen, wordt een gegevensset imbalanced training, waardoor het model worden meer nauwkeurige bij het voorspellen van een label dan de andere produceren. U bent waarschijnlijk betere resultaten worden weergegeven als u ten minste een 1:2-verhouding tussen het label met de minste afbeeldingen en het label met de meeste afbeeldingen onderhouden. Bijvoorbeeld, als het label met het grootste aantal afbeeldingen 500 installatiekopieën bevat, moet het label met de minste afbeeldingen ten minste 250 afbeeldingen voor training.

## <a name="train-more-diverse-images"></a>Meer verschillende afbeeldingen trainen

Installatiekopieën die representatief zijn voor wat worden verzonden naar de classificatie bij normaal gebruik bieden. Bijvoorbeeld, als u een classificatie 'apple', de gehanteerde classificatie mogelijk niet nauwkeurig als u foto's van de appels in platen, maar zorg voorspellingen op foto's van appels op structuren alleen trainen. Met inbegrip van tal van installatiekopieën zorgen ervoor dat de classificatie is niet gericht en goed kunt generaliseren. Hieronder worden enkele manieren die kunt u uw training diverser ingesteld:

__Achtergrond:__ installatiekopieën van het object in het zicht van verschillende achtergronden (dat wil zeggen, de vruchten op plaat versus vruchten in eigenschappenverzameling voor kruidenierswaren) bieden. Foto's in de context zijn beter dan foto's in het zicht van neutrale achtergronden, zoals ze vindt u meer informatie voor de classificatie.

![Afbeelding van achtergrond-voorbeelden](./media/getting-started-improving-your-classifier/background.png)

__Verlichting:__ installatiekopieën voorzien van uiteenlopende belichting (dat wil zeggen, genomen met flash, hoge blootstelling, enz.), met name als de afbeeldingen die worden gebruikt voor voorspelling verschillende belichting hebben. Het is ook handig om op te nemen van installatiekopieën met gevarieerde verzadiging, hue en helderheid.

![Afbeelding van verlichting voorbeelden](./media/getting-started-improving-your-classifier/lighting.png)

__Objectgrootte van:__ Geef installatiekopieën waarin de objecten van zijn verschillende grootte voor het vastleggen van verschillende onderdelen van het object. Een foto van Boeket bijvoorbeeld worden gepresenteerd bananen en een Close-up van een enkele bananen. Verschillende sizing helpt de classificatie beter te generaliseren.

![Afbeelding van de grootte van voorbeelden](./media/getting-started-improving-your-classifier/size.png)

__Camerahoek:__ installatiekopieën die zijn gemaakt met verschillende camerahoeken te bieden. Als uw foto's worden genomen met een set van vaste camera's (zoals surveillancecamera's), zorg er dan voor dat u een ander label toewijzen aan elke camera, zelfs als ze dezelfde objecten om te voorkomen dat overfitting - modellering van niet-gerelateerde objecten (zoals lampposts) als de belangrijkste functie vastleggen.

![Afbeelding van de hoek-voorbeelden](./media/getting-started-improving-your-classifier/angle.png)

__Stijl:__ afbeeldingen van verschillende stijlen van dezelfde klasse (dat wil zeggen, verschillende soorten citrus) bieden. Echter, hebt u afbeeldingen van objecten van hebt u aanzienlijk verschillende stijlen (dat wil zeggen, Mickey muis ten opzichte van een echte gesprekken rat), het verdient aanbeveling hun afzonderlijke functies label als afzonderlijke klassen beter vertegenwoordigen.

![Afbeelding van de stijl van voorbeelden](./media/getting-started-improving-your-classifier/style.png)

## <a name="use-images-submitted-for-prediction"></a>Gebruik-installatiekopieën die zijn ingediend voor voorspelling

De Custom Vision Service slaat installatiekopieën verzonden naar het eindpunt van de voorspelling. Als u wilt deze installatiekopieën gebruiken voor het verbeteren van de classificatie, gebruikt u de volgende stappen uit:

1. Voor installatiekopieën die worden verzonden naar de classificatie opent de [Custom Vision webpagina](https://customvision.ai), gaat u naar uw project en selecteer de __voorspellingen__ tabblad. De standaardweergave ziet u afbeeldingen van de huidige iteratie. U kunt de __iteratie__ vervolgkeuzelijst veld om installatiekopieën verzonden tijdens de vorige iteraties weer te geven.

    ![Afbeelding van het tabblad voorspellingen](./media/getting-started-improving-your-classifier/predictions.png)

2. Beweeg de muisaanwijzer over een afbeelding om te zien van de labels die door de classificatie zijn voorspeld. Afbeeldingen worden gerangschikt, zodat de afbeeldingen die de meeste winst aan de classificatie bieden kunnen aan de bovenkant zijn. Selecteer een andere sortering, gebruikt u de __sorteren__ sectie. Als u wilt een afbeelding toevoegen aan uw bestaande trainingsgegevens, selecteer de installatiekopie, selecteer de juiste code en klikt u op __opslaan en sluiten__. De installatiekopie wordt verwijderd uit __voorspellingen__ en toegevoegd aan de training-installatiekopieën. U kunt deze weergeven door de __Trainingsafbeeldingen__ tabblad.

    ![Afbeelding van de labels pagina](./media/getting-started-improving-your-classifier/tag.png)

3. Gebruik de __Train__ knop de classificatie te trainen.

## <a name="visually-inspect-predictions"></a>Visueel inspecteren voorspellingen

Als u wilt controleren voorspellingen van de installatiekopie, selecteer de __Trainingsafbeeldingen__ tabblad en selecteer vervolgens __iteratie geschiedenis__. Installatiekopieën die worden beschreven met een rood kader zijn onjuist voorspeld.

![Afbeelding van de geschiedenis herhaling](./media/getting-started-improving-your-classifier/iteration.png)

Soms visueel inspecteren kunt patronen die u verhelpen kunt door aanvullende gegevens toe te voegen of te wijzigen van bestaande trainingsgegevens identificeren. Een classificatie voor apple versus lichtgroen kan bijvoorbeeld onjuist alle groen appels labelen als lijm. U kunt mogelijk kunt dit probleem oplossen door toe te voegen en het trainen van gegevens met gelabelde afbeeldingen van groen appels leveren.

## <a name="unexpected-classification"></a>Onverwachte indeling

De classificatie leert soms verkeerd kenmerken die uw installatiekopieën met elkaar gemeen hebben. Als u het maken van een classificatie voor appels versus citrus en opgegeven installatiekopieën van appels in handen en citrus in wit platen zijn, kan bijvoorbeeld de classificatie voor handen versus wit platen in plaats van appels versus citrus trainen.

![Afbeelding van onverwachte indeling](./media/getting-started-improving-your-classifier/unexpected.png)

Op dit probleem te corrigeren, gebruikt u de bovenstaande richtlijnen op training met meer verschillende installatiekopieën: installatiekopieën voorzien van verschillende hoeken, achtergronden, grootte van objecten, groepen en andere varianten.

## <a name="negative-image-handling"></a>Verwerking van negatieve afbeeldingen

De Custom Vision Service biedt ondersteuning voor de verwerking van sommige automatische negatieve afbeeldingen. In het geval waar u het bouwen van een gedeeltelijk versus bananen classificatie en een afbeelding van een schoen voor voorspelling verzenden, moet de classificatie die installatiekopie als dicht bij 0% score voor gedeeltelijk en bananen.

In gevallen waar de negatieve afbeeldingen slechts een variant van de afbeeldingen in training gebruikt zijn, is het aan de andere kant waarschijnlijk dat het model wordt het classificeren van de installatiekopieën van het negatieve als een gelabelde klasse vanwege de grote overeenkomsten. Bijvoorbeeld, als u een oranje versus pompelmoezen en pomelo's classificatie hebt en u in een afbeelding van een clementine feed, kan deze de clementine beoordelen als oranje. Dit probleem kan optreden omdat veel functies van de clementine (kleur, vorm, patroon, natuurlijke habitat, enzovoort) lijkt op die van appels.  Als uw negatieve afbeeldingen van deze aard zijn, het verdient aanbeveling om te maken van een of meer tags ('Overig') gescheiden en label negatieve installatiekopieën met deze tag tijdens de training om toe te staan van het model beter onderscheid maken tussen deze klassen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u kunt testen installatiekopieën via een programma door indienen bij de Voorspellings-API.

> [!div class="nextstepaction"]
[De voorspellings-API gebruiken](use-prediction-api.md)
