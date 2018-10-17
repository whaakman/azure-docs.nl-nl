---
title: 'Zelfstudie: Een ONNX-model gebruiken met Windows ML: Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Lees hoe u een Windows UWP-app maakt die gebruikmaakt van een ONNX-model dat uit Azure Cognitive Services is geëxporteerd.
services: cognitive-services
author: larryfr
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 06/19/2018
ms.author: larryfr
ms.openlocfilehash: 3a9e9bc92ce38c4bb8d6d83c8017fa223342e7d2
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365601"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Zelfstudie: Een ONNX-model uit Custom Vision gebruiken met Windows ML (preview)

Lees hoe u een ONNX-model gebruikt dat uit de Custom Vision Service is geëxporteerd met Windows ML (preview).

De informatie in dit document laat zien hoe u een ONNX-bestand gebruikt dat is geëxporteerd uit de Custom Vision Service met Windows ML. Er wordt een voorbeeld gegeven van een Windows UWP-toepassing. Het voorbeeld bevat een getraind model dat honden en katten kan herkennen. Ook worden er stappen weergegeven om uw eigen model bij dit voorbeeld te gebruiken.

> [!div class="checklist"]
> * Over de voorbeeld-app
> * De voorbeeldcode halen
> * Het voorbeeld uitvoeren
> * Uw eigen model gebruiken

## <a name="prerequisites"></a>Vereisten

* Een apparaat met Windows 10 met:

    * Een camera.

    * Visual Studio 2017 versie 15.7 of later waarbij de __ontwikkelworkload van Universal Windows Platform__ is ingeschakeld.

    * Ontwikkelmodus ingeschakeld. Zie het document [Ontwikkeling voor uw apparaat inschakelen](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) voor meer informatie.

