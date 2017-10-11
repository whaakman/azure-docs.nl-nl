In deze sectie kunt u code bijwerken in uw bestaande Mobile Apps back-end-project voor het verzenden van een push-melding telkens wanneer een nieuw item wordt toegevoegd. Dit wordt mogelijk gemaakt door de [sjabloon](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) functie van Azure Notification Hubs, platformoverschrijdende pushes inschakelen. De verschillende clients worden geregistreerd voor pushmeldingen met behulp van sjablonen en een enkele universal push toegang krijgen tot alle clientplatforms.

Kies een van de volgende procedures die overeenkomt met uw back-end projecttype&mdash;beide [.NET back-end](#dotnet) of [Node.js-back-end](#nodejs).

### <a name="dotnet"></a>.NET-back-end-project
1. In Visual Studio met de rechtermuisknop op het serverproject en klik op **NuGet-pakketten beheren**. Zoeken naar `Microsoft.Azure.NotificationHubs`, en klik vervolgens op **installeren**. Hiermee installeert u de Notification Hubs-bibliotheek voor het verzenden van meldingen vanuit uw back-end.
2. Open in het serverproject **domeincontrollers** > **TodoItemController.cs**, en voeg de volgende using-instructies:

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

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Dit stuurt de melding van een sjabloon die het item bevat. Tekst als er een nieuw item wordt toegevoegd.
4. Het serverproject publiceren.

### <a name="nodejs"></a>Node.js-back-end-project
1. Als u dit nog niet hebt gedaan, [downloaden van het snelstartproject dat back-end](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), of gebruik anders de [online-editor in Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Vervang de bestaande code in todoitem.js door het volgende:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
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

    Dit stuurt de melding van een sjabloon waarin de item.text wanneer een nieuw item wordt ingevoerd.
3. Bij het bewerken van het bestand op uw lokale computer opnieuw publiceren het serverproject.
