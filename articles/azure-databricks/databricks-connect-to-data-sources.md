---
title: Verbinding maken met verschillende gegevensbronnen van Azure Databricks | Microsoft Docs
description: Informatie over het verbinding maken met verschillende gegevensbronnen van Azure Databricks.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2018
ms.author: nitinme
ms.openlocfilehash: 865313a7c6eabd847529b88ff5fff0b7db438fa5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30174275"
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>Verbinding maken met gegevensbronnen van Azure Databricks

In dit artikel bevat koppelingen naar alle andere gegevensbronnen in Azure die kunnen worden verbonden met Azure Databricks. Volg de voorbeelden in deze koppelingen om gegevens te extraheren uit de Azure gegevensbronnen (bijvoorbeeld Azure Blob Storage, Azure Event Hubs, enzovoort) in een cluster met Azure Databricks en analytische taken wordt uitgevoerd. 

## <a name="prerequisites"></a>Vereisten

* U hebt een Azure-Databricks werkruimte en een Spark-cluster. Volg de instructies voor [aan de slag met Azure Databricks](quickstart-create-databricks-workspace-portal.md).

## <a name="data-sources-for-azure-databricks"></a>Gegevensbronnen voor Azure Databricks

De volgende lijst bevat de gegevensbronnen in Azure die u met Azure Databricks gebruiken kunt. Zie voor een volledige lijst van gegevensbronnen die kunnen worden gebruikt met Azure Databricks [gegevensbronnen voor Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html).

- [Azure SQL-database](https://docs.azuredatabricks.net/spark/latest/data-sources/sql-databases.html)

    Deze koppeling bevat de DataFrame-API voor het verbinden met SQL-databases via JDBC en het beheer van de parallelle uitvoering van leesbewerkingen via de JDBC-interface. Dit onderwerp bevat gedetailleerde voorbeelden van de API Scala gebruiken met verkorte Python en Spark SQL-voorbeelden aan het einde.
- [Azure Data Lake Store](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html)

    Deze koppeling bevat voorbeelden voor het gebruik van de Azure Active Directory-service-principal voor verificatie met Data Lake Store. Het bevat ook instructies voor het openen van de gegevens in Data Lake Store uit Azure Databricks.

- [Azure Blob Storage](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html)

    Deze koppeling biedt voorbeelden voor het rechtstreeks toegang krijgen tot Azure Blob Storage vanuit Azure Databricks met toegangssleutel of de SAS voor een bepaalde container. De koppeling bevat ook informatie over toegang krijgen tot de Azure Blob Storage vanuit Azure Databricks RDD API gebruiken.

- [Azure Cosmos DB](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

    Deze koppeling vindt u instructies voor het gebruik van de [Azure Cosmos DB Spark connector](https://github.com/Azure/azure-cosmosdb-spark) van Azure Databricks voor toegang tot gegevens in Azure Cosmos DB.

- [Azure Event Hubs](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/eventhubs-connector.html)

    Deze koppeling vindt u instructies voor het gebruik van de [Azure Event Hubs Spark connector](https://github.com/Azure/azure-event-hubs-spark) van Azure Databricks voor toegang tot gegevens in Azure Event Hubs.

- [Azure SQL Data Warehouse](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/sql-data-warehouse.html)

    Deze koppeling bevat instructies over het gebruik van de Azure SQL Data Warehouse-connector verbinding maken vanuit Azure Databricks.
    

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over bronnen vanaf waar u gegevens in Azure Databricks importeren kunt, [gegevensbronnen voor Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#).


