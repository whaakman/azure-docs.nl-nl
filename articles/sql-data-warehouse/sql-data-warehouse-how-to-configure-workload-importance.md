---
title: Urgentie van de werkbelasting configureren in Azure SQL Data Warehouse | Microsoft Docs
description: Informatie over het instellen van de aanvraag niveau belang.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: workload-management
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: e4d410f32068b4d3035dcab0c61b7b9205103690
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67588685"
---
# <a name="configure-workload-importance-in-azure-sql-data-warehouse"></a>Urgentie van de werkbelasting configureren in Azure SQL Data Warehouse

Urgentie instellen in de SQL Data Warehouse, kunt u van invloed zijn op de planning van query's. Query's met hogere prioriteit wordt gepland om uit te voeren voordat u query's met lagere prioriteit. Als u wilt toewijzen belang op query's, die u wilt maken van een classificatie van de werkbelasting.

## <a name="create-a-workload-classifier-with-importance"></a>Maken van een classificatie van de werkbelasting met urgentie

In een datawarehouse-scenario hebt u vaak gebruikers hoeven hun query's snel worden uitgevoerd.  De gebruiker kan zijn managers van het bedrijf die willen uitvoeren van rapporten of de gebruiker mogelijk een analist een ad-hoc-query uit te voeren. U maakt een classificatie van de werkbelasting om toe te wijzen belang aan een query.  De volgende voorbeelden gebruiken de nieuwe [werkbelasting classificatie maken](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) syntaxis voor het maken van twee classificaties.  Membername kan zijn voor één gebruiker of groep. Afzonderlijke gebruiker classificaties hebben voorrang op de rol van classificaties. Bestaande datawarehouse om gebruikers te zoeken, voert u de volgende uit:

```sql
Select name from sys.sysusers
```

Voer voor het maken een classificatie van de werkbelasting voor een gebruiker met hogere prioriteit:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
                   ,MEMBERNAME        = 'name'  
                   ,IMPORTANCE        =  above_normal);  

```

Maken van een classificatie van de werkbelasting voor een gebruiker die ad-hoc-query's uitvoeren met lagere prioriteit worden uitgevoerd:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
                   ,MEMBERNAME        = 'name'  
                   ,IMPORTANCE        =  below_normal);  
```

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over het beheer van de werkbelasting [werkbelasting classificatie](sql-data-warehouse-workload-classification.md)
- Zie voor meer informatie over belang [werkbelasting urgentie](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [Ga naar beheren en bewaken van de werkbelasting belang](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
