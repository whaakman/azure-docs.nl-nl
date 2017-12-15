---
title: Unieke sleutels in Azure Cosmos DB | Microsoft Docs
description: Informatie over het gebruik van unieke sleutels in uw Azure DB die Cosmos-database.
services: cosmos-db
keywords: Unique key-beperking, schending van unique key-beperking
author: rafats
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b15d5041-22dd-491e-a8d5-a3d18fa6517d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: rafats
ms.openlocfilehash: c530b34edf9bfa0651b7b114dcf7e8add0d906ed
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Unieke sleutels in Azure Cosmos-DB

Unieke sleutels kunnen ontwikkelaars de mogelijkheid een laag van de integriteit van gegevens toevoegen aan de database. Door het maken van een unieke sleutel beleid wanneer een container is gemaakt, zorgt u ervoor de uniekheid van een of meer waarden per [partitiesleutel](partition-data.md). Als een container is gemaakt met een unieke sleutel beleid, voorkomt u dat het maken van alle nieuwe of bijgewerkte items met waarden dat er dubbele waarden die zijn opgegeven door de unique key-beperking.   

> [!NOTE]
> Unieke sleutels worden ondersteund door de nieuwste versies van de [.NET](sql-api-sdk-dotnet.md) en [.NET Core](sql-api-sdk-dotnet-core.md) SQL SDK's, en de [MongoDB API](mongodb-feature-support.md#unique-indexes). De tabel-API en een Graph API bieden geen ondersteuning voor unieke sleutels op dit moment. 
> 
>

## <a name="use-case"></a>Gebruiksvoorbeeld

