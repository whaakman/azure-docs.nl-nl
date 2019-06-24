---
title: 'Zelfstudie: Start de Insluitende lezer (Node.js)'
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie maakt u een Node.js-toepassing waarmee de overweldigende lezer wordt gestart.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 98b46636be321bfe87c08687600894d0c8ab54db
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311713"
---
# <a name="tutorial-launch-the-immersive-reader-nodejs"></a>Zelfstudie: Start de Insluitende lezer (Node.js)

In de [overzicht](./overview.md), hebt u geleerd over wat de overweldigende lezer is en hoe deze wordt geïmplementeerd bewezen technieken om te lezen begrip verbeteren voor taal cursisten, omvatten van opkomende lezers en studenten met het leren van verschillen. In deze zelfstudie bevat informatie over het maken van een Node.js-webtoepassing waarmee de overweldigende lezer wordt gestart. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Node.js-web-app maken met Express
> * Een toegangstoken verkrijgen
> * Start de overweldigende lezer met voorbeelden voor inhoud
> * Geef de taal van uw inhoud
> * Geef de taal van de interface boeiende lezer
> * Start de overweldigende lezer met math inhoud

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor boeiende lezer. Download er eentje door [deze instructies](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
* [Node.js](https://nodejs.org/) en [Yarn](https://yarnpkg.com)
* Een IDE zoals [Visual Studio Code](https://code.visualstudio.com/)

## <a name="create-a-nodejs-web-app-with-express"></a>Een Node.js-web-app maken met Express

Maak een Node.js-web-app met de `express-generator` hulpprogramma.

```bash
npm install express-generator -g
express --view=pug myapp
cd myapp
```

Installeren van de yarn-afhankelijkheden en afhankelijkheden toevoegen `request` en `dotenv`, die later in de zelfstudie wordt gebruikt.

```bash
yarn
yarn add request
yarn add dotenv
```

## <a name="acquire-an-access-token"></a>Een toegangstoken verkrijgen

Schrijf nu een back-end-API voor het ophalen van een toegangstoken met behulp van de abonnementssleutel van uw. U moet uw abonnementssleutel en -eindpunt voor deze stap. U kunt uw abonnementssleutel vinden in de pagina sleutels van uw resource boeiende lezer in Azure portal. U vindt het eindpunt op de pagina overzicht.

Wanneer u uw abonnementssleutel en het eindpunt hebt, maakt u een nieuw bestand met de naam _.env_, en plak de volgende code in het vervangen van `{YOUR_SUBSCRIPTION_KEY}` en `{YOUR_ENDPOINT}` met uw abonnementssleutel en eindpunt, respectievelijk.

```text
SUBSCRIPTION_KEY={YOUR_SUBSCRIPTION_KEY}
ENDPOINT={YOUR_ENDPOINT}
```

Vergeet niet om door te voeren van dit bestand in broncodebeheer, omdat deze bevat geheimen die niet openbaar worden gemaakt.

Open vervolgens _app.js_ en voeg het volgende toe aan de bovenkant van het bestand. Dit in het knooppunt de abonnementssleutel en het eindpunt als omgevingsvariabelen geladen.

```javascript
require('dotenv').config();
```

Open de _routes\index.js_ bestands- en de volgende importeren aan de bovenkant van het bestand:

```javascript
var request = require('request');
```

Voeg vervolgens de volgende code direct onder die regel. Deze code maakt een API-eindpunt dat een toegangstoken met behulp van de abonnementssleutel van uw opgehaald en retourneert vervolgens dit token.

```javascript
router.get('/token', function(req, res, next) {
  request.post({
    headers: {
        'Ocp-Apim-Subscription-Key': process.env.SUBSCRIPTION_KEY,
        'content-type': 'application/x-www-form-urlencoded'
    },
    url: process.env.ENDPOINT
  },
  function(err, resp, token) {
    return res.send(token);
  });
});
```

Deze API-eindpunt moet worden beveiligd achter een vorm van verificatie (bijvoorbeeld [OAuth](https://oauth.net/2/)); dat werk buiten het bereik van deze zelfstudie is.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Start de overweldigende lezer met voorbeelden voor inhoud

1. Open _views\layout.pug_, en voeg de volgende code onder de `head` labels, voordat de `body` tag. Deze `script` tags laden de [boeiende lezer SDK](https://github.com/Microsoft/immersive-reader-sdk) en jQuery.

    ```pug
    script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js')
    script(src='https://code.jquery.com/jquery-3.3.1.min.js')
    ```

2. Open _views\index.pug_, en vervang de inhoud ervan door de volgende code. Deze code vult de pagina met enkele voorbeelden voor inhoud en voegt een knop waarmee de overweldigende lezer wordt gestart.

    ```pug
    extends layout

    block content
      h2(id='title') Geography
      p(id='content') The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers.
      div(class='immersive-reader-button' data-button-style='iconAndText' data-locale='en-US' onclick='launchImmersiveReader()')
      script.
        function launchImmersiveReader() {
          // First, get a token using our /token endpoint
          $.ajax('/token', { success: token => {
            // Second, grab the content from the page
            const content = {
              title: document.getElementById('title').innerText,
              chunks: [ {
                content: document.getElementById('content').innerText + '\n\n',
                lang: 'en'
              } ]
            };

            // Third, launch the Immersive Reader
            ImmersiveReader.launchAsync(token, content);
          }});
        }
    ```

3. Onze web-app is nu gereed. Start de app door te voeren:

    ```bash
    npm start
    ```

4. Open uw browser en navigeer naar _http://localhost:3000_ . U ziet de bovenstaande inhoud op de pagina. Klik op de **boeiende lezer** knop om te starten van de overweldigende lezer met uw inhoud.

## <a name="specify-the-language-of-your-content"></a>Geef de taal van uw inhoud

De overweldigende lezer biedt ondersteuning voor verschillende talen. U kunt de taal van uw inhoud door de onderstaande stappen te volgen.

1. Open _views\index.pug_ en voeg de volgende code hieronder de `p(id=content)` code die u in de vorige stap hebt toegevoegd. Deze code voegt bepaalde inhoud Spaans inhoud aan de pagina.

    ```pug
    p(id='content-spanish') El estudio de las formas terrestres de la Tierra se llama geografía física. Los accidentes geográficos pueden ser montañas y valles. También pueden ser glaciares, lagos o ríos.
    ```

2. In de JavaScript-code, voeg de volgende boven de aanroep van `ImmersiveReader.launchAsync`. Deze code geeft de Spaans inhoud in de overweldigende lezer.

    ```pug
    content.chunks.push({
      content: document.getElementById('content-spanish').innerText + '\n\n',
      lang: 'es'
    });
    ```

3. Navigeer naar _http://localhost:3000_ opnieuw. U ziet de Spaanse tekst op de pagina, en wanneer u op **boeiende lezer**, wordt het weergegeven in de overweldigende lezer ook.

## <a name="specify-the-language-of-the-immersive-reader-interface"></a>Geef de taal van de interface boeiende lezer

Standaard de taal van de interface boeiende Reader komt overeen met de taalinstellingen van de browser. U kunt ook de taal van de interface boeiende lezer opgeven met de volgende code.

1. In _views\index.pug_, vervangt u de aanroep `ImmersiveReader.launchAsync(token, content)` met de volgende code.

    ```javascript
    const options = {
        uiLang: 'fr',
    }
    ImmersiveReader.launchAsync(token, content, options);
    ```

2. Navigeer naar _http://localhost:3000_ . Wanneer u de overweldigende lezer start, wordt de interface worden weergegeven in het Frans.

## <a name="launch-the-immersive-reader-with-math-content"></a>Start de overweldigende lezer met math inhoud

U kunt math inhoud in de overweldigende lezer opnemen met behulp van [MathML](https://developer.mozilla.org/en-US/docs/Web/MathML).

1. Wijzigen _views\index.pug_ om op te nemen met de volgende code boven de aanroep van `ImmersiveReader.launchAsync`:

    ```javascript
    const mathML = '<math xmlns="http://www.w3.org/1998/Math/MathML" display="block"> \
      <munderover> \
        <mo>∫</mo> \
        <mn>0</mn> \
        <mn>1</mn> \
      </munderover> \
      <mrow> \
        <msup> \
          <mi>x</mi> \
          <mn>2</mn> \
        </msup> \
        <mo>ⅆ</mo> \
        <mi>x</mi> \
      </mrow> \
    </math>';

    content.chunks.push({
      content: mathML,
      mimeType: 'application/mathml+xml'
    });
    ```

2. Navigeer naar _http://localhost:3000_ . Wanneer u de overweldigende lezer en Ga naar de onderkant start, ziet u de formule math.

## <a name="next-steps"></a>Volgende stappen

* Verken de [boeiende lezer SDK](https://github.com/Microsoft/immersive-reader-sdk) en de [boeiende lezer SDK-referentie](./reference.md)
* Bekijk voorbeelden van code op [GitHub](https://github.com/microsoft/immersive-reader-sdk/samples/advanced-csharp)