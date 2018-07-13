---
title: Face-API-C#-zelfstudie | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Maak een eenvoudige Windows-app die gebruikmaakt van de Cognitive Services Face-API voor het detecteren van functies van gezichten wordt uitgevoerd in een afbeelding.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ghogen
ms.openlocfilehash: b51760f889db27aa25e54582070ee7d3adcf66f8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665239"
---
# <a name="connecting-to-cognitive-services-face-api-by-using-connected-services-in-visual-studio"></a>Verbinding maken met Cognitive Services Face-API met behulp van Connected Services in Visual Studio

Met behulp van de Cognitive Services Face-API, kunt u detecteren, analyseren, organiseren en tag gezichten in foto's.

In dit artikel en de bijbehorende artikelen bieden details voor het gebruik van de Visual Studio Connected Service-functie voor Cognitive Services Face-API. De mogelijkheid is beschikbaar in zowel Visual Studio 2017 15,7 of hoger, met de Cognitive Services-extensie geïnstalleerd.

## <a name="prerequisites"></a>Vereisten

- **Een Azure-abonnement**. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 versie 15,7** met de **webontwikkeling** werkbelasting geïnstalleerd. [Nu downloaden](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="create-a-project-and-add-support-for-cognitive-services-face-api"></a>Maak een project en voeg ondersteuning toe voor Cognitive Services Face-API

1. Maak een nieuwe ASP.NET Core web-project. Gebruik de sjabloon leeg project. 

1. In **Solution Explorer**, kiest u **toevoegen** > **Connected Service**.
   De Connected Service-pagina wordt weergegeven met services die u aan uw project toevoegen kunt.

   ![Connected Service menu-item toevoegen](./media/vs-face-connected-service/Connected-Service-Menu.PNG)

1. Kies in het menu van de beschikbare services, **Cognitive Services Face-API**.

   ![Kies de service verbinding maken met](./media/vs-face-connected-service/Cog-Face-Connected-Service-0.PNG)

   Als u bent aangemeld bij Visual Studio en Azure-abonnement dat is gekoppeld aan uw account hebt, wordt een pagina weergegeven met een vervolgkeuzelijst met uw abonnementen.

   ![Selecteer uw abonnement](media/vs-face-connected-service/Cog-Face-Connected-Service-1.PNG)

1. Selecteer het abonnement dat u wilt gebruiken, en kies vervolgens een naam op voor de Face-API of de koppeling bewerken naar de automatisch gegenereerde naam wijzigen, kiest u de resourcegroep en de prijscategorie kiezen.

   ![Verbonden servicedetails bewerken](media/vs-face-connected-service/Cog-Face-Connected-Service-2.PNG)

   Volg de koppeling voor meer informatie over de Prijscategorieën.

1. Kies toevoegen om toe te voegen ondersteund voor de Service die zijn verbonden.
   Visual Studio Hiermee wijzigt u het project om toe te voegen van de NuGet-pakketten, vermeldingen in configuratie en andere wijzigingen voor de ondersteuning van een verbinding van de Face-API.

## <a name="use-the-face-api-to-detect-attributes-of-faces-in-an-image"></a>De Face-API gebruiken voor het detecteren van kenmerken van gezichten wordt uitgevoerd in een afbeelding

1. Voeg de volgende using-instructies in Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   using System.Net.Http;
   using System.Net.Http.Headers;
   ```
 
1. Voeg een veld voor configuratie en een constructor die initialiseert het veld configuratie in Opstartklasse configuratie inschakelen in uw programma toevoegen.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Toevoegen van een afbeeldingenmap in de wwwroot-map in uw project en een afbeeldingsbestand toevoegen aan de wwwroot-map. Een voorbeeld: u kunt een van de installatiekopieën gebruiken op deze [Face-API-pagina](https://azure.microsoft.com/services/cognitive-services/face/). Klik met de rechtermuisknop op een van de installatiekopieën, opslaan op uw lokale vaste schijf en klik in Solution Explorer met de rechtermuisknop op de afbeeldingenmap en choosee **toevoegen** > **bestaand Item** toe te voegen aan uw project. Uw project ziet er ongeveer als volgt in Solution Explorer:
 
   ![de afbeeldingenmap met afbeeldingsbestand](media/vs-face-connected-service/Cog-Face-Connected-Service-6.PNG)

1. Met de rechtermuisknop op het afbeeldingsbestand, kiest u eigenschappen en kies vervolgens **kopiëren indien nieuwer**.

   ![Kopiëren indien nieuwer](media/vs-face-connected-service/Cog-Face-Connected-Service-5.PNG)
 
1. Vervang de methode configureren door de volgende code voor toegang tot de Face-API en testen van een installatiekopie. De tekenreeks imagePath naar het juiste pad en de bestandsnaam voor de afbeelding voor face wijzigen.

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
    De code in deze stap wordt een HTTP-aanvraag met een aanroep naar de Face REST-API met behulp van de sleutel die u hebt toegevoegd toen u de gekoppelde service hebt toegevoegd.

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

1. De web-App uitvoeren en zien wat Face-API is gevonden in de afbeelding.
 
   ![Afbeelding voor Face-API en opgemaakte resultaten](media/vs-face-connected-service/Cog-Face-Connected-Service-4.PNG)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep. Hiermee verwijdert u de cognitive service en gerelateerde bronnen. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de in deze snelstart gebruikte resourcegroep in de zoekresultaten ziet, selecteert u deze.
1. Selecteer **Resourcegroep verwijderen**.
1. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Face-API door te lezen die de [Face-API-documentatie](Overview.md).
