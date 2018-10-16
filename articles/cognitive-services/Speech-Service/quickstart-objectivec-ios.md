---
title: 'Snelstartgids: Spraakherkenning in Objective-C op iOS op basis van de Cognitive Services Speech SDK herkennen'
titleSuffix: Microsoft Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in Objective-C op iOS op basis van de Cognitive Services spraak-SDK
services: cognitive-services
author: chlandsi
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 10/12/2018
ms.author: chlandsi
ms.openlocfilehash: ce9979d8d300f2308a4b7a22791c242409f2c988
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341171"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-using-the-cognitive-services-speech-sdk"></a>Snelstartgids: Spraakherkenning in Objective-C op iOS op basis van de Cognitive Services Speech SDK herkennen

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u hoe u een iOS-app maken in Objective-C met behulp van de Cognitive Services Speech SDK moet een geluidsbestand met opgenomen spraak naar tekst te transcriberen.

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [de spraakservice gratis uitproberen](get-started.md).
* Een Mac met Xcode 9.4.1 geïnstalleerd als iOS-ontwikkelomgeving. In deze zelfstudie is bedoeld voor iOS versie 11.4. Als u geen Xcode hebt nog, kunt u het installeren van de [App Store](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12).

## <a name="get-the-speech-sdk-for-ios"></a>De spraak-SDK voor iOS ophalen

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Speech SDK van Cognitive Services is `1.0.1`.

De Cognitive Services spraak-SDK voor Mac- en iOS wordt gedistribueerd als een Cocoa-Framework.
Deze kan worden gedownload vanaf https://aka.ms/csspeech/iosbinary. Download het bestand naar uw basismap.

## <a name="create-an-xcode-project"></a>Een Xcode-Project maken 

Xcode Start en een nieuw project starten door te klikken op **bestand** > **nieuw** > **Project**.
Kies in het dialoogvenster voor het selecteren van sjabloon de ' iOS Single View App "sjabloon.

In de dialoogvensters die volgen, de volgende selecties maken:

1. Het dialoogvenster Opties project
    1. Voer een naam op voor de Quick Start-app, bijvoorbeeld `helloworld`.
    1. Voer een naam van de juiste organisatie en de organisatie-id, als u al een Apple developer-account hebt. Voor testdoeleinden kunt u elke naam zijn, zoals Kies `testorg`. Als u wilt de app te ondertekenen, moet u ook een goede inrichtingsprofiel. Raadpleeg de [Apple developer-site](https://developer.apple.com/) voor meer informatie.
    1. Zorg ervoor dat de dat objective-C is gekozen als de taal voor het project.
    1. Schakel alle selectievakjes in voor tests en de belangrijkste gegevens.
    ![Instellingen voor het project](media/sdk/qs-objectivec-project-settings.png)
1. Selecteer projectmap
    1. Kies uw basismap op het project worden geplaatst. Hiermee maakt u een `helloworld` map in de basismap die de bestanden voor de Xcode-project bevat.
    1. Het maken van een Git-opslagplaats voor dit voorbeeldproject uitschakelen.
    1. Aanpassen van de paden naar de SDK in de *projectinstellingen*.
        1. In de **algemene** tabblad onder de **ingesloten binaire bestanden** header, de SDK-bibliotheek toevoegen als een framework: **toevoegen ingesloten binaire bestanden** > **toevoegen ...**  > Navigeren naar uw basismap en kiest u het bestand `MicrosoftCognitiveServicesSpeech.framework`. Hierdoor wordt de SDK-bibliotheek ook automatisch toegevoegd aan de koptekst **gekoppelde Framework en bibliotheken**.
        ![Toegevoegde Framework](media/sdk/qs-objectivec-framework.png)
        1. Ga naar de **Build Settings** tabblad en activeren **alle** instellingen.
        1. De map toe te voegen `$(SRCROOT)/..` naar de *zoekpaden kader* onder de **zoekpaden** kop.
        ![Framework zoekpad instelling](media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Instellen van de gebruikersinterface

De voorbeeldapp heeft een zeer eenvoudige gebruikersinterface: twee knoppen spraakherkenning starten van bestand of van de microfoon invoer- en een tekstlabel om het resultaat weer te geven.
De gebruikersinterface is ingesteld in de `Main.storyboard` deel uitmaakt van het project.
De XML-weergave van de hoofd-storyboard openen met de rechtermuisknop op de `Main.storyboard` vermelding van het project structuur en het selecteren van **openen als...**   >  **Broncode**.
Vervang de automatisch gegenereerde XML door dit:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Download de [wav-voorbeeldbestand](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) door met de rechtermuisknop op de koppeling en het kiezen van **doel opslaan als...** . De wav-bestand toevoegen aan het project als een resource door deze in een venster Zoeken in de hoofdmap van de Project-weergave te slepen.
Klik op **voltooien** in het volgende dialoogvenster zonder de instellingen te wijzigen.
1. Vervang de inhoud van de automatisch gegenereerde `ViewController.m` door het bestand:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. Vervang de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.
1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).
1. De aanvraag voor toegang tot de microfoon toevoegen. Klik met de rechtermuisknop de `Info.plist` vermelding van de projectstructuur van het en selecteer **openen als...**   >  **Broncode**. Voeg de volgende regels in de `<dict>` sectie en sla het bestand.
    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="building-and-running-the-sample"></a>Het bouwen en uitvoeren van het voorbeeld

1. De foutopsporingsuitvoer zichtbaar maken (**weergave** > **fouten opsporen in gebied** > **activeren Console**).
1. Kies de iOS-simulator of een iOS-apparaat dat is verbonden met uw ontwikkelcomputer als bestemming voor de app uit de lijst in de **Product** -> **bestemming** menu.
1. Ontwikkelen en uitvoeren van de voorbeeldcode in de iOS-simulator door het selecteren van **Product** -> **uitvoeren** in het menu of klikken op de **afspelen** knop.
De spraak-SDK ondersteunt momenteel alleen 64-bits iOS-platforms.
1. Nadat u op de knop 'Herkennen (bestand)' in de app klikt, ziet u de inhoud van het audiobestand 'Wat is het weer, zoals?' in het onderste gedeelte van het scherm.

 ![Gesimuleerde iOS-App](media/sdk/qs-objectivec-simulated-app.png)

1. Nadat u op de knop 'Herkennen (microfoon)' in de app en stel een paar woorden dat, ziet u de tekst die u hebt gesproken in het onderste gedeelte van het scherm.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek naar dit voorbeeld in de map `quickstart/objectivec-ios`.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Onze voorbeelden ophalen](speech-sdk.md#get-the-samples)

