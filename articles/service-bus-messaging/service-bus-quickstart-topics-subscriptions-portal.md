---
title: Snelstartgids - Azure portal gebruiken om Service Bus-onderwerpen en abonnementen te maken | Microsoft Docs
description: In deze snelstartgids leert u hoe u een Service Bus-onderwerp en abonnementen op dat onderwerp maakt met behulp van de Azure-portal.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: e943dbff71fdbd98773587c8ac5a62dd081b49a2
ms.sourcegitcommit: a95dcd3363d451bfbfea7ec1de6813cad86a36bb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62739562"
---
# <a name="quickstart-use-the-azure-portal-to-create-a-service-bus-topic-and-subscriptions-to-the-topic"></a>Quickstart: De Azure portal gebruiken voor het maken van een Service Bus-onderwerp en -abonnementen naar het onderwerp
In deze snelstartgids gebruikt u de Azure-portal te maken van een Service Bus-onderwerp en maak abonnementen op dat onderwerp. 

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Wat zijn Service Bus-onderwerpen en -abonnementen?
Service Bus-onderwerpen en -abonnementen bieden ondersteuning voor een berichtencommunicatiemodel op basis van *publiceren/abonneren*. Wanneer u gebruikmaakt van onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet direct met elkaar. In plaats daarvan wisselen ze berichten uit via een onderwerp dat als intermediair fungeert.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Tegenstelling tot het Service Bus-wachtrijen, waarin elk bericht wordt verwerkt door een enkele gebruiker voorzien onderwerpen en -abonnementen in de vorm van een een-op-veel communicatie, met behulp van een patroon voor publiceren/abonneren. Het is mogelijk meerdere abonnementen voor een onderwerp te registreren. Wanneer een bericht naar een onderwerp wordt verzonden, wordt dit vervolgens beschikbaar gemaakt voor elk abonnement, waar het vervolgens zelfstandig wordt afgehandeld/verwerkt. Een abonnement op een onderwerp lijkt op een virtuele wachtrij die kopieën van de berichten ontvangen die naar het onderwerp zijn verzonden. U kunt optioneel filterregels voor een onderwerp op basis van per abonnement, zodat u kunt filteren of beperken welke berichten naar een onderwerp worden ontvangen door welke berichtabonnementen registreren.

Service Bus-onderwerpen en abonnementen kunnen u schalen voor het verwerken van een groot aantal berichten over een groot aantal gebruikers en toepassingen.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-topics-three-subscriptions-portal](../../includes/service-bus-create-topics-three-subscriptions-portal.md)]



## <a name="next-steps"></a>Volgende stappen
Als u wilt weten hoe u berichten verzenden naar een onderwerp en de berichten uitgewisseld via een abonnement ontvangt, Zie het volgende artikel: Selecteer de programmeertaal die in de inhoudsopgave. 

> [!div class="nextstepaction"]
> [Publiceren en abonneren op berichten](service-bus-dotnet-how-to-use-topics-subscriptions.md)
