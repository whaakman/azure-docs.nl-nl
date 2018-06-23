---
title: Snelstartgids voor Azure cognitieve Services, Bing Web Search API gaat | Microsoft Docs
description: Snel aan de slag met Ga taal query uitvoeren op de Bing zoeken-API voor Web in cognitieve Microsoft-Services in Azure.
services: cognitive-services
author: Nhoya
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 03/09/2018
ms.author: rosh
ms.reviewer: nhoyadx@gmail.com, v-gedod
ms.openlocfilehash: 86cb67d46bca40c83c2f175ab7fdf6fbf52cb02f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344846"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-go"></a>Oproep- en -antwoord: uw eerste Bing Web-zoekopdracht in Ga

De Bing Web zoeken-API is vergelijkbaar met Bing.com, zoekresultaten relevant zijn voor de gebruiker query retourneren. De resultaten kunnen webpagina's, afbeeldingen, video's, nieuws en entiteiten, samen met verwante zoekquery's, correcties tijdzones, eenheidconversie, vertalingen en berekeningen bevatten. De resultaten zijn gebaseerd op hun relevantie en de laag van de Bing zoeken-API's waarop u bent geabonneerd.

Raadpleeg de [API-referentiemateriaal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) voor meer informatie over de API's.

## <a name="prerequisites"></a>Vereisten
U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing zoeken-API's**. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.

Installeer de [gaat u de binaire bestanden](https://golang.org/dl/).
 
In deze zelfstudie wordt alleen **core** bibliotheken, dus er geen externe afhankelijkheden zijn.

## <a name="core-libraries"></a>Core-bibliotheken

Gebruik `http` de aanvraag verzenden naar het eindpunt `ioutil` lezen van het antwoord `time` en `json` voor het afhandelen van de json en `fmt` afdrukken van de uitvoer

```
package main

import (
    "fmt"
    "net/http"
    "io/iutil"
    "time"
    "encoding/json"
)
```

## <a name="define-variables"></a>Variabelen definiëren
Eindpunt van de API en de zoekterm instellen.

```
//This is the valid endpoint at the time of the writing
const endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/search"
//API token
token := "YOUR-ACCESS-KEY"
searchTerm := "Microsoft Cognitive Services"
```

## <a name="building-and-sending-the-request"></a>Opbouwen en verzenden van de aanvraag

```
//Declare a new GET request
req, err := http.NewRequest("GET", endpoint, nil)
if err != nil {
    panic(err)
}
//Add the payload to the request
param := req.URL.Query()
param.Add("q", searchTerm)
//Encoding the payload
req.URL.RawQuery = param.Encode()

//Insert the API token in the request header
req.Header.Add("Ocp-Apim-Subscription-Key", token)

//create new client
client := new(http.Client)
//Send the request
resp, err := client.Do(req)
if err != nil {
    panic(err)
}
//Close the response body at the function exit
defer resp.Body.Close() 
```

## <a name="printing-the-answer"></a>Het antwoord afdrukken
Het zoekresultaat is binnen de `resp` variabele. De hoofdtekst van het antwoord van de variabele afdrukken.

```
body, err := ioutil.ReadAll(resp.Body)
if err != nil {
    panic(err)
}
//Convert body from byte to string.
fmt.Println(string(body))
```

## <a name="put-everything-together"></a>Alles samenstellen

```
package main
import (
    "fmt"
    "net/http"
    "io/ioutil"
    "time"
    "encoding/json"
)

//The struct that will contain the answer
type BingAnswer struct {
        Type         string `json:"_type"`
        QueryContext struct {
                OriginalQuery string `json:"originalQuery"`
        } `json:"queryContext"`
        WebPages struct {
                WebSearchURL          string `json:"webSearchUrl"`
                TotalEstimatedMatches int    `json:"totalEstimatedMatches"`
                Value                 []struct {
                        ID               string    `json:"id"`
                        Name             string    `json:"name"`
                        URL              string    `json:"url"`
                        IsFamilyFriendly bool      `json:"isFamilyFriendly"`
                        DisplayURL       string    `json:"displayUrl"`
                        Snippet          string    `json:"snippet"`
                        DateLastCrawled  time.Time `json:"dateLastCrawled"`
                        SearchTags       []struct {
                                Name    string `json:"name"`
                                Content string `json:"content"`
                        } `json:"searchTags,omitempty"`
                        About []struct {
                                Name string `json:"name"`
                        } `json:"about,omitempty"`
                } `json:"value"`
        } `json:"webPages"`
        RelatedSearches struct {
                ID    string `json:"id"`
                Value []struct {
                        Text         string `json:"text"`
                        DisplayText  string `json:"displayText"`
                        WebSearchURL string `json:"webSearchUrl"`
                } `json:"value"`
        } `json:"relatedSearches"`
        RankingResponse struct {
                Mainline struct {
                        Items []struct {
                                AnswerType  string `json:"answerType"`
                                ResultIndex int    `json:"resultIndex"`
                                Value       struct {
                                        ID string `json:"id"`
                                } `json:"value"`
                        } `json:"items"`
                } `json:"mainline"`
                Sidebar struct {
                        Items []struct {
                                AnswerType string `json:"answerType"`
                                Value      struct {
                                        ID string `json:"id"`
                                } `json:"value"`
                        } `json:"items"`
                } `json:"sidebar"`
        } `json:"rankingResponse"`
}

func main() {
    const endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/search"
    token := "YOUR-ACCESS-KEY"
    searchTerm := "Microsoft Cognitive Services"
    
    req, err := http.NewRequest("GET", endpoint, nil)
    if err != nil {
        panic(err)
    }
    
    param := req.URL.Query()
    param.Add("q", searchTerm)
    req.URL.RawQuery = param.Encode()

    req.Header.Add("Ocp-Apim-Subscription-Key", token)
    
    client := new(http.Client)
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close() 
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        panic(err)
    }
    //creating a new answer struct
    ans := new(BingAnswer)
    err = json.Unmarshal(body, &ans)
    if err != nil {
         fmt.Println(err)
    }
    //Iterate over search results
    for _, result := range ans.WebPages.Value {
         //Printing result name and URL
         fmt.Println(result.Name, "||", result.URL)
    }

}
```

## <a name="next-steps"></a>Volgende stappen

[Overzicht van Bing webpagina's zoeken](../overview.md)  
[Probeer deze](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Verkrijgen van een gratis proefversie toegangssleutel](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)
[Bing Web Search API-verwijzingen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)

