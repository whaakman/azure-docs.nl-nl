## <a name="create-the-webapi-project"></a>Het WebAPI-project maken
In de volgende secties wordt een nieuwe ASP.NET WebAPI-back-end gemaakt. Dit heeft drie hoofddoelen:

1. **Clients verifiëren**: later wordt een berichtenhandler toegevoegd voor het verifiëren van aanvragen van clients en het koppelen van de gebruiker aan de aanvraag.
2. **Registraties van clientmeldingen**: later voegt u een controller toe voor het afhandelen van nieuwe registraties voor een clientapparaat voor het ontvangen van meldingen. De naam van de geverifieerde gebruiker wordt automatisch aan de registratie toegevoegd als een [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx).
3. **Meldingen verzenden naar clients**: later voegt u ook een controller toe om gebruikers een manier te bieden voor het activeren van een beveiligde push naar apparaten en clients die zijn gekoppeld aan de tag. 

In de volgende stappen ziet u hoe u de nieuwe ASP.NET WebAPI-back-end maakt: 

> [!IMPORTANT]
> Als u Visual Studio 2015 of eerder gebruikt, moet u voordat u deze zelfstudie begint controleren of u de meest recente versie van NuGet Package Manager hebt geïnstalleerd. Start Visual Studio om dit te controleren. Klik vanuit het menu **Tools** op **Extensions and Updates**. Zoek naar **NuGet Package Manager** voor uw versie van Visual Studio en controleer of u de meest recente versie hebt. Als dit niet het geval is, verwijder dan NuGet Package Manager en installeer het opnieuw.
> 
> ![][B4]
> 
> [!NOTE]
> Controleer of u de Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) voor website-implementatie hebt geïnstalleerd.
> 
> 

1. Start Visual Studio of Visual Studio Express. Klik op **Server Explorer** en meld u aan bij uw Azure-account. Voor Visual Studio moet u aangemeld zijn om de website-resources voor uw account te maken.
2. Klik in Visual Studio op **Bestand**, klik vervolgens op **Nieuw** en daarna op **Project**, vouw **Sjablonen**, **Visual C#** uit en klik op **Web** en **ASP.NET-webtoepassing**, typ de naam **AppBackend** en klik op **OK**. 
   
    ![][B1]
3. Klik in het dialoogvenster **Nieuw ASP.NET-project** op **Web-API** en daarna op **OK**.
   
    ![][B2]
4. Kies in het dialoogvenster **Microsoft Azure-web-app configureren** een abonnement en een **App Service-abonnement** dat u al hebt gemaakt. U kunt ook **Nieuw App Service-plan opstellen** kiezen en er een via het dialoogvenster maken. U hebt geen database nodig voor deze zelfstudie. Nadat u uw App Service-plan hebt geselecteerd, klikt u op **OK** om het project te maken.
   
    ![][B5]

## <a name="authenticating-clients-to-the-webapi-backend"></a>Clients verifiëren bij de WebAPI-back-end
In deze sectie maakt u een nieuwe berichtenhandlerklasse met de naam **AuthenticationTestHandler** voor de nieuwe back-end. Deze klasse wordt afgeleid van [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) en toegevoegd als een berichtenhandler zodat deze alle aanvragen die de back-end ontvangt, kan verwerken. 

