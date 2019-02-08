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
ms.openlocfilehash: e6b949824ec5da60c5e2485be830e61d156a11ff
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55830705"
---
1. Meld u aan bij de [Firebase-console](https://firebase.google.com/console/). Maak een nieuw Firebase-project als u er nog geen hebt.
2. Nadat u uw project hebt gemaakt, selecteert u **Firebase toevoegen aan uw Android-app**. 

    ![Firebase toevoegen aan uw Android-app](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Op de **Firebase toevoegen aan uw Android-app** pagina, de volgende stappen uit: 
    1. Voor **Android pakketnaam**, Kopieer de waarde van uw **applicationId** in van uw toepassing **build.gradle** bestand. In dit voorbeeld heeft `com.fabrikam.fcmtutorial1app`. 

        ![Geef de naam van het pakket](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. Selecteer **app registreren**. 
4. Selecteer **downloaden van google-services.json**, sla het bestand in de **app** map van uw project en selecteer vervolgens **volgende**. 

    ![Google-services.json downloaden](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
5. Controleer het volgende **configuratiewijzigingen** aan uw project in Android Studio. 
    1.  In uw **niveau van het project-build.gradle** bestand (&lt;project&gt;/build.gradle), de volgende instructie voor toevoegen de **afhankelijkheden** sectie. 

        ```
        classpath 'com.google.gms:google-services:4.0.1'
        ```
    2. In uw **op app-niveau build.gradle** bestand (&lt;project&gt;/&lt;app-module&gt;/build.gradle), de volgende instructie voor toevoegen de  **afhankelijkheden** sectie. 

        ```
        implementation 'com.google.firebase:firebase-core:16.0.1'
        ```

    3. Voeg de volgende regel toe aan het einde van de **op app-niveau build.gradle** bestand na de sectie dependenices. 

        ```
        apply plugin: 'com.google.gms.google-services'
        ```        
 
        ![build.gradle configuratiewijzigingen](./media/notification-hubs-enable-firebase-cloud-messaging/build-gradle-configurations.png)
6. Selecteer **volgende** op de pagina. 
7. Selecteer **Sla deze stap over** op de pagina. 

    ![De laatste stap overslaan](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)1. 
8. Selecteer in de Firebase-console het tandwiel van uw project. Selecteer vervolgens **Projectinstellingen**.

    ![Selecteer Projectinstellingen](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Als u dit nog niet hebt gedownload de **google-services.json** bestand naar de **app** map van uw Android Studio-project, kunt u doen dit op deze pagina. 
5. Schakel over naar de **Cloud Messaging** tabblad bovenaan. 
6. Kopieer en bewaar de **serversleutel** voor later gebruik. In dat geval gebruikt u deze waarde in voor uw notification hub configureren.
