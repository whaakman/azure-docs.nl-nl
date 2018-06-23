---
title: e-commerce catalogus toezicht met machine learning en AI met Azure inhoud beheerder | Microsoft Docs
description: Automatisch e-commerce catalogussen met machine learning en AI gemiddeld
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/25/2017
ms.author: sajagtap
ms.openlocfilehash: 5cb93941751c8744a19a5473d13f1e135a589eaa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344532"
---
# <a name="ecommerce-catalog-moderation-with-machine-learning"></a>e-commerce catalogus toezicht met machine learning

In deze zelfstudie wordt informatie over het intelligent machine learning-gebaseerde als e-commerce catalogus toezicht door AI-technologieën machine ondersteund in combinatie met menselijke controle voor een systeem intelligent catalogus implementeren.

![Geclassificeerde productafbeeldingen](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="business-scenario"></a>Bedrijfsscenario

Ondersteunde machine technologieën gebruiken om te classificeren en gemiddelde productafbeeldingen in deze categorieën:

1. Volwassene (bloot)
2. Mooie (suggestieve)
3. Beroemdheden
4. VS-vlaggen
5. Toys
6. Pennen

## <a name="tutorial-steps"></a>Zelfstudie stappen

De zelfstudie leidt u door de volgende stappen uit:

1. Registreren en een team inhoud beheerder maken.
2. Configureer controle labels (labels) voor potentiële beroemdheden en markering inhoud.
3. Inhoud beheerder van de installatiekopie API gebruiken om te scannen op mogelijke inhoud voor volwassenen en mooie.
4. De Computer Vision-API gebruiken om te scannen op mogelijke beroemdheden.
5. De aangepaste visie-service gebruiken om te scannen op mogelijke aanwezigheid van vlaggen.
6. De nuanced scanresultaten voor menselijke revisie en definitieve besluitvorming aanwezig.

## <a name="create-a-team"></a>Een team maken

Raadpleeg de [Quick Start](quick-start.md) pagina aanmelden voor inhoud beheerder en het maken van een team. Opmerking de **Team-ID** van de **referenties** pagina.


## <a name="define-custom-tags"></a>Aangepaste labels definiëren

Raadpleeg de [labels](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) artikel aangepaste labels toevoegen. Naast de ingebouwde **volwassenen** en **mooie** labels, de nieuwe labels kunnen het hulpprogramma controleren om de beschrijvende namen voor de labels weer te geven.

In ons geval definiëren we deze aangepaste labels (**beroemdheden**, **vlag**, **ons**, **toy**, **pen**):

![Aangepaste labels configureren](images/tutorial-ecommerce-tags2.PNG)

## <a name="list-your-api-keys-and-endpoints"></a>Lijst van uw API-sleutels en -eindpunten

1. De zelfstudie maakt gebruik van drie API's en de bijbehorende sleutels en API-eindpunten.
2. Uw API-eindpunten zijn verschillend op basis van uw abonnement regio's en de beheerder controleren Inhoudsteam-ID.

> [!NOTE]
> De zelfstudie is ontworpen voor het abonnement sleutels in de regio's die zichtbaar zijn in de volgende eindpunten te gebruiken. Moet overeenkomen met uw API-sleutels met de regio anders uw sleutels niet met de volgende eindpunten werkt URI's:

         // Your API keys
        public const string ContentModeratorKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string ComputerVisionKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string CustomVisionKey = "XXXXXXXXXXXXXXXXXXXX";

        // Your end points URLs will look different based on your region and Content Moderator Team ID.
        public const string ImageUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessImage/Evaluate";
        public const string ReviewUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0/teams/YOURTEAMID/reviews";
        public const string ComputerVisionUri = "https://westcentralus.api.cognitive.microsoft.com/vision/v1.0";
        public const string CustomVisionUri = "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/Prediction/XXXXXXXXXXXXXXXXXXXX/url";

## <a name="scan-for-adult-and-racy-content"></a>Zoeken naar inhoud voor volwassenen en mooie

1. Deze functie kent een afbeeldings-URL en een matrix met sleutel-waardeparen als parameters.
2. De inhoud beheerder installatiekopie API om op te halen van de scores volwassene en Racy aangeroepen.
3. Als de score groter is dan 0,4 (bereik ligt tussen 0 en 1), wordt de waarde in de **ReviewTags** matrix aan **True**.
4. De **ReviewTags** matrix wordt gebruikt voor het markeren van de bijbehorende tag in het hulpprogramma voor beoordeling.

        public static bool EvaluateAdultRacy(string ImageUrl, ref KeyValuePair[] ReviewTags)
        {
            float AdultScore = 0;
            float RacyScore = 0;

            var File = ImageUrl;
            string Body = $"{{\"DataRepresentation\":\"URL\",\"Value\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(ImageUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // {“answers”:[{“answer”:“Hello”,“questions”:[“Hi”],“score”:100.0}]}
                // Parse the response body. Blocking!
                GetAdultRacyScores(response.Content.ReadAsStringAsync().Result, out AdultScore, out RacyScore);
            }

            ReviewTags[0] = new KeyValuePair();
            ReviewTags[0].Key = "a";
            ReviewTags[0].Value = "false";
            if (AdultScore > 0.4)
            {
                ReviewTags[0].Value = "true";
            }

            ReviewTags[1] = new KeyValuePair();
            ReviewTags[1].Key = "r";
            ReviewTags[1].Value = "false";
            if (RacyScore > 0.3)
            {
                ReviewTags[1].Value = "true";
            }
            return response.IsSuccessStatusCode;
        }

