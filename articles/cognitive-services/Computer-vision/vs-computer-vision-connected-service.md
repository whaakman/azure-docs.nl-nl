---
title: Computer Vision-C#-zelfstudie | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Verbinding maken met Cognitive Services-Computer Vision vanuit een ASP.NET Core-webtoepassing.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: ghogen
ms.openlocfilehash: 76ca1215144a5caa40971e1eda23f6462f7bf27b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665253"
---
# <a name="connecting-to-cognitive-services-computer-vision-api-by-using-connected-services-in-visual-studio"></a>Verbinding maken met Cognitive Services-Computer Vision-API met behulp van Connected Services in Visual Studio

U kunt met behulp van de Computer Vision-API voor Cognitive Services, extraheren van uitgebreide informatie om te categoriseren en verwerken van visuele gegevens en gebruik toezicht op afbeeldingen met machines om uw services te voeren.

In dit artikel en de bijbehorende artikelen bieden details voor het gebruik van de Visual Studio Connected Service-functie voor Cognitive Services-Computer Vision-API. De mogelijkheid is beschikbaar in zowel Visual Studio 2017 15,7 of hoger, met de Cognitive Services-extensie geïnstalleerd.

## <a name="prerequisites"></a>Vereisten

- **Een Azure-abonnement**. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 versie 15,7** met de **webontwikkeling** werkbelasting geïnstalleerd. [Nu downloaden](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-cognitive-services-computer-vision-api"></a>Ondersteuning toegevoegd aan uw project voor Cognitive Services-Computer Vision-API

1. Maak een nieuwe ASP.NET Core web-project. Gebruik de sjabloon leeg project. 

1. In **Solution Explorer**, kiest u **toevoegen** > **Connected Service**.
   De Connected Service-pagina wordt weergegeven met services die u aan uw project toevoegen kunt.

   ![Connected Service menu-item toevoegen](../media/vs-common/Connected-Service-Menu.PNG)

1. Kies in het menu van de beschikbare services, **Cognitive Services-Computer Vision-API**.

   ![Kies de service verbinding maken met](./media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-0.PNG)

   Als u bent aangemeld bij Visual Studio en Azure-abonnement dat is gekoppeld aan uw account hebt, wordt een pagina weergegeven met een vervolgkeuzelijst met uw abonnementen.

   ![Selecteer uw abonnement](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-1.PNG)

1. Selecteer het abonnement dat u wilt gebruiken, en kies vervolgens een naam op voor de Computer Vision-API of de koppeling bewerken naar de automatisch gegenereerde naam wijzigen, kiest u de resourcegroep en de prijscategorie kiezen.

   ![Verbonden servicedetails bewerken](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-2.PNG)

   Volg de koppeling voor meer informatie over de Prijscategorieën.

1. Kies toevoegen om toe te voegen ondersteund voor de Service die zijn verbonden.
   Visual Studio Hiermee wijzigt u het project om toe te voegen van de NuGet-pakketten, vermeldingen in configuratie en andere wijzigingen voor de ondersteuning van een verbinding van de Computer Vision-API. Het venster uitvoer toont het logboek van wat in uw project gebeurt er. U ziet er ongeveer als volgt uit:

   ```output
   [4/26/2018 5:15:31.664 PM] Adding Computer Vision API to the project.
   [4/26/2018 5:15:32.084 PM] Creating new ComputerVision...
   [4/26/2018 5:15:32.153 PM] Creating new Resource Group...
   [4/26/2018 5:15:40.286 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Vision.ComputerVision' version 1.0.2-preview.
   [4/26/2018 5:15:44.117 PM] Retrieving keys...
   [4/26/2018 5:15:45.602 PM] Changing appsettings.json setting: ComputerVisionAPI_ServiceKey=<service key>
   [4/26/2018 5:15:45.606 PM] Changing appsettings.json setting: ComputerVisionAPI_ServiceEndPoint=https://australiaeast.api.cognitive.microsoft.com/vision/v2.0
   [4/26/2018 5:15:45.609 PM] Changing appsettings.json setting: ComputerVisionAPI_Name=WebApplication-Core-ComputerVision_ComputerVisionAPI
   [4/26/2018 5:15:46.747 PM] Successfully added Computer Vision API to the project.
   ```
 
## <a name="use-the-computer-vision-api-to-detect-attributes-of-an-image"></a>De Computer Vision-API gebruiken voor het detecteren van kenmerken van een installatiekopie

1. Voeg de volgende using-instructies in Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   using System.Net.Http;
   using System.Net.Http.Headers;
   ```
 
1. Voegt een veld configuratie toe en een constructor die initialiseert het veld configuratie in de `Startup` klasse om in te schakelen van de configuratie in uw programma.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Toevoegen van een afbeeldingenmap in de wwwroot-map in uw project en een afbeeldingsbestand toevoegen aan de wwwroot-map. Een voorbeeld: u kunt een van de installatiekopieën gebruiken op deze [Computer Vision-API-pagina](https://azure.microsoft.com/services/cognitive-services/computer-vision/). Met de rechtermuisknop op een van de installatiekopieën, opslaan op uw lokale vaste schijf en klik in Solution Explorer met de rechtermuisknop op de afbeeldingenmap en choosee **toevoegen** > **bestaand Item** toe te voegen aan uw project. Uw project ziet er ongeveer als volgt in Solution Explorer: 
  
   ![de afbeeldingenmap met afbeeldingsbestand](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-3.PNG) 

1. Met de rechtermuisknop op het afbeeldingsbestand, kiest u eigenschappen en kies vervolgens **kopiëren indien nieuwer**. 

   ![Kopiëren indien nieuwer](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-5.PNG) 
 
1. Vervang de methode configureren door de volgende code voor toegang tot de Computer Vision-API en testen van een installatiekopie.

   ```csharp
        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            // TODO: Change this to your image's path on your site. 
            string imagePath = @"images/subway.png";

            // Enable static files such as image files. 
            app.UseStaticFiles();

            string visionApiKey = this.configuration["ComputerVisionAPI_ServiceKey"];
            string visionApiEndPoint = this.configuration["ComputerVisionAPI_ServiceEndPoint"];

            HttpClient client = new HttpClient();

            // Request headers.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", visionApiKey);

            // Request parameters. A third optional parameter is "details".
            string requestParameters = "visualFeatures=Categories,Description,Color&language=en";

            // Assemble the URI for the REST API Call.
            string uri = visionApiEndPoint + "/analyze" + "?" + requestParameters;

            HttpResponseMessage response;

            // Request body. Posts an image you've added to your site's images folder. 
            var fileInfo = env.WebRootFileProvider.GetFileInfo(imagePath);
            byte[] byteData = GetImageAsByteArray(fileInfo.PhysicalPath);

            string contentString = string.Empty;
            using (ByteArrayContent content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");

                // Execute the REST API call.
                response = client.PostAsync(uri, content).Result;

                // Get the JSON response.
                contentString = response.Content.ReadAsStringAsync().Result;
            }

            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.Run(async (context) =>
            {
                await context.Response.WriteAsync("<h1>Cognitive Services Demo</h1>");
                await context.Response.WriteAsync($"<p><b>Test Image:</b></p>");
                await context.Response.WriteAsync($"<div><img src=\"" + imagePath + "\" /></div>");
                await context.Response.WriteAsync($"<p><b>Computer Vision API results:</b></p>");
                await context.Response.WriteAsync("<p>");
                await context.Response.WriteAsync(JsonPrettyPrint(contentString));
                await context.Response.WriteAsync("<p>");
            });
        }

   ```
    Hier de code wordt een HTTP-aanvraag met de URI en de installatiekopie als binaire inhoud voor een aanroep van de Computer Vision REST-API.

1. Voeg de helperfuncties GetImageAsByteArray en JsonPrettyPrint toe.

   ```csharp
        /// <summary>
        /// Returns the contents of the specified file as a byte array.
        /// </summary>
        /// <param name="imageFilePath">The image file to read.</param>
        /// <returns>The byte array of the image data.</returns>
        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            string INDENT_STRING = "    ";
            var indent = 0;
            var quoted = false;
            var sb = new StringBuilder();
            for (var i = 0; i < json.Length; i++)
            {
                var ch = json[i];
                switch (ch)
                {
                    case '{':
                    case '[':
                        sb.Append(ch);
                        if (!quoted)
                        {
                            sb.AppendLine();
                        }
                        break;
                    case '}':
                    case ']':
                        if (!quoted)
                        {
                            sb.AppendLine();
                        }
                        sb.Append(ch);
                        break;
                    case '"':
                        sb.Append(ch);
                        bool escaped = false;
                        var index = i;
                        while (index > 0 && json[--index] == '\\')
                            escaped = !escaped;
                        if (!escaped)
                            quoted = !quoted;
                        break;
                    case ',':
                        sb.Append(ch);
                        if (!quoted)
                        {
                            sb.AppendLine();
                        }
                        break;
                    case ':':
                        sb.Append(ch);
                        if (!quoted)
                            sb.Append(" ");
                        break;
                    default:
                        sb.Append(ch);
                        break;
                }
            }
            return sb.ToString();
        }
   ```

1. De web-App uitvoeren en zien wat Computer Vision-API in uw installatiekopie is gevonden.

   ![Computer Vision-API-installatiekopie en opgemaakte resultaten](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-4.PNG)  

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep. Hiermee verwijdert u de cognitive service en gerelateerde bronnen. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de in deze snelstart gebruikte resourcegroep in de zoekresultaten ziet, selecteert u deze.
2. Selecteer **Resourcegroep verwijderen**.
3. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Computer Vision-API door te lezen die de [Computer Vision-API-documentatie](Home.md).
