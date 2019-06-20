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
ms.openlocfilehash: 9800848b24523a0eb5992e64ce1580cc53b0e3a0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176583"
---
U kunt nu het hulpprogramma Data Explorer in Azure Portal gebruiken om een grafiekdatabase te maken. 

1. Klik op **Data Explorer** > **Nieuwe grafiek**.

    Uiterst rechts wordt het gebied **Grafiek toevoegen** weergegeven. Mogelijk moet u naar rechts scrollen om het te bekijken.

    ![Azure Portal Data Explorer, pagina Grafiek toevoegen](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer-graph.png)

2. Geef op de pagina **Grafiek toevoegen** de instellingen voor de nieuwe grafiek op.

    Instelling|Voorgestelde waarde|Description
    ---|---|---
    Database-id|voorbeelddatabase|Voer *sample-database* in als de naam voor de nieuwe database. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen `/ \ # ?` bevatten of eindigen op een spatie.
    Grafiek-id|voorbeeldgrafiek|Voer *sample-graph* in als de naam voor uw nieuwe verzameling. Voor namen van grafieken gelden dezelfde tekenvereisten als voor database-id's.
    Opslagcapaciteit|Vast (10 GB)|Laat de standaardwaarde **Vast (10 GB)** staan. Deze waarde is de opslagcapaciteit van de database.
    Doorvoer|400 RU‘s|Wijzig de doorvoer in 400 aanvraageenheden per seconde (RU/s). U kunt de doorvoer later opschalen als u de latentie wilt beperken.

3. Zodra het formulier is ingevuld, klikt u op **OK**.
