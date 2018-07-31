---
title: SQL Data Warehouse aanbevelingen - concepten | Microsoft Docs
description: Meer informatie over aanbevelingen voor SQL Data Warehouse en hoe ze worden gegenereerd
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/27/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 2cbd691c29039c65b98d8b0191e9e278d2440f09
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39348350"
---
# <a name="sql-data-warehouse-recommendations"></a>Aanbevelingen voor SQL datawarehouse

Dit artikel beschrijft de aanbevelingen die is geleverd door SQL Data Warehouse via Azure Advisor.  

SQL Data Warehouse biedt aanbevelingen om te controleren of uw datawarehouse wordt voortdurend geoptimaliseerd voor prestaties. Aanbevelingen voor datawarehouse is nauw geïntegreerd met [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) zodat u kunt met best practices rechtstreeks binnen de [Azure-portal](https://aka.ms/Azureadvisor). SQL Data Warehouse de huidige status van uw datawarehouse analyseert, Telemetrie en bevat aanbevelingen voor de werkbelasting van uw actieve dagelijks worden verzameld. De aanbeveling scenario's met ondersteunde datawarehouse worden hieronder beschreven, samen met het toepassen van de aanbevolen acties.

Als u feedback over de SQL Data Warehouse Advisor hebt of ondervindt problemen, neem contact op [ sqldwadvisor@service.microsoft.com ](mailto:sqldwadvisor@service.microsoft.com).   

Klik op [hier](https://aka.ms/Azureadvisor) om te controleren op uw aanbevelingen vandaag nog! Deze functie is momenteel van toepassing op datawarehouses alleen Gen2. 

## <a name="data-skew"></a>Gegevensverschil

Gegevensverschil kan leiden tot van knelpunten in verkeer of de resource aanvullende gegevens wanneer uw workload wordt uitgevoerd. De volgende documentatie beschreven show gegevensverschil identificeren en te voorkomen dat dit gebeurt door het selecteren van een optimale distributiesleutel.

- [Identificeren en scheeftrekken verwijderen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Geen verouderde statistieken of

Uit suboptimale statistieken kan grote invloed is op prestaties van query's, dit leiden de SQL Data Warehouse-queryoptimalisatieprogramma voor het genereren van suboptimale queryplannen tot kan. De volgende documentatie worden de aanbevolen procedures behandeld voor het maken en bijwerken van statistieken beschreven:

- [Het maken en bijwerken van tabelstatistieken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistic)

Deze twee aanbevelingen voor de advisor wordt continu wordt uitgevoerd de volgende [T-SQL-script](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables) voor het identificeren van tabellen die worden beïnvloed door aanbevelingen voor scheeftrekken en statistieken.
