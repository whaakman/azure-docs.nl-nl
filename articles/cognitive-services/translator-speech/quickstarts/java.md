---
title: Java-Snelstartgids voor Azure cognitieve Services, Microsoft Translator Speech-API | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de Microsoft Translator Speech-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: 447164bd8d786fbfc46dff878b77f11a286bcfb8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344680"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-java"></a>Quick Start voor Microsoft Translator spraak API met Java 
<a name="HOLTop"></a>

Dit artikel laat zien hoe u met de Microsoft Translator Speech-API woorden gesproken wav-bestanden.

## <a name="prerequisites"></a>Vereisten

U moet [JDK 7 of 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) compileren en uitvoeren van deze code. U mag een IDE voor Java gebruiken als u een favoriet hebben, maar worden volstaan met een teksteditor.

U moet de [javax.websocket-api-1.1.jar](http://central.maven.org/maven2/javax/websocket/javax.websocket-api/1.1/) en [tyrus zelfstandige-client 1.9.jar](http://repo1.maven.org/maven2/org/glassfish/tyrus/bundles/tyrus-standalone-client/1.9/) bestanden.

U moet een wav-bestand met de naam 'speak.wav' in dezelfde map als het uitvoerbare bestand dat van de volgende code worden gecompileerd. Dit WAV-bestand moet zich in de standaard PCM, 16-bits, 16kHz mono-indeling. U ontvangt deze een wav-bestand van de [conversieprogramma tekst uitspreken API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Microsoft Translator Speech-API**. U moet een betaald abonnementssleutel van uw [Azure-dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Spraak vertalen

De volgende code vertaalt spraak van één taal.

1. Maak een nieuwe Java-project in uw favoriete IDE.
2. Voeg de code hieronder.
3. Vervang de `key` waarde met een geldige toegangssleutel voor uw abonnement.
4. Voer het programma.

Config.Java:

```java
import java.util.*;

import javax.websocket.ClientEndpointConfig;

public class Config extends ClientEndpointConfig.Configurator {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    static String key = "ENTER KEY HERE";

    @Override
    public void beforeRequest (Map<String,List<String>> headers) {
        headers.put("Ocp-Apim-Subscription-Key", Arrays.asList (key));
    }
}
```

Client.Java:

```java
import java.io.*;
import java.net.*;
import java.util.*;

import javax.websocket.ClientEndpoint;
import javax.websocket.CloseReason;
import javax.websocket.ContainerProvider;
import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.WebSocketContainer;

/* Useful reference links:
    https://docs.oracle.com/javaee/7/api/javax/websocket/Session.html
    https://docs.oracle.com/javaee/7/api/javax/websocket/RemoteEndpoint.Basic.html
    https://docs.oracle.com/javaee/7/api/javax/websocket/OnMessage.html
    http://www.oracle.com/technetwork/articles/java/jsr356-1937161.html
*/

@ClientEndpoint(configurator = Config.class)
public class Client {
    static String host = "wss://dev.microsofttranslator.com";
    static String path = "/speech/translate";
    static String params = "?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa";
    static String uri = host + path + params;

    static String input_path = "speak.wav";
    static String output_path = "speak2.wav";

    Session session = null;

    @OnOpen
    public void onOpen(Session session) {
        this.session = session;
        System.out.println ("Connected.");
        SendAudio ();
    }

    @OnMessage
    public void onTextMessage(String message, Session session){
        System.out.println ("Text message received.");
        System.out.println (message);
    }

/*
Use the following signature to receive the message in parts:
    public void onBinaryMessage(byte[] buffer, boolean last, Session session)
Use the following signature to receive the entire message:
    public void onBinaryMessage(byte[] buffer, Session session)
See:
    https://docs.oracle.com/javaee/7/api/javax/websocket/OnMessage.html
*/
    @OnMessage
    public void onBinaryMessage(byte[] buffer, Session session) {
        try {
            System.out.println ("Binary message received.");
            FileOutputStream stream = new FileOutputStream(output_path);
            stream.write(buffer);
            stream.close();
            System.out.println ("Message contents written to file.");
            Close ();
        } catch (Exception e) {
            e.printStackTrace ();
        }
    }

    @OnError
    public void onError(Session session, Throwable e) {
        System.out.println ("Error message received: " + e.getMessage ());
    }

    @OnClose
    public void myOnClose (CloseReason reason) {
        System.out.println ("Close message received: " + reason.getReasonPhrase());
    }

    public void SendAudio() {
        try {
            File file = new File(input_path);
            FileInputStream stream = new FileInputStream (file);
            byte buffer[] = new byte[(int)file.length()];
            stream.read(buffer);
            stream.close();
            sendMessage (buffer);
        } catch (Exception e) {
            e.printStackTrace ();
        }
    }

    public void sendMessage(byte[] message) {
        try {
            System.out.println ("Sending audio.");
            OutputStream stream = this.session.getBasicRemote().getSendStream();
            stream.write (message);
/* Make sure the audio file is followed by silence.
 * This lets the service know that the audio input is finished. */
            System.out.println ("Sending silence.");
            byte silence[] = new byte[3200000];
            stream.write (silence);
            stream.flush();
        } catch (Exception e) {
            e.printStackTrace ();
        }
    }

    public void Connect () throws Exception {
        try {
            System.out.println("Connecting.");
            WebSocketContainer container = ContainerProvider.getWebSocketContainer();
/* Some code samples show container.connectToServer as returning a Session, but this seems to be false. */
            container.connectToServer(this, new URI(uri));
        } catch (Exception e) {
            e.printStackTrace ();
        }
    }

    public void Close () throws Exception {
        try {
            System.out.println("Closing connection.");
            this.session.close ();
        } catch (Exception e) {
            e.printStackTrace ();
        }
    }
}
```

Speak.Java:

```java
/*
Download javax.websocket-api-1.1.jar from:
    http://central.maven.org/maven2/javax/websocket/javax.websocket-api/1.1/
Download tyrus-standalone-client-1.9.jar from:
    http://repo1.maven.org/maven2/org/glassfish/tyrus/bundles/tyrus-standalone-client/1.9/

Compile and run with:
    javac Config.java -cp .;javax.websocket-api-1.1.jar
    javac Client.java -cp .;javax.websocket-api-1.1.jar;tyrus-standalone-client-1.9.jar
    javac Speak.java
    java -cp .;javax.websocket-api-1.1.jar;tyrus-standalone-client-1.9.jar Speak
*/

public class Speak {
    public static void main(String[] args) {
        try {
            Client client = new Client ();
            client.Connect ();
            // Wait for the reply.
            System.out.println ("Press any key to exit the application at any time.");
            System.in.read();
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

**Spraak antwoord vertalen**

Een geslaagde resultaat is het maken van een bestand met de naam 'speak2.wav'. Het bestand bevat de omzetting van de woorden 'speak.wav' gesproken.

[Terug naar boven](#HOLTop)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Conversieprogramma spraak-zelfstudie](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Zie ook 

[Conversieprogramma spraak-overzicht](../overview.md)
[API-referentiemateriaal](http://docs.microsofttranslator.com/speech-translate.html)
