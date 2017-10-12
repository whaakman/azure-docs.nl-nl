---
title: Een SQL Data Warehouse maken in Azure Portal | Microsoft Docs
description: Leer hoe u een Azure SQL Data Warehouse maakt in Azure Portal
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 552e496e-d560-419c-9996-6bbc80c521cb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 24ed2d8bad3090e378acf2a42fb909dee0a8517b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-sql-data-warehouse"></a>Een Azure SQL Data Warehouse maken
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

In deze zelfstudie gebruikt u Azure Portal om een SQL Data Warehouse te maken die een AdventureWorksDW-voorbeelddatabase bevat.

## <a name="prerequisites"></a>Vereisten
Om aan de slag te gaan, hebt u het volgende nodig:

* **Azure-account**: ga naar [Gratis proefversie van Azure][Azure Free Trial] of [Azure-tegoed met MSDN][MSDN Azure Credits] om een account te maken.
* **Azure SQL Server**: zie [Een Azure SQL Database met Azure Portal maken][Create an Azure SQL database in the Azure portal] voor meer informatie.

> [!NOTE]
> Het maken van een SQL Data Warehouse kan een nieuwe factureerbare service tot gevolg hebben.  Zie [Prijzen van SQL Data Warehouse][SQL Data Warehouse pricing] voor meer informatie.
>
>

## <a name="create-a-sql-data-warehouse"></a>Een SQL Data Warehouse maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **+ Nieuw** > **Databases** > **SQL Data Warehouse**.

    ![Maken](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. Vul op de blade **SQL Data Warehouse** de benodigde informatie in en druk op Maken.

    ![Database maken](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * **Server**: we raden u aan om eerst de server te selecteren.  
   * **Databasenaam**: de naam die wordt gebruikt om naar de SQL Data Warehouse te verwijzen.  De waarde moet uniek zijn op de server.
   * **Prestaties**: we raden u aan om te beginnen met 400 [DWU's][DWU]. U kunt de schuifregelaar naar links of rechts bewegen om de prestaties van uw datawarehouse aan te passen, of u kunt later omhoog of omlaag schalen.  Meer informatie over DWU's vindt u in onze documentatie over [schalen](sql-data-warehouse-manage-compute-overview.md) of op onze [pagina met prijzen][SQL Data Warehouse pricing].
   * **Abonnement**: selecteer het [abonnement] waaronder deze SQL Data Warehouse wordt gefactureerd.
   * **Resourcegroep**: [resourcegroepen][Resource group] zijn containers die speciaal zijn ontworpen om u te helpen bij het beheren van een verzameling Azure-resources. Meer informatie over [resourcegroepen](../azure-resource-manager/resource-group-overview.md).
   * **Bron selecteren**: klik op **Bron selecteren** > **Voorbeeld**. Azure vult de optie **Select sample** (Selecteer voorbeeld) automatisch in met AdventureWorksDW.

   > [!NOTE]
   > De standaardsortering voor een SQL Data Warehouse is SQL_Latin1_General_CP1_CI_AS. Als er een andere sortering is vereist, kan met [T-SQL][T-SQL] een andere sortering voor de database worden gemaakt.
   >
   >

1. Klik op **Maken** om uw SQL Data Warehouse te maken.
2. Wacht enkele minuten. Wanneer uw datawarehouse klaar is, keert u terug naar [Azure Portal](https://portal.azure.com). U vindt de SQL Data Warehouse op het dashboard onder de SQL-databases, of in de resourcegroep die u hebt gebruikt om deze te maken.

    ![portalweergave](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Volgende stappen
Nu u een SQL Data Warehouse hebt gemaakt, kunt u [verbinding maken](sql-data-warehouse-connect-overview.md) en query's gaan uitvoeren.

Zie [Loading overview](sql-data-warehouse-overview-load.md) (Laden: overzicht) als u gegevens in SQL Data Warehouse wilt laden.

Als u een bestaande database probeert te migreren naar SQL Data Warehouse, raadpleeg dan [Migration overview](sql-data-warehouse-overview-migrate.md) (Migreren: overzicht) of gebruik het [Migratieprogramma](sql-data-warehouse-migrate-migration-utility.md).

Firewallregels kunnen ook worden geconfigureerd met behulp van Transact-SQL. Zie [sp_set_firewall_rule][sp_set_firewall_rule] en [sp_set_database_firewall_rule][sp_set_database_firewall_rule] voor meer informatie.

Het is ook een goed idee om te kijken naar de [Aanbevolen procedures][Best practices].

<!--Article references-->
[Create an Azure SQL database in the Azure portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[resource groups]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[Best practices]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md
[abonnement]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
