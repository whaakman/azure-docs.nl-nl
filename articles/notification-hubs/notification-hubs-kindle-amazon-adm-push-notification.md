---
title: Pushmeldingen verzenden naar Kindle-apps met Azure Notification Hubs | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs pushmeldingen verstuurt naar een Kindle-toepassing.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 346fc8e5-294b-4e4f-9f27-7a82d9626e93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-kindle
ms.devlang: Java
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/29/2019
ms.author: jowargo
ms.openlocfilehash: edd0e12460e07cfd2990cc43a9056ed06b84fb1d
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927015"
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Aan de slag met Azure Notification Hubs voor Kindle-apps

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In deze zelfstudie ziet u hoe u met Azure Notification Hubs pushmeldingen verstuurt naar een Kindle-toepassing. U maakt een lege Kindle-app die pushmeldingen ontvangt via Amazon Device Messaging (ADM).

In deze zelfstudie gaat u code maken of bijwerken om de volgende taken uit te voeren:

> [!div class="checklist"]
> * Een nieuwe app toevoegen aan de portal voor ontwikkelaars
> * Maak een API-sleutel.
> * Maken en een notification hub configureren
> * Uw toepassing instellen
> * De ADM-berichtenhandler maken
> * Uw API-sleutel aan uw app toevoegen
> * De app uitvoeren
> * Een testmelding verzenden

## <a name="prerequisites"></a>Vereisten

