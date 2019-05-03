---
title: 'Snelstart: Spraak herkennen, Objective-C - Speech Services'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in Objective-C in macOS met behulp van de spraak-SDK
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/03/2019
ms.author: chlandsi
ms.openlocfilehash: f25c3801553b0ac0c725170cda95f5c1eacc3637
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65020746"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>Quickstart: Het herkennen van gesproken tekst in Objective-C in macOS met behulp van de spraak-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u hoe u een macOS-app maken in Objective-C met behulp van de Cognitive Services Speech SDK spraak geregistreerd via een microfoon om tekst te transcriberen.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, bekijkt u de lijst vereisten:

* Een [abonnementssleutel](get-started.md) voor Speech Service
* Een macOS-computer met [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) of hoger en macOS 10.13 of hoger

## <a name="get-the-speech-sdk-for-macos"></a>De spraak-SDK ophalen voor macOS

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Speech SDK van Cognitive Services is `1.5.0`.

De Cognitive Services spraak-SDK voor Mac wordt gedistribueerd als een bundel framework.
Het kan worden gebruikt in Xcode-projecten als een [CocoaPod](https://cocoapods.org/), of die zijn gedownload van https://aka.ms/csspeech/macosbinary en handmatig worden gekoppeld. Deze handleiding wordt een CocoaPod gebruikt.

## <a name="create-an-xcode-project"></a>Een Xcode-project maken

Start Xcode en start een nieuw project starten door te klikken op **File** > **New** > **Project**.
Kies de sjabloon 'Cocoa App' in het dialoogvenster voor het selecteren van sjabloon.

Maak in de dialoogvensters die volgen de volgende selecties:

1. Het dialoogvenster Project Options
    1. Voer een naam in voor de snelstart-app, bijvoorbeeld `helloworld`.
    1. Voer de naam van de juiste organisatie en een organisatie-id als u al een Apple developer-account hebt. Voor testdoeleinden kunt u elke naam kiezen, zoals `testorg`. Om de app te kunnen ondertekenen, hebt u een geschikt inrichtingsprofiel nodig. Raadpleeg de [Apple-site voor ontwikkelaars](https://developer.apple.com/) voor meer informatie.
    1. Zorg ervoor dat Objective-C als de taal voor het project is gekozen.
    1. Schakel de selectievakjes om te gebruiken van communicatie en een toepassing op basis van een document maken. De eenvoudige gebruikersinterface voor de voorbeeld-app via een programma gemaakt.
    1. Schakel alle selectievakjes voor tests en essentiële gegevens uit.
    ![Projectinstellingen](media/sdk/qs-objectivec-macos-project-settings.png)
1. Projectmap selecteren
    1. Kies een map op het project worden geplaatst. Hiermee maakt u een `helloworld`-map in uw basismap die alle bestanden voor het Xcode-project bevat.
    1. Schakel het maken van een Git-opslagplaats uit voor dit voorbeeldproject.
1. De rechten voor toegang tot netwerk en microfoon instellen. Klik op de naam van de app in de eerste regel in het overzicht aan de linkerkant om te gaan naar de app-configuratie en kies vervolgens het tabblad 'Mogelijkheden'.
    1. Schakel de instelling "App sandbox" voor de app.
    1. Schakel de selectievakjes in voor toegang tot "Uitgaande verbindingen" en 'Microfoon'.
    ![Sandbox-instellingen](media/sdk/qs-objectivec-macos-sandbox.png)
1. De app moet ook gebruik van de microfoon in declareert de `Info.plist` bestand. Klik op het bestand in het overzicht en de sleutel 'Privacy--microfoon gebruik Description', met een waarde zoals 'Microfoon is nodig voor spraakherkenning' toevoegen.
    ![Instellingen in Info.plist](media/sdk/qs-objectivec-macos-info-plist.png)
1. Sluit de Xcode-project. U gebruikt een ander exemplaar van deze later na het instellen van de CocoaPods.

## <a name="install-the-sdk-as-a-cocoapod"></a>De SDK installeren als een CocoaPod

1. De afhankelijkheidsbeheerder CocoaPod installeren zoals beschreven in de [installatie-instructies](https://guides.cocoapods.org/using/getting-started.html).
1. Ga naar de map van uw voorbeeld-app (`helloworld`). Plaats een tekstbestand met de naam van de `Podfile` en de volgende inhoud in die map:
    ```
    target 'helloworld' do
        platform :osx, '10.13'
        pod 'MicrosoftCognitiveServicesSpeech-macOS', '~> 1.5.0'
    end
    ```
1. Navigeer naar de `helloworld` map in een terminal en voer de opdracht `pod install`. Dit genereert een `helloworld.xcworkspace` Xcode-werkruimte met zowel de voorbeeld-app en de SDK spraak als een afhankelijkheid. Deze werkruimte wordt gebruikt in het volgende.

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Open de `helloworld.xcworkspace` werkruimte in Xcode.
1. Vervang de inhoud van de automatisch gegenereerde `AppDelegate.m` door het bestand: [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/helloworld/AppDelegate.m#code)]
1. Vervang de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.
1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Maak de foutopsporingsuitvoer zichtbaar (**View** > **Debug Area** > **Activate Console**).
1. Bouwen en uitvoeren van de voorbeeldcode hiervoor **Product** -> **uitvoeren** in het menu of klikken op de **afspelen** knop.
1. Nadat u op de knop en stel een paar woorden dat, ziet u de tekst die u hebt gesproken in het onderste gedeelte van het scherm. Wanneer u de app voor de eerste keer uitvoert, moet u gevraagd de apptoegang geven tot de microfoon van uw computer.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Objective-C-voorbeelden op GitHub verkennen](https://aka.ms/csspeech/samples)

