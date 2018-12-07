---
title: Pushmeldingen toevoegen aan uw Universal Windows Platform (UWP)-app | Microsoft Docs
description: Meer informatie over het gebruik van Azure App Service Mobile Apps en Azure Notification Hubs om pushmeldingen te verzenden aan uw Universal Windows Platform (UWP)-app.
services: app-service\mobile,notification-hubs
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 6de1b9d4-bd28-43e4-8db4-94cd3b187aa3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: 7efd853e7b66933cac811625d7510139864f41f3
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001852"
---
# <a name="add-push-notifications-to-your-windows-app"></a>Pushmeldingen toevoegen aan uw Windows-app

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie voegt u pushmeldingen kunt verzenden naar de [Windows-snelstartgids](app-service-mobile-windows-store-dotnet-get-started.md) project, zodat een pushmelding wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade quick start-serverproject niet gebruikt, moet u de push notification-uitbreidingspakket. Zie [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

## <a name="configure-hub"></a>Een Notification Hub configureren

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a>Uw app voor pushmeldingen registreren

U moet uw app naar de Microsoft Store verzenden en configureer vervolgens uw serverproject te integreren met [Windows Notification Services (WNS)](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview) voor het verzenden van pushmeldingen.

1. In Visual Studio Solution Explorer met de rechtermuisknop op het UWP-app-project, klikt u op **Store** > **App aan de Store koppelen...** .

    ![App koppelen aan Microsoft Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)

2. In de wizard, klikt u op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor uw app in **een nieuwe appnaam reserveren**, klikt u vervolgens op **Reserve**.
3. Nadat de app-registratie is gemaakt, selecteert u de naam van de nieuwe app, klikt u op **volgende**, en klik vervolgens op **koppelen**. Hiermee wordt de vereiste registratiegegevens voor Microsoft Store toegevoegd aan het toepassingsmanifest.
4. Navigeer naar de [Portal voor Appregistratie](https://apps.dev.microsoft.com/) en meld u aan met uw Microsoft-account. Klik op de Windows Store-app die u in de vorige stap hebt gekoppeld.
5. In de registratiepagina geleid, noteer de waarde onder **toepassingsgeheimen** en de **pakket-SID**, waarmee u wordt vervolgens het configureren van de back-end van uw mobiele app.

    ![App koppelen aan Microsoft Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > Het clientgeheim en de pakket-SID zijn belangrijke beveiligingsreferenties. Deel deze waarden met niemand en distribueer ze niet met uw app. De **toepassings-Id** aan de geheime sleutel wordt gebruikt om verificatie van de Microsoft-Account te configureren.

[App Center](https://docs.microsoft.com/appcenter/sdk/push/uwp#prerequisite---register-your-app-for-windows-notification-services-wns) bevat ook instructies voor het configureren van de UWP-apps voor pushmeldingen.

## <a name="configure-the-backend-to-send-push-notifications"></a>Configureren van de back-end om pushmeldingen te verzenden

[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <a id="update-service"></a>Bijwerken van de server om pushmeldingen te verzenden

Gebruik de procedure hieronder die overeenkomt met uw back-end-projecttype&mdash;beide [.NET back-end](#dotnet) of [Node.js-back-end](#nodejs).

### <a name="dotnet"></a>.NET-back-end-project

1. Met de rechtermuisknop op de serverproject in Visual Studio, en klikt u op **NuGet-pakketten beheren**, Microsoft.Azure.NotificationHubs Zoek en klik vervolgens op **installeren**. Hiermee installeert u de Notification Hubs-clientbibliotheek.
2. Vouw **Controllers**, open TodoItemController.cs en voeg de volgende using-instructies toe:

    ```csharp
    using System.Collections.Generic;
    using Microsoft.Azure.NotificationHubs;
    using Microsoft.Azure.Mobile.Server.Config;
    ```

3. In de **PostTodoItem** methode, voeg de volgende code na het aanroepen van **InsertAsync**:

    ```csharp
    // Get the settings for the server project.
    HttpConfiguration config = this.Configuration;
    MobileAppSettingsDictionary settings =
        this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

    // Get the Notification Hubs credentials for the Mobile App.
    string notificationHubName = settings.NotificationHubName;
    string notificationHubConnection = settings
        .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

    // Create the notification hub client.
    NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

    // Define a WNS payload
    var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                            + item.Text + @"</text></binding></visual></toast>";
    try
    {
        // Send the push notification.
        var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

        // Write the success result to the logs.
        config.Services.GetTraceWriter().Info(result.State.ToString());
    }
    catch (System.Exception ex)
    {
        // Write the failure result to the logs.
        config.Services.GetTraceWriter()
            .Error(ex.Message, null, "Push.SendAsync Error");
    }
    ```

    Deze code wordt de notification hub voor het verzenden van een pushmelding ontvangen nadat een nieuw item invoegen is.

4. Het serverproject publiceren.

### <a name="nodejs"></a>Node.js-back-end-project
1. Als u dit nog niet hebt gedaan, [downloaden van de Quick Start-project](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) of gebruik anders de [online-editor in Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Vervang de bestaande code in het bestand todoitem.js door het volgende:

    ```javascript
    var azureMobileApps = require('azure-mobile-apps'),
    promises = require('azure-mobile-apps/src/utilities/promises'),
    logger = require('azure-mobile-apps/src/logger');

    var table = azureMobileApps.table();

    table.insert(function (context) {
    // For more information about the Notification Hubs JavaScript SDK,
    // see https://aka.ms/nodejshubs
    logger.info('Running TodoItem.insert');

    // Define the WNS payload that contains the new item Text.
    var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                + context.item.text + "</text></binding></visual></toast>";

    // Execute the insert.  The insert returns the results as a Promise,
    // Do the push as a post-execute action within the promise flow.
    return context.execute()
        .then(function (results) {
            // Only do the push if configured
            if (context.push) {
                // Send a WNS native toast notification.
                context.push.wns.sendToast(null, payload, function (error) {
                    if (error) {
                        logger.error('Error while sending push notification: ', error);
                    } else {
                        logger.info('Push notification sent successfully!');
                    }
                });
            }
            // Don't forget to return the results from the context.execute()
            return results;
        })
        .catch(function (error) {
            logger.error('Error while running context.execute: ', error);
        });
    });

    module.exports = table;
    ```

    Hierdoor wordt een pop-upmelding voor WNS waarin de item.text wanneer een nieuwe taak wordt ingevoegd verzonden.

3. Tijdens het bewerken van het bestand op uw lokale computer, publiceert u opnieuw het serverproject.

## <a id="update-app"></a>Pushmeldingen toevoegen aan uw app
Uw app moet vervolgens worden geregistreerd voor pushmeldingen op opstarten. Als u al verificatie hebt ingeschakeld, zorg ervoor dat de gebruiker zich aanmeldt voordat u probeert te registreren voor pushmeldingen.

1. Open de **App.xaml.cs** projectbestand en voeg de volgende `using` instructies:

    ```csharp
    using System.Threading.Tasks;
    using Windows.Networking.PushNotifications;
    ```

2. Voeg in hetzelfde bestand de volgende **InitNotificationsAsync** methodedefinitie naar de **App** klasse:

    ```csharp
    private async Task InitNotificationsAsync()
    {
        // Get a channel URI from WNS.
        var channel = await PushNotificationChannelManager
            .CreatePushNotificationChannelForApplicationAsync();

        // Register the channel URI with Notification Hubs.
        await App.MobileService.GetPush().RegisterAsync(channel.Uri);
    }
    ```

    Deze code wordt de kanaal-URI voor de app opgehaald uit WNS, en vervolgens die kanaal-URI voor uw App Service Mobile App geregistreerd.

3. Aan de bovenkant van de **OnLaunched** gebeurtenis-handler in **App.xaml.cs**, toevoegen de **asynchrone** modifier aan de methodedefinitie van de en voeg de volgende oproep verzenden naar de nieuwe  **InitNotificationsAsync** methode, zoals in het volgende voorbeeld:

    ```csharp
    protected async override void OnLaunched(LaunchActivatedEventArgs e)
    {
        await InitNotificationsAsync();

        // ...
    }
    ```

    Dit zorgt ervoor dat de tijdelijke kanaal-URI wordt geregistreerd telkens wanneer die de toepassing wordt gestart.

4. Opnieuw opbouwen uw UWP-app-project. Uw app is nu gereed om pop-upmeldingen te ontvangen.

## <a id="test"></a>Testpushmeldingen in uw app

[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <a id="more"></a>Volgende stappen

Meer informatie over pushmeldingen te verzenden:

* [De beheerde client gebruiken voor Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications) sjablonen beschikt u over flexibiliteit voor het verzenden van platformonafhankelijke pushes en gelokaliseerde pushes. Informatie over het registreren van sjablonen.
* [Push-melding problemen](../notification-hubs/notification-hubs-push-notification-fixer.md) er zijn diverse redenen waarom meldingen mogelijk verwijderd of niet doen terechtkomen op apparaten. Dit onderwerp ziet u hoe om te analyseren en te bepalen wat de oorzaak van fouten bij push-melding.

Houd rekening met voortgezet u aan bij een van de volgende zelfstudies:

* [Verificatie toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-users.md) Lees hoe u gebruikers van uw app verifieert met een id-provider.
* [Offlinesynchronisatie inschakelen voor uw app](app-service-mobile-windows-store-dotnet-get-started-offline-data.md) meer informatie over het toevoegen van Offlineondersteuning aan uw app met behulp van een back-end van de mobiele App. Met offlinesynchronisatie kunnen eindgebruikers interactie aangaan met een mobiele app&mdash;gegevens weergeven, toevoegen of wijzigen&mdash;ook als er geen netwerkverbinding is.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
