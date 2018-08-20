---
title: bestand opnemen
description: bestand opnemen
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/25/2018
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 1d3bfb7bc8a5432392dba3b0c5019902b3e59773
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513761"
---
1. Klik op de knop **App Services**, selecteer uw Mobile Apps-back-end, selecteer **Quickstart** en selecteer ten slotte uw clientplatform (iOS, Android, Xamarin, Cordova).

    ![Azure-portal met Mobile Apps Quickstart gemarkeerd][quickstart]

1. Als er nog geen databaseverbinding is geconfigureerd, maakt u er een door de volgende stappen uit te voeren:

    ![Azure-portal met Mobile Apps: verbinding maken met database][connect]

    a. Maak een nieuwe SQL-database en -server. Mogelijk moet u het naamveld van de verbindingstekenreeks laten staan op de standaardwaarde MS_TableConnectionString om stap 3 hieronder te kunnen voltooien.

    ![Azure-portal met Mobile Apps: nieuwe database en server maken][server]

    b. Wacht totdat de gegevensverbinding tot stand is gebracht.

    ![Melding in de Azure-portal dat de gegevensverbinding is gemaakt][notification]

    c. Gegevensverbinding moet zijn gelukt.

    ![Melding 'U hebt al een gegevensverbinding' in de Azure-portal][already-connection]

1. Selecteer bij **2. Een tabel-API maken** de optie Node.js voor **Back-endtaal**.

1. Accepteer de bevestiging en selecteer vervolgens **Takentabel maken**.
    Met deze actie wordt er een nieuwe takentabel in uw database gemaakt.

    >[!IMPORTANT]
    > Als u een bestaande back-end overschakelt naar Node.js, wordt alle inhoud overschreven. Als u in plaats daarvan een .NET-back-end wilt maken, raadpleegt u [Work with the .NET back-end server SDK for Mobile Apps][instructions] (Werken met de .NET-back-end-server-SDK voor Mobile Apps).

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
