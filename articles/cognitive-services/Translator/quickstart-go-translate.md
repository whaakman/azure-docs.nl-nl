---
title: 'Snelstartgids: Tekst vertalen, Go - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids vertaalt u binnen tien minuten tekst vanuit één taal naar een andere taal met de Translator Text-API en Go.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: a1218442aedf94028fe1e08f02f13ce1c0d05008
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56738110"
---
# <a name="quickstart-use-the-translator-text-api-to-translate-a-string-using-go"></a>Snelstartgids: De Translator Text-API gebruiken om een tekenreeks te vertalen met Go

In deze snelstartgids leert u hoe u een tekenreeks van het Engels naar het Italiaans en Duits vertaalt met Go en de Translator Text-REST API.

Voor deze snelstart is een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een Translator Text-resource vereist. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Go](https://golang.org/doc/install)
* Een Azure-abonnementssleutel voor Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Een project maken en de vereiste modules importeren

Maak een nieuw Go-project met uw favoriete IDE of editor. Kopieer dit codefragment naar uw project in een bestand met de naam `translate-text.go`.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>De hoofdfunctie toevoegen

In dit voorbeeld laat u de Translator Text-abonnementssleutel ophalen uit de omgevingsvariabele `TRANSLATOR_TEXT_KEY`. Als u niet bekend bent met omgevingsvariabelen, kunt u `subscriptionKey` als tekenreeks instellen en een opmerking plaatsen in de voorwaardelijke instructie.

Kopieer deze code naar uw project:

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our translate function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    translate(subscriptionKey)
}
```

## <a name="create-a-function-to-translate-text"></a>Een functie maken om tekst te vertalen

U gaat nu een functie maken om tekst te vertalen. Met deze functie wordt één argument gebruikt, uw Translator Text-abonnementssleutel.

```go
func translate(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Maak nu de URL. De URL wordt gebouwd met behulp van de methoden `Parse()` en `Query()`. U ziet dat de parameters worden toegevoegd met de methode `Add()`. In dit voorbeeld vertaalt u van het Engels naar het Italiaans en het Duits: `de` en `it`.

Kopieer deze code naar de functie `translate`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/translate?api-version=3.0")
q := u.Query()
q.Add("to", "de")
q.Add("to", "it")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Meer informatie over eindpunten, routes en aanvraagparameters vindt u in [Translator Text-API 3.0: vertalen](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate).

## <a name="create-a-struct-for-your-request-body"></a>Een structuur maken voor de aanvraagbody

Vervolgens maakt u een anonieme structuur voor de aanvraagbody en codeert u deze met `json.Marshal()` als JSON. Voeg deze code toe aan de functie `translate`.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "Hello, world!"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>De aanvraag maken

Nu u de aanvraagbody als JSON hebt gecodeerd, kunt u de POST-aanvraag maken en de Translator Text-API aanroepen.

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers to the request
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>Het antwoord verwerken en afdrukken

Voeg deze code toe aan de functie `translate` om het JSON-antwoord te decoderen en het resultaat vervolgens op te maken en af te drukken.

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
go run translate-text.go
```

Als u uw code graag wilt vergelijken met de onze, kunt u het volledige voorbeeld vinden op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Voorbeeldantwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Verken Go-pakketten voor Cognitive Services-API's uit de [Azure SDK voor Go](https://github.com/Azure/azure-sdk-for-go) op GitHub.

> [!div class="nextstepaction"]
> [Go-pakketten verkennen op GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>Zie ook

Meer informatie over het gebruik van de Translator Text-API voor:

* [Tekst transcriberen](quickstart-go-transliterate.md)
* [Een taal identificeren op basis van de invoer](quickstart-go-detect.md)
* [Alternatieve vertalingen verkrijgen](quickstart-go-dictionary.md)
* [Een lijst ophalen van ondersteunde talen](quickstart-go-languages.md)
* [De zinlengte in invoer bepalen](quickstart-go-sentences.md)
