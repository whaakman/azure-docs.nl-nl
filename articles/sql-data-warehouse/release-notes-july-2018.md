---
title: Azure SQL Data Warehouse opmerkingen bij de Release van juni 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/23/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 5f4d39c6aa1a5c2c30e84fbf26535fe3ee7799a6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216494"
---
# <a name="whats-new-in-azure-sql-data-warehouse-july-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? Juli 2018
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in juli 2018.


## <a name="finer-granularity-for-cross-region-and-server-restores"></a>Hoe fijner granulariteit voor meerdere regio's en -server herstellen
U kunt nu herstellen in regio's en servers met behulp van een herstelpunt in plaats van het selecteren van geografisch redundante back-ups die worden uitgevoerd om de 24 uur. Cross-regio en server terugzetten worden ondersteund voor beide gebruiker gedefinieerde of Automatische herstelpunten fijner granulariteit voor de beveiliging van aanvullende gegevens inschakelen. Met meer beschikbare herstelpunten, u kunt er zeker van zijn dat uw datawarehouse worden logisch consistent is bij het herstellen van verschillende regio's.

![Terugzetten opdracht - Azure SQL datawarehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6ac23972-9ec0-4502-ab10-7b6bc1a3d947.png)
![opties voor terugzetten voor - Azure SQL datawarehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6c63bd0e-9c52-414d-b4be-d3bd3774ee08.png)

Zie voor meer informatie de [Accelerated en flexibele herstelpunten](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) blogbericht.

## <a name="20-minute-restorations"></a>Herstelprocedures van 20 minuten
SQL Data Warehouse biedt nu het herstellen van een datawarehouse is **minder dan 20 minuten** binnen dezelfde regio, ongeacht de grootte van de database. De hersteltijd is van toepassing ongeacht of de herstelbewerking naar dezelfde of een andere logische server binnen dezelfde regio. Het proces is bovendien verbeterd om te verminderen de hoeveelheid tijd die nodig is om een herstelpunt te maken. In kleine prestatieniveaus (lagere DWU-instellingen), de gebruikerservaring is een *2 x vermindering* in de tijd voor het maken van de momentopname.

Zie voor meer informatie de [Accelerated en flexibele herstelpunten](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) blogbericht.

## <a name="custom-restoration-configurations"></a>Aangepaste herstel-configuraties
U kunt nu het prestatieniveau (DWU) wijzigen bij het herstellen in Azure portal. U kunt ook herstellen naar een bijgewerkte Gen2 datawarehouse. Door terug te zetten naar een Gen 2-exemplaar, kunt u nu de impact van Gen2 voordat evalueren [upgraden van uw datawarehouse Gen1](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation).

![Aangepaste herstel-configuratie - Azure SQL datawarehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/f4c410c7-8515-409c-a983-0976792b8628.png)

## <a name="spdescribeundeclaredparameters"></a>SP_DESCRIBE_UNDECLARED_PARAMETERS
De [sp_describe_undeclared_parameters](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql) opgeslagen procedure wordt vaak gebruikt door hulpprogramma's voor het ophalen van metagegevens over parameters in de Transact-SQL-batch. De procedure retourneert één rij voor elke parameter in de batch met de afgeleid type-informatie voor die parameter. 

```sql
EXEC sp_describe_undeclared_parameters
    @tsql = 
    N'SELECT
        object_id, name, type_desc
      FROM
        sys.indexes
      WHERE
        object_id = @id;'
```

De resultatenset bevat metagegevens over de `@id` parameter (gedeeltelijke resultaten die worden weergegeven)
```
parameter_ordinal | name | suggested_system_type_id | suggested_system_type_name
--------------------------------------------------------------------------------
1                 | @id  | 56                       | int
```