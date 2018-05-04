---
title: Transparante gegevensversleuteling in SQL datawarehouse (Portal) | Microsoft Docs
description: Transparante gegevensversleuteling (TDE) in SQL datawarehouse
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 9c4abb0416acc656a4cfae332377c398260191de
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Aan de slag met Transparent Data Encryption (TDE) in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Overzicht van beveiliging](sql-data-warehouse-overview-manage-security.md)
> * [Verificatie](sql-data-warehouse-authentication.md)
> * [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Versleuteling (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Vereiste bevoegdheden
U moet een beheerder of als lid van de rol dbmanager zijn zodat Transparent Data Encryption (TDE).

## <a name="enabling-encryption"></a>Codering inschakelen
Schakel TDE voor een SQL Data Warehouse door de volgende stappen uit te voeren:

1. Openen van de database in de [Azure-portal](https://portal.azure.com)
2. Klik in de databaseblade op de **instellingen** knop
3. Selecteer de **transparante gegevensversleuteling** optie ![][1]
4. Selecteer de **op** instelling ![][2]
5. Selecteer **opslaan**
   ![][3]  

## <a name="disabling-encryption"></a>Codering uitschakelen
Als u wilt uitschakelen TDE voor een SQL Data Warehouse, de volgende stappen uit te voeren:

1. Openen van de database in de [Azure-portal](https://portal.azure.com)
2. Klik in de databaseblade op de **instellingen** knop
3. Selecteer de **transparante gegevensversleuteling** optie ![][1]
4. Selecteer de **uit** instelling ![][4]
5. Selecteer **opslaan**
   ![][5]  

## <a name="encryption-dmvs"></a>Versleuteling DMV 's
Versleuteling kan worden bevestigd met de volgende DMV's:

* [sys.databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
