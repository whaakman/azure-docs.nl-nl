---
title: 'Quickstart: Een web-App waarmee de overweldigende lezer wordt gestart (C#)'
titlesuffix: Azure Cognitive Services
description: In deze snelstartgids een volledig nieuwe WebApp bouwen en de functionaliteit boeiende lezer toevoegen.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 77d95383c801038c256ccb2bf386ddf06048cf78
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311796"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-c"></a>Quickstart: Een web-App waarmee de overweldigende lezer wordt gestart (C#)

De [boeiende lezer](https://www.onenote.com/learningtools) is een liggen ontworpen hulpprogramma dat wordt geïmplementeerd bewezen technieken om te lezen begrip te verbeteren.

In deze Quick Start, een volledig nieuwe WebApp bouwen en de overweldigende lezer integreren met behulp van de overweldigende Reader-SDK. Een volledig werkend voorbeeld van deze snelstartgids is beschikbaar [hier](https://github.com/Microsoft/immersive-reader-sdk/samples/quickstart-csharp).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads)
* Een abonnementssleutel voor boeiende lezer. Download er eentje door [deze instructies](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).

## <a name="create-a-web-app-project"></a>Een web-app-project maken

Maak een nieuw project in Visual Studio, met behulp van de sjabloon ASP.NET Core-webtoepassing met ingebouwde Model-View-Controller.

![Nieuw project](./media/vswebapp.png)

![Nieuwe ASP.NET Core-webtoepassing](./media/vsmvc.png)

## <a name="acquire-an-access-token"></a>Een toegangstoken verkrijgen

U moet uw abonnementssleutel en -eindpunt voor deze stap. U kunt uw abonnementssleutel vinden in de pagina sleutels van uw resource boeiende lezer in Azure portal. U vindt het eindpunt op de pagina overzicht.

Met de rechtermuisknop op het project in de _Solution Explorer_ en kies **Gebruikersgeheimen beheren**. Hiermee opent u een bestand met de naam _secrets.json_. Vervang de inhoud van het bestand door het volgende op en verstrek hierbij uw abonnementssleutel en eindpunt indien van toepassing.

```json
{
  "SubscriptionKey": YOUR_SUBSCRIPTION_KEY,
  "Endpoint": YOUR_ENDPOINT
}
```

Open _Controllers\HomeController.cs_, en vervang de `HomeController` klasse met de volgende code.

```csharp
public class HomeController : Controller
{
    private readonly string SubscriptionKey;
    private readonly string Endpoint;

    public HomeController(Microsoft.Extensions.Configuration.IConfiguration configuration)
    {
        SubscriptionKey = configuration["SubscriptionKey"];
        Endpoint = configuration["Endpoint"];

        if (string.IsNullOrEmpty(Endpoint) || string.IsNullOrEmpty(SubscriptionKey))
        {
            throw new ArgumentNullException("Endpoint or subscriptionKey is null!");
        }
    }

    public IActionResult Index()
    {
        return View();
    }

    [Route("token")]
    public async Task<string> Token()
    {
        return await GetTokenAsync();
    }

    /// <summary>
    /// Exchange your Azure subscription key for an access token
    /// </summary>
    private async Task<string> GetTokenAsync()
    {
        using (var client = new System.Net.Http.HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", SubscriptionKey);
            using (var response = await client.PostAsync(Endpoint, null))
            {
                return await response.Content.ReadAsStringAsync();
            }
        }
    }
}
```

## <a name="add-sample-content"></a>Voorbeeldinhoud toevoegen

Nu gaan we enkele voorbeelden voor inhoud toevoegen aan deze web-app. Open _Views\Home\Index.cshtml_ en vervang de automatisch gegenereerde code door dit voorbeeld:

```html
<h1 id='title'>Geography</h1>
<span id='content'>
    <p>The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers. Landforms are sometimes called physical features. It is important for students to know about the physical geography of Earth. The seasons, the atmosphere and all the natural processes of Earth affect where people are able to live. Geography is one of a combination of factors that people use to decide where they want to live.</p>
</span>

<div class='immersive-reader-button' data-button-style='iconAndText' onclick='launchImmersiveReader()'></div>

@section scripts {
<script type='text/javascript' src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js'></script>
<script type='text/javascript' src='https://code.jquery.com/jquery-3.3.1.min.js'></script>
<script type='text/javascript'>
    function getImmersiveReaderTokenAsync() {
        return new Promise((resolve) => {
            $.ajax({
                url: '/token',
                type: 'GET',
                success: token => {
                    resolve(token);
                }
            });
        });
    }

    async function launchImmersiveReader() {
        const content = {
            title: document.getElementById('title').innerText,
            chunks: [ {
                content: document.getElementById('content').innerText,
                lang: 'en'
            } ]
        };

        const token = await getImmersiveReaderTokenAsync();
        ImmersiveReader.launchAsync(token, content, { uiZIndex: 1000000 });
    }
</script>
}
```

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

Selecteer in de menubalk **fouten opsporen > Foutopsporing starten**, of druk op **F5** om de toepassing te starten.

In de browser weergegeven:

![Voorbeeld-app](./media/quickstart-result.png)

Wanneer u op de knop 'Boeiende lezer' klikt, ziet u de overweldigende lezer geopend waarin de inhoud op de pagina.

![Insluitende lezer](./media/quickstart-immersive-reader.png)

## <a name="next-steps"></a>Volgende stappen

* Weergave de [zelfstudie](./tutorial.md) om te zien wat u kunt doen met de overweldigende Reader-SDK
* Verken de [boeiende lezer SDK](https://github.com/Microsoft/immersive-reader-sdk) en de [boeiende lezer SDK-referentie](./reference.md)