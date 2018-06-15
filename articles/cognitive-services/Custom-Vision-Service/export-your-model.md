---
title: Uw aangepaste visie-servicemodel exporteren naar mobiele - Service aangepaste visie - Azure cognitieve Services | Microsoft Docs
description: Informatie over het exporteren van het model voor gebruik bij het maken van mobiele toepassingen.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: ce8f42d6239867dd217cddfc61a27d7835dc9c9b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345919"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Exporteren van het model voor gebruik met mobiele apparaten

Aangepaste visie Service kunt classificaties om uit te voeren offline worden geëxporteerd. U kunt uw geëxporteerde classificatie insluiten in een toepassing en het lokaal uitvoeren op een apparaat voor realtime classificatie. 

Aangepaste visie Service ondersteunt de volgende uitvoer:

* __Tensorflow__ voor __Android__.
* __CoreML__ voor __iOS11__.
* __ONNX__ voor __Windows ML__.
* Een Windows- of Linux __container__. De container bevat een Tensorflow modelleren en code voor het gebruik van de aangepaste visie Service API-service. 

> [!IMPORTANT]
> Aangepaste visie Service exporteert alleen __comprimeren__ domeinen. De modellen die worden gegenereerd door compact domeinen zijn geoptimaliseerd voor de beperkingen van realtime classificatie op mobiele apparaten. Classificaties gebouwd met een compact domein mogelijk iets minder nauwkeurig dan een standaard domein met dezelfde mate van trainingsgegevens.
>
> Zie voor meer informatie over het verbeteren van de classificaties de [verbetering van de classificatie](getting-started-improving-your-classifier.md) document.

## <a name="convert-to-a-compact-domain"></a>Converteren naar een compact domein

> [!NOTE]
> De stappen in deze sectie zijn alleen van toepassing als er een bestaande classificatie die niet is ingesteld op het domein te comprimeren.
 
Als u wilt converteren van het domein van een bestaande classificatie, gebruikt u de volgende stappen uit:

1. Van de [aangepaste visie pagina](https://customvision.ai), selecteer de __Start__ pictogram om een lijst met projecten weer te geven. U kunt ook de [ https://customvision.ai/projects ](https://customvision.ai/projects) uw projecten kunnen bekijken.

    ![Afbeelding van de oorspronkelijke lijst met pictogram en projecten](./media/export-your-model/projects-list.png)

2. Selecteer een project en selecteer vervolgens de __Tandwielpictogram__ pictogram in de rechterbovenhoek van de pagina.

    ![Afbeelding van het pictogram versnelling](./media/export-your-model/gear-icon.png)

3. In de __domeinen__ sectie, selecteer een __comprimeren__ domein. Selecteer __wijzigingen opslaan__ de wijzigingen wilt opslaan.

    ![Afbeelding van domeinen selectie](./media/export-your-model/domains.png)

4. Selecteer in het begin van de pagina __Train__ naar retrain met behulp van het nieuwe domein.

## <a name="export-your-model"></a>Exporteren van uw model

Als u wilt exporteren het model na retraining, gebruik de volgende stappen:

1. Ga naar de **prestaties** tabblad en selecteer __exporteren__. 

    ![Afbeelding van het pictogram voor exporteren](./media/export-your-model/export.png)

    > [!TIP]
    > Als de __exporteren__ vermelding is niet beschikbaar en vervolgens een compact domein maakt geen gebruik van de geselecteerde herhaling. Gebruik de __iteraties__ gedeelte van deze pagina moet een herhaling die gebruikmaakt van een domein compact selecteren en selecteer vervolgens __exporteren__.

2. Selecteer de exportindeling en selecteer vervolgens __exporteren__ voor het downloaden van het model.

## <a name="next-steps"></a>Volgende stappen

Het model van uw geëxporteerde integreren met een toepassing. Verschillende voorbeeldtoepassingen zijn beschikbaar:

* Een voorbeeld voor [met behulp van het geëxporteerde CoreML-model in een iOS-toepassing](https://go.microsoft.com/fwlink/?linkid=857726) voor realtime installatiekopie classificatie met Swift
* IOS-voorbeeldtoepassing voor [met behulp van het geëxporteerde CoreML-model met Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) voor realtime installatiekopie classificatie 
* Voorbeeld voor [met behulp van het geëxporteerde Tensorflow-model in een Android-toepassing](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) voor realtime installatiekopie classificatie 
* [Met behulp van uw model Tensorflow met Windows](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/export-model-python)
* Voorbeeld voor [met behulp van het geëxporteerde ONNX-model met Windows-Machine Learning](https://azure.microsoft.com/en-us/resources/samples/cognitive-services-onnx-customvision-sample/)
