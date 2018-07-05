---
title: Maken waarvoor Kafka is ingeschakeld Azure Event Hubs | Microsoft Docs
description: Maak een Kafka ingeschakeld Azure Event Hubs-naamruimte met behulp van de Azure portal
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 05/07/2018
ms.author: shvija
ms.openlocfilehash: 7a05a89fdf99efb6470ee9c8695f349cf22b8ebb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442124"
---
# <a name="create-kafka-enabled-event-hubs"></a>Maken van eventhubs waarvoor Kafka is ingeschakeld

Azure Event Hubs is een Big Data Platform as a Service (PaaS) waarmee miljoenen gebeurtenissen per seconde kan opnemen, en lage latentie en hoge doorvoer voor realtime analyses en visualisatie te streamen.

Azure Event Hubs voor Kafka-ecosystemen biedt u een eindpunt. Dit eindpunt kunt u uw Event Hubs-naamruimte te begrijpen systeemeigen [Apache Kafka](https://kafka.apache.org/intro) message protocol en API's. Met deze functie kunt u communiceren met uw eventhubs als u met Kafka-onderwerpen zonder te wijzigen van uw clients protocol of uitvoeren van uw eigen clusters. Eventhubs voor Kafka-ecosystemen ondersteunt [Apache Kafka-versies 1.0](https://kafka.apache.org/10/documentation.html) en hoger.

In dit artikel wordt beschreven hoe u een Event Hubs-naamruimte maken en haal de verbindingsreeks die is vereist voor het verbinding maken met Kafka-toepassingen met Kafka-functionaliteit eventhubs.

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Maak een Kafka ingeschakeld Event Hubs-naamruimte

1. Aanmelden bij de [Azure-portal][Azure portal], en klikt u op **een resource maken** op linksboven in het scherm.

2. Zoeken naar Event Hubs en selecteer de opties die hier worden weergegeven:
    
    ![Zoeken naar Event Hubs in de portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Geef een unieke naam op en schakel van Kafka in de naamruimte. Klik op **Create**.
    
    ![Een naamruimte maken](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Nadat de naamruimte is gemaakt, op de **instellingen** klikt u op tab **beleid voor gedeelde toegang** om op te halen van de verbindingsreeks.

    ![Klik op beleid voor gedeelde toegang](./media/event-hubs-create/create-event-hub7.png)

5. U kunt de standaard **RootManageSharedAccessKey**, of een nieuw beleid toe te voegen. Klik op de beleidsnaam en kopieer de verbindingsreeks. 
    
    ![Selecteer een beleid](./media/event-hubs-create/create-event-hub8.png)
 
6. Deze verbindingsreeks toevoegen aan de configuratie van uw Kafka-toepassing.

U kunt nu gebeurtenissen uit uw toepassingen die gebruikmaken van het Kafka-protocol in Event Hubs streamen.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over Event Hubs:

* [Stream naar Event Hubs van uw Kafka-toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Meer informatie over Event Hubs voor Kafka-ecosystemen](event-hubs-for-kafka-ecosystem-overview.md)
* [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
