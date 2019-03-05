---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 8146489a913ce863cee7534331231a248a3ea7ac
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56890976"
---
1. Ga op uw Mac naar de [Azure Portal]. Klik op **Alle Services** > **App Services** > en op de back-end die u zojuist hebt gemaakt. Kies de taal van uw voorkeur in de instellingen van de mobiele app:

    - Objective-C &ndash; **Quickstart** > **iOS (Objective-C)**
    - Swift &ndash; **Quickstart** > **iOS (Swift)**

    Selecteer onder **3. Configureer uw clienttoepassing**, klik op **Downloaden**. Er wordt dan een volledig Xcode-project gedownload, dat vooraf is geconfigureerd om verbinding te maken met uw back-end. Open het project met Xcode.

1. Druk op de knop **Uitvoeren** om het project te bouwen en de app te starten in de iOS-emulator.

1. Klik in de app op het plusteken (**+**), typ zinvolle tekst, zoals *Voltooi de zelfstudie* en klik vervolgens op de knop Opslaan. Er wordt nu een POST-aanvraag verzonden naar de back-end van Azure die u eerder hebt geïmplementeerd. De back-end voegt gegevens van de aanvraag toe aan de SQL-takentabel en stuurt informatie over de nieuw opgeslagen items terug naar de mobiele app. Deze gegevens worden in de lijst in de mobiele app weergegeven.

   ![Snelstartgids-app uitgevoerd op iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure Portal]: https://portal.azure.com/
