---
title: 'Quickstart: Aangepaste spraak op de eerste virtuele assistent (Preview), Java (Windows, Linux) - spraakservices'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids leert u hoe u kunt de Cognitive Services spraak Software Development Kit (SDK) gebruiken in een Java-consoletoepassing. U leert hoe u verbinding kunt maken van de clienttoepassing een eerder gemaakte Bot Framework-bot geconfigureerd voor het gebruik van het kanaal directe regel spraak en inschakelen van een virtuele assistent voice-first-ervaring.
services: cognitive-services
author: bidishac
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: bidishac
ms.openlocfilehash: 4e9010bed54d0b2a7cb1a95b9e01e5ba02ea9fd5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027017"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-java"></a>Quickstart: Maken van een stem op de eerste virtuele assistent met de SDK van de spraak-, Java

In dit artikel hebt u een Java-consoletoepassing maakt met behulp van de [Cognitive Services Speech SDK](speech-sdk.md). De toepassing maakt verbinding met een eerder opgestelde bot geconfigureerd voor het gebruik van de directe regel spraak-kanaal, een stem-aanvraag verzenden en retourneren een voice response-activiteit (indien geconfigureerd). De toepassing is gemaakt met de Speech SDK Maven-pakket en de Eclipse Java-IDE op Windows, Ubuntu Linux of macOS. De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* Besturingssysteem: Windows (64-bits), Ubuntu Linux 16.04/18.04 (64-bits) of macOS 10.13 of hoger
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) of [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).
* Een vooraf geconfigureerde bot gemaakt met behulp van Bot Framework versie 4.2 of hoger. De bot moet zich abonneren op het nieuwe kanaal 'Directe regel spraak' Toon invoer ontvangen.

    > [!NOTE]
    > Preview-versie, het kanaal directe regel spraak ondersteunt momenteel alleen de **westus2** regio.

    > [!NOTE]
    > De proefversie van 30 dagen voor de prijscategorie wordt beschreven in standaard [Speech Services gratis uitproberen](get-started.md) is beperkt tot **westus** (niet **westus2**) en is dus niet compatibel met Direct Regel spraak. Gratis en standard-laag **westus2** abonnementen compatibel zijn.

