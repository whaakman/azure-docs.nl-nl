---
title: Pushmeldingen toevoegen aan uw app Universal Windows Platform (UWP) | Microsoft Docs
description: Informatie over het gebruik van Azure App Service Mobile Apps en Azure Notification Hubs pushmeldingen verzendt naar uw app Universal Windows Platform (UWP).
services: app-service\mobile,notification-hubs
documentationcenter: windows
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6de1b9d4-bd28-43e4-8db4-94cd3b187aa3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: a14bb0320c1f6a563f766a6a0fad5cf556fe7b70
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-windows-app"></a>Pushmeldingen toevoegen aan uw Windows-app
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie hebt u pushmeldingen toevoegen de [snel starten van Windows](app-service-mobile-windows-store-dotnet-get-started.md) project, zodat een pushmelding wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade quick start-serverproject niet gebruikt, moet u het push notification-uitbreidingspakket. Zie [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

## <a name="configure-hub"></a>Een Notification Hub configureren
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a>Uw app voor pushmeldingen registreren
U moet uw app naar de Windows Store verzenden, moet u uw serverproject voor de integratie met Windows Notification Services (WNS) voor het verzenden van pushmeldingen configureren.

1. Klik in Visual Studio Solution Explorer met de rechtermuisknop op de UWP-app-project, klikt u op **Store** > **App aan de Store koppelen...** .

    ![App aan Windows Store koppelen](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. Klik in de wizard op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor uw app in **een nieuwe appnaam reserveren**, klikt u vervolgens op **Reserve**.
3. Nadat de registratie van de app is gemaakt, selecteert u de naam van de nieuwe app, klikt u op **volgende**, en klik vervolgens op **koppelen**. Hierdoor worden de vereiste registratiegegevens voor Windows Store toegevoegd aan het toepassingsmanifest.  
4. Navigeer naar de [Windows-ontwikkelaarscentrum](https://dev.windows.com/en-us/overview), voor het aanmelden met je Microsoft-account, klikt u op de nieuwe app-registratie in **mijn apps**, vouw vervolgens **Services** > **Pushmeldingen**.
5. In de **Pushmeldingen** pagina, klikt u op **Live Services site** onder **Microsoft Azure Mobile Services**.
6. In de registratiepagina Noteer de waarde onder **toepassing geheimen** en de **pakket-SID**, die u vervolgens worden gebruikt voor het configureren van uw back-end voor de mobiele app.

    ![App aan Windows Store koppelen](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > Het clientgeheim en de pakket-SID zijn belangrijke beveiligingsreferenties. Deel deze waarden met niemand en distribueer ze niet met uw app. De **toepassings-Id** wordt gebruikt met het geheim verificatie van de Microsoft-Account configureren.
   >
   >

## <a name="configure-the-backend-to-send-push-notifications"></a>Configureer de back-end om pushmeldingen te verzenden
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <a id="update-service"></a>Werk de beheerserver om pushmeldingen te verzenden
Gebruik de procedure hieronder die overeenkomt met uw back-end-projecttype&mdash;beide [.NET back-end](#dotnet) of [back-end voor Node.js](#nodejs).

### <a name="dotnet"></a>.NET-back-endproject
1. In Visual Studio met de rechtermuisknop op het serverproject en klik op **NuGet-pakketten beheren**, zoekt u Microsoft.Azure.NotificationHubs en klik vervolgens op **installeren**. Hiermee installeert u de clientbibliotheek van Notification Hubs.
2. Vouw **domeincontrollers**, opent u TodoItemController.cs en voeg de volgende using-instructies:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. In de **PostTodoItem** methode, voeg de volgende code na het aanroepen van **InsertAsync**:

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

    Deze code geeft aan dat de notification hub een push-melding verzenden wanneer een nieuw item invoegen is.
4. Het serverproject publiceren.

### <a name="nodejs"></a>Back-endproject node.js
1. Als u dit nog niet hebt gedaan, [downloaden van het snelstartproject](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) of gebruik anders de [online-editor in Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Vervang de bestaande code in het bestand todoitem.js door het volgende:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
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

    Hierdoor wordt een pop-upmelding WNS waarin de item.text tijdens het invoegen van een nieuwe takenlijstitem verzonden.
3. Bij het bewerken van het bestand op uw lokale computer opnieuw publiceren het serverproject.

## <a id="update-app"></a>Pushmeldingen toevoegen aan uw app
Vervolgens moet uw app registreren voor pushmeldingen op opstarten. Wanneer u verificatie al hebt ingeschakeld, zorg ervoor dat de gebruiker zich aanmeldt voordat u probeert te registreren voor pushmeldingen.

1. Open de **App.xaml.cs** projectbestand en voeg de volgende `using` instructies:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. Voeg in hetzelfde bestand de volgende **InitNotificationsAsync** methodedefinitie naar de **App** klasse:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Deze code de ChannelURI voor de app opgehaald uit WNS en wordt vervolgens die ChannelURI geregistreerd met uw App Service-mobiele App.
3. Aan de bovenkant van de **OnLaunched** gebeurtenis-handler in **App.xaml.cs**, toevoegen de **asynchrone** aanpassingsfunctie voor de methodedefinitie en voeg de volgende oproep verzenden naar de nieuwe **InitNotificationsAsync** methode, zoals in het volgende voorbeeld:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Dit zorgt ervoor dat de tijdelijke ChannelURI wordt geregistreerd telkens wanneer die de toepassing wordt gestart.
4. Uw UWP-app-project opnieuw worden opgebouwd. Uw app is nu gereed om pop-upmeldingen te ontvangen.

## <a id="test"></a>Pushmeldingen testen in uw app
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <a id="more"></a>Volgende stappen
Meer informatie over pushmeldingen:

* [De beheerde client gebruiken voor Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications)  
  Sjablonen bieden u de flexibiliteit om platformoverschrijdende pushes en gelokaliseerde pushes te verzenden. Informatie over het registreren van sjablonen.
* [Push notification problemen vaststellen](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Er zijn diverse redenen waarom meldingen mogelijk ophalen verwijderd of kunnen niet eindigen op apparaten. Dit onderwerp leest u hoe te analyseren en te achterhalen van de hoofdoorzaken van fouten voor push-melding.

U kunt u verder gaat u aan bij een van de volgende zelfstudies:

* [Verificatie toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Ontdek hoe u gebruikers van uw app verifieert met een id-provider.
* [Offlinesynchronisatie voor uw app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Informatie over het toevoegen van offlineondersteuning aan uw app met een back-end voor mobiele apps. Met offlinesynchronisatie kunnen eindgebruikers interactie aangaan met een mobiele app&mdash;gegevens weergeven, toevoegen of wijzigen&mdash;ook als er geen netwerkverbinding is.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
