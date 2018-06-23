---
title: JavaScript Quick Start voor het voorbeeld in de Project-URL - cognitieve Microsoft-Services | Microsoft Docs
description: Het voorbeeldscript snel aan de slag met de Preview-API van Bing-URL in cognitieve Microsoft-Services in Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 9041a88a292fb2dabead69195ebc3074e2ecf486
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345288"
---
# <a name="url-preview-in-javascript"></a>Voorbeeld van de URL in JavaScript 

De volgende toepassing met één pagina gebruikt JavaScript voor het maken van een voorbeeld-URL voor de site SwiftKey: https://swiftkey.com/en. 

## <a name="prerequisites"></a>Vereisten

Ophalen van een toegangssleutel voor de gratis proefversie [cognitieve Services Labs](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Code-scenario
De volgende javascript-voorbeeld bevat een tekstvak voor de invoer object waarin de gebruiker invoeren in de URL voor de preview.  Wanneer de gebruiker klikt op de **Preview** knop, de routes van de methode onclick naar `getPreview` waar code genereert voor een webaanvraag voor de **UrlPreview** eindpunt.

De code maakt een *XMLHttpRequest*, voegt de *Ocp-Apim-Subscription-Key* header en -sleutel, en stuurt de aanvraag.  Een asynchrone gebeurtenis-handler voor het verwerken van het antwoord wordt toegevoegd.

Als het antwoord is geretourneerd, de handler wijst de JSON-tekst van het antwoord op de `demo` lid op de pagina. Andere elementen antwoord zijn ingesteld op de volgende leden om weer te geven.

**Onbewerkte JSON-antwoord**

````
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile typing",
  "url": "https://swiftkey.com/en",
  "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

````

**De actieve demo**

![Voorbeeld van de Preview van JavaScript-Url](./media/java-script-demo.png)

## <a name="running-the-application"></a>De toepassing uitvoeren

De toepassing uitvoeren:

1. Vervang de `YOUR-SUBSCRIPTION-KEY` waarde met een geldige toegangssleutel voor uw abonnement.
2. Sla de HTML- en het script naar een bestand met extensie .html.
3. De webpagina worden uitgevoerd in een browser.
4. Gebruik de bestaande URL of een andere invoeren in het tekstvak.
5. Klik op de **Preview** knop.

**Broncode:**

```
<!DOCTYPE html>

<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="utf-8" />
    <title>urlPreview Demo</title>
</head>
<body>
    <h3>URL Preview Demo</h3>

    Page to preview: <input type="url" id="myURL" value="https://swiftkey.com/en">
    <button onclick="getPreview()">Preview</button>

    <p id="demo"></p>
    <br />
    <p id="jsonDesc"></p>
    <p><a id="familyFriendly"></a></p>
    <a id="contentUrl"></a>
    <p />
    <img id="jsonImage" />

    <script>

        var BING_ENDPOINT = "https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search"; 
        var key = "YOUR-SUBSCRIPTION-KEY";
        var xhr;

        function getPreview() {
            xhr = new XMLHttpRequest();

            var queryUrl = BING_ENDPOINT + "?q=" +
                encodeURIComponent(document.getElementById("myURL").value);
            xhr.open('GET', queryUrl, true);
            xhr.setRequestHeader("Ocp-Apim-Subscription-Key", key);

            xhr.send();

            xhr.addEventListener("readystatechange", processRequest, false);
        }

        function processRequest(e) {

            if (xhr.readyState == 4 && xhr.status == 200) {
                document.getElementById("demo").innerHTML = xhr.responseText;
                var obj = JSON.parse(xhr.responseText);
                document.getElementById("jsonDesc").innerHTML = obj.description;
                document.getElementById("familyFriendly").innerHTML = "Family Friendly: " + obj.isFamilyFriendly;
                document.getElementById("contentUrl").innerHTML = obj.url;
                document.getElementById("contentUrl").href = obj.url;
                document.getElementById("jsonImage").width = 350;
                document.getElementById("jsonImage").src = obj.primaryImageOfPage.contentUrl;

            }

        }

    </script>

</body>
</html>

```

## <a name="next-steps"></a>Volgende stappen
- [Quick Start C#](csharp.md)
- [Java-Quick Start](java-quickstart.md)
- [Knooppunt Quick Start](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)