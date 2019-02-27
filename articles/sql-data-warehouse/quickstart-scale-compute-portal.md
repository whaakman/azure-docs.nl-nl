---
title: 'Quickstart: De schaal van rekenkracht vergroten in Azure SQL Data Warehouse - Azure portal | Microsoft Docs'
description: Rekenkracht uitschalen in Azure SQL Data Warehouse in Azure Portal. De schaal van rekenkracht vergroten voor betere prestaties of de schaal juist verkleinen om kosten te besparen.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: implement
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 2e3bb00cee679671ed382ee46690fc4dff8bddfb
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56453882"
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Quickstart: Rekenkracht uitschalen in Azure SQL Data Warehouse in de Azure-portal

Rekenkracht uitschalen in Azure SQL Data Warehouse in Azure Portal. [Vergroot de schaal van Compute](sql-data-warehouse-manage-compute-overview.md) voor betere prestaties of verklein de schaal juist om kosten te besparen. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Voordat u begint

U kunt een eigen datawarehouse schalen. U kunt ook [Quickstart: maken en verbinden - portal](create-data-warehouse-portal.md) gebruiken om een datawarehouse met de naam **mySampleDataWarehouse** te maken.  Met deze snelstart wordt **mySampleDataWarehouse** geschaald.

>[!Note]
>Uw datawarehouse moet online zijn om te kunnen worden geschaald. 

## <a name="scale-compute"></a>De schaal van Compute aanpassen

Compute-resources van SQL Data Warehouse kunnen worden geschaald door het aantal warehouse-eenheden te vergroten of verkleinen. Met behulp van de quickstart [maken en verbinden - portal] (create-data-warehouse-portal.md) is **mySampleDataWarehouse** gemaakt en vervolgens gestart met 400 DWU's. In de volgende stappen wordt het aantal DWU's voor **mySampleDataWarehouse** aangepast.

DWU’s wijzigen:

1. Klik op **SQL Data Warehouses** op de linkerpagina in de Azure Portal.
2. Selecteer **mySampleDataWarehouse** op de pagina **SQL Data Warehouses**. De datawarehouse wordt geopend.
3. Klik op **Schalen**.

    ![Op Schalen klikken](media/quickstart-scale-compute-portal/click-scale.png)

2. Verplaats in het paneel Schalen de schuifregelaar naar links of rechts om de DWU-instelling te wijzigen.

    ![Schuifregelaar verplaatsen](media/quickstart-scale-compute-portal/scale-dwu.png)

3. Klik op **Opslaan**. Er verschijnt een bevestigingsbericht. Klik op **Ja** om te bevestigen of **Nee** om te annuleren.

    ![Op Opslaan klikken](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Volgende stappen
U hebt nu geleerd hoe u de rekenkracht voor uw datawarehouse wijzigt. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.

> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