Als u Ubuntu 16.04/18.04 uitvoert, zorg er dan voor dat deze afhankelijkheden zijn geïnstalleerd voordat u begint met Eclipse:

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2 wget
```

Als u Windows uitvoert (64-bits), controleert u of u de Microsoft Visual hebt geïnstalleerd C++ Redistributable voor uw platform:
* [Download Microsoft Visual C++ Redistributable voor Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## <a name="optional-get-started-fast"></a>Optioneel: Snel aan de slag

In deze Quick Start wordt beschreven, stap voor stap het maken van een eenvoudige clienttoepassing die verbinding maken met uw bot spraak ingeschakeld. Als u liever meteen in de volledige, klaar om te compileren broncode gebruikt in deze snelstartgids is beschikbaar in de [spraak SDK voorbeelden](https://aka.ms/csspeech/samples) onder de `quickstart` map.

## <a name="create-and-configure-project"></a>Project maken en configureren

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

Bovendien, als logboekregistratie wilt inschakelen, werken de **pom.xml** bestand om op te nemen van de volgende afhankelijkheden.

   ```xml
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.5</version>
    </dependency>
   ```

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Om een nieuwe lege klasse aan uw Java-project toe te voegen, selecteert u **Bestand** > **Nieuw** > **Klasse**.

1. Voer in het venster **Nieuwe Java-klasse**, in het veld **Pakket**, **speechsdk.quickstart** in en voer in het veld **Naam** **Main** in.

   ![Schermopname van het venster Nieuwe Java-klasse](media/sdk/qs-java-jre-06-create-main-java.png)

1. Open het zojuist gemaakte **Main** klasse en vervang de inhoud van de `Main.java` -bestand met de volgende vanaf code.

    ```java
    package speechsdk.quickstart;

    import java.io.IOException;
    import java.io.PipedOutputStream;
    import java.util.HashMap;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import com.microsoft.cognitiveservices.speech.ResultReason;
    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.dialog.BotConnectorConfig;
    import com.microsoft.cognitiveservices.speech.dialog.SpeechBotConnector;

    public class Main {
        final Logger log = LoggerFactory.getLogger(Main.class);

        public static void main(String[] args) {
            // New code will go here
        }
    }
    ```

1. In de **belangrijkste** methode, eerst configureert u uw `BotConnectorConfig` en maken een `SpeechBotConnector` exemplaar. Dit maakt verbinding met de rechtstreekse regel spraak kanaal om te communiceren met uw bot. Een `AudioConfig` exemplaar wordt ook gebruikt om op te geven van de bron voor audio-invoer. In dit voorbeeld wordt de microfoon standaard wordt gebruikt met `AudioConfig.fromDefaultMicrophoneInput()`.

    * Vervang de tekenreeks `YourSubscriptionKey` met uw abonnementssleutel die u kunt ophalen uit [hier](get-started.md).
    * Vervang de tekenreeks `YourServiceRegion` met de [regio](regions.md) die zijn gekoppeld aan uw abonnement.
    * Vervang de tekenreeks `YourChannelSecret` met uw geheim rechtstreekse regel spraak-kanaal.

    > [!NOTE]
    > Preview-versie, het kanaal directe regel spraak ondersteunt momenteel alleen de **westus2** regio.

    > [!NOTE]
    > De proefversie van 30 dagen voor de prijscategorie wordt beschreven in standaard [Speech Services gratis uitproberen](get-started.md) is beperkt tot **westus** (niet **westus2**) en is dus niet compatibel met Direct Regel spraak. Gratis en standard-laag **westus2** abonnementen compatibel zijn.

    ```java
    final String channelSecret = "YourChannelSecret"; // Your channel secret
    final String subscriptionKey = "YourSubscriptionKey"; // your subscription key
    final String region = "YourServiceRegion"; // Your service region. Currently assumed to be westus2
    final BotConnectorConfig botConnectorConfig = BotConnectorConfig.fromSecretKey(channelSecret, subscriptionKey, region);

    // Configure audio input from microphone.
    final AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();

    // Create a SpeechjBotConnector instance
    final SpeechBotConnector botConnector = new SpeechBotConnector(botConnectorConfig, audioConfig);
    ```

1. `SpeechBotConnector` afhankelijk van diverse gebeurtenissen om te communiceren de bot-activiteiten, herkenningsresultaten spraak en andere informatie. Deze gebeurtenislisteners naast toevoegen.

    ```java
    // Recognizing will provide the intermediate recognized text while an audio stream is being processed
    botConnector.recognizing.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognizing speech event text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // Recognized will provide the final recognized text once audio capture is completed
    botConnector.recognized.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognized speech event reason text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // SessionStarted will notify when audio begins flowing to the service for a turn
    botConnector.sessionStarted.addEventListener((o, sessionEventArgs) -> {
        log.info("Session Started event id: {} ", sessionEventArgs.getSessionId());
    });

    // SessionStopped will notify when a turn is complete and it's safe to begin listening again
    botConnector.sessionStopped.addEventListener((o, sessionEventArgs) -> {
        log.info("Session stopped event id: {}", sessionEventArgs.getSessionId());
    });

    // Canceled will be signaled when a turn is aborted or experiences an error condition
    botConnector.canceled.addEventListener((o, canceledEventArgs) -> {
        log.info("Canceled event details: {}", canceledEventArgs.getErrorDetails());
        botConnector.disconnectAsync();
    });

    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities
    botConnector.activityReceived.addEventListener((o, activityEventArgs) -> {
        String act = activityEventArgs.getActivity().serialize();
        log.info("Received activity: {}", act);
    });
    ```

1. Verbinding maken met de `SpeechBotConnector` directe regel Speech door het aanroepen van de `connectAsync()` methode. Als u wilt testen van uw bot, kunt u aanroepen de `listenOnceAsync` methode voor het verzenden van audio-invoer van de microfoon. Daarnaast ook kunt u de `sendActivityAsync` methode voor het verzenden van een aangepaste activiteit als een geserialiseerde tekenreeks. Deze aangepaste activiteiten kunnen zorgen dat aanvullende gegevens die uw bot in de conversatie gebruikt.

    ```java
    botConnector.connectAsync();
    // Start listening.
    System.out.println("Say something ...");
    botConnector.listenOnceAsync();

    // botConnector.sendActivityAsync(...)
    ```

1. Wilt u de wijzigingen aan de `Main` bestand.

1. Voor de ondersteuning van antwoord afspelen, voegt u een extra klasse die nuttige methoden bevat ter ondersteuning van audio. Om in te schakelen audio, kunt u een andere nieuwe lege klasse toevoegen aan uw Java-project: Selecteer **bestand** > **nieuw** > **klasse**.

1. In de **nieuwe Java-klasse** venster, geef **speechsdk.quickstart** in de **pakket** veld en **AudioPlayer** in de **Naam** veld.

   ![Schermopname van het venster Nieuwe Java-klasse](media/sdk/qs-java-jre-06-create-main-java.png)

1. Open het zojuist gemaakte **AudioPlayer** klasse en vervangen door de code hieronder.

    ```java
    import static javax.sound.sampled.AudioFormat.Encoding.PCM_SIGNED;

    import java.io.InputStream;
    import java.io.PipedInputStream;
    import java.io.PipedOutputStream;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    import java.util.concurrent.atomic.AtomicBoolean;

    import javax.sound.sampled.AudioFormat;
    import javax.sound.sampled.AudioSystem;
    import javax.sound.sampled.DataLine;
    import javax.sound.sampled.SourceDataLine;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;


    public class AudioPlayer {

        public static final int SAMPLE_RATE = 16000; // 16Hz sampling rate
        public static final int SAMPLE_SIZE_IN_BITS = 16; // 16 bit PCM
        public static final int CHANNELS = 1; // Use Mono / Single channel

        public static final int FRAME_RATE = 16000;
        public static final int FRAME_SIZE = 2;

        private static final Logger log = LoggerFactory.getLogger(AudioPlayer.class);
        private AtomicBoolean isPlaying = new AtomicBoolean(false);
        private ExecutorService executorService = Executors.newSingleThreadExecutor();

        public boolean isPlaying() {
            return isPlaying.get();
        }

        public void stopPlaying() {
            isPlaying.set(false);
        }

        public void play(final PipedOutputStream pipedOutputStream) {
            // The current audio supported by the Microsoft Bot framework ~ 16-bit PCM encoding, 16KHz sampling rate.
            final AudioFormat defaultFormat = new AudioFormat(PCM_SIGNED, SAMPLE_RATE, SAMPLE_SIZE_IN_BITS, CHANNELS, FRAME_SIZE, FRAME_RATE, false);
            try {
                final PipedInputStream inputStream = new PipedInputStream(pipedOutputStream);

                executorService.submit(() -> {
                    try {
                        isPlaying.set(true);
                        play(inputStream, defaultFormat);
                        inputStream.close();
                    } catch (Exception e) {
                        log.error("Exception thrown during playback. Message: {}", e.getMessage(), e);
                    }
                });
            } catch (Exception e) {
                log.error("Exception thrown during playback. Message: {}", e.getMessage(), e);
            }
        }

        private void play(final InputStream inputStream, final AudioFormat targetFormat) throws Exception {
            final byte[] buffer = new byte[1024];
            final DataLine.Info info = new DataLine.Info(SourceDataLine.class, targetFormat);
            final SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
            line.open();
            if (line != null) {
                line.start();
                int bytesRead = 0;
                while (bytesRead != -1) {
                    bytesRead = inputStream.read(buffer, 0, buffer.length);
                    if (bytesRead != -1) {
                        line.write(buffer, 0, bytesRead);
                    }
                }
                line.drain();
                line.stop();
                line.close();
            }
        }
    }
    ```

1. Wilt u de wijzigingen aan de `AudioPlayer` bestand.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

Druk op F11 of selecteer **Uitvoeren** > **Fouten opsporen**.
De console een bericht weergegeven met 'Bijvoorbeeld iets' op dit moment, spreekt u mogelijk een Engelse woordgroep of meer woorden die uw bot begrijpt. Uw stem wordt verzonden naar uw bot via de rechtstreekse regel spraak kanaal waar deze wordt herkend, door uw bot verwerkt en het antwoord wordt geretourneerd als een activiteit. Als uw bot spraak als antwoord resulteert, het geluid wordt afgespeeld terug met behulp van de `AudioPlayer` klasse.

![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>Volgende stappen

Op GitHub vindt u aanvullende voorbeelden, zoals hoe u spraak kunt lezen vanuit een audiobestand.

> [!div class="nextstepaction"]
> [Bekijk Java-voorbeelden op GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Snelstart: Omzetten van spraak, Java (Windows, Linux)](quickstart-translate-speech-java-jre.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