1. Klik in Solution Explorer met de rechtermuisknop op het project **AppBackend**, klik op **Toevoegen** en klik vervolgens op **Klasse**. Geef de nieuwe klasse de naam **AuthenticationTestHandler.cs**, en klik op **Toevoegen** om de klasse te genereren. Deze klasse wordt gebruikt voor verificatie van gebruikers met behulp van *Basisverificatie* om het eenvoudig te houden. Uw app kan een willekeurig verificatieschema gebruiken.
2. Voeg in AuthenticationTestHandler.cs de volgende `using`-instructies toe:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. Vervang in AuthenticationTestHandler.cs de `AuthenticationTestHandler`-klassedefinitie door de volgende code. 
   
    Deze handler verifieert de aanvraag wanneer de volgende drie voorwaarden alle waar zijn:
   
   * De aanvraag bevatte een *Autorisatie*header. 
   * De aanvraag maakt gebruik van *basis*verificatie. 
   * De gebruikersnaamtekenreeks en de wachtwoordtekenreeks zijn dezelfde tekenreeks.
     
     Anders wordt de aanvraag geweigerd. Dit is geen bestaande benadering op verificatie en autorisatie. Dit is slechts een zeer eenvoudig voorbeeld voor deze zelfstudie.
     
     Als het aanvraagbericht is geverifieerd en geautoriseerd door de `AuthenticationTestHandler`, wordt de basisverificatiegebruiker toegevoegd aan de huidige aanvraag in de [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Gebruikersgegevens in de HttpContext worden later gebruikt door een andere controller (RegisterController) om een [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx) toe te voegen aan de aanvraag voor meldingen van registraties.
     
       public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
               if (authorizationHeader != null && authorizationHeader
                   .StartsWith("Basic ", StringComparison.InvariantCultureIgnoreCase))
               {
                   string authorizationUserAndPwdBase64 =
                       authorizationHeader.Substring("Basic ".Length);
                   string authorizationUserAndPwd = Encoding.Default
                       .GetString(Convert.FromBase64String(authorizationUserAndPwdBase64));
                   string user = authorizationUserAndPwd.Split(':')[0];
                   string password = authorizationUserAndPwd.Split(':')[1];
     
                   if (verifyUserAndPwd(user, password))
                   {
                       // Attach the new principal object to the current HttpContext object
                       HttpContext.Current.User =
                           new GenericPrincipal(new GenericIdentity(user), new string[0]);
                       System.Threading.Thread.CurrentPrincipal =
                           System.Web.HttpContext.Current.User;
                   }
                   else return Unauthorized();
               }
               else return Unauthorized();
     
               return base.SendAsync(request, cancellationToken);
           }
     
           private bool verifyUserAndPwd(string user, string password)
           {
               // This is not a real authentication scheme.
               return user == password;
           }
     
           private Task<HttpResponseMessage> Unauthorized()
           {
               var response = new HttpResponseMessage(HttpStatusCode.Forbidden);
               var tsc = new TaskCompletionSource<HttpResponseMessage>();
               tsc.SetResult(response);
               return tsc.Task;
           }
       }
     
     > [!NOTE]
     > **Opmerking over beveiliging**: de `AuthenticationTestHandler`-klasse biedt geen echte verificatie. Het wordt alleen gebruikt om basisverificatie na te bootsen en is niet beveiligd. U moet een veilig verificatiemechanisme implementeren in uw productietoepassingen en -services.                
     > 
     > 
4. Voeg de volgende code toe aan het einde van de `Register`-methode in de klasse **App_Start/WebApiConfig.cs** om de berichtenhandler te registreren:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. Sla uw wijzigingen op.

## <a name="registering-for-notifications-using-the-webapi-backend"></a>Registreren voor meldingen met behulp van de WebAPI-back-end
In deze sectie voegen we een nieuwe controller toe aan de WebAPI-back-end om aanvragen voor het registreren van een gebruiker en apparaat voor meldingen af te handelen met behulp van de clientbibliotheek voor notification hubs. De controller voegt een gebruikerstag toe voor de gebruiker die door de `AuthenticationTestHandler` is geverifieerd en gekoppeld aan de HttpContext. De tag heeft de indeling van de tekenreeks, `"username:<actual username>"`.

1. Klik in Solution Explorer met de rechtermuisknop op het project **AppBackend** en klik vervolgens op **NuGet-pakketten beheren**.
2. Klik aan de linkerkant op **Online** en zoek naar **Microsoft.Azure.NotificationHubs** in het vak **Zoeken**.
3. Klik in de lijst met resultaten op **Microsoft Azure Notification Hubs**, en klik vervolgens op **installeren**. Voltooi de installatie en sluit vervolgens het venster Nuget Package Manager.
   
    Hiermee wordt een verwijzing toegevoegd aan de Azure Notification Hubs SDK met het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-pakket</a>.
4. We gaan nu een nieuw klassebestand maken waarmee de verbinding met de notification hub wordt gebruikt voor het verzenden van meldingen. Klik in Solution Explorer met de rechtermuisknop op de map **Modellen** en klik achtereenvolgens op **Toevoegen** en **Klasse**. Noem de nieuwe klasse **Notifications.cs** en klik vervolgens op **Toevoegen** om de klasse te genereren. 
   
    ![][B6]
5. Voeg in Notifications.cs de volgende `using`-instructie toe aan het begin van het bestand:
   
        using Microsoft.Azure.NotificationHubs;
