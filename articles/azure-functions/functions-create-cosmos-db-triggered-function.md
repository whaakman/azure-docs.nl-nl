---
title: Maak een functie die wordt geactiveerd door Azure Cosmos DB | Microsoft Docs
description: Azure Functions gebruik te maken van een zonder server-functie die wordt aangeroepen wanneer gegevens worden toegevoegd aan een database in Azure Cosmos DB.
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: bc497d71-75e7-47b1-babd-a060a664adca
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/02/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 1ff4c2e024faba777fc479b3cd5864e097bbfce1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-triggered-by-azure-cosmos-db"></a>Maak een functie die wordt geactiveerd door Azure Cosmos-DB

Informatie over het maken van een functie geactiveerd wanneer de gegevens aan worden toegevoegd of gewijzigd in Azure Cosmos DB. Zie voor meer informatie over Azure Cosmos DB, [Azure Cosmos DB: zonder Server database computing met behulp van Azure Functions](..\cosmos-db\serverless-computing-database.md).

![Bekijk het bericht in de logboeken.](./media/functions-create-cosmos-db-triggered-function/quickstart-completed.png)

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

+ Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Een Azure-functie-app maken

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Vervolgens maakt u een functie in de nieuwe functie-app.

<a name="create-function"></a>

## <a name="create-azure-cosmos-db-trigger"></a>Azure Cosmos DB trigger maken

1. Vouw de functie-app uit en klik op de knop **+** naast **Functies**. Als dit de eerste functie in de functie-app is, selecteert u **Aangepaste functie**. U ziet nu de volledige set het functiesjablonen.

    ![De Quick Start-pagina van Functions in Azure Portal](./media/functions-create-cosmos-db-triggered-function/add-first-function.png)

2. Zoek en kies de **Azure CosmosDBTrigger** sjabloon voor de gewenste taal.

    ![De functie Azure Cosmos DB geactiveerd maken](./media/functions-create-cosmos-db-triggered-function/select-cosmos-db-trigger-portal.png)

3. Configureer de nieuwe trigger met de instellingen die zijn opgegeven in de tabel hieronder de installatiekopie.

    ![De functie Azure Cosmos DB geactiveerd maken](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-settings.png)
    
    | Instelling      | Voorgestelde waarde  | Beschrijving                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Een naam voor de functie opgeven** | Standaard | Gebruik de standaardnaam van de functie voorgesteld door de sjabloon. |
    | **Databasenaam** | Taken | Naam van de database met de verzameling moeten worden bewaakt. |
    | **Naam van verzameling** | Items | Naam van de verzameling moeten worden bewaakt. |
    | **Verzameling van de lease maken als deze niet bestaat** | Geselecteerd | De verzameling bestaat nog niet, dus moet u deze maken. |

4. Selecteer **nieuw** naast de **Azure DB die Cosmos-account verbinding** label en kies een bestaande database van de Cosmos-account of **+ nieuw**. 
 
    ![Azure DB die Cosmos-verbinding configureren](./media/functions-create-cosmos-db-triggered-function/functions-create-CosmosDB.png)

6. Wanneer u een nieuwe database van de Cosmos-account maakt, gebruikt u de **nieuwe account** instellingen zoals opgegeven in de tabel.

    | Instelling      | Voorgestelde waarde  | Beschrijving                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **ID** | Naam van de database | Unieke ID voor de Azure DB die Cosmos-database  |
    | **API** | SQL (DocumentDB) | In dit onderwerp maakt gebruik van de documentdatabase API.  |
    | **Abonnement** | Azure-abonnement | Azure-abonnement  |
    | **Resourcegroep** | myResourceGroup |  Gebruik de bestaande resourcegroep die uw functie-app bevat. |
    | **Locatie**  | West-Europa | Selecteer een locatie die zich in de buurt van uw functie-app bevindt of van andere apps die gebruikmaken van de opgeslagen documenten.  |

6. Klik op **OK** om de database te maken. Het maken van de database kan een paar minuten duren. Nadat de database is gemaakt, wordt de verbindingsreeks voor de database opgeslagen als een functie-app-instelling. De naam van deze appinstelling wordt ingevoegd in **Azure DB die Cosmos-account verbinding**. 

