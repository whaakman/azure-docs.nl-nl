---
title: Gebruik Azure Cosmos DB explorer om uw gegevens te beheren | Microsoft Docs
description: Azure Cosmos DB explorer is een zelfstandige webconsole interface waarmee u de gegevens die zijn opgeslagen in Azure Cosmos DB weergeven en beheren.
services: cosmos-db
author: deborahc
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: dech
ms.openlocfilehash: 8d1bd0d4331937e37307140e17e5aed1a6e3b0ff
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060177"
---
# <a name="use-azure-cosmos-db-explorer-to-manage-your-data"></a>Azure Cosmos DB explorer gebruiken om uw gegevens te beheren 

Azure Cosmos DB explorer is een zelfstandige webconsole interface waarmee u de gegevens die zijn opgeslagen in Azure Cosmos DB weergeven en beheren. Azure Cosmos DB explorer is gelijk aan de bestaande **Data Explorer** tabblad dat is beschikbaar in Azure portal wanneer u een Azure Cosmos DB-account maakt. De belangrijke voordelen van Azure Cosmos DB explorer ten opzichte van de bestaande Data explorer zijn:

* U hebt een volledig scherm-onroerend goed om uw gegevens, uitvoeren van query's, opgeslagen procedures, triggers weer te geven en de resultaten ervan bekijken.  

* U kunt tijdelijk of permanent lees- of lees-/ schrijftoegang opgeven aan uw databaseaccount en de verzamelingen aan andere gebruikers die geen toegang tot Azure portal of een abonnement hebben.  

* U kunt de resultaten van de query delen met andere gebruikers die geen toegang tot Azure portal of een abonnement hebben.  

## <a name="access-azure-cosmos-db-explorer"></a>Toegang tot Azure Cosmos DB explorer

1. Aanmelden bij [Azure-Portal](https://portal.azure.com/). 

2. Van **alle resources**, zoeken en navigeer naar uw Azure Cosmos DB-account, selecteer sleutels en kopieer de **Primary Connection String**.  

3. Ga naar https://cosmos.azure.com/, plak de verbindingsreeks en selecteert u **Connect**. Met behulp van de verbindingsreeks, kunt u toegang tot de Azure Cosmos DB explorer zonder een tijdslimiet.  

   Als u wilt dat voor andere gebruikers tijdelijk toegang tot uw Azure Cosmos DB-account, kunt u dit doen met behulp van de lezen / schrijven en lezen toegangs-URL's. 

4. Open de **Data Explorer** Selecteer **Open volledig scherm**. In het pop-updialoogvenster, kunt u twee weergeven toegang tot URL's – **lezen / schrijven** en **lezen**. Deze URL's kunnen u uw Azure Cosmos DB-account tijdelijk delen met andere gebruikers. Toegang tot het account verloopt binnen 24 uur, waarmee u verbinding maken kunt met behulp van een nieuw toegangs-URL of de verbindingsreeks. 

   **Lezen / schrijven** – wanneer u de URL voor lezen / schrijven met andere gebruikers delen ze kunnen weergeven en wijzigen van de databases, verzamelingen, query's en andere resources die zijn gekoppeld aan dat specifieke account.

   **Lezen** : wanneer u de URL van de alleen-lezen met andere gebruikers delen, kunnen ze de databases, verzamelingen, query's en andere resources die zijn gekoppeld aan dat specifieke account bekijken. Bijvoorbeeld, als u delen van resultaten van een query met uw collega's die geen toegang hebt tot Azure portal of uw Azure Cosmos DB-account wilt, kunt u opgeven ze met deze URL.

   Kies het type toegang dat u wilt het account met open en klik op **Open**. Nadat u de Verkenner openen, is de ervaring is hetzelfde als u had met het tabblad Data Explorer in Azure portal.   

   ![Open Azure Cosmos DB explorer](./media/data-explorer/open-data-explorer-with-access-url.png)

## <a name="known-issues"></a>Bekende problemen

Op dit moment de **Open volledig scherm** ervaring waarmee u delen van tijdelijke lezen-schrijven of lezen-toegang wordt nog niet ondersteund voor Azure Cosmos DB Gremlin en Table-API-accounts. U kunt nog steeds uw Gremlin en Table-API-accounts weergeven door de verbindingsreeks wordt doorgegeven aan Azure Cosmos DB Explorer. 

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt geleerd hoe u aan de slag met Azure Cosmos DB explorer voor het beheren van uw gegevens, u kunt volgende:

* Beginnen met het definiëren van [query's](sql-api-sql-query-reference.md) met behulp van SQL-syntaxis en uit te voeren [programmeren op de server](programming.md) met behulp van opgeslagen procedures, UDF's, wordt geactiveerd. 