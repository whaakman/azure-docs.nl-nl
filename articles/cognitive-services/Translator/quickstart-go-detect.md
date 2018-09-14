---
title: 'Translator Text: taal bepalen op basis van tekst met Go | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart bepaalt u de taal van de brontekst met behulp van de Translator Text-API met Go in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: nolachar
ms.openlocfilehash: 29fac1a079455a65cc3d430c3030fed99f5cfce1
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "43769107"
---
# <a name="quickstart-identify-language-from-text-with-go"></a>Snelstart: taal bepalen op basis van tekst met Go

In deze snelstart bepaalt u de taal van de brontekst met behulp van de Translator Text-API.

## <a name="prerequisites"></a>Vereisten

U moet de [Go-distributie](https://golang.org/doc/install) installeren om deze code uit te voeren. De voorbeeldcode maakt alleen gebruik van **kern**bibliotheken. Er zijn daarom geen externe afhankelijkheden.

Als u de Translator Text-API wilt gebruiken, moet u ook een abonnementssleutel hebben. Lees hoe u zich kunt [registreren voor de Translator Text-API](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Detect-aanvraag

Met de volgende code bepaalt u de taal van de brontekst met behulp van de methode [Detect](./reference/v3-0-detect.md).

1. Maak een nieuw Go-project in uw favoriete code-editor.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `subscriptionKey` door een geldige toegangssleutel voor uw abonnement.
4. Sla het bestand op met de extensie .go.
5. Open een opdrachtprompt op een computer waarop Go is geïnstalleerd.
6. Compileer het bestand. Bijvoorbeeld: go build quickstart-detect.go.
7. Voer het bestand uit. Bijvoorbeeld: quickstart-detect.

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
    const uriPath = "/detect?api-version=3.0"

    const uri = uriBase + uriPath

    const text = "Salve, mondo!"

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

## <a name="detect-response"></a>Detect-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "alternatives": [
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "pt",
        "score": 1
      },
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "en",
        "score": 1
      }
    ],
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "language": "it",
    "score": 1
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Verken Go-pakketten voor Cognitive Services-API's uit de [Azure SDK voor Go](https://github.com/Azure/azure-sdk-for-go) in GitHub.

> [!div class="nextstepaction"]
> [Go-pakketten verkennen in GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
