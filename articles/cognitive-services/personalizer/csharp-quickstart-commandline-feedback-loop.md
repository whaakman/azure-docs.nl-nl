---
title: Feedback-lus - Personalizer
titleSuffix: Azure Cognitive Services
description: Personaliseren van inhoud in deze C# Quick Start met de Personalizer-service.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/08/2019
ms.author: edjez
ms.openlocfilehash: 85252680fcc4d2592d242762d01040c3859b14a2
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442081"
---
# <a name="quickstart-personalize-content-using-c"></a>Quickstart: Aan persoonlijke voorkeuren aanpassen met behulp van inhoudC# 

Gepersonaliseerde inhoud weergeven in dit C# Quick Start met de Personalizer-service.

In dit voorbeeld ziet u hoe u de Personalizer-clientbibliotheek voor C# naar de volgende acties uitvoeren: 

 * Een lijst met acties voor persoonlijke instellingen rangschikken.
 * Rapport beloning om toe te wijzen aan het begin gerangschikt op basis van de selectie van de gebruiker voor de opgegeven gebeurtenis actie.

Aan de slag met Personalizer omvat de volgende stappen:

1. Verwijzing naar de SDK 
1. Code schrijven voor de rangschikking van de acties die u wilt weergeven voor uw gebruikers
1. Schrijven van code voor het verzenden van beloningen met het trainen van de lus.

## <a name="prerequisites"></a>Vereisten

