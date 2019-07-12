---
title: Beheren en controleren van de werkbelasting van belang zijn in Azure SQL Data Warehouse | Microsoft Docs
description: Informatie over het beheren en controleren van de aanvraag niveau belang.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: workload-management
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 30afe1805748012b0a137c865c799580f79d31d8
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67588642"
---
# <a name="manage-and-monitor-workload-importance-in-azure-sql-data-warehouse"></a>Beheren en controleren van de werkbelasting van belang zijn in Azure SQL Data Warehouse

Beheren en controleren op belang van de aanvraag in Azure SQL Data Warehouse met behulp van DMV's en catalogusweergaven.

## <a name="monitor-importance"></a>Monitor urgentie

Bewaken met behulp van de nieuwe kolom die belang in belang de [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=azure-sqldw-latest) dynamische Beheerweergave.
De onderstaande bewaking van query's tijd verzenden en de begintijd voor query's. Controleer de tijd van verzenden en begintijd samen met belang om te zien hoe het plannen van belang wordt beïnvloed.

```sql
SELECT s.login_name, r.status, r.importance, r.submit_time, r.start_time
  FROM sys.dm_pdw_exec_sessions s
  JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
  WHERE r.resource_class is not null
ORDER BY r.start_time
```

Om verder te zoeken in het schema voor query's worden, de catalogusweergaven te gebruiken.

## <a name="manage-importance-with-catalog-views"></a>Urgentie met catalogusweergaven beheren

De catalogusweergave sys.workload_management_workload_classifiers bevat informatie over classificaties in uw Azure SQL Data Warehouse-exemplaar. Als u wilt uitsluiten voert u het systeem gedefinieerde classificaties die zijn toegewezen aan de resource-klassen uit de volgende code:

```sql
SELECT *
  FROM sys.workload_management_workload_classifiers
  WHERE classifier_id > 12
```

De catalogusweergave [sys.workload_management_workload_classifier_details](/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql?view=azure-sqldw-latest), bevat informatie over de parameters die worden gebruikt bij het maken van de classificatie.  De onderstaande query laat zien dat ExecReportsClassifier is gemaakt op de ```membername``` parameter waarden met ExecutiveReports:

```sql
SELECT c.name,cd.classifier_type, classifier_value
  FROM sys.workload_management_workload_classifiers c
  JOIN sys.workload_management_workload_classifier_details cd
    ON cd.classifier_id = c.classifier_id
  WHERE c.name = 'ExecReportsClassifier'
```

![Queryresultaten](./media/sql-data-warehouse-how-to-manage-and-monitor-workload-importance/wlm-query-results.png)

Ter vereenvoudiging van het oplossen van problemen misclassification, raden wij dat u resource-klassetoewijzingen rol verwijderen bij het maken van classificaties van de werkbelasting. De onderstaande code retourneert bestaande resource rollidmaatschappen klasse. Voer sp_droprolemember voor elk ```membername``` geretourneerd uit de bijbehorende resourceklasse.
Hieronder volgt een voorbeeld voor bestaan voordat u verwijdert een classificatie van de werkbelasting te controleren:

```sql
IF EXISTS (SELECT 1 FROM sys.workload_management_workload_classifiers WHERE name = 'ExecReportsClassifier')
  DROP WORKLOAD CLASSIFIER ExecReportsClassifier;
GO
```

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over classificatie [werkbelasting classificatie](sql-data-warehouse-workload-classification.md).
- Zie voor meer informatie over belang [werkbelasting urgentie](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [Ga naar het belang van de werkbelasting configureren](sql-data-warehouse-how-to-configure-workload-importance.md)
