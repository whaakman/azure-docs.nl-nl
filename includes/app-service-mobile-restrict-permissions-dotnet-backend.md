---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: b609a708a987194398c53bdf83f0d6e1f281808d
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "67176528"
---
Api's in een Mobile Apps back-end kunnen standaard anoniem worden aangeroepen. Vervolgens moet u de toegang beperken tot alleen geverifieerde clients.  

* **Node. js-back-end (via de Azure Portal)** :  

    Klik in uw Mobile Apps instellingen op **eenvoudige tabellen** en selecteer uw tabel. Klik op **machtigingen wijzigen**, selecteer **geverifieerde toegang alleen** voor alle machtigingen en klik vervolgens op **Opslaan**.
* **.Net-back-C#end ()** :  

    Navigeer in het server project naar **controllers** > **TodoItemController.cs**. Voeg als `[Authorize]` volgt het kenmerk toe aan de klasse **TodoItemController** . Als u de toegang tot specifieke methoden wilt beperken, kunt u dit kenmerk ook Toep assen op deze methoden in plaats van op de klasse. Publiceer het server project opnieuw.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Node. js-back-end (via node. js-code)** :  

    Als u verificatie wilt vereisen voor toegang tot de tabel, voegt u de volgende regel toe aan het node. js-server script:

        table.access = 'authenticated';

    Zie [How to: (Engelstalig) voor meer informatie. Verificatie vereisen voor toegang tot tabellen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Zie [How to: (Engelstalig) voor meer informatie over het downloaden van het project Quick Start van uw site. Down load het project van de Snelstartgids voor de back-](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart)end van node. js met git.