* U moet een [Personalizer service](how-to-settings.md) om uw abonnement-sleutel en het eindpunt service-url te achterhalen. 
* [Visual Studio 2015 of 2017](https://visualstudio.microsoft.com/downloads/).
* De Microsoft.Azure.CognitiveServices.Personalizer SDK NuGet-pakket. Hieronder vindt u de installatie-instructies.

## <a name="change-the-model-update-frequency"></a>De model-updatefrequentie wijzigen

In de Personalizer-resource in Azure portal, wijzigt u de **Model updatefrequentie** 10 seconden. Dit wordt trainen de service snel, zodat u kunt zien hoe de eerste actie verandert voor elke herhaling

![Model updatefrequentie wijzigen](./media/settings/configure-model-update-frequency-settings.png)

## <a name="creating-a-new-console-app-and-referencing-the-personalizer-sdk"></a>Het maken van een nieuwe consoletoepassing en verwijzen naar de SDK Personalizer 

<!--
Get the latest code as a Visual Studio solution from [GitHub] (add link).
-->

1. Maak in Visual Studio een nieuwe Visual C#-console-app.
1. Installeer het Personalizer client-bibliotheek NuGet-pakket. Selecteer in het menu **extra**, selecteer **Nuget package Manager**, klikt u vervolgens **NuGet-pakketten beheren voor oplossing**.
1. Selecteer de **Bladeren** tabblad, en klik in de **zoeken** vak `Microsoft.Azure.CognitiveServices.Personalizer`.
1. Selecteer **Microsoft.Azure.CognitiveServices.Personalizer** wanneer deze wordt weergegeven.
1. Schakel het selectievakje naast de projectnaam van uw en selecteer **installeren**.

## <a name="add-the-code-and-put-in-your-personalizer-and-azure-keys"></a>Voeg de code toe en plaats in uw sleutels Personalizer en Azure

1. Vervang Program.cs door de code hieronder. 
1. Vervang `serviceKey` waarde met uw geldige Personalizer-abonnementssleutel.
1. Vervang `serviceEndpoint` met uw service-eindpunt. Een voorbeeld is `https://westus2.api.cognitive.microsoft.com/`.
1. Voer het programma uit.

## <a name="add-code-to-rank-the-actions-you-want-to-show-to-your-users"></a>Code toevoegen om te rangschikken van de acties die u wilt weergeven voor uw gebruikers

De volgende C# code is een volledig overzicht te geven van gebruikersgegevens, _features en informatie over uw inhoud _acties_, naar Personalizer met behulp van de SDK. Personalizer retourneert de bovenkant gerangschikt actie om de gebruiker weer te geven.  

```csharp
using Microsoft.Azure.CognitiveServices.Personalizer;
using Microsoft.Azure.CognitiveServices.Personalizer.Models;
using System;
using System.Collections.Generic;
using System.Linq;

namespace PersonalizerExample
{
    class Program
    {
        // The key specific to your personalizer service instance; e.g. "0123456789abcdef0123456789ABCDEF"
        private const string ApiKey = "";

        // The endpoint specific to your personalizer service instance; e.g. https://westus2.api.cognitive.microsoft.com/
        private const string ServiceEndpoint = "";

        static void Main(string[] args)
        {
            int iteration = 1;
            bool runLoop = true;

            // Get the actions list to choose from personalizer with their features.
            IList<RankableAction> actions = GetActions();

            // Initialize Personalizer client.
            PersonalizerClient client = InitializePersonalizerClient(ServiceEndpoint);

            do
            {
                Console.WriteLine("\nIteration: " + iteration++);

                // Get context information from the user.
                string timeOfDayFeature = GetUsersTimeOfDay();
                string tasteFeature = GetUsersTastePreference();

                // Create current context from user specified data.
                IList<object> currentContext = new List<object>() {
                    new { time = timeOfDayFeature },
                    new { taste = tasteFeature }
                };

                // Exclude an action for personalizer ranking. This action will be held at its current position.
                IList<string> excludeActions = new List<string> { "juice" };

                // Generate an ID to associate with the request.
                string eventId = Guid.NewGuid().ToString();

                // Rank the actions
                var request = new RankRequest(actions, currentContext, excludeActions, eventId);
                RankResponse response = client.Rank(request);

                Console.WriteLine("\nPersonalizer service thinks you would like to have: " + response.RewardActionId + ". Is this correct? (y/n)");

                float reward = 0.0f;
                string answer = GetKey();

                if (answer == "Y")
                {
                    reward = 1;
                    Console.WriteLine("\nGreat! Enjoy your food.");
                }
                else if (answer == "N")
                {
                    reward = 0;
                    Console.WriteLine("\nYou didn't like the recommended food choice.");
                }
                else
                {
                    Console.WriteLine("\nEntered choice is invalid. Service assumes that you didn't like the recommended food choice.");
                }

                Console.WriteLine("\nPersonalizer service ranked the actions with the probabilities as below:");
                foreach (var rankedResponse in response.Ranking)
                {
                    Console.WriteLine(rankedResponse.Id + " " + rankedResponse.Probability);
                }

                // Send the reward for the action based on user response.
                client.Reward(response.EventId, new RewardRequest(reward));

                Console.WriteLine("\nPress q to break, any other key to continue:");
                runLoop = !(GetKey() == "Q");

            } while (runLoop);
        }

        /// <summary>
        /// Initializes the personalizer client.
        /// </summary>
        /// <param name="url">Azure endpoint</param>
        /// <returns>Personalizer client instance</returns>
        static PersonalizerClient InitializePersonalizerClient(string url)
        {
            PersonalizerClient client = new PersonalizerClient(
                new ApiKeyServiceClientCredentials(ApiKey)) {Endpoint = url};

            return client;
        }

        /// <summary>
        /// Get users time of the day context.
        /// </summary>
        /// <returns>Time of day feature selected by the user.</returns>
        static string GetUsersTimeOfDay()
        {
            string[] timeOfDayFeatures = new string[] { "morning", "afternoon", "evening", "night" };

            Console.WriteLine("\nWhat time of day is it (enter number)? 1. morning 2. afternoon 3. evening 4. night");
            if (!int.TryParse(GetKey(), out int timeIndex) || timeIndex < 1 || timeIndex > timeOfDayFeatures.Length)
            {
                Console.WriteLine("\nEntered value is invalid. Setting feature value to " + timeOfDayFeatures[0] + ".");
                timeIndex = 1;
            }

            return timeOfDayFeatures[timeIndex - 1];
        }

        /// <summary>
        /// Gets user food preference.
        /// </summary>
        /// <returns>Food taste feature selected by the user.</returns>
        static string GetUsersTastePreference()
        {
            string[] tasteFeatures = new string[] { "salty", "sweet" };

            Console.WriteLine("\nWhat type of food would you prefer (enter number)? 1. salty 2. sweet");
            if (!int.TryParse(GetKey(), out int tasteIndex) || tasteIndex < 1 || tasteIndex > tasteFeatures.Length)
            {
                Console.WriteLine("\nEntered value is invalid. Setting feature value to " + tasteFeatures[0] + ".");
                tasteIndex = 1;
            }

            return tasteFeatures[tasteIndex - 1];
        }

        /// <summary>
        /// Creates personalizer actions feature list.
        /// </summary>
        /// <returns>List of actions for personalizer.</returns>
        static IList<RankableAction> GetActions()
        {
            IList<RankableAction> actions = new List<RankableAction>
            {
                new RankableAction
                {
                    Id = "pasta",
                    Features =
                    new List<object>() { new { taste = "salty", spiceLevel = "medium" }, new { nutritionLevel = 5, cuisine = "italian" } }
                },

                new RankableAction
                {
                    Id = "ice cream",
                    Features =
                    new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionalLevel = 2 } }
                },

                new RankableAction
                {
                    Id = "juice",
                    Features =
                    new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionLevel = 5 }, new { drink = true } }
                },

                new RankableAction
                {
                    Id = "salad",
                    Features =
                    new List<object>() { new { taste = "salty", spiceLevel = "low" }, new { nutritionLevel = 8 } }
                }
            };

            return actions;
        }

        private static string GetKey()
        {
            return Console.ReadKey().Key.ToString().Last().ToString().ToUpper();
        }
    }
}
```

## <a name="run-the-program"></a>Het programma uitvoeren

Bouw het programma en voer het uit. Quick Start wordt u gevraagd een aantal vragen voor het verzamelen van gebruikersvoorkeuren, functies, ook wel geeft vervolgens de bovenste actie.

![Quick Start wordt u gevraagd een aantal vragen voor het verzamelen van gebruikersvoorkeuren, functies, ook wel geeft vervolgens de bovenste actie.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

## <a name="clean-up-resources"></a>Resources opschonen
Verwijder wanneer u klaar bent met de snelstart alle bestanden die in de snelstart zijn gemaakt. 

## <a name="next-steps"></a>Volgende stappen

[De werking van Personalizer](how-personalizer-works.md)


