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
ms.openlocfilehash: 83149a8422db25106a97b1711c0ae9ce3c6603eb
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465664"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-java"></a>Quickstart: Maken van een stem op de eerste virtuele assistent met de SDK van de spraak-, Java

Snelstartgidsen zijn ook beschikbaar voor [spraak-naar-tekst](quickstart-java-jre.md) en [spraakomzetting](quickstart-translate-speech-java-jre.md).

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

    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
    import com.microsoft.cognitiveservices.speech.dialog.BotConnectorConfig;
    import com.microsoft.cognitiveservices.speech.dialog.SpeechBotConnector;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import javax.sound.sampled.AudioFormat;
    import javax.sound.sampled.AudioSystem;
    import javax.sound.sampled.DataLine;
    import javax.sound.sampled.SourceDataLine;
    import java.io.InputStream;

    public class Main {
        final Logger log = LoggerFactory.getLogger(Main.class);

        public static void main(String[] args) {
            // New code will go here
        }

        private void playAudioStream(PullAudioOutputStream audio) {
            ActivityAudioStream stream = new ActivityAudioStream(audio);
            final ActivityAudioStream.ActivityAudioFormat audioFormat = stream.getActivityAudioFormat();
            final AudioFormat format = new AudioFormat(
                    AudioFormat.Encoding.PCM_SIGNED,
                    audioFormat.getSamplesPerSecond(),
                    audioFormat.getBitsPerSample(),
                    audioFormat.getChannels(),
                    audioFormat.getFrameSize(),
                    audioFormat.getSamplesPerSecond(),
                    false);
            try {
                int bufferSize = format.getFrameSize();
                final byte[] data = new byte[bufferSize];

                SourceDataLine.Info info = new DataLine.Info(SourceDataLine.class, format);
                SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
                line.open(format);

                if (line != null) {
                    line.start();
                    int nBytesRead = 0;
                    while (nBytesRead != -1) {
                        nBytesRead = stream.read(data);
                        if (nBytesRead != -1) {
                            line.write(data, 0, nBytesRead);
                        }
                    }
                    line.drain();
                    line.stop();
                    line.close();
                }
                stream.close();

            } catch (Exception e) {
                e.printStackTrace();
            }
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

    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities.
    botConnector.activityReceived.addEventListener((o, activityEventArgs) -> {
        final String act = activityEventArgs.getActivity().serialize();
            log.info("Received activity {} audio", activityEventArgs.hasAudio() ? "with" : "without");
            if (activityEventArgs.hasAudio()) {
                playAudioStream(activityEventArgs.getAudio());
            }
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

1. Voor de ondersteuning van antwoord afspelen, voegt u een extra klasse die de PullAudioOutputStream-object geretourneerd door de getAudio() API om een java InputStream voor eenvoudig te verwerken transformeert. Deze ActivityAudioStream is een speciale klasse die wordt verwerkt door audio reactie van het kanaal' directe regel spraak'. Accessors om op te halen van audio-indeling informatie vereist voor het verwerken van afspelen en biedt: Selecteer voor die **bestand** > **nieuw** > **klasse**.

1. In de **nieuwe Java-klasse** venster, geef **speechsdk.quickstart** in de **pakket** veld en **ActivityAudioStream** in de  **Naam** veld.

1. Open het zojuist gemaakte **ActivityAudioStream** klasse en vervangen door de code hieronder.

    ```java
    package com.speechsdk.quickstart;

    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;

    import java.io.IOException;
    import java.io.InputStream;


    public final class ActivityAudioStream extends InputStream {
        /**
         * The number of samples played per second. (16 kHz)
         */
        public static final long SAMPLE_RATE = 16000;
        /**
         * The number of bits in each sample of a sound that has this format. (16 bits)
         */
        public static final int BITS_PER_SECOND = 16;
        /**
         * The number of audio channels in this format (1 for mono).
         */
        public static final int CHANNELS = 1;
        /**
         * The number of bytes in each frame of a sound that has this format (2).
         */
        public static final int FRAME_SIZE = 2;

        /**
         * Reads up to a specified maximum number of bytes of data from the audio
         * stream, putting them into the given byte array.
         *
         * @param b   the buffer into which the data is read
         * @param off the offset, from the beginning of array <code>b</code>, at which
         *            the data will be written
         * @param len the maximum number of bytes to read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b, int off, int len) {
            byte[] tempBuffer = new byte[len];
            int n = (int) this.pullStreamImpl.read(tempBuffer);
            for (int i = 0; i < n; i++) {
                if (off + i > b.length) {
                    throw new ArrayIndexOutOfBoundsException(b.length);
                }
                b[off + i] = tempBuffer[i];
            }
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Reads the next byte of data from the activity audio stream if available.
         *
         * @return the next byte of data, or -1 if the end of the stream is reached
         * @see #read(byte[], int, int)
         * @see #read(byte[])
         * @see #available
         * <p>
         */
        @Override
        public int read() {
            byte[] data = new byte[1];
            int temp = read(data);
            if (temp <= 0) {
                // we have a weird situation if read(byte[]) returns 0!
                return -1;
            }
            return data[0] & 0xFF;
        }

        /**
         * Reads up to a specified maximum number of bytes of data from the activity audio stream
         * putting them into the given byte array.
         *
         * @param b the buffer into which the data is read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b) {
            int n = (int) pullStreamImpl.read(b);
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Skips over and discards a specified number of bytes from this
         * audio input stream.
         *
         * @param n the requested number of bytes to be skipped
         * @return the actual number of bytes skipped
         * @throws IOException if an input or output error occurs
         * @see #read
         * @see #available
         */
        @Override
        public long skip(long n) {
            if (n <= 0) {
                return 0;
            }
            if (n <= Integer.MAX_VALUE) {
                byte[] tempBuffer = new byte[(int) n];
                return read(tempBuffer);
            }
            long count = 0;
            for (long i = n; i > 0; i -= Integer.MAX_VALUE) {
                int size = (int) Math.min(Integer.MAX_VALUE, i);
                byte[] tempBuffer = new byte[size];
                count += read(tempBuffer);
            }
            return count;
        }

        /**
         * Closes this audio input stream and releases any system resources associated
         * with the stream.
         */
        @Override
        public void close() {
            this.pullStreamImpl.close();
        }

        /**
         * Fetch the audio format for the ActivityAudioStream. The ActivityAudioFormat defines the sample rate, bits per sample and the # channels
         *
         * @return instance of the ActivityAudioFormat associated with the stream
         */
        public ActivityAudioStream.ActivityAudioFormat getActivityAudioFormat() {
            return activityAudioFormat;
        }

        /**
         * Returns the maximum number of bytes that can be read (or skipped over) from this
         * audio input stream without blocking.
         *
         * @return the number of bytes that can be read from this audio input stream without blocking.
         * As this implementation does not buffer this will be defaulted to 0
         */
        @Override
        public int available() {
            return 0;
        }

        public ActivityAudioStream(final PullAudioOutputStream stream) {
            pullStreamImpl = stream;
            this.activityAudioFormat = new ActivityAudioStream.ActivityAudioFormat(SAMPLE_RATE, BITS_PER_SECOND, CHANNELS, FRAME_SIZE, AudioEncoding.PCM_SIGNED);
        }

        private PullAudioOutputStream pullStreamImpl;

        private ActivityAudioFormat activityAudioFormat;

        /**
         * ActivityAudioFormat is an internal format which contains metadata regarding the type of arrangement of
         * audio bits in this activity audio stream.
         */
        static class ActivityAudioFormat {

            private long samplesPerSecond;
            private int bitsPerSample;
            private int channels;
            private int frameSize;
            private AudioEncoding encoding;

            public ActivityAudioFormat(long samplesPerSecond, int bitsPerSample, int channels, int frameSize, AudioEncoding encoding) {
                this.samplesPerSecond = samplesPerSecond;
                this.bitsPerSample = bitsPerSample;
                this.channels = channels;
                this.encoding = encoding;
                this.frameSize = frameSize;
            }

            /**
             * Fetch the number of samples played per second for the associated audio stream format.
             *
             * @return the number of samples played per second
             */
            public long getSamplesPerSecond() {
                return samplesPerSecond;
            }

            /**
             * Fetch the number of bits in each sample of a sound that has this audio stream format.
             *
             * @return the number of bits per sample
             */
            public int getBitsPerSample() {
                return bitsPerSample;
            }

            /**
             * Fetch the number of audio channels used by this audio stream format.
             *
             * @return the number of channels
             */
            public int getChannels() {
                return channels;
            }

            /**
             * Fetch the default number of bytes in a frame required by this audio stream format.
             *
             * @return the number of bytes
             */
            public int getFrameSize() {
                return frameSize;
            }

            /**
             * Fetch the audio encoding type associated with this audio stream format.
             *
             * @return the encoding associated
             */
            public AudioEncoding getEncoding() {
                return encoding;
            }
        }

        /**
         * Enum defining the types of audio encoding supported by this stream
         */
        public enum AudioEncoding {
            PCM_SIGNED("PCM_SIGNED");

            String value;

            AudioEncoding(String value) {
                this.value = value;
            }
        }
    }

    ```

1. Wilt u de wijzigingen aan de `ActivityAudioStream` bestand.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

Druk op F11 of selecteer **Uitvoeren** > **Fouten opsporen**.
De console een bericht weergegeven met 'Bijvoorbeeld iets' op dit moment, spreekt u mogelijk een Engelse woordgroep of meer woorden die uw bot begrijpt. Uw stem wordt verzonden naar uw bot via de rechtstreekse regel spraak kanaal waar deze wordt herkend, door uw bot verwerkt en het antwoord wordt geretourneerd als een activiteit. Als uw bot spraak als antwoord resulteert, het geluid wordt afgespeeld terug met behulp van de `AudioPlayer` klasse.

![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-java-jre-08-console-output.png)

## <a name="next-steps"></a>Volgende stappen

Op GitHub vindt u aanvullende voorbeelden, zoals hoe u spraak kunt lezen vanuit een audiobestand.

> [!div class="nextstepaction"]
> [Bekijk Java-voorbeelden op GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Snelstart: Omzetten van spraak, Java (Windows, Linux)](quickstart-translate-speech-java-jre.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
