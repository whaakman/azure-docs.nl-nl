---
title: bestand opnemen
description: bestand opnemen
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 02/05/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: fb27386881e89cd9056d0efccb7d3c301867bd83
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55965984"
---
1. Selecteer in **Android Studio** in het menu de optie **Hulpprogramma’s**, en selecteer **SDK Manager**. 
2. Selecteer de doelversie van de Android SDK die wordt gebruikt in het project, en selecteer **Pakketgegevens weergeven**. 

    ![Android SDK Manager - doelversie selecteren](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Selecteer **Google API’s**, als dit nog niet is geïnstalleerd.

    ![Android SDK Manager - Google API’s geselecteerd](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. Schakel over naar het tabblad **SDK-hulpprogramma's** . Als u Google Play Service nog niet hebt geïnstalleerd, selecteert u **Google Play Services** zoals wordt weergegeven in de onderstaande afbeelding. Klik vervolgens op **Toepassen** om met de installatie te beginnen. Noteer het SDK-pad om het in een later stadium te kunnen gebruiken.

    ![Android SDK Manager - Google Play Services geselecteerd](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. Klik op **OK** zodra het dialoogvenster **Wijziging bevestigen** wordt weergegeven. De vereiste onderdelen worden geïnstalleerd met behulp van het installatieprogramma voor onderdelen. Selecteer **Voltooien** zodra de onderdelen zijn geïnstalleerd.
4. Selecteer **OK** om het dialoogvenster **Instellingen voor nieuwe projecten** te sluiten.  
5. Open het bestand `build.gradle` in de **app**-map en voeg deze regel toe onder `dependencies`. 

    ```gradle
    implementation 'com.google.android.gms:play-services-gcm:16.0.0'
    ```
5. Selecteer in de werkbalk het pictogram **Nu synchroniseren**.

    ![Synchroniseren met Gradle](./media/notification-hubs-android-studio-add-google-play-services/gradle-sync.png)
1. Open **AndroidManifest.xml** en voeg dit label toe aan het label *toepassing*.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
