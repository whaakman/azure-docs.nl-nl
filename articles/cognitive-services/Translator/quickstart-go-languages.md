---
title: 'Snelstartgids: Ondersteunde talen ophalen, Go - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze snelstart haalt u een lijst met ondersteunde talen op voor vertaling, transliteratie en opzoeken in woordenlijsten en voorbeelden met behulp van de Translator Text-API met Go.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: erhopf
ms.openlocfilehash: f750bfb07ee273f7b1d355657bfd3c4808f84ef5
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55891713"
---
# <a name="quickstart-use-the-translator-text-api-to-get-a-list-of-supported-languages-using-go"></a>Snelstartgids: De Translator Text-API gebruiken om een lijst met ondersteunde talen op te halen met Go

In deze snelstartgids leert u hoe u een GET-aanvraag maakt waarmee u een lijst met ondersteunde talen kunt ophalen met behulp van Go en de Translator Text-REST API.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Go](https://golang.org/doc/install)

## <a name="create-a-project-and-import-required-modules"></a>Een project maken en de vereiste modules importeren

Maak een nieuw Go-project met uw favoriete IDE of editor. Kopieer dit codefragment naar uw project in een bestand met de naam `get-languages.go`.

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)
```

## <a name="create-the-main-function"></a>De hoofdfunctie toevoegen

We maken de hoofdfunctie voor de toepassing. U ziet dat deze bestaat uit één regel code. Dat komt doordat we één functie maken voor het ophalen en afdrukken van de lijst met ondersteunde talen voor Translator Text.

Kopieer deze code naar uw project:

```go
func main() {
    /*
     * This calls our getLanguages function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    getLanguages()
}
```

## <a name="create-a-function-to-get-a-list-of-supported-languages"></a>Een functie maken om een lijst van ondersteunde talen op te halen

U gaat nu een functie maken om een lijst van ondersteunde talen op te halen.

```go
func getLanguages() {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Maak nu de URL. De URL wordt gebouwd met behulp van de methoden `Parse()` en `Query()`.

Kopieer deze code naar de functie `getLanguages`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/languages")
q := u.Query()
q.Add("api-version", "3.0")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Meer informatie over eindpunten, routes en aanvraagparameters vindt u in [Translator Text-API 3.0: talen](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

## <a name="build-the-request"></a>De aanvraag maken

Nu u de aanvraagbody als JSON hebt gecodeerd, kunt u de POST-aanvraag maken en de Translator Text-API aanroepen.

```go
// Build the HTTP GET request
req, err := http.NewRequest("GET", u.String(), nil)
if err != nil {
    log.Fatal(err)
}
// Add required headers
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>Het antwoord verwerken en afdrukken

Voeg deze code toe aan de functie `getLanguages` om het JSON-antwoord te decoderen en het resultaat vervolgens op te maken en af te drukken.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Alles samenvoegen

Dat was het. U hebt een eenvoudig programma gemaakt dat we de Translator Text-API zullen noemen. Er is een JSON-antwoord geretourneerd. Het is nu tijd om uw programma uit te voeren:

```console
go run get-languages.go
```

Als u uw code graag wilt vergelijken met de onze, kunt u het volledige voorbeeld vinden op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Voorbeeldantwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
{
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
  },
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
  }
}
```

## <a name="next-steps"></a>Volgende stappen

Verken Go-pakketten voor Cognitive Services-API's uit de [Azure SDK voor Go](https://github.com/Azure/azure-sdk-for-go) op GitHub.

> [!div class="nextstepaction"]
> [Go-pakketten verkennen op GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>Zie ook

Meer informatie over het gebruik van de Translator Text-API voor:

* [Tekst vertalen](quickstart-go-translate.md)
* [Tekst transcriberen](quickstart-go-transliterate.md)
* [Een taal identificeren op basis van de invoer](quickstart-go-detect.md)
* [Alternatieve vertalingen verkrijgen](quickstart-go-dictionary.md)
* [De zinlengte in invoer bepalen](quickstart-go-sentences.md)
