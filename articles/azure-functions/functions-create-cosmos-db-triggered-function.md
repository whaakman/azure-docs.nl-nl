---
title: Een door Azure Cosmos DB geactiveerde functie maken | Microsoft Docs
description: Gebruik Azure Functions om een functie zonder server te maken die wordt aangeroepen wanneer gegevens worden toegevoegd aan een database in Azure Cosmos DB.
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
ms.date: 12/07/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: ff0c468179ef7b71151b64426bf2e6701d5032fe
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="create-a-function-triggered-by-azure-cosmos-db"></a>Een door Azure Cosmos DB geactiveerde functie maken

Leer hoe u een functie maakt die wordt geactiveerd wanneer gegevens worden toegevoegd of gewijzigd in Azure Cosmos DB. Zie [Azure Cosmos DB: Serverless database computing using Azure Functions](..\cosmos-db\serverless-computing-database.md) (Azure Cosmos DB: database berekenen zonder server met Azure Functions) voor meer informatie over Azure Cosmos DB.

![Bekijk het bericht in de logboeken.](./media/functions-create-cosmos-db-triggered-function/quickstart-completed.png)

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

+ Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Een Azure-functie-app maken

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Vervolgens maakt u een functie in de nieuwe functie-app.

<a name="create-function"></a>

## <a name="create-azure-cosmos-db-trigger"></a>Een Azure Cosmos DB-trigger maken

1. Vouw de functie-app uit en klik op de knop **+** naast **Functies**. Als dit de eerste functie in de functie-app is, selecteert u **Aangepaste functie**. U ziet nu de volledige set het functiesjablonen.

    ![De Quick Start-pagina van Functions in Azure Portal](./media/functions-create-cosmos-db-triggered-function/add-first-function.png)

2. Typ `cosmos` in het zoekveld en kies vervolgens de gewenste taal voor de trigger-sjabloon voor de Azure Cosmos DB.

    ![De Azure Cosmos DB-trigger kiezen](./media/functions-create-cosmos-db-triggered-function/select-cosmos-db-trigger-portal.png)

3. Configureer de nieuwe trigger met de instellingen zoals opgegeven in de tabel onder de afbeelding.

    ![Maak de door Azure Cosmos DB geactiveerde functie](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-settings.png)
    
    | Instelling      | Voorgestelde waarde  | Beschrijving                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Naam** | Standaard | Gebruik de standaardnaam voor de functie die wordt voorgesteld in de sjabloon. |
    | **Naam van verzameling** | Items | Naam van de verzameling die moet worden gecontroleerd. |
    | **Leaseverzameling maken als deze nog niet bestaat** | Geselecteerd | De verzameling bestaat nog niet, dus moet u deze maken. |
    | **Databasenaam** | Taken | Naam van de database met de verzameling die moet worden gecontroleerd. |

4. Selecteer **Nieuw** naast het label **De verbinding met het Azure Cosmos DB-account**, en kies een bestaand Cosmos DB-account of kies **+ Nieuwe maken**. 
 
    ![Azure Cosmos DB-verbinding configureren](./media/functions-create-cosmos-db-triggered-function/functions-create-CosmosDB.png)

6. Gebruik de instellingen voor **Nieuw account** zoals opgegeven in de tabel, wanneer u een nieuw Cosmos DB-account maakt.

    | Instelling      | Voorgestelde waarde  | Beschrijving                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **ID** | Naam van de database | Unieke id voor de Azure Cosmos DB-database  |
    | **API** | SQL (DocumentDB) | In dit onderwerp wordt gebruikgemaakt van de documentdatabase-API.  |
    | **Abonnement** | Azure-abonnement | Azure-abonnement  |
    | **Resourcegroep** | myResourceGroup |  Gebruik de bestaande resourcegroep die uw functie-app bevat. |
    | **Locatie**  | West-Europa | Selecteer een locatie die zich in de buurt van uw functie-app bevindt of van andere apps die gebruikmaken van de opgeslagen documenten.  |

6. Klik op **OK** om de database te maken. Het maken van de database kan een paar minuten duren. Nadat de database is gemaakt, wordt de verbindingsreeks voor de database opgeslagen als een functie-app-instelling. De naam van deze app-instelling wordt ingevoegd in **Azure Cosmos DB-accountverbinding**. 

