---
title: Maken van een IoT DevKit translator met behulp van Azure Functions en Cognitive Services | Microsoft Docs
description: Gebruik van de microfoon op een IoT DevKit een voicemailbericht ontvangen en vervolgens met Azure Cognitive Services voor het verwerken van deze in de vertaalde tekst in het Engels
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/28/2018
ms.author: liydu
ms.openlocfilehash: acfff95afacfa1e5c75a799ba84d64cfa0579f66
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592088"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>IoT DevKit AZ3166 met Azure Functions en Cognitive Services gebruiken om te maken van een vertaler

In dit artikel leert u hoe u IoT DevKit als een conversieprogramma voor taal met behulp van [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/). Deze records van uw stem en wordt omgezet in het Engelse tekst die wordt weergegeven op het scherm DevKit.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een compatibele alles-in-een Arduino-bord met uitgebreide randapparatuur en sensoren. U kunt ontwikkelen via [Visual Studio Code-extensie voor Arduino](https://aka.ms/arduino). En wordt geleverd met een groeiend [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) voor het prototype Internet of Things (IoT)-oplossingen die van Microsoft Azure-services profiteren.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) aan:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden

Een actief Azure-abonnement. Als u een hebt, kunt u registreren via een van deze twee methoden:

* Activeer een [gratis 30-daagse proefversie Microsoft Azure-account](https://azure.microsoft.com/free/)
* Claim uw [Azure-tegoed](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) bent u MSDN of Visual Studio-abonnee

## <a name="open-the-project-folder"></a>Open de projectmap

Open eerst de projectmap. 

### <a name="start-vs-code"></a>Start VS Code

- Zorg ervoor dat uw DevKit is verbonden met uw PC.

- Start VS Code.

- De DevKit verbinden met uw computer.

### <a name="open-the-arduino-examples-folder"></a>Open de map Arduino-voorbeelden

Vouw linkerkant **ARDUINO voorbeelden > Voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **DevKitTranslator**. Een nieuwe VS Code-venster wordt geopend, het weergeven van de projectmap. Als u de voorbeelden voor MXCHIP AZ3166 gedeelte niet ziet, zorg ervoor dat uw apparaat goed is aangesloten en start VS Code opnieuw.  

![Voorbeelden van IoT DevKit](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_examples.png)

U kunt het voorbeeld ook openen vanuit het opdrachtenpalet. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) om te openen de command palette, typt u **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

## <a name="provision-azure-services"></a>Azure-services inrichten

Typ in het venster van de oplossing `Ctrl+P` (Mac OS: `Cmd+P`) en voer `task cloud-provision`.

In de VS Code terminal leidt een interactieve vanaf de opdrachtregel u voor het inrichten van alle benodigde Azure-services:

![Cloud inrichten taak](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-provision.png)

## <a name="deploy-the-azure-function"></a>De Azure-functie implementeren

Gebruik `Ctrl+P` (Mac OS: `Cmd+P`) om uit te voeren `task cloud-deploy` om de Azure Functions-code te implementeren. Dit proces duurt doorgaans 2 tot en met 5 minuten om te voltooien.

![Implementatietaak voor de cloud](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-deploy.png)

Nadat de Azure-functie is geïmplementeerd, vult u het bestand azure_config.h door de naam van de functie-app. U kunt navigeren naar [Azure-portal](https://portal.azure.com/) te vinden:

![Azure-functie-app-naam worden gevonden](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-function.png)

> [!NOTE]
> Als de Azure-functie niet correct werkt, controleert u de ["error complicatie voor Azure-functie"-pagina in de veelgestelde vragen over het IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) om dit te verhelpen.

## <a name="build-and-upload-the-device-code"></a>Bouwen en de apparaatcode uploaden

1. Gebruik `Ctrl+P` (Mac OS: `Cmd+P`) om uit te voeren `task config-device-connection`.

2. De terminal u wordt gevraagd of u wilt gebruiken van de verbindingsreeks die wordt opgehaald uit `task cloud-provision` stap. U kunt ook uw eigen apparaatverbindingsreeks invoer door te selecteren **'Nieuwe maken...'**

3. De terminal vraagt u om in te voeren van de configuratiemodus. Om dit te doen, houdt u ingedrukt A, en vervolgens push- en release van de knop opnieuw instellen. Het scherm wordt weergegeven de DevKit-ID en 'Configuration'.

   ![Verificatie en uploaden van de schets Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/config-device-connection.png)

4. Na `task config-device-connection` is voltooid, klikt u op `F1` laden van VS Code-opdrachten en selecteer `Arduino: Upload`, vervolgens het controleren van VS Code wordt gestart en de Arduino uploaden schetsen.

   ![Verificatie en uploaden van de schets Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/arduino-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

## <a name="test-the-project"></a>Het project testen

Na de initialisatie van de app, volg de instructies op het scherm DevKit. De standaardtaal van de bron is Chinees.

Selecteer een andere taal voor vertaling:

1. Druk op een knop om setup te activeren.

2. Druk op de knop B aan het startscherm van alle ondersteunde talen van de bron.

3. Druk op een knop om te bevestigen van uw keuze van taal van de bron.

4. Knop B ingedrukt te houden terwijl spreken en loslaat B voor het starten van de vertaling.

5. De vertaalde tekst in het Engels wordt weergegeven op het scherm.

![Blader naar de taal selecteren](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Resultaat van de vertaling](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

Op het scherm van het resultaat vertaling kunt u het volgende doen:

- Druk op knoppen A en B om te bladeren en selecteer de taal van de bron.

- Druk op de knop B om te communiceren. Als u wilt de stem verzenden en ophalen van de vertaalde tekst, laat u de B-knop.

## <a name="how-it-works"></a>Hoe werkt het?

![mini-solution-voice-to-tweet-diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

De Arduino schets records uw stem vervolgens een HTTP post vragen om een Azure-functie te activeren. De Azure-functie roept de spraakomzetting cognitive service API voor het uitvoeren van de vertaling. Nadat de Azure-functie de vertaalde tekst haalt, verzendt deze een C2D (cloud-naar-apparaat)-bericht naar het apparaat. De vertaling wordt vervolgens weergegeven op het scherm.

## <a name="change-device-id"></a>Apparaat-ID wijzigen

De geregistreerd in Azure IoT Hub apparaat-ID is **AZ3166**. Zie voor het wijzigen van de apparaat-ID, hoe u [aanpassen van de IoT-apparaat-ID voor de DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u de [IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of contact opnemen met ons opnemen met behulp van de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over het gebruik van de IoT DevKit als een translator met behulp van Azure Functions en Cognitive Services. In deze instructies, hebt u geleerd hoe u:

> [!div class="checklist"]
> * Gebruik Visual Studio Code-taak voor het automatiseren van de bepalingen van de cloud
> * Verbindingsreeks voor Azure IoT-apparaat configureren
> * De Azure-functie implementeren
> * De vertaling van gesproken bericht testen

Ga door naar de andere zelfstudies voor meer informatie:

> [!div class="nextstepaction"]
> [IoT DevKit AZ3166 verbinden met Azure IoT Remote Monitoring solution accelerator](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
