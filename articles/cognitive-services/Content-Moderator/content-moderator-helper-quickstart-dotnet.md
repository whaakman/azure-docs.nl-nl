---
title: 'Snelstart: Een beheerclient maken voor .NET - Content Moderator'
titlesuffix: Azure Cognitive Services
description: Een Content Moderator-client retourneren met behulp van Azure Content Moderator SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: aa3c2d5c408969aa3127562607ca8d4e89f44f2e
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54262265"
---
# <a name="quickstart-helper-code-to-return-a-content-moderator-client"></a>Quickstart: Helpercode voor het retourneren van een Content Moderator-client

In dit artikel vindt u informatie en codevoorbeelden om aan de slag te gaan met de Content Moderator SDK voor .NET om een Content Moderator-client voor uw abonnement te maken.

De bibliotheek wordt gebruikt door andere snelstarts in deze sectie.

In dit artikel wordt ervan uitgegaan dat u al bekend bent met Visual Studio en C#.

> [!IMPORTANT]
> Deze klassebibliotheek bevat alleen code die bedoeld is voor demonstratiedoeleinden.
> Als u deze code aanpast voor gebruik bij de productie, gebruik dan een veilige methode voor het opslaan en gebruiken van uw abonnementssleutel voor Content Moderator.

## <a name="sign-up-for-content-moderator-services"></a>Registreren voor de Content Moderator-services

Om de Content Moderator-services via de REST-API of de SDK te kunnen gebruiken, hebt u een abonnementssleutel nodig.
Raadpleeg de quickstart [Content Moderator uitproberen op het web](quick-start.md) voor informatie over het verkrijgen van de sleutel.

## <a name="create-your-visual-studio-project"></a>Het Visual Studio-project maken

1. Maak een nieuw **Class Library (.NET Framework)** project.

   In de voorbeeldcode heb ik het project **ModeratorHelper** genoemd.

1. Voeg een verwijzing naar de Framework-assembly **System.Configuration** toe.

### <a name="install-required-packages"></a>De vereiste pakketten installeren

Installeer de volgende NuGet-pakketten:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="create-the-content-moderator-client"></a>De Content Moderator-client maken

Vervang de inhoud van het bestand ModeratorHelper.cs door de volgende code:

    using Microsoft.CognitiveServices.ContentModerator;

    namespace ModeratorHelper
    {
    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "myRegion";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "myKey";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }
    }


> [!IMPORTANT]
> Werk de velden **AzureRegion** en **CMSubscriptionKey** bij met de waarden van uw regio-id en de abonnementssleutel.

Nu hebt u een snelle manier om een Content Moderator-client voor uw abonnement te maken.

## <a name="next-steps"></a>Volgende stappen

[Download de Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere snelstarts over Content Moderator voor .NET en begin met de integratie.