7. Klik op **maken** geactiveerd voor het maken van uw Azure-Cosmos-DB functie. Nadat de functie is gemaakt, wordt de functiecode op basis van sjabloon weergegeven.  

    ![Cosmos DB functie sjabloon in C#](./media/functions-create-cosmos-db-triggered-function/function-cosmosdb-template.png)

    Deze functie-sjabloon schrijft het aantal documenten en de eerste document-ID in de logboeken. 

Vervolgens verbinding met uw Azure DB die Cosmos-account en maakt de **taken** verzameling in de database. 

## <a name="create-the-items-collection"></a>De verzameling Items maken

1. Open een tweede exemplaar van de [Azure-portal](https://portal.azure.com) in een nieuw tabblad in de browser. 

2. Vouw de pictogrammenbalk aan de linkerkant van de portal type `cosmos` in het zoekveld en selecteer **Azure Cosmos DB**.

    ![Zoeken naar de service Azure Cosmos-DB](./media/functions-create-cosmos-db-triggered-function/functions-search-cosmos-db.png)

2. Kies uw Azure DB die Cosmos-account en selecteer vervolgens de **Data Explorer**. 
 
3. In **verzamelingen**, kies **taskDatabase** en selecteer **nieuwe verzameling**.

    ![Een verzameling maken](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection.png)

4. In **verzameling toevoegen**, gebruikt u de instellingen die worden weergegeven in de tabel hieronder de installatiekopie. 
 
    ![De taskCollection definiëren](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection2.png)
 
    | Instelling|Voorgestelde waarde|Beschrijving |
    | ---|---|--- |
    | **Database-ID** | Taken |De naam voor de nieuwe database. Dit moet overeenkomen met de naam zijn gedefinieerd in uw functiebinding. |
    | **De verzamelings-ID** | Items | De naam voor de nieuwe verzameling. Dit moet overeenkomen met de naam zijn gedefinieerd in uw functiebinding.  |
    | **Opslagcapaciteit** | Vast (10 GB)|Gebruik de standaardwaarde. Deze waarde is de opslagcapaciteit van de database. |
    | **Doorvoer** |400 RU| Gebruik de standaardwaarde. U kunt de doorvoer later opschalen als u de latentie wilt beperken. |
    | **[Partitiesleutel](../cosmos-db/partition-data.md#design-for-partitioning)** | /category|Een partitiesleutel waarmee gegevens gelijkmatig worden gedistribueerd naar elke partitie. Het is belangrijk dat u de juiste partitiesleutel selecteert bij het maken van een prestatieverzameling. | 

1. Klik op **OK** maken de **taken** verzameling. Het duurt even voor de verzameling te worden gemaakt.

Nadat de verzameling die is opgegeven in de functiebinding bestaat, kunt u de functie testen door documenten toe te voegen aan deze nieuwe verzameling.

## <a name="test-the-function"></a>De functie testen

1. Vouw de nieuwe **taskCollection** verzameling in Data Explorer kiezen **documenten**, selecteer daarna **Nieuw Document**.

    ![Maak een document in taskCollection](./media/functions-create-cosmos-db-triggered-function/create-document-in-collection.png)

2. De inhoud van het nieuwe document vervangen door de volgende inhoud en kies vervolgens **opslaan**.

        {
            "id": "task1",
            "category": "general",
            "description": "some task"
        }

1. Overschakelen naar het eerste tabblad in de browser die de functie in de portal bevat. Vouw de functie Logboeken en controleer of dat het nieuwe document de functie is geactiveerd. Ziet u dat de `task1` document-ID-waarde wordt geschreven naar de logboeken. 

    ![Bekijk het bericht in de logboeken.](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-view-logs.png)

4. (Optioneel) Ga terug naar uw document een wijziging aanbrengt en klik op **Update**. Vervolgens gaat u terug naar de functie Logboeken en controleer of de update is ook geactiveerd voor de functie.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt een functie die wordt uitgevoerd wanneer een document wordt toegevoegd of gewijzigd in uw Azure-Cosmos-database hebt gemaakt.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md) (Opslagwachtrijbindingen van Azure Functions) voor meer informatie over activeringen van Queue Storage.
