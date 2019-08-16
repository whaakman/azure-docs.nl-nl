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
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "68002680"
---
U kunt nu het hulp programma Data Explorer in de Azure Portal gebruiken om een Data Base en container te maken. 

1. Klik op **Data Explorer** > **nieuwe container**. 
    
    Het gebied **container toevoegen** wordt helemaal rechts weer gegeven. mogelijk moet u naar rechts scrollen om het weer te geven.

    ![Het Azure Portal Data Explorer, Blade container toevoegen](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection-preview.png)

2. Voer op de pagina **container toevoegen** de instellingen voor de nieuwe container in.

    |Instelling|Voorgestelde waarde|Description
    |---|---|---|
    |**Database-id**|Taken|Voer *Taken* in als de naam voor de nieuwe database. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen /, \\, # of ? bevatten en mogen niet eindigen met een spatie. Schakel de optie **doorvoer database inrichten** in, zodat u de door Voer die is ingericht voor de data base, kunt delen in alle containers in de data base. Deze optie helpt ook bij het besparen van kosten. |
    |**Doorvoer**|400|De door Voer bij 400 aanvraag eenheden per seconde (RU/s) behouden. U kunt de doorvoer later opschalen als u de latentie wilt beperken.| 
    |**Container-ID**|Items|Voer *items* in als de naam voor de nieuwe container. Container-Id's hebben dezelfde teken vereisten als database namen.|
    |**Partitie sleutel**| /category| Het voor beeld dat in dit artikel wordt beschreven, maakt gebruik van *Category* als de partitie sleutel. Door een partitiesleutel in te stellen, kan met Azure Cosmos DB de verzameling worden geschaald, zodat deze voldoet aan de behoeften van uw toepassing met betrekking tot opslag en doorvoer. In het algemeen heeft een goede partitiesleutel een breed bereik aan afzonderlijke waarden, wat resulteert in een gelijkmatige verdeling van opslag- en aanvraagvolume voor uw gehele workload. [Meer informatie over partitioneren.](../articles/cosmos-db/partitioning-overview.md)|
    
    Naast de voor gaande instellingen kunt u eventueel **unieke sleutels** voor de container toevoegen. In dit voorbeeld laten we het veld leeg. Unieke sleutels bieden ontwikkelaars de mogelijkheid om een gegevensintegriteitslaag aan de database toe te voegen. Door een beleid voor unieke sleutels te maken tijdens het maken van een container, zorgt u ervoor dat een of meer waarden per partitie sleutel uniek zijn. Raadpleeg het artikel [Unique keys in Azure Cosmos DB](../articles/cosmos-db/unique-keys.md) (Unieke sleutels in Azure Cosmos DB) voor meer informatie.
    
    Selecteer **OK**. Data Explorer worden de nieuwe data base en container weer gegeven.

