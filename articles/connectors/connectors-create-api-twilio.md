---
title: De Twilio-Connector in Azure Logic apps toevoegen | Microsoft Docs
description: Overzicht van de Twilio Connector met parameters van de REST-API
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a790ac51b0fea7e3fa379d20e0e094e7ce0d7696
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-twilio-connector"></a>Aan de slag met de Twilio-connector
Verbinding maken met Twilio globale SMS, MMS en IP-berichten te verzenden en ontvangen. Met Twilio, kunt u het volgende doen:

* Bouw uw zakelijke flow op basis van de gegevens die u van Twilio krijgt. 
* Acties als er een bericht en lijst berichten gebruiken. Deze acties reageert en vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld, wanneer u een nieuwe Twilio-bericht ontvangt, kunt u nemen dit bericht en een Service Bus-werkstroom. 

Aan de slag door het maken van een logische app; Zie [een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-twilio"></a>Maak een verbinding met Twilio
Wanneer u deze Connector aan uw logische apps toevoegen, typt u de volgende Twilio-waarden:

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| Account-ID |Ja |Voer uw Twilio-account-ID |
| Toegangstoken |Ja |Voer uw Twilio-toegangstoken |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Als u een toegangstoken Twilio hebt, raadpleegt u [gebruikersidentiteit & toegangstokens](https://www.twilio.com/docs/api/chat/guides/identity).

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/twilio/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [API's lijst](apis-list.md).