---
title: Een vertraging in logische apps toevoegen | Microsoft Docs
description: Overzicht van de vertraging en de vertraging-tot acties en het gebruik ervan met een logische app van Azure.
services: ''
documentationcenter: ''
author: jeffhollan
manager: erikre
editor: ''
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 15e581454b60319ab734f2fa5faf0d90e0a7c8bf
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893721"
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Aan de slag met de vertraging en de vertraging-tot acties
Met behulp van de vertraging en "vertraging-totdat" acties, werkstroom voor scenario's.

U kunt bijvoorbeeld:

* Wachten tot een weekdag voor een statusupdate verzenden via e-mail.
* De werkstroom uitstellen totdat er een HTTP-aanroep is tijd om te voltooien voordat u hervatten en bij het ophalen van het resultaat.

Om te beginnen met de vertragingsactie in een logische app, Zie [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-delay-actions"></a>De vertraging-acties gebruiken

Een actie is een bewerking die wordt uitgevoerd door de werkstroom die is gedefinieerd in een logische app. 
[Meer informatie over acties](../connectors/apis-list.md).

Hier volgt een voorbeeld van de volgorde van het gebruik van een vertraging stap in een logische app:

1. Na het toevoegen van een trigger, klikt u op **nieuwe stap** een actie toevoegen.
2. Zoeken naar **vertraging** om de vertraging-acties. In dit voorbeeld selecteren we **vertraging**.
   
    ![Vertraging acties](./media/connectors-native-delay/using-action-1.png)
3. Voer een van de eigenschappen van het configureren van de vertraging.
   
    ![Vertraging config](./media/connectors-native-delay/using-action-2.png)
4. Klik op **opslaan** om te publiceren en de logische app activeren.

## <a name="action-details"></a>Actiedetails
De trigger met terugkeerpatroon heeft de volgende eigenschappen die kunnen worden geconfigureerd.

### <a name="delay-action"></a>Vertragingsactie
Deze actie vertraagt het uitvoeren van een bepaalde periode.
Een * betekent dat het een verplicht veld.

| Weergavenaam | Naam van eigenschap | Description |
| --- | --- | --- |
| Aantal * |count |Het aantal tijdseenheden vertraging |
| Unit* |eenheid |De tijdseenheid: `Second`, `Minute`, `Hour`, of `Day` |

<br>

### <a name="delay-until-action"></a>Vertraging-tot actie
Deze actie uitstelt de uitvoering tot een opgegeven datum/tijd.
Een * betekent dat het een verplicht veld.

| Weergavenaam | Naam van eigenschap | Description |
| --- | --- | --- |
| Jaar * |tijdstempel |Het jaar te vertragen tot (GMT) |
| Maand * |tijdstempel |De maand te vertragen tot (GMT) |
| Dag * |tijdstempel |Voor het uitstellen tot (GMT) |

<br>

## <a name="next-steps"></a>Volgende stappen
Nu uitproberen van het platform en [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). U kunt de beschikbare connectors in logic apps verkennen door te kijken onze [lijst van API's](apis-list.md).

