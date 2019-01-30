---
title: bestand opnemen
description: bestand opnemen
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 01/04/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: f00ca7ddf44a9d5b850cd47520970a0396a0c1b5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453084"
---
1. Open Android SDK Manager door te klikken op het pictogram op de werkbalk van Android Studio of door in het menu te klikken op **Extra** > **Android** > **SDK Manager**. Zoek naar de doelversie van de Android SDK die in uw project wordt gebruikt, open deze door te klikken op **Pakketgegevens weergeven** en kies **Google API's**, als dat nog niet is geïnstalleerd.
2. Klik op het tabblad **SDK-hulpprogramma's** . Als u Google Play Service nog niet hebt geïnstalleerd, klikt u op **Google Play Services** zoals hieronder weergegeven. Klik vervolgens op **Toepassen** om met de installatie te beginnen. Noteer het SDK-pad om het in een later stadium te kunnen gebruiken.

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Open het bestand `build.gradle` in de app-map.

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Voeg deze regel toe onder `dependencies`:

    ```text
    compile 'com.google.android.gms:play-services-gcm:12.0.0'
    ```
5. Klik op het pictogram **Project met Gradle-bestanden synchroniseren** in de werkbalk.
6. Open **AndroidManifest.xml** en voeg dit label toe aan het label *toepassing*.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
