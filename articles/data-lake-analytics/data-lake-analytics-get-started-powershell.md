---
title: Aan de slag met Azure Data Lake Analytics met Azure PowerShell | Microsoft Docs
description: 'Gebruik Azure PowerShell om een Data Lake Analytics-account te maken, een Data Lake Analytics-taak te maken met U-SQL en de taak te verzenden. '
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 6985dff332928d704f30e167c3bddb62bcc6cac1
ms.contentlocale: nl-nl
ms.lasthandoff: 05/09/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Zelfstudie: Aan de slag met Azure Data Lake Analytics met Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informatie over het gebruik van Azure PowerShell om Azure Data Lake Analytics-accounts te maken en vervolgens U SQL-taken te verzenden en uit te voeren. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u beschikken over de volgende informatie:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een werkstation met Azure PowerShell**. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).

## <a name="preparing-for-the-tutorial"></a>Voorbereiding voor de zelfstudie
Om een Data Lake Analytics-account te maken, moet u eerst het volgende opgeven:

* **Azure Resource Group**: een Data Lake Analytics-account moet worden gemaakt binnen een Azure-resourcegroep.
* **Naam van de Data Lake Analytics-account**: de naam van de Data Lake Analytics-account mag alleen kleine letters en cijfers bevatten.
* **Locatie**: een van de Azure-datacenters die ondersteuning biedt voor Data Lake Analytics.
* **Data Lake Store-standaardaccount**: elk Data Lake Store Analytics-account heeft een Data Lake Store-standaardaccount. Deze accounts moeten zich op dezelfde locatie bevinden.

De PowerShell-fragmenten in deze zelfstudie gebruiken deze variabelen om deze informatie op te slaan

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="create-a-data-lake-analytics-account"></a>Een Data Lake Analytics-account maken

Als u nog geen resourcegroep hebt om te gebruiken, maakt u er een. 

```
New-AzureRmResourceGroup -Name  $rg -Location $location
```

Elke Data Lake Analytics-account vereist een Data Lake Store-standaardaccount dat wordt gebruikt voor het opslaan van logboeken. U kunt een nieuw account maken of een bestaand account gebruiken. 

```
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

Wanneer een resourcegroep en Data Lake Store-account beschikbaar zijn, kunt u een Data Lake Analytics-account maken.

```
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Informatie krijgen over een Data Lake Analytics-account

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>Een U-SQL-taak verzenden

Maak een tekstbestand met het volgende U-SQL-script.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Verzend het script.

```
Submit-AdlJob -AccountName $adla –ScriptPath "d:\test.usql"Submit
```

## <a name="monitor-u-sql-jobs"></a>U-SQL-taken bewaken

Geef alle taken in de account weer in een lijst. De uitvoer bevat de taken die momenteel worden uitgevoerd en de taken die onlangs zijn voltooid.

```
Get-AdlJob -Account $adla
```

Haal de status van een bepaalde taak op.

```
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

U hoeft niet steeds Get-AdlAnalyticsJob aan te roepen tot een taak is voltooid als u de cmdlet Wait-AdlJob gebruikt.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Nadat de taak is voltooid, controleert u of het uitvoerbestand is gemaakt door de bestanden in een map weer te geven.

```
Get-AdlStoreChildItem -Account $adls -Path "/"
```

Controleer op de aanwezigheid van een bestand.

```
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading-files"></a>Bestanden uploaden en downloaden

Download de uitvoer van het U-SQL-script.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv"  -Destination "D:\data.csv"
```


Upload een bestand dat moet worden gebruikt als invoer voor een U-SQL-script.

```
Import-AdlStoreItem -AccountName $adls -Path "D:\data.tsv" -Destination "/data_copy.csv" 
```

## <a name="see-also"></a>Zie ook
* Als u dezelfde zelfstudie wilt bekijken met een ander hulpprogramma, klikt u op de tabselectors boven aan de pagina.
* Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) om U-SQL te leren.
* Zie [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md) voor informatie over beheertaken.

