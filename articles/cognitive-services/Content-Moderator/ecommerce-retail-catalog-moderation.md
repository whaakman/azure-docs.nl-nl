---
title: 'Zelfstudie: e-commerce-catalogus beheer - Content Moderator'
titlesuffix: Azure Cognitive Services
description: E-commerce-catalogi automatisch modereren met machine learning en AI.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: sajagtap
ms.openlocfilehash: 285590435a7e3c31d45d5d154d4e430ed3252838
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53256227"
---
# <a name="tutorial-ecommerce-catalog-moderation-with-machine-learning"></a>Zelfstudie: e-commerce-catalogus modereren met machine learning

In deze zelfstudie leren we hoe we intelligente op machine learning gebaseerde moderatie van e-commercecatalogi kunnen implementeren door AI-technologieën te combineren met menselijke moderatie om een ​​intelligent catalogussysteem te bieden.

![Geclassificeerde productafbeeldingen](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="business-scenario"></a>Bedrijfsscenario

Door machines ondersteunde technologieën gebruiken om productafbeeldingen in de volgende categorieën te classificeren en modereren:

1. Voor volwassenen (naakt)
2. Ongepast (suggestief)
3. Beroemdheden
4. Amerikaanse vlaggen
5. Speelgoed
6. Pennen

## <a name="tutorial-steps"></a>Stappen in de zelfstudie

In deze zelfstudie gaat u de volgende stappen uitvoeren:

1. Aanmelden en een Content Moderator-team samenstellen.
2. Moderatietags (labels) voor mogelijke beroemdheden en vlaginhoud configureren.
3. De Image-API van Content Moderator gebruiken om te scannen op mogelijke ongepaste inhoud of inhoud voor volwassenen.
4. De Computer Vision-API gebruiken om te scannen op mogelijke beroemdheden.
5. De Custom Vision service gebruiken om te scannen op mogelijke aanwezigheid van vlaggen.
6. De genuanceerde scanresultaten presenteren voor menselijke beoordeling en uiteindelijke besluitvorming.

## <a name="create-a-team"></a>Een team maken

Raadpleeg de [snelstart](quick-start.md)-pagina voor informatie over hoe u zich aanmeldt voor Content Moderator en een team samenstelt. Noteer de **Team-id** op de pagina **Referenties**.


## <a name="define-custom-tags"></a>Aangepaste tags definiëren

Raadpleeg het artikel [Tags](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) voor het toevoegen van aangepaste tags. Naast de ingebouwde tags **adult** (voor volwassenen) en **racy** (ongepast), bieden de nieuwe tags de het beoordelingsprogramma de mogelijkheid om de beschrijvende namen voor de tags weer te geven.

In ons geval definiëren we de aangepaste tags (**celebrity** (beroemdheid), **flag** (vlag), **us** (VS), **software**, **pen**):

![Aangepaste tags configureren](images/tutorial-ecommerce-tags2.PNG)

## <a name="list-your-api-keys-and-endpoints"></a>Uw API-sleutels en eindpunten weergeven

1. De zelfstudie gebruikt drie API's en de bijbehorende sleutels en API-eindpunten.
2. Uw API-eindpunten zijn verschillend op basis van uw abonnementsregio's en de team-id van het Content Moderator-beoordelingsteam.

> [!NOTE]
> De zelfstudie is ontworpen om abonnementssleutels te gebruiken in de regio's die zichtbaar zijn in de volgende eindpunten. Zorg ervoor dat uw API-sleutels overeenkomen met de regio-URI’s, anders werken uw sleutels mogelijk niet met de volgende eindpunten:

         // Your API keys
        public const string ContentModeratorKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string ComputerVisionKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string CustomVisionKey = "XXXXXXXXXXXXXXXXXXXX";

        // Your end points URLs will look different based on your region and Content Moderator Team ID.
        public const string ImageUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessImage/Evaluate";
        public const string ReviewUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0/teams/YOURTEAMID/reviews";
        public const string ComputerVisionUri = "https://westcentralus.api.cognitive.microsoft.com/vision/v1.0";
        public const string CustomVisionUri = "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/Prediction/XXXXXXXXXXXXXXXXXXXX/url";

## <a name="scan-for-adult-and-racy-content"></a>Scannen op inhoud voor volwassenen en ongepaste inhoud

1. De functie gebruikt een afbeeldings-URL en een matrix van sleutel-waardeparen als parameters.
2. De Image API van de Content Moderator wordt aangeroepen om de scores voor Adult en Racy te krijgen.
3. Als de score hoger is dan 0,4 (bereik ligt tussen 0 en 1), wordt de waarde in de **ReviewTags**-matrix ingesteld op **true** (waar).
4. De **ReviewTags**-matrix wordt gebruikt om de overeenkomstige tag in het beoordelingsprogramma te markeren.

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

## <a name="scan-for-celebrities"></a>Scannen op beroemdheden

1. Registreer u voor een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) van de [Computer Vision API](https://azure.microsoft.com/services/cognitive-services/computer-vision/).
2. Klik op de knop **API-sleutel ophalen**.
3. Accepteer de voorwaarden.
4. Om u aan te melden, maakt u een keuze uit de lijst met beschikbare internetaccounts.
5. Let op de API-sleutels die worden weergegeven op uw servicepagina.
    
   ![Computer Vision API-sleutels](images/tutorial-computer-vision-keys.PNG)
    
6. Raadpleeg de projectbroncode voor de functie die de afbeelding scant met de Computer Vision API.

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

## <a name="classify-into-flags-toys-and-pens"></a>Classificeren in vlaggen, speelgoed en pennen

1. [Meld u aan](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/) bij de [Custom Vision API-preview](https://www.customvision.ai/).
2. Gebruik de [snelstart](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) om uw aangepaste classificatie voor het detecteren van de mogelijke aanwezigheid van vlaggen, speelgoed en pennen te bouwen.
   ![Custom Vision Training Images](images/tutorial-ecommerce-custom-vision.PNG)
3. [Haal de eindpunt-URL van de voorspelling op](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api) voor uw aangepaste classificatie.
4. Raadpleeg de broncode van het project om de functie te zien waarmee het voorspellingseindpunt van uw aangepaste classificatie wordt aangeroepen om uw afbeelding te scannen.

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
 
## <a name="reviews-for-human-in-the-loop"></a>Beoordelingen voor persoon in de lus

1. In de vorige paragrafen hebt u de binnenkomende afbeeldingen gescand op inhoud voor volwassen en ongepaste inhoud (Content Moderator), beroemdheden (Computer Vision) en vlaggen (Custom Vision).
2. Maak op basis van onze matchdrempels voor elke scan de genuanceerde gevallen beschikbaar voor menselijke beoordeling in het beoordelingsprogramma.
        public static bool CreateReview(string ImageUrl, KeyValuePair[] Metadata) {

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

## <a name="submit-batch-of-images"></a>Batch met afbeeldingen verzenden

1. In deze zelfstudie wordt ervan uitgegaan dat er een map 'C:Test' bestaat die een tekstbestand met een lijst met afbeeldings-URL's bevat.
2. De volgende code controleert het bestaan ​​van het bestand en leest alle URL's naar het geheugen.
            // Controleer een testdirectory op een tekstbestand met de lijst met afbeeldings-URL's om te scannen var topdir = @"C:\test\"; var Urlsfile = topdir + "Urls.txt";

            if (!Directory.Exists(topdir))
                return;

            if (!File.Exists(Urlsfile))
            {
                return;
            }

            // Read all image URLs in the file
            var Urls = File.ReadLines(Urlsfile);

## <a name="initiate-all-scans"></a>Alle scans initiëren

1. Deze functie op het hoogste niveau doorloopt alle afbeeldings-URL's in het tekstbestand dat we eerder noemden.
2. Ze worden met elke API gescand, en als de betrouwbaarheidsscore van de overeenkomst binnen onze criteria valt, wordt een beoordeling gemaakt voor menselijke moderators.
             // voor elke afbeeldings-URL in het bestand... foreach (var Url in Urls) { // Initialiseer een nieuwe matrix met beoordelingstags ReviewTags = new KeyValuePair[MAXTAGSCOUNT];

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

Alle SDK's en voorbeelden van Microsoft Cognitive Services worden gelicentieerd met de MIT-licentie. Zie de [LICENTIE](https://microsoft.mit-license.org/) voor meer informatie.

## <a name="developer-code-of-conduct"></a>Gedragscode voor ontwikkelaars

Ontwikkelaars die gebruikmaken van Cognitive Services, inclusief deze clientbibliotheek en dit voorbeeld, worden geacht de 'gedragscode voor ontwikkelaars voor Microsoft Cognitive Services' te volgen, te vinden op http://go.microsoft.com/fwlink/?LinkId=698895 (Engelstalig).

## <a name="next-steps"></a>Volgende stappen

Bouw de zelfstudie en breid deze uit met de [projectbronbestanden](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) op GitHub.
