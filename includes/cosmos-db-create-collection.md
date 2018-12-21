---
title: bestand opnemen
description: bestand opnemen
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: e287741fd6643c2eba192a9e29f46219faf520ec
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111769"
---
U kunt nu het hulpprogramma Data Explorer in Azure Portal gebruiken om een database en een verzameling te maken. 

1. Klik op **Data Explorer** > **Nieuwe verzameling**. 
    
    Uiterst rechts wordt het gebied **Verzameling toevoegen** weergegeven. Mogelijk moet u naar rechts scrollen om het te bekijken.

    ![Azure Portal Data Explorer, deelvenster Verzameling toevoegen](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. Geef op de pagina **Verzameling toevoegen** de instellingen voor de nieuwe verzameling op.

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Database-id|Taken|Voer *Taken* in als de naam voor de nieuwe database. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen `/, \\, #, ?` bevatten en mogen niet eindigen met een spatie.
    Verzamelings-id|Items|Voer *Items* in als de naam voor de nieuwe verzameling. Voor id’s van verzamelingen gelden dezelfde tekenvereisten als voor databasenamen.
    Partitiesleutel| <Your partition key>| Voer een partitiesleutel in, zoals */userid*.
    Doorvoer|400 RU|Wijzig de doorvoer in 400 aanvraageenheden per seconde (RU/s). U kunt de doorvoer later opschalen als u de latentie wilt beperken. 
    
    Naast de voornoemde instellingen kunt u indien gewenst **unieke sleutels** voor de verzameling toevoegen. In dit voorbeeld laten we het veld leeg. Unieke sleutels bieden ontwikkelaars de mogelijkheid om een gegevensintegriteitslaag aan de database toe te voegen. Door een beleid met unieke sleutels te maken als u een verzameling maakt, zorgt u ervoor dat een of meer waarden per partitiesleutel uniek zijn. Raadpleeg het artikel [Unique keys in Azure Cosmos DB](../articles/cosmos-db/unique-keys.md) (Unieke sleutels in Azure Cosmos DB) voor meer informatie.
    
    Klik op **OK**.

    In Data Explorer worden de nieuwe database en verzameling weergegeven.

    ![Data Explorer in Azure Portal, waarin de nieuwe database en verzameling worden weergegeven](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)