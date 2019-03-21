---
title: 'Zelfstudie: Face-API C#'
titleSuffix: Azure Cognitive Services
description: Maak een eenvoudige Windows-app die gebruikmaakt van de service Cognitive Services Face-API waarmee gezichtskenmerken kunnen worden herkend.
services: cognitive-services
author: ghogen
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: ghogen
ms.openlocfilehash: aafa115ee1e270b903e958e5b43826dfb4ff8bdb
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57435223"
---
# <a name="connecting-to-cognitive-services-face-api-by-using-connected-services-in-visual-studio"></a>Verbinding maken met de Cognitive Service Face-API met behulp van Connected Services in Visual Studio

Met behulp van de Cognitive Services Face-API, kunt u gezichten in foto's detecteren, analyseren, organiseren en taggen.

Dit artikel en de bijbehorende artikelen bieden details voor het gebruik van de Visual Studio Connected Service-functie voor de Cognitive Services Face-API. De mogelijkheid is beschikbaar in Visual Studio 2017 15.7 of hoger wanneer de Cognitive Services-extensie is geïnstalleerd.

## <a name="prerequisites"></a>Vereisten

- **Een Azure-abonnement**. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 versie 15.7** met de workload **Webontwikkeling** geïnstalleerd. [Nu downloaden](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="create-a-project-and-add-support-for-cognitive-services-face-api"></a>Maak een project en voeg ondersteuning toe voor Cognitive Services Face-API

1. Maak een nieuw ASP.NET Core-webproject. Gebruik de sjabloon Leeg project. 

1. In **Solution Explorer** kiest u **Connected Service** > **Toevoegen**.
   De Connected Service-pagina wordt weergegeven met services die u aan uw project kunt toevoegen.

   ![Voeg een Connected Service menu-item toe](./media/vs-face-connected-service/Connected-Service-Menu.PNG)

1. Kies in het menu van de beschikbare services, **Cognitive Services Face-API**.

   ![Kies een service om verbinding mee te maken](./media/vs-face-connected-service/Cog-Face-Connected-Service-0.PNG)

   Als u bent aangemeld bij Visual Studio en een Azure-abonnement hebt dat is gekoppeld aan uw account, wordt een pagina weergegeven met een vervolgkeuzelijst met uw abonnementen.

   ![Selecteer uw abonnement](media/vs-face-connected-service/Cog-Face-Connected-Service-1.PNG)

1. Selecteer het abonnement dat u wilt gebruiken, en kies een naam voor de Face-API, of kies de Bewerken-koppeling om de automatisch gegenereerde naam te wijzigen, een resourcegroep te kiezen en de prijscategorie te kiezen.

   ![Details van verbonden services bewerken](media/vs-face-connected-service/Cog-Face-Connected-Service-2.PNG)

   Volg de link voor meer informatie over de prijscategorieën.

1. Kies Toevoegen om ondersteuning van Connected Service toe te voegen.
   Visual Studio wijzigt uw project om NuGet-pakketten toe te voegen, configuratie in bestanden toe te voegen en andere wijzigingen voor de ondersteuning van een verbinding met Face-API toe te voegen.

## <a name="use-the-face-api-to-detect-attributes-of-faces-in-an-image"></a>Gebruik de Face-API gebruiken voor het detecteren van kenmerken van gezichten in een afbeelding

1. Voeg het volgende toe in Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   using System.Net.Http;
   using System.Net.Http.Headers;
   ```
 
1. Voeg een configuratieveld toe en voeg een constructor toe die het veld initialiseert in Opstartklasse om de configuratie in uw programma toe te voegen.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Voeg in de wwwroot-map van uw project een afbeeldingenmap toe en voeg een afbeeldingsbestand toe aan de wwwroot-map. Een voorbeeld: u kunt een van de installatiekopieën gebruiken op deze [Face-API-pagina](https://azure.microsoft.com/services/cognitive-services/face/). Klik met de rechtermuisknop op een van de installatiekopieën, opslaan op uw lokale vaste schijf en klik in Solution Explorer met de rechtermuisknop op de afbeeldingenmap en kies **toevoegen** > **bestaand Item** toe te voegen aan uw project. Uw project ziet er ongeveer als volgt uit in Solution Explorer:
 
   ![afbeeldingenmap met afbeeldingsbestand](media/vs-face-connected-service/Cog-Face-Connected-Service-6.PNG)

1. Klik met de rechtermuisknop op het afbeeldingsbestand, kies Eigenschappen en kies vervolgens **Kopiëren indien nieuwer**.

   ![Kopiëren indien nieuwer](media/vs-face-connected-service/Cog-Face-Connected-Service-5.PNG)
 
1. Vervang de configuratiemethode door de volgende code voor toegang tot de Face-API en testen van een installatiekopie. Verander de imagePath-tekenreeks naar het juiste pad en bestandsnaam voor uw gezichtsafbeelding.

   ```csharp
    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            // TODO: Change this to your image's path on your site.
            string imagePath = @"images/face1.png";

            // Enable static files such as image files.
            app.UseStaticFiles();

            string faceApiKey = this.configuration["FaceAPI_ServiceKey"];
            string faceApiEndPoint = this.configuration["FaceAPI_ServiceEndPoint"];

            HttpClient client = new HttpClient();

            // Request headers.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", faceApiKey);

            // Request parameters. A third optional parameter is "details".
            string requestParameters = "returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";

            // Assemble the URI for the REST API Call.
            string uri = faceApiEndPoint + "/detect?" + requestParameters;

            // Request body. Posts an image you've added to your site's images folder.
            var fileInfo = env.WebRootFileProvider.GetFileInfo(imagePath);
            var byteData = GetImageAsByteArray(fileInfo.PhysicalPath);

            string contentStringFace = string.Empty;
            using (ByteArrayContent content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");

                // Execute the REST API call.
                var response = client.PostAsync(uri, content).Result;

                // Get the JSON response.
                contentStringFace = response.Content.ReadAsStringAsync().Result;
            }

            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.Run(async (context) =>
            {
                await context.Response.WriteAsync($"<p><b>Face Image:</b></p>");
                await context.Response.WriteAsync($"<div><img src=\"" + imagePath + "\" /></div>");
                await context.Response.WriteAsync($"<p><b>Face detection API results:</b></p>");
                await context.Response.WriteAsync("<p>");
                await context.Response.WriteAsync(JsonPrettyPrint(contentStringFace));
                await context.Response.WriteAsync("<p>");
            });
        }
   ```
    De code in deze stap creëert een HTTP-aanvraag met een aanroep naar de Face REST-API met behulp van de sleutel die u hebt toegevoegd toen u de connected service hebt toegevoegd.

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

1. De webtoepassing uitvoeren en zien wat Face-API gevonden heeft in de afbeelding.
 
   ![Face-API afbeelding opgemaakte resultaten](media/vs-face-connected-service/Cog-Face-Connected-Service-4.PNG)

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep als u deze niet meer nodig hebt. Hiermee verwijdert u de Cognitive Service en gerelateerde resources. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de in deze snelstart gebruikte resourcegroep in de zoekresultaten ziet, selecteert u deze.
1. Selecteer **Resourcegroep verwijderen**.
1. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Face-API kunt u vinden in de [Face-API-documentatie](Overview.md).
