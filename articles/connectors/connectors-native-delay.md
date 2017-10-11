---
title: Toevoegen van een vertraging in logic apps | Microsoft Docs
description: Overzicht van de vertraging en vertraging-totdat acties en hoe u deze met een Azure logic app gebruikt.
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 5f4f7052d48b4ca4ed91212d970551141e78e852
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Aan de slag met vertraging en vertraging-totdat acties
Met behulp van de vertraging en ' vertraging-totdat ' acties, werkstroom-scenario's kan worden voltooid.

U kunt bijvoorbeeld:

* Wachten totdat een weekdag een statusupdate verzenden via e-mail.
* De werkstroom uit te stellen tot er een HTTP-aanroep is tijd om te voltooien voordat wordt hervat en het ophalen van het resultaat.

Om te beginnen met de actie vertraging in een logische app, Zie [een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>De vertraging acties gebruiken
Een actie is een bewerking die wordt uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](connectors-overview.md).

Hier volgt een voorbeeld van de volgorde van het gebruik van een vertraging stap in een logische app:

1. Klik na het toevoegen van een trigger **nieuwe stap** een actie toevoegen.
2. Zoeken naar **vertraging** online zetten van de acties vertraging. In dit voorbeeld selecteren we **vertraging**.
   
    ![Vertraging acties](./media/connectors-native-delay/using-action-1.png)
3. Voltooi alle van de actie-eigenschappen voor het configureren van de vertraging.
   
    ![Vertraging config](./media/connectors-native-delay/using-action-2.png)
4. Klik op **opslaan** publiceren en activeren van de logische app.

## <a name="action-details"></a>Actiedetails
De trigger terugkeerpatroon heeft de volgende eigenschappen die kunnen worden geconfigureerd.

### <a name="delay-action"></a>Vertraging actie
Deze actie een vertraging de uitvoeren voor een bepaalde periode.
A * houdt in dat een vereist veld.

| Weergavenaam | De naam van eigenschap | Beschrijving |
| --- | --- | --- |
| Aantal * |Aantal |Het aantal tijdseenheden vertraging |
| Eenheid * |eenheid |De tijdseenheid: `Second`, `Minute`, `Hour`, of`Day` |

<br>

### <a name="delay-until-action"></a>Vertraging-totdat de actie
Deze actie een vertraging de uitvoeren tot een opgegeven datum/tijd.
A * houdt in dat een vereist veld.

| Weergavenaam | De naam van eigenschap | Beschrijving |
| --- | --- | --- |
| Jaar * |tijdstempel |Het jaar tot uitstellen tot (GMT) |
| Maand * |tijdstempel |De maand tot uitstellen tot (GMT) |
| Dag * |tijdstempel |De dag uitstellen tot (GMT) |

<br>

## <a name="next-steps"></a>Volgende stappen
Nu uitproberen van het platform en [een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md). U kunt de beschikbare connectors in logische apps door te kijken verkennen onze [API's lijst](apis-list.md).

