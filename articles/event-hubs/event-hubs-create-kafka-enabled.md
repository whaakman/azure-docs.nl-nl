---
title: Apache Kafka ingeschakeld Azure Eventhubs maken | Microsoft Docs
description: Maak een Kafka ingeschakeld Azure Event Hubs-naamruimte met behulp van de Azure portal
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2018
ms.author: bahariri
ms.openlocfilehash: 7ce12f9dcaa15ade95274419f99c13d5915dbaaa
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42060183"
---
# <a name="create-apache-kafka-enabled-event-hubs"></a>Apache Kafka ingeschakeld eventhubs maken

Azure Event Hubs is een Big Data Platform as a Service (PaaS) waarmee miljoenen gebeurtenissen per seconde kan opnemen, en lage latentie en hoge doorvoer voor realtime analyses en visualisatie te streamen.

Azure Event Hubs biedt u een Kafka-eindpunt. Dit eindpunt kunt u uw Event Hubs-naamruimte te begrijpen systeemeigen [Apache Kafka](https://kafka.apache.org/intro) message protocol en API's. Met deze functie kunt u communiceren met uw eventhubs als u met Kafka-onderwerpen zonder te wijzigen van uw clients protocol of uitvoeren van uw eigen clusters. Biedt ondersteuning voor Eventhubs [Apache Kafka-versies 1.0](https://kafka.apache.org/10/documentation.html) en hoger.

In dit artikel wordt beschreven hoe u een Event Hubs-naamruimte maken en haal de verbindingsreeks die is vereist voor het verbinding maken met Kafka-toepassingen met Kafka-functionaliteit eventhubs.

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken waarvoor Kafka is ingeschakeld

1. Aanmelden bij de [Azure-portal][Azure portal], en klikt u op **een resource maken** op linksboven in het scherm.

2. Zoek naar Event Hubs en selecteer de hier getoonde opties:
    
    ![Zoeken naar Event Hubs in de portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Geef een unieke naam op en schakel Kafka in op de naamruimte. Klik op **Create**.
    
    ![Een naamruimte maken](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Als de naamruimte is gemaakt, klikt u op het tabblad **Instellingen** op **Beleid voor gedeelde toegang** om de verbindingsreeks op te halen.

    ![Klikken op Beleid voor gedeelde toegang](./media/event-hubs-create/create-event-hub7.png)

5. U kunt de standaardwaarde **RootManageSharedAccessKey** kiezen of een nieuwe beleid toevoegen. Klik op de beleidsnaam en kopieer de verbindingsreeks. 
    
    ![Beleid selecteren](./media/event-hubs-create/create-event-hub8.png)
 
6. Voeg deze verbindingsreeks toe aan de Kafka-toepassingsconfiguratie.

U kunt nu gebeurtenissen vanaf uw toepassing, waarbij gebruikgemaakt wordt van het Kafka-protocol, naar Event Hubs streamen.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over Event Hubs:

* [Stream naar Event Hubs van uw Kafka-toepassingen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Meer informatie over Eventhubs voor Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Meer informatie over Event Hubs](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
