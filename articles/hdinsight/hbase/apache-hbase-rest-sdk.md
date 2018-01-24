---
title: De .NET SDK voor HBase - Azure HDInsight gebruiken | Microsoft Docs
description: Gebruik de HBase .NET SDK maken en verwijderen van tabellen, en voor het lezen en schrijven van gegevens.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: ashishth
ms.openlocfilehash: 083150fe5f8787ba791d3d692db73c5156f11e55
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-hbase-net-sdk"></a>De HBase .NET SDK gebruiken

[HBase](apache-hbase-overview.md) biedt twee primaire keuzes werken met uw gegevens: [Hive-query's en aanroepen naar de HBase-RESTful-API](apache-hbase-tutorial-get-started-linux.md). U kunt werken rechtstreeks met de REST-API met behulp van de `curl` opdracht of een vergelijkbaar hulpprogramma.

Voor C# en .NET-toepassingen, de [Microsoft HBase REST-clientbibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.HBase.Client/) een clientbibliotheek boven op de HBase REST-API biedt.

## <a name="install-the-sdk"></a>De SDK installeren

De HBase .NET SDK is beschikbaar als een NuGet-pakket kan worden geïnstalleerd vanuit de Visual Studio **NuGet Package Manager Console** met de volgende opdracht:

    Install-Package Microsoft.HBase.Client

## <a name="instantiate-a-new-hbaseclient-object"></a>Het instantiëren van een nieuw HBaseClient-object

Voor het gebruik van de SDK exemplaar maken van een nieuwe `HBaseClient` object, dat wordt doorgegeven `ClusterCredentials` bestaat uit de `Uri` voor uw cluster, en de Hadoop-gebruikersnaam en wachtwoord.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net"), "USERNAME", "PASSWORD");
client = new HBaseClient(credentials);
```

Vervangen door CLUSTERNAME de naam van HDInsight HBase-cluster, en de gebruikersnaam en wachtwoord met de Hadoop-referenties opgegeven voor het maken van het cluster. De standaardnaam van de Hadoop-gebruiker is **admin**.

## <a name="create-a-new-table"></a>Een nieuwe tabel maken

HBase opslaat-gegevens in de tabellen. Een tabel bestaat uit een *Rowkey*, de primaire sleutel en een of meer groepen van kolommen met de naam *kolomfamilies*. De gegevens in elke tabel horizontaal wordt gedistribueerd door een bereik Rowkey in *regio's*. Elke regio heeft een begin- en -sleutel. Een tabel kan een of meer regio's hebben. Wanneer de gegevens in de tabel groeit, splitst HBase grote regio's in kleinere regio's. Regio's worden opgeslagen in *regio servers*, waar één regio server meerdere regio's kan opslaan.

De gegevens fysiek worden opgeslagen *HFiles*. Een enkele HFile bevat gegevens voor één tabel, één regio en één kolom gezin. Rijen in HFile, gesorteerd op Rowkey worden opgeslagen. Elke HFile heeft een *B +-structuur* index voor snelle voor het ophalen van de rijen.

Geef voor het maken van een nieuwe tabel, een `TableSchema` en kolommen. De volgende code wordt gecontroleerd of de tabel 'RestSDKTable' bestaat al - zo niet, de tabel is gemaakt.

```csharp
if (!client.ListTablesAsync().Result.name.Contains("RestSDKTable"))
{
    // Create the table
    var newTableSchema = new TableSchema {name = "RestSDKTable" };
    newTableSchema.columns.Add(new ColumnSchema() {name = "t1"});
    newTableSchema.columns.Add(new ColumnSchema() {name = "t2"});

    await client.CreateTableAsync(newTableSchema);
}
```

Deze nieuwe tabel heeft twee kolomfamilies, t1 en t2. Omdat kolomfamilies afzonderlijk in verschillende HFiles worden opgeslagen, is het verstandig om een aparte kolom-serie voor vaak opgevraagde gegevens. In de volgende [gegevens invoegen](#insert-data) bijvoorbeeld kolommen worden toegevoegd aan de kolom t1 familie.

## <a name="delete-a-table"></a>Een tabel verwijderen

Een tabel verwijderen:

```csharp
await client.DeleteTableAsync("RestSDKTable");
```

## <a name="insert-data"></a>Gegevens invoegen

Als gegevens wilt invoegen, kunt u een unieke rij-sleutel als de rij-id opgeven. Alle gegevens worden opgeslagen in een `byte[]` matrix. De volgende code definieert en voegt de `title`, `director`, en `release_date` kolommen in de familie t1-kolom als deze kolommen zijn het meest frequent gebruikte. De `description` en `tagline` kolommen worden toegevoegd aan de kolom t2 familie. U kunt uw gegevens partitioneren in kolomfamilies indien nodig.

```csharp
var key = "fifth_element";
var row = new CellSet.Row { key = Encoding.UTF8.GetBytes(key) };
var value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:title"),
    data = Encoding.UTF8.GetBytes("The Fifth Element")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:director"),
    data = Encoding.UTF8.GetBytes("Luc Besson")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:release_date"),
    data = Encoding.UTF8.GetBytes("1997")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t2:description"),
    data = Encoding.UTF8.GetBytes("In the colorful future, a cab driver unwittingly becomes the central figure in the search for a legendary cosmic weapon to keep Evil and Mr Zorg at bay.")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t2:tagline"),
    data = Encoding.UTF8.GetBytes("The Fifth is life")
};
row.values.Add(value);

