---
title: Uw model exporteren naar Mobile-Custom Vision Service
titleSuffix: Azure Cognitive Services
description: Meer informatie over het exporteren van uw model voor gebruik bij het maken van mobiele apps.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: 554a392a7f815a6e646927f137b1e6c2856099bd
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561084"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Uw model exporteren voor gebruik met mobiele apparaten

Custom Vision Service kunnen classificaties worden geëxporteerd om offline uit te voeren. U kunt uw geëxporteerde classificatie insluiten in een toepassing en deze lokaal uitvoeren op een apparaat voor realtime-classificatie.

Custom Vision Service ondersteunt de volgende export bewerkingen:

* __Tensor flow__ voor __Android__.
* __CoreML__ voor __iOS11__.
* __ONNX__ voor __Windows ml__.
* Een Windows-of Linux- __container__. De container bevat een tensor flow-model en een service code voor het gebruik van de Custom Vision Service-API. 

> [!IMPORTANT]
> Custom Vision Service exporteert alleen __compacte__ domeinen. De modellen die door compacte domeinen worden gegenereerd, zijn geoptimaliseerd voor de beperkingen van real-time classificatie op mobiele apparaten. Classificaties die zijn gebouwd met een compact domein, zijn mogelijk iets minder nauw keuriger dan een standaard domein met dezelfde hoeveelheid trainings gegevens.
>
> Zie het [classificatie document verbeteren](getting-started-improving-your-classifier.md) voor meer informatie over het verbeteren van uw classificaties.

## <a name="convert-to-a-compact-domain"></a>Converteren naar een compact domein

> [!NOTE]
> De stappen in deze sectie zijn alleen van toepassing als u een bestaande classificatie hebt die niet is ingesteld op een compact domein.

Gebruik de volgende stappen om het domein van een bestaande classificatie te converteren:

1. Selecteer op de [pagina aangepaste visie](https://customvision.ai)het pictogram __Start__ om een lijst met uw projecten weer te geven. U kunt ook de [https://customvision.ai/projects](https://customvision.ai/projects) gebruiken om uw projecten te bekijken.

    ![Afbeelding van de lijst Start pictogram en projecten](./media/export-your-model/projects-list.png)

2. Selecteer een project en selecteer vervolgens het __tandwiel__ pictogram in de rechter bovenhoek van de pagina.

    ![Afbeelding van het tandwiel pictogram](./media/export-your-model/gear-icon.png)

3. Selecteer in de sectie __domeinen__ een __compact__ domein. Selecteer __wijzigingen opslaan__ om de wijzigingen op te slaan.

    ![Afbeelding van de selectie van domeinen](./media/export-your-model/domains.png)

4. Selecteer aan de bovenkant van de pagina __trainen__ om opnieuw te trainen met het nieuwe domein.

## <a name="export-your-model"></a>Uw model exporteren

Gebruik de volgende stappen om het model na het opnieuw trainen te exporteren:

1. Ga naar het tabblad **prestaties** en selecteer __exporteren__. 

    ![Afbeelding van het pictogram voor exporteren](./media/export-your-model/export.png)

    > [!TIP]
    > Als de __export__ vermelding niet beschikbaar is, gebruikt de geselecteerde iteratie geen compact domein. Gebruik de  sectie iteraties van deze pagina om een herhaling te selecteren die gebruikmaakt van een compact domein, en selecteer vervolgens __exporteren__.

2. Selecteer de export indeling en selecteer vervolgens __exporteren__ om het model te downloaden.

## <a name="next-steps"></a>Volgende stappen

Integreer uw geëxporteerde model in een toepassing door een van de volgende artikelen of voor beelden te verkennen:

* [Uw tensor flow-model gebruiken met python](export-model-python.md)
* [Uw ONNX-model gebruiken met Windows Machine Learning](custom-vision-onnx-windows-ml.md)
* Zie het voor beeld voor [CoreML-model in een IOS-toepassing voor de](https://go.microsoft.com/fwlink/?linkid=857726) classificatie van realtime-afbeeldingen met SWIFT.
* Zie het voor beeld voor [tensor flow-model in een Android-toepassing](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) voor realtime-afbeeldings classificatie op Android.
* Zie het voor beeld voor het [CoreML-model met Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) voor de classificatie van realtime-afbeeldingen in een Xamarin IOS-app.
