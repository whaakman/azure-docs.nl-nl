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
ms.openlocfilehash: 63888d5dd103b585cf18e4787d3e779810163e3d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Beheren van de rekencapaciteit in Azure SQL Data Warehouse (Azure-portal)
> [!div class="op_single_selector"]
> * [Overzicht](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a>De rekencapaciteit schaal
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Rekenresources wijzigen:

1. Open de [Azure-portal][Azure portal], opent u de database en klik op **Scale**.

    ![Klik op schaal][1]
2. In de blade Scale Verplaats de schuifregelaar naar links of rechts om de DWU-instelling te wijzigen.

    ![Schuifregelaar][2]
3. Klik op **Opslaan**. Er verschijnt een bevestigingsbericht. Klik op **Ja** om te bevestigen of **geen** om te annuleren.

    ![Op Opslaan klikken][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>De rekencapaciteit onderbreken
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Een database onderbreken:

1. Open de [Azure-portal] [ Azure portal] en open uw database. U ziet dat de Status is **Online**.

    ![Onlinestatus][6]
2. Als u wilt onderbreken berekenings-en geheugenbronnen, klikt u op **onderbreken**, en vervolgens een bevestigingsbericht weergegeven. Klik op **Ja** om te bevestigen of **geen** om te annuleren.

    ![Onderbreken bevestigen][7]
3. Terwijl de SQL Data Warehouse met het starten van de database, de status is **onderbreken**.
4. Wanneer de status is **onderbroken**, de pauze-bewerking wordt uitgevoerd en u bent niet meer in rekening worden gebracht dwu's.

    ![Status onderbreken][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Compute hervatten
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Een database hervatten:

1. Open de [Azure-portal] [ Azure portal] en open uw database. U ziet dat de Status is **onderbroken**.

    ![Database onderbreken][4]
2. Het database klikt u op hervatten **Start**, en vervolgens een bevestigingsbericht weergegeven. Klik op **Ja** om te bevestigen of **geen** om te annuleren.

    ![Bevestig hervatten][5]
3. Terwijl SQL Data Warehouse kan de database wordt gestart, wordt de status 'Hervat'.
4. Wanneer de status is **online**, de database is gereed.

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie [Beheeroverzicht][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
