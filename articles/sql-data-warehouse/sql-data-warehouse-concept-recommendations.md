---
title: SQL Data Warehouse aanbevelingen - concepten | Microsoft Docs
description: Meer informatie over aanbevelingen voor SQL Data Warehouse en hoe ze worden gegenereerd
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/27/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 48d64873f0a8c3754ac5c3ecda2294c0f337b9d5
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094601"
---
# <a name="sql-data-warehouse-recommendations"></a>Aanbevelingen voor SQL datawarehouse

Dit artikel beschrijft de aanbevelingen die is geleverd door SQL Data Warehouse via Azure Advisor.  

SQL Data Warehouse biedt aanbevelingen om te controleren of uw datawarehouse wordt voortdurend geoptimaliseerd voor prestaties. Datawarehouse aanbevelingen zijn nauw geïntegreerd met [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) zodat u kunt met best practices rechtstreeks binnen de [Azure-portal](https://aka.ms/Azureadvisor). SQL Data Warehouse de huidige status van uw datawarehouse analyseert, Telemetrie en bevat aanbevelingen voor de werkbelasting van uw actieve dagelijks worden verzameld. De aanbeveling scenario's met ondersteunde datawarehouse worden hieronder beschreven, samen met het toepassen van de aanbevolen acties.

Als u feedback over de SQL Data Warehouse Advisor hebt of ondervindt problemen, contact op met [ sqldwadvisor@service.microsoft.com ](mailto:sqldwadvisor@service.microsoft.com).   

Klik op [hier](https://aka.ms/Azureadvisor) om te controleren op uw aanbevelingen vandaag nog! Deze functie is momenteel van toepassing op datawarehouses alleen Gen2. 

## <a name="data-skew"></a>Gegevensverschil

Gegevensverschil kan leiden tot van knelpunten in verkeer of de resource aanvullende gegevens wanneer uw workload wordt uitgevoerd. De volgende documentatie beschreven show gegevensverschil identificeren en te voorkomen dat dit gebeurt door het selecteren van een optimale distributiesleutel.

- [Identificeren en scheeftrekken verwijderen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Geen verouderde statistieken of

Uit suboptimale statistieken kan grote invloed is op prestaties van query's, dit leiden de SQL Data Warehouse-queryoptimalisatieprogramma voor het genereren van suboptimale queryplannen tot kan. De volgende documentatie worden de aanbevolen procedures behandeld voor het maken en bijwerken van statistieken beschreven:

- [Het maken en bijwerken van tabelstatistieken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistic)

Als u wilt de lijst met betrokken tabellen zien door deze aanbevelingen, voer de volgende [T-SQL-script](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables). De advisor wordt continu uitgevoerd voor de dezelfde T-SQL-script voor het genereren van deze aanbevelingen.
