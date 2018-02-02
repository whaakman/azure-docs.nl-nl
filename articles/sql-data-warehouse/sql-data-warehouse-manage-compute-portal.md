---
title: Beheren van de rekencapaciteit in Azure SQL Data Warehouse (Azure-portal) | Microsoft Docs
description: Azure portal taken voor het beheren van rekencapaciteit. Het aantal rekenresources door dwu's aan te passen. Of onderbreken en hervatten rekenresources kosten besparen.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: f56e62576cae0c594f26bcddf44528032bd5ea69
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Beheren van de rekencapaciteit in Azure SQL Data Warehouse (Azure-portal)
Het aantal rekenresources in Azure SQL Data Warehouse met behulp van de Azure-portal.

## <a name="scale-compute-power"></a>De rekencapaciteit schaal
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Rekenresources wijzigen:

1. Open de [Azure-portal][Azure portal], opent u de database en klik op **Scale**.

    ![Klik op schaal][1]
2. In de blade Scale Verplaats de schuifregelaar naar links of rechts om de DWU-instelling te wijzigen.

    ![Schuifregelaar][2]
3. Klik op **Opslaan**. Er verschijnt een bevestigingsbericht. Klik op **Ja** om te bevestigen of **geen** om te annuleren.

    ![Op Opslaan klikken][3]


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie [Beheeroverzicht][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png


<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
