---
title: 'Quickstart: Spraak, doel-C-spraak service herkennen'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het herkennen van spraak in doel-C op macOS met behulp van de Speech SDK
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 45cd1210ee6af3c456171a427729f6e16caf2d58
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559364"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>Quickstart: Spraak herkennen in doel-C op macOS met behulp van de Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u hoe u een macOS-app kunt maken in doel-C met behulp van de SDK van Cognitive Services speech om spraak opnamen van een microfoon naar tekst te transcriberen.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, bekijkt u de lijst vereisten:

* Een [abonnementssleutel](get-started.md) voor Speech Service
* Een macOS-computer met [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) of hoger en MacOS 10,13 of hoger

## <a name="get-the-speech-sdk-for-macos"></a>De Speech SDK voor macOS ophalen

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Speech SDK van Cognitive Services is `1.6.0`.

De Cognitive Services Speech SDK voor Mac wordt gedistribueerd als een framework-bundel.
Het kan worden gebruikt in Xcode-projecten als een [CocoaPod](https://cocoapods.org/), of worden https://aka.ms/csspeech/macosbinary gedownload en hand matig worden gekoppeld. Deze hand leiding maakt gebruik van een CocoaPod.

## <a name="create-an-xcode-project"></a>Een Xcode-project maken

Start Xcode en start een nieuw project starten door te klikken op **File** > **New** > **Project**.
Kies in het dialoog venster sjabloon selectie de sjabloon ' cacao-app '.

Maak in de dialoogvensters die volgen de volgende selecties:

1. Het dialoogvenster Project Options
    1. Voer een naam in voor de snelstart-app, bijvoorbeeld `helloworld`.
    1. Voer de juiste organisatie naam en een organisatie-id in als u al een Apple-ontwikkelaars account hebt. Voor testdoeleinden kunt u elke naam kiezen, zoals `testorg`. Om de app te kunnen ondertekenen, hebt u een geschikt inrichtingsprofiel nodig. Raadpleeg de [Apple-site voor ontwikkelaars](https://developer.apple.com/) voor meer informatie.
    1. Zorg ervoor dat Objective-C als de taal voor het project is gekozen.
    1. Schakel de selectie vakjes uit om Story boards te gebruiken en om een op documenten gebaseerde toepassing te maken. De eenvoudige gebruikers interface voor de voor beeld-app wordt programmatisch gemaakt.
    1. Schakel alle selectievakjes voor tests en essentiële gegevens uit.
    ![Projectinstellingen](media/sdk/qs-objectivec-macos-project-settings.png)
1. Projectmap selecteren
    1. Kies een map waarin u het project wilt plaatsen. Hiermee maakt u een `helloworld`-map in uw basismap die alle bestanden voor het Xcode-project bevat.
    1. Schakel het maken van een Git-opslagplaats uit voor dit voorbeeldproject.
1. Stel de rechten in voor toegang tot het netwerk en de microfoon. Klik op de naam van de app in de eerste regel van het overzicht aan de linkerkant om naar de app-configuratie te gaan en kies vervolgens het tabblad mogelijkheden.
    1. Schakel de instelling app sandbox in voor de app.
    1. Schakel de selectie vakjes voor uitgaande verbindingen en microfoon toegang in.
    ![Sandbox-instellingen](media/sdk/qs-objectivec-macos-sandbox.png)
1. De app moet ook het gebruik van de microfoon in het `Info.plist` bestand declareren. Klik op het bestand in het overzicht en voeg de sleutel ' beschrijving privacy-microfoon gebruik ' toe, met een waarde als ' microfoon is vereist voor spraak herkenning '.
    ![Instellingen in info. plist](media/sdk/qs-objectivec-macos-info-plist.png)
1. Sluit het project Xcode. Later gebruikt u een ander exemplaar na het instellen van de CocoaPods.

## <a name="install-the-sdk-as-a-cocoapod"></a>De SDK installeren als een CocoaPod

1. Installeer de CocoaPod dependency manager zoals beschreven in de [installatie-instructies](https://guides.cocoapods.org/using/getting-started.html).
1. Navigeer naar de map van uw voor beeld-`helloworld`app (). Plaats een tekst bestand met de naam `Podfile` en de volgende inhoud in die map:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/Podfile)]
1. Ga naar de `helloworld` map in een Terminal en voer de opdracht `pod install`uit. Hiermee wordt een `helloworld.xcworkspace` Xcode-werk ruimte gegenereerd met zowel de voor beeld-app als de spraak-SDK als een afhankelijkheid. Deze werk ruimte wordt gebruikt in het volgende.

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Open de `helloworld.xcworkspace` werk ruimte in Xcode.
1. Vervang de inhoud van het automatisch gegenereerde bestand `AppDelegate.m` door:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/helloworld/AppDelegate.m#code)]
1. Vervang de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.
1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Maak de foutopsporingsuitvoer zichtbaar (**View** > **Debug Area** > **Activate Console**).
1. Bouw de voorbeeld code en voer deze uit door in het menu **product** > **uitvoeren** te selecteren of te klikken op de knop **afspelen** .
1. Nadat u op de knop hebt geklikt en enkele woorden hebt gedicteerd, ziet u de tekst die u in het onderste gedeelte van het scherm hebt gesp roken. Wanneer u de app voor de eerste keer uitvoert, wordt u gevraagd om de app toegang te geven tot de microfoon van uw computer.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Objective-C-voorbeelden op GitHub verkennen](https://aka.ms/csspeech/samples)
