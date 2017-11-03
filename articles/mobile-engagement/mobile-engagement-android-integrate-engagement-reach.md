---
title: Azure Mobile Engagement Android SDK-integratie
description: Meest recente updates en -procedures voor Android-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ec3fab3-35ec-458e-bf41-6cdd69e3fa44
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 06/27/2016
ms.author: piyushjo
ms.openlocfilehash: 26ba47b19f3a503693d60d344ad39b9eba74fe99
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-integrate-engagement-reach-on-android"></a>Het integreren van Engagement bereiken op Android
> [!IMPORTANT]
> U moet de integratie procedure beschreven in de manier waarop integreren engagement voor Android-document voordat u deze handleiding volgen.
> 
> 

## <a name="standard-integration"></a>Standaard-integratie

Kopieer Reach-bronbestanden van de SDK in uw project:

* Kopieer de bestanden van de `res/layout` map geleverd met de SDK in de `res/layout` map van uw toepassing.
* Kopieer de bestanden van de `res/drawable` map geleverd met de SDK in de `res/drawable` map van uw toepassing.

Bewerk uw `AndroidManifest.xml` bestand:

* Voeg de volgende sectie (tussen de `<application>` en `</application>` labels):
  
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/plain" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/html" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
              <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
              <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
          </activity>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
              <action android:name="android.intent.action.BOOT_COMPLETED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
          </receiver>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
              <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
          </receiver>
* U moet deze machtiging aan replay-systeemmeldingen die niet is geklikt tijdens het opstarten (anders ze op schijf worden behouden, maar niet meer weergegeven, moet u echt opnemen).
  
          <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
* Geef een pictogram dat wordt gebruikt voor meldingen (zowel in-app en het systeem die zijn) door te kopiëren en bewerken van de volgende sectie (tussen de `<application>` en `</application>` labels):
  
          <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [!IMPORTANT]
> Deze sectie is **verplichte** als u van plan systeemmeldingen gebruiken bent bij het maken van Reach-campagnes. Android wordt voorkomen dat systeemmeldingen zonder pictogrammen wordt weergegeven. Dus als u deze sectie weglaat, wordt uw eindgebruikers niet mogelijk om ze te ontvangen.
> 
> 

