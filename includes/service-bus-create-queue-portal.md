---
title: bestand opnemen
description: bestand opnemen
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 06/29/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7ed298fc8f13685c4872c4c54ba1e447debea79f
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702629"
---
Zorg dat u al een Service Bus-naamruimte hebt gemaakt, zoals [hier][namespace-how-to] wordt weergegeven.

1. Meld u aan bij [Azure Portal][azure-portal].
2. Klik in het navigatiedeelvenster links in de portal op **Service Bus** (als u geen **Service Bus** ziet, klik dan op **Alle services**).
3. Klik op de naamruimte waarin u de wachtrij wilt maken. In dit geval is dit **sbnstest1**.
   
    ![Een wachtrij maken][createqueue1]
4. Klik in het venster voor de naamruimte op **Wachtrijen** en klik vervolgens in het venster **Wachtrijen** op **+ Wachtrij**.
   
    ![Selecteer Wachtrijen][createqueue2]
5. Voer de waarde voor **Wachtrijnaam** in en hanteer voor de andere waarden de standaardinstellingen.
   
    ![Selecteer Nieuw][createqueue3]
6. Klik onder aan het venster op **Maken**.

[createqueue1]: ./media/service-bus-create-queue-portal/create-queue1.png
[createqueue2]: ./media/service-bus-create-queue-portal/create-queue2.png
[createqueue3]: ./media/service-bus-create-queue-portal/create-queue3.png

[namespace-how-to]: ../articles/service-bus-messaging/service-bus-create-namespace-portal.md
[azure-portal]: https://portal.azure.com
