---
title: 'Snelstartgids: Onderbreken en hervatten compute in Azure SQL Data Warehouse - PowerShell | Microsoft Docs'
description: Gebruik PowerShell om te onderbreken compute in Azure SQL Data Warehouse om kosten te besparen. Berekeningen hervatten wanneer u klaar bent voor gebruik van het datawarehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: ef341a1528bf759461abfb7cfc6d878fd8a44cb4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
ms.locfileid: "31598894"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-sql-data-warehouse-with-powershell"></a>Snelstartgids: Onderbreken en hervatten compute in Azure SQL Data Warehouse met PowerShell
Gebruik PowerShell om te onderbreken compute in Azure SQL Data Warehouse om kosten te besparen. [Compute hervatten](sql-data-warehouse-manage-compute-overview.md) wanneer u bent klaar voor gebruik van het datawarehouse.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

Deze zelfstudie vereist Azure PowerShell-moduleversie 5.1.1 of hoger. Voer ` Get-Module -ListAvailable AzureRM` vinden van de versie u hebt op dit moment. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Voordat u begint

Deze snelstartgids wordt ervan uitgegaan dat u hebt al een SQL datawarehouse die u kunt onderbreken en hervatten. Als u maken wilt, kunt u [maken en Connect - portal](create-data-warehouse-portal.md) voor het maken van een datawarehouse aangeroepen **mySampleDataWarehouse**.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met behulp van de opdracht [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) en volg de instructies op het scherm.

```powershell
Connect-AzureRmAccount
```

Als u wilt zien welke abonnement u gebruikt, [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

```powershell
Get-AzureRmSubscription
```

Als u gebruiken een ander abonnement dan de standaardwaarde wilt, voert u [Select-AzureRmSubscription](/powershell/module/azurerm.profile/select-azurermsubscription).

```powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Datawarehouse-gegevens niet opzoeken

Zoek de databasenaam, de servernaam en de resourcegroep voor het datawarehouse dat u wilt onderbreken en hervatten.

Volg deze stappen voor de locatie-informatie voor uw datawarehouse niet vinden.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **SQL-databases** op de pagina naar links van de Azure portal.
3. Selecteer **mySampleDataWarehouse** van de **SQL-databases** pagina. Hiermee opent u het datawarehouse.

    ![Servergroep en de bron](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Noteer de naam voor datawarehouse, de naam van de database. Ook noteert u de naam van de server en de resourcegroep. U
5.  deze in de opdrachten onderbreken en hervatten.
6. Als uw server foo.database.windows.net, gebruikt u alleen het eerste deel als de naam van de server in de PowerShell-cmdlets. In de voorgaande afbeelding is de volledige servernaam newserver 20171113.database.windows.net. Verwijderen van het achtervoegsel en gebruik **newserver 20171113** als de naam van de server in de PowerShell-cmdlet.

## <a name="pause-compute"></a>De rekencapaciteit onderbreken
Als u wilt opslaan kosten, kunt u onderbreken en hervatten van compute bronnen op de aanvraag. Bijvoorbeeld als u de database's nachts en tijdens het weekend niet gebruikt, kunt u tijdens de tijdstippen onderbreken en hervatten gedurende de dag. Er zijn geen kosten voor de rekenresources terwijl de database is onderbroken. U blijven echter in rekening gebracht voor opslag.

Een database onderbreken, gebruikt u de [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase.md) cmdlet. Het volgende voorbeeld wordt een datawarehouse met de naam onderbroken **mySampleDataWarehouse** gehost op een server met de naam **newserver 20171113**. De server zich in een Azure-resourcegroep met de naam **myResourceGroup**.


```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
```

Een Variant deze volgende voorbeeld wordt de database opgehaald in het $database-object. Geeft vervolgens het object [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase). De resultaten worden opgeslagen in de resultDatabase object. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```


## <a name="resume-compute"></a>Compute hervatten
Voor het starten van een database, gebruikt u de [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase) cmdlet. Het volgende voorbeeld wordt een database met naam mySampleDataWarehouse gehost op een server met de naam newserver 20171113 gestart. De server zich in een Azure-resourcegroep met de naam myResourceGroup.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" -DatabaseName "mySampleDataWarehouse"
```

Een Variant deze volgende voorbeeld wordt de database opgehaald in het $database-object. Geeft vervolgens het object [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase.md) en slaat de resultaten in $resultDatabase. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

## <a name="clean-up-resources"></a>Resources opschonen

Er worden kosten in rekening gebracht voor datawarehouse-eenheden en gegevens die zijn opgeslagen in uw datawarehouse. Deze compute- en opslagresources worden apart in rekening gebracht.

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
