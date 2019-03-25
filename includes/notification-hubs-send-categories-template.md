---
title: bestand opnemen
description: bestand opnemen
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/30/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: ce8496596f25b85719b8a6dff849ebf0fc3e5dc3
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58405598"
---
In deze sectie verzendt u het laatste nieuws als gelabelde sjabloonmeldingen vanuit een .NET-console-app. 

1. Maak in Visual Studio, een nieuwe Visual C# consoletoepassing: een. Selecteer in het menu **bestand** > **nieuw** > **Project**.
    b. Vouw **Visual C#** , en selecteer **Windows Desktop**. 
    c. Selecteer **Console-App (.NET Framework)** in de lijst met sjablonen. 
    d. Voer een **naam** voor de app. 
    e. Selecteer een **map** voor de app.
    f. Selecteer **OK** om het project te maken. 
2. Selecteer in het hoofdmenu van Visual Studio **extra** > **NuGet Package Manager** > **Package Manager Console** en klik vervolgens in het consolevenster Voer de volgende tekenreeks:
   
    ```
    Install-Package Microsoft.Azure.NotificationHubs
    ```
   
3. Selecteer **Enter**.  
    Met deze actie wordt een verwijzing toegevoegd aan de Azure Notification Hubs-SDK met het [Microsoft.Azure.Notification Hubs NuGet-pakket].

4. Open het Program.cs-bestand en volg de volgende `using`-instructie:
   
    ```csharp
    using Microsoft.Azure.NotificationHubs;
    ```

5. Voeg in de klasse `Program` de volgende methode toe of vervang de methode als deze al bestaat:
   
    ```csharp
    private static async void SendTemplateNotificationAsync()
    {
        // Define the notification hub.
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");

        // Create an array of breaking news categories.
        var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};

        // Send the notification as a template notification. All template registrations that contain
        // "messageParam" and the proper tags will receive the notifications.
        // This includes APNS, GCM, WNS, and MPNS template registrations.

        Dictionary<string, string> templateParams = new Dictionary<string, string>();

        foreach (var category in categories)
        {
            templateParams["messageParam"] = "Breaking " + category + " News!";
            await hub.SendTemplateNotificationAsync(templateParams, category);
        }
    }
    ```   
   
    Met deze code wordt een sjabloonmelding verzonden voor elk van de zes labels in de tekenreeksmatrix. Het gebruik van labels zorgt ervoor dat apparaten meldingen ontvangen voor de geregistreerde categorieën.

5. Vervang in de voorgaande code de tijdelijke aanduidingen `<hub name>` en `<connection string with full access>` door de naam van de meldingenhub en de verbindingsreeks voor *DefaultFullSharedAccessSignature* uit het dashboard van de meldingenhub.

6. Voeg de volgende regels in de **Main**-methode toe:
   
    ```csharp
    SendTemplateNotificationAsync();
    Console.ReadLine();
    ```

7. Bouw de console-app.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Get started with Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hubs REST interface]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Add push notifications for Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[How to use Notification Hubs from Java or PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.Notification Hubs NuGet-pakket]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
