---
title: Azure Event Hubs Capture inschakelen via de portal | Microsoft Docs
description: Schakel de functie Event Hubs Capture in met behulp van de Azure-portal.
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 1f3d373944b909db290f6cf2da7bf12a8a00e1c5
ms.contentlocale: nl-nl
ms.lasthandoff: 06/28/2017


---

<a id="enable-event-hubs-capture-using-the-azure-portal" class="xliff"></a>

# Event Hubs Capture inschakelen met behulp van de Azure-portal

Wanneer u de gebeurtenishub maakt, kunt u Capture configureren met behulp van de [Azure-portal](https://portal.azure.com). U kunt Capture inschakelen door te klikken op de knop **Aan** in de portalblade **Event Hub maken**. U configureert vervolgens een Opslagaccount en -container door te klikken op de sectie **Container** van de blade. Omdat Event Hubs Capture gebruikmaakt van service-naar-serviceverificatie met opslag, hoeft u geen verbindingsreeks voor opslag op te geven. De objectkiezer selecteert automatisch de resource-URI voor uw opslagaccount. Als u Azure Resource Manager gebruikt, moet u deze URI expliciet als een tekenreeks opgeven.

Het standaardtijdvenster is 5 minuten. De minimale waarde is 1, de maximale 15. Het venster **Grootte** heeft een bereik van 10-500 MB.

![][1]

<a id="adding-capture-to-an-existing-event-hub" class="xliff"></a>

## Capture aan een bestaande gebeurtenishub toevoegen

Capture kan worden geconfigureerd op bestaande gebeurtenishubs in Event Hubs-naamruimten. De functie is niet beschikbaar voor oudere naamruimten van het type **Messaging** of **Mixed**. Om Capture in te schakelen op een bestaande gebeurtenishub of de Capture-instellingen te wijzigen, klikt u op de naamruimte om de blade **Essentials** te laden en vervolgens klikt u op de gebeurtenishub waarvan u de Capture-instelling wilt inschakelen of wijzigen. Ten slotte klikt u op de sectie **Eigenschappen** van de geopende blade zoals weergegeven in de volgende afbeelding:

![][2]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png

<a id="next-steps" class="xliff"></a>

## Volgende stappen

U kunt ook Event Hubs Capture configureren met behulp van Azure Resource Manager-sjablonen. Zie voor meer informatie [Capture inschakelen met behulp van een Azure Resource Manager-sjabloon](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).

