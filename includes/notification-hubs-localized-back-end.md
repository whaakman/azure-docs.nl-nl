



Bij het verzenden van Sjabloonmeldingen u alleen hoeft te bieden van een set eigenschappen in ons geval ontvangt van ons de set eigenschappen die de gelokaliseerde versie van het huidige nieuws voor exemplaar:

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


Deze sectie wordt beschreven hoe u meldingen met een console-app verzenden

De opgenomen code zendt voor Windows Store en iOS-apparaten, omdat de back-end naar een van de ondersteunde apparaten verzenden kunt.

### <a name="to-send-notifications-using-a-c-console-app"></a>Voor het verzenden van meldingen via een C#-consoletoepassing
Wijzig de `SendTemplateNotificationAsync` methode in de console-app die u eerder hebt gemaakt met de volgende code. U ziet hoe in dit geval hoeft niet meerdere meldingen voor verschillende talen en platforms wilt verzenden.

        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending the notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
            // This includes APNS, GCM, WNS, and MPNS template registrations.
            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
            var locales = new string[] { "English", "French", "Mandarin" };

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";

                // Sending localized News for each tag too...
                foreach( var locale in locales)
                {
                    string key = "News_" + locale;

                    // Your real localized news content would go here.
                    templateParams[key] = "Breaking " + category + " News in " + locale + "!";
                }

                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
        }


Houd er rekening mee dat deze eenvoudige aanroep de gelokaliseerde stukje nieuws levert **alle** uw apparaten, ongeacht het platform, zoals uw Notification Hub bouwt en levert de juiste systeemeigen nettolading voor alle apparaten die zijn geabonneerd op een specifieke label.

### <a name="sending-the-notification-with-mobile-services"></a>Verzenden van de melding met Mobile Services
In de planner van uw mobiele Service, kunt u het volgende script:

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });


