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
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: faf17bcac66a70fc78bb171e172886fd2dcadca8
ms.contentlocale: nl-nl
ms.lasthandoff: 06/16/2017


---
<a id="get-started-with-azure-data-lake-analytics-using-azure-powershell" class="xliff"></a>

# Aan de slag met Azure Data Lake Analytics met Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informatie over het gebruik van Azure PowerShell om Azure Data Lake Analytics-accounts te maken en vervolgens U SQL-taken te verzenden en uit te voeren. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.

<a id="prerequisites" class="xliff"></a>

## Vereisten

Voordat u met deze zelfstudie begint, moet u beschikken over de volgende informatie:

* **Een Azure Data Lake Analytics-account**. Zie [Aan de slag met Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).
* **Een werkstation met Azure PowerShell**. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).

<a id="log-in-to-azure" class="xliff"></a>

## Meld u aan bij Azure.

In deze zelfstudie wordt ervan uitgegaan dat u al bekend bent met het gebruik van Azure PowerShell. Het is voornamelijk belangrijk dat u weet hoe u zich aanmeldt bij Azure. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) als u hulp nodig hebt.

Aanmelden met de naam van een abonnement:

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

In plaats van de abonnementsnaam, kunt u ook een abonnements-id gebruiken om u aan te melden:

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

Als dit lukt, ziet de uitvoer van deze opdracht er  uit als de volgende tekst:

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

<a id="preparing-for-the-tutorial" class="xliff"></a>

## Voorbereiding voor de zelfstudie

De PowerShell-fragmenten in deze zelfstudie gebruiken deze variabelen om deze informatie op te slaan:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

<a id="get-information-about-a-data-lake-analytics-account" class="xliff"></a>

## Informatie krijgen over een Data Lake Analytics-account

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

<a id="submit-a-u-sql-job" class="xliff"></a>

## Een U-SQL-taak verzenden

Maak een PowerShell-variabele om het U-SQL-script op te slaan.

```
$script = @"
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

"@
```

Verzend het script.

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

U kunt het script ook opslaan als een bestand en verzenden met de volgende opdracht:

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


Haal de status van een bepaalde taak op. Blijf deze cmdlet gebruiken totdat de taak is voltooid.

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

U hoeft niet steeds Get-AdlAnalyticsJob aan te roepen tot een taak is voltooid als u de cmdlet Wait-AdlJob gebruikt.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Download het uitvoerbestand.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

<a id="see-also" class="xliff"></a>

## Zie ook
* Als u dezelfde zelfstudie wilt bekijken met een ander hulpprogramma, klikt u op de tabselectors boven aan de pagina.
* Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) om U-SQL te leren.
* Zie [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md) voor informatie over beheertaken.

