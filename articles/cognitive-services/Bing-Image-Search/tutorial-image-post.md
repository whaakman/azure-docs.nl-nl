---
title: Bing installatiekopie uploaden om inzicht te krijgen | Microsoft Docs
description: Consoletoepassing die gebruikmaakt van de Bing-API voor het zoeken van afbeelding voor het uploaden van een installatiekopie en afbeelding inzichten verkrijgen.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 12/07/2017
ms.author: v-gedod
ms.openlocfilehash: f0bf32a9951527a072fffe464f6b5f50d0f237a2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344433"
---
# <a name="tutorial-bing-image-upload-for-insights"></a>Zelfstudie: Bing installatiekopie uploaden om inzicht te krijgen

De API van Bing installatiekopie Search biedt een `POST` optie voor het uploaden van een installatiekopie en zoek naar informatie die relevant zijn voor de installatiekopie. Deze C#-consoletoepassing uploadt een afbeelding die met behulp van het eindpunt zoeken naar afbeelding voor meer informatie over de installatiekopie.
De resultaten zijn kort JSON-objecten, zoals de volgende:

![[JSON-resultaten]](media/cognitive-services-bing-images-api/jsonResult.jpg)

In deze zelfstudie wordt het volgende uitgelegd:

> [!div class="checklist"]
> * Gebruik de zoekfunctie installatiekopie `/details` eindpunt in een `POST` aanvraag
> * Headers voor de aanvraag opgeven
> * URL-parameters gebruiken om op te geven resultaten
> * Uploaden van de image-gegevens en verzenden de `POST` aanvraag
> * De JSON-resultaten naar de console afdrukken

## <a name="app-components"></a>App-onderdelen

De zelfstudie toepassing bestaat uit drie delen:

> [!div class="checklist"]
> * De eindpuntconfiguratie om op te geven voor het zoeken naar Bing-afbeelding eindpunt en de vereiste headers
> * Installatiekopie-bestand uploaden voor `POST` aanvraag naar het eindpunt
> * Bij het parseren van de JSON-resultaten die de gegevens zijn geretourneerd door de `POST` aanvraag

## <a name="scenario-overview"></a>Overzicht van scenario's
Er zijn [drie zoeken naar afbeelding eindpunten](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-endpoint). De `/details` eindpunt kunt gebruiken een `POST` aanvraag met image-gegevens in de hoofdtekst van de aanvraag.
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
De `modules` URL-parameter volgende `/details?` geeft u op wat voor soort informatie de resultaten bevatten:
* `modules=All`
* `modules=RecognizedEntities` (personen of zichtbaar in de afbeelding plaatsen)

Geef `modules=All` in de `POST` aanvraag voor het ophalen van JSON-tekst met de volgende lijst:
* `bestRepresentativeQuery` -een Bing-query waarmee afbeeldingen vergelijkbaar met de installatiekopie van het geüploade geretourneerd
* `detectedObjects` zoals een selectiekader of hotspots in de afbeelding
* `image` metagegevens
* `imageInsightsToken` -token voor een latere `GET` opgehaald van een aanvraag `RecognizedEntities` (personen of zichtbaar in de afbeelding plaatsen) 
* `imageTags`
* `pagesIncluding` -Webpagina's met de installatiekopie
* `relatedSearches`
* `visuallySimilarImages`

Geef `modules=RecognizedEntities` in de `POST` aanvraag voor het ophalen van alleen de `imageInsightsToken`, die wordt gebruikt in een latere `GET` aanvraag. Het identificeert personen of zichtbaar in de afbeelding wordt geplaatst.

## <a name="webclient-and-headers-for-the-post-request"></a>WebClient en -koppen voor de POST-aanvragen
Maak een `WebClient` object en de headers instellen. Alle aanvragen naar de Bing zoeken-API vereisen een `Ocp-Apim-Subscription-Key`. Een `POST` ook voor de aanvraag voor het uploaden van een installatiekopie van een opgegeven `ContentType: multipart/form-data`.

```
            WebClient client = new WebClient();
            client.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            client.Headers["ContentType"] = "multipart/form-data"; 
```

## <a name="upload-the-image-and-get-results"></a>Upload de installatiekopie en resultaten

De `WebClient` klasse bevat de `UpLoadFile` methode die wordt opgemaakt gegevens voor de `POST` aanvraag. Deze indelingen de `RequestStream` en aanroepen `HttpWebRequest`, een groot aantal complexiteit te vermijden.
Roep `WebClient.UpLoadFile` met de `/details` eindpunt en het installatiekopiebestand te uploaden. Het antwoord is binaire gegevens die eenvoudig is geconverteerd naar JSON. 

De JSON-tekst gebruiken om te initialiseren van een exemplaar van de `SearchResult` structuur (Zie de [broncode toepassing](tutorial-image-post-source.md) voor context).
```        
         const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/details";

        // The image to upload. Replace with your file and path.
        const string imageFile = "ansel-adams-tetons-snake-river.jpg";
            
        byte[] resp = client.UploadFile(uriBase + "?modules=All", imageFile);
        var json = System.Text.Encoding.Default.GetString(resp);

        // Create result object for return
        var searchResult = new SearchResult()
        {
            jsonResult = json,
            relevantHeaders = new Dictionary<String, String>()
        };
```

## <a name="print-the-results"></a>De resultaten afdrukken
De rest van de code parseert het resultaat van JSON en verwerkt deze in de console.

```
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

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
            }

            return sb.ToString().Trim();
        }
```
## <a name="get-request-using-the-imageinsightstoken"></a>Aanvraag met behulp van de ImageInsightsToken ophalen
Gebruik de `ImageInsightsToken` geretourneerd met de resultaten van een `POST`, maak een `GET` verzoek als volgt:
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details?InsightsToken="bcid_A2C4BB81AA2C9EF8E049C5933C546449*ccid_osS7gaos*mid_BF7CC4FC4A882A3C3D56E644685BFF7B8BACEAF2
```
Als er persoonsgegevens personen of plaatsen in de afbeelding, wordt deze aanvraag informatie over deze geretourneerd.
De [snelstartgidsen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search) talrijke codevoorbeelden bevatten.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bing installatiekopie Search API-verwijzingen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

