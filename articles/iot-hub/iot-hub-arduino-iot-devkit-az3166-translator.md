---
title: Maken van een IoT DevKit translator met behulp van Azure Functions en Cognitive Services | Microsoft Docs
description: Gebruik van de microfoon op een IoT DevKit een voicemailbericht ontvangen en vervolgens met Azure Cognitive Services voor het verwerken van deze in de vertaalde tekst in het Engels
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 12/19/2018
ms.author: liydu
ms.openlocfilehash: 038b1d9fa319837f3877c20c9fc3b1b83970e7b4
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54158615"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>IoT DevKit AZ3166 met Azure Functions en Cognitive Services gebruiken om te maken van een vertaler

In dit artikel leert u hoe u IoT DevKit als een conversieprogramma voor taal met behulp van [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/). Deze records van uw stem en wordt omgezet in het Engelse tekst die wordt weergegeven op het scherm DevKit.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een compatibele alles-in-een Arduino-bord met uitgebreide randapparatuur en sensoren. U kunt ontwikkelen voor met behulp van [Azure IoT-apparaat Workbench](https://aka.ms/iot-workbench) of [hulpprogramma's voor Azure IoT](https://aka.ms/azure-iot-tools) -uitbreidingspakket in Visual Studio Code. De [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) bevat voorbeeldtoepassingen om u te helpen prototype IoT-oplossingen.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in deze zelfstudie hebt voltooid, moet u eerst de volgende taken uitvoeren:

* Voorbereiden van uw DevKit met de volgende stappen in [IoT DevKit AZ3166 verbinding maken met Azure IoT Hub in de cloud](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).

## <a name="create-azure-cognitive-service"></a>Azure Cognitive Service maken

1. Klik in de Azure-portal op **een resource maken** en zoek naar de **spraak**. Vul het formulier in om de spraak-Service te maken.
  ![Spraakservice](media/iot-hub-arduino-iot-devkit-az3166-translator/speech-service.png)

1. Ga naar de spraakservice u zojuist hebt gemaakt, klikt u op **sleutels** sectie om te kopiëren en noteer de **Key1** voor DevKit toegang tot deze.
  ![Kopieer sleutels](media/iot-hub-arduino-iot-devkit-az3166-translator/copy-keys.png)

## <a name="open-sample-project"></a>Open voorbeeldproject

1. Zorg ervoor dat uw IoT DevKit is **niet verbonden** op uw computer. VS Code eerst start, en vervolgens de DevKit verbinden met uw computer.

1. Klik op `F1` om te openen de command palette, typ en selecteer **Azure IoT-apparaat Workbench: Voorbeelden openen...** . Selecteer vervolgens **IoT DevKit** als bord.

1. Zoek in de pagina IoT Workbench voorbeelden **DevKit Translator** en klikt u op **Open voorbeeld**. Vervolgens selecteert het standaardpad voor het downloaden van de voorbeeldcode.
  ![Open voorbeeld](media/iot-hub-arduino-iot-devkit-az3166-translator/open-sample.png)

## <a name="use-speech-service-with-azure-functions"></a>Spraak-Service gebruiken met Azure Functions

1. In VS Code, klikt u op `F1`, typt en selecteer **Azure IoT-apparaat Workbench: Azure-Services inrichten...** . ![Azure-services inrichten](media/iot-hub-arduino-iot-devkit-az3166-translator/provision.png)

1. Volg de stappen voor het voltooien van het inrichten van Azure IoT Hub en Azure Functions.
  ![Stappen voor inrichten](media/iot-hub-arduino-iot-devkit-az3166-translator/provision-steps.png)

  Noteer de naam van het Azure IoT Hub-apparaat die u hebt gemaakt.

1. Open `Functions\DevKitTranslatorFunction.cs` en bijwerken van de volgende regels code met de naam van de devce en de Spraakservice-sleutel die u hebt genoteerd.
  ```csharp
  // Subscription Key of Speech Service
  const string speechSubscriptionKey = "";

  // Region of the speech service, see https://docs.microsoft.com/azure/cognitive-services/speech-service/regions for more details.
  const string speechServiceRegion = "";

  // Device ID
  const string deviceName = "";
  ```

1. Klik op `F1`, typt en selecteer **Azure IoT-apparaat Workbench: Implementeren naar Azure...** . Als u VS Code wordt gevraagd om bevestiging voor opnieuw implementeren, klikt u op **Ja**.
  ![Waarschuwing implementeren](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-warning.png)

1. Zorg ervoor dat de implementatie is gelukt.
  ![Succes implementeren](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-success.png)

1. In Azure portal, gaat u naar **Functions-Apps** sectie, zoek de Azure-functie-app zojuist hebt gemaakt. Klik op `devkit_translator`, klikt u vervolgens op **<> / functie-URL ophalen** om te kopiëren van de URL.
  ![Functie-URL kopiëren](media/iot-hub-arduino-iot-devkit-az3166-translator/get-function-url.png)

1. Plak de URL in `azure_config.h` bestand.
  ![Azure config](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-config.png)

  > [!NOTE]
  > Als de functie-app niet goed werkt, controleert u deze [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) sectie om dit te verhelpen.

## <a name="build-and-upload-device-code"></a>Bouw en apparaatcode uploaden

1. Overschakelen van de DevKit naar **configuratiemodus** door:
  * Houd **A**.
  * Druk **opnieuw** knop.

  U ziet het scherm wordt weergegeven de DevKit-ID en **configuratie**.

  ![Configuratiemodus DevKit](media/iot-hub-arduino-iot-devkit-az3166-translator/devkit-configuration-mode.png)

1. Klik op `F1`, typt en selecteer **Azure IoT-apparaat Workbench: Configureren van apparaatinstellingen... > Config Apparaatverbindingsreeks**. Selecteer **Selecteer IoT Hub apparaat Connection String** te configureren op de DevKit.
  ![Verbindingsreeks configureren](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string.png)

1. U ziet de melding zodra deze voltooid.
  ![Configureren van de connection string geslaagd](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string-success.png)

1. Klik op `F1` Typ wederom en selecteer **Azure IoT-apparaat Workbench: Uploaden apparaatcode**. Het compileren wordt gestart en de code uploaden naar DevKit.
  ![Apparaat uploaden](media/iot-hub-arduino-iot-devkit-az3166-translator/device-upload.png)

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

## <a name="how-it-works"></a>Hoe het werkt

![mini-solution-voice-to-tweet-diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

De IoT DevKit registreert uw stem vervolgens plaatst een HTTP-aanvraag aan Azure Functions-trigger. Azure Functions-aanroepen de spraakomzetting cognitive service API voor het uitvoeren van de vertaling. Nadat Azure Functions de vertaalde tekst haalt, verzendt deze een C2D-bericht naar het apparaat. De vertaling wordt vervolgens weergegeven op het scherm.

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u de [IoT DevKit Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of contact opnemen met ons opnemen met behulp van de volgende kanalen:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

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
