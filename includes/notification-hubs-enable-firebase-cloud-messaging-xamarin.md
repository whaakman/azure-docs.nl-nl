---
title: bestand opnemen
description: bestand opnemen
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/11/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7e047ba57a61d2f327544ec795f640f5066962f6
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509836"
---
1. Meld u aan bij de [Firebase-console](https://firebase.google.com/console/). Maak een nieuw Firebase-project als u er nog geen hebt.
2. Nadat u uw project hebt gemaakt, selecteert u **Firebase toevoegen aan uw Android-app**. 

    ![Firebase toevoegen aan uw Android-app](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)

3. Op de **Firebase toevoegen aan uw Android-app** pagina, de volgende stappen uit: 
1. 
    1. Voor de **Android pakketnaam**, voer een naam in voor het pakket. Bijvoorbeeld: `tutorials.tutoria1.xamarinfcmapp`. 

        ![Geef de naam van het pakket](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)

2. Selecteer **app registreren**. 
1. 
1. Selecteer **downloaden van google-services.json**. Sla het bestand in de **app** map van uw project en selecteer **volgende**. 

    ![Google-services.json downloaden](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)

6. Selecteer **Next**. 
7. Selecteer **Sla deze stap over**. 

    ![De laatste stap overslaan](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)

8. Selecteer in de Firebase-console het tandwiel van uw project. Selecteer vervolgens **Projectinstellingen**.

    ![Selecteer Projectinstellingen](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)

4. Als u dit nog niet hebt gedownload de **google-services.json** -bestand, kunt u doen downloaden op deze pagina. 

1. Schakel over naar de **Cloud Messaging** tabblad bovenaan. 

1. Kopieer en bewaar de **oude serversleutel** voor later gebruik. In dat geval gebruikt u deze waarde in voor uw notification hub configureren.
