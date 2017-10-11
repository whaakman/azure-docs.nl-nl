---
title: Registratie van beheer
description: In dit onderwerp wordt uitgelegd hoe u apparaten registreren met notification hubs kunnen pushmeldingen worden ontvangen.
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: fd0ee230-132c-4143-b4f9-65cef7f463a1
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: a1a349150ef4c7837932706f0c4fcc8d022ec7ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="registration-management"></a>Registratiebeheer
## <a name="overview"></a>Overzicht
In dit onderwerp wordt uitgelegd hoe u apparaten registreren met notification hubs kunnen pushmeldingen worden ontvangen. Het onderwerp wordt beschreven registraties op hoog niveau en vervolgens maakt u kennis met de twee belangrijkste patronen voor het registreren van apparaten: registreren van het apparaat rechtstreeks met de notification hub en registreren via een back-end voor de toepassing. 

## <a name="what-is-device-registration"></a>Wat is de registratie
Registreren van apparaten met een Notification Hub wordt bereikt met een **registratie** of **installatie**.

#### <a name="registrations"></a>Registraties
Een registratie koppelt de ingang Platform Notification Service (PNS) voor een apparaat met tags en mogelijk een sjabloon. De PNS-ingang is mogelijk een ChannelURI, apparaattoken of GCM-registratie-id. Labels worden gebruikt voor het routeren van meldingen naar de juiste set apparaat verwerkt. Zie voor meer informatie [Routering en code-expressies](notification-hubs-tags-segment-push-message.md). Sjablonen worden gebruikt voor de per-registratie transformatie implementeren. Zie voor meer informatie [sjablonen](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Installaties
Een installatie is een uitgebreide registratie met een groot aantal push gerelateerd eigenschappen. Het is de nieuwste en beste aanpak voor het registreren van uw apparaten. Echter niet ondersteund door de clientkant .NET SDK ([Notification Hub SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) vanaf nog.  Dit betekent dat als u vanaf het clientapparaat zelf registreert, moet u zou gebruiken de [Notification Hubs REST-API](https://msdn.microsoft.com/library/mt621153.aspx) benadering voor ondersteuning van installaties. Als u een back-endservice gebruikt, moet u gebruikmaken van [Notification Hub SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Hier volgen enkele belangrijke voordelen voor het gebruik van installaties:

* Maken of bijwerken van een installatie is volledig idempotent. Zo kunt u het opnieuw zonder vragen over dubbele registraties.
* Het installatiemodel kunt eenvoudig doen afzonderlijke pushes - doelen specifiek apparaat. Een Systeemlabel **' $InstallationId: [omwille van] '** automatisch met elke installatie op basis van de registratie wordt toegevoegd. U kunt dus een verzenden naar deze code op een specifiek apparaat zonder extra programmeren aanroepen.
* Met installaties kunt u doen gedeeltelijke registratie-updates. De gedeeltelijke update van een installatie wordt aangevraagd met een PATCH methode met de [JSON-Patch standaard](https://tools.ietf.org/html/rfc6902). Dit is bijzonder nuttig wanneer u wilt bijwerken, tags voor de registratie. U hoeft te halen de registratie van de gehele en verzend de vorige labels opnieuw.

Een installatie mag de volgende eigenschappen. Voor een volledig overzicht van de installatie-eigenschappen-Zie [maken of een installatie met REST-API worden overschreven](https://msdn.microsoft.com/library/azure/mt621153.aspx) of [installatie-eigenschappen](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) voor de.

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }



Het is belangrijk te weten dat registraties en installaties standaard niet langer verloopt.

Registraties en installaties moeten een geldige PNS-ingang voor elk apparaat/kanaal bevatten. Omdat de PNS-ingangen kunnen alleen worden verkregen in een client-app op het apparaat, wordt een patroon is rechtstreeks op het apparaat aan de clientapp registreren. Aan de andere kant beveiligingsoverwegingen en zakelijke logica die zijn gerelateerd aan tags u mogelijk voor het beheren van de apparaatregistratie in de back-end van de app. 

#### <a name="templates"></a>Sjablonen
Als u wilt gebruiken [sjablonen](notification-hubs-templates-cross-platform-push-messages.md), installatie van het apparaat ook bevatten alle sjablonen die zijn gekoppeld aan het apparaat in een JSON formatteren (Zie het bovenstaande voorbeeld). De sjabloonnamen helpen bij het doel van verschillende sjablonen voor hetzelfde apparaat.

Houd er rekening mee dat de sjabloonnaam van elke wordt toegewezen aan een instantie van de sjabloon en een optionele set van labels. Elk platform kunt bovendien aanvullende Sjablooneigenschappen hebben. Voor Windows Store (met WNS) en Windows Phone 8 (met behulp van MPNS), kan een extra set van headers deel uitmaken van de sjabloon. In het geval van APNs, kunt u een eigenschap verloopdatum instellen moet een constante of een sjabloonexpressie. Voor een volledig overzicht van de installatie-eigenschappen-Zie [maken of een installatie met REST overschrijven](https://msdn.microsoft.com/library/azure/mt621153.aspx) onderwerp.

#### <a name="secondary-tiles-for-windows-store-apps"></a>Secundaire tegels voor Windows Store-Apps
Voor toepassingen voor Windows Store-client, verzenden van meldingen naar secundaire tegels is hetzelfde als ze worden verzonden naar de primaire alias. Dit wordt ook ondersteund in installaties. Houd er rekening mee dat secundaire tegels hebben een verschillende ChannelUri die de SDK op uw clientapp transparant verwerkt.

Het woordenboek SecondaryTiles maakt gebruik van de dezelfde TileId die wordt gebruikt voor het object SecondaryTiles in uw Windows Store-app niet maken.
Net als bij de primaire ChannelUri kunt ChannelUris secundaire tegels op elk moment wijzigen. Om de installaties behouden in de notification hub is bijgewerkt, moet het apparaat te vernieuwen met de huidige ChannelUris van de secundaire tegels.

## <a name="registration-management-from-the-device"></a>Beheer van de registratie van het apparaat
Bij het beheren van de registratie van de client-apps, is alleen de back-end die verantwoordelijk is voor het verzenden van meldingen. Client-apps PNS-ingangen up-to-date te houden en registreer labels. De volgende afbeelding ziet u dit patroon.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Het apparaat eerst de PNS-ingang opgehaald uit de PNS vervolgens rechtstreeks door de notification hub geregistreerd. Nadat de registratie geslaagd is, kan de back-end voor de app een melding die gericht is op deze inschrijving verzenden. Zie voor meer informatie over het verzenden van meldingen [Routering en code-expressies](notification-hubs-tags-segment-push-message.md).
Merk op dat in dit geval gebruikt u luistert alleen rechten voor toegang tot uw notification hubs van het apparaat. Zie voor meer informatie [beveiliging](notification-hubs-push-notification-security.md).

Registreren van het apparaat is de eenvoudigste methode, maar er enkele nadelen.
Het eerste nadeel is dat een client-app alleen de labels bijwerken kunt wanneer de app actief is. Bijvoorbeeld, als een gebruiker heeft twee apparaten die geregistreerd labels die gerelateerd zijn aan sport-teams, wanneer het eerste apparaat voor een aanvullende code (bijvoorbeeld Seahawks registreert), zal het tweede apparaat het geen meldingen ontvangen over de Seahawks totdat de app op het tweede apparaat is een tweede keer uitgevoerd. Meer in het algemeen als labels worden beïnvloed door meerdere apparaten, is het beheren van de labels van de back-end een wenselijk optie.
Het tweede nadeel van beheer van de registratie van de client-app is dat extra aandacht beveiligen van de registratie aan specifieke tags is vereist omdat apps kunnen worden hacked, zoals wordt beschreven in de sectie "Tag beveiligingsniveau."

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Voorbeeldcode registreren bij een notification hub van een apparaat gebruikmaakt van een installatie
Op dit moment is dit wordt alleen ondersteund met behulp van de [Notification Hubs REST-API](https://msdn.microsoft.com/library/mt621153.aspx).

U kunt ook de PATCH methode met behulp van de [JSON-Patch standaard](https://tools.ietf.org/html/rfc6902) voor het bijwerken van de installatie.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Voorbeeldcode registreren bij een notification hub van een apparaat een registratie
Deze methoden maken of bijwerken van een registratie voor het apparaat waarop ze worden aangeroepen. Dit betekent dat de registratie van de hele om bij te werken op de greep of de labels, moet overschrijven. Houd er rekening mee dat registraties tijdelijk is en zijn dus moet u altijd een betrouwbare archief met de huidige codes die een specifiek apparaat moet hebben.

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
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

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }

    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Beheer van de registratie van een back-end
Het beheer van registraties vanuit de back-end vereist aanvullende code schrijven. De app van het apparaat moet de bijgewerkte PNS-ingang naar de back-end telkens wanneer de app gestart (samen met tags en sjablonen) en de back-end moet bijwerken deze ingang op de notification hub opgeven. De volgende afbeelding ziet u dit ontwerp.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

De voordelen van het beheer van registraties vanuit de back-end zijn de mogelijkheid om te wijzigen van tags op registraties, zelfs wanneer de bijbehorende app op het apparaat is niet actief en de clientapp verifiëren voordat u een label toevoegt aan de registratie ervan.

#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Voorbeeldcode registreren bij een notification hub vanuit een back-end gebruikmaakt van een installatie
Het clientapparaat wordt nog steeds opgehaald van de PNS-ingang en relevante installatie-eigenschappen als voordat en aangepaste API aanroepen op de back-end die u kunt uitvoeren van de registratie en autoriseren van tags enzovoort. De back-end kunt gebruikmaken van de [Notification Hub SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

U kunt ook de PATCH methode met behulp van de [JSON-Patch standaard](https://tools.ietf.org/html/rfc6902) voor het bijwerken van de installatie.

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Voorbeeldcode registreren bij een notification hub van een apparaat een registratie-id
U kunt CRUDS basisbewerkingen op registraties uitvoeren van uw app-end. Bijvoorbeeld:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");

    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


De back-end moet verwerken gelijktijdigheid van taken tussen registratie-updates. Service Bus biedt Optimistisch gelijktijdigheidbeheer voor het beheer van de registratie. Dit is met het gebruik van ETag op beheerbewerkingen registratie geïmplementeerd op het niveau van HTTP. Deze functie is transparant gebruikt door de Microsoft-SDKs die Veroorzaak een exception als een update is geweigerd voor gelijktijdigheid redenen. De app-back-end is verantwoordelijk voor het verwerken van deze uitzonderingen en de update opnieuw, indien nodig.