* Als u campagnes met systeemmeldingen met behulp van de grote afbeelding maakt, moet u de volgende machtigingen toevoegen (nadat de `</application>` tag) indien deze ontbreken:
  
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
  
  * Op Android M en als uw toepassing gericht op Android-API-niveau 23 of hoger, ``WRITE_EXTERNAL_STORAGE`` machtiging vereist goedkeuring van een gebruiker. Lees [in deze sectie](mobile-engagement-android-integrate-engagement.md#android-m-permissions).
* Voor systeemmeldingen kunt u ook opgeven in de Reach-campagne als het apparaat moet ring en/of trillen. Om te werken, hebt u zeker dat u de volgende machtiging gedeclareerd (nadat de `</application>` tag):
  
          <uses-permission android:name="android.permission.VIBRATE" />
  
  Zonder deze machtiging Android voorkomt u dat systeemmeldingen worden weergegeven als u de ring of de optie vibrate in de Reach-campagnes gecontroleerd.

## <a name="native-push"></a>Native Pushberichten
Nu u de Reach-module hebt geconfigureerd, moet u native pushberichten om het ontvangen van de campagnes op het apparaat te kunnen configureren.

We ondersteunen twee services op Android:

* Google Play-apparaten: Gebruik [Google Cloud Messaging] volgens de [hoe GCM integreren met Engagement handleiding](mobile-engagement-android-gcm-integrate.md) handleiding.
* Amazon-apparaten: Gebruik [Amazon Device Messaging] volgens de [hoe ADM integreren met Engagement handleiding](mobile-engagement-android-adm-integrate.md) handleiding.

Als u zich richten Amazon- en Google Play-apparaten, de twee mogelijke wilt hebben alles wat binnen 1 AndroidManifest.xml/APK voor ontwikkeling. Maar bij het indienen van Amazon, kunnen ze uw toepassing afwijzen als ze GCM code vinden.

In dat geval moet u meerdere APKs gebruiken.

**Uw toepassing is nu gereed om te ontvangen en weergeven van reach-campagnes!**

## <a name="how-to-handle-data-push"></a>Hoe worden gegevens-push verwerkt
### <a name="integration"></a>Integratie
Als u wilt dat uw toepassing te kunnen ontvangen van gegevens-pushes Reach, u moet een subklasse zijn van maken `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` en ernaar wordt verwezen in de `AndroidManifest.xml` bestand (tussen de `<application>` en/of `</application>` labels):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Vervolgens kunt u overschrijven de `onDataPushStringReceived` en `onDataPushBase64Received` retouraanroepen. Hier volgt een voorbeeld:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Category
De categorieparameter is optioneel bij het maken van een campagne Push-gegevens en kunt dat u gegevens wilt filteren pushes. Dit is handig als u verschillende broadcast ontvangers afhandeling van verschillende soorten gegevens-pushes hebt of als u wilt pushen van verschillende typen van `Base64` gegevens en wilt u het type waardoor ze identificeren voordat ze worden geparseerd.

### <a name="callbacks-return-parameter"></a>De retouraanroepen retourparameter
Hier volgen enkele richtlijnen voor het afhandelen van de retourparameter van goed `onDataPushStringReceived` en `onDataPushBase64Received`:

* Een ontvanger broadcast moet retourneren `null` in de callback als het niet weet hoe een gegevens-push verwerkt. U moet de categorie gebruiken om te bepalen of uw broadcast ontvanger het gegevens-push of niet verwerken moet.
* Een van de ontvanger broadcast moet retourneren `true` in de callback als deze de gegevens-push accepteert.
* Een van de ontvanger broadcast moet retourneren `false` in de callback als het gegevens-push herkent, maar deze om welke reden dan ook negeert. Bijvoorbeeld retourneren `false` wanneer de ontvangen gegevens is ongeldig.
* Als een ontvanger retourneert broadcast `true` terwijl een andere een retourneert `false` voor de dezelfde gegevens-push het gedrag is niet gedefinieerd, moet u nooit die.

Het retourtype wordt alleen gebruikt voor de Reach-statistieken:

* `Replied`Als een van de broadcast ontvangers ofwel geretourneerd wordt verhoogd `true` of `false`.
* `Actioned`alleen als een van de broadcast ontvangers geretourneerd wordt verhoogd `true`.

## <a name="how-to-customize-campaigns"></a>Het aanpassen van de campagnes
U kunt de indelingen die beschikbaar zijn in de SDK bereiken wijzigen voor het aanpassen van campagnes.

U moet alle id's die in de indelingen houden en de typen van de weergaven die gebruikmaken van een id in, met name voor tekst en afbeeldingen weergaven houden. Sommige weergaven worden alleen gebruikt voor het verbergen of weergeven van gebieden, zodat hun type kan worden gewijzigd. Controleer of de broncode als u van plan bent om te wijzigen van het type van een weergave in de opgegeven indelingen.

### <a name="notifications"></a>Meldingen
Er zijn twee typen meldingen: systeem en in-app-meldingen die andere indelingsbestanden gebruiken.

#### <a name="system-notifications"></a>Systeemmeldingen
Om aan te passen systeemmeldingen die u wilt gebruiken, de **categorieën**. U kunt naar gaan [categorieën](#categories).

#### <a name="in-app-notifications"></a>In-app-meldingen
Een in-app-melding is standaard een weergave die dynamisch wordt toegevoegd aan de huidige activiteit gebruikersinterface dankzij de Android-methode `addContentView()`. Dit is een melding overlay genoemd. Melding overlays zijn ideaal voor een snelle integratie omdat ze niet vereisen dat u elke lay-out in uw toepassing wijzigen.

Voor het wijzigen van het uiterlijk van uw notification overlays u gewoon het bestand kan wijzigen `engagement_notification_area.xml` aan uw behoeften.

> [!NOTE]
> Het bestand `engagement_notification_overlay.xml` is de taak die wordt gebruikt voor het maken van een melding overlay, waaronder het bestand `engagement_notification_area.xml`. U kunt ook aanpassen aan uw behoeften (bijvoorbeeld voor het systeemvak in de overlay plaatsing).
> 
> 

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Indeling van de melding als onderdeel van de indeling van een activiteit bevatten
Overlays zijn ideaal voor een snelle integratie, maar worden onhandig of neveneffecten hebben in bijzondere gevallen. Het systeem overlay kan worden aangepast op een activiteitenniveau van de, zodat u gemakkelijk om te voorkomen dat bijwerkingen voor speciale activiteiten.

U kunt de indeling van onze melding opnemen in uw bestaande indeling dankzij de Android **omvatten** instructie. Hieronder volgt een voorbeeld van een gewijzigde `ListActivity` lay-out die net bevat een `ListView`.

**Voordat de Engagement-integratie:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Na de Engagement-integratie:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

In dit voorbeeld hebben we een bovenliggende container toegevoegd, omdat de oorspronkelijke indeling een lijstweergave als het bovenste element gebruikt. We hebben ook toegevoegd `android:layout_weight="1"` kunnen hieronder een lijstweergave geconfigureerd met een weergave toevoegen `android:layout_height="fill_parent"`.

De SDK Engagement bereiken worden automatisch gedetecteerd dat de indeling van de melding is opgenomen in deze activiteit en een overlay voor deze activiteit wordt niet worden toegevoegd.

> [!TIP]
> Als u een ListActivity in uw toepassing gebruikt, een zichtbaar Reach-overlay wordt voorkomen dat u reageert op meer items in de lijstweergave geklikt. Dit is een bekend probleem. U kunt dit probleem omzeilen raden we u om in te sluiten van de indeling van de melding in de indeling van uw eigen lijst activiteit zoals in het vorige voorbeeld.
> 
> 

##### <a name="disabling-application-notification-per-activity"></a>Het uitschakelen van de melding per activiteit
Als u niet wilt dat de overlay worden toegevoegd aan de activiteit en als u de indeling van de melding niet in uw eigen lay-out opnemen, kunt u uitschakelen met de overlay voor deze activiteit in de `AndroidManifest.xml` door toe te voegen een `meta-data` sectie, zoals in het volgende voorbeeld:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Categorieën
Als u de opgegeven indelingen wijzigt, wijzigt u het uiterlijk van al uw meldingen. Categorieën kunnen u verschillende gerichte lijkt (mogelijk gedrag) definiëren voor meldingen. Een categorie kan worden opgegeven wanneer u een Reach-campagne maken. Houd er rekening mee dat categorieën ook kunnen u aanpassen aankondigingen en polls, die verderop in dit document wordt beschreven.

Voor het registreren van een categorie-handler voor uw meldingen, moet u een aanroep van toevoegen wanneer de toepassing wordt geïnitialiseerd.

> [!IMPORTANT]
> Lees de waarschuwing voor het kenmerk android: proces \<android-sdk-engagement-process\> in de manier waarop integreren engagement op Android onderwerp voordat u doorgaat.
> 
> 

Het volgende voorbeeld wordt ervan uitgegaan dat u de vorige waarschuwing bevestigd en gebruikt een subklasse zijn van `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

De `MyNotifier` object is de implementatie van de meldings-handler categorie. Het is ofwel een implementatie van de `EngagementNotifier` -interface of een subklasse van de standaardimplementatie: `EngagementDefaultNotifier`.

Houd er rekening mee dat de dezelfde melder verscheidene categorieën kan verwerken, kunt u deze registreren als volgt:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Ter vervanging van de standaardimplementatie categorie, kunt u uw implementatie zoals registreren in het volgende voorbeeld:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

De huidige categorie gebruikt in een handler is doorgegeven als een parameter in de meeste methoden kunt u overschrijven `EngagementDefaultNotifier`.

Deze is doorgegeven als een `String` parameter of indirect in een `EngagementReachContent` object waarvoor een `getCategory()` methode.

U kunt de meeste van een proces voor het maken van de melding wijzigen door gaat herdefiniëren methoden op `EngagementDefaultNotifier`, voor meer geavanceerde aanpassing gerust bekijken op de technische documentatie en op de broncode.

##### <a name="in-app-notifications"></a>In-app-meldingen
Als u alleen alternatieve indelingen worden gebruikt voor een specifieke categorie wilt, kunt u dit implementeren als in het volgende voorbeeld:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Voorbeeld van `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Zoals u ziet is de overlay weergave-id anders dan de standaard één. Het is belangrijk dat elke indeling een unieke id voor overlays gebruiken.

**Voorbeeld van `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Zoals u ziet is de melding gebied weergave-id anders dan de standaard één. Het is belangrijk dat elke indeling wordt gebruikt voor een unieke id voor de melding gebieden.

Dit eenvoudige voorbeeld van de categorie kunt u een toepassing (of in-app) meldingen die aan de bovenkant van het scherm worden weergegeven. Er is niet veranderd met de standaard-id's in het systeemvak zelf.

Als u wijzigen wilt, hebt u definiëren de `EngagementDefaultNotifier.prepareInAppArea` methode. Het is raadzaam om te zoeken op de technische documentatie en op de broncode van `EngagementNotifier` en `EngagementDefaultNotifier` als u wilt dat dit niveau van Geavanceerd aanpassen.

##### <a name="system-notifications"></a>Systeemmeldingen
Door uit te breiden `EngagementDefaultNotifier`, kunt u overschrijven `onNotificationPrepared` voor het wijzigen van de melding die is voorbereid door de standaardimplementatie.

Bijvoorbeeld:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

In dit voorbeeld wordt een Systeemmelding voor een inhoud wordt weergegeven als een continue gebeurtenis geactiveerd wanneer de categorie 'continue' wordt gebruikt.

Als u wilt bouwen de `Notification` object vanaf het begin, kunt u terugkeren `false` aan de methode en de aanroep `notify` uzelf op de `NotificationManager`. In dat geval is het belangrijk dat u een `contentIntent`, een `deleteIntent` en de bericht-id die wordt gebruikt door `EngagementReachReceiver`.

Hier volgt een voorbeeld van een dergelijke implementatie:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Aankondigingen met alleen een melding
Het beheer van het klikken op een melding alleen aankondiging kan worden aangepast door het overschrijven van `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` te wijzigen van het voorbereide `Intent`. Met deze methode kunt u eenvoudig de vlaggen afstemmen.

Bijvoorbeeld om toe te voegen de `SINGLE_TOP` vlag:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Voor oudere Engagement gebruikers, houd er rekening mee dat systeemmeldingen zonder actie URL nu de toepassing start als deze op de achtergrond, zodat deze methode kan worden aangeroepen met een aankondiging zonder actie-URL. U moet rekening houden met die bij het aanpassen van het doel.

U kunt ook implementeren `EngagementNotifier.executeNotifAnnouncementAction` vanaf het begin.

##### <a name="notification-life-cycle"></a>Levenscyclus van de kennisgeving
Wanneer u de standaardcategorie gebruikt, een aantal methoden levenscyclus worden aangeroepen op de `EngagementReachInteractiveContent` object rapport statistieken en de status van de campagne werken:

* Wanneer de melding wordt weergegeven in de toepassing of op de statusbalk plaatsen de `displayNotification` methode (die statistieken) wordt aangeroepen door `EngagementReachAgent` als `handleNotification` retourneert `true`.
* Als de melding wordt gesloten, de `exitNotification` methode wordt aangeroepen, statistiek wordt gerapporteerd en volgende campagnes kunnen nu worden verwerkt.
* Als u de melding klikt, `actionNotification` is aangeroepen, statistiek gemeld en wordt het bijbehorende doel wordt gestart.

Als uw implementatie van `EngagementNotifier` het standaardgedrag omzeilt u aan te roepen van deze methoden van de levenscyclus van de door u zelf hebt. De volgende voorbeelden ziet enkele gevallen waarbij het standaardgedrag wordt overgeslagen:

* U kunt niet uitbreiden `EngagementDefaultNotifier`, zoals u categorie verwerking helemaal geïmplementeerd.
* Voor systeemmeldingen, overrode u de `onNotificationPrepared` en u aangepast `contentIntent` of `deleteIntent` in de `Notification` object.
* Voor in-app-meldingen, u overrode `prepareInAppArea`, zorg ervoor dat toegewezen ten minste `actionNotification` op een van uw U.I bepaalt.

> [!NOTE]
> Als `handleNotification` er wordt een uitzondering, de inhoud wordt verwijderd en `dropContent` wordt aangeroepen. Dit wordt gemeld in statistieken en volgende campagnes kunnen nu worden verwerkt.
> 
> 

### <a name="announcements-and-polls"></a>Aankondigingen en polls
#### <a name="layouts"></a>Indelingen
U kunt de `engagement_text_announcement.xml`, `engagement_web_announcement.xml` en `engagement_poll.xml` bestanden voor het aanpassen van tekst aankondigingen, web aankondigingen en polls.

Deze bestanden delen twee algemene indeling voor de titel gebieden en de knop. De indeling voor de titel is `engagement_content_title.xml` en het eponymous drawable-bestand wordt gebruikt voor de achtergrond. De indeling voor de knoppen met de actie en afsluiten is `engagement_button_bar.xml` en het eponymous drawable-bestand wordt gebruikt voor de achtergrond.

In een poll de indeling van de vraag en hun mogelijkheden zijn dynamisch vergroot met behulp van verschillende keren de `engagement_question.xml` lay-bestand voor de vragen en de `engagement_choice.xml` -bestand voor de opties.

#### <a name="categories"></a>Categorieën
##### <a name="alternate-layouts"></a>Alternatieve indelingen
Zoals meldingen, kan de campagne categorie worden gebruikt voor alternatieve indelingen voor uw aankondigingen en polls hebben.

Bijvoorbeeld: voor het maken van een categorie voor een aankondiging tekst, kunt u uitbreiden `EngagementTextAnnouncementActivity` en verwijst het `AndroidManifest.xml` bestand:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Houd er rekening mee dat de categorie van het filter opzet wordt gebruikt om het verschil met het standaard aankondiging activiteit maken.

De SDK bereiken gebruikt het systeem opzet omzetten van de juiste activiteit voor een specifieke categorie en vallen terug op de standaardcategorie als de omzetting is mislukt.

Hebt u voor het implementeren van `MyCustomTextAnnouncementActivity`, als u alleen wilt wijzigen van de lay-out (maar houd de dezelfde weergave-id's), u hoeft alleen te definiëren van de klasse zoals in het volgende voorbeeld:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Ter vervanging van de standaardcategorie van tekst aankondigingen gewoon vervangen `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` door uw implementatie.

Web-aankondigingen en polls kunnen worden aangepast op soortgelijke wijze.

U kunt uitbreiden voor web-aankondigingen `EngagementWebAnnouncementActivity` en declareren van de activiteit in de `AndroidManifest.xml` zoals in het volgende voorbeeld:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

U kunt uitbreiden voor polls `EngagementPollActivity` en declareren uw in de `AndroidManifest.xml` zoals in het volgende voorbeeld:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Implementatie maken
U kunt de categorieën voor uw activiteiten aankondiging (en de poll) implementeren zonder het uitbreiden van een van de `Engagement*Activity` klassen die zijn opgegeven door de SDK bereiken. Dit is bijvoorbeeld handig als u wilt definiëren van een indeling die niet de dezelfde weergaven worden gebruikt als de standaard-indelingen.

Zoals voor aanpassing van de geavanceerde melding, is het raadzaam om te kijken naar de broncode van de standaardimplementatie.

Hier volgen enkele dingen rekening moet houden: Reach de activiteit met een specifiek doel (overeenkomt met de opzet filter), plus een extra parameter die de inhoud-id wordt gestart.

U kunt dit doen voor het ophalen van het inhoudsobject waarin de velden die u hebt opgegeven bij het maken van de campagne op de website:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

U moet voor statistieken, rapporteert de inhoud wordt weergegeven in de `onResume` gebeurtenis:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Vergeet niet om aan te roepen ofwel `actionContent(this)` of `exitContent(this)` op het inhoudsobject voordat de activiteit wordt verplaatst naar achtergrond.

Als niet aan te ofwel roepen `actionContent` of `exitContent`, statistieken niet verzonden (d.w.z. geen analytics op de campagne) en belangrijker nog, de volgende campagnes wordt niet gewaarschuwd totdat het toepassingsproces opnieuw wordt gestart.

De afdrukstand of andere configuratiewijzigingen kunnen de code maken lastig zijn om te bepalen of de activiteit krijgt de achtergrond of niet, de standaardimplementatie wordt gecontroleerd of de inhoud wordt gerapporteerd als is afgesloten als de gebruiker de activiteit verlaat (ofwel door te drukken `HOME`of `BACK`) maar niet als de afdrukstand wordt gewijzigd.

Dit is het interessante deel van de implementatie:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Zoals u zien kunt als u aangeroepen `actionContent(this)` en vervolgens de activiteit voltooid `exitContent(this)` veilig kan worden aangeroepen zonder geen effect.

[here]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html
