---
title: Aangepaste visie ONNX model met Windows ML - cognitieve Services | Microsoft Docs
description: Informatie over het maken van een Windows-UWP-app die gebruikmaakt van een model ONNX is geëxporteerd uit cognitieve Services.
services: cognitive-services
author: larryfr
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: larryfr
ms.openlocfilehash: 0b128ba1800e74c20c09a9c5711c8473f1dd00d0
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939457"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Zelfstudie: Een model ONNX uit aangepaste visie gebruiken met Windows ML (preview)

Informatie over het gebruik van een model ONNX is geëxporteerd uit de service aangepaste visie met Windows ML (preview).

De informatie in dit document wordt beschreven hoe een ONNX-bestand dat is geëxporteerd uit de Service van de visie aangepast met Windows ML wilt gebruiken. Een voorbeeld Windows UWP-toepassing wordt verstrekt. Een getraind model dat honden en katten herkennen kan is opgenomen in het voorbeeld. Stappen vindt u ook op hoe u uw eigen model met dit voorbeeld kunt.

> [!div class="checklist"]
> * Over de voorbeeld-app
> * De voorbeeldcode halen
> * Het voorbeeld uitvoert
> * Uw eigen model gebruiken

## <a name="prerequisites"></a>Vereisten

* Een Windows 10-apparaat met:

    * Een camera.

    * Visual Studio 2017 versie 15.7 of hoger, waarop de __ontwikkeling van universele Windows-Platform__ werkbelasting ingeschakeld.

    * Developer-modus is ingeschakeld. Zie voor meer informatie de [inschakelen van uw apparaat voor het ontwikkelen van](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) document.

