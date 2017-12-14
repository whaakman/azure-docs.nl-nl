## <a name="webapi-project"></a>WebAPI-Project
1. Open in Visual Studio de **AppBackend** project dat u hebt gemaakt in de **gebruikers waarschuwen** zelfstudie.
2. Vervang in Notifications.cs, het gehele **meldingen** klasse met de volgende code. Zorg ervoor dat de tijdelijke aanduidingen vervangt door uw verbindingsreeks (met volledige toegang) voor uw notification hub en de naam van de hub. U vindt deze waarden uit de [Azure-portal](http://portal.azure.com). Deze module vertegenwoordigt nu de verschillende beveiligde meldingen die worden verzonden. Op een volledige implementatie worden de meldingen opgeslagen in een database. voor het gemak opslaan in dit geval wordt deze in het geheugen.
   
        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications
        {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",     "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

1. In NotificationsController.cs, vervang de code in de **NotificationsController** klasse-definitie maken met de volgende code. Dit onderdeel een manier voor het apparaat voor het ophalen van de melding veilig geïmplementeerd, en biedt ook een manier (voor de doeleinden van deze zelfstudie) om te activeren van een beveiligde push op uw apparaten. Houd er rekening mee dat wanneer u de melding verzendt met de notification hub, wordt alleen de een onbewerkte melding met de ID van de melding (en geen daadwerkelijke bericht) verzenden:
   
       public NotificationsController()
       {
           Notifications.Instance.CreateNotification("This is a secure notification!");
       }
   
       // GET api/notifications/id
       public Notification Get(int id)
       {
           return Notifications.Instance.ReadNotification(id);
       }
   
       public async Task<HttpResponseMessage> Post()
       {
           var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
           var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
           // windows
           var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                           new Dictionary<string, string> {
                               {"X-WNS-Type", "wns/raw"}
                           });
           await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);
   
           // apns
           await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);
   
           // gcm
           await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }


Houd er rekening mee dat de `Post` methode verzendt nu geen toast-melding. Een onbewerkte melding met de ID van de melding en geen gevoelige inhoud wordt verzonden. Controleer ook of de verzendbewerking voor de platforms die u geen referenties zijn geconfigureerd op uw notification hub hebt als ze in fouten resulteren opmerkingen te maken.

1. Er wordt nu opnieuw deze app wordt een Azure-Website zodat deze toegankelijk is vanaf alle apparaten implementeren. Klik met de rechtermuisknop op het project **AppBackend** en selecteer **Publiceren**.
2. Selecteer de Azure-Website als uw doel publiceren. Meld u aan met uw Azure-account en selecteer een bestaande of nieuwe Website en noteer de **doel-URL** eigenschap in de **verbinding** tabblad. Naar deze URL wordt verderop in deze zelfstudie verwezen als uw *back-endeindpunt*. Klik op **Publish**.

