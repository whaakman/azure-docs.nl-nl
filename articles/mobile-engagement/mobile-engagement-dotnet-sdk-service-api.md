---
title: Met behulp van .NET SDK voor toegang tot Azure Mobile Engagement Service API 's
description: Hierin wordt beschreven hoe u de Mobile Engagement .NET SDK gebruiken voor toegang tot Azure Mobile Engagement Service API 's
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: c07728aa-43f2-4238-8b4a-c9eddf9d838b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 6a497189268c5a1b7e269cc57904ebc77c1906fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Met behulp van .NET SDK voor toegang tot Azure Mobile Engagement Service API 's
Beschrijft een reeks API's voor het beheer van apparaten, de Azure Mobile Engagement/te pushen Reach-campagnes enzovoort. Om te kunnen communiceren met deze API's, wij bieden u ook een [Swagger-bestand](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) waarmee u met hulpprogramma's kunt voor het genereren van SDK's voor uw voorkeurstaal. Wordt u aangeraden de [AutoRest](https://github.com/Azure/AutoRest) hulpprogramma voor het genereren van de SDK van onze Swagger-bestand.

> [!NOTE]
> De Azure Mobile Engagement-service wordt in maart 2018 beëindigd en is momenteel alleen beschikbaar voor bestaande klanten. Zie [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/) voor meer informatie.

We hebben een .NET-SDK op een vergelijkbare manier waarmee u communiceren met deze API's met behulp van een C#-wrapper gemaakt en u hoeft te doen de token verificatie-onderhandeling en vernieuw zelf.  

Dit voorbeeld wordt de reeks stappen te volgen voor het gebruik van de .NET SDK:

1. U moet eerst, om de verificatie voor uw API's met behulp van de Azure Active Directory zoals beschreven in te stellen [hier](mobile-engagement-api-authentication.md#authentication). Aan het einde van de volgende stappen u moet beschikken over een geldige **SubscriptionId**, **TenantId**, **ApplicationId** en **geheim**. 
2. We een eenvoudige app voor Windows-Console gebruiken om te werken met de .NET SDK met het scenario voor het maken van een campagne aankondiging demonstreren. Dus opent u Visual Studio en maak een **consoletoepassing**.   
3. Vervolgens moet u de .NET SDK die beschikbaar als is download **Management-bibliotheek van Microsoft Azure Engagement** in de Nuget-galerie [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
   Als u het Nuget vanuit Visual Studio installeert, moet u ervoor zorgen dat u ingeschakeld hebt de **Include prerelease** optie bij het zoeken naar het pakket:
   
    ![][1]
4. In de `Program.cs` bestand, het toevoegen van de volgende naamruimten:
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;
5. Vervolgens moet u de volgende constanten die we gebruiken voor verificatie en interactie met de Mobile Engagement-App die u in de campagne aankondiging maakt definiëren:
   
        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";
   
        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";
   
        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";
6. Definieer de `EngagementManagementClient` variabele die wordt gebruikt om de Mobile Engagement SDK-methoden aanroept:
   
        static EngagementManagementClient engagementClient; 
7. Het volgende toevoegen aan uw `Main` methode:
   
        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();
   
                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }
8. Definieer de volgende methode op die voor het initialiseren van zorgt de `EngagementManagementClient` door eerst te verifiëren en zichzelf vervolgens te koppelen met de Mobile Engagement-App waarin u van plan bent om de campagne aankondiging te maken:
   
        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
   
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;
   
            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }
   
   > [!IMPORTANT]
   > Merk op dat u wilt gebruiken, de **App resourcenaam** zoals gedefinieerd in de Azure-beheerportal voor de parameter AppName. 
   > 
   > 
9. Ten slotte definiëren de CreateCampaign-methode op die zorgt voor een eenvoudige maken met de eerder geïnitialiseerd EngagementClient **op elk gewenst moment** & **melding alleen-lezen** campagne met een titel en het bericht: 
   
        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );
   
            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }
10. Voer de console-app en u ziet het volgende op de campagne gemaakt:
    
    **Campagne-Id '159' is gemaakt en de status 'Concept'**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
