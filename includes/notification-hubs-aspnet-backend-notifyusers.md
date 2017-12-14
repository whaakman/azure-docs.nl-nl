## <a name="create-the-webapi-project"></a>Het WebAPI-project maken
De volgende secties worden besproken voor het maken van een nieuwe ASP.NET WebAPI back-end. Dit proces heeft drie hoofddoelen:

* **Clients verifiëren**: toevoegen van een berichtenhandler later voor het authenticeren van aanvragen van clients en de gebruiker koppelen aan de aanvraag.

* **Registreren voor meldingen met behulp van de WebAPI back-end**: toevoegen van een domeincontroller voor het afhandelen van nieuwe registraties voor een client-apparaat om meldingen te ontvangen. De geverifieerde gebruikersnaam wordt automatisch toegevoegd aan de registratie als een [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx).

* **Meldingen verzenden naar clients**: U ook een domeincontroller bieden een manier voor gebruikers voor het activeren van een beveiligde push naar apparaten en clients die zijn gekoppeld aan de tag toevoegen. 

Maak de back-end van de nieuwe ASP.NET WebAPI als volgt: 

> [!IMPORTANT]
> Als u van Visual Studio 2015 gebruikmaakt of eerder, voordat u deze zelfstudie begint, zorg ervoor dat kunt u de meest recente versie van NuGet Package Manager voor Visual Studio hebt geïnstalleerd. 
>
>Start Visual Studio om dit te controleren. Op de **extra** selecteert u **uitbreidingen en Updates**. Zoeken naar **NuGet Package Manager** in uw versie van Visual Studio en zorg ervoor dat u de meest recente versie hebt. Als uw versie niet de nieuwste versie is, verwijdert u deze en installeer het NuGet Package Manager.
 
![][B4]

> [!NOTE]
> Controleer of u de Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) voor website-implementatie hebt geïnstalleerd.
> 
> 

1. Start Visual Studio of Visual Studio Express. 

2. Selecteer **Server Explorer**, en meld u aan bij uw Azure-account. Voor het maken van de website-resources voor uw account, moet u zijn aangemeld.

3. Selecteer in Visual Studio **bestand** > **nieuw** > **Project**, vouw **sjablonen**, vouw **Visual C#**, en selecteer vervolgens **Web** en **ASP.NET-webtoepassing**.

4. In de **naam** in het vak **AppBackend**, en selecteer vervolgens **OK**. 
   
    ![Het venster Nieuw Project][B1]

5. In de **nieuw ASP.NET-Project** Selecteer de **Web API** selectievakje en selecteer vervolgens **OK**.
   
    ![Het venster nieuw ASP.NET-Project][B2]

6. In de **Microsoft Azure Web-App configureren** venster, selecteert u een abonnement en klikt u op de **App Service-abonnement** lijst, een van de volgende handelingen uit:

    * Selecteer een app service-abonnement dat u al hebt gemaakt. 
    * Selecteer **maken van een nieuwe app service-abonnement**, en maak vervolgens een. 
    
  U hebt geen database nodig voor deze zelfstudie. Nadat u uw app service-abonnement hebt geselecteerd, selecteert u **OK** om het project te maken.
   
    ![Het venster Microsoft Azure Web-App configureren][B5]

## <a name="authenticate-clients-to-the-webapi-back-end"></a>Verificatie van clients naar de WebAPI-back-end
In deze sectie maakt u een nieuwe berichtenhandler klasse met de naam **AuthenticationTestHandler** voor de nieuwe back-end. Deze klasse wordt afgeleid van [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) en toegevoegd als een berichtenhandler zodat deze alle verwerken kan die afkomstig zijn in de back-end aanvragen. 

1. Klik in Solution Explorer met de rechtermuisknop op de **AppBackend** project, selecteer **toevoegen**, en selecteer vervolgens **klasse**. 
 
2. Naam van de nieuwe klasse **AuthenticationTestHandler.cs**, en selecteer vervolgens **toevoegen** voor het genereren van de klasse. Verificatie van gebruikers met behulp van deze klasse *basisverificatie* voor eenvoud. Uw app kunt gebruiken een verificatieschema.

3. Voeg in AuthenticationTestHandler.cs de volgende `using`-instructies toe:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

