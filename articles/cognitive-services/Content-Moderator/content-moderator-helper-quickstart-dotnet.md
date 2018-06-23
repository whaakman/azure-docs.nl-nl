---
title: Azure Content beheerder SDK voor .NET Help-methode | Microsoft Docs
description: Het retourneren van een client inhoud beheerder is met Azure inhoud beheerder SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 36f2124708731f78f34849d8210ed39ea8f59140
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344436"
---
# <a name="helper-code-to-return-a-content-moderator-client"></a>Help-code voor het retourneren van een client inhoud beheerder

In dit artikel bevat informatie en codevoorbeelden om u te helpen aan de slag met de inhoud beheerder SDK voor .NET voor het maken van een client inhoud beheerder voor uw abonnement.

De bibliotheek wordt gebruikt door andere snelstartgidsen in deze sectie.

In dit artikel wordt ervan uitgegaan dat u al bekend met Visual Studio en C# bent.

> [!IMPORTANT]
> Deze klassenbibliotheek bevat de code die is bedoeld voor demonstratiedoeleinden alleen.
> Als u deze code voor gebruik in productie aanpassen, moet u een beveiligde methode opslaan en gebruiken van uw abonnement inhoud beheerder sleutel gebruiken.

## <a name="sign-up-for-content-moderator-services"></a>Aanmelden voor inhoud beheerder services

Voordat u inhoud beheerder services via de REST-API of de SDK gebruiken kunt, moet u een abonnementssleutel.
Raadpleeg de [Quick Start](quick-start.md) voor meer informatie over hoe u de sleutel kunt verkrijgen.

## <a name="create-your-visual-studio-project"></a>Visual Studio-project maken

1. Maak een nieuwe **Class Library (.NET Framework)** project.

   In de voorbeeldcode ik het project de naam **ModeratorHelper**.

1. Voeg een verwijzing naar de **System.Configuration** Framework-assembly.

### <a name="install-required-packages"></a>De vereiste pakketten installeren

De volgende NuGet-pakketten installeren:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="create-the-content-moderator-client"></a>De client inhoud beheerder maken

De inhoud van het bestand ModeratorHelper.cs vervangen door de volgende code:

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
> Update de **AzureRegion** en **CMSubscriptionKey** velden met de waarden van uw regio-id en -abonnement sleutel.

U hebt nu een snelle manier om een client inhoud beheerder voor uw abonnement te maken.

## <a name="next-steps"></a>Volgende stappen

[Downloaden van de Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere inhoud beheerder snelstartgidsen voor .NET, en op uw integratie aan de slag.
