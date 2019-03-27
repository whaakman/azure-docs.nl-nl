---
title: 'Quickstart: Onderbreken en hervatten compute in Azure SQL Data Warehouse - PowerShell | Microsoft Docs'
description: Gebruik PowerShell om te onderbreken compute in Azure SQL Data Warehouse om kosten te besparen. Compute hervat wanneer u klaar bent voor het gebruik van het datawarehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 03/20/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: ce1fd1af404f5fc44bc202be08cd2c2f1b4ef909
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58443831"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-sql-data-warehouse-with-powershell"></a>Quickstart: Onderbreken en hervatten compute in Azure SQL Data Warehouse met PowerShell

Gebruik PowerShell om te onderbreken compute in Azure SQL Data Warehouse om kosten te besparen. [Compute hervatten](sql-data-warehouse-manage-compute-overview.md) wanneer u bent klaar voor gebruik van het datawarehouse.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In deze snelstartgids wordt ervan uitgegaan dat u hebt al een SQL datawarehouse die u kunt onderbreken en hervatten. Als u maken wilt, kunt u [maken en verbinden - portal](create-data-warehouse-portal.md) te maken van een datawarehouse met de naam **mySampleDataWarehouse**.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de [Connect AzAccount](/powershell/module/az.accounts/connect-azaccount) opdracht en volgt u de op het scherm aanwijzingen.

```powershell
Connect-AzAccount
```

Als u wilt zien welke abonnement dat u gebruikt, [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription).

```powershell
Get-AzSubscription
```

Als u gebruiken een ander abonnement dan de standaard wilt, voert u [Set AzContext](/powershell/module/az.accounts/set-azcontext).

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Datawarehousegegevens opzoeken

Zoek de databasenaam, de servernaam en de resourcegroep op voor het datawarehouse dat u wilt onderbreken en hervatten.

Volg deze stappen om de locatiegegevens voor uw datawarehouse op te zoeken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op de linkerpagina in Azure Portal op **SQL-databases**.
3. Selecteer **mySampleDataWarehouse** op de pagina **SQL-databases**. De datawarehouse wordt geopend.

    ![Servernaam en resourcegroep](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Noteer de naam van het datawarehouse de naam van de database. Noteer ook de naam van de server en de resourcegroep.
6. Als uw server foo.database.windows.net is, gebruikt u alleen het eerste deel als de servernaam in de PowerShell-cmdlets. Op de voorgaande afbeelding is de volledige servernaam newserver 20171113.database.windows.net. Verwijder het achtervoegsel en gebruik **newserver-20171113** als de naam van de server in de PowerShell-cmdlet.

## <a name="pause-compute"></a>De rekencapaciteit onderbreken

Om kosten te besparen, kunt u onderbreken en hervatten van de compute-resources op de aanvraag. Bijvoorbeeld, als u de database niet tijdens de nacht en tijdens het weekend gebruikt, kunt u tijdens deze perioden onderbreken, en hervatten gedurende de dag. Er zijn geen kosten voor compute-resources, terwijl de database is onderbroken. U blijven echter in rekening gebracht voor opslag.

Als u wilt onderbreken van een database, gebruikt u de [stand-by-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase) cmdlet. Het volgende voorbeeld wordt een datawarehouse met de naam onderbroken **mySampleDataWarehouse** die worden gehost op een server met de naam **newserver-20171113**. De server zich in een Azure-resourcegroep met de naam **myResourceGroup**.


```Powershell
Suspend-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
```

Een Variant deze volgende voorbeeld wordt de database in het object $database. Deze geeft vervolgens het object [stand-by-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase). De resultaten worden opgeslagen in de resultDatabase object. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```


## <a name="resume-compute"></a>De rekencapaciteit hervatten

Voor het starten van een database, gebruikt u de [hervatten AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase) cmdlet. Het volgende voorbeeld wordt een database met de naam die wordt gehost op een server met de naam newserver-20171113 mySampleDataWarehouse gestart. De server zich in een Azure-resourcegroep met de naam myResourceGroup.

```Powershell
Resume-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" -DatabaseName "mySampleDataWarehouse"
```

Een Variant deze volgende voorbeeld wordt de database in het object $database. Deze geeft vervolgens het object [hervatten AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase) en worden de resultaten opgeslagen in $resultDatabase. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzSqlDatabase
$resultDatabase
```

## <a name="check-status-of-your-data-warehouse-operation"></a>Controleer de status van uw datawarehouse-bewerking

Om te controleren of de status van uw datawarehouse, gebruikt u de [Get-AzureRmSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/azurerm.sql/Get-AzureRmSqlDatabaseActivity#description) cmdlet.

```
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "ResourceGroup01" -ServerName "Server01" -DatabaseName "Database02"
```

## <a name="clean-up-resources"></a>Resources opschonen

Er worden kosten in rekening gebracht voor datawarehouse-eenheden en gegevens die zijn opgeslagen in uw datawarehouse. Deze compute- en opslagresources worden apart in rekening gebracht.

- Als u dat de gegevens in storage wilt, onderbreken.
- Als u in de toekomst geen kosten meer wilt hebben, kunt u de datawarehouse verwijderen.

Volg deze stappen om de resources op te schonen zoals gewenst.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en klikt u op uw datawarehouse.

    ![Resources opschonen](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Als u het berekenen wilt onderbreken, klikt u op de knop **Onderbreken**. Als het datawarehouse is onderbroken, ziet u een knop **Start**.  Als u het berekenen wilt hervatten, klikt u op **Start**.

3. Als u de datawarehouse wilt verwijderen zodat er geen kosten in rekening worden gebracht voor berekenen of opslaan, klikt u op **Verwijderen**.

4. Als u wilt verwijderen van de SQL-server die u hebt gemaakt, klikt u op **mynewserver-20171113.database.windows.net**, en klik vervolgens op **verwijderen**.  Wees voorzichtig met verwijderen. Als u de server verwijdert, worden ook alle databases verwijderd die zijn toegewezen aan de server.

5. Als u de resourcegroep wilt verwijderen, klikt u op **myResourceGroup**. Klik vervolgens op **Resourcegroep verwijderen**.


## <a name="next-steps"></a>Volgende stappen

U hebt nu onderbroken en hervat de rekenkracht voor uw datawarehouse. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.

> [!div class="nextstepaction"]
> [Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
