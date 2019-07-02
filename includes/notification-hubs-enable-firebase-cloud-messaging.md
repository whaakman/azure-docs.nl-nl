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
ms.openlocfilehash: fef6122eceda213fb6353ada53033d0d1e27fd7e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509089"
---
1. Meld u aan bij de [Firebase-console](https://firebase.google.com/console/). Maak een nieuw Firebase-project als u er nog geen hebt.
2. Nadat u uw project hebt gemaakt, selecteert u **Firebase toevoegen aan uw Android-app**. 

    ![Firebase toevoegen aan uw Android-app](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Op de **Firebase toevoegen aan uw Android-app** pagina, de volgende stappen uit: 
    1. Voor **Android pakketnaam**, Kopieer de waarde van uw **applicationId** build.gradle-bestand van uw toepassing. In dit voorbeeld heeft `com.fabrikam.fcmtutorial1app`. 

        ![Geef de naam van het pakket](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. Selecteer **app registreren**. 
4. Selecteer **downloaden van google-services.json**, sla het bestand in de **app** map van uw project en selecteer vervolgens **volgende**. 

    ![Google-services.json downloaden](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
5. Controleer het volgende **configuratiewijzigingen** aan uw project in Android Studio. 
    1.  In uw project op serverniveau build.gradle-bestand (&lt;project&gt;/build.gradle), de volgende instructie voor toevoegen de **afhankelijkheden** sectie. 

        ```
        classpath 'com.google.gms:google-services:4.0.1'
        ```
    2. In uw app-niveau build.gradle-bestand (&lt;project&gt;/&lt;app-module&gt;/build.gradle), de volgende instructie voor toevoegen de **afhankelijkheden** sectie. 

        ```
        implementation 'com.google.firebase:firebase-core:16.0.1'
        ```

    3. Voeg de volgende regel aan het einde van het app-niveau build.gradle-bestand na het gedeelte met afhankelijkheden. 

        ```
        apply plugin: 'com.google.gms.google-services'
        ```        
    4. Selecteer **nu synchroniseren** op de werkbalk. 
 
        ![build.gradle configuratiewijzigingen](./media/notification-hubs-enable-firebase-cloud-messaging/build-gradle-configurations.png)
6. Selecteer **Next**. 
7. Selecteer **Sla deze stap over**. 

    ![De laatste stap overslaan](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)
8. Selecteer in de Firebase-console het tandwiel van uw project. Selecteer vervolgens **Projectinstellingen**.

    ![Selecteer Projectinstellingen](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Als u het google-services.json-bestand in dit nog niet hebt gedownload de **app** map van uw Android Studio-project, kunt u doen dit op deze pagina. 
5. Schakel over naar de **Cloud Messaging** tabblad bovenaan. 
6. Kopieer en bewaar de **serversleutel** voor later gebruik. In dat geval gebruikt u deze waarde in voor uw hub configureren.
