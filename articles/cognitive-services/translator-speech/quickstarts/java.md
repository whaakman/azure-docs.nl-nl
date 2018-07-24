---
title: Java-Quickstart voor Azure Cognitive Services, Microsoft Translator Speech-API | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met behulp van de Microsoft Translator Speech-API in Microsoft Cognitive Services op Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: b06ee0c37d52f81db1a3bad6907690619002ef7c
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205500"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-java"></a>Snelstartgids voor Microsoft Translator Speech-API met Java 
<a name="HOLTop"></a>

In dit artikel leest u hoe de Microsoft Translator Speech-API gebruiken voor de omzetting van woorden die in een wav-bestand.

## <a name="prerequisites"></a>Vereisten

U moet [JDK 7 of 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) compileren en uitvoeren van deze code. U kunt een Java-IDE gebruiken als u een favoriet hebt, maar worden volstaan met een teksteditor.

U moet de volgende bestanden.
- [javax.websocket-api-1.1.jar (of nieuwer)](https://mvnrepository.com/artifact/javax.websocket/javax.websocket-api)
- [jetty-http-9.4.11.v20180605.jar (of nieuwer)](https://mvnrepository.com/artifact/org.eclipse.jetty/jetty-http)
- [jetty-i/o-9.4.11.v20180605.jar (of nieuwer)](https://mvnrepository.com/artifact/org.eclipse.jetty/jetty-io)
- [jetty-util-9.4.11.v20180605.jar (of nieuwer)](https://mvnrepository.com/artifact/org.eclipse.jetty/jetty-util)
- [websocket-api-9.4.11.v20180605.jar (of nieuwer)](https://mvnrepository.com/artifact/org.eclipse.jetty.websocket/websocket-api)
- [websocket-client-9.4.11.v20180605.jar (of nieuwer)](https://mvnrepository.com/artifact/org.eclipse.jetty.websocket/websocket-client)
- [websocket-gemeenschappelijke-9.4.11.v20180605.jar (of nieuwer)](https://mvnrepository.com/artifact/org.eclipse.jetty.websocket/websocket-common)
- [javax-websocket-client-impl-9.4.11.v20180605.jar (of nieuwer)](https://mvnrepository.com/artifact/org.eclipse.jetty.websocket/javax-websocket-client-impl)
- [jetty-client-9.4.11.v20180605.jar (of nieuwer)](https://mvnrepository.com/artifact/org.eclipse.jetty/jetty-client)

U moet een wav-bestand met de naam 'speak.wav' in dezelfde map als het uitvoerbare bestand dat uit de onderstaande code worden gecompileerd. Dit WAV-bestand moet zich in de standard PCM, 16-bits, 16kHz mono-indeling. U vindt deze een wav-bestand van de [Text to Speech-API](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

Hebt u een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Microsoft Translator Speech-API**. U moet een betaald abonnement-sleutel van uw [Azure-dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Spraak vertalen

De volgende code wordt omgezet in spraak in één taal.

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

import org.eclipse.jetty.client.*;
import org.eclipse.jetty.http.*;
import org.eclipse.jetty.io.*;
import org.eclipse.jetty.util.*;
import org.eclipse.jetty.websocket.api.*;
import org.eclipse.jetty.websocket.client.io.*;
import org.eclipse.jetty.websocket.common.scopes.*;

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
 * This lets the service know that the audio file is finished.
 * At 32 bytes per millisecond, this is 10 seconds of silence. */
            System.out.println ("Sending silence.");
            byte silence[] = new byte[320000];
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
/* The response message might exceed the default max message size of 65536. */
            container.setDefaultMaxBinaryMessageBufferSize(131072);
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
Download javax.websocket-api-1.1.jar (or newer) from:
    http://central.maven.org/maven2/javax/websocket/javax.websocket-api/1.1/
Download jetty-http-9.4.11.v20180605.jar (or newer) from:
    https://mvnrepository.com/artifact/org.eclipse.jetty/jetty-http
Download jetty-io-9.4.11.v20180605.jar (or newer) from:
    https://mvnrepository.com/artifact/org.eclipse.jetty/jetty-io
Download jetty-util-9.4.11.v20180605.jar (or newer) from:
    https://mvnrepository.com/artifact/org.eclipse.jetty/jetty-util
Download websocket-api-9.4.11.v20180605.jar (or newer) from:
    https://mvnrepository.com/artifact/org.eclipse.jetty.websocket/websocket-api
Download websocket-client-9.4.11.v20180605.jar (or newer) from:
    https://mvnrepository.com/artifact/org.eclipse.jetty.websocket/websocket-client
Download websocket-common-9.4.11.v20180605.jar (or newer) from:
    https://mvnrepository.com/artifact/org.eclipse.jetty.websocket/websocket-common
Download javax-websocket-client-impl-9.4.11.v20180605.jar (or newer) from:
    https://mvnrepository.com/artifact/org.eclipse.jetty.websocket/javax-websocket-client-impl
Download jetty-client-9.4.11.v20180605.jar (or newer) from:
    https://mvnrepository.com/artifact/org.eclipse.jetty/jetty-client

Compile and run with:
    javac Config.java -cp .;javax.websocket-api-1.1.jar
    javac Client.java -cp .;javax.websocket-api-1.1.jar;javax-websocket-client-impl-9.4.11.v20180605.jar;websocket-common-9.4.11.v20180605.jar;jetty-util-9.4.11.v20180605.jar;jetty-io-9.4.11.v20180605.jar;websocket-api-9.4.11.v20180605.jar;websocket-client-9.4.11.v20180605.jar;jetty-client-9.4.11.v20180605.jar;jetty-http-9.4.11.v20180605.jar
    javac Speak.java -cp .;javax.websocket-api-1.1.jar;javax-websocket-client-impl-9.4.11.v20180605.jar;websocket-common-9.4.11.v20180605.jar;jetty-util-9.4.11.v20180605.jar;jetty-io-9.4.11.v20180605.jar;websocket-api-9.4.11.v20180605.jar;websocket-client-9.4.11.v20180605.jar;jetty-client-9.4.11.v20180605.jar;jetty-http-9.4.11.v20180605.jar
    java -cp .;javax.websocket-api-1.1.jar;javax-websocket-client-impl-9.4.11.v20180605.jar;websocket-common-9.4.11.v20180605.jar;jetty-util-9.4.11.v20180605.jar;jetty-io-9.4.11.v20180605.jar;websocket-api-9.4.11.v20180605.jar;websocket-client-9.4.11.v20180605.jar;jetty-client-9.4.11.v20180605.jar;jetty-http-9.4.11.v20180605.jar Speak
*/

import java.lang.Thread;

public class Speak {
    public static void main(String[] args) {
        try {
            Client client = new Client ();
            client.Connect ();
            // Wait for the reply.
            Thread.sleep (5000);
            System.out.println ("Press Enter to exit the application at any time.");
            System.in.read();
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

**Spraak antwoord vertalen**

Geslaagd gevolg hiervan is het maken van een bestand met de naam 'speak2.wav'. Het bestand bevat de vertaling van gesproken in "speak.wav" woorden.

[Terug naar boven](#HOLTop)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Translator Speech-zelfstudie](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van Translator Speech](../overview.md)
[API-verwijzing](http://docs.microsofttranslator.com/speech-translate.html)