Als een voorbeeld bekijken we hoe een gebruikersdatabase die zijn gekoppeld aan een [sociale toepassing](use-cases.md#web-and-mobile-applications) kan profiteren van een unieke sleutel beleid dat op e-mailadressen. Door het maken van de gebruiker e-mailadres een unieke sleutel, u elke record heeft een uniek e-mailadres en er worden geen nieuwe records kunnen worden gemaakt met dubbele e-mailadressen. 

Als u gebruikers mogelijk te maken wilt meerdere records met dezelfde e-mailadres, maar niet de dezelfde voornaam, achternaam en het e-mailadres, kunt u andere paden aan het beleid voor unieke sleutels toevoegen. Dus in plaats van een unieke sleutel gewoon op basis van een e-mailadres maken, kunt u een unieke sleutel die is een combinatie van de voornaam, de achternaam en het e-mailbericht. In dit geval wordt elke unieke combinatie van de drie paden is toegestaan, zodat de database kan items bevatten die de volgende waarden voor pad hebben. Elk van deze records zou slagen voor het beleid voor unieke sleutels.  

**Toegestane waarden voor unieke sleutel firstName, lastName en e-mailadres**

|Voornaam|Achternaam|E-mailadres|
|---|---|---|
|Gaby|Duperre|gaby@contoso.com |
|Gaby|Duperre|gaby@fabrikam.com|
|Ivan|Duperre|gaby@fabrikam.com|
|    |Duperre|gaby@fabrikam.com|
|    |       |gaby@fabraikam.com|

Als u hebt geprobeerd een andere record met een van de combinaties die worden vermeld in de bovenstaande tabel invoegen, ontvangt u een foutbericht dat aangeeft dat de unique key-beperking niet is voldaan. De Azure DB die Cosmos retourneert fout is "Resource met opgegeven id of naam bestaat al." of "Resource met de opgegeven id, naam of unieke index bestaat." 

## <a name="using-unique-keys"></a>Met behulp van unieke sleutels

Unieke sleutels moeten worden gedefinieerd als de container wordt gemaakt en de unieke sleutel is afgestemd op de partitiesleutel. Als u wilt maken op het vorige voorbeeld als u op basis van postcode partitioneren, kunt u de records uit de tabel is gedupliceerd in elke partitie kunnen hebben.

Bestaande containers kunnen niet worden bijgewerkt voor het gebruik van unieke sleutels.

Zodra een container is gemaakt met een unieke sleutel beleid, wordt het beleid niet wijzigen tenzij u de container opnieuw maken. Als u bestaande gegevens die u implementeren unieke sleutels wilt op hebt, de nieuwe container maken en vervolgens de juiste hulpprogramma voor gegevensmigratie gebruiken de gegevens worden verplaatst naar de nieuwe container. Voor de SQL-containers, gebruikt u de [hulpprogramma voor gegevensmigratie](import-data.md). Gebruik voor MongoDB-containers [mongoimport.exe of mongorestore.exe](mongodb-migrate.md).

Maximaal 16 pad waarden (bijvoorbeeld /firstName, /lastName, /address/zipCode, enz.) kan worden opgenomen in elke unieke sleutel. 

Het beleid voor elke unieke sleutels kan maximaal 10 unique key-beperkingen of combinaties hebben. Dus het vorige voorbeeld die gebruikmaakt van eerst achternaam, e-mailadres is slechts één beperking en drie van de 16 mogelijke paden die beschikbaar wordt gebruikt. 

Aanvraag eenheid kosten voor het maken, bijwerken en verwijderen van een item zijn enigszins hoger wanneer er een unieke sleutel beleid op de container. 

Sparse unieke sleutels worden niet ondersteund. Als er waarden voor sommige unieke paden ontbreken, worden deze behandeld als een speciale null-waarde deel aan de beperking voor uniekheid neemt.

## <a name="sql-api-sample"></a>SQL-API-voorbeeld

De volgende voorbeeldcode laat zien hoe een nieuwe SQL-container maken met twee unique key-beperkingen. De eerste beperking is de firstName, lastName, e-beperking die wordt beschreven in het vorige voorbeeld. De beperking van de tweede is het adres gebruikers/postcode. Een voorbeeld JSON-bestand dat gebruikmaakt van de paden in dit beleid voor unieke sleutels volgt het codevoorbeeld. 

```csharp
// Create a collection with two separate UniqueKeys, one compound key for /firstName, /lastName,
// and /email, and another for /address/zipCode.
private static async Task CreateCollectionIfNotExistsAsync(string dataBase, string collection)
{
    try
    {
        await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(dataBase, collection));
    }
    catch (DocumentClientException e)
    {
        if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
        {
            DocumentCollection myCollection = new DocumentCollection();
            myCollection.Id = collection;
            myCollection.PartitionKey.Paths.Add("/pk");
            myCollection.UniqueKeyPolicy = new UniqueKeyPolicy
            {
                UniqueKeys =
                new Collection<UniqueKey>
                {
                    new UniqueKey { Paths = new Collection<string> { "/firstName" , "/lastName" , "/email" }}
                    new UniqueKey { Paths = new Collection<string> { "/address/zipCode" } },

                }
            };
            await client.CreateDocumentCollectionAsync(
                UriFactory.CreateDatabaseUri(dataBase),
                myCollection,
                new RequestOptions { OfferThroughput = 2500 });
        }
        else
        {
            throw;
        }
    }
```

Voorbeeld van JSON-document.

```json
{
    "id": "1",
    "firstName": "Gaby",
    "lastName": "Duperre",
    "email": "gaby@contoso.com",
    "address": [
        {            
            "line1": "100 Some Street",
            "line2": "Unit 1",
            "city": "Seattle",
            "state": "WA",
            "zipCode": 98012
        }
    ],
}
```
## <a name="mongodb-api-sample"></a>MongoDB-API-voorbeeld

De volgende opdracht voorbeeld laat zien hoe een unieke index maken op de firstName, lastName, en e-velden van de Gebruikersverzameling voor de MongoDB-API. Hierdoor wordt de uniekheid voor een combinatie van alle drie de velden op alle documenten in de verzameling. Voor MongoDB-API-verzamelingen, wordt de unieke index gemaakt nadat de verzameling is gemaakt, maar voordat de verzameling in te vullen.

```
db.users.createIndex( { firstName: 1, lastName: 1, email: 1 }, { unique: true } )
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe unieke sleutels voor items in een database maken. Als u een container voor het eerst maakt, controleert u [partitioneren van gegevens in Azure Cosmos DB](partition-data.md) als unieke sleutels en partitiesleutels zijn afhankelijk van elkaar. 


