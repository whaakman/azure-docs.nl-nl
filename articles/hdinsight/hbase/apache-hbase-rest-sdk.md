---
title: De HBase .NET SDK - Azure HDInsight gebruiken
description: Gebruik de HBase .NET SDK maken en verwijderen van tabellen, en voor het lezen en schrijven van gegevens.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/13/2017
ms.author: ashishth
ms.openlocfilehash: 89c74b0c2144776d3bbc8a87f660b546ad40987f
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495416"
---
# <a name="use-the-net-sdk-for-apache-hbase"></a>Gebruik de .net SDK voor Apache HBase

[Apache HBase](apache-hbase-overview.md) twee primaire manieren om te werken met uw gegevens: [Apache Hive-query's en aanroepen naar de HBase-RESTful-API](apache-hbase-tutorial-get-started-linux.md). U kunt werken rechtstreeks met de REST-API met behulp van de `curl` opdracht of een vergelijkbaar hulpprogramma.

Voor C# en .NET-toepassingen, de [Microsoft HBase REST-clientbibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.HBase.Client/) biedt een clientbibliotheek boven op de HBase REST-API.

## <a name="install-the-sdk"></a>De SDK installeren

De HBase .NET SDK wordt geleverd als een NuGet-pakket, die kan worden geïnstalleerd vanuit de Visual Studio **NuGet Package Manager Console** met de volgende opdracht:

    Install-Package Microsoft.HBase.Client

## <a name="instantiate-a-new-hbaseclient-object"></a>Exemplaar van een nieuwe HBaseClient-object maken

Voor het gebruik van de SDK, exemplaar maken van een nieuwe `HBaseClient` -object doorgeven in een `ClusterCredentials` bestaat uit de `Uri` aan het cluster, en de Hadoop-gebruikersnaam en wachtwoord.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net"), "USERNAME", "PASSWORD");
client = new HBaseClient(credentials);
```

Vervang CLUSTERNAME met de naam van HDInsight HBase-cluster, en de gebruikersnaam en wachtwoord door de Apache Hadoop-referenties opgegeven voor het maken van clusters. De standaardnaam van de Hadoop-gebruiker is **admin**.

## <a name="create-a-new-table"></a>Een nieuwe tabel maken

HBase worden gegevens opgeslagen in tabellen. Een tabel bestaat uit een *Rowkey*, de primaire sleutel en een of meer groepen van kolommen met de naam *kolomfamilies*. De gegevens in elke tabel horizontaal wordt gedistribueerd door een bereik Rowkey in *regio's*. Elke regio heeft een begin- en -sleutel. Een tabel kan een of meer regio's hebben. Als de gegevens in de tabel groeit, wordt grote regio's in HBase gesplitst in kleinere regio's. Regio's worden opgeslagen in *regioservers*, waar één regioserver meerdere regio's kan opslaan.

De gegevens fysiek worden opgeslagen in *HFiles*. Een enkele HFile bevat gegevens voor één tabel, één regio en een kolomfamilie. Rijen in HFile worden opgeslagen op Rowkey gesorteerd. Elke HFile heeft een *B +-structuur* index voor het snel ophalen van de rijen.

Geef voor het maken van een nieuwe tabel, een `TableSchema` en kolommen. De volgende code wordt gecontroleerd of de tabel 'RestSDKTable' bestaat al - als niet het geval is, de tabel wordt gemaakt.

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

Deze nieuwe tabel heeft twee kolomfamilies, t1 en t2. Aangezien kolomfamilies apart zijn opgeslagen in verschillende HFiles, worden het verstandig om een afzonderlijke kolom-familie voor vaak opgevraagde gegevens. In de volgende [gegevens invoegen](#insert-data) bijvoorbeeld kolommen worden toegevoegd aan de t1 kolomfamilie.

## <a name="delete-a-table"></a>Een tabel verwijderen

Een tabel verwijderen:

```csharp
await client.DeleteTableAsync("RestSDKTable");
```

## <a name="insert-data"></a>Gegevens invoegen

Voor het invoegen van gegevens, kunt u een unieke rij-sleutel opgeven als de rij-id. Alle gegevens worden opgeslagen in een `byte[]` matrix. De volgende code definieert en voegt de `title`, `director`, en `release_date` kolommen aan de kolom t1, als deze kolommen zijn de meest gebruikte. De `description` en `tagline` kolommen worden toegevoegd aan het tijdstip t2 kolomfamilie. U kunt uw gegevens partitioneren in kolomfamilies indien nodig.

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

HBase implementeert [Cloud BigTable](https://cloud.google.com/bigtable/), zodat de gegevensindeling uitziet:

![Gebruiker met de Cluster-gebruikersrol](./media/apache-hbase-rest-sdk/table.png)

## <a name="select-data"></a>Gegevens selecteren

Voor het lezen van gegevens uit een HBase-tabel geeft de tabel en rij de sleutel aan de `GetCellsAsync` methode om terug te keren de `CellSet`.

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

In dit geval retourneert de code alleen de eerste overeenkomende rij, als er moet slechts één rij voor een unieke sleutel. De geretourneerde waarde is gewijzigd in `string` indeling van de `byte[]` matrix. U kunt ook de waarde converteren naar andere typen, zoals een geheel getal voor de releasedatum van de film:

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

Maakt gebruik van HBase `scan` een of meer rijen op te halen. In dit voorbeeld vraagt meerdere rijen in batches van 10 en haalt gegevens waarvan u de belangrijkste waarden tussen 25 en 35 liggen. Bij het ophalen van alle rijen, verwijdert u de scanner voor het opschonen van resources.

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

* [Aan de slag met een voorbeeld voor Apache HBase in HDInsight](apache-hbase-tutorial-get-started-linux.md)
* Maken van een end-to-end toepassing met [realtime Twitter-gevoel met Apache HBase analyseren](../hdinsight-hbase-analyze-twitter-sentiment.md)
