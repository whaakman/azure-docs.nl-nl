---
title: 'Snelstartgids: Compute in Azure SQL Data Warehouse - Azure-portal uitbreiden | Microsoft Docs'
description: Azure portal taken voor het beheren van rekencapaciteit. Het aantal rekenresources door dwu's aan te passen. Of onderbreken en hervatten rekenresources kosten besparen.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/31/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 6b86042ed6b95ba49fa2089ba36b1dbe9a61cc40
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Snelstartgids: Scale compute in Azure SQL Data Warehouse in de Azure portal

Scale compute in Azure SQL Data Warehouse in de Azure portal. [Uitschalen compute](sql-data-warehouse-manage-compute-overview.md) voor betere prestaties of schaal back-berekeningen voor het opslaan van kosten. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Voordat u begint

U kunt een datawarehouse die u al hebt of gebruik schalen [Quick Start: maken en koppelen - portal](create-data-warehouse-portal.md) voor het maken van een datawarehouse met de naam **mySampleDataWarehouse**.  Deze snelstartgids wordt geschaald **mySampleDataWarehouse**.

## <a name="scale-compute"></a>De schaal van Compute aanpassen

In SQL Data Warehouse, kunt u vergroten of verkleinen van rekenresources door datawarehouse units aan te passen. De [maken en Connect - portal](create-data-warehouse-portal.md) gemaakt **mySampleDataWarehouse** en met 400 dwu's wordt geïnitialiseerd. De volgende stappen aanpassen de dwu's voor **mySampleDataWarehouse**.

Datawarehouse units wijzigen:

1. Klik op **SQL-databases** op de pagina naar links van de Azure portal.
2. Selecteer **mySampleDataWarehouse** van de **SQL-databases** pagina. Hiermee opent u het datawarehouse.
3. Klik op **Scale**.

    ![Klik op schaal](media/quickstart-scale-compute-portal/click-scale.png)

2. In het deelvenster Scale Verplaats de schuifregelaar naar links of rechts om de DWU-instelling te wijzigen.

    ![Schuifregelaar](media/quickstart-scale-compute-portal/scale-dwu.png)

3. Klik op **Opslaan**. Er verschijnt een bevestigingsbericht. Klik op **Ja** om te bevestigen of **geen** om te annuleren.

    ![Op Opslaan klikken](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Volgende stappen
U hebt nu geleerd hoe schalen compute voor uw datawarehouse. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.

> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
