---
title: 'Quickstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST-API en C#'
titleSuffix: Azure Cognitive Services
description: Leer hoe u een afbeelding uploadt naar de Bing Visual Search-API en inzichten in de afbeelding verkrijgt.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 3/28/2019
ms.author: scottwhi
ms.openlocfilehash: d2f5e87bd6c6780e8504abe1753e90eca5db763a
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58880403"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-c"></a>Quickstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST-API en C#

Deze snelstartgids ziet u hoe u een installatiekopie uploaden naar de Bing visuele zoekopdrachten-API en om de inzichten die het resultaat weer te geven.

## <a name="prerequisites"></a>Vereisten

* Elke versie van [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* De [Json.NET framework](https://www.newtonsoft.com/json), verkrijgbaar is als een NuGet-pakket.
* Als u Linux/MacOS, kunt u deze gebruikt voor een toepassing uitvoeren [Mono](https://www.mono-project.com/).

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Een project maken en initialiseren

1. Maak een nieuwe console-oplossing met de naam BingSearchApisQuickStart in Visual Studio. De volgende naamruimten toevoegen aan de belangrijkste codebestand:

    ```csharp
    using System;
    using System.Text;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    ```

2. Variabelen voor uw abonnementssleutel, het eindpunt en het pad toevoegen aan de installatiekopie die u wilt uploaden:

    ```csharp
        const string accessKey = "<my_subscription_key>";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
        static string imagePath = @"<path_to_image>";
    ```

3. Maken van een methode met de naam `GetImageFileName()` om op te halen van het pad voor uw installatiekopie:
    
    ```csharp
    static string GetImageFileName(string path)
            {
                return new FileInfo(path).Name;
            }
    ```

4. Maak een methode voor het ophalen van de binaire gegevens van de afbeelding:

    ```csharp
    static byte[] GetImageBinary(string path)
    {
        return File.ReadAllBytes(path);
    }
    ```

## <a name="build-the-form-data"></a>De formuliergegevens maken

Als u wilt een lokale installatiekopie uploaden, moet u eerst de gegevens te verzenden naar de API bouwen. De gegevens moet bevatten de `Content-Disposition` header, de `name` parameter moet worden ingesteld op 'afbeelding', en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De inhoud van het formulier bevat de binaire gegevens van de installatiekopie. De grootte van de maximale installatiekopie die u kunt uploaden is 1 MB.

    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

1. Grens tekenreeksen als u wilt opmaken van het formulier postgegevens toevoegen. Grens tekenreeksen bepalen de start, einde en regeleinden tekens voor de gegevens:

    ```csharp
    // Boundary strings for form data in body of POST.
    const string CRLF = "\r\n";
    static string BoundaryTemplate = "batch_{0}";
    static string StartBoundaryTemplate = "--{0}";
    static string EndBoundaryTemplate = "--{0}--";
    ```

2. Gebruik de volgende variabelen parameters toevoegen aan de gegevens van het formulier:

    ```csharp
    const string CONTENT_TYPE_HEADER_PARAMS = "multipart/form-data; boundary={0}";
    const string POST_BODY_DISPOSITION_HEADER = "Content-Disposition: form-data; name=\"image\"; filename=\"{0}\"" + CRLF +CRLF;
    ```

3. Maak een functie met de naam `BuildFormDataStart()` te maken van het begin van de gegevens met behulp van de grens tekenreeksen en het pad voor installatiekopie:
    
    ```csharp
        static string BuildFormDataStart(string boundary, string filename)
        {
            var startBoundary = string.Format(StartBoundaryTemplate, boundary);

            var requestBody = startBoundary + CRLF;
            requestBody += string.Format(POST_BODY_DISPOSITION_HEADER, filename);

            return requestBody;
        }
    ```

4. Maak een functie met de naam `BuildFormDataEnd()` aan het einde van de gegevens met behulp van de tekenreeksen grens maken:
    
    ```csharp
        static string BuildFormDataEnd(string boundary)
        {
            return CRLF + CRLF + string.Format(EndBoundaryTemplate, boundary) + CRLF;
        }
    ```

## <a name="call-the-bing-visual-search-api"></a>De Bing Visual Search-API aanroepen

1. Maak een functie voor het aanroepen van de Bing visuele zoekopdrachten-eindpunt en de JSON-antwoord retourneren. De functie omvat het begin en einde van de gegevens, een bytematrix die de image-gegevens bevat en een `contentType` waarde.

2. Gebruik een `WebRequest` voor het opslaan van de URI, contentType waarde en headers.  

3. Gebruik `request.GetRequestStream()` in uw formulier en image-gegevens schrijven en vervolgens het antwoord ophalen. De functie moet er ongeveer als hieronder wordt weergegeven:
        
    ```csharp
        static string BingImageSearch(string startFormData, string endFormData, byte[] image, string contentTypeValue)
        {
            WebRequest request = HttpWebRequest.Create(uriBase);
            request.ContentType = contentTypeValue;
            request.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            request.Method = "POST";

            // Writes the boundary and Content-Disposition header, then writes
            // the image binary, and finishes by writing the closing boundary.
            using (Stream requestStream = request.GetRequestStream())
            {
                StreamWriter writer = new StreamWriter(requestStream);
                writer.Write(startFormData);
                writer.Flush();
                requestStream.Write(image, 0, image.Length);
                writer.Write(endFormData);
                writer.Flush();
                writer.Close();
            }

            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            return json;
        }
    ```

## <a name="create-the-main-method"></a>Maken van de Main-methode

1. In de `Main` -methode van uw toepassing, de bestandsnaam en binaire gegevens van uw installatiekopie ophalen:

    ```csharp
    var filename = GetImageFileName(imagePath);
    var imageBinary = GetImageBinary(imagePath);
    ```

2. Maak de POST-body door de grens hiervoor te bepalen. Roep vervolgens `startFormData()` en `endFormData` om de formuliergegevens te maken:

    ```csharp
    // Set up POST body.
    var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
    var startFormData = BuildFormDataStart(boundary, filename);
    var endFormData = BuildFormDataEnd(boundary);
    ```

3. Maak de `ContentType` waarde door opmaak `CONTENT_TYPE_HEADER_PARAMS` en de rand van de gegevens van het formulier:

    ```csharp
    var contentTypeHdrValue = string.Format(CONTENT_TYPE_HEADER_PARAMS, boundary);
    ```

4. De API-reactie ophalen door het aanroepen van `BingImageSearch()` en afdrukken van het antwoord:

    ```csharp
    var json = BingImageSearch(startFormData, endFormData, imageBinary, contentTypeHdrValue);
    Console.WriteLine(json);
    Console.WriteLine("enter any key to continue");
    Console.readKey();
    ```

## <a name="using-httpclient"></a>Met behulp van HttpClient

Als u `HttpClient`, kunt u de `MultipartFormDataContent` klasse voor het bouwen van de gegevens. De volgende secties van de code gebruikt ter vervanging van de bijbehorende methoden in het vorige voorbeeld.

Vervang de `Main` methode met deze code:

```csharp
        static void Main()
        {
            try
            {
                Console.OutputEncoding = System.Text.Encoding.UTF8;

                if (accessKey.Length == 32)
                {
                    if (IsImagePathSet(imagePath))
                    {
                        var filename = GetImageFileName(imagePath);
                        Console.WriteLine("Getting image insights for image: " + filename);
                        var imageBinary = GetImageBinary(imagePath);

                        var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
                        var json = BingImageSearch(imageBinary, boundary, uriBase, accessKey);

                        Console.WriteLine("\nJSON Response:\n");
                        Console.WriteLine(JsonPrettyPrint(json));
                    }
                }
                else
                {
                    Console.WriteLine("Invalid Bing Visual Search API subscription key!");
                    Console.WriteLine("Please paste yours into the source code.");
                }

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }
        }
```

Vervang de `BingImageSearch` methode met deze code:

```csharp
        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response.
        /// </summary>
        static string BingImageSearch(byte[] image, string boundary, string uri, string subscriptionKey)
        {
            var requestMessage = new HttpRequestMessage(HttpMethod.Post, uri);
            requestMessage.Headers.Add("Ocp-Apim-Subscription-Key", accessKey);

            var content = new MultipartFormDataContent(boundary);
            content.Add(new ByteArrayContent(image), "image", "myimage");
            requestMessage.Content = content;

            var httpClient = new HttpClient();

            Task<HttpResponseMessage> httpRequest = httpClient.SendAsync(requestMessage, HttpCompletionOption.ResponseContentRead, CancellationToken.None);
            HttpResponseMessage httpResponse = httpRequest.Result;
            HttpStatusCode statusCode = httpResponse.StatusCode;
            HttpContent responseContent = httpResponse.Content;

            string json = null;

            if (responseContent != null)
            {
                Task<String> stringContentsTask = responseContent.ReadAsStringAsync();
                json = stringContentsTask.Result;
            }

            return json;
        }
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een visuele zoekopdrachten één pagina web-app maken](../tutorial-bing-visual-search-single-page-app.md)
