---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: b609a708a987194398c53bdf83f0d6e1f281808d
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133367"
---
Standaard worden-API's in een back-end van Mobile Apps anoniem aangeroepen. Vervolgens moet u de toegang beperken tot alleen geverifieerde clients.  

* **Node.js terug beëindigen (via de Azure portal)** :  

    Klik in de instellingen voor mobiele Apps op **eenvoudige tabellen** en selecteer de tabel. Klik op **machtigingen wijzigen**, selecteer **geverifieerde toegang alleen** voor alle machtigingen, en klik vervolgens op **opslaan**.
* **.NET-back-end (C#)**:  

    In het serverproject, gaat u naar **Controllers** > **TodoItemController.cs**. Toevoegen de `[Authorize]` kenmerk aan de **TodoItemController** klasse, als volgt. Om toegang te beperken tot specifieke methoden, kunt u dit kenmerk ook van toepassing alleen op deze methoden in plaats van de klasse. Het serverproject publiceren.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Node.js-back-end (via Node.js-code)** :  

    Als u wilt verificatie vereisen voor toegang tot tabellen, voeg de volgende regel aan het Node.js-server-script:

        table.access = 'authenticated';

    Zie voor meer informatie, [hoe: verificatie vereisen voor toegang tot tabellen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Zie voor informatie over het downloaden van het snelstartproject dat code van uw site, [hoe: downloaden van de Node.js-back-end snelstartgids code van uw project met behulp van Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
