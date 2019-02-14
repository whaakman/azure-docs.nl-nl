---
title: 'Quickstart: Project URL Preview, JavaScript'
titlesuffix: Azure Cognitive Services
description: Voorbeeldscript om snel aan de slag te gaan met de Bing URL-voorbeeld-API met JavaScript.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 03/16/2018
ms.author: rosh
ms.openlocfilehash: 1f3c37e770c2edd76bd299771648b2de29d42dd9
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873645"
---
# <a name="quickstart-url-preview-in-javascript"></a>Quickstart: URL Preview in JavaScript 

De volgende toepassing met één pagina gebruikt JavaScript om een URL-voorbeeld te maken voor de site SwiftKey: https://swiftkey.com/en. 

## <a name="prerequisites"></a>Vereisten

Vraag een toegangssleutel aan voor de gratis proefversie van [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-url-preview)

## <a name="code-scenario"></a>Codescenario
Het volgende JavaScript-voorbeeld bevat een object voor tekstvakinvoer waarin de gebruiker de URL invoert waarvan een voorbeeld moet worden weergegeven.  Wanneer de gebruiker op de knop **Preview** klikt, zorgt de onclick-methode voor omleiding naar `getPreview` waar code een webaanvraag genereert naar het eindpunt **UrlPreview**.

De code maakt een *XMLHttpRequest*, voegt de header *Ocp-Apim-Subscription-Key* en de sleutel toe, en verstuurt de aanvraag.  Er wordt een asynchrone gebeurtenis-handler toegevoegd voor het verwerken van de respons.

Als de respons goed wordt geretourneerd, wijst de handler de JSON-tekst van de respons toe aan de alinea `demo` op de pagina. Andere responselementen worden ingesteld op de volgende alinea's voor weergave.

**Onbewerkte JSON-respons**

```
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

```

**De actieve demo**

![JavaScript-voorbeeld van Url Preview](./media/java-script-demo.png)

## <a name="running-the-application"></a>De toepassing uitvoeren

De toepassing uitvoeren:

1. Vervang de waarde `YOUR-SUBSCRIPTION-KEY` door een geldige toegangssleutel voor uw abonnement.
2. Sla de HTML-code en het script op in een bestand met de extensie .html.
3. Voer de webpagina uit in een browser.
4. Gebruik de bestaande URL of typ een andere URL in het tekstvak.
5. Klik op de knop **Preview**.

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
- [Snelstart voor C#](csharp.md)
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor Node](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
