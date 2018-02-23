---
title: 'Snelstartgids: Onderbreken en hervatten compute in Azure SQL Data Warehouse - Azure-portal | Microsoft Docs'
description: Azure portal taken die onderbreken berekenen voor een Azure SQL Data Warehouse om kosten te besparen. Berekeningen hervatten wanneer u klaar bent voor gebruik van het datawarehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/23/2018
ms.author: barbkess
ms.openlocfilehash: 30dede32b35f995f89e2946af34da10353f55212
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Snelstartgids: Onderbreken en hervatten berekenen voor een Azure SQL Data Warehouse in de Azure portal
De rekencapaciteit onderbreken voor een Azure SQL Data Warehouse om kosten te besparen. [Compute hervatten](sql-data-warehouse-manage-compute-overview.md) wanneer u bent klaar voor gebruik van het datawarehouse.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Voordat u begint

Gebruik [maken en Connect - portal](create-data-warehouse-portal.md) voor het maken van een datawarehouse aangeroepen **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>De rekencapaciteit onderbreken
Als u wilt opslaan kosten, kunt u onderbreken en hervatten van compute bronnen op de aanvraag. Bijvoorbeeld als u niet de database's nachts en in het weekend gebruikt, kunt u tijdens de tijdstippen onderbreken en hervatten gedurende de dag. U geen afgeschreven rekenresources terwijl de database is onderbroken. U blijft echter in rekening gebracht voor opslag. 

Volg deze stappen voor het onderbreken van een SQL datawarehouse.

1. Klik op **SQL-databases** op de pagina naar links van de Azure portal.
2. Selecteer **mySampleDataWarehouse** van de **SQL-databases** pagina. Hiermee opent u het datawarehouse. 
3. Op de **mySampleDataWarehouse** pagina, zoals u ziet, **Status** is **Online**.

    ![COMPUTE online](media/pause-and-resume-compute-portal/compute-online.png)

4. Het datawarehouse onderbreken, klikt u op de **onderbreken** knop. 
5. Een vraag bevestiging gevraagd of u wilt doorgaan. Klik op **Ja**.
6. Wacht even en noteert u de **Status** is **onderbreken**.

    ![Onderbreken](media/pause-and-resume-compute-portal/pausing.png)

7. Wanneer de onderbreking voltooid is, wordt de status is **onderbroken** en het keuzerondje is **Start**.
8. De rekenresources voor het datawarehouse zijn nu offline. U geen afgeschreven voor compute totdat u de service hervat.

    ![COMPUTE offline](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Compute hervatten
Volg deze stappen voor het hervatten van een SQL datawarehouse.

1. Klik op **SQL-databases** op de pagina naar links van de Azure portal.
2. Selecteer **mySampleDataWarehouse** van de **SQL-databases** pagina. Hiermee opent u het datawarehouse. 
3. Op de **mySampleDataWarehouse** pagina, zoals u ziet, **Status** is **onderbroken**.

    ![COMPUTE offline](media/pause-and-resume-compute-portal/compute-offline.png)

4. Als u wilt doorgaan met het datawarehouse, klikt u op **Start**. 
5. Een vraag bevestiging gevraagd of u wilt starten. Klik op **Ja**.
6. U ziet de **Status** is **hervat**.

    ![Hervatten](media/pause-and-resume-compute-portal/resuming.png)

7. Wanneer het datawarehouse weer online is, wordt de status is **Online** en het keuzerondje is **onderbreken**.
8. Nu de rekenresources voor het datawarehouse online zijn en u kunt de service. Kosten voor de berekening is hervat.

    ![COMPUTE online](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Resources opschonen

Er zijn wordt in rekening gebracht voor datawarehouse eenheden en de gegevens die zijn opgeslagen in uw datawarehouse. Deze compute- en opslagresources worden apart in rekening gebracht. 

- Als u dat de gegevens in de opslag wilt, onderbreken compute.
- Als u in de toekomst geen kosten meer wilt hebben, kunt u de datawarehouse verwijderen. 

Volg deze stappen om de resources op te schonen zoals gewenst.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en klik op het datawarehouse.

    ![Resources opschonen](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Als u het berekenen wilt onderbreken, klikt u op de knop **Onderbreken**. Als het datawarehouse is onderbroken, ziet u een knop **Start**.  Als u het berekenen wilt hervatten, klikt u op **Start**.

2. Als u de datawarehouse wilt verwijderen zodat er geen kosten in rekening worden gebracht voor berekenen of opslaan, klikt u op **Verwijderen**.

3. Als u wilt verwijderen van de SQL-server die u hebt gemaakt, klikt u op **mynewserver 20171113.database.windows.net**, en klik vervolgens op **verwijderen**.  Wees voorzichtig met verwijderen. Als u de server verwijdert, worden ook alle databases verwijderd die zijn toegewezen aan de server.

4. Als u de resourcegroep wilt verwijderen, klikt u op **myResourceGroup**. Klik vervolgens op **Resourcegroep verwijderen**.


## <a name="next-steps"></a>Volgende stappen
U hebt nu onderbroken en hervat compute voor uw datawarehouse. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.

> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
