---
title: Maak Kafka ingeschakeld Azure Event Hubs | Microsoft Docs
description: Maak een Kafka ingeschakeld Azure Event Hubs-naamruimte met behulp van de Azure-portal
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 05/07/2018
ms.author: shvija
ms.openlocfilehash: 4f1d21be3c19dfbc764485fea47b6d4cb2171b3c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="create-kafka-enabled-event-hubs"></a>Maak Kafka ingeschakeld Event Hubs

Azure Event Hubs is een Big Data Platform als een Service (PaaS) die miljoenen gebeurtenissen per seconde opgenomen en lage latentie en hoge doorvoersnelheid voor realtime-analyses en visualisatie biedt streaming.

Azure Event Hubs voor ecosystemen Kafka biedt u een eindpunt. Dit eindpunt maakt uw naamruimte Event Hubs om te begrijpen systeemeigen [Apache Kafka](https://kafka.apache.org/intro) bericht API's en -protocol. Met deze functie kunt u communiceren met uw Event Hubs u net als bij Kafka onderwerpen zonder uw clients protocol wijzigen of uw eigen clusters die zijn uitgevoerd. Event Hubs voor Kafka ecosystemen ondersteunt [Apache Kafka versies 1.0](https://kafka.apache.org/10/documentation.html) en hoger.

Dit artikel wordt beschreven voor het maken van een Event Hubs-naamruimte en ophalen van de verbinding vereist om te verbinden Kafka toepassingen Kafka tekenreeks ingeschakeld Event Hubs.

## <a name="prerequisites"></a>Vereisten

Als u niet een Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Maak een Kafka ingeschakeld Event Hubs-naamruimte

1. Meld u aan bij de [Azure-portal][Azure portal], en klik op **maken van een resource** op linksboven op het scherm.

2. Zoeken naar Event Hubs en selecteer de opties die hier worden weergegeven:
    
    ![Zoeken naar Event Hubs in de portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. **Naamruimte maken**, door een unieke naam geven en Kafka inschakelen op de naamruimte. Klik op **Create**.
    
    ![Een naamruimte maken](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Zodra de naamruimte is gemaakt, op de **instellingen** tabblad **gedeeld toegangsbeleid** ophalen van de verbindingsreeks.

    ![Klik op een gedeeld toegangsbeleid](./media/event-hubs-create/create-event-hub7.png)

5. U kunt de standaard **RootManageSharedAccessKey**, of een nieuw beleid toe te voegen. Klik op de beleidsnaam en kopieer de verbindingsreeks. 
    
    ![Selecteer een beleid](./media/event-hubs-create/create-event-hub8.png)
 
6. Deze verbindingsreeks toevoegen aan de configuratie van uw Kafka-toepassing.

U kunt nu gebeurtenissen uit uw toepassingen die gebruikmaken van het protocol Kafka in Event Hubs streamen.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs, gaat u naar deze koppelingen:

* [Stream in Event Hubs uit uw Kafka-toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Meer informatie over Event Hubs voor Kafka ecosysteem](event-hubs-for-kafka-ecosystem-overview.md)
* [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
