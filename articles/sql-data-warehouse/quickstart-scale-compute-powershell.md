---
title: 'Snelstartgids: Compute in Azure SQL Data Warehouse - PowerShell uitbreiden | Microsoft Docs'
description: PowerShell-taken uit te schalen rekenresources door datawarehouse units aan te passen.
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
ms.openlocfilehash: a3a435d6bdb0d35c96349540d5e9f9b5be61bd9b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-powershell"></a>Snelstartgids: Scale compute in Azure SQL Data Warehouse in PowerShell

Scale compute in Azure SQL Data Warehouse in PowerShell. [Uitschalen compute](sql-data-warehouse-manage-compute-overview.md) voor betere prestaties of schaal back-berekeningen voor het opslaan van kosten. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

Deze zelfstudie vereist Azure PowerShell-moduleversie 5.1.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` vinden van de versie u hebt op dit moment. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps.md). 

## <a name="before-you-begin"></a>Voordat u begint

Deze snelstartgids wordt ervan uitgegaan dat u hebt al een SQL datawarehouse die kunnen worden geschaald. Als u maken wilt, gebruikt u [maken en Connect - portal](create-data-warehouse-portal.md) voor het maken van een datawarehouse aangeroepen **mySampleDataWarehouse**. 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) en volg de instructies op het scherm.

```powershell
Add-AzureRmAccount
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

4. Noteer de naam datawarehouse die wordt gebruikt als de databasenaam van de. Ook noteert u de naam van de server en de resourcegroep. U wilt gebruiken in het onderbreken en hervatten opdrachten.
5. Als uw server foo.database.windows.net, gebruikt u alleen het eerste deel als de naam van de server in de PowerShell-cmdlets. In de voorgaande afbeelding is de volledige servernaam newserver 20171113.database.windows.net. We gebruiken **newserver 20171113** als de naam van de server in de PowerShell-cmdlet.

## <a name="scale-compute"></a>De schaal van Compute aanpassen

In SQL Data Warehouse, kunt u vergroten of verkleinen van rekenresources door datawarehouse units aan te passen. De [maken en Connect - portal](create-data-warehouse-portal.md) gemaakt **mySampleDataWarehouse** en met 400 dwu's wordt geïnitialiseerd. De volgende stappen aanpassen de dwu's voor **mySampleDataWarehouse**.

Datawarehouse als eenheden wilt wijzigen, gebruikt u de [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) PowerShell-cmdlet. Het volgende voorbeeld wordt de datawarehouse-eenheden op DW300 voor de database **mySampleDataWarehouse** die wordt gehost in de resourcegroep **myResourceGroup** op server  **mynewserver 20171113**.

```Powershell
Set-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
```

## <a name="check-database-state"></a>Controleer de databasestatus van de

Als de huidige status van het datawarehouse wilt weergeven, gebruikt de [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) PowerShell-cmdlet. Hiermee wordt de status van de **mySampleDataWarehouse** database in ResourceGroup **myResourceGroup** en server **mynewserver 20171113.database.windows.net**.

```powershell
Get-AzureRmSqlDatabase -ResourceGroupName myResourceGroup -ServerName mynewserver-20171113 -DatabaseName mySampleDataWarehouse
```

Dit resulteert in ongeveer het volgende:

```powershell   
ResourceGroupName             : myResourceGroup
ServerName                    : mynewserver-20171113
DatabaseName                  : mySampleDataWarehouse
Location                      : North Europe
DatabaseId                    : 34d2ffb8-b70a-40b2-b4f9-b0a39833c974
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1_General_CP1_CI_AS
CatalogCollation              :
MaxSizeBytes                  : 263882790666240
Status                        : Online
CreationDate                  : 11/20/2017 9:18:12 PM
CurrentServiceObjectiveId     : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
CurrentServiceObjectiveName   : DW300
RequestedServiceObjectiveId   : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           :
Tags                          :
ResourceId                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
                                resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/mynewserver-20171113/databases/mySampleDataWarehouse
CreateMode                    :
ReadScale                     : Disabled
ZoneRedundant                 : False
```

U kunt vervolgens controleren de **Status** van de database. In dit geval kunt u zien dat deze database online is.  Wanneer u deze opdracht uitvoert, ontvangt u een waarde voor de Status van Online, onderbreken, hervat, Scaling of onderbroken.

## <a name="next-steps"></a>Volgende stappen
U hebt nu geleerd hoe schalen compute voor uw datawarehouse. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.

> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
