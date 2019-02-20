---
title: 'Zelfstudie: Verbinding maken met de Text Analytics-service met behulp van Connected Services in Visual Studio'
titleSuffix: Azure Cognitive Services
description: Verbinding leren maken met Text Analytics vanuit een ASP.NET Core-webtoepassing.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: tutorial
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: a37ea5fdad941e1559e470ccb22303a8e3c7ca3d
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243889"
---
# <a name="tutorial-connect-to-the-text-analytics-service-with-connected-services-in-visual-studio"></a>Zelfstudie: Verbinding maken met de Text Analytics-service met behulp van Connected Services in Visual Studio

Met Text Analytics-service kunt u uitgebreide informatie extraheren om visuele gegevens te extraheren en te verwerken, en kunt u afbeeldingen computerondersteund beheren om uw services te cureren.

Dit artikel en de bijbehorende artikelen bieden details voor het gebruik van de Visual Studio Connected Service-functie voor de Text Analytics-Service. De mogelijkheid is beschikbaar in Visual Studio 2017 15.7 of hoger wanneer de Cognitive Services-extensie is geïnstalleerd.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- Visual Studio 2017 versie 15.7, met de workload Web Development geïnstalleerd. [Nu downloaden](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-the-text-analytics-service"></a>Voeg ondersteuning toe aan uw project voor de Text Analytics-Service

1. Maak een nieuw ASP.NET Core-webproject met de naam TextAnalyticsDemo. Gebruik de projectsjabloon webtoepassing (Model-View-Controller) met de standaardinstellingen. Het is belangrijk om het project de naam MyWebApplication te geven zodat de naamruimte overeenkomt wanneer u code hebt gekopieerd in het project.  Het voorbeeld in dit artikel gebruikt MVC, maar u kunt de Text Analytics Connected Service gebruiken met elk type ASP.NET-project.

1. Dubbelklik in **Solution Explorer** op het **Connected Service**-item.
   De Connected Service-pagina wordt weergegeven met services die u aan uw project toevoegen kunt.

   ![Schermafbeelding van Connected Service in Solution Explorer](../media/vs-common/Connected-Services-Solution-Explorer.PNG)

1. Kies in het menu van de beschikbare services, **Sentiment evalueren met Text Analytics**.

   ![Schermafbeelding van het Connected Services-scherm](./media/vs-text-connected-service/Cog-Text-Connected-Service-0.PNG)

   Als u bent aangemeld bij Visual Studio en een Azure-abonnement hebt dat is gekoppeld aan uw account, wordt een pagina weergegeven met een vervolgkeuzelijst met uw abonnementen.

   ![Schermafbeelding van het Text Analytics Connected Services-scherm](media/vs-text-connected-service/Cog-Text-Connected-Service-1.PNG)

1. Selecteer het abonnement dat u wilt gebruiken, en kies een naam voor de Text Analytics-Service, of kies de **bewerken** koppeling om de automatisch gegenereerde naam te wijzigen, een resourcegroep te kiezen en de prijscategorie te kiezen.

   ![Schermafbeelding van de resourcegroep en prijscategorie-velden](media/vs-text-connected-service/Cog-Text-Connected-Service-2.PNG)

   Volg de link voor meer informatie over de prijscategorieën.

1. Kies **Toevoegen** om ondersteuning van Connected Service toe te voegen.
   Visual Studio wijzigt uw project om NuGet-pakketten toe te voegen, configuratie in bestanden toe te voegen en andere wijzigingen voor de ondersteuning van een verbinding met de Text Analytics-Service toe te voegen. Het **Uitvoervenster** laat u het logboek zien met wat er met uw project is gebeurd. Er verschijnt informatie die er ongeveer als volgt uitziet:

   ```output
    [6/1/2018 3:04:02.347 PM] Adding Text Analytics to the project.
    [6/1/2018 3:04:02.906 PM] Creating new Text Analytics...
    [6/1/2018 3:04:06.314 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Language' version 1.0.0-preview...
    [6/1/2018 3:04:56.759 PM] Retrieving keys...
    [6/1/2018 3:04:57.822 PM] Updating appsettings.json setting: 'ServiceKey' = '<service key>'
    [6/1/2018 3:04:57.827 PM] Updating appsettings.json setting: 'ServiceEndPoint' = 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0'
    [6/1/2018 3:04:57.832 PM] Updating appsettings.json setting: 'Name' = 'TextAnalyticsDemo'
    [6/1/2018 3:05:01.840 PM] Successfully added Text Analytics to the project.
    ```
 
## <a name="use-the-text-analytics-service-to-detect-the-language-for-a-text-sample"></a>Gebruik de Text Analytics-Service voor het detecteren van de taal van een tekstvoorbeeld.

1. Voeg het volgende toe in Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Net.Http;
   using System.Net.Http.Headers;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   ```
 
1. Voeg een configuratieveld toe en voeg een constructor toe die het veld initialiseert in de Opstartklasse om de configuratie in uw programma toe te voegen.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Voeg een klassebestand toe in de Controller-map met de naam DemoTextAnalyzeController en vervang de inhoud ervan door de volgende code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Extensions.Configuration;
    using TextAnalyticsDemo.Models;
    
    namespace TextAnalyticsDemo.Controllers
    {
        public class DemoTextAnalyzeController : Controller
        {
            private IConfiguration configuration;
    
            public DemoTextAnalyzeController(IConfiguration configuration)
            {
                this.configuration = configuration;
            }
    
            public IActionResult TextAnalyzeResult(TextAnalyzeModel model)
            {
    
                if (!string.IsNullOrWhiteSpace(model.TextStr))
                {
                    ITextAnalyticsAPI client = this.GetTextAnalyzeClient(new MyHandler());
                    model.AnalyzeResult = client.DetectLanguage(
                        new BatchInput(
                            new List<Input>()
                            {
                                new Input("id",model.TextStr)
                            }));
                }
                return View(model);
            }
    
            [HttpPost("Analyze")]
            public IActionResult Analyze(TextAnalyzeModel model)
            {
                return RedirectToAction("TextAnalyzeResult", model);
            }
    
            // Using the ServiceKey from the configuration file,
            // get an instance of the Text Analytics client.
            private ITextAnalyticsAPI GetTextAnalyzeClient(DelegatingHandler handler)
            {
                string key = configuration.GetSection("CognitiveServices")["TextAnalytics:ServiceKey"];
    
                ITextAnalyticsAPI client = new TextAnalyticsAPI( handlers: handler);
                client.SubscriptionKey = key;
                client.AzureRegion = AzureRegions.Westus;
    
                return client;
            }
        }
    }
    ```
    
    De code bevat de GetTextAnalyzeClient om het client-object op te halen die u kunt gebruiken voor het aanroepen van de Text Analytics API en een aanvraaghandler waarmee DetectLanguage wordt aangeroepen voor de opgegeven tekst.

1. Voeg de MyHandler helperklasse toe die wordt gebruikt door de bovenstaande code.

    ```csharp
        class MyHandler : DelegatingHandler
        {
            protected async override Task<HttpResponseMessage> SendAsync(
            HttpRequestMessage request, CancellationToken cancellationToken)
            {
                // Call the inner handler.
                var response = await base.SendAsync(request, cancellationToken);
                
                return response;
            }
        }
    ```

1. In de map Modellen, voegt u een klasse toe voor het model.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    
    namespace Demo.Models
    {
        public class TextAnalyzeModel
        {
            public string TextStr { get; set; }
    
            public LanguageBatchResult AnalyzeResult { get; set; }
    
            public SentimentBatchResult AnalyzeResult2 { get; set; }
        }
    }
    ```

1. Een weergave om de geanalyseerde tekst weer te geven, de gedetecteerde taal en de score van het betrouwbaarheidsniveau van de analyse. U doet dit door met de rechtermuisknop op de **Weergaven** map te klikken, kies **toevoegen**, klikt u vervolgens **weergave**. Geef in het dialoogvenster dat wordt weergegeven de naam _TextAnalyzeResult_, accepteer de standaardwaarden voor het toevoegen van een nieuw bestand met de naam _TextAnalyzeResult.cshtml_ in de **Weergaven** map en kopieer de volgende inhoud daar in:
    
    ```cshtml
    @using System
    @model Demo.Models.TextAnalyzeModel
    
    @{
        ViewData["Title"] = "TextAnalyzeResult";
    }
    
    <h2>Text Language</h2>
    
    <div class="row">
        <section>
            <form asp-controller="DemoTextAnalyze" asp-action="Analyze" method="POST"
                  class="form-horizontal" enctype="multipart/form-data">
                <table width="90%">
                    <tr>
                        <td>
                            <input type="text" name="TextStr" class="form-control" />
                        </td>
                        <td>
                            <button type="submit" class="btn btn-default">Analyze</button>
                        </td>
                    </tr>
                </table>
            </form>
        </section>
    </div>
    
    <h2>Result</h2>
    <div>
        <dl class="dl-horizontal">
            <dt>
                Text :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.TextStr)
            </dd>
            <dt>
                Language Name :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Name)
            </dd>
            <dt>
                Score :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Score)
            </dd>
        </dl>
    </div>
    <div>
        <hr />
        <p>
            <a asp-controller="Home" asp-action="Index">Return to Index</a>
        </p>
    </div>
    
    ```
 
1. Het voorbeeld bouwen en lokaal uitvoeren. Voer wat tekst in en bekijk welke taal door Text Analytics wordt gedetecteerd.
   
## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep als u deze niet meer nodig hebt. Hiermee verwijdert u de Cognitive Service en gerelateerde resources. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de resourcegroep van de zelfstudie in de zoekresultaten ziet, selecteert u deze.
2. Selecteer **Resourcegroep verwijderen**.
3. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Text Analytics-service kunt u lezen in de [Text Analytics-documentatie](index.yml).
