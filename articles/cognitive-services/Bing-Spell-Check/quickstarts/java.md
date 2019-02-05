---
title: 'Quickstart: Bing Spellingcontrole-API, Java'
titlesuffix: Azure Cognitive Services
description: Haal informatie en codevoorbeelden op om u te helpen snel aan de slag te gaan met de Bing Spellingcontrole-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: aahi
ms.openlocfilehash: d859d2815f47d753c56002a9484f97149e71f1e4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159245"
---
# <a name="quickstart-for-bing-spell-check-api-with-java"></a>Quickstart voor Bing Spellingcontrole-API met Java 

In dit artikel leest u hoe u de [Bing Spellingcontrole-API](https://azure.microsoft.com/services/cognitive-services/spell-check/) kunt gebruiken in combinatie met Java. De Spellingcontrole-API retourneert een lijst met woorden die niet worden herkend, samen met voorgestelde vervangingen. Normaal gesproken zou u tekst naar deze API sturen en vervolgens de voorgestelde vervangingen in de tekst doorvoeren of deze weergeven aan de gebruikers van uw toepassing, zodat ze zelf kunnen beslissen of de vervangingen moeten worden doorgevoerd. Dit artikel laat zien hoe een aanvraag met de tekst "Hollo, wrld!" wordt verzonden. De voorgestelde vervangingen zijn "Hello" en "world".

## <a name="prerequisites"></a>Vereisten

U hebt [JDK 7 of 8](https://aka.ms/azure-jdks) nodig om deze code te compileren en uit te voeren. U kunt een Java-IDE gebruiken als u daar graag mee werkt, maar een teksteditor volstaat.

U moet een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing Spellingcontrole-API voor Bing versie 7** hebben. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/#lang) is voldoende voor deze quickstart. U hebt de toegangssleutel nodig die wordt verstrekt bij het activeren van uw gratis proefversie of u gebruikt de sleutel van een betaald abonnement vanuit uw Azure-dashboard.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="get-spell-check-results"></a>Resultaten van spellingcontrole ophalen

1. Maak een nieuw Java-project in uw favoriete IDE.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `subscriptionKey` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```java
import java.io.*;
import java.net.*;
import javax.net.ssl.HttpsURLConnection;

public class HelloWorld {

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/spellcheck";

    // NOTE: Replace this example key with a valid subscription key.
    static String key = "ENTER KEY HERE";

    static String mkt = "en-US";
    static String mode = "proof";
    static String text = "Hollo, wrld!";

    public static void check () throws Exception {
        String params = "?mkt=" + mkt + "&mode=" + mode;
        URL url = new URL(host + path + params);
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        connection.setRequestProperty("Content-Length", "" + text.length() + 5);
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", key);
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        wr.writeBytes("text=" + text);
        wr.flush();
        wr.close();

        BufferedReader in = new BufferedReader(
        new InputStreamReader(connection.getInputStream()));
        String line;
        while ((line = in.readLine()) != null) {
            System.out.println(line);
        }
        in.close();
    }

    public static void main(String[] args) {
        try {
            check ();
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

**Antwoord**

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie over Bing Spellingcontrole](../tutorials/spellcheck.md)

## <a name="see-also"></a>Zie ook

- [Overzicht van Bing Spellingcontrole](../proof-text.md)
- [Referentie voor de Bing Spellingcontrole-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
