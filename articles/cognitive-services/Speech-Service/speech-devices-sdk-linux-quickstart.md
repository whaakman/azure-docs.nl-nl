---
title: 'Quickstart: De spraak-apparaten SDK worden uitgevoerd op Linux - spraakservices'
titleSuffix: Azure Cognitive Services
description: Vereisten en instructies voor het aan de slag met een Linux-SDK voor spraak-apparaten.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/10/2019
ms.author: erhopf
ms.openlocfilehash: d755f3388466369ee1edc3d9ff1e353173babc10
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723389"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-linux"></a>Quickstart: Uitvoeren van de spraakherkenning Devices SDK-voorbeeld-app op Linux

In deze snelstartgids leert u hoe u de spraak-apparaten-SDK voor Linux gebruiken om te maken van een product spraak ingeschakeld of worden gebruikt als een [conversatie transcriptie](conversation-transcription-service.md) apparaat. Momenteel wordt alleen de [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/) wordt ondersteund.

De toepassing is gemaakt met de spraak-SDK-pakket en de Eclipse Java-IDE (v4) op 64-bits Linux (Ubuntu 16.04, 18.04 Ubuntu, Debian 9). De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

Deze handleiding is vereist een [Azure Cognitive Services](get-started.md) -account met een resource Speech Services. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

