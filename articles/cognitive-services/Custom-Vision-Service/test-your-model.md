---
title: Testen en opnieuw trainen van een Azure - Service aangepaste visie - model cognitieve Services | Microsoft Docs
description: Informatie over het testen van een installatiekopie en vervolgens worden gebruikt voor het model opnieuw trainen.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 1933b1a45844ac99308baebe59b49687a957abfa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345040"
---
# <a name="test-and-retrain-a-model-with-custom-vision-service"></a>Testen en opnieuw trainen van een model met aangepaste visie Service

Nadat u uw model trainen, kunt u snel testen met behulp van de afbeelding van een lokaal opgeslagen of een online-installatiekopie. De test maakt gebruik van de meest recent getraind herhaling.

## <a name="test-your-model"></a>Uw model testen

1. Van de [aangepaste visie webpagina](https://customvision.ai), selecteer uw project. Selecteer **snelle Test** aan de rechterkant van de bovenste menubalk. Deze actie wordt een venster met het label geopend **snelle Test**.

    ![De knop snel testen wordt weergegeven in de rechterbovenhoek van het venster.](./media/test-your-model/quick-test-button.png)

2. In de **snel testen** venster, klikt u in de **indienen installatiekopie** veld en voer de URL van de installatiekopie die u wilt gebruiken voor uw test. Als u de installatiekopie van een lokaal opgeslagen in plaats daarvan gebruiken wilt, klikt u op de **bladeren lokale bestanden** en selecteer een afbeelding voor lokaal bestand.

    ![Afbeelding van de pagina van de afbeelding verzenden](./media/test-your-model/submit-image.png)

De geselecteerde afbeelding wordt weergegeven in het midden van de pagina. En vervolgens de resultaten worden weergegeven onder de afbeelding in de vorm van een tabel met twee kolommen, met het label **labels** en **vertrouwen**. Nadat u de resultaten bekijken, kunt u sluiten de **snelle Test** venster.

U kunt nu deze testinstallatiekopie toevoegen aan uw model en vervolgens opnieuw uw model te trainen.

## <a name="use-the-predicted-image-for-training"></a>De voorspelde installatiekopie gebruiken voor training.

Gebruik de volgende stappen voor het gebruik van de installatiekopie van het eerder verzonden voor training:

1. Voor installatiekopieën die zijn ingediend bij de classificatie weergeven, opent u de [aangepaste visie webpagina](https://customvision.ai) en selecteer de __voorspellingen__ tabblad.

    ![Afbeelding van het tabblad voorspellingen](./media/test-your-model/predictions-tab.png)

    > [!TIP]
    > De standaardweergave ziet u installatiekopieën van de huidige herhaling. U kunt de __herhaling__ vervolgkeuzelijst veld afbeeldingen die zijn ingediend in het vorige iteraties bekijken.

2. Beweeg de muisaanwijzer over een afbeelding om te zien van de labels die door de classificatie zijn voorspeld.

    > [!TIP]
    > Afbeeldingen zijn gerangschikt, zodat de afbeeldingen die de meeste prestatievoordelen classificatie kunnen bieden aan de bovenkant. Selecteer een andere sortering, gebruiken de __sorteren__ sectie.

    Als u wilt een afbeelding toevoegen aan uw trainingsgegevens, selecteert u de installatiekopie, selecteert u het label en selecteer vervolgens __opslaan en sluiten__. De afbeelding wordt verwijderd uit __voorspellingen__ en toegevoegd aan de training-installatiekopieën. U kunt het weergeven door de __Training installatiekopieën__ tabblad.

    ![Afbeelding van de labels pagina](./media/test-your-model/tag-image.png)

3. Gebruik de __Train__ knop opnieuw trainen van de classificatie.

## <a name="next-steps"></a>Volgende stappen

[De classificatie verbeteren](getting-started-improving-your-classifier.md)