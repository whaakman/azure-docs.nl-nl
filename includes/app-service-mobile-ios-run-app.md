---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 1d1d593b7305e0cd9899f4ec388cb441ced90b10
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134120"
---
1. Ga op uw Mac naar de [Azure Portal]. Klik op **Alle Services** > **App Services** > en op de back-end die u zojuist hebt gemaakt. Kies de taal van uw voorkeur in de instellingen van de mobiele app:

    - Objective-C &ndash; **Quickstart** > **iOS (Objective-C)**
    - Swift &ndash; **Quickstart** > **iOS (Swift)**

    Selecteer onder **3. Configureer uw clienttoepassing**, klik op **Downloaden**. Er wordt dan een volledig Xcode-project gedownload, dat vooraf is geconfigureerd om verbinding te maken met uw back-end. Open het project met Xcode.

1. Druk op de knop **Uitvoeren** om het project te bouwen en de app te starten in de iOS-emulator.

1. Typ in de app een stukje zinvolle tekst, zoals *Voltooi de zelfstudie* en klik vervolgens op het plusteken (**+**). Er wordt nu een POST-aanvraag verzonden naar de back-end van Azure die u eerder hebt geïmplementeerd. De back-end voegt gegevens van de aanvraag toe aan de SQL-takentabel en stuurt informatie over de nieuw opgeslagen items terug naar de mobiele app. Deze gegevens worden in de lijst in de mobiele app weergegeven.

   ![Snelstartgids-app uitgevoerd op iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure Portal]: https://portal.azure.com/