De broncode voor de [voorbeeldtoepassing](https://aka.ms/sdsdk-download-JRE) is opgenomen in de SDK van de apparaten spraak. Het is ook [beschikbaar op GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* Besturingssysteem: 64-bit Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9)
* [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) of [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html) alleen.
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).
* Download de nieuwste versie van de [spraak Devices SDK](https://aka.ms/sdsdk-download-JRE) voor Java en de zip uitpakken naar uw werkmap.
   > [!NOTE]
   > Het Java Runtime Environment-voorbeeld-Release.zip-bestand bevat de JRE-voorbeeld-app en deze snelstartgids wordt ervan uitgegaan dat de app wordt uitgepakt naar /home/wcaltest/JRE-Sample-Release

Zorg ervoor dat deze afhankelijkheden zijn geïnstalleerd voordat u begint met Eclipse.

* Op Ubuntu:

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.0 libasound2
  ```

* Op Debian 9:

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.2 libasound2
  ```

Conversatie transcriptie is momenteel alleen beschikbaar voor 'en-US' en 'zh-CN', in de regio's 'centralus' en 'Oost-Aziatische'. U moet een spraak-sleutel in een van deze regio's te gebruiken van de conversatie transcriptie hebben.

Als u van plan bent de intenties gebruiken, moet u een [Language Understanding Service (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) abonnement. Zie voor meer informatie over LUIS en intentieherkenning [spraak intents met LUIS, herkent C# ](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp). Een [LUIS-model sample](https://aka.ms/sdsdk-luis) is beschikbaar voor deze app.

## <a name="create-and-configure-the-project"></a>Maken en configureren van het project

1. Start Eclipse.

1. In de **Eclipse IDE starten**, in de **werkruimte** en voer de naam van een nieuwe werkruimtemap. Selecteer vervolgens **Starten**.

   ![Schermopname van Eclipse Launcher](media/speech-devices-sdk/eclipse-launcher-linux.png)

1. Binnen enkele ogenblikken verschijnt het hoofdvenster van de Eclipse-IDE. Sluit het welkomstscherm als dit verschijnt.

1. In de menubalk Eclipse een nieuw project maken door te kiezen **bestand** > **nieuw** > **Java-Project**. Als deze niet beschikbaar Kies **Project** en vervolgens **Java-Project**.

1. De **nieuwe Java-Project** wizard wordt gestart. **Blader** voor de locatie van het voorbeeldproject. Selecteer **Finish**.

   ![Schermopname van de wizard Nieuw Java-project](media/speech-devices-sdk/eclipse-new-java-project-linux.png)

1. In de **Package explorer**, met de rechtermuisknop op uw project. Kies in het contextmenu **Configureren** > **Naar Maven-project converteren**. Selecteer **Finish**.

   ![Schermopname van Package explorer](media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. In de **Package explorer**, met de rechtermuisknop op uw project. Kies **eigenschappen**, klikt u vervolgens **uitvoeren/foutopsporingsinstellingen** > **nieuwe...** > **Java-toepassing**. 

1. De **-configuratie bewerken** venster wordt weergegeven. In de **naam** veld **Main**, en gebruik **zoeken** voor de **Main-klasse** om te zoeken en selecteer  **com.microsoft.cognitiveservices.speech.samples.FunctionsList**.

   ![Schermopname van configuratie van de lancering bewerken](media/speech-devices-sdk/eclipse-edit-launch-configuration-linux.png)

1. Ook uit de **-configuratie bewerken** venster selecteren de **omgeving** pagina en **nieuw**. De **nieuwe omgevingsvariabele** venster wordt weergegeven. In de **naam** veld **LD_LIBRARY_PATH** en in de **waarde** veld Geef de map met de bestanden *.so bijvoorbeeld   **/home/wcaltest / JRE-voorbeeld-Release**

1. Kopie `kws.table` en `participants.properties` in de projectmap **doelklassen /**


## <a name="configure-the-sample-application"></a>De voorbeeld-App configureren

1. Uw abonnementssleutel spraak toevoegen aan de broncode. Als u proberen intentieherkenning wilt, voegt u ook uw [Language Understanding service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonnementssleutel en de toepassing-id.

   Voor spraak en LUIS, uw gegevens krijgt `FunctionsList.java`:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

    Als u gebruikmaakt van conversatie transcriptie, zijn ook nodig is uw sleutel en de regio spraakgegevens `Cts.java`:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
    ```

1. Het standaard wake woord (trefwoord) is 'Computer'. U kunt ook een van de opgegeven andere woorden, zoals 'Machine' of 'Assistent' activeren. De bronbestanden voor deze alternatieve wake-woorden zijn in de SDK van de spraak-apparaten in de map trefwoord. Bijvoorbeeld, `/home/wcaltest/JRE-Sample-Release/keyword/Computer` bevat de bestanden die worden gebruikt voor de wake-woord 'Computer'.

   > [!TIP]
   > U kunt ook [maken van een aangepaste wake-woord](speech-devices-sdk-create-kws.md).

    Voor het gebruik van een nieuwe wake-woord, werk de volgende twee regels in `FunctionsList.java`, en het woord wake pakket kopiëren naar uw app. Bijvoorbeeld, u het wake-woord 'Computer' van het word-pakket van wake `kws-machine.zip`:

   * Kopieer het pakket van de wake-word in de projectmap **doelklassen/** .

   * Update de `FunctionsList.java` met het sleutelwoord en naam van het pakket:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

## <a name="run-the-sample-application-from-eclipse"></a>De voorbeeldtoepassing uitvoeren vanuit Eclipse

1. In de menubalk Eclipse **uitvoeren** > **uitvoeren** 

1. De voorbeeldtoepassing met spraak Devices SDK wordt gestart en worden de volgende opties weergegeven:

   ![Voorbeeld van de voorbeeldtoepassing spraak Devices SDK en opties](media/speech-devices-sdk/java-sample-app-linux.png)

1. Probeer de nieuwe **conversatie transcriptie** demo. Start te transcriberen met **sessie** > **Start**. Iedereen is standaard een gast. Echter, als u van deelnemer stem handtekeningen hebben ze kunnen worden geplaatst in `participants.properties` in de projectmap **doelklassen/** . Voor het genereren van de handtekening gesproken kijken [(SDK) gesprekken transcriberen](how-to-use-conversation-transcription-service.md).

   ![Conversatie transcriptie demotoepassing](media/speech-devices-sdk/cts-sample-app-linux.png)

## <a name="create-and-run-standalone-the-application"></a>Maken en voer de toepassing uit zelfstandige

1. In de **Package explorer**, met de rechtermuisknop op uw project. Kies **exporteren**. 
1. De **exporteren** venster wordt weergegeven. Vouw **Java** en selecteer **uitvoerbaar JAR-bestand** en selecteer vervolgens **volgende**.

   ![Schermafbeelding van het venster uitvoer](media/speech-devices-sdk/eclipse-export-linux.png) 

1. De **uitvoerbaar JAR-bestand exporteren** venster wordt weergegeven. Kies een **exportbestemming** voor de toepassing en selecteer vervolgens **voltooien**.
 
   ![Schermafbeelding van uitvoerbaar JAR-bestand exporteren](media/speech-devices-sdk/eclipse-export-jar-linux.png)

1. . Plaats `kws.table` en `participants.properties` in de doelmap die hierboven wordt gekozen als deze bestanden nodig door de toepassing is.

1. De LD_LIBRARY_LIB ingesteld op de map met de *.so-bestanden

     ```bash
     export LD_LIBRARY_PATH=/home/wcaltest/JRE-Sample-Release
     ```

1. De zelfstandige toepassing uit te voeren

     ```bash
     java -jar SpeechDemo.jar
     ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Opmerkingen bij de release bekijken](devices-sdk-release-notes.md)
