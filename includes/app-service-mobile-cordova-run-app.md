---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: 8d7731480b6239c572d39f52b6a0217d2ac48d25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66240231"
---
1. Navigeer naar het oplossingsbestand in het clientproject (.sln) en open dit met Visual Studio.

2. Kies in Visual Studio het oplossingsplatform (Android, iOS of Windows) in de vervolgkeuzelijst naast de begin-pijl. Selecteer een specifiek implementatieapparaat of een emulator door op de vervolgkeuzelijst op de groene pijl te klikken. U kunt het standaard-Android-platform en de standaard-Ripple-emulator gebruiken. Voor geavanceerdere zelfstudies (bijv. over pushmeldingen) moet u een ondersteund apparaat of ondersteunde emulator selecteren.

3. Open het bestand `ToDoActivity.java` in deze map - ZUMOAPPNAME/app/src/main/java/com/voorbeeld/zumoappname. De toepassingsnaam is `ZUMOAPPNAME`.

4. Ga naar de [Azure-portal](https://portal.azure.com/) en navigeer naar de mobiele app die u hebt gemaakt. Op de `Overview` blade, zoekt u de URL op die het openbare eindpunt voor uw mobiele app. Voorbeeld: de sitenaam voor mijn app-naam 'test123' worden https://test123.azurewebsites.net.

5. Ga naar de `index.js` bestand op in ZUMOAPPNAME/www/js/index.js en `onDeviceReady()` methode, Vervang `ZUMOAPPURL` parameter met de openbare eindpunt hierboven.

    `client = new WindowsAzure.MobileServiceClient('ZUMOAPPURL');`
    
    wordt
    
    `client = new WindowsAzure.MobileServiceClient('https://test123.azurewebsites.net');`
    
6. Druk op F5 of klik op de groene pijl om uw Cordova-app te bouwen en uit te voeren. Accepteer het beveiligingsdialoogvenster in de emulator waarin om toegang tot het netwerk wordt gevraagd, als dit wordt weergegeven.

7. Nadat de app is gestart op het apparaat of emulator, typt u zinvolle tekst in **Voer nieuwe tekst**, zoals *Voltooi de zelfstudie* en klik vervolgens op de **toevoegen** knop.

De back-end voegt gegevens van de aanvraag toe aan de TodoItem-tabel in de SQL Database en stuurt informatie over de nieuw opgeslagen items terug naar de mobiele app. Deze gegevens worden in de lijst in de mobiele app weergegeven.

U kunt stap 3 t/m 5 herhalen voor andere platforms.