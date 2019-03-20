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
ms.openlocfilehash: 90ffebf5f3375e94545f82a95f5c240ad845bd94
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57908260"
---
1. Meld u aan bij de [Firebase-console](https://firebase.google.com/console/). Maak een nieuw Firebase-project als u er nog geen hebt.
2. Nadat u uw project hebt gemaakt, selecteert u **Firebase toevoegen aan uw Android-app**. 

    ![Firebase toevoegen aan uw Android-app](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Op de **Firebase toevoegen aan uw Android-app** pagina, de volgende stappen uit: 
    1. Voor **Android pakketnaam**, voer een naam in voor het pakket. Bijvoorbeeld: `tutorials.tutoria1.xamarinfcmapp`. 

        ![Geef de naam van het pakket](./media/notification-hubs-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    2. Selecteer **app registreren**. 
4. Selecteer **downloaden van google-services.json**, sla het bestand in de **app** map van uw project en selecteer vervolgens **volgende**. 

    ![Google-services.json downloaden](./media/notification-hubs-enable-firebase-cloud-messaging/download-google-service-button.png)
6. Selecteer **volgende** op de pagina. 
7. Selecteer **Sla deze stap over** op de pagina. 

    ![De laatste stap overslaan](./media/notification-hubs-enable-firebase-cloud-messaging/skip-this-step.png)
8. Selecteer in de Firebase-console het tandwiel van uw project. Selecteer vervolgens **Projectinstellingen**.

    ![Selecteer Projectinstellingen](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Als u dit nog niet hebt gedownload de **google-services.json** -bestand, kunt u doen dit op deze pagina. 
5. Schakel over naar de **Cloud Messaging** tabblad bovenaan. 
6. Kopieer en bewaar de **serversleutel** voor later gebruik. In dat geval gebruikt u deze waarde in voor uw notification hub configureren.
