---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 505eac0996129a17b6b68e8ab4ea2d4fc80fd473
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132859"
---
1. Ga naar de [Azure Portal]. Klik op **Door alles bladeren** > **Mobile Apps** > de back-end die u zojuist hebt gemaakt. Klik in de instellingen van de mobiele app op **Quickstart** > **Android**. Onder **Uw clienttoepassing configureren** klikt u op **Downloaden**. Er wordt dan voor een app een volledig Android-project gedownload dat vooraf is geconfigureerd om verbinding te maken met uw back-end. 
2. Open het project met **Android Studio** via **Project importeren (Eclipse ADT, Gradle enzovoort)**. Zorg ervoor dat u deze importselectie maakt om eventuele JDK-fouten te voorkomen.
3. Druk op de knop **App uitvoeren** om het project te bouwen en de app te starten in de Android-emulator.
4. Typ zinvolle tekst in de app, zoals *De zelfstudie voltooien* en klik vervolgens op de knop Toevoegen. Er wordt nu een POST-aanvraag verzonden naar de back-end van Azure die u eerder hebt geïmplementeerd. De back-end voegt gegevens van de aanvraag toe aan de SQL-takentabel en stuurt informatie over de nieuw opgeslagen items terug naar de mobiele app. Deze gegevens worden in de lijst in de mobiele app weergegeven. 
   
    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure Portal]: https://portal.azure.com/
