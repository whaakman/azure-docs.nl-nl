---
title: Registratiebeheer
description: In dit onderwerp wordt uitgelegd hoe u apparaten registreren met notification hubs om te kunnen ontvangen van pushmeldingen.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: fd0ee230-132c-4143-b4f9-65cef7f463a1
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.author: jowargo
ms.date: 01/23/2019
ms.openlocfilehash: 028e9a2973ed524037f6415d9e802f947458cfa6
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58166766"
---
# <a name="registration-management"></a>Registratiebeheer

## <a name="overview"></a>Overzicht

In dit onderwerp wordt uitgelegd hoe u apparaten registreren met notification hubs om te kunnen ontvangen van pushmeldingen. Het onderwerp wordt beschreven registraties op hoog niveau en introduceert de twee belangrijkste patronen voor het registreren van apparaten: registratie van het apparaat rechtstreeks met de notification hub en registreren via de back-end van een toepassing.

## <a name="what-is-device-registration"></a>Wat is het registreren van apparaten

Device Registration service met een Notification Hub wordt gerealiseerd met behulp van een **registratie** of **installatie**.

### <a name="registrations"></a>Registraties

Een registratie wordt gekoppeld aan de ingang Platform Notification Service (PNS) voor een apparaat met tags en mogelijk een sjabloon. De PNS-ingang is mogelijk een kanaal-URI, apparaattoken of FCM registratie-id. Labels worden gebruikt voor het routeren van meldingen naar de juiste set apparaat verwerkt. Zie voor meer informatie, [Routering en code-expressies](notification-hubs-tags-segment-push-message.md). Sjablonen worden voor het implementeren van per-registratie-transformatie gebruikt. Zie [Sjablonen](notification-hubs-templates-cross-platform-push-messages.md) voor meer informatie.

> [!NOTE]
> Azure Notification Hubs ondersteunt maximaal 60 tags per registratie.

### <a name="installations"></a>Installaties