* (Optioneel) Een ONNX-bestand dat uit de Custom Vision Service is geëxporteerd. Zie het document [Uw model exporteren om met mobiele apparaten te gebruiken](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) voor meer informatie.

    > [!NOTE]
    > Als u uw eigen model wilt gebruiken, volgt u de stappen in de sectie [Uw eigen model gebruiken](#use-your-own-model).

## <a name="about-the-example-app"></a>Over de voorbeeld-app

De toepassing is een generieke Windows UWP-toepassing. De toepassing gebruikt de camera op uw Windows 10-apparaat om afbeeldingen naar het model te sturen. De tags en scores die door het model worden geretourneerd, worden weergegeven onder de videopreview.

* Zodra gegevens via de camera worden ingevoerd, wordt [MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader) gebruikt om afzonderlijke frames te extraheren. De frames worden naar het model verzonden om ze van een score te voorzien.

* Het model retourneert de tags waarmee het is getraind en een zwevende waarde die aangeeft hoe zeker het is dat de afbeelding dat item bevat.

### <a name="the-ui"></a>De gebruikersinterface

De gebruikersinterface voor de voorbeeldtoepassing wordt gemaakt met behulp van de besturingselementen __CaptureElement__ en __TextBlock__. Het element CaptureElement toont een preview van de video uit de camera en TextBlock toont de resultaten die door het model zijn geretourneerd. 

### <a name="the-model"></a>Het model

Het model (`cat-or-dog.onnx`) dat van het voorbeeld is voorzien, is gemaakt en getraind met behulp van de Custom Vision Service van Cognitive Services. Het getrainde model is vervolgens geëxporteerd als ONNX-model. Zie de documenten [Een classificatie bouwen ](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) en [Uw model exporteren om met mobiele apparaten te gebruiken](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) voor meer informatie over het gebruik van deze service.

> [!IMPORTANT]
> Het model dat van het voorbeeld is voorzien, is getraind met een kleine set met afbeeldingen van honden en katten. Dit model kan honden en katten daarom misschien niet heel erg goed herkennen.

### <a name="the-model-class-file"></a>Klassebestand van het model

Wanneer u een ONNX-bestand aan een Windows UWP-toepassing toevoegt, wordt een .cs-bestand gemaakt. Dit bestand heeft dezelfde naam als het `.onnx`-bestand (`cat-or-dog` in dit voorbeeld) en bevat de klassen die worden gebruikt om met het model uit C# te werken. De entiteiten in de gegenereerde klasse kunnen echter namen hebben zoals `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. U kunt de naam van deze items veilig wijzigen in een beschrijvende naam (klik hiervoor op de rechtermuisknop en kies Naam wijzigen).

> [!NOTE]
> De voorbeeldcode heeft de gegenereerde klasse en methodenamen geherstructureerd in het volgende:
>
> * `ModelInput`
> * `ModelOutput`
> * `Model`
> * `CreateModel`

### <a name="camera-access"></a>Cameratoegang

Het tabblad __Mogelijkheden__ in het `Package.appxmanifest`-bestand wordt geconfigureerd zodat toegang tot de webcam en microfoon mogelijk is.

> [!NOTE]
> Hoewel in dit voorbeeld geen audio wordt gebruikt, moest ik de microfoon inschakelen voordat ik toegang kon krijgen tot de camera op mijn apparaat.

De toepassing probeert de camera aan de achterkant van uw apparaat op te halen als uw apparaat deze heeft. De toepassing gebruikt de klasse [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) om video op te nemen met de camera. [MediaFrameReader](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) wordt gebruikt om videoframes vast te leggen en naar het model te verzenden.

## <a name="get-the-example-code"></a>De voorbeeldcode halen

De voorbeeldtoepassing is beschikbaar op [https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP](https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP).

## <a name="run-the-example"></a>Het voorbeeld uitvoeren

1. Gebruik de `F5`-sleutel om de toepassing te starten vanuit Visual Studio. U wordt mogelijk gevraagd de Ontwikkelmodus in te schakelen. Zie het document [Ontwikkeling voor uw apparaat inschakelen](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) voor meer informatie.

2. Wanneer u hierom wordt gevraagd, geeft u de toepassing toestemming om de camera en microfoon op uw apparaat te openen.

3. Richt de camera op een hond of kat. De score voor of de afbeelding een hond of kat bevat, wordt weergegeven onder de preview in de toepassing.

    > [!TIP]
    > Als u geen hond of kat in de buurt hebt, kunt u een foto van een hond of kat gebruiken.

## <a name="use-your-own-model"></a>Uw eigen model gebruiken

Als u uw eigen model wilt gebruiken, volgt u de volgende stappen:

> [!IMPORTANT]
> Aan de hand van de stappen in deze sectie wijzigt u de naam van het huidige model (cat-or-dog.cs) en herstructureert u de namen van de klasse en de methode van het nieuwe model. Hiermee voorkomt u naamconflicten met het voorbeeldmodel.

1. Train een model met behulp van de Custom Vision Service. Zie het document [Een classificatie bouwen](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) voor informatie over het trainen van een model.

2. Exporteer het getrainde model als ONNX-model. Zie het document [Uw model exporteren om met mobiele apparaten te gebruiken](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) voor informatie over het exporteren van een model.

3. In Solution Explorer klikt u met de rechtermuisknop op __cat-or-dog.cs__ en wijzigt u de naam in __cat-or-dog.txt__. Door de naam te wijzigen, voorkomt u naamconflicten met het nieuwe model.

    > [!TIP]
    > U kunt ook verschillende namen voor de klassenamen in het nieuwe model gebruiken, maar het is gemakkelijker om de bestaande namen opnieuw te gebruiken.

4. In Solution Explorer klikt u met de rechtermuisknop op het item __VisionApp__ en selecteert u vervolgens __Toevoegen__ > __Bestaand item...__.

5. Als u een klasse voor het model wilt genereren, selecteert u het ONNX-bestand dat u wilt importeren en vervolgens selecteert u de knop __Toevoegen__. Er wordt een nieuwe klasse toegevoegd met dezelfde naam als het ONNX-bestand (maar dan met de extensie `.cs`) in Solution Explorer.

6. Open het gegenereerde .cs-bestand en zoek de namen van de volgende items:

    > [!IMPORTANT]
    > Gebruik het `cat-or-dog.txt`-voorbeeldbestand als richtlijn voor het herkennen van de klassen en functies.

    * De klasse die de modelinvoer genereert. De gegenereerde naam is vergelijkbaar met `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Wijzig de naam van deze klasse in __ModelInput__.
    * De klasse de die modeluitvoer definieert. De gegenereerde naam is vergelijkbaar met `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelOutput`. Wijzig de naam van deze klasse in __ModelOutput__.
    * De klasse die het model definieert. De gegenereerde naam is vergelijkbaar met `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Wijzig de naam van deze klasse in __Model__.
    * De methode waarmee het model wordt gemaakt. De gegenereerde naam is vergelijkbaar met `Create_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Wijzig de naam van deze methode in __CreateModel__.

7. In Solution Explorer verplaatst u het `.onnx`-bestand naar de map __Assets__. 

8. Als u het ONNX-bestand in het toepassingspakket wilt insluiten, selecteert u het `.onnx`-bestand en stelt u __Actie bouwen__ in de eigenschappen in op __Inhoud__.

9. Open het bestand __MainPage.xaml.cs__. Zoek de volgende regel en wijzig de bestandsnaam in het nieuwe `.onnx`-bestand:

    ```csharp
    var file = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/cat-or-dog.onnx"));
    ```

    Met deze wijziging wordt het nieuwe model in runtime geladen.

10. Bouw de app en voer deze uit. Voor het scoren van afbeeldingen wordt nu het nieuwe model gebruikt.

## <a name="next-steps"></a>Volgende stappen

Als u andere manieren wilt ontdekken om een Custom Vision-model te exporteren en te gebruiken, raadpleegt u de volgende documenten:

* [Uw model exporteren](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Geëxporteerd Tensorflow-model in een Android-toepassing gebruiken](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)
* [Geëxporteerd CoreML-model in een Swift iOS-toepassing gebruiken](https://go.microsoft.com/fwlink/?linkid=857726)
* [Geëxporteerd CoreML-model in een iOS-toepassing gebruiken met Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)

Voor meer informatie over het gebruik van ONNX-modellen met Windows ML raadpleegt u het document [Een model in uw app integreren met Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/integrate-model).
