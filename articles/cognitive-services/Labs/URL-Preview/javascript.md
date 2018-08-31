---
title: JavaScript-Snelstartgids voor het Project URL-voorbeeld - Microsoft Cognitive Services | Microsoft Docs
description: 'Voorbeeldscript: snel aan de slag met behulp van de Preview-API voor Bing-URL in de Microsoft Cognitive Services op Azure.'
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: dda6f7c105dfbadc3c22f0c008aa8759fe12fa03
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301349"
---
# <a name="url-preview-in-javascript"></a>URL-voorbeeld in JavaScript 

De volgende toepassing met één pagina JavaScript gebruikt om te maken van een URL-voorbeeld voor de site SwiftKey: https://swiftkey.com/en. 

## <a name="prerequisites"></a>Vereisten

Ophalen van een toegangssleutel voor de gratis proefversie [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-url-preview)

## <a name="code-scenario"></a>Code-scenario
De volgende javascript-voorbeeld bevat een tekstvak invoer object waarin de gebruiker de URL om een voorbeeld van invoeren.  Wanneer de gebruiker klikt op de **Preview** knop, de onclick methode routes naar `getPreview` waar code genereert voor een webaanvraag naar de **UrlPreview** eindpunt.

De code maakt een *XMLHttpRequest*, voegt de *Ocp-Apim-Subscription-Key* -header en -sleutel, en stuurt de aanvraag.  Een asynchrone gebeurtenis-handler voor het verwerken van het antwoord wordt toegevoegd.

Als de reactie is geretourneerd, de handler wijst de JSON-tekst van het antwoord op de `demo` lid op de pagina. Andere antwoord-elementen zijn ingesteld op de volgende leden om weer te geven.

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

![Voorbeeld van de JavaScript-Url-voorbeeld](./media/java-script-demo.png)

## <a name="running-the-application"></a>De toepassing uitvoeren

Voor het uitvoeren van de toepassing:

1. Vervang de `YOUR-SUBSCRIPTION-KEY` waarde met een geldige toegangssleutel voor uw abonnement.
2. Sla de HTML-code en het script als een bestand met de extensie .html.
3. De webpagina wordt weergegeven in een browser uitvoeren.
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
- [Snelstartgids voor C#](csharp.md)
- [Snelstartgids voor Java](java-quickstart.md)
- [Knooppunt-snelstartgids](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
