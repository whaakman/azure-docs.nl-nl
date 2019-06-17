---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: a5bde1a56bf6a1f5fca4b775c7c8e9bb7477eb6b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66240275"
---
1. Open het gedownloade clientproject met Xcode.

2. Ga naar de [Azure-portal](https://portal.azure.com/) en navigeer naar de mobiele app die u hebt gemaakt. Op de `Overview` blade, zoekt u de URL op die het openbare eindpunt voor uw mobiele app. Voorbeeld: de sitenaam voor mijn app-naam 'test123' worden https://test123.azurewebsites.net.

3. Open het bestand voor Swift project `ToDoTableViewController.swift` in deze map - ZUMOAPPNAME/ZUMOAPPNAME/ToDoTableViewController.swift. De toepassingsnaam is `ZUMOAPPNAME`.

4. In `viewDidLoad()` methode en vervang `ZUMOAPPURL` parameter met de openbare eindpunt hierboven.

    `let client = MSClient(applicationURLString: "ZUMOAPPURL")`

    wordt
    
    `let client = MSClient(applicationURLString: "https://test123.azurewebsites.net")`
    
5. Open het bestand voor Objective-C-project, `QSTodoService.m` in deze map - ZUMOAPPNAME/ZUMOAPPNAME. De toepassingsnaam is `ZUMOAPPNAME`.

6. In `init` methode en vervang `ZUMOAPPURL` parameter met de openbare eindpunt hierboven.

    `self.client = [MSClient clientWithApplicationURLString:@"ZUMOAPPURL"];`

    wordt
    
    `self.client = [MSClient clientWithApplicationURLString:@"https://test123.azurewebsites.net"];`

7. Druk op de knop **Uitvoeren** om het project te bouwen en de app te starten in de iOS-emulator.

8. Klik in de app op het plusteken ( **+** ), typ zinvolle tekst, zoals *Voltooi de zelfstudie* en klik vervolgens op de knop Opslaan. Er wordt nu een POST-aanvraag verzonden naar de back-end van Azure die u eerder hebt geïmplementeerd. De back-end voegt gegevens van de aanvraag toe aan de SQL-takentabel en stuurt informatie over de nieuw opgeslagen items terug naar de mobiele app. Deze gegevens worden in de lijst in de mobiele app weergegeven.

   ![Snelstartgids-app uitgevoerd op iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure portal]: https://portal.azure.com/
