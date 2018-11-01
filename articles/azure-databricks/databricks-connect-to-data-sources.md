---
title: 'Verbinding maken met verschillende gegevensbronnen vanuit Azure Databricks '
description: Leer hoe u verbinding maken met verschillende gegevensbronnen vanuit Azure Databricks.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: mamccrea
ms.openlocfilehash: 8055b5d7e6c53abc385a99d9190a38603ebb968b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50417443"
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>Verbinding maken met gegevensbronnen van Azure Databricks

In dit artikel bevat koppelingen naar alle andere gegevensbronnen in Azure die kunnen worden verbonden met Azure Databricks. Ga als volgt in de voorbeelden in deze koppelingen naar gegevens ophalen uit de Azure-gegevensbronnen (bijvoorbeeld Azure Blob Storage, Azure Event Hubs, enzovoort) in een Azure Databricks-cluster en voert u analytische taken op deze. 

## <a name="prerequisites"></a>Vereisten

* Er moet een Azure Databricks-werkruimte en een Spark-cluster. Volg de instructies op [aan de slag met Azure Databricks](quickstart-create-databricks-workspace-portal.md).

## <a name="data-sources-for-azure-databricks"></a>Gegevensbronnen voor Azure Databricks

De volgende lijst bevat de gegevensbronnen in Azure die u met Azure Databricks gebruiken kunt. Zie voor een volledige lijst van gegevensbronnen die kunnen worden gebruikt met Azure Databricks, [gegevensbronnen voor Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html).

- [Azure SQL-database](https://docs.azuredatabricks.net/spark/latest/data-sources/sql-databases.html)

    Deze koppeling bevat de DataFrame-API voor het verbinden met SQL-databases met behulp van JDBC en het beheer van de parallelle uitvoering van leesbewerkingen via de JDBC-interface. In dit onderwerp bevat gedetailleerde voorbeelden van het gebruik van de Scala-API met verkorte Python en Spark SQL-voorbeelden aan het einde.
- [Azure Data Lake Store](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html)

    Deze koppeling bevat voorbeelden over het gebruik van de service-principal voor Azure Active Directory om te verifiëren met Data Lake Store. Ook vindt u instructies over hoe u toegang tot de gegevens in Data Lake Store vanuit Azure Databricks.

- [Azure Blob Storage](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html)

    Deze koppeling bevat voorbeelden over het rechtstreeks toegang tot Azure Blob Storage vanuit Azure Databricks met behulp van de toegangssleutel of de SAS voor een bepaalde container. De koppeling biedt ook informatie over toegang krijgen tot de Azure Blob-opslag vanuit Azure Databricks met behulp van de RDD-API.

- [Azure Cosmos DB](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

    Deze koppeling vindt u instructies voor het gebruik van de [Azure Cosmos DB Spark-connector](https://github.com/Azure/azure-cosmosdb-spark) vanuit Azure Databricks voor toegang tot gegevens in Azure Cosmos DB.

- [Azure Event Hubs](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/eventhubs-connector.html)

    Deze koppeling vindt u instructies voor het gebruik van de [Spark voor Azure Event Hubs-connector](https://github.com/Azure/azure-event-hubs-spark) vanuit Azure Databricks voor toegang tot gegevens in Azure Event Hubs.

- [Azure SQL Data Warehouse](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/sql-data-warehouse.html)

    Deze koppeling vindt u instructies over het gebruik van de Azure SQL Data Warehouse-connector verbinding maken vanuit Azure Databricks.
    

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over bronnen van waar u gegevens in Azure Databricks importeren kunt, [gegevensbronnen voor Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#).