* (Optioneel) Een ONNX-bestand dat is geëxporteerd uit de visie van aangepaste service. Zie voor meer informatie de [exporteren van het model voor gebruik met een mobiel apparaat](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) document.

    > [!NOTE]
    > Voor het gebruik van uw eigen model, volg de stappen in de [gebruik van uw eigen model](#use-your-own-model) sectie.

## <a name="about-the-example-app"></a>Over de voorbeeld-app

De toepassing is een algemene Windows UWP-toepassing. Dit maakt gebruik van de camera op uw Windows 10-apparaat op te geven van installatiekopieën van het model. De labels en scores geretourneerd door het model worden weergegeven onder de video preview.

* Als de gegevens afkomstig via de camera [MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader) wordt gebruikt voor het uitpakken van afzonderlijke frames. De frames worden verzonden naar het model voor score berekenen.

* Het model retourneert de labels die het op is getraind en een float-waarde die aangeeft hoe vertrouwen is dat de installatiekopie van het item bevat.

### <a name="the-ui"></a>De gebruikersinterface

De gebruikersinterface voor de voorbeeldtoepassing wordt gemaakt met __CaptureElement__ en __TextBlock__ besturingselementen. De CaptureElement geeft een voorbeeld van de video van de camera en de TextBlock geeft de resultaten uit het model. 

### <a name="the-model"></a>Het model

Het model (`cat-or-dog.onnx`) meegeleverd met het voorbeeld is gemaakt en getraind met behulp van de visie van de aangepaste cognitieve Services-service. Het getrainde model is wordt geëxporteerd als een ONNX-model. Zie voor meer informatie over het gebruik van deze service de [het bouwen van een classificatie](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) en [exporteren van het model voor gebruik met een mobiel apparaat](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) documenten.

> [!IMPORTANT]
> Het opgegeven met het voorbeeld model is getraind met een kleine set aquaduct en cat-installatiekopieën. Zodat deze mogelijk niet de wereld beste op honden en katten herkent.

### <a name="the-model-class-file"></a>Het modelbestand voor klasse

Wanneer u een ONNX-bestand naar een Windows-UWP-toepassing toevoegt, maakt deze een bestand .cs. Dit bestand heeft dezelfde naam als de `.onnx` bestand (`cat-or-dog` in dit voorbeeld) en bevat de klassen gebruikt om te werken met het model van C#. De entiteiten in de gegenereerde klasse kunnen echter hebben namen als `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. U kunt deze vermeldingen veilig wijzigen (met de rechtermuisknop op, wijzig de naam) naar een beschrijvende naam.

> [!NOTE]
> De voorbeeldcode heeft de gegenereerde namen voor de klasse en een methode met de volgende geherstructureerd:
>
> * `ModelInput`
> * `ModelOutput`
> * `Model`
> * `CreateModel`

### <a name="camera-access"></a>Camera-toegang

De __mogelijkheden__ tabblad de `Package.appxmanifest` -bestand is geconfigureerd voor toegang tot de webcam en microfoon.

> [!NOTE]
> Hoewel dit voorbeeld geen audio gebruik, had ik de microfoon inschakelen voordat ik heb toegang tot de camera op mijn apparaat.

De toepassing probeert op te halen van de camera op de achterkant van het apparaat als deze beschikbaar is. Dit maakt gebruik van de [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) klasse vastleggen vanaf de camera gestart. [MediaFrameReader](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) wordt gebruikt voor het vastleggen van video frames en ze verzenden naar het model.

## <a name="get-the-example-code"></a>De voorbeeldcode halen

De voorbeeldtoepassing is beschikbaar op [ https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP ](https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP).

## <a name="run-the-example"></a>Het voorbeeld uitvoert

1. Gebruik de `F5` sleutel voor de toepassing niet starten vanuit Visual Studio. U wordt mogelijk gevraagd om in te schakelen van modus voor ontwikkelaars. Zie voor meer informatie de [inschakelen van uw apparaat voor het ontwikkelen van](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) document.

2. Als u wordt gevraagd, kunt u de toepassing toegang tot de camera en microfoon op uw apparaat.

3. De camera op een aquaduct of cat punt. De score voor of de afbeelding een aquaduct of cat bevat wordt weergegeven onder het voorbeeld in de toepassing.

    > [!TIP]
    > Als u een aquaduct of cat handige niet hebt, kunt u een foto van een aquaduct of cat.

## <a name="use-your-own-model"></a>Uw eigen model gebruiken

Gebruik de volgende stappen voor het gebruik van uw eigen model:

> [!IMPORTANT]
> De stappen in deze sectie Wijzig de naam van het huidige model (cat-of-dog.cs) en de namen van de klasse en de methode van het nieuwe model opsplitsen. Dit is om te voorkomen dat naamconflicten met het voorbeeldmodel.

1. Het gebruik van de aangepaste visie service model trainen. Zie voor meer informatie over hoe het model trainen aan de [het bouwen van een classificatie](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier).

2. Exporteer het getrainde model als een ONNX-model. Zie voor informatie over het exporteren van een model, de [exporteren van het model voor gebruik met een mobiel apparaat](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) document.

3. Klik in Solution Explorer met de rechtermuisknop op de __cat of dog.cs__ en wijzigt u de naam __cat of dog.txt__. Naam van het voorkomt naamconflicten met het nieuwe model.

    > [!TIP]
    > U kunt ook verschillende namen voor de klassenamen van de in het nieuwe model, maar de namen van bestaande hergebruiken is eenvoudiger.

4. Klik in Solution Explorer met de rechtermuisknop op de __VisionApp__ vermelding en selecteer vervolgens __toevoegen__ > __bestaand item...__ .

5. Voor het genereren van een klasse voor het model, selecteert u het bestand ONNX om te importeren en selecteer vervolgens de __toevoegen__ knop. Een nieuwe klasse met dezelfde naam als het bestand ONNX (maar er is een `.cs` extensie) is toegevoegd in Solution Explorer.

6. Open het bestand gegenereerde .cs en zoekt u de namen van de volgende items:

    > [!IMPORTANT]
    > Gebruik het voorbeeld `cat-or-dog.txt` bestand als een handleiding voor het herkennen van de klassen en -functies.

    * De klasse die het model invoer definieert. Het is mogelijk dat de gegenereerde naam vergelijkbaar met `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Wijzig de naam van deze klasse een __ModelInput__.
    * De klasse die definieert de uitvoer van het model. Het is mogelijk dat de gegenereerde naam vergelijkbaar met `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelOutput`. Wijzig de naam van deze klasse een __ModelOutput__.
    * De klasse die het model definieert. Het is mogelijk dat de gegenereerde naam vergelijkbaar met `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Wijzig de naam van deze klasse een __Model__.
    * De methode die wordt gemaakt van het model. Het is mogelijk dat de gegenereerde naam vergelijkbaar met `Create_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Wijzig de naam van deze methode kan worden __CreateModel__.

7. Klik in Solution Explorer, gaan de `.onnx` bestand naar de __activa__ map. 

8. Om het bestand ONNX opnemen in het toepassingspakket, selecteer de `.onnx` bestands- en stel __Build Action__ naar __inhoud__ in de eigenschappen.

9. Open de __MainPage.xaml.cs__ bestand. Zoek de volgende regel en wijzig de bestandsnaam aan de nieuwe `.onnx` bestand:

    ```csharp
    var file = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/cat-or-dog.onnx"));
    ```

    Deze wijziging wordt het nieuwe model tijdens runtime geladen.

10. Ontwikkel en voer de app. Het nieuwe model wordt nu gebruikt voor de beoordeling van afbeeldingen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor het detecteren van andere manieren om te exporteren en een aangepaste visie model:

* [Exporteren van uw model](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Geëxporteerde Tensorflow model gebruiken in een Android-toepassing](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)
* [Geëxporteerde CoreML model gebruiken in een Swift iOS-toepassing](https://go.microsoft.com/fwlink/?linkid=857726)
* [Gebruik geëxporteerd CoreML model in een toepassing met Xamarin voor iOS](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)

Zie voor meer informatie over het gebruik van ONNX modellen met Windows ML de [een model integreren in uw app voor Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/integrate-model) document.
