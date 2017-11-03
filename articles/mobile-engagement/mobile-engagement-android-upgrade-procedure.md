---
title: Azure Mobile Engagement Android SDK-integratie
description: Meest recente updates en -procedures voor Android-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 11618586-c709-49ca-bcd8-745323ff1af6
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1f047f93fa8bc852b28c86e91d0c007a94fb4299
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="upgrade-procedures"></a>Upgradeprocedures
Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, hebt u de volgende punten overwegen bij het upgraden van de SDK.

U moet verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Bijvoorbeeld als u migreert van 1.4.0 naar u moet eerst Volg de procedure 'van 1.4.0 naar 1.5.0' 1.6.0 vervolgens de procedure 'van 1.5.0 naar 1.6.0'.

Ongeacht de versie die u een van upgrade, u moet vervangen de `mobile-engagement-VERSION.jar` door de nieuwe.

## <a name="from-420-to-421"></a>Van 4.2.0 naar 4.2.1
Deze stap kunt daadwerkelijk worden uitgevoerd op elke versie van de SDK en een beveiligingsverbetering is wanneer u integreert Reach-activiteiten.

U moet nu toevoegen `exported="false"` op alle Reach-activiteiten.

Reach activiteiten ziet er nu als volgt op uw `AndroidManifest.xml`:

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

## <a name="from-400-to-410"></a>Van 4.0.0 naar 4.1.0
De SDK nu ingang nieuwe machtiging model van Android M.

Als u functies van de locatie of grote afbeelding meldingen Lees [in deze sectie](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Naast het nieuwe model van de machtiging we bieden nu ondersteuning configureren locatie functies tijdens runtime.
We zijn nog steeds compatibel zijn met de manifest-parameters voor de locatie, maar gedeprecieerd. Runtime-configuratie wordt gebruikt, verwijdert u de volgende secties van uw ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

en gelezen [procedure wordt dit bijgewerkt](mobile-engagement-android-integrate-engagement.md#location-reporting) runtime-configuratie gebruiken.

## <a name="from-300-to-400"></a>Van 3.0.0 naar 4.0.0
### <a name="native-push"></a>Native pushberichten
Native pushberichten (GCM/ADM) is nu ook gebruikt voor in de app-meldingen dus moet u de referenties van native pushberichten voor elk type pushcampagne configureren.

Als dat niet al Volg [deze procedure](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml
Reach-integratie is gewijzigd in ``AndroidManifest.xml``.

Vervang deze waarde:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Door

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
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

Er is mogelijk een scherm laden nu wanneer u klikt u op een aankondiging (met tekst of web content) of een poll.
U moet deze optie voor dergelijke campagnes werken in 4.0.0 toevoegen:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Resources
Sluit de nieuwe `res/layout/engagement_loading.xml` -bestand in uw project.

## <a name="from-240-to-300"></a>Van 2.4.0 naar 3.0.0
De volgende beschrijft het migreren van een SDK-integratie van de service van Capptain SAS Capptain in een app die is aangedreven door Azure Mobile Engagement. Als u vanaf een eerdere versie migreert, neem contact op met de website Capptain om te migreren naar 2.4.0 eerst en pas de volgende procedure.

> [!IMPORTANT]
> Capptain en Mobile Engagement zijn niet dezelfde services en de procedure die hieronder wordt alleen uitgelegd hoe u voor het migreren van de client-app. Migreren van de SDK in de app wordt niet uw gegevens migreren vanaf de servers Capptain naar de Mobile Engagement-servers.
> 
> 

### <a name="jar-file"></a>JAR-bestand
Vervang `capptain.jar` door `mobile-engagement-VERSION.jar` in uw `libs` map.

### <a name="resource-files"></a>Bronbestanden
Elke resource-bestand dat wordt geleverd (voorafgegaan door `capptain_`) moet worden vervangen door de nieuwe verwijzingen (voorafgegaan door `engagement_`).

Als u deze bestanden hebt aangepast, hebt u opnieuw toepassen op de nieuwe bestanden aanpassing **ook alle id's in de resource-bestanden zijn gewijzigd**.

### <a name="application-id"></a>Toepassings-id
Engagement gebruikt nu een verbindingsreeks voor het configureren van de SDK-id's zoals de toepassings-id.

U moet gebruiken `EngagementAgent.init` methode in uw activiteiten starten als volgt:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven op de Azure Portal.

Verwijder op elke aanroep `CapptainAgent.configure` als `EngagementAgent.init` vervangt deze methode.

De `appId` kunnen niet meer worden geconfigureerd met `AndroidManifest.xml`.

Verwijder dit gedeelte van uw `AndroidManifest.xml` als u deze hebt:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java-API
Elke aanroep van een Javaklasse van onze SDK heeft moet worden gewijzigd; bijvoorbeeld: `CapptainAgent.getInstance(this)` naam moet worden gewijzigd `EngagementAgent.getInstance(this)`, `extends CapptainActivity` naam moet worden gewijzigd `extends EngagementActivity` enzovoort...

Als u met standaard agent voorkeursextensie bestanden zijn geïntegreerd, de standaardnaam is nu `engagement.agent` en de sleutel `engagement:agent`.

Bij het maken van web-aankondigingen de binder Javascript is nu `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml
Een groot aantal wijzigingen hebben plaatsgevonden er, de service niet meer wordt gedeeld en veel ontvangers kunnen niet worden geëxporteerd meer.

De servicedeclaratie is nu eenvoudiger; Verwijder het filter opzet en alle metagegevens erin en toevoegen `exportable=false`.

Plus Alles voor het gebruik van engagement wordt gewijzigd.

Nu ziet er als:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Wanneer u testen Logboeken inschakelen wilt, worden de metagegevens is nu verplaatst naar de code van de toepassing en is gewijzigd:

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

Alle andere metagegevens NET zijn gewijzigd, wordt hier de volledige lijst (met loop rename alleen de door u):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>

            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play en SmartAd bijhouden is verwijderd van de SDK die u zojuist hebt moet u deze verwijderen zonder vervanging:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

De Reach-activiteiten, worden nu als volgt gedefinieerd:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

Als u aangepaste Reach-activiteiten hebt, moet u alleen wijzigen de opzet acties zodat deze overeenkomt met een `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` of `com.microsoft.azure.engagement.reach.intent.action.POLL`.

De broadcast ontvangers zijn gewijzigd, plus we nu toevoegen `exported=false`. Hier volgt een volledige lijst van de ontvangers die u met de nieuwe-specificatie (van loop rename alleen de door u):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Het bijhouden van de ontvanger is verwijderd, zodat u deze sectie verwijderen:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Houd er rekening mee dat de declaratie van uw implementatie van de ontvanger broadcast **EngagementMessageReceiver** is gewijzigd de `AndroidManifest.xml`. Dit is omdat de API voor het verzenden en willekeurige XMPP berichten van willekeurige XMPP entiteiten verwijderen en de API voor het verzenden en ontvangen van berichten tussen apparaten die zijn verwijderd. Dus u moet ook verwijderen de volgende callbacks van uw **EngagementMessageReceiver** implementatie:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

en

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Verwijder op geen enkele aanroep **EngagementAgent** voor:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

en

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard
Proguard configuratie kan worden beïnvloed door rebranding, de regels zijn nu ziet eruit als:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

