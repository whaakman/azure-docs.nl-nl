---
title: Azure SQL Database JSON-functies | Microsoft Docs
description: Azure SQL Database kunt u parse, query- en de gegevens opmaken in de notatie JSON (JavaScript Object)-notatie.
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 55860105-2f5f-4b10-87a0-99faa32b5653
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.date: 11/15/2016
ms.author: jovanpop
ms.workload: On Demand
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 8877b0bb779501df003ce11d66d9625c2f99e9ce
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Aan de slag met JSON-functies in Azure SQL Database
Azure SQL Database kunt u parseren en een query over gegevens die worden weergegeven in JavaScript Object Notation [(JSON)](http://www.json.org/) formatteren en relationele gegevens worden geëxporteerd als JSON-tekst.

JSON is een populair gegevensindeling gebruikt voor het uitwisselen van gegevens in moderne webtoepassingen en mobiele toepassingen. JSON wordt ook gebruikt voor het opslaan van semi-gestructureerde gegevens in logboekbestanden of in de NoSQL-databases zoals [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/). Veel REST-webservices worden opgemaakt als JSON-tekst of gegevens accepteert opgemaakt als JSON. De meeste Azure services, zoals [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), en [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) REST-eindpunten die retourneren of JSON gebruiken.

Azure SQL Database kunt u eenvoudig werken met JSON-gegevens en het integreren van uw database met moderne services.

## <a name="overview"></a>Overzicht
Azure SQL Database biedt de volgende functies voor het werken met JSON-gegevens:

![JSON-functies](./media/sql-database-json-features/image_1.png)

Als u JSON-tekst hebt, kunt u gegevens ophalen uit JSON of Controleer of JSON correct is geformatteerd met behulp van de ingebouwde functies [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), en [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). De [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) functie kunt u de waarde in JSON-tekst wordt bijgewerkt. Voor meer query's en analyse geavanceerde, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) functie een matrix met JSON-objecten kan omzetten in een set rijen. Een SQL-query kan worden uitgevoerd op de geretourneerde resultatenset. Ten slotte wordt er is een [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) component waarmee u gegevens in de relationele tabellen wordt opgeslagen als JSON-tekst opmaken.

## <a name="formatting-relational-data-in-json-format"></a>Opmaak van relationele gegevens in JSON-indeling
Als u een webservice dat vergt gegevens uit de database layer en biedt een reactie in JSON-indeling of clientzijde JavaScript frameworks en bibliotheken die gegevens accepteert die zijn opgemaakt als JSON hebt, kunt u de inhoud van uw database opmaken als JSON rechtstreeks in een SQL-query. U niet meer nodig om de toepassingscode die indelingen van resultaten van Azure SQL Database als JSON of sommige JSON-serialisatie-bibliotheek voor het converteren van de resultaten in tabelvorm query en vervolgens serialiseren objecten naar JSON-indeling. In plaats daarvan kunt u de component FOR JSON opmaken van de resultaten van de SQL-query als JSON-code in Azure SQL Database en deze rechtstreeks in uw toepassing gebruiken.

In het volgende voorbeeld worden rijen uit de tabel Sales.Customer opgemaakt als JSON met behulp van de component FOR JSON:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

De resultaten van de query de component FOR JSON PATH opgemaakt als JSON-tekst. Kolomnamen worden gebruikt als de sleutels, de celwaarden als JSON-waarden worden gegenereerd:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

De resultatenset is opgemaakt als een JSON-matrix waarbij elke rij is opgemaakt als een afzonderlijk JSON-object.

PAD geeft aan dat u de indeling van de uitvoer van uw JSON-resultaat met puntnotatie in Kolomaliassen kunt aanpassen. De volgende query wordt de naam van de sleutel 'CustomerName' in de JSON-indeling van de uitvoer en plaatst het telefoon-en faxnummer in het onderliggende object 'Neem contact op met':

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

De uitvoer van deze query ziet er als volgt:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

In dit voorbeeld wordt een JSON-object in plaats van een matrix geretourneerd door te geven de [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) optie. U kunt deze optie gebruiken als u weet dat u een enkel object als gevolg van de query retourneert.

De belangrijkste waarde van de FOR JSON-component is dat Hiermee kunt u complexe hiërarchische gegevens worden geretourneerd uit de database die is geformatteerd als geneste JSON-objecten of -matrices. Het volgende voorbeeld laat zien hoe Orders die deel uitmaken van de klant als een geneste matrix van Orders opnemen:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

In plaats van afzonderlijke query's voor het ophalen van gegevens van de klant en dan om een lijst met verwante Orders ophalen, kun je de benodigde gegevens met één query op, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>Werken met JSON-gegevens
Als u geen strikt gestructureerde gegevens op als u complexe onderliggende objecten, matrices of hiërarchische gegevens hebt of als uw gegevensstructuren evolueren gedurende een bepaalde periode, kunt de JSON-indeling u elke complexe gegevensstructuur vertegenwoordigt.

JSON wordt een tekstuele indeling die kan worden gebruikt als andere tekenreekstype in Azure SQL Database. U kunt verzenden of JSON-gegevens opslaan als een standaard NVARCHAR:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

De JSON-gegevens die in dit voorbeeld wordt weergegeven met behulp van het type NVARCHAR(MAX). JSON kan worden ingevoegd in deze tabel of als een argument van de opgeslagen procedure met behulp van standaard Transact-SQL-syntaxis, zoals wordt weergegeven in het volgende voorbeeld:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Elke client-side taal of de bibliotheek die geschikt is voor tekenreeksgegevens in Azure SQL Database werkt ook met JSON-gegevens. JSON kan worden opgeslagen in een tabel die ondersteuning biedt voor het type NVARCHAR, zoals een tabel geoptimaliseerd voor geheugen of een systeemversietabel. JSON wordt geen eventuele beperkingen in de client-side '-code of in de databaselaag.

## <a name="querying-json-data"></a>JSON-gegevens opvragen
Als u gegevens die zijn opgemaakt als JSON opgeslagen in Azure SQL-tabellen, kunnen JSON-functies u deze gegevens in een SQL-query.

JSON-functies die beschikbaar in Azure SQL database kunt zijn u gegevens die zijn opgemaakt als JSON als elke andere SQL-gegevenstype behandelen. U kunt eenvoudig waarden ophalen uit de JSON-tekst en JSON-gegevens in een query gebruiken:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

De functie JSON_VALUE haalt een waarde uit JSON-tekst opgeslagen in de kolom met gegevens. Deze functie gebruikt een JavaScript-achtige pad om te verwijzen naar een waarde in JSON-tekst om uit te pakken. De geëxtraheerde waarde kan worden gebruikt in een deel van de SQL-query.

De functie JSON_QUERY is vergelijkbaar met JSON_VALUE. In tegenstelling tot JSON_VALUE pakt deze functie complexe onderliggende object zoals matrices of objecten die in JSON-tekst worden geplaatst.

De functie JSON_MODIFY kunt u het pad van de waarde opgeven in de JSON-tekst die moet worden bijgewerkt, evenals een nieuwe waarde die de oude heeft, worden overschreven. Op deze manier kunt u eenvoudig bijwerken JSON-tekst zonder de volledige structuur reparsing.

Aangezien JSON is opgeslagen in een standaardtekst, zijn er geen garanties dat de waarden in tekstkolommen opgeslagen juist zijn opgemaakt. U kunt controleren tekst opgeslagen in JSON-kolom is correct opgemaakt met behulp van standaard check-beperkingen voor Azure SQL Database en de functie ISJSON:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Als de ingevoerde tekst is juist opgemaakt JSON, de functie ISJSON retourneert de waarde 1. Bij elke invoegen of bijwerken van de JSON-kolom, controleert deze beperking of nieuwe tekstwaarde is niet een verkeerd ingedeelde JSON.

## <a name="transforming-json-into-tabular-format"></a>Omzetten van JSON in tabelvorm
Azure SQL Database kunt u verzamelingen JSON in tabelvorm indeling en load of query JSON-gegevens te transformeren.

OPENJSON is een tabelwaarde functie die wordt geparseerd JSON-tekst, zoekt de client een matrix met JSON-objecten, de elementen van de matrix doorlopen en retourneert één rij in het uitvoerresultaat van de voor elk element van de matrix.

![JSON in tabelvorm](./media/sql-database-json-features/image_2.png)

In het bovenstaande voorbeeld kunnen we opgeven waar u de JSON-matrix die moet worden geopend (in de $. Orders pad), welke kolommen moeten worden geretourneerd als resultaat, en over de JSON-waarden die worden geretourneerd als cellen.

We kunnen transformeert u een JSON-matrix in de @orders variabele in een set rijen, deze resultaatset analyseren of invoegen van rijen in een standaard tabel:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
De verzameling van orders opgemaakt als een JSON-matrix en die als een parameter voor de opgeslagen procedure kan worden geparseerd en ingevoegd in de tabel Orders wordt verstrekt.

## <a name="next-steps"></a>Volgende stappen
Voor informatie over het integreren van JSON in uw toepassing, bekijk deze resources:

* [Blog van TechNet](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [MSDN-documentatie](https://msdn.microsoft.com/library/dn921897.aspx)
* [Channel 9 video](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Zie voor meer informatie over verschillende scenario's voor het integreren van JSON in uw toepassing, de demo's in deze [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) of vinden van een scenario dat overeenkomt met uw gebruiksvoorbeeld in [JSON blogberichten](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).

