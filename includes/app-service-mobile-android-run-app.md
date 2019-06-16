---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: 63c54f8af91b6b4a76ba49d5e6fc7b3cda9f5b98
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66240321"
---
1. Open het project met **Android Studio** via **Project importeren (Eclipse ADT, Gradle enzovoort)** . Zorg ervoor dat u deze importselectie maakt om eventuele JDK-fouten te voorkomen.

2. Open het bestand `ToDoActivity.java` in deze map - ZUMOAPPNAME/app/src/main/java/com/voorbeeld/zumoappname. De toepassingsnaam is `ZUMOAPPNAME`.

3. Ga naar de [Azure-portal](https://portal.azure.com/) en navigeer naar de mobiele app die u hebt gemaakt. Op de `Overview` blade, zoekt u de URL op die het openbare eindpunt voor uw mobiele app. Voorbeeld: de sitenaam voor mijn app-naam 'test123' worden https://test123.azurewebsites.net.

4. In `onCreate()` methode en vervang `ZUMOAPPURL` parameter met de openbare eindpunt hierboven.
    
    `new MobileServiceClient("ZUMOAPPURL", this).withFilter(new ProgressFilter());` 
    
    wordt
    
    `new MobileServiceClient("https://test123.azurewebsites.net", this).withFilter(new ProgressFilter());`
    
5. Druk op de knop **App uitvoeren** om het project te bouwen en de app te starten in de Android-emulator.

4. Typ zinvolle tekst in de app, zoals *De zelfstudie voltooien* en klik vervolgens op de knop Toevoegen. Er wordt nu een POST-aanvraag verzonden naar de back-end van Azure die u eerder hebt geïmplementeerd. De back-end voegt gegevens van de aanvraag toe aan de SQL-takentabel en stuurt informatie over de nieuw opgeslagen items terug naar de mobiele app. Deze gegevens worden in de lijst in de mobiele app weergegeven.
    ![Snelstartgids voor Android](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)