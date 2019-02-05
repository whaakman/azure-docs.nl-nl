---
title: 'Quickstart: Lijst met ondersteunde talen ophalen, Java - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze quickstart haalt u een lijst met ondersteunde talen op voor vertaling, transcriptie en het opzoeken in woordenlijsten met behulp van de Translator Text-API.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 12/03/2018
ms.author: erhopf
ms.openlocfilehash: 937fd58b28a3e64f7f4f9fc4bf52e8280af81136
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226967"
---
# <a name="quickstart-use-the-translator-text-api-to-get-a-list-of-supported-languages-using-java"></a>Snelstart: De Translator Text-API gebruiken om een lijst met ondersteunde talen op te halen met Java

In deze quickstart haalt u een lijst met ondersteunde talen op voor vertaling, transcriptie en het opzoeken in woordenlijsten met behulp van de Translator Text-API.

Voor deze snelstart is een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een Translator Text-resource vereist. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

* [JDK 7 of hoger](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle](https://gradle.org/install/)
* Een Azure-abonnementssleutel voor Translator Text

## <a name="initialize-a-project-with-gradle"></a>Een project initialiseren met Gradle

We beginnen met het maken van een werkmap voor dit project. Voer de volgende opdracht uit vanaf de opdrachtregel (of terminal):

```console
mkdir get-languages-sample
cd get-languages-sample
```

Vervolgens gaat u een Gradle-project initialiseren. Met deze opdracht maakt u essentiële buildbestanden voor Gradle, maar nog belangrijker is dat ook `build.gradle.kts` wordt gemaakt, dat tijdens runtime wordt gebruikt om de toepassing te maken en te configureren. Voer de volgende opdracht uit vanuit uw werkmap:

```console
gradle init --type basic
```

Wanneer u wordt gevraagd om een **DSL** te kiezen, selecteert u **Kotlin**.

## <a name="configure-the-build-file"></a>Het buildbestand configureren

Zoek `build.gradle.kts` en open dit met uw favoriete IDE- of teksteditor. Kopieer het vervolgens in deze buildconfiguratie:

```
plugins {
    java
    application
}
application {
    mainClassName = "GetLanguages"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("com.squareup.okhttp:okhttp:2.5.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Opmerking: dit voorbeeld heeft afhankelijkheden op OkHttp voor HTTP-aanvragen, en Gson voor het verwerken en parseren van JSON. Zie [Nieuwe Gradle-builds maken](https://guides.gradle.org/creating-new-gradle-builds/) voor meer informatie over buildconfiguraties.

## <a name="create-a-java-file"></a>Een Java-bestand maken

We gaan een map maken voor de voorbeeld-app. Voer vanuit uw werkmap de volgende opdracht uit:

```console
mkdir -p src/main/java
```

Maak vervolgens in deze map een bestand met de naam `GetLanguages.java`.

## <a name="import-required-libraries"></a>Vereiste bibliotheken importeren

Open `GetLanguages.java` en voeg deze importinstructies toe:

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;
```

## <a name="define-variables"></a>Variabelen definiëren

U moet eerst een openbare klasse voor uw project maken:

```java
public class GetLanguages {
  // All project code goes here...
}
```

Voeg deze regels toe aan de klasse `GetLanguages`:

```java
String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
String url = "https://api.cognitive.microsofttranslator.com/languages?api-version=3.0";
```

## <a name="create-a-client-and-build-a-request"></a>Een client maken en een aanvraag samenstellen

Voeg deze regel toe aan de klasse `GetLanguages` om `OkHttpClient` te instantiëren:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

Stel vervolgens de GET-aanvraag samen.

```java
// This function performs a GET request.
public String Get() throws IOException {
    Request request = new Request.Builder()
            .url(url).get()
            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey)
            .addHeader("Content-type", "application/json").build();
    Response response = client.newCall(request).execute();
    return response.body().string();
}
```

## <a name="create-a-function-to-parse-the-response"></a>Een functie maken voor het parseren van het antwoord

Met deze eenvoudige functie wordt het JSON-antwoord van de Translator Text-service geparseerd en verfraaid.

```java
// This function prettifies the json response.
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonElement json = parser.parse(json_text);
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="put-it-all-together"></a>Alles samenvoegen

De laatste stap bestaat uit het maken van een aanvraag en het ontvangen van een antwoord. Voeg deze regels toe aan het project:

```java
public static void main(String[] args) {
    try {
        GetLanguages getLanguagesRequest = new GetLanguages();
        String response = getLanguagesRequest.Get();
        System.out.println(prettify(response));
    } catch (Exception e) {
        System.out.println(e);
    }
}
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Nu kunt u de voorbeeld-app gaan uitvoeren. Ga vanaf de opdrachtregel (of terminalsessie) naar de hoofdmap van uw werkmap en voer de volgende opdracht uit:

```console
gradle build
```

## <a name="sample-response"></a>Voorbeeldantwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  },
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de voorbeeldcode voor deze snelstartgids en andere, zoals vertaling en transliteratie, evenals andere Translator Text-voorbeeldprojecten op GitHub.

> [!div class="nextstepaction"]
> [Bekijk Java-voorbeelden op GitHub](https://aka.ms/TranslatorGitHub?type=&language=java)

## <a name="see-also"></a>Zie ook

* [Tekst vertalen](quickstart-java-translate.md)
* [Tekst transcriberen](quickstart-java-transliterate.md)
* [Een taal identificeren op basis van de invoer](quickstart-java-detect.md)
* [Alternatieve vertalingen verkrijgen](quickstart-java-dictionary.md)
* [De zinlengte in invoer bepalen](quickstart-java-sentences.md)
