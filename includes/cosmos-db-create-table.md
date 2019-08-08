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
ms.openlocfilehash: 5743d785afb87aef6b3a89af6dc8eb18f66b164d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854668"
---
U kunt nu het hulpprogramma Data Explorer in Azure Portal gebruiken om een database en een tabel te maken. 

1. Selecteer **Data Explorer** > **nieuwe tabel**. 
    
    Helemaal rechts wordt het gebied **Tabel toevoegen** weergegeven. Mogelijk moet u naar rechts scrollen om het te zien.

    ![Data Explorer in de Azure Portal](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

2. Geef op de pagina **Tabel toevoegen** de instellingen voor de nieuwe tabel op.

    Instelling|Voorgestelde waarde|Description
    ---|---|---
    Tabel-id|voorbeeldtabel|De id voor de nieuwe tabel. Voor tabelnamen gelden dezelfde tekenvereisten als voor database-id's. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen `/ \ # ?` bevatten of eindigen op een spatie.
    Doorvoer|400 RU‘s|Wijzig de doorvoer in 400 aanvraageenheden per seconde (RU/s). U kunt de doorvoer later opschalen als u de latentie wilt beperken.

3. Selecteer **OK**.

4. In Data Explorer worden de nieuwe database en tabel weergegeven.

   ![Data Explorer in Azure Portal, met de nieuwe database en tabel](./media/cosmos-db-create-table/azure-cosmos-db-new-table.png)