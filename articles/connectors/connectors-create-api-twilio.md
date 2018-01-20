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
ms.openlocfilehash: 50361a3342a0d14ae02b2cb478bbb0f74b61bba0
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-twilio-connector"></a>Aan de slag met de Twilio-connector
Verbinding maken met Twilio globale SMS, MMS en IP-berichten te verzenden en ontvangen. Met Twilio, kunt u het volgende doen:

* Bouw uw zakelijke flow op basis van de gegevens die u van Twilio krijgt. 
* Acties als er een bericht en lijst berichten gebruiken. Deze acties reageert en vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld, wanneer u een nieuwe Twilio-bericht ontvangt, kunt u nemen dit bericht en een Service Bus-werkstroom. 

Aan de slag door het maken van een logische app; Zie [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-twilio"></a>Maak een verbinding met Twilio
Wanneer u deze Connector aan uw logische apps toevoegen, typt u de volgende Twilio-waarden:

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| Account-id |Ja |Voer uw Twilio-account-ID |
| Toegangstoken |Ja |Voer uw Twilio-toegangstoken |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Als u een toegangstoken Twilio hebt, raadpleegt u [gebruikersidentiteit & toegangstokens](https://www.twilio.com/docs/api/chat/guides/identity).

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/twilio/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [API's lijst](apis-list.md).