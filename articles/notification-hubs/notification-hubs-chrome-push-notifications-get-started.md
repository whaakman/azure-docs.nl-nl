---
title: Pushmeldingen verzenden naar Chrome-apps met Azure Notification Hubs | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Chrome-app.
services: notification-hubs
keywords: mobiele pushmeldingen,pushmeldingen,pushmelding,pushmeldingen chroom
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: ffac79209f998f1ef042ed5fcb4d8c6a45de2c2e
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447034"
---
# <a name="tutorial-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Zelfstudie: Pushmeldingen verzenden naar Chrome-apps met Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Deze zelfstudie laat zien u hoe u een Notification Hub maakt en pushmeldingen naar een Google Chrome-voorbeeldapp verzendt met behulp van [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/). De Chrome-app wordt uitgevoerd in de context van een Google Chrome-browser en wordt geregistreerd bij de Notification Hub.

> [!NOTE]
> Pushmeldingen in een Chrome-app zijn geen algemene meldingen in de browser. Ze zijn specifiek voor het uitbreidbaarheidsmodel van de browser (Zie [Overzicht van Chrome-apps] voor meer informatie). Chrome-apps kunnen niet alleen op de pc worden gebruikt, maar ook op mobiele apparaten (Android en iOS) via Apache Cordova. Zie [Chrome-apps op mobiele apparaten] voor meer informatie.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * [Google Cloud Messaging inschakelen](#register)
> * [Uw Notification Hub configureren](#configure-hub)
> * [Uw Chrome-app verbinden met de Notification Hub](#connect-app)
> * [Een pushmelding naar uw Chrome-app verzenden](#send)

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a id="register"></a>Google Cloud Messaging inschakelen

1. Navigeer naar de website [Google Cloud Console](https://console.cloud.google.com/cloud-resource-manager) en meld u aan met de referenties voor uw Google-account.
2. Selecteer **Project maken** op de werkbalk.

    ![Knop Project maken](media/notification-hubs-chrome-get-started/create-project-button.png)
3. Geef een geschikte **Project Name** op en klik vervolgens op de knop **Create**.
4. Selecteer het meldingspictogram (bel) op de werkbalk en selecteer het bericht **Project maken**.

    ![Melding Project maken](media/notification-hubs-chrome-get-started/project-creation-notification.png)
5. Noteer het projectnummer in het veld **Project Number** op de pagina **Projects** van het project dat u hebt gemaakt. U gebruikt het projectnummer als de **GCM-afzender-id** in de Chrome-app om uzelf te registreren bij GCM.

    ![Google Cloud Console - Projectnummer](media/notification-hubs-chrome-get-started/gcm-project-number.png)
6. Selecteer in het dashboard **Naar API-overzicht**.

    ![Knop Naar API-overzicht](media/notification-hubs-chrome-get-started/go-to-apis-overview-button.png)
7. Selecteer op de pagina API en services **API's en services inschakelen**.

    ![API's en services inschakelen](media/notification-hubs-chrome-get-started/enable-apis-and-services.png)
8. Zoek in de lijst met behulp van het trefwoord **cloudberichten**. Selecteer **Google Cloud Messaging** in de gefilterde lijst.

    ![Google Cloud Messaging API](media/notification-hubs-chrome-get-started/google-cloud-messaging-api.png)
9. Op de pagina **Google Cloud Messaging** selecteert u **Inschakelen**.

    ![GCM inschakelen](media/notification-hubs-chrome-get-started/enable-gcm.png)
10. Ga op de pagina **API en services** naar het tabblad **Referenties**. Selecteer **Referenties maken** en selecteer vervolgens **API-sleutel**.

    ![Knop API-sleutel maken](media/notification-hubs-chrome-get-started/create-api-key-button.png)
11. Selecteer in het dialoogvenster **API-sleutel gemaakt** de knop Kopiëren om de sleutel naar het Klembord te kopiëren. Sla de sleutel op in de gewenste locatie. U gebruikt deze waarde in de volgende sectie om de Notification Hub te configureren voor het verzenden van pushmeldingen naar GCM.

    ![API-pagina](media/notification-hubs-chrome-get-started/api-created-page.png)
12. Selecteer uw API-sleutel in de lijst **API-sleutels**. Selecteer op de pagina API-sleutel **IP-adressen (webservers, cron-taken, enzovoort)** , voer **0.0.0.0/0** in als IP-adres en klik op Opslaan.

    ![IP-adressen inschakelen](media/notification-hubs-chrome-get-started/enable-ip-addresses.png)

## <a id="configure-hub"></a>Een Notification Hub maken

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. Selecteer **Google (GCM)** in de categorie **MELDINGSINSTELLINGEN**, voer de **API-sleutel** voor het GCM-project in en klik op **Opslaan**.

&emsp;&emsp;&emsp;&emsp;![Azure Notification Hubs - Google (GCM)](media/notification-hubs-chrome-get-started/configure-gcm-api-key.png)

## <a id="connect-app"></a>Uw Chrome-app verbinden met de Notification Hub

De Notification Hub is nu geconfigureerd voor GCM en u hebt de verbindingsreeksen om uw app te registreren voor het ontvangen en verzenden van pushmeldingen.

### <a name="create-a-new-chrome-app"></a>Een nieuwe Chrome-app maken

Het onderstaande voorbeeld is gebaseerd op het [Chrome App GCM-voorbeeld] waarin op de aanbevolen manier een Chrome-app wordt gemaakt. In deze sectie worden de stappen beschreven die specifiek van toepassing zijn op Azure Notification Hubs.

> [!NOTE]
> We raden u aan de bron voor deze Chrome-app te downloaden vanaf [Chrome App Notification Hub Sample].

De Chrome-app wordt gemaakt via JavaScript en u kunt uw favoriete tekstverwerkingsprogramma hiervoor gebruiken. De Chrome-app ziet er als volgt uit:

![Google Chrome-app][15]

1. Maak een map en geef deze de naam `ChromePushApp`. De naam is willekeurig. Als u een andere naam gebruikt, moet u het pad in de vereiste codesegmenten vervangen.
2. Download [crypto-js library] in de map die u in de tweede stap hebt gemaakt. Deze bibliotheekmap bevat twee submappen: `components` en `rollups`.
3. Maak een `manifest.json`-bestand. Alle Chrome-apps worden ondersteund door een manifestbestand met de metagegevens van de app en alle machtigingen die worden verleend aan de app wanneer de gebruiker de app installeert.

    ```json
    {
        "name": "NH-GCM Notifications",
        "description": "Chrome platform app.",
        "manifest_version": 2,
        "version": "0.1",
        "app": {
        "background": {
            "scripts": ["background.js"]
        }
        },
        "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
        "icons": { "128": "gcm_128.png" }
    }
    ```

    Let op het `permissions`-element, dat aangeeft dat deze Chrome-app pushmeldingen van GCM mag ontvangen. In onze voorbeeld-app wordt ook pictogrambestand `gcm_128.png` gebruikt, dat zich bevindt op de bronlocatie die opnieuw wordt gebruikt op basis van het oorspronkelijke GCM-voorbeeld. U kunt het vervangen door een andere afbeelding die voldoet aan de [vereisten voor een pictogram](https://developer.chrome.com/apps/manifest/icons).
4. Maak een bestand met de naam `background.js` met de volgende code:

    ```javascript
    // Returns a new notification ID used in the notification.
    function getNotificationId() {
        var id = Math.floor(Math.random() * 9007199254740992) + 1;
        return id.toString();
    }

    function messageReceived(message) {
        // A message is an object with a data property that
        // consists of key-value pairs.

        // Concatenate all key-value pairs to form a display string.
        var messageString = "";
        for (var key in message.data) {
        if (messageString != "")
            messageString += ", "
        messageString += key + ":" + message.data[key];
        }
        console.log("Message received: " + messageString);

        // Pop up a notification to show the GCM message.
        chrome.notifications.create(getNotificationId(), {
        title: 'GCM Message',
        iconUrl: 'gcm_128.png',
        type: 'basic',
        message: messageString
        }, function() {});
    }

    var registerWindowCreated = false;

    function firstTimeRegistration() {
        chrome.storage.local.get("registered", function(result) {

        registerWindowCreated = true;
        chrome.app.window.create(
            "register.html",
            {  width: 520,
                height: 500,
                frame: 'chrome'
            },
            function(appWin) {}
        );
        });
    }

    // Set up a listener for GCM message event.
    chrome.gcm.onMessage.addListener(messageReceived);

    // Set up listeners to trigger the first-time registration.
    chrome.runtime.onInstalled.addListener(firstTimeRegistration);
    chrome.runtime.onStartup.addListener(firstTimeRegistration);
    ```

    Met dit bestand wordt de HTML van het Chrome-app-venster (`register.html`) weergegeven en ook de handler `messageReceived` gedefinieerd om de binnenkomende pushberichten te verwerken.
5. Maak een bestand met de naam `register.html`. Hiermee wordt de gebruikersinterface van de Chrome-app gedefinieerd.

   > [!NOTE]
   > In dit voorbeeld werken we met **CryptoJS v3.1.2**. Als u een andere versie van de bibliotheek hebt gedownload, moet u de versie in het `src`-pad vervangen door de juiste versie.

    ```html
    <html>
        <head>
            <title>GCM Registration</title>
            <script src="register.js"></script>
            <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
            <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>
        <body>
            Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
            <button id="registerWithGCM">Register with GCM</button>
            <br/>
            <br/>
            <br/>

            Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
            Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

            <br/>
            <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>
            <br/>
            <br/>

            <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>
        </body>
    </html>
    ```
6. Maak een bestand met de naam `register.js` met de code in de volgende stap. Dit bestand definieert het script achter `register.html`. Chrome-apps ondersteunen geen inline-uitvoeringen. U moet dus een afzonderlijke back-up van het script maken voor uw gebruikersinterface.

    ```javascript
    var registrationId = "";
    var hubName        = "", connectionString = "";
    var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

    window.onload = function() {
        document.getElementById("registerWithGCM").onclick = registerWithGCM;  
        document.getElementById("registerWithNH").onclick = registerWithNH;
        updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
    }

    function updateLog(status) {
        currentStatus = document.getElementById("console").innerHTML;
        if (currentStatus != "") {
        currentStatus = currentStatus + "\n\n";
        }

        document.getElementById("console").innerHTML = currentStatus  + status;
    }

    function registerWithGCM() {
        var senderId = document.getElementById("senderId").value.trim();
        chrome.gcm.register([senderId], registerCallback);

        // Prevent register button from being clicked again before the registration finishes.
        document.getElementById("registerWithGCM").disabled = true;
    }

    function registerCallback(regId) {
        registrationId = regId;
        document.getElementById("registerWithGCM").disabled = false;

        if (chrome.runtime.lastError) {
        // When the registration fails, handle the error and retry the
        // registration later.
        updateLog("Registration failed: " + chrome.runtime.lastError.message);
        return;
        }

        updateLog("Registration with GCM succeeded.");
        document.getElementById("registerWithNH").disabled = false;

        // Mark that the first-time registration is done.
        chrome.storage.local.set({registered: true});
    }

    function registerWithNH() {
        hubName = document.getElementById("hubName").value.trim();
        connectionString = document.getElementById("connectionString").value.trim();
        splitConnectionString();
        generateSaSToken();
        sendNHRegistrationRequest();
    }

    // From http://msdn.microsoft.com/library/dn495627.aspx
    function splitConnectionString()
    {
        var parts = connectionString.split(';');
        if (parts.length != 3)
        throw "Error parsing connection string";

        parts.forEach(function(part) {
        if (part.indexOf('Endpoint') == 0) {
        endpoint = 'https' + part.substring(11);
        } else if (part.indexOf('SharedAccessKeyName') == 0) {
        sasKeyName = part.substring(20);
        } else if (part.indexOf('SharedAccessKey') == 0) {
        sasKeyValue = part.substring(16);
        }
        });

        originalUri = endpoint + hubName;
    }

    function generateSaSToken()
    {
        targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
        var expiresInMins = 10; // 10 minute expiration

        // Set expiration in seconds.
        var expireOnDate = new Date();
        expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
        var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
        .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
        .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
        .getUTCSeconds()) / 1000;
        var tosign = targetUri + '\n' + expires;

        // Using CryptoJS.
        var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
        var base64signature = signature.toString(CryptoJS.enc.Base64);
        var base64UriEncoded = encodeURIComponent(base64signature);

        // Construct authorization string.
        sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
    }

    function sendNHRegistrationRequest()
    {
        var registrationPayload =
        "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
        "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
            "<content type=\"application/xml\">" +
                "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                    "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                "</GcmRegistrationDescription>" +
            "</content>" +
        "</entry>";

        // Update the payload with the registration ID obtained earlier.
        registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

        var url = originalUri + "/registrations/?api-version=2014-09";
        var client = new XMLHttpRequest();

        client.onload = function () {
        if (client.readyState == 4) {
            if (client.status == 200) {
            updateLog("Notification Hub Registration successful!");
            updateLog(client.responseText);
            } else {
            updateLog("Notification Hub Registration did not succeed!");
            updateLog("HTTP Status: " + client.status + " : " + client.statusText);
            updateLog("HTTP Response: " + "\n" + client.responseText);
            }
        }
        };

        client.onerror = function () {
            updateLog("ERROR - Notification Hub Registration did not succeed!");
        }

        client.open("POST", url, true);
        client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
        client.setRequestHeader("Authorization", sasToken);
        client.setRequestHeader("x-ms-version", "2014-09");

        try {
            client.send(registrationPayload);
        }
        catch(err) {
            updateLog(err.message);
        }
    }
    ```

    Het script heeft de volgende belangrijke parameters:

    * `window.onload` definieert de knop-klikgebeurtenissen van de twee knoppen in de gebruikersinterface. Met de eerste gebeurtenis-handler voor de knop wordt de registratie bij GCM uitgevoerd. Voor de tweede handler wordt gebruikgemaakt van de registratie-id die na de registratie bij GCM van Azure Notification Hubs wordt geretourneerd.
    * `updateLog` is de functie waarmee wordt toegestaan dat met de code gegevens in het logboek worden geregistreerd.
    * `registerWithGCM` is de eerste knop-klikhandler. Hiermee wordt `chrome.gcm.register` GCM aangeroepen om het huidige exemplaar van de Chrome-app te registreren.
    * `registerCallback` is de callbackfunctie die wordt aangeroepen wanneer de aanroep van de GCM-registratie wordt geretourneerd.
    * `registerWithNH` is de tweede knop-klikhandler. Hiermee wordt geregistreerd bij Notification Hubs. Deze ontvangt `hubName` en `connectionString` (door de gebruiker opgegeven) en stelt de REST-API-aanroep samen voor registratie bij Notification Hubs.
    * `splitConnectionString` en `generateSaSToken` zijn helpers voor de JavaScript-implementatie van het proces voor het maken van een SaS-token, dat in alle REST-API-aanroepen moet worden gebruikt. Zie [Algemene begrippen](https://msdn.microsoft.com/library/dn495627.aspx) voor meer informatie.
    * `sendNHRegistrationRequest` is de functie waarmee een HTTP REST-aanroep naar Azure Notification Hubs wordt gemaakt.
    * `registrationPayload` definieert de XML-nettolading van de registratie. Zie [REST-API maken voor de registratie van NH] voor meer informatie. Werk de registratie-id hierin bij met de informatie die is ontvangen van GCM.
    * `client` is een exemplaar van `XMLHttpRequest` dat door de toepassing wordt gebruikt om de HTTP POST-aanvraag te doen. Werk de `Authorization`-header bij met `sasToken`. Als deze aanroep is voltooid, wordt dit exemplaar van de Chrome-app geregistreerd bij Azure Notification Hubs.

    De algehele mappenstructuur voor dit project moet er als volgt uitzien:  ![Google Chrome-app - mappenstructuur][21]

### <a name="set-up-and-test-your-chrome-app"></a>Uw Chrome-app instellen en testen

1. Open uw Chrome-browser. Open **Chrome-extensies** en schakel **Modus voor ontwikkelaars** in.

    ![Google Chrome - Modus voor ontwikkelaars inschakelen][16]
2. Klik op **Uitgepakte extensie laden** en navigeer naar de map waarin u de bestanden hebt gemaakt. U kunt eventueel ook het **hulpprogramma Chrome Apps& Extensions Developer** gebruiken. Dit hulpprogramma is zelf een Chrome-app (geïnstalleerd via de Chrome Web Store) en biedt geavanceerde mogelijkheden voor foutopsporing tijdens de ontwikkeling van uw Chrome-app.

    ![Google Chrome - Uitgepakte extensie laden][17]
3. Als de Chrome-app foutloos is gemaakt, wordt uw app weergegeven.

    ![Google Chrome - Chrome-app weergeven][18]
4. Voer het **projectnummer** in dat u eerder hebt ontvangen van de **Google Cloud Console** als de afzender-id en klik op **Registreren bij GCM**. Het volgende bericht wordt weergegeven: **Registratie bij GCM is voltooid.**

    ![Google Chrome - Chrome-app aanpassen][19]
5. Voer de **Naam van de Notification Hub** en de **DefaultListenSharedAccessSignature** in die u eerder hebt ontvangen van de portal en klik op **Registreren bij Azure Notification Hub**. Als het goed is, wordt het volgende bericht weergegeven: **Registratie van Notification Hub gelukt!**, evenals de details van het registratie-antwoord dat de registratie-id van Azure Notification Hubs bevat.

    ![Google Chrome - Details voor de Notification Hub opgeven][20]  

## <a name="send"></a>Een melding naar uw Chrome-app verzenden

Voor testdoeleinden verzenden we Chrome-pushmeldingen met een .NET-consoletoepassing.

> [!NOTE]
> U kunt pushmeldingen via Notification Hubs verzenden vanuit elke back-end via de openbare [REST-interface](https://msdn.microsoft.com/library/windowsazure/dn223264.aspx). Bekijk de [portal met documentatie](https://azure.microsoft.com/documentation/services/notification-hubs/) voor meer platformonafhankelijke voorbeelden.

1. Klik in Visual Studio in het menu **File** (Bestand) op **New** (Nieuw) en vervolgens **Project**. Klik onder **Visual C#** op **Windows** en **Console Application** en klik vervolgens op **OK**.  Met deze stap maakt u een nieuw consoletoepassingsproject.
2. Klik in het menu **Tools** op **NuGet Package Manager** en **Package Manager Console**. Package Manager Console wordt in het onderste venster weergegeven.
3. Voer de volgende opdracht uit in het consolevenster:

    ```shell
    Install-Package Microsoft.Azure.NotificationHubs
    ```

   Hiermee wordt automatisch een verwijzing naar de Azure Service Bus-SDK met het [WindowsAzure.ServiceBus NuGet-pakket](http://nuget.org/packages/WindowsAzure.ServiceBus/) toegevoegd.
4. Open `Program.cs` en voeg de volgende `using` instructie toe:

    ```csharp
    using Microsoft.Azure.NotificationHubs;
    ```
5. Voeg in de klasse `Program` de volgende methode toe:

    ```csharp
    private static async void SendNotificationAsync()
    {
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
        String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
        await hub.SendGcmNativeNotificationAsync(message);
    }
    ```

    Vervang de tijdelijke aanduiding `<hub name>` door de naam van de Notification Hub die wordt weergegeven in de [portal](https://portal.azure.com) op de pagina Notification Hub. Vervang ook de tijdelijke aanduiding voor de verbindingsreeks door de verbindingsreeks genaamd `DefaultFullSharedAccessSignature` die u hebt verkregen in de sectie voor het configureren van uw Notification Hub.

    > [!NOTE]
    > Zorg ervoor dat u de verbindingsreeks met het toegangsrecht **Full** (Volledig) gebruikt, dus niet **Listen** (Luisteren). Met de verbindingsreeks met het toegangsrecht **Listen** kunnen geen pushmeldingen worden verzonden.

6. Voeg de volgende aanroepen toe aan de methode `Main`:

    ```csharp
    SendNotificationAsync();
    Console.ReadLine();
    ```

7. Zorg ervoor dat Chrome wordt uitgevoerd en start de consoletoepassing.
8. De volgende meldingspop-up wordt op uw bureaublad weergegeven.

    ![Google Chrome - Melding][13]
9. U kunt ook al uw meldingen zien in het venster Chrome-meldingen in de taakbalk (in Windows) als Chrome wordt uitgevoerd.

    ![Google Chrome - Lijst met meldingen](./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png)

> [!NOTE]
> Hiervoor hoeft u de Chrome-app niet te starten of te openen in de browser (de Chrome-browser zelf moet wel worden uitgevoerd). U kunt ook een geconsolideerde weergave van al uw meldingen bekijken in het venster Chrome-berichten.

## <a name="next-steps"> </a>Volgende stappen

In deze zelfstudie hebt u meldingen verzonden naar alle clients die zijn geregistreerd bij de back-end. Ga verder met de volgende zelfstudie als u wilt weten hoe u pushmeldingen kunt verzenden naar specifieke apparaten:

> [!div class="nextstepaction"]
>[Pushmeldingen verzenden naar specifieke apparaten](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- Images. -->
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome App Notification Hub Sample]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Notification Hubs Overview]: notification-hubs-push-notification-overview.md
[Overzicht van Chrome-apps]: https://developer.chrome.com/apps/about_apps
[Chrome App GCM-voorbeeld]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome-apps op mobiele apparaten]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[REST-API maken voor de registratie van NH]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[crypto-js library]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging for Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs Notify Users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Notification Hubs breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
