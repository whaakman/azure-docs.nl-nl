---
title: 'Quickstart: De speech apparaten SDK uitvoeren op de Windows Speech-Service'
titleSuffix: Azure Cognitive Services
description: Vereisten en instructies om aan de slag te gaan met een Windows speech-apparaat-SDK.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/10/2019
ms.author: erhopf
ms.openlocfilehash: ad90a6443cc1c94bcdb730e783b82dfdd4798676
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553128"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-windows"></a>Quickstart: De voor beeld-app voor de speech-apparaten SDK uitvoeren in Windows

In deze Quick Start leert u hoe u de speech apparaten SDK voor Windows kunt gebruiken om een product met spraak herkenning te bouwen of dit te gebruiken als een [conversatie transcriptie](conversation-transcription-service.md) -apparaat. Momenteel wordt alleen [Azure KINECT DK](https://azure.microsoft.com/services/kinect-dk/) ondersteund.

De toepassing is gebouwd met het Speech SDK-pakket en de eclips Java IDE (v4) op 64-bits Windows. De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

Voor deze hand leiding is een [Azure Cognitive Services](get-started.md) -account vereist met een speech Services-resource. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

De bron code voor de [voorbeeld toepassing](https://aka.ms/sdsdk-download-JRE) is opgenomen in de speech-apparaten SDK. Het is ook [beschikbaar op GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* Besturingssysteem: 64-bits Windows
* [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* Alleen [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) of [jdk 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html) .
* [Microsoft Visual C++ Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).
* Down load de nieuwste versie van de [Speech apparaten SDK](https://aka.ms/sdsdk-download-JRE) voor Java en pak het. zip-bestand uit in uw werkmap.
   > [!NOTE]
   > Het bestand JRE-Sample-Release. zip bevat de JRE-voor beeld-app. in deze Snelstartgids wordt ervan uitgegaan dat de app is geëxtraheerd naar C:\SDSDK\JRE-Sample-Release

De conversatie transcriptie is momenteel alleen beschikbaar voor ' nl-US ' en ' zh-CN ' in de regio's ' midden ' en ' EastAsia '. U moet een spraak sleutel hebben in een van deze regio's om conversatie transcriptie te kunnen gebruiken.

Als u van plan bent om de doel stellingen te gebruiken, hebt u een [Luis-abonnement (Language Understanding service)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) nodig. Zie voor meer informatie over LUIS en intentie herkenning [spraak intentie herkennen met Luis, C# ](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp). Er is een voor beeld van een [Luis-model](https://aka.ms/sdsdk-luis) beschikbaar voor deze app.

## <a name="create-and-configure-the-project"></a>Het project maken en configureren

1. Start Eclipse.

1. Voer in het **Start programma voor de eclips IDE**in het veld **werk ruimte** de naam in van een nieuwe map voor de werk ruimte. Selecteer vervolgens **Starten**.

   ![Schermopname van Eclipse Launcher](media/speech-devices-sdk/eclipse-launcher.png)

1. Binnen enkele ogenblikken verschijnt het hoofdvenster van de Eclipse-IDE. Sluit het welkomstscherm als dit verschijnt.

1. Maak een nieuw project op de menu balk van de eclips door **bestand** > **Nieuw** > **Java-project**te kiezen. Als deze niet beschikbaar is, kiest u **project** en vervolgens **Java project**.

1. De wizard **nieuw Java-project** wordt gestart. **Blader** naar de locatie van het voorbeeld project. Selecteer **Finish**.

   ![Schermopname van de wizard Nieuw Java-project](media/speech-devices-sdk/eclipse-new-java-project.png)

1. Klik in de **pakket Verkenner**met de rechter muisknop op uw project. Kies in het contextmenu **Configureren** > **Naar Maven-project converteren**. Selecteer **Finish**.

   ![Schermopname van Package explorer](media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. Kopiëren `kws.table`en  naar de projectmap target\classes `participants.properties` `Microsoft.CognitiveServices.Speech.extension.pma.dll`

## <a name="configure-the-sample-application"></a>De voorbeeld toepassing configureren

1. Voeg uw spraak abonnee sleutel toe aan de bron code. Als u proberen intentieherkenning wilt, voegt u ook uw [Language Understanding service](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonnementssleutel en de toepassing-id.

   Voor spraak-en LUIS gaat u naar `FunctionsList.java`de volgende informatie:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

    Als u gesprek transcriptie gebruikt, zijn uw spraak sleutel en regio gegevens ook nodig in `Cts.java`:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
    ```

1. Het standaard wake woord (trefwoord) is 'Computer'. U kunt ook een van de opgegeven andere woorden, zoals 'Machine' of 'Assistent' activeren. De bronbestanden voor deze alternatieve wake-woorden zijn in de SDK van de spraak-apparaten in de map trefwoord. `C:\SDSDK\JRE-Sample-Release\keyword\Computer` Bevat bijvoorbeeld de bestanden die worden gebruikt voor het Ontwaak woord "computer".

   > [!TIP]
   > U kunt ook [maken van een aangepaste wake-woord](speech-devices-sdk-create-kws.md).

    Als u een nieuw Ontwaak woord wilt gebruiken, werkt u de `FunctionsList.java`volgende twee regels in en kopieert u het Ontwaak woord pakket naar uw app. Als u bijvoorbeeld het Ontwaak woord ' machine ' uit het Ontwaak `kws-machine.zip`programma wilt gebruiken:

   * Kopieer het Ontwaak woord pakket naar de projectmap **/klassen**van het project.

   * Werk de `FunctionsList.java` met het sleutel woord en de pakket naam bij:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

## <a name="run-the-sample-application-from-eclipse"></a>De voorbeeld toepassing vanuit een eclips uitvoeren

1.  >  Voer > in de menu balk van de eclips**uitvoeren als** **Java-toepassing**uit. Selecteer vervolgens **FunctionsList** en **OK**.

   ![Scherm opname van een Java-toepassing selecteren](media/speech-devices-sdk/eclipse-run-sample.png)

1. De voorbeeldtoepassing met spraak Devices SDK wordt gestart en worden de volgende opties weergegeven:

   ![Voorbeeld van de voorbeeldtoepassing spraak Devices SDK en opties](media/speech-devices-sdk/java-sample-app-windows.png)

1. Probeer de nieuwe **transcriptie** -demo van de conversatie. Begin met transcriberen met**Start**van de **sessie** > . Standaard is iedereen een gast. Als u echter de spraak handtekeningen van een deel nemer hebt, kunnen ze in een `participants.properties` bestand worden geplaatst in het **doel/klassen**van de projectmap. Als u de spraak handtekening wilt genereren, kijkt u naar Transcribe- [gesprekken (SDK)](how-to-use-conversation-transcription-service.md).

   ![Transcriptie-toepassing voor demo gesprekken](media/speech-devices-sdk/cts-sample-app-windows.png)

## <a name="create-and-run-a-standalone-application"></a>Een zelfstandige toepassing maken en uitvoeren

1. Klik in de **pakket Verkenner**met de rechter muisknop op uw project. Kies **exporteren**. 

1. Het venster **exporteren** wordt weer gegeven. Vouw **Java** uit en selecteer het **uitvoer bare jar-bestand** en selecteer vervolgens **volgende**.

   ![Scherm opname van het venster exporteren](media/speech-devices-sdk/eclipse-export-windows.png) 

1. Het venster **uitvoer bare JAR file export** wordt weer gegeven. Kies een **export doel** voor de toepassing en selecteer vervolgens **volt ooien**.
 
   ![Scherm opname van uitvoer bare JAR-bestand exporteren](media/speech-devices-sdk/eclipse-export-jar-windows.png)

1. Plaats `kws.table` `participants.properties` ,,`pma.dll` en indehierbovengekozendoelmap,omdatdezebestandennodigzijnvoordetoepassing.`Microsoft.CognitiveServices.Speech.extension.pma.dll` `unimic_runtime.dll`

1. De zelfstandige toepassing uitvoeren

     ```powershell
     java -jar SpeechDemo.jar
     ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Opmerkingen bij de release bekijken](devices-sdk-release-notes.md)