7. Klik op **Maken** om de door Azure Cosmos DB geactiveerde functie te maken. Nadat de functie is gemaakt, wordt de functiecode op basis van een sjabloon weergegeven.  

    ![Cosmos DB-functiesjabloon in C#](./media/functions-create-cosmos-db-triggered-function/function-cosmosdb-template.png)

    Met deze functiesjabloon worden het aantal documenten en de eerste document-id naar de logboeken geschreven. 

Vervolgens maakt u verbinding met het Azure Cosmos DB-account en maakt u de verzameling **Taken** in de database. 

## <a name="create-the-items-collection"></a>De verzameling Items maken

1. Open een tweede exemplaar van [Azure Portal](https://portal.azure.com) op een nieuw tabblad in de browser. 

2. Vouw aan de linkerkant van de portal de pictogrammenbalk uit, typ `cosmos` in het zoekveld en selecteer **Azure Cosmos DB**.

    ![Zoek naar de Azure Cosmos DB-service](./media/functions-create-cosmos-db-triggered-function/functions-search-cosmos-db.png)

2. Kies uw Azure Cosmos DB-account en selecteer vervolgens **Data Explorer**. 
 
3. Kies in **Verzamelingen** de optie **taskDatabase**, en selecteer **Nieuwe verzameling**.

    ![Een verzameling maken](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection.png)

4. Gebruik in **Verzameling toevoegen** de instellingen die worden weergegeven in de tabel onder de afbeelding. 
 
    ![De taskCollection definiëren](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection2.png)
 
    | Instelling|Voorgestelde waarde|Beschrijving |
    | ---|---|--- |
    | **Database-id** | Taken |De naam voor de nieuwe database. Dit moet overeenkomen met de naam die is gedefinieerd in de functiebinding. |
    | **Verzamelings-id** | Items | De naam voor de nieuwe verzameling. Dit moet overeenkomen met de naam die is gedefinieerd in de functiebinding.  |
    | **Opslagcapaciteit** | Vast (10 GB)|Gebruik de standaardwaarde. Deze waarde is de opslagcapaciteit van de database. |
    | **Doorvoer** |400 RU| Gebruik de standaardwaarde. U kunt de doorvoer later opschalen als u de latentie wilt beperken. |
    | **[Partitiesleutel](../cosmos-db/partition-data.md#design-for-partitioning)** | /category|Een partitiesleutel waarmee gegevens gelijkmatig worden gedistribueerd naar elke partitie. Het is belangrijk dat u de juiste partitiesleutel selecteert bij het maken van een prestatieverzameling. | 

1. Klik op **OK** om de verzameling **Taken** te maken. Het kan even duren voor de verzameling is gemaakt.

Nadat de verzameling die is opgegeven in de functiebinding, is gemaakt, kunt u de functie testen door documenten toe te voegen aan deze nieuwe verzameling.

## <a name="test-the-function"></a>De functie testen

1. Vouw de nieuwe verzameling **taskCollection** uit in Data Explorer, kies **Documenten** en selecteer vervolgens **Nieuw document**.

    ![Maak een document in taskCollection](./media/functions-create-cosmos-db-triggered-function/create-document-in-collection.png)

2. Vervang de inhoud van het nieuwe document door de volgende inhoud en kies vervolgens **Opslaan**.

        {
            "id": "task1",
            "category": "general",
            "description": "some task"
        }

1. Schakel over naar het eerste browsertabblad dat de functie in de portal bevat. Vouw de functielogboeken uit en controleer of de functie is geactiveerd met behulp van het nieuwe document. U ziet nu dat de waarde voor de `task1`-document-id naar de logboeken is geschreven. 

    ![Bekijk het bericht in de logboeken.](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-view-logs.png)

4. (Optioneel) Ga terug naar het document, breng een wijziging aan en klik op **Bijwerken**. Ga vervolgens terug naar de functielogboeken en controleer of met de update ook de functie is geactiveerd.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een functie gemaakt die wordt uitgevoerd wanneer een document wordt toegevoegd of gewijzigd in uw Azure Cosmos DB-account.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie [Azure Cosmos DB bindings for Azure Functions](functions-bindings-cosmosdb.md) (Azure Cosmos DB-bindingen voor Azure Functions) voor meer informatie over Azure Cosmos DB-triggers.
