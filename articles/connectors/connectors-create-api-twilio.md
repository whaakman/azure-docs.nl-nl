---
title: De Twilio-Connector in uw Azure Logic apps toevoegen | Microsoft Docs
description: Overzicht van de Twilio-Connector met de REST-API-parameters
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 8bcf69a7c8e04cb45d795fd0d6f20d477c15865d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38652002"
---
# <a name="get-started-with-the-twilio-connector"></a>Aan de slag met de Twilio-connector
Verbinding maken met Twilio naar globale SMS-, MMS- en IP-berichten verzenden en ontvangen. Met Twilio, kunt u het volgende doen:

* Bouw uw bedrijfswerkstroom op basis van de gegevens die u van Twilio krijgt. 
* Acties die een bericht, berichten weergeven en meer gebruiken. Deze acties reageert, en vervolgens de uitvoer beschikbaar voor andere acties. Bijvoorbeeld wanneer u een nieuw Twilio-bericht ontvangt, kunt u dit bericht nemen en gebruikt een Service Bus-werkstroom. 

Aan de slag met het maken van een logische app; Zie [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-twilio"></a>Een verbinding maken met Twilio
Wanneer u deze Connector aan uw logische apps toevoegen, voert u de volgende Twilio-waarden:

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| Account-id |Ja |Geef uw Twilio-account-ID |
| Toegangstoken |Ja |Voer uw Twilio-toegangstoken |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Als u een Twilio-toegangstoken hebt, raadpleegt u [gebruiker Identity & Access Tokens](https://www.twilio.com/docs/api/chat/guides/identity).

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook eventuele beperkingen in de [connectorgegevens](/connectors/twilio/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [lijst van API's](apis-list.md).