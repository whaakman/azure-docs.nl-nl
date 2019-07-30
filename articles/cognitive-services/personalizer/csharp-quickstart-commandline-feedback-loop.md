---
title: 'Quickstart: Een feedback-lus maken-persoonlijker'
titleSuffix: Azure Cognitive Services
description: Persoonlijke inhoud in deze C# Snelstartgids met de personaler-service.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 06/11/2019
ms.author: diberry
ms.openlocfilehash: 54aa23071fef09058a1702218d6b7fc920363518
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68662802"
---
# <a name="quickstart-personalize-content-using-c"></a>Quickstart: Inhoud personaliseren metC# 

Persoonlijke inhoud in deze C# Quick Start weer geven met de personaler service.

In dit voor C# beeld wordt gedemonstreerd hoe u de personaler-client bibliotheek kunt gebruiken om de volgende acties uit te voeren: 

 * Rang schikking van een lijst met acties voor persoonlijke instellingen.
 * Rapport beloning om toe te wijzen aan de geclassificeerde actie op basis van de selectie van de gebruiker voor de opgegeven gebeurtenis.

Aan de slag met Personaler bestaat uit de volgende stappen:

1. Verwijzen naar de SDK 
1. Schrijven van code voor het rangschikken van de acties die u wilt weer geven aan uw gebruikers,
1. Schrijven van code voor het verzenden van beloningen voor het trainen van de lus.

## <a name="prerequisites"></a>Vereisten

* U hebt een [personaler service](how-to-settings.md) nodig om uw abonnements sleutel en URL voor de eind punt-service op te halen. 
* [Visual Studio 2015 of 2017](https://visualstudio.microsoft.com/downloads/).
* Het [micro soft. Azure. CognitiveServices. personaler](https://go.microsoft.com/fwlink/?linkid=2092272) SDK NuGet-pakket. Hieronder vindt u de installatie-instructies.

## <a name="change-the-model-update-frequency"></a>De update frequentie van het model wijzigen

Wijzig de **Update frequentie** van het model in de Azure Portal persoonlijker in de resource in 10 seconden. Hiermee wordt de service snel getraind, zodat u kunt zien hoe de belangrijkste actie voor elke herhaling wordt gewijzigd.

Wanneer een aangepaste lus voor het eerst wordt geïnstantieerd, is er geen model, omdat er geen belonings-API-aanroepen zijn voor de trein van. Rang gesprekken retour neren een gelijke kansen voor elk item. Uw toepassing moet toch altijd inhoud plaatsen met de uitvoer van RewardActionId.

![Update frequentie van het model wijzigen](./media/settings/configure-model-update-frequency-settings.png)

## <a name="creating-a-new-console-app-and-referencing-the-personalizer-sdk"></a>Een nieuwe console-app maken en verwijzen naar de Personaler SDK 

<!--
Get the latest code as a Visual Studio solution from [GitHub] (add link).
-->

1. Maak in Visual Studio een nieuwe Visual C#-console-app.
1. Installeer het NuGet-pakket voor de Personaler-client bibliotheek. Selecteer in het menu **extra**, selecteer **Nuget package manager**en vervolgens **Nuget-pakketten beheren voor oplossing**.
1. Controleer **include Prerelease**.
1. Selecteer het tabblad **Bladeren** en typ `Microsoft.Azure.CognitiveServices.Personalizer`in het **zoekvak** .
1. Selecteer **micro soft. Azure. CognitiveServices. personaler** wanneer het wordt weer gegeven.
1. Schakel het selectie vakje in naast de naam van uw project en selecteer **installeren**.

## <a name="add-the-code-and-put-in-your-personalizer-and-azure-keys"></a>Voeg de code toe en plaats deze in uw persoonlijke voor keuren en Azure-sleutels

1. Vervang Program.cs door de code hieronder. 
1. Vervang `serviceKey` de waarde door uw geldige abonnements sleutel voor persoonlijker.
1. Vervang `serviceEndpoint` door uw service-eind punt. Een voorbeeld is `https://westus2.api.cognitive.microsoft.com/`.
1. Voer het programma uit.

## <a name="add-code-to-rank-the-actions-you-want-to-show-to-your-users"></a>Voeg code toe om de acties te rangschikken die u wilt weer geven aan uw gebruikers

De volgende C# code is een volledige lijst voor het door geven van gebruikers gegevens, _features en informatie over uw inhoud, _acties_, naar persoonlijke voor keuren met behulp van de SDK. Persoonlijker retourneert de bovenste geclassificeerde actie om uw gebruiker weer te geven.  

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

Bouw het programma en voer het uit. In het Quick Start-programma wordt een aantal vragen gesteld om gebruikers voorkeuren, ook wel bekend als-functies, te verzamelen en de meest voorkomende actie te bieden.

![In het Quick Start-programma wordt een aantal vragen gesteld om gebruikers voorkeuren, ook wel bekend als-functies, te verzamelen en de meest voorkomende actie te bieden.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

## <a name="clean-up-resources"></a>Resources opschonen
Verwijder wanneer u klaar bent met de snelstart alle bestanden die in de snelstart zijn gemaakt. 

## <a name="next-steps"></a>Volgende stappen

[Hoe Personaler werkt](how-personalizer-works.md)