- [Android Studio](https://developer.android.com/studio/?gclid=CjwKCAjwwZrmBRA7EiwA4iMzBPZ9YYmR0pbb5LtjnWhWCxe8PWrmjmeaR6ad5ksCr_j2mmkVj_-o6hoCAqwQAvD_BwE).
- Volg de stappen in [Uw ontwikkelingsomgeving instellen](https://developer.amazon.com/docs/fire-tablets/ft-set-up-your-development-environment.html) om uw ontwikkelingsomgeving voor Kindle in te stellen.

## <a name="add-a-new-app-to-the-developer-portal"></a>Een nieuwe app toevoegen aan de portal voor ontwikkelaars

1. Meld u aan bij de [Amazon-Portal voor ontwikkelaars](https://developer.amazon.com/apps-and-games/console/apps/list.html).
2. Selecteer **nieuwe App toevoegen**, en selecteer vervolgens **Android**.  

    ![Knop Nieuwe app toevoegen](./media/notification-hubs-kindle-get-started/add-new-app-button.png)
1. Op de **nieuwe App indienen** pagina, volgt u deze stappen uit om de **Toepassingssleutel**:
    1. Voer een naam voor de **App titel**.
    2. Selecteer een **categorie** (bijvoorbeeld: education)
    4. Voer een e-mailadres voor de **klant ondersteuning voor e-mailadres** veld. 
    5. Selecteer **Opslaan**.

        ![Nieuwe App-verzendpagina](./media/notification-hubs-kindle-get-started/new-app-submission-page.png) 
2.  Aan de bovenkant en schakel over naar de **Mobile advertenties** tabblad en voer de volgende stappen uit: 
    1. Geef op of uw app voornamelijk op kinderen onder de 13 jaar wordt omgeleid. Selecteer voor deze zelfstudie **Nee**.
    2. Selecteer **Indienen**. 

        ![De pagina mobiele advertenties](./media/notification-hubs-kindle-get-started/mobile-ads-page.png)
    3. Kopiëren de **Toepassingssleutel** uit de **Mobile advertenties** pagina. 

        ![Toepassingssleutel](./media/notification-hubs-kindle-get-started/application-key.png)
3.  Selecteer **Apps en Services** menu aan de bovenkant en selecteer uw toepassing in de lijst. 

    ![Selecteer uw app in lijst](./media/notification-hubs-kindle-get-started/all-apps-select.png)
4. Schakel over naar de **Device Messaging** tabblad, en als volgt te werk: 
    1. Selecteer **maken van een nieuw beveiligingsprofiel**.
    2. Voer een **naam** voor uw beveiligingsprofiel. 
    3. Voer **beschrijving** voor uw beveiligingsprofiel. 
    4. Selecteer **Opslaan**. 
    5. Selecteer **weergave beveiligingsprofiel** op de pagina. 
5. Nu op de **beveiligingsprofiel** pagina, de volgende stappen uit: 
    1. Schakel over naar de **Webinstellingen** tabblad en kopieer de **Client-ID** en **Clientgeheim** waarde voor later gebruik. 

        ![Client-ID en -geheim ophalen](./media/notification-hubs-kindle-get-started/client-id-secret.png) 
    2. Schakel over naar de **instellingen voor Android/Kindle** pagina, en houd de pagina geopend. U hebt deze waarden invoeren in de volgende sectie. 

## <a name="create-an-api-key"></a>Maak een API-sleutel.
1. Open een opdrachtprompt met beheerdersbevoegdheden.
2. Navigeer naar de map Android SDK.
3. Voer de volgende opdracht in:

    ```shell
    keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
    ```
4. Voor het **KeyStore**-wachtwoord typt u **android**.
5. Kopieer de **MD5** en **SHA256** vingerafdrukken. 
6. Back-ups maken in de portal voor ontwikkelaars op de **instellingen voor Android/Kindle** tabblad, de volgende stappen uit: 
    1. Voer een **naam voor de API-sleutel**. 
    2. Voer de **naam van het pakket** voor uw app (bijvoorbeeld **com.fabrikam.mykindleapp**) en de **MD5** waarde.
        
        >[!IMPORTANT]
        > Wanneer u een app in Android Studio maakt, gebruikt u hier de dezelfde pakketnaam die u hebt opgegeven. 
    1. Plak de **MD5 handtekening** u eerder hebt gekopieerd. 
    2. Plak de **SHA256 handtekening** u eerder hebt gekopieerd.  
    3. Selecteer **nieuwe sleutel genereren**.

        ![Instellingen voor android/Kindle - sleutel genereren](./media/notification-hubs-kindle-get-started/android-kindle-settings.png)
    4. Selecteer nu **weergeven** in de lijst om te zien van de API-sleutel. 

        ![Instellingen voor android/Kindle - API-sleutel weergeven](./media/notification-hubs-kindle-get-started/show-api-key-button.png) 
    5. In de **API-sleutel Details** venster, kopieert u de API-sleutel en sla deze ergens. Selecteer **X** in de rechterbovenhoek om het venster te sluiten. 


## <a name="create-and-configure-a-notification-hub"></a>Maken en een notification hub configureren

1. Volg de stappen in de [een Azure notification hub maken in Azure portal](create-notification-hub-portal.md) artikel voor een notification hub maken. 
2. Selecteer **Amazon (ADM)** onder **instellingen** menu.
3. Plak de **Client-ID** en **Clientgeheim** u eerder hebt opgeslagen. 
4. Selecteer **Opslaan** op de werkbalk. 

    ![ADM-instellingen voor een notification hub configureren](./media/notification-hubs-kindle-get-started/configure-notification-hub.png)
5. Selecteer **toegangsbeleid** op het menu aan de linkerkant en selecteer de **kopie** knop voor de verbindingsreeks voor de **DefaultListenSharedAccessSignature** beleid. Sla de sleutel op in de gewenste locatie. U gebruikt deze verderop in de broncode. 

    ![Notification hub - verbindingsreeks voor luisteren](./media/notification-hubs-kindle-get-started/event-hub-listen-connection-string.png)    

## <a name="set-up-your-application"></a>Uw toepassing instellen

1. Start Android Studio. 
2. Selecteer **bestand**, wijst u **nieuw**, en selecteer vervolgens **nieuw Project**. 
3. In de **Kies uw project** venster op de **telefoons en tablets** tabblad **lege activiteit**, en selecteer **volgende**. 
4. In de **configureren van uw project** venster de volgende stappen uit:
    1. Voer een **naam voor uw toepassing**. U kunt deze Hiermee overeenkomt met de naam van de toepassing die u hebt gemaakt in de Amazon-Portal voor ontwikkelaars. 
    2. Voer een **naam voor het pakket**. 
        
        >[!IMPORTANT]
        >Naam van het pakket moet overeenkomen met de naam van het pakket dat u hebt opgegeven in de Amazon-Portal voor ontwikkelaars.
    3. Controleren en bijwerken van de resterende waarden waar nodig. 
    4. Selecteer **Finish**. 

        ![Configureren van Android-project](./media/notification-hubs-kindle-get-started/new-android-studio-project.png)
5. Download [Amazon ontwikkelaar-SDK voor Android](https://developer.amazon.com/sdk-download) bibliotheek naar de vaste schijf. Pak het SDK-zipbestand uit.
6. In Android Studio, wijzigt u de mapstructuur van **Android** naar **Project** als deze nog niet is ingesteld op **Project**. 

    ![Android Studio - overschakelen naar projectstructuur](./media/notification-hubs-kindle-get-started/android-studio-project-view.png)
7. Vouw **app** om te zien de **bibliotheken** map in de structuurweergave wordt weergegeven.     
8. Navigeer naar de map waar u de Android-SDK's van Amazon hebt gedownload in een File Explorer-venster.
9. Druk op **CTRL** en slepen en neerzetten het **amazon-apparaat-berichten-1.0.1.jar** van het bestand in de **lib** knooppunt in de structuurweergave wordt weergegeven. 

    ![Android Studio - Amazon Device Messaging JAR toevoegen](./media/notification-hubs-kindle-get-started/drag-drop-amazon-device-messaging-jar.png)
9. In de **kopie** venster **OK**. Als u ziet de **verplaatsen** venster in plaats van **kopie** venster sluiten en probeer het opnieuw met slepen en neerzetten **CTRL** knop die wordt ingedrukt. 

    ![Android Studio - JAR kopiëren](./media/notification-hubs-kindle-get-started/copy-jar-window.png)
10. De volgende instructie voor toevoegen de **van app build.gradle** -bestand in de **afhankelijkheden** sectie: `implementation files('libs/amazon-device-messaging-1.0.1.jar')`. 

    ![Android Studio - ADM toevoegen aan de app build.gradle](./media/notification-hubs-kindle-get-started/adm-build-gradle.png)
11. In de `Build.Gradle` bestand voor de **app**, voeg de volgende regels in de **afhankelijkheden** sectie: 

    ```gradle
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.6@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```
12. Voeg de volgende opslagplaats toe **nadat** de **afhankelijkheden** sectie:

    ```gradle
    repositories {
        maven {
            url "https://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```
13. In de editor voor de **build.gradle** bestand voor de **app**, selecteer **nu synchroniseren** op de werkbalk. 

    ![Android Studio - build.gradle synchronisatie van de app.](./media/notification-hubs-kindle-get-started/gradle-sync-now.png)
14. Ga terug naar de Android-structuur in de structuurweergave wordt weergegeven.  De Amazon-naamruimte in het basismanifestelement toevoegen:

    ```xml
    xmlns:amazon="http://schemas.amazon.com/apk/res/android"
    ```
   
    ![Amazon-naamruimte in het manifest](./media/notification-hubs-kindle-get-started/amazon-namespace-manifest.png)
2. Voeg machtigingen toe als het eerste element onder het manifestelement. Vervang **[naam van uw pakket]** met het pakket dat u gebruikt voor het maken van uw app.

    ```xml
    <permission
        android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
        android:protectionLevel="signature" />

    <uses-permission android:name="android.permission.INTERNET"/>

    <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

    <!-- This permission allows your app access to receive push notifications
    from ADM. -->
    <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

    <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    ```
3. Voeg het volgende element in als het eerste onderliggende item van het toepassingselement. Vervang **[naam van uw pakket]** met de naam van het pakket waarmee u uw app hebt gemaakt. In de volgende stap maakt u de klasse MyADMMessageHandler. 

    ```xml
        <amazon:enable-feature
            android:name="com.amazon.device.messaging"
            android:required="true"/>
        <service
            android:name="[YOUR PACKAGE NAME].MyADMMessageHandler"
            android:exported="false" />
        <receiver
            android:name="[YOUR PACKAGE NAME].MyADMMessageHandler$Receiver"
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
                <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
                <!-- Replace the name in the category tag with your app's package name. -->
                <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>
    ```

## <a name="create-your-adm-message-handler"></a>De ADM-berichtenhandler maken

1. Voeg een nieuwe klasse aan de `com.fabrikam.mykindleapp` -pakket in het project die eigenschappen van overneemt `com.amazon.device.messaging.ADMMessageHandlerBase` en noem het `MyADMMessageHandler`, zoals wordt weergegeven in de volgende afbeelding:

    ![MyADMMessageHandler klasse maken](./media/notification-hubs-kindle-get-started/create-adm-message-handler.png)

2. Voeg de volgende `import` instructies toe aan de `MyADMMessageHandler` klasse:

    ```java
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.support.v4.app.NotificationCompat;
    import android.util.Log;
    import com.amazon.device.messaging.ADMMessageReceiver;
    import com.microsoft.windowsazure.messaging.NotificationHub;
    ```

3. Voeg de volgende code toe aan de klasse die u hebt gemaakt. Houd er rekening mee de `[HUB NAME]` en `[LISTEN CONNECTION STRING]` met de naam van uw notification hub en listen-verbindingsreeks: 

    ```java
    public static final int NOTIFICATION_ID = 1;
    private NotificationManager mNotificationManager;
    NotificationCompat.Builder builder;
    private static NotificationHub hub;

    public static NotificationHub getNotificationHub(Context context) {
        Log.v("com.wa.hellokindlefire", "getNotificationHub");
        if (hub == null) {
            hub = new NotificationHub("[HUB NAME]", "[HUB NAMESPACE CONNECTION STRING]", context);
        }
        return hub;
    }

    public MyADMMessageHandler() {
        super("MyADMMessageHandler");
    }

    @Override
    protected void onMessage(Intent intent) {
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
    }

    @Override
    protected void onRegistrationError(String s) {

    }

    @Override
    protected void onRegistered(String s) {
        try {
            getNotificationHub(getApplicationContext()).register(s);
        } catch (Exception e) {
            Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
        }
    }

    @Override
    protected void onUnregistered(String s) {
        try {
            getNotificationHub(getApplicationContext()).unregister();
        } catch (Exception e) {
            Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
        }
    }

    public static class Receiver extends ADMMessageReceiver
    {
        public Receiver()
        {
            super(MyADMMessageHandler.class);
        }
    }

    private void sendNotification(String msg) {
        Context ctx = getApplicationContext();

        mNotificationManager = (NotificationManager)
                ctx.getSystemService(Context.NOTIFICATION_SERVICE);

        PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);


        NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                        .setSmallIcon(R.mipmap.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle()
                                .bigText(msg))
                        .setContentText(msg);

        mBuilder.setContentIntent(contentIntent);
        mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
    }
    ```

## <a name="add-your-api-key-to-your-app"></a>Uw API-sleutel aan uw app toevoegen
1. Volg deze stappen voor het toevoegen van een elementenmap aan het project. 
    1. Schakel over naar de **Project** weergeven. 
    2. Met de rechtermuisknop op **app**.
    3. Selecteer **Nieuw**.
    4. Selecteer **map**. 
    5. Selecteer **map Assets**. 

        ![Menu voor activa-map toevoegen](./media/notification-hubs-kindle-get-started/add-assets-folder-menu.png)    
    6. Op de **onderdeel configureren** pagina, de volgende stappen uit:
        1. Selecteer **locatie wijzigen**
        2. Bevestig dat de map is ingesteld op: `src/main/assets`.
        3. Selecteer **Finish**. 
        
            ![Map voor assets configureren](./media/notification-hubs-kindle-get-started/configure-asset-folder.png)
2. Toevoegen van een bestand met de naam **api_key.txt** naar de **activa** map. Vouw in de structuurweergave wordt weergegeven, **app**, vouw **src**, vouw **belangrijkste**, en met de rechtermuisknop op **activa**, wijst u **nieuw**, en selecteer vervolgens **bestand**. Voer **api_key.txt** voor de bestandsnaam. 3. 
5. Kopieer de API-sleutel die u hebt gegenereerd in de portal voor ontwikkelaars van Amazon naar het bestand api_key.txt. 
6. Maak het project. 

## <a name="run-the-app"></a>De app uitvoeren
1. Veeg vanaf de bovenkant van de Kindle-apparaat, en klikt u op **instellingen**, en klik vervolgens op **Mijn account** en registreren met een geldig Amazon-account.
2. De app uitvoeren op een apparaat Kindle vanaf Android Studio. 

> [!NOTE]
> Als er een probleem optreedt, controleert u het tijdstip van de emulator (of het apparaat). De tijdswaarde moet juist zijn. Voer de volgende opdracht uit de map Android SDK platform-hulpprogramma's uit als u de tijd van de Kindle-emulator wilt wijzigen:

```shell
adb shell  date -s "yyyymmdd.hhmmss"
```

## <a name="send-a-notification-message"></a>Een testmelding verzenden

Een bericht verzenden met .NET:

```csharp
static void Main(string[] args)
{
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

    hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
}
```

Zie voor een voorbeeld van code, [in dit voorbeeld op GitHub](https://github.com/Azure/azure-notificationhubs-dotnet/blob/master/Samples/SendPushSample/SendPushSample/Program.cs).

![][7]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u meldingen uitgezonden naar al uw Kindle-apparaten die zijn geregistreerd bij de back-end. Ga verder met de volgende zelfstudie als u wilt weten hoe u pushmeldingen kunt verzenden naar specifieke Kindle-apparaten:  In de volgende zelfstudie wordt uitgelegd hoe u pushmeldingen naar specifieke Android-apparaten verstuurt, maar deze logica kan ook worden gebruikt voor het versturen van pushmeldingen naar specifieke Kindle-apparaten.

> [!div class="nextstepaction"]
>[Pushmeldingen verzenden naar specifieke apparaten](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- URLs. -->
[Amazon developer portal]: https://developer.amazon.com/home.html
[download the SDK]: https://developer.amazon.com/public/resources/development-tools/sdk
[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