Een installatie is een uitgebreide eigenschappen met betrekking tot inschrijving met een eigenschappenverzameling van pushmeldingen. Het is de meest recente en beste aanpak voor het registreren van uw apparaten. Echter niet ondersteund door de SDK voor .NET-clientzijde ([Notification Hub SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) nog.  Dit betekent dat als u vanaf het clientapparaat zelf registreert, moet u zou gebruiken de [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx) benadering voor de ondersteuning van installaties. Als u een back-endservice gebruikt, moet u kunnen gebruiken [Notification Hub SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Hier volgen enkele belangrijke voordelen voor het gebruik van installaties:

- Het maken of bijwerken van een installatie is volledig idempotent zijn. Zo kunt u het opnieuw zonder klachten over dubbele registraties.
- De installatiemodel kunt eenvoudig doen afzonderlijke pushes - die zijn gericht op specifieke apparaat. Een tag system **"$InstallationId: [installationId]"** wordt automatisch toegevoegd aan elke installatie op basis van registratie. U kunt dus een verzenden naar deze tag op een specifiek apparaat zonder dat u hoeft te doen extra coderen aanroepen.
- Met behulp van installaties kunt u een gedeeltelijke registratie-updates. De gedeeltelijke update van een installatie wordt aangevraagd met een PATCH-methode met de [JSON-Patch standard](https://tools.ietf.org/html/rfc6902). Dit is handig als u wilt bijwerken van tags op de registratie. U hoeft niet te opgehaald van de registratie van de gehele en verzend de vorige labels opnieuw.

Een installatie, kan de volgende eigenschappen bevatten. Zie voor een volledige lijst met de installatie-eigenschappen, [maken of overschrijven van een installatie met REST-API](https://msdn.microsoft.com/library/azure/mt621153.aspx) of [installatie-eigenschappen](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx).

```javascript
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
```

> [!NOTE]
> Standaard verlopen registratie en-installaties en niet.

Registraties en installaties moeten een geldige PNS-ingang voor elk apparaat/kanaal bevatten. Omdat PNS-ingangen kunnen alleen worden verkregen in een client-app op het apparaat, wordt een patroon is om rechtstreeks op dat apparaat met de client-app te registreren. Aan de andere kant beveiligingsoverwegingen en zakelijke logica die betrekking hebben op tags mogelijk moet u voor het beheren van device Registration service in de app-back-end.

### <a name="templates"></a>Sjablonen

Als u wilt gebruiken [sjablonen](notification-hubs-templates-cross-platform-push-messages.md), installatie van het apparaat bevat ook alle sjablonen die zijn gekoppeld aan het apparaat in een JSON-indeling (Zie het bovenstaande voorbeeld). De sjabloonnamen helpen bij het doel verschillende sjablonen voor hetzelfde apparaat.

De sjabloonnaam van elke wordt toegewezen aan de hoofdtekst van een sjabloon en een optionele set van labels. Elk platform kan bovendien extra eigenschappen hebben. Voor Windows Store (met WNS) en Windows Phone 8 (met behulp van MPNS), kan een extra set headers deel uitmaken van de sjabloon. In het geval van APNs, kunt u de eigenschap van een verlopen instelt, moet een constante of een sjabloonexpressie. Voor een volledige lijst met de installatie-eigenschappen zien, [maken of overschrijven van een installatie met REST](https://msdn.microsoft.com/library/azure/mt621153.aspx) onderwerp.

### <a name="secondary-tiles-for-windows-store-apps"></a>Secundaire tegels voor Windows Store-Apps

Voor Windows Store-clienttoepassingen, verzenden van meldingen naar secundaire tegels is hetzelfde als ze worden verzonden naar de primaire database. Dit wordt ook ondersteund in installaties. Secundaire tegels hebben een ander kanaal-URI, die de SDK in uw client-app transparant verwerkt.

De woordenlijst SecondaryTiles maakt gebruik van de dezelfde TileId die wordt gebruikt voor het maken van het object SecondaryTiles in uw Windows Store-app.
Net als bij de primaire kanaal-URI, kunt ChannelUris van secundaire tegels op elk moment wijzigen. Zorgen dat de installatie van het in de notification hub bijgewerkt, moet het apparaat te vernieuwen met de huidige ChannelUris van de secundaire tegels.

## <a name="registration-management-from-the-device"></a>Registratiebeheer van de van het apparaat

Bij het beheren van device Registration service van de client-apps, is de back-end alleen verantwoordelijk voor het verzenden van meldingen. Client-apps up-to-date houden van PNS-ingangen en registreren van tags. De volgende afbeelding ziet u dit patroon.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Het apparaat eerst de PNS-ingang opgehaald uit de PNS en vervolgens rechtstreeks door de notification hub geregistreerd. Wanneer de registratie geslaagd is, kan de back-end een melding die gericht is op deze inschrijving kunt verzenden. Zie voor meer informatie over hoe u meldingen verzendt, [Routering en code-expressies](notification-hubs-tags-segment-push-message.md).

In dit geval u enige Listen-rechten voor toegang tot uw meldingshubs van het apparaat. Zie voor meer informatie, [Security](notification-hubs-push-notification-security.md).

Registreren van het apparaat is de eenvoudigste methode, maar er enkele nadelen:

- Een client-app kunt de labels alleen bijwerken wanneer de app actief is. Bijvoorbeeld, als een gebruiker heeft twee apparaten die geregistreerd tags met betrekking tot sport teams, wanneer het eerste apparaat wordt geregistreerd voor een aanvullende tag (bijvoorbeeld Seahawks), wordt het tweede apparaat het geen meldingen ontvangen over de Seahawks totdat de app op het tweede apparaat een tweede keer uitgevoerd. Meer over het algemeen is tags beheren vanaf de back-end wanneer tags worden beïnvloed door meerdere apparaten, een optie voor het wenselijk.
- Aangezien apps kunnen worden gehackt, vereist beveiliging van de registratie op specifieke labels extra aandacht, zoals wordt beschreven in de sectie "Tag beveiligingsniveau."

### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Voorbeeldcode om te registreren bij een meldingshub die vanaf een apparaat met een installatie

Op dit moment, dit wordt alleen ondersteund met behulp van de [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx).

U kunt ook gebruiken voor het gebruik van de PATCH methode de [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) voor het bijwerken van de installatie.

```
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

// If we have not stored an installation id in application data, create and store as application data.
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
```

### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Voorbeeldcode om te registreren bij een meldingshub die vanaf een apparaat met een registratie

Deze methoden maken of bijwerken van een registratie voor het apparaat waarop ze worden genoemd. Dit betekent dat de registratie van de gehele om bij te werken van de greep of de tags, moet overschrijven. Houd er rekening mee dat registraties tijdelijke, zijn zodat u moet altijd een betrouwbare opslag met de huidige codes die een specifiek apparaat nodig heeft.

```
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
```

## <a name="registration-management-from-a-backend"></a>Registratiebeheer van de van een back-end

Registraties beheren vanaf de back-end vereist het schrijven van aanvullende code. De app van het apparaat moet opgeven voor de bijgewerkte PNS-ingang naar de back-end telkens wanneer de app wordt gestart (samen met tags en sjablonen) en de back-end deze ingang op de notification hub moet bijwerken. De volgende afbeelding ziet u dit ontwerp.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

De voordelen van registraties beheren vanaf de back-end omvatten de mogelijkheid om te wijzigen van tags op registraties, zelfs als de bijbehorende app op het apparaat is niet actief, en om te verifiëren van de client-app voordat u een label toevoegt aan de registratie ervan.

### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Voorbeeldcode om te registreren bij een meldingshub die vanuit een back-end met behulp van een installatie

Het client-apparaat wordt nog steeds haalt de PNS-ingang en de relevante installatie-eigenschappen als voordat en een aangepaste API wordt aangeroepen op de back-end die kan uitvoeren van de registratie en autoriseren van tags enzovoort. De back-end kan gebruikmaken van de [Notification Hub SDK voor back-end-bewerkingen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

U kunt ook gebruiken voor het gebruik van de PATCH methode de [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) voor het bijwerken van de installatie.

```
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
        case "fcm":
            installation.Platform = NotificationPlatform.Fcm;
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
```

### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Voorbeeldcode om te registreren bij een meldingshub die vanaf een apparaat met een registratie-ID

U kunt eenvoudige CRUDS-bewerkingen op registraties uitvoeren vanuit uw back-end. Bijvoorbeeld:

```
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
```

De back-end moet gelijktijdigheid tussen registratie-updates worden verwerkt. Service Bus biedt functionaliteit voor optimistische gelijktijdigheid-besturingselement voor het registratiebeheer van de. Dit is geïmplementeerd op het niveau van de HTTP-, met het gebruik van de ETag op bewerkingen. Deze functie is transparant gebruikt door Microsoft-SDKs, die een uitzondering te genereren als een update is geweigerd voor gelijktijdigheid redenen. De back-end is verantwoordelijk voor het verwerken van deze uitzonderingen en u de update opnieuw uitvoert, indien nodig.
