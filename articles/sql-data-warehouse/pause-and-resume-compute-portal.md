---
title: 'Snelstartgids: Onderbreken en hervatten compute in Azure SQL Data Warehouse - Azure portal | Microsoft Docs'
description: Gebruik de Azure portal om te onderbreken compute in Azure SQL Data Warehouse om kosten te besparen. Compute hervat wanneer u klaar bent voor het gebruik van het datawarehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: a82d3cfcbc4481c970f5a31ad2de711fb1562657
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43246584"
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Snelstartgids: Onderbreken en hervatten berekenen voor een Azure SQL Data Warehouse in Azure portal
Gebruik de Azure portal om te onderbreken compute in Azure SQL Data Warehouse om kosten te besparen. [Compute hervatten](sql-data-warehouse-manage-compute-overview.md) wanneer u bent klaar voor gebruik van het datawarehouse.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Voordat u begint

Gebruik [maken en verbinden - portal](create-data-warehouse-portal.md) te maken van een datawarehouse met de naam **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>De rekencapaciteit onderbreken
Om kosten te besparen, kunt u onderbreken en hervatten van de compute-resources op de aanvraag. Bijvoorbeeld, als u niet de database's nachts en tijdens het weekend gebruikt, kunt u tijdens deze perioden onderbreken, en hervatten gedurende de dag. U wordt niet in rekening gebracht voor rekenresources terwijl de database is onderbroken. U blijft echter in rekening gebracht voor opslag. 

Volg deze stappen als u wilt onderbreken van een SQL datawarehouse.

1. Klik op de linkerpagina in Azure Portal op **SQL-databases**.
2. Selecteer **mySampleDataWarehouse** op de pagina **SQL-databases**. De datawarehouse wordt geopend. 
3. Op de **mySampleDataWarehouse** pagina, ziet u **Status** is **Online**.

    ![COMPUTE, online](media/pause-and-resume-compute-portal/compute-online.png)

4. Als u wilt onderbreken van het datawarehouse, klikt u op de **onderbreken** knop. 
5. Een vraag bevestiging gevraagd of u wilt doorgaan. Klik op **Ja**.
6. Wacht even en klikt u vervolgens ziet u de **Status** is **onderbreken**.

    ![Onderbreken](media/pause-and-resume-compute-portal/pausing.png)

7. Wanneer de onderbreken-bewerking voltooid is, wordt de status is **onderbroken** en is het keuzerondje **Start**.
8. De rekenresources voor het datawarehouse zijn nu offline. U wordt niet in rekening gebracht voor rekenkracht totdat u de service hervat.

    ![COMPUTE offline](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>De rekencapaciteit hervatten
Volg deze stappen voor het hervatten van een SQL datawarehouse.

1. Klik op de linkerpagina in Azure Portal op **SQL-databases**.
2. Selecteer **mySampleDataWarehouse** op de pagina **SQL-databases**. De datawarehouse wordt geopend. 
3. Op de **mySampleDataWarehouse** pagina, ziet u **Status** is **onderbroken**.

    ![COMPUTE offline](media/pause-and-resume-compute-portal/compute-offline.png)

4. Als u wilt doorgaan met het datawarehouse, klikt u op **Start**. 
5. Een vraag bevestiging gevraagd of u wilt starten. Klik op **Ja**.
6. U ziet dat de **Status** is **hervatten**.

    ![Hervatten](media/pause-and-resume-compute-portal/resuming.png)

7. Wanneer het datawarehouse weer online is, wordt de status is **Online** en is het keuzerondje **onderbreken**.
8. Nu de rekenresources voor het datawarehouse online zijn en kunt u de service. Kosten voor compute is hervat.

    ![COMPUTE, online](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Resources opschonen

In rekening worden gebracht voor datawarehouse-eenheden en de gegevens die zijn opgeslagen in uw datawarehouse. Deze compute- en opslagresources worden apart in rekening gebracht. 

- Als u dat de gegevens in storage wilt, onderbreken.
- Als u in de toekomst geen kosten meer wilt hebben, kunt u de datawarehouse verwijderen. 

Volg deze stappen om de resources op te schonen zoals gewenst.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en klikt u op uw datawarehouse.

    ![Resources opschonen](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Als u het berekenen wilt onderbreken, klikt u op de knop **Onderbreken**. Als het datawarehouse is onderbroken, ziet u een knop **Start**.  Als u het berekenen wilt hervatten, klikt u op **Start**.

2. Als u de datawarehouse wilt verwijderen zodat er geen kosten in rekening worden gebracht voor berekenen of opslaan, klikt u op **Verwijderen**.

3. Als u wilt verwijderen van de SQL-server die u hebt gemaakt, klikt u op **mynewserver-20171113.database.windows.net**, en klik vervolgens op **verwijderen**.  Wees voorzichtig met verwijderen. Als u de server verwijdert, worden ook alle databases verwijderd die zijn toegewezen aan de server.

4. Als u de resourcegroep wilt verwijderen, klikt u op **myResourceGroup**. Klik vervolgens op **Resourcegroep verwijderen**.


## <a name="next-steps"></a>Volgende stappen
U hebt nu onderbroken en hervat de rekenkracht voor uw datawarehouse. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.

> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
