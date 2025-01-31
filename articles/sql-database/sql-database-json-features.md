---
title: Werken met JSON-gegevens in Azure SQL Database | Microsoft Docs
description: Azure SQL Database kunt u om te parseren, query- en de gegevens opmaken in JavaScript Object Notation (JSON)-notatie.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
manager: craigg
ms.date: 01/15/2019
ms.openlocfilehash: 77f6125980c43817230b8a8d4beb32757f23e6c2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60702954"
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Aan de slag met JSON-functies in Azure SQL Database
Azure SQL Database kunt u parseren en query uitvoeren op gegevens die worden weergegeven in JavaScript Object Notation [(JSON)](https://www.json.org/) formatteren en exporteren van uw relationele gegevens als JSON-tekst. De volgende JSON-scenario's zijn beschikbaar in Azure SQL Database:
- [Opmaak van relationele gegevens in JSON-indeling](#formatting-relational-data-in-json-format) met behulp van `FOR JSON` component.
- [Werken met JSON-gegevens](#working-with-json-data)
- [JSON-gegevens opvragen](#querying-json-data) met behulp van JSON-functies.
- [JSON transformeren in tabelvorm](#transforming-json-into-tabular-format) met behulp van `OPENJSON` functie.

## <a name="formatting-relational-data-in-json-format"></a>Opmaak van relationele gegevens in JSON-indeling
Als u een webservice die neemt gegevens uit de database-laag en biedt een antwoord in de JSON-indeling of client-side JavaScript-frameworks en bibliotheken die gegevens accepteren die zijn opgemaakt als JSON hebt, kunt u de inhoud van uw database opmaken als JSON rechtstreeks in een SQL-query. U niet meer nodig om de toepassingscode die resultaten van Azure SQL Database als JSON opmaakt of bevatten sommige serialisatiebibliotheek JSON converteren tabellaire queryresultaten en vervolgens serialiseren objecten naar de JSON-indeling. In plaats daarvan kunt u de FOR JSON-component SQL-queryresultaten opmaken als JSON in Azure SQL Database en deze rechtstreeks in uw toepassing gebruiken.

In het volgende voorbeeld worden rijen uit de tabel Sales.Customer opgemaakt als JSON met behulp van de component FOR JSON:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

De resultaten van de query de component FOR JSON PATH opgemaakt als JSON-tekst. De namen van kolommen worden gebruikt als sleutels, terwijl de celwaarden worden gegenereerd als JSON-waarden:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

De resultatenset wordt opgemaakt als een JSON-matrix waarbij elke rij is opgemaakt als een afzonderlijk JSON-object.

PAD geeft aan dat u de indeling van de uitvoer van uw JSON-resultaat met puntnotatie in Kolomaliassen kunt aanpassen. De volgende query wordt de naam van de sleutel "Klantnaam" in de uitvoer-JSON-indeling en phone-en faxnummer plaatst in het onderliggende object 'Neem contact op met':

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

De uitvoer van deze query ziet er zo uit:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

In dit voorbeeld wordt een JSON-object in plaats van een matrix geretourneerd door op te geven de [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) optie. U kunt deze optie gebruiken als u weet dat u een enkel object als gevolg van de query wilt retourneren.

De belangrijkste waarde van de FOR JSON-component is dat Hiermee kunt u complexe hiërarchische gegevens geretourneerd uit de database die is opgemaakt als een geneste JSON-objecten of -matrices. Het volgende voorbeeld laat zien hoe om op te nemen van de rijen uit de `Orders` tabel die deel uitmaken van de `Customer` als een geneste matrix met `Orders`:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

In plaats van afzonderlijke query's waarmee gegevens van de klant en vervolgens om op te halen een lijst van gerelateerde Orders, krijgt u de benodigde gegevens met een eenvoudige query uitvoeren, zoals wordt weergegeven in de volgende voorbeelduitvoer:

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
Als u geen strikt gestructureerde gegevens, hebt u complexe onderliggende objecten, matrices of hiërarchische gegevens, of als uw gegevensstructuren verloop van tijd, kan de JSON-indeling u helpen te vertegenwoordigen de structuur van een complexe gegevens.

JSON is een tekstuele indeling die kan worden gebruikt als een andere tekenreekstype in Azure SQL Database. U kunt verzenden of JSON-gegevens worden opgeslagen als een standaard NVARCHAR:

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

De JSON-gegevens die in dit voorbeeld wordt weergegeven met behulp van het type NVARCHAR(MAX). JSON kan worden ingevoegd in deze tabel of opgegeven als een argument van de opgeslagen procedure met behulp van standaard Transact-SQL-syntaxis zoals weergegeven in het volgende voorbeeld:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Elke client-side-taal of bibliotheek die met het gegevenstype in Azure SQL Database werkt werkt ook met JSON-gegevens. JSON kan worden opgeslagen in een tabel die ondersteuning biedt voor het type NVARCHAR, zoals een tabel geoptimaliseerd voor geheugen of een systeemversietabel. JSON heeft niet tot een beperking in de client-side-code of in de databaselaag.

## <a name="querying-json-data"></a>Een query uitvoeren op JSON-gegevens
Hebt u gegevens die zijn opgemaakt als JSON die zijn opgeslagen in Azure SQL-tabellen, kunnen JSON-functies u deze gegevens in een SQL-query.

JSON-functies die beschikbaar in Azure SQL database kunt zijn u gegevens die zijn opgemaakt als JSON als een ander SQL-gegevenstype behandelen. U kunt eenvoudig waarden ophalen uit de JSON-tekst en JSON-gegevens in elke query gebruiken:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

De functie JSON_VALUE haalt een waarde uit die zijn opgeslagen in de kolom in de JSON-tekst. Deze functie maakt gebruik van een JavaScript-achtige pad om te verwijzen naar een waarde in JSON-tekst om op te halen. De opgehaalde waarde kan worden gebruikt in een deel van de SQL-query.

De functie JSON_QUERY is vergelijkbaar met JSON_VALUE. In tegenstelling tot JSON_VALUE pakt deze functie complexe subobject zoals matrices of objecten die worden geplaatst in JSON-tekst.

De functie JSON_MODIFY kunt u het pad van de waarde opgeven in de JSON-tekst die moet worden bijgewerkt, evenals een nieuwe waarde die de oude versie wordt overschreven. Op deze manier die kunt u eenvoudig JSON-tekst zonder de volledige structuur reparsing bijwerken.

Omdat JSON worden opgeslagen in een standaardtekst, zijn er geen garanties dat de waarden die zijn opgeslagen in tekstkolommen correct zijn opgemaakt. U kunt controleren of de tekst die is opgeslagen in JSON-kolom is correct opgemaakt met behulp van standaard check-beperkingen voor Azure SQL Database en de functie ISJSON:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Als de invoertekst is juist opgemaakt JSON, de functie ISJSON retourneert de waarde 1. Deze beperking wordt bij elke invoegen of bijwerken van JSON-kolom of nieuwe tekstwaarde is onjuist gevormd JSON.

## <a name="transforming-json-into-tabular-format"></a>JSON transformeren in tabelvorm
Azure SQL-Database kunt u JSON-verzamelingen in tabelvorm indeling en belasting of query JSON-gegevens te transformeren.

OPENJSON is een tabelwaarde functie die wordt geparseerd JSON-tekst, wordt gezocht naar een matrix met JSON-objecten, doorloopt de elementen van de matrix en retourneert één rij in het resultaat van de uitvoer voor elk element van de matrix.

![JSON in tabelvorm](./media/sql-database-json-features/image_2.png)

In het bovenstaande voorbeeld kunnen we opgeven waar u de JSON-matrix die moet worden geopend (in de $. Orders pad), welke kolommen moeten worden geretourneerd als resultaat en waar u kunt de JSON-waarden die worden geretourneerd als cellen vinden.

We kunt gebruiken om een JSON-matrix in de @orders variabele in een set van rijen, deze resultatenset analyseren of invoegen van rijen in een standard tabel:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    FROM OPENJSON (@orders)
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
Voor informatie over het integreren van JSON in uw toepassing, kunt u deze resources:

* [TechNet Blog](https://blogs.technet.microsoft.com/dataplatforminsider/20../../json-in-sql-server-2016-part-1-of-4/)
* [MSDN-documentatie](https://msdn.microsoft.com/library/dn921897.aspx)
* [Channel 9-video](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Zie voor meer informatie over verschillende scenario's voor het integreren van JSON in uw toepassing, de demo's in deze [Channel 9-video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) of zoeken van een scenario dat overeenkomt met uw use-case in [JSON blogberichten](https://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).