6. Vervang de `Notifications`-klassedefinitie maken met het volgende en zorg ervoor dat u de twee tijdelijke aanduidingen vervangt door de verbindingsreeks (met volledige toegang) voor uw notification hub en de naam van de hub (beschikbaar in de [klassieke Azure-portal](http://manage.windowsazure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Vervolgens maken we een nieuwe controller met de naam **RegisterController**. Klik in Solution Explorer met de rechtermuisknop op de map **Controllers** en klik achtereenvolgens op **Toevoegen** en **Controller**. Klik op het item **Web API 2-controller - leeg** item en klik vervolgens op **Toevoegen**. Noem de nieuwe klasse **RegisterController** en klik vervolgens opnieuw op **Toevoegen** om de controller te genereren.
   
    ![][B7]
   
    ![][B8]
8. Voeg in RegisterController.cs de volgende `using`-instructies toe:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. Voeg de volgende code in de `RegisterController`-klassedefinitie. In deze code voegen we een gebruikerstag toe voor de gebruiker die is gekoppeld aan de HttpContext. De gebruiker is geverifieerd en gekoppeld aan de HttpContext door het berichtenfilter dat we hebben toegevoegd, `AuthenticationTestHandler`. U kunt ook optionele controles toevoegen om te controleren of de gebruiker rechten heeft voor het registreren voor de aangevraagde tags.
   
        private NotificationHubClient hub;
   
        public RegisterController()
        {
            hub = Notifications.Instance.Hub;
        }
   
        public class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }
   
        // POST api/register
        // This creates a registration id
        public async Task<string> Post(string handle = null)
        {
            string newRegistrationId = null;
   
            // make sure there are no existing registrations for this push handle (used for iOS and Android)
            if (handle != null)
            {
                var registrations = await hub.GetRegistrationsByChannelAsync(handle, 100);
   
                foreach (RegistrationDescription registration in registrations)
                {
                    if (newRegistrationId == null)
                    {
                        newRegistrationId = registration.RegistrationId;
                    }
                    else
                    {
                        await hub.DeleteRegistrationAsync(registration);
                    }
                }
            }
   
            if (newRegistrationId == null) 
                newRegistrationId = await hub.CreateRegistrationIdAsync();
   
            return newRegistrationId;
        }
   
        // PUT api/register/5
        // This creates or updates a registration (with provided channelURI) at the specified id
        public async Task<HttpResponseMessage> Put(string id, DeviceRegistration deviceUpdate)
        {
            RegistrationDescription registration = null;
            switch (deviceUpdate.Platform)
            {
                case "mpns":
                    registration = new MpnsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "wns":
                    registration = new WindowsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "apns":
                    registration = new AppleRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "gcm":
                    registration = new GcmRegistrationDescription(deviceUpdate.Handle);
                    break;
                default:
                    throw new HttpResponseException(HttpStatusCode.BadRequest);
            }
   
            registration.RegistrationId = id;
            var username = HttpContext.Current.User.Identity.Name;
   
            // add check if user is allowed to add these tags
            registration.Tags = new HashSet<string>(deviceUpdate.Tags);
            registration.Tags.Add("username:" + username);
   
            try
            {
                await hub.CreateOrUpdateRegistrationAsync(registration);
            }
            catch (MessagingException e)
            {
                ReturnGoneIfHubResponseIsGone(e);
            }
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        // DELETE api/register/5
        public async Task<HttpResponseMessage> Delete(string id)
        {
            await hub.DeleteRegistrationAsync(id);
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        private static void ReturnGoneIfHubResponseIsGone(MessagingException e)
        {
            var webex = e.InnerException as WebException;
            if (webex.Status == WebExceptionStatus.ProtocolError)
            {
                var response = (HttpWebResponse)webex.Response;
                if (response.StatusCode == HttpStatusCode.Gone)
                    throw new HttpRequestException(HttpStatusCode.Gone.ToString());
            }
        }
10. Sla uw wijzigingen op.

## <a name="sending-notifications-from-the-webapi-backend"></a>Meldingen verzenden vanuit de WebAPI-back-end
In deze sectie voegt u een nieuwe controller toe die een manier biedt voor clientapparaten om een melding te verzenden op basis van de gebruikersnaamtag met behulp van de Azure Notification Hubs Service Management-bibliotheek in de ASP.NET WebAPI-back-end.

1. Maak een andere nieuwe controller met de naam **NotificationsController**. Maak deze op dezelfde manier als waarop u **RegisterController** in de vorige sectie hebt gemaakt.
2. Voeg in NotificationsController.cs de volgende `using`-instructies toe:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. Voeg de volgende methode toe aan de klasse **NotificationsController**.
   
    Met deze code wordt een meldingstype verzonden op basis van de `pns`-parameter (Platform Notification Service, PNS). De waarde van `to_tag` wordt gebruikt om de tag *gebruikersnaam* in te stellen in het bericht. Deze tag moet overeenkomen met een gebruikersnaamtag van een actieve notification hub-registratie. Het meldingsbericht wordt opgehaald uit de hoofdtekst van de POST-aanvraag en geformatteerd voor de doel-PNS. 
   
    Afhankelijk van de PNS (Platform Notification Service) die uw ondersteunde apparaten gebruiken om meldingen te ontvangen, worden andere meldingen met verschillende indelingen ondersteund. Bijvoorbeeld op Windows-apparaten kunt u een [pop-upmelding met WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) gebruiken die niet rechtstreeks wordt ondersteund door een andere PNS. Uw back-end moet de melding dus in een ondersteunde melding indelen voor de PNS van apparaten die u wilt ondersteunen. Gebruik vervolgens de juiste API voor verzending in de [NotificationHubClient-klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)
   
        public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag)
        {
            var user = HttpContext.Current.User.Identity.Name;
            string[] userTag = new string[2];
            userTag[0] = "username:" + to_tag;
            userTag[1] = "from:" + user;
   
            Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;
            HttpStatusCode ret = HttpStatusCode.InternalServerError;
   
            switch (pns.ToLower())
            {
                case "wns":
                    // Windows 8.1 / Windows Phone 8.1
                    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
                                "From " + user + ": " + message + "</text></binding></visual></toast>";
                    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
                    break;
                case "apns":
                    // iOS
                    var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);
                    break;
                case "gcm":
                    // Android
                    var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendGcmNativeNotificationAsync(notif, userTag);
                    break;
            }
   
            if (outcome != null)
            {
                if (!((outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Abandoned) ||
                    (outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Unknown)))
                {
                    ret = HttpStatusCode.OK;
                }
            }
   
            return Request.CreateResponse(ret);
        }
4. Druk op **F5** om de toepassing uit te voeren en om de juistheid van uw werk tot nu toe te controleren. De app moet een webbrowser starten en de ASP.NET-startpagina weergeven. 

## <a name="publish-the-new-webapi-backend"></a>De nieuwe WebAPI-back-end publiceren
1. We gaan nu deze app implementeren op een Azure-website zodat deze toegankelijk is vanaf alle apparaten. Klik met de rechtermuisknop op het project **AppBackend** en selecteer **Publiceren**.
2. Selecteer **Microsoft Azure App Service** als publicatiedoel en klik vervolgens op **Publiceren**. Hiermee opent u het dialoogvenster App Service maken, waarmee u alle Azure-resources kunt maken die nodig zijn voor het uitvoeren van uw ASP.NET-web-app in Azure.

    ![][B15]
3. Selecteer uw Azure-account in het dialoogvenster **App Service maken**. Klik op **Type wijzigen** en selecteer **Web-app**. Houd de gegeven **Web-appnaam** en selecteer het **Abonnement**, de **Resourcegroep** en het **App Service-plan**.  Klik op **Create**.

4. Noteer de **Site-URL**-eigenschap in de sectie **Samenvatting**. Naar deze URL wordt verderop in deze zelfstudie verwezen als uw *back-endeindpunt*. Klik op **Publish**.

5. Zodra de wizard is voltooid, wordt de ASP.NET-web-app naar Azure gepubliceerd. Daarna wordt de app gestart in de standaardbrowser.  Uw toepassing is zichtbaar in Azure App Services.

De URL maakt gebruik van de web-appnaam die u eerder hebt opgegeven, met de notatie http://<app_naam>.azurewebsites.net.

[B1]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push1.png
[B2]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push2.png
[B3]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push3.png
[B4]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push4.png
[B5]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push5.png
[B6]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push6.png
[B7]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push7.png
[B8]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push8.png
[B14]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push14.png
[B15]: ./media/notification-hubs-aspnet-backend-notifyusers/publish-to-app-service.png
[B16]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users16.PNG
[B18]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users18.PNG
