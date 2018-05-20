---
title: bestand opnemen
description: bestand opnemen
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 04/05/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 540c9775ae56798ce2d2d87fed4924244c31412f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
1. Open Android SDK Manager door te klikken op het pictogram op de werkbalk van Android Studio of door in het menu te klikken op **Extra** > **Android** > **SDK Manager**. Zoek naar de doelversie van de Android SDK die in uw project wordt gebruikt, open deze door te klikken op **Pakketgegevens weergeven** en kies **Google API's**, als dat nog niet is geïnstalleerd.
2. Klik op het tabblad **SDK-hulpprogramma's** . Als u Google Play Service nog niet hebt geïnstalleerd, klikt u op **Google Play Services** zoals hieronder weergegeven. Klik vervolgens op **Toepassen** om met de installatie te beginnen. 
   
    Noteer het SDK-pad om het in een later stadium te kunnen gebruiken. 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Open het bestand **build.gradle** in de app-map.
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Voeg deze regel toe onder *Afhankelijkheden*: 
    
    ```java
        compile 'com.google.android.gms:play-services-gcm:12.0.0'
    ```
5. Klik op het pictogram **Project met Gradle-bestanden synchroniseren** in de werkbalk.
6. Open **AndroidManifest.xml** en voeg dit label toe aan het label *toepassing*.
   
    ```java
    <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />
    ```

