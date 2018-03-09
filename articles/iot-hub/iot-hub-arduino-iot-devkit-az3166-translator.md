---
title: IoT DevKit vertaler met Azure-functie en cognitieve Services | Microsoft Docs
description: Gebruik microfoon op IoT DevKit spraakbericht en cognitieve Azure-Services voor het verwerken van het in vertaalde tekst in het Engels ontvangen.
services: iot-hub
documentationcenter: 
author: liydu
manager: timlt
tags: 
keywords: 
ms.service: iot-hube
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2018
ms.author: liydu
ms.openlocfilehash: 5f74e43d20b5954ae4808f82a34372d1e2190481
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="use-iot-devkit-az3166-with-azure-function-and-cognitive-services-to-make-a-language-translator"></a>IoT DevKit AZ3166 met Azure-functie en cognitieve Services gebruiken om te maken van een vertaler taal

In dit artikel leert u hoe IoT DevKit als een vertaler taal maken via [cognitieve Azure-Services](https://azure.microsoft.com/services/cognitive-services/). Het registreert uw stem en wordt deze omgezet in Engelse tekst die wordt weergegeven op het scherm DevKit.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een Arduino alles in één mededelingenbord compatibel is met de randapparaten uitgebreide en sensoren. U kunt ontwikkelen via [Visual Studio Code-uitbreiding voor Arduino](https://aka.ms/arduino). En beschikt u over een groeiende [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om u te begeleiden prototype Internet der dingen (IoT) oplossingen die van Microsoft Azure-services gebruikmaken.

## <a name="what-you-need"></a>Wat u nodig hebt

Voltooid de [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) naar:

* Hebt u uw DevKit verbonden met Wi-Fi
* De ontwikkelomgeving voorbereiden

Een actief Azure-abonnement. Als u geen abonnement hebt, kunt u via een van deze twee manieren registreren:

* Activeren van een [gratis 30-daagse evaluatieversie Microsoft Azure-account](https://azureinfo.microsoft.com/us-freetrial.html)
* Claim uw [Azure-tegoeden](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) als u MSDN of Visual Studio-abonnee bent

## <a name="step-1-open-the-project-folder"></a>Step 1. Open de projectmap

### <a name="a-start-vs-code"></a>A. VS Code starten

- Zorg ervoor dat uw DevKit niet is verbonden met uw PC.
- VS Code starten
- De DevKit aansluiten op uw computer.

VS-Code wordt automatisch de DevKit vindt en opent een pagina Inleiding:

![Introductiepagina](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_start.png)

### <a name="b-open-the-arduino-examples-folder"></a>B. Open de map Arduino-voorbeelden

Vouw de linkerkant **ARDUINO voorbeelden > Voorbeelden voor MXCHIP AZ3166 > AzureIoT**, en selecteer **DevKitTranslator**. Er wordt een nieuw venster van de VS Code geopend met de projectmap DEVKITTRANSLATOR in het.

![Voorbeelden van IoT DevKit](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_examples.png)

Als u per ongeluk het venster sluit, kunt u deze opnieuw openen. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) typt u de opdracht om palet te openen, **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

## <a name="step-2-provision-azure-services"></a>Stap 2. Azure-services inrichten

Typ in het venster solution `Ctrl+P` (Mac OS: `Cmd+P`) en voer `task cloud-provision`.

In de terminal Code tegenover leidt een interactieve opdrachtregel u voor het inrichten van alle benodigde Azure-services:

![Cloud inrichten taak](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-provision.png)

## <a name="step-3-deploy-azure-functions"></a>Stap 3. Azure Functions implementeren

Gebruik `Ctrl+P` (Mac OS: `Cmd+P`) om uit te voeren `task cloud-deploy` om de Azure Functions-code te implementeren. Dit proces duurt doorgaans 2 tot en met 5 minuten:

![Implementatietaak voor de cloud](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-deploy.png)

Nadat Azure-functie is geïmplementeerd, vult u het bestand azure_config.h met de functie app-naam. U kunt navigeren naar [Azure-portal](https://portal.azure.com/) te vinden:

![Azure-functie app-naam vinden](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-function.png)

> [!NOTE]
> Als de Azure-functie niet goed werkt, controleert u dit [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) sectie te verhelpen.

## <a name="step-4-build-and-upload-the-device-code"></a>Stap 4. Maken en uploaden van de apparaatcode

1. Gebruik `Ctrl+P` (Mac OS: `Cmd+P`) om uit te voeren `task config-device-connection`.

2. De terminal u wordt gevraagd of u wilt gebruiken van de verbindingsreeks die wordt opgehaald uit `task cloud-provision` stap. U kunt ook uw eigen apparaat verbindingsreeks invoeren door te selecteren **'Nieuw'**

3. De terminal vraagt u om de configuratie activeren. Om dit te doen, houdt u A, en vervolgens push en release van de knop herstellen. Het scherm wordt weergegeven voor de DevKit-ID en 'Configuration'.
  ![Verificatie en uploaden van het schema Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/config-device-connection.png)

4. Na `task config-device-connection` is voltooid, klikt u op `F1` tegenover codeopdrachten laden en selecteer `Arduino: Upload`, VS-Code wordt gestart controleren en werk de Arduino uploaden: ![controle en het uploaden van het schema Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/arduino-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

## <a name="test-the-project"></a>Het project testen

Volg de instructies op het scherm DevKit na de initialisatie van de app. De standaardtaal voor de bron is Chinees.

Selecteer een andere taal voor vertaling:

1. Druk op een knop om setup te activeren.
2. Druk op de knop B scrollen alle ondersteunde talen van de bron.
3. Druk op de knop A uw keuze van taal voor de bron te bevestigen.
4. Knop B ingedrukt te houden tijdens het spreken en loslaat B voor het initiëren van de vertaling.
5. De vertaalde tekst in het Engels wordt weergegeven op het scherm.

![Blader naar de taal selecteren](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![De vertaling van resultaat](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

U kunt op het scherm voor het resultaat van de vertaling van:

- Druk op de knop A en B om te bladeren en selecteer de taal van de bron.
- Druk op B om te communiceren, om de stem verzenden en krijgt u de Vertalingtekst van release

## <a name="how-it-works"></a>Hoe werkt het?

![mini-solution-voice-to-tweet-diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

De schema-records Arduino uw stem boekt vervolgens een HTTP-aanvraag activeren van Azure Functions. Azure Functions roept de cognitieve service spraak vertaler API de vertaling doen. Nadat Azure Functions de van Vertalingtekst haalt, wordt een C2D-bericht verzonden naar het apparaat. De vertaling wordt weergegeven op het scherm.

## <a name="change-device-id"></a>Apparaat-ID wijzigen

De geregistreerd in Azure IoT Hub apparaat-ID is **AZ3166**. Als u wijzigen wilt, voert u de [hier instructies](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of bereiken aan ons van de volgende kanalen:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

Nu u de IoT DevKit als een vertaler met behulp van Azure-functie en cognitieve Services. In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Gebruik Visual Studio Code-taak voor het automatiseren van de bepalingen van de cloud
> * Verbindingsreeks voor Azure IoT-apparaat configureren
> * De functie Azure implementeren
> * De vertaling voice-bericht testen

Ga naar de andere zelfstudies voor meer informatie:

> [!div class="nextstepaction"]
> [Verbinding maken met IoT DevKit AZ3166 Azure IoT Suite voor externe controle](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)