---
title: De classificatie met behulp van aangepaste visie Service - cognitieve Azure-Services te verbeteren | Microsoft Docs
description: Informatie over het verbeteren van de kwaliteit van uw aangepaste visie Service-classificatie.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 65b1424f259066c7d5bd6b2b508d2a4052ff0527
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344904"
---
# <a name="how-to-improve-your-classifier"></a>Het verbeteren van de classificatie

Informatie over het verbeteren van de kwaliteit van uw aangepaste visie Service-classificatie. De kwaliteit van de classificatie is afhankelijk van de kwaliteit van de gelabelde gegevens die u bieden. 

## <a name="train-more-varied-images"></a>Meer uiteenlopende installatiekopieën trainen

Installatiekopieën van het label voorzien van verschillende hoeken, achtergrond, de objectgrootte van het, groepen van foto's en andere varianten en verbetert de classificatie. Foto's in de context zijn beter dan foto's voor neutrale achtergrond. Installatiekopieën die representatief zijn voor wat worden verzonden naar de classificatie die bij normaal gebruik bevatten.

Zie voor meer informatie over het toevoegen van installatiekopieën van de [bouwen van een classificatie](getting-started-build-a-classifier.md) document.

> [!IMPORTANT]
> Houd er rekening mee voor het trainen van de classificatie nadat u de afbeeldingen hebt toegevoegd.

## <a name="use-images-submitted-for-prediction"></a>Installatiekopieën die zijn ingediend voor de prognose gebruiken

De aangepaste visie Service slaat installatiekopieën verzonden met het eindpunt van de voorspelling. Als u deze afbeeldingen wilt verbeteren van de classificatie, gebruikt u de volgende stappen uit:

1. Voor installatiekopieën die zijn ingediend bij de classificatie weergeven, opent u de [aangepaste visie webpagina](https://customvision.ai) en selecteer de __voorspellingen__ tabblad.

    ![Afbeelding van het tabblad voorspellingen](./media/getting-started-improving-your-classifier/predictions-tab.png)

    > [!TIP]
    > De standaardweergave ziet u installatiekopieën van de huidige herhaling. U kunt de __herhaling__ vervolgkeuzelijst veld afbeeldingen die zijn ingediend in het vorige iteraties bekijken.

2. Beweeg de muisaanwijzer over een afbeelding om te zien van de labels die door de classificatie zijn voorspeld.

    > [!TIP]
    > Afbeeldingen zijn gerangschikt, zodat de afbeeldingen die de meeste prestatievoordelen classificatie kunnen bieden aan de bovenkant. Selecteer een andere sortering, gebruiken de __sorteren__ sectie.

    Als u wilt een afbeelding toevoegen aan uw trainingsgegevens, selecteert u de installatiekopie, selecteert u het label en selecteer vervolgens __opslaan en sluiten__. De afbeelding wordt verwijderd uit __voorspellingen__ en toegevoegd aan de training-installatiekopieën. U kunt het weergeven door de __Training installatiekopieën__ tabblad.

    ![Afbeelding van de labels pagina](./media/getting-started-improving-your-classifier/tag-image.png)

3. Gebruik de __Train__ knop opnieuw trainen van de classificatie.

## <a name="visually-inspect-predictions"></a>Voorspellingen visueel controleren

Als u wilt controleren installatiekopie voorspellingen, selecteer de __Training installatiekopieën__ tabblad en selecteer vervolgens __herhaling geschiedenis__. Installatiekopieën die worden beschreven met een rood kader zijn onjuist voorspeld.

![Afbeelding van de geschiedenis van de herhaling](./media/getting-started-improving-your-classifier/iteration-history.png)

Soms visuele inspectie kunt identificeren patronen die u vervolgens corrigeren kunt door aanvullende trainingsgegevens toe te voegen. Een classificatie voor rozen versus daises mogelijk alle wit rozen bijvoorbeeld onjuist label als daises. U kunt mogelijk dit probleem oplossen door toe te voegen en het geven van trainingsgegevens met gelabelde afbeeldingen van wit rozen.

## <a name="unexpected-classification"></a>Onverwachte indeling

De classificatie leert soms kenmerken die uw installatiekopieën gemeen hebben. U wilt bijvoorbeeld een classificatie voor rozen versus tulpen maken. U opgeven installatiekopieën van tulpen in velden en rozen in een rode vaas voor de blauwe wanden. Deze gegevens opgegeven, kan de classificatie voor veld versus wanden + vaas in plaats van rozen versus tulpen trainen.

Voor dit probleem oplossen, gebruikt u de instructies op training met meer verspreid afbeeldingen: afbeeldingen voorzien van verschillende hoeken, achtergrond, grootte van objecten, groepen en andere varianten.

## <a name="negative-image-handling"></a>Verwerking van negatieve afbeeldingen

De aangepaste visie-Service ondersteunt de verwerking van sommige automatische negatieve afbeeldingen. Als u een cat versus aquaduct classificatie maken wilt en verzenden van een installatiekopie van een schoen voor voorspelling, moet de classificatie die installatiekopie als dicht bij 0% beoordelen voor zowel cat als aquaduct. 

> [!WARNING]
> De automatische methode werkt voor duidelijk negatief installatiekopieën. Werkt mogelijk niet goed in gevallen waarin de installatiekopieën van het negatieve zojuist een variant van de afbeeldingen in training gebruikt. 
>
> Als u een husky versus corgi classificatie hebt en u in een installatiekopie van een Pomeranian feed, kan deze de Pomeranian score als een Husky. Als uw negatieve afbeeldingen van deze aard, maakt u een nieuw label (zoals 'Overig'), en dit toepassen op de installatiekopieën van het negatieve training.

## <a name="next-steps"></a>Volgende stappen

[Gebruik de voorspelling API](use-prediction-api.md)