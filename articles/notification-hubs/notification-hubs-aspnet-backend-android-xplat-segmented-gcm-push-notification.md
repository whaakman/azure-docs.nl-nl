---
title: Notification Hubs voor belangrijk nieuws zelfstudie - Android
description: Informatie over het gebruik van Azure Service Bus Notification Hubs voor belangrijk nieuws meldingen verzenden naar Android-apparaten.
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 3c23cb80-9d35-4dde-b26d-a7bfd4cb8f81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 76ec01c874fceedab7d76b2ef58e4b45b5489f58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Notification Hubs gebruiken om belangrijk nieuws te verzenden
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Overzicht
Dit onderwerp leest u het gebruik van Azure Notification Hubs voor belangrijk nieuws meldingen naar een Android-app-broadcast. Als u klaar gaat u kunnen registreren voor nieuwscategorieën die u geïnteresseerd bent in op te splitsen en pushmeldingen voor deze categorieën ontvangen. Dit scenario is een algemene patroon voor veel apps waarbij moeten meldingen worden verzonden naar groepen gebruikers die interesse in deze, zoals RSS-lezer, apps voor muziek ventilatoren, enzovoort eerder is gedeclareerd.

Broadcast-scenario's zijn ingeschakeld door een of meer *labels* bij het maken van een registratie in de notification hub. Wanneer u meldingen worden verzonden naar een label, ontvangen alle apparaten die zijn geregistreerd voor het label de melding. Omdat tags gewoon tekenreeksen zijn, hoeven niet vooraf zijn ingericht. Raadpleeg voor meer informatie over tags [Notification Hubs-Routering en code-expressies](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Vereisten
In dit onderwerp is gebaseerd op de app die u hebt gemaakt in [aan de slag met Notification Hubs][get-started]. Voordat u deze zelfstudie begint, u moet al hebt voltooid [aan de slag met Notification Hubs][get-started].

## <a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app.
De eerste stap is het toevoegen van de UI-elementen naar uw bestaande belangrijkste activiteit waarmee de gebruiker kan de categorieën selecteren om te registreren. De categorieën die door een gebruiker is geselecteerd worden op het apparaat opgeslagen. Wanneer de app wordt gestart, wordt de apparaatregistratie van een in uw notification hub met de geselecteerde categorieën gemaakt als labels.

1. Open het bestand res/layout/activity_main.xml en vervang de inhoud met de volgende opties:
   
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">
   
                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>
2. Open uw bestand res/values/strings.xml en voeg de volgende regels:
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    De grafische indeling main_activity.xml ziet er nu als volgt:
   
    ![][A1]
3. Maak nu een klasse **meldingen** in hetzelfde pakket als uw **MainActivity** klasse.
   
        import java.util.HashSet;
        import java.util.Set;
   
        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;
   
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;
   
            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
   
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }
   
            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }
   
            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }
   
            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
   
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
   
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
   
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }
   
        }
   
    Deze klasse maakt gebruik van de lokale opslag voor het opslaan van de categorieën van nieuws die dit apparaat heeft ontvangen. Het bevat ook methoden om te registreren voor deze categorieën.
4. In uw **MainActivity** klasse verwijdert u uw persoonlijke velden voor **NotificationHub** en **GoogleCloudMessaging**, en voeg een veld voor **meldingen**:
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. Klik in de **onCreate** methode, verwijdert u de initialisatie van de **hub** veld en de **registerWithNotificationHubs** methode. Voeg de volgende regels met initialiseren van een exemplaar van de **meldingen** klasse. 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`en `HubListenConnectionString` al moet zijn ingesteld met de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen door de naam van uw notification hub en de verbindingsreeks voor *DefaultListenSharedAccessSignature* die u eerder hebt verkregen.

    > [AZURE.NOTE] Omdat de referenties die worden gedistribueerd met een client-app niet over het algemeen veilig, moet u de sleutel voor listen toegang alleen distribueren met uw clientapp. Luisteren toegang kunnen uw app registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd en kunnen niet worden meldingen verzonden. De volledige toegang tot de sleutel wordt gebruikt in een beveiligde back-endservice voor het verzenden van meldingen en bestaande registraties wijzigen.


1. Vervolgens voegt u de volgende import en `subscribe` methode voor het afhandelen van de knop aanmelden klikt u op de gebeurtenis:
   
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;
   
        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();
   
            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");
   
            notifications.storeCategoriesAndSubscribe(categories);
        }
   
    Deze methode maakt u een lijst met categorieën en gebruikt de **meldingen** klasse voor het opslaan van de lijst in de lokale opslag en registreren van de bijbehorende tags voor uw notification hub. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

Uw app is nu in staat een aantal categorieën opslaan in lokale opslag op het apparaat en registreren bij de notification hub wanneer de gebruiker de selectie van categorieën wijzigt.

## <a name="register-for-notifications"></a>Registreren voor meldingen
Deze stappen registreren bij de notification hub bij het opstarten met behulp van de categorieën die zijn opgeslagen in de lokale opslag.

> [!NOTE]
> Omdat de registratie-id toegewezen door Google Cloud Messaging (GCM) op elk gewenst moment wijzigen kunt, kunt u moet registreren voor meldingen vaak ter voorkoming van fouten van de melding. In dit voorbeeld registreert voor melding van elke keer dat de app wordt gestart. Voor apps die vaak worden uitgevoerd, kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie om bandbreedte te besparen als minder dan een dag is verstreken sinds de vorige registratie.
> 
> 

1. Voeg de volgende code toe aan het einde van de **onCreate** methode in de **MainActivity** klasse:
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    Dit zorgt ervoor dat telkens wanneer de app wordt gestart de categorieën van lokale opslag haalt en een registratie voor deze categorieën vraagt. 
2. Werk vervolgens de `onStart()` methode van de `MainActivity` klasse als volgt:
   
    @Overridevoid onStart() {beveiligd
   
        super.onStart();
        isVisible = true;
   
        Set<String> categories = notifications.retrieveCategories();
   
        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }
   
    Hiermee vernieuwt u de belangrijkste activiteit op basis van de status van de eerder opgeslagen categorieën.

De app is nu voltooid en een set categorieën kunt opslaan in de lokale opslag gebruikt om u te registreren bij de notification hub wanneer de gebruiker de selectie van categorieën wijzigt van apparaat. Vervolgens definiëren we een back-end die categorie meldingen naar deze app kunt verzenden.

## <a name="sending-tagged-notifications"></a>Verzenden van meldingen met tags
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Voer de app en meldingen genereren
1. In Android Studio bouwen van de app en start deze op een apparaat of emulator.
   
    Opmerking dat de UI-app biedt een reeks Schakelknoppen waarmee u kunt kiezen uit de categorieën om u te abonneren op.
2. Een of meer categorieën Schakelknoppen inschakelen en klik vervolgens op **abonneren**.
   
    De app de geselecteerde categorieën converteert naar labels en een nieuwe apparaatregistratie voor de geselecteerde codes aanvragen van de notification hub. De geregistreerde categorieën worden geretourneerd en weergegeven in een pop-upmelding.
3. Een nieuwe melding verzenden door het uitvoeren van de .NET-consoletoepassing.  U kunt ook met tags Sjabloonmeldingen met behulp van het foutopsporingstabblad van uw notification hub in verzenden de [klassieke Azure-Portal].
   
    Meldingen voor de geselecteerde categorieën weergegeven als pop-upmeldingen.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe belangrijk nieuws per categorie-broadcast. Houd rekening met het voltooien van een van de volgende zelfstudies die andere geavanceerde scenario's voor Notification Hubs markeren:

* [Notification Hubs gebruiken voor het uitzenden van gelokaliseerde belangrijk nieuws]
  
    Informatie over het uitbreiden van de app belangrijk nieuws zodat gelokaliseerde verzenden van meldingen.

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Notification Hubs gebruiken voor het uitzenden van gelokaliseerde belangrijk nieuws]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[klassieke Azure-Portal]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