## <a name="scan-for-celebrities"></a>Scannen voor beroemdheden

1. Aanmelden voor een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) van de [Computer Vision-API](https://azure.microsoft.com/services/cognitive-services/computer-vision/).
2. Klik op de **API-sleutel ophalen** knop.
3. De voorwaarden accepteren.
4. Als u wilt aanmelden, kiezen uit de lijst met Internet-accounts beschikbaar.
5. Noteer de API-sleutels op de pagina weergegeven.
    
   ![Computer Vision-API-sleutels](images/tutorial-computer-vision-keys.PNG)
    
6. Raadpleeg de broncode project voor de functie waarmee de installatiekopie met de Computer Vision-API worden gescand.

         public static bool EvaluateComputerVisionTags(string ImageUrl, string ComputerVisionUri, string ComputerVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage Response = CallAPI(ComputerVisionUri, ComputerVisionKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (Response.IsSuccessStatusCode)
            {
                ReviewTags[2] = new KeyValuePair();
                ReviewTags[2].Key = "cb";
                ReviewTags[2].Value = "false";

                ComputerVisionPrediction CVObject = JsonConvert.DeserializeObject<ComputerVisionPrediction>(Response.Content.ReadAsStringAsync().Result);

                if ((CVObject.categories[0].detail != null) && (CVObject.categories[0].detail.celebrities.Count() > 0))
                {                 
                    ReviewTags[2].Value = "true";
                }
            }

            return Response.IsSuccessStatusCode;
        }

## <a name="classify-into-flags-toys-and-pens"></a>Classificeren in vlaggen toys en pennen

1. [Meld u aan](https://www.customvision.ai/account/signin) naar de [Vision-API voor aangepaste preview](https://www.customvision.ai/).
2. Gebruik de [Quick Start](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) voor het bouwen van uw aangepaste classificatie voor het detecteren van de mogelijke aanwezigheid van vlaggen, toys en pennen.
   ![Aangepaste visie Training installatiekopieën](images/tutorial-ecommerce-custom-vision.PNG)
3. [Ophalen van de voorspelling eindpunt-URL](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api) voor uw aangepaste classificatie.
4. Raadpleeg de broncode project voor een overzicht van de functie die uw aangepaste classificatie voorspelling-eindpunt om te scannen van uw installatiekopie aanroept.

        public static bool EvaluateCustomVisionTags(string ImageUrl, string CustomVisionUri, string CustomVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(CustomVisionUri, CustomVisionKey, CallType.POST,
                                                   "Prediction-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // Parse the response body. Blocking!
                SaveCustomVisionTags(response.Content.ReadAsStringAsync().Result, ref ReviewTags);
            }
            return response.IsSuccessStatusCode;
        }       
 
## <a name="reviews-for-human-in-the-loop"></a>Beoordelingen voor human in de lus

1. In de vorige secties, kunt u de binnenkomende afbeeldingen voor volwassenen en mooie (inhoud beheerder), beroemdheden (Computer Vision) en vlaggen (aangepaste visie) gescande.
2. Op basis van onze drempelwaarden overeenkomst voor elke scan, de nuanced gevallen beschikbaar maken voor menselijke controleren in het hulpprogramma voor beoordeling.
        openbare statische bool {CreateReview (tekenreeks ImageUrl, KeyValuePair [] metagegevens)

            ReviewCreationRequest Review = new ReviewCreationRequest();
            Review.Item[0] = new ReviewItem();
            Review.Item[0].Content = ImageUrl;
            Review.Item[0].Metadata = new KeyValuePair[MAXTAGSCOUNT];
            Metadata.CopyTo(Review.Item[0].Metadata, 0);

            //SortReviewItems(ref Review);

            string Body = JsonConvert.SerializeObject(Review.Item);

            HttpResponseMessage response = CallAPI(ReviewUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            return response.IsSuccessStatusCode;
        }

## <a name="submit-batch-of-images"></a>Batch van afbeeldingen verzenden

1. Deze zelfstudie wordt ervan uitgegaan van een map 'C:Test' met een tekstbestand met een lijst met URL's van de installatiekopie.
2. De volgende code controleert op de aanwezigheid van het bestand en leest alle URL's in het geheugen.
            Controleer voor een testmap voor een tekstbestand met de lijst met URL's afbeelding om te scannen var topdir = @"C:\test\"; var Urlsfile = topdir + 'Urls.txt';

            if (!Directory.Exists(topdir))
                return;

            if (!File.Exists(Urlsfile))
            {
                return;
            }

            // Read all image URLs in the file
            var Urls = File.ReadLines(Urlsfile);

## <a name="initiate-all-scans"></a>Alle scans initiëren

1. Deze functie op het hoogste niveau in een lus alle afbeelding-URL's in het tekstbestand dat eerder vermeld.
2. Het scant ze met elke API en als de overeenkomst vertrouwen score binnen onze criteria valt, maakt een beoordeling voor menselijke moderators.
             installatiekopie voor elk URL in het bestand... foreach (var Url in URL's) {/ / Initiatize van een nieuwe revisie labels array ReviewTags = nieuwe KeyValuePair [MAXTAGSCOUNT];

                // Evaluate for potential adult and racy content with Content Moderator API
                EvaluateAdultRacy(Url, ref ReviewTags);

                // Evaluate for potential presence of celebrity (ies) in images with Computer Vision API
                EvaluateComputerVisionTags(Url, ComputerVisionUri, ComputerVisionKey, ref ReviewTags);

                // Evaluate for potential presence of custom categories other than Marijuana
                EvaluateCustomVisionTags(Url, CustomVisionUri, CustomVisionKey, ref ReviewTags);

                // Create review in the Content Moderator review tool
                CreateReview(Url, ReviewTags);
            }

## <a name="license"></a>Licentie

Alle Microsoft cognitieve Services SDK's en voorbeelden worden in licentie gegeven aan de MIT-licentie. Zie voor meer informatie [licentie](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Gedragscode voor ontwikkelaars

Ontwikkelaars die cognitieve Services, waaronder deze clientbibliotheek & voorbeeld verwacht voert u de 'Developer Code van uitvoeren voor Microsoft cognitieve Services', gevonden op http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Volgende stappen

Maken en uitbreiden van de zelfstudie met behulp van de [bronbestanden project](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) op Github.
