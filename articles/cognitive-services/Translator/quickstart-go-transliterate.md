---
title: 'Snelstart: Tekstscript converteren,Go - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids converteert u tekst in één taal van het ene script naar het andere met de Translator Text-API met Go.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: erhopf
ms.openlocfilehash: fd41eff65c312c125594bb3251f9c4fe74108eaf
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648357"
---
# <a name="quickstart-transliterate-text-with-the-translator-text-rest-api-go"></a>Snelstart: Transliteratie gebruiken voor tekst met de Translator Text REST API (Go)

In deze snelstartgids converteert u tekst in één taal van het ene schrift naar het andere met de Translator Text-API.

## <a name="prerequisites"></a>Vereisten

U moet de [Go-distributie](https://golang.org/doc/install) installeren om deze code uit te voeren. De voorbeeldcode maakt alleen gebruik van **kern**bibliotheken. Er zijn daarom geen externe afhankelijkheden.

Als u de Translator Text-API wilt gebruiken, moet u ook een abonnementssleutel hebben. Lees hoe u zich kunt [registreren voor de Translator Text-API](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Transliterate-aanvraag

Het volgende converteert tekst in één taal van het ene schrift naar het andere met behulp van de methode [Transliterate](./reference/v3-0-transliterate.md).

1. Maak een nieuw Go-project in uw favoriete code-editor.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `subscriptionKey` door een geldige toegangssleutel voor uw abonnement.
4. Sla het bestand op met de extensie .go.
5. Open een opdrachtprompt op een computer waarop Go is geïnstalleerd.
6. Compileer het bestand. Bijvoorbeeld: go build quickstart-transliterate.go.
7. Voer het bestand uit. Bijvoorbeeld: quickstart-tansliterate.

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    const uriBase = "https://api.cognitive.microsofttranslator.com"
    const uriPath = "/transliterate?api-version=3.0"

    // Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script
    const params = "&language=ja&fromScript=jpan&toScript=latn"

    const uri = uriBase + uriPath + params

    // Transliterate "good afternoon".
    const text = "こんにちは"

    r := strings.NewReader("[{\"Text\" : \"" + text + "\"}]")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.FormatInt(req.ContentLength, 10))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    resp, err := client.Do(req)
    if err != nil {
        fmt.Printf("Error on request: %v\n", err)
        return
    }
    defer resp.Body.Close()

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Printf("Error reading response body: %v\n", err)
        return
    }

    var f interface{}
    json.Unmarshal(body, &f)

    jsonFormatted, err := json.MarshalIndent(f, "", "  ")
    if err != nil {
        fmt.Printf("Error producing JSON: %v\n", err)
        return
    }
    fmt.Println(string(jsonFormatted))
}
```

## <a name="transliterate-response"></a>Transliterate-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "script": "latn",
    "text": "konnnichiha"
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Verken Go-pakketten voor Cognitive Services-API's uit de [Azure SDK voor Go](https://github.com/Azure/azure-sdk-for-go) op GitHub.

> [!div class="nextstepaction"]
> [Go-pakketten verkennen op GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
