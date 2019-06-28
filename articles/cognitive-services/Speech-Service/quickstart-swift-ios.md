---
title: 'Quickstart: Herkent-spraak, Swift - spraakservices'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in Swift op iOS op basis van de spraak-SDK
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/26/2019
ms.author: chlandsi
ms.openlocfilehash: feebca23e38596cff67e07f64e7acf1f7b6252e9
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467430"
---
# <a name="quickstart-recognize-speech-in-swift-on-ios-using-the-speech-sdk"></a>Quickstart: Spraak in Swift herkennen in iOS met de spraak-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u hoe u een iOS-app maken in Swift met behulp van de Cognitive Services Speech SDK spraak geregistreerd via een microfoon om tekst te transcriberen.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, bekijkt u de lijst vereisten:

* Een [abonnementssleutel](get-started.md) voor de Speech-Service.
* Een macOS-computer met [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) of hoger en [CocoaPods](https://cocoapods.org/) geïnstalleerd.

## <a name="get-the-speech-sdk-for-ios"></a>De Speech-SDK voor iOS ophalen

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Speech SDK van Cognitive Services is `1.6.0`. Houd er rekening mee dat in deze zelfstudie niet zonder wijzigingen voor een eerdere versie van de SDK werkt.

De Cognitive Services spraak-SDK voor iOS wordt gedistribueerd als een bundel framework.
Het kan worden gebruikt in Xcode-projecten als een [CocoaPod](https://cocoapods.org/), of die zijn gedownload van https://aka.ms/csspeech/macosbinary en handmatig worden gekoppeld. Deze handleiding wordt een CocoaPod gebruikt.

## <a name="create-an-xcode-project"></a>Een Xcode-project maken

Start Xcode en start een nieuw project starten door te klikken op **File** > **New** > **Project**.
Kies in het dialoogvenster voor het selecteren van sjablonen de sjabloon 'iOS Single View App'.

Maak in de dialoogvensters die volgen de volgende selecties:

1. Het dialoogvenster Project Options
    1. Voer een naam in voor de snelstart-app, bijvoorbeeld `helloworld`.
    1. Voer de naam van de juiste organisatie en een organisatie-id als u al een Apple developer-account hebt. Voor testdoeleinden kunt u elke naam kiezen, zoals `testorg`. Om de app te kunnen ondertekenen, hebt u een geschikt inrichtingsprofiel nodig. Raadpleeg de [Apple-site voor ontwikkelaars](https://developer.apple.com/) voor meer informatie.
    1. Zorg ervoor dat Swift is gekozen als de taal voor het project.
    1. Schakel de selectievakjes om te gebruiken van communicatie en een toepassing op basis van een document maken. De eenvoudige gebruikersinterface voor de voorbeeld-app via een programma gemaakt.
    1. Schakel alle selectievakjes voor tests en essentiële gegevens uit.
1. Projectmap selecteren
    1. Kies een map op het project worden geplaatst. Hiermee maakt u een `helloworld` map in de gekozen map waarin de bestanden voor de Xcode-project.
    1. Schakel het maken van een Git-opslagplaats uit voor dit voorbeeldproject.
1. De app moet ook gebruik van de microfoon in declareert de `Info.plist` bestand. Klik op het bestand in het overzicht en de sleutel 'Privacy--microfoon gebruik Description', met een waarde zoals 'Microfoon is nodig voor spraakherkenning' toevoegen.
    ![Instellingen in Info.plist](media/sdk/qs-swift-ios-info-plist.png)
1. Sluit de Xcode-project. U gebruikt een ander exemplaar van deze later na het instellen van de CocoaPods.

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Plaats een nieuw headerbestand met de naam van de `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` in de `helloworld` binnen de helloworld-project en plak de volgende code in deze map: [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]
1. Het relatieve pad toevoegen `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` project aan de bridging header de SWIFT-instellingen voor de helloworld-doel in de *Objective-C Bridging Header* veld ![eigenschappen van koptekst](media/sdk/qs-swift-ios-bridging-header.png)
1. Vervang de inhoud van de automatisch gegenereerde `AppDelegate.swift` door het bestand: [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/AppDelegate.swift#code)]
1. Vervang de inhoud van de automatisch gegenereerde `ViewController.swift` door het bestand: [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/ViewController.swift#code)]
1. In `ViewController.swift`, vervang de tekenreeks `YourSubscriptionKey` met de abonnementssleutel van uw.
1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="install-the-sdk-as-a-cocoapod"></a>De SDK installeren als een CocoaPod

1. De afhankelijkheidsbeheerder CocoaPod installeren zoals beschreven in de [installatie-instructies](https://guides.cocoapods.org/using/getting-started.html).
1. Ga naar de map van uw voorbeeld-app (`helloworld`). Plaats een tekstbestand met de naam van de `Podfile` en de volgende inhoud in die map: [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/Podfile)]
1. Navigeer naar de `helloworld` map in een terminal en voer de opdracht `pod install`. Dit genereert een `helloworld.xcworkspace` Xcode-werkruimte met zowel de voorbeeld-app en de SDK spraak als een afhankelijkheid. Deze werkruimte wordt gebruikt in het volgende.

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Open de `helloworld.xcworkspace` werkruimte in Xcode.
1. Maak de foutopsporingsuitvoer zichtbaar (**View** > **Debug Area** > **Activate Console**).
1. Kies de iOS-simulator of een iOS-apparaat dat is verbonden met uw ontwikkelcomputer als bestemming voor de app uit de lijst in de **Product** > **bestemming** menu.
1. Bouw de voorbeeldcode en voer deze uit in de iOS-simulator door **Product** > **Run** in het menu te selecteren of op de knop **Play** te klikken.
1. Nadat u op de knop 'Herkennen' in de app en stel een paar woorden dat, ziet u de tekst die u hebt gesproken in het onderste gedeelte van het scherm.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Onze C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

