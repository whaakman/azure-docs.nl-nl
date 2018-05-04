---
title: 'Snelstart: rekenkracht uitschalen in Azure SQL Data Warehouse - Azure Portal | Microsoft Docs'
description: Rekenkracht uitschalen in Azure SQL Data Warehouse in Azure Portal. Schaal de rekenkracht uit voor betere prestaties of verklein de schaal om kosten te besparen.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.component: implement
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: d93ea939a938fa88615161d3d048ee1881dd319a
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Snelstart: rekenkracht uitschalen in Azure SQL Data Warehouse in Azure Portal

Rekenkracht uitschalen in Azure SQL Data Warehouse in Azure Portal. [Schaal rekenkracht uit](sql-data-warehouse-manage-compute-overview.md) voor betere prestaties of verklein de schaal om kosten te besparen. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Voordat u begint

U kunt een eigen datawarehouse schalen. U kunt ook [Snelstart: maken en verbinden - portal](create-data-warehouse-portal.md) gebruiken om een datawarehouse met de naam **mySampleDataWarehouse** te maken.  Met deze snelstart wordt **mySampleDataWarehouse** geschaald.

## <a name="scale-compute"></a>De schaal van Compute aanpassen

In SQL Data Warehouse kunt u het aantal rekenresources verhogen of verlagen door de DWU’s aan te passen. Met behulp van [Maken en verbinden - portal](create-data-warehouse-portal.md) is **mySampleDataWarehouse** gemaakt en vervolgens gestart met 400 DWU’s. In de volgende stappen wordt het aantal DWU’s voor **mySampleDataWarehouse** aangepast.

DWU’s wijzigen:

1. Klik op de linkerpagina in Azure Portal op **SQL-databases**.
2. Selecteer **mySampleDataWarehouse** op de pagina **SQL-databases**. De datawarehouse wordt geopend.
3. Klik op **Schalen**.

    ![Op Schalen klikken](media/quickstart-scale-compute-portal/click-scale.png)

2. Verplaats in het paneel Schalen de schuifregelaar naar links of rechts om de DWU-instelling te wijzigen.

    ![Schuifregelaar verplaatsen](media/quickstart-scale-compute-portal/scale-dwu.png)

3. Klik op **Opslaan**. Er verschijnt een bevestigingsbericht. Klik op **Ja** om te bevestigen of **Nee** om te annuleren.

    ![Op Opslaan klikken](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Volgende stappen
U hebt nu geleerd hoe u de rekenkracht voor uw datawarehouse kunt schalen. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.

> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
