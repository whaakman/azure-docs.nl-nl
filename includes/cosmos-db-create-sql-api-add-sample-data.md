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
ms.openlocfilehash: 92b739424d1d6f0e1eb89d5993718f5c36162204
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
U kunt nu gegevens aan uw nieuwe verzameling toevoegen met behulp van Data Explorer.

1. De nieuwe database wordt in Data Explorer weergegeven in het deelvenster Verzamelingen. Vouw de database **Taken** uit, vouw de verzameling **Items** uit, klik op **Documenten** en klik vervolgens op **Nieuwe documenten**. 

   ![Nieuwe documenten maken in Data Explorer in de Azure Portal](./media/cosmos-db-create-sql-api-add-sample-data/azure-cosmosdb-data-explorer-new-document.png)
  
2. Voeg nu een document toe aan de verzameling met de volgende structuur.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Zodra u de JSON hebt toegevoegd aan het tabblad **Documenten** klikt u op **Opslaan**.

    ![JSON-gegevens kopiëren en op Opslaan klikken in Data Explorer in Azure Portal](./media/cosmos-db-create-sql-api-add-sample-data/azure-cosmosdb-data-explorer-save-document.png)

4.  Maak nog één document en sla dit op. In het document voegt u een unieke waarde toe voor de eigenschap `id`. Wijzig de andere eigenschappen naar eigen inzicht. De nieuwe documenten kunnen elke gewenste structuur hebben, omdat in Azure Cosmos DB uw gegevens geen schema krijgen opgelegd.