var set = new CellSet();
set.rows.Add(row);

await client.StoreCellsAsync("RestSDKTable", set);
```

HBase implementeert BigTable, zodat de gegevensindeling op het volgende lijkt:

![Gebruiker met de gebruikersrol van het Cluster](./media/apache-hbase-rest-sdk/table.png)

## <a name="select-data"></a>Gegevens selecteren

Om te lezen van gegevens uit een HBase-tabel doorgeven naam- en rijkoppen sleutel van de tabel aan de `GetCellsAsync` methode om te retourneren de `CellSet`.

```csharp
var key = "fifth_element";

var cells = await client.GetCellsAsync("RestSDKTable", key);
// Get the first value from the row.
Console.WriteLine(Encoding.UTF8.GetString(cells.rows[0].values[0].data));
// Get the value for t1:title
Console.WriteLine(Encoding.UTF8.GetString(cells.rows[0].values
    .Find(c => Encoding.UTF8.GetString(c.column) == "t1:title").data));
// With the previous insert, it should yield: "The Fifth Element"
```

In dit geval wordt retourneert de code alleen de eerste overeenkomende rij, aangezien er moet slechts één rij voor een unieke sleutel. De geretourneerde waarde is gewijzigd in `string` opmaken van de `byte[]` matrix. U kunt ook de waarde converteren naar andere typen, zoals een geheel getal voor de film Releasedatum:

```csharp
var releaseDateField = cells.rows[0].values
    .Find(c => Encoding.UTF8.GetString(c.column) == "t1:release_date");
int releaseDate = 0;

if (releaseDateField != null)
{
    releaseDate = Convert.ToInt32(Encoding.UTF8.GetString(releaseDateField.data));
}
Console.WriteLine(releaseDate);
// Should return 1997
```

## <a name="scan-over-rows"></a>Rijen wordt gescand

Maakt gebruik van HBase `scan` voor het ophalen van een of meer rijen. Dit voorbeeld meerdere rijen in batches van 10-aanvragen en haalt gegevens waarvan u de belangrijkste waarden tussen 25 en 35 zijn. Bij het ophalen van alle rijen, verwijder de scanner om resources op te schonen.

```csharp
var tableName = "mytablename";

// Assume the table has integer keys and we want data between keys 25 and 35
var scanSettings = new Scanner()
{
    batch = 10,
    startRow = BitConverter.GetBytes(25),
    endRow = BitConverter.GetBytes(35)
};
RequestOptions scanOptions = RequestOptions.GetDefaultOptions();
scanOptions.AlternativeEndpoint = "hbaserest0/";
ScannerInformation scannerInfo = null;
try
{
    scannerInfo = await client.CreateScannerAsync(tableName, scanSettings, scanOptions);
    CellSet next = null;
    while ((next = client.ScannerGetNextAsync(scannerInfo, scanOptions).Result) != null)
    {
        foreach (var row in next.rows)
        {
            // ... read the rows
        }
    }
}
finally
{
    if (scannerInfo != null)
    {
        await client.DeleteScannerAsync(tableName, scannerInfo, scanOptions);
    }
}
```

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met een voorbeeld van Apache HBase in HDInsight](apache-hbase-tutorial-get-started-linux.md)
* Een end-to-end-toepassing met bouwen [realtime Twitter-gevoel met HBase analyseren](../hdinsight-hbase-analyze-twitter-sentiment.md)