4. Vervang in AuthenticationTestHandler.cs, de `AuthenticationTestHandler` klasse-definitie maken met de volgende code: 
   
    De handler machtigen van de aanvraag bij de volgende drie voorwaarden voldaan wordt:
   
   * De aanvraag bevat een *autorisatie* header. 
   * De aanvraag maakt gebruik van *basis*verificatie. 
   * De gebruikersnaamtekenreeks en de wachtwoordtekenreeks zijn dezelfde tekenreeks.
     
  Anders wordt de aanvraag geweigerd. Dit is geen bestaande benadering op verificatie en autorisatie. Dit is slechts een zeer eenvoudige voorbeeld voor deze zelfstudie.
     
  Als het request-bericht is geverifieerd en gemachtigd door `AuthenticationTestHandler`, de basisverificatie-gebruiker is gekoppeld aan de huidige aanvraag op [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Gebruikersgegevens in HttpContext wordt gebruikt door een andere domeincontroller (RegisterController) later om toe te voegen een [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx) op de melding registratie-aanvraag.
     
       public class AuthenticationTestHandler : DelegatingHandler
       {
           protected override Task<HttpResponseMessage> SendAsync(
           HttpRequestMessage request, CancellationToken cancellationToken)
           {
               var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
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
     > Opmerking over beveiliging: de `AuthenticationTestHandler` klasse biedt geen true verificatie. Het wordt alleen gebruikt om basisverificatie na te bootsen en is niet beveiligd. U moet een veilig verificatiemechanisme implementeren in uw productietoepassingen en -services.                
     > 
     > 
5. De volgende code voor het registreren van de berichtenhandler toevoegen aan het einde van de `Register` methode in de **App_Start/WebApiConfig.cs** klasse:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());

6. Sla uw wijzigingen op.

## <a name="register-for-notifications-by-using-the-webapi-back-end"></a>Registreren voor meldingen met behulp van de WebAPI-back-end
In deze sectie kunt u een nieuwe domeincontroller toevoegen aan de WebAPI-back-end voor het aanvragen van een gebruiker en een apparaat voor meldingen registreren met behulp van de clientbibliotheek voor notification hubs. De controller voegt een gebruikerstag voor de gebruiker die is geverifieerd en gekoppeld aan HttpContext door `AuthenticationTestHandler`. De tag heeft de indeling van de tekenreeks, `"username:<actual username>"`.

1. Klik in Solution Explorer met de rechtermuisknop op de **AppBackend** project en selecteer vervolgens **NuGet-pakketten beheren**.

2. Selecteer in het linkerdeelvenster **Online** en klikt u op de **Search** in het vak **Microsoft.Azure.NotificationHubs**.

3. Selecteer in de lijst met resultaten **Microsoft Azure Notification Hubs**, en selecteer vervolgens **installeren**. De installatie te voltooien en sluit het venster NuGet Package Manager.
   
    Met deze actie wordt een verwijzing toegevoegd aan de Azure Notification Hubs-SDK met het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-pakket</a>.

4. Maak een nieuwe klassebestand waarmee de verbinding met de notification hub die wordt gebruikt om meldingen te verzenden. Klik in Solution Explorer met de rechtermuisknop op de **modellen** map, selecteer **toevoegen**, en selecteer vervolgens **klasse**. Naam van de nieuwe klasse **Notifications.cs**, en selecteer vervolgens **toevoegen** voor het genereren van de klasse. 
   
    ![Het venster Nieuw Item toevoegen][B6]

5. Voeg in Notifications.cs de volgende `using`-instructie toe aan het begin van het bestand:
   
        using Microsoft.Azure.NotificationHubs;

6. Vervang de `Notifications` klasse-definitie maken met de volgende code en vervang de twee tijdelijke aanduidingen door de verbindingsreeks (met volledige toegang) voor uw notification hub en de naam van de hub (beschikbaar op [Azure-portal](http://portal.azure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Maak vervolgens een nieuwe domeincontroller met de naam **RegisterController**. Klik in Solution Explorer met de rechtermuisknop op de **domeincontrollers** map, selecteer **toevoegen**, en selecteer vervolgens **Controller**. 

8. Selecteer **Web API 2-Controller - leeg**, en selecteer vervolgens **toevoegen**.
   
    ![Het venster Add Scaffold][B7]
   
9. In de **controllernaam** in het vak **RegisterController** als naam voor de nieuwe klasse en selecteer vervolgens **toevoegen**.

    ![Het venster Controller toevoegen][B8]

10. Voeg in RegisterController.cs de volgende `using`-instructies toe:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

11. Voeg de volgende code in de `RegisterController`-klassedefinitie. Houd er rekening mee dat in deze code we een label van de gebruiker voor de gebruiker die gekoppeld aan HttpContext toevoegen. De gebruiker is geverifieerd en gekoppeld zijn aan HttpContext door het berichtenfilter die wordt toegevoegd, `AuthenticationTestHandler`. U kunt ook optionele controles toevoegen om te controleren of de gebruiker rechten heeft voor het registreren voor de aangevraagde tags.
   
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
12. Sla uw wijzigingen op.

## <a name="send-notifications-from-the-webapi-back-end"></a>Meldingen verzenden vanuit de WebAPI-back-end
In deze sectie voegt u een nieuwe domeincontroller waarmee een manier voor clientapparaten een melding te verzenden. De melding is gebaseerd op de gebruikersnaam-tag die gebruikmaakt van Azure Notification Hubs-Service Management-bibliotheek in de ASP.NET WebAPI-back-end.

1. Maak een andere nieuwe domeincontroller met de naam **NotificationsController** dezelfde manier als u hebt gemaakt **RegisterController** in de vorige sectie.

2. Voeg in NotificationsController.cs de volgende `using`-instructies toe:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

3. Voeg de volgende methode voor de **NotificationsController** klasse:
   
    Deze code verzendt een meldingstype dat gebaseerd op het Platform Notification Service (PNS) `pns` parameter. De waarde van `to_tag` wordt gebruikt om de tag *gebruikersnaam* in te stellen in het bericht. Deze tag moet overeenkomen met een gebruikersnaamtag van een actieve notification hub-registratie. Het meldingsbericht wordt opgehaald uit de hoofdtekst van de POST-aanvraag en geformatteerd voor de doel-PNS. 
   
    Afhankelijk van de PNS dat uw ondersteunde apparaten gebruiken om meldingen te ontvangen, worden de meldingen worden ondersteund door verschillende bestandsindelingen. Bijvoorbeeld, op Windows-apparaten, kunt u een [toasten we melding met WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) die rechtstreeks wordt niet ondersteund door een andere PNS. In een exemplaar moet uw back-end van de melding in een ondersteunde melding voor de PNS van apparaten dat u van plan bent te formatteren. Gebruik de juiste API verzenden op de [NotificationHubClient klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx).
   
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

4. Uitvoeren van de toepassing en controleer de juistheid van uw werk tot nu toe, selecteer de **F5** sleutel. De app opent een webbrowser en deze wordt weergegeven op de startpagina van ASP.NET. 

## <a name="publish-the-new-webapi-back-end"></a>De back-end van de nieuwe WebAPI publiceren
U kunt vervolgens de app implementeren naar een Azure-website zodat deze toegankelijk is vanaf alle apparaten. 

1. Met de rechtermuisknop op de **AppBackend** project en selecteer vervolgens **publiceren**.

2. Selecteer **Microsoft Azure App Service** als uw publicatiedoel en selecteer vervolgens **publiceren**.  
    De Create App Service-venster wordt geopend. Hier kunt u alle benodigde Azure-resources voor het uitvoeren van de ASP.NET-web-app in Azure.

    ![De Microsoft Azure App Service-tegel][B15]

3. In de **Create App Service** venster, selecteer uw Azure-account. Selecteer **Type wijzigen** > **Web-App**. Gebruik de standaardwaarde **Web-Appnaam**, en selecteer vervolgens de **abonnement**, **resourcegroep**, en **App Service-Plan**. 

4. Selecteer **Maken**.

5. Noteer de **Site-URL**-eigenschap in de sectie **Samenvatting**. Deze URL is uw *back-end-eindpunt* verderop in de zelfstudie. 

6. Selecteer **publiceren**.

Nadat u de wizard hebt voltooid, wordt de ASP.NET-web-app gepubliceerd naar Azure en vervolgens wordt de app in de standaardbrowser geopend.  De toepassing is in Azure App Services kan worden weergegeven.

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
