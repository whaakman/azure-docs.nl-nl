---
author: spelluru
ms.service: service-bus
ms.topic: include
ms.date: 11/09/2018
ms.author: spelluru
ms.openlocfilehash: b8cf4217ca6c80be998b92e71c3ba29c4f68bce2
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52269693"
---
## <a name="webapi-project"></a>WebAPI-Project
1. Open in Visual Studio, de **AppBackend** project dat u hebt gemaakt in de **gebruikers waarschuwen** zelfstudie.
2. Vervang de gehele in Notifications.cs **meldingen** klasse met de volgende code. Zorg ervoor dat de tijdelijke aanduidingen vervangt door uw verbindingsreeks (met volledige toegang) voor uw notification hub en de naam van de hub. U vindt deze waarden uit de [Azure-portal](http://portal.azure.com). Deze module geeft nu de verschillende beveiligde meldingen die worden verzonden. Voor een volledige implementatie, worden de meldingen opgeslagen in een database. voor het gemak opslaan in dit geval we deze in het geheugen.
   
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

1. Voeg in NotificationsController.cs, vervang de code binnen de **NotificationsController** klassedefinitie maken met de volgende code. Dit onderdeel implementeert een manier voor het apparaat de melding veilig ophaalt, en biedt ook een manier (voor de doeleinden van deze zelfstudie) voor het activeren van een beveiligde push naar uw apparaten. Houd er rekening mee dat de melding verzenden naar de notification hub, we alleen wanneer een onbewerkte melding met de ID van de melding (en geen daadwerkelijke bericht verzenden):
   
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


Houd er rekening mee dat de `Post` methode verzendt nu geen een pop-upmelding. Stuurt een onbewerkte melding alleen de meldings-ID en niet alle gevoelige inhoud bevat. Zorg er ook opmerkingen van de bewerking voor het verzenden voor het platform waarvoor u referenties niet geconfigureerd op de notification hub, zijn zoals ze tot fouten leiden zal.

1. We gaan deze app naar een Azure-Website nu opnieuw zodat deze toegankelijk is vanaf alle apparaten implementeren. Klik met de rechtermuisknop op het project **AppBackend** en selecteer **Publiceren**.
2. Selecteer de Azure-Website als het doel van uw publiceren. Meld u aan met uw Azure-account en selecteer een bestaande of nieuwe Website en noteer de **doel-URL** eigenschap in de **verbinding** tabblad. Naar deze URL wordt verderop in deze zelfstudie verwezen als uw *back-endeindpunt*. Klik op **Publish**.

