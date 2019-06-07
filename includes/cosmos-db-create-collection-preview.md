---
title: bestand opnemen
description: bestand opnemen
services: cosmos-db
author: deborahc
ms.service: cosmos-db
ms.topic: include
ms.date: 11/19/2018
ms.author: dech
ms.custom: include file
ms.openlocfilehash: 331886f01345aba576cd8f96f95077f9bbdae704
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754320"
---
U kunt nu het hulpprogramma Data Explorer in Azure portal gebruiken om een database en de container te maken. 

1. Klik op **Data Explorer** > **nieuwe Container**. 
    
    De **Container toevoegen** gebied aan de rechterkant wordt weergegeven, moet u mogelijk naar rechts scrollen om te zien.

    ![De Azure portal Data Explorer, blade Container toevoegen](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection-preview.png)

2. In de **toevoegen container** pagina, de instellingen voor de nieuwe container invoeren.

    |Instelling|Voorgestelde waarde|Description
    |---|---|---|
    |**Database-id**|Taken|Voer *Taken* in als de naam voor de nieuwe database. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen /, \\, # of ? bevatten en mogen niet eindigen met een spatie. Controleer de **inrichten database doorvoer** optie, het kunt u de doorvoer die is ingericht met de database in de containers in de database delen. Deze optie helpt ook bij de kosten te besparen. |
    |**Doorvoer**|400|Laat de doorvoer in 400 aanvraageenheden per seconde (RU/s). U kunt de doorvoer later opschalen als u de latentie wilt beperken.| 
    |**Container-ID**|Items|Voer *Items* als de naam voor de nieuwe container. Container-id's hebben dezelfde tekenvereisten als voor databasenamen.|
    |**Partitiesleutel**| /category| In het voorbeeld dat wordt beschreven in dit artikel wordt *Category* als de partitiesleutel. Door een partitiesleutel in te stellen, kan met Azure Cosmos DB de verzameling worden geschaald, zodat deze voldoet aan de behoeften van uw toepassing met betrekking tot opslag en doorvoer. In het algemeen heeft een goede partitiesleutel een breed bereik aan afzonderlijke waarden, wat resulteert in een gelijkmatige verdeling van opslag- en aanvraagvolume voor uw gehele workload. [Meer informatie over partitioneren.](../articles/cosmos-db/partitioning-overview.md)|
    
    Naast de voornoemde instellingen kunt u optioneel kunt toevoegen **unieke sleutels** voor de container. In dit voorbeeld laten we het veld leeg. Unieke sleutels bieden ontwikkelaars de mogelijkheid om een gegevensintegriteitslaag aan de database toe te voegen. Het maken van een beleid met unieke sleutels tijdens het maken van een container, zorgt u ervoor dat een of meer waarden per partitiesleutel. Raadpleeg het artikel [Unique keys in Azure Cosmos DB](../articles/cosmos-db/unique-keys.md) (Unieke sleutels in Azure Cosmos DB) voor meer informatie.
    
    Selecteer **OK**. Data Explorer worden weergegeven voor de nieuwe database en de container.

