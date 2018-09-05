---
title: Unieke sleutels in Azure Cosmos DB | Microsoft Docs
description: Informatie over het gebruik van unieke sleutels in uw Azure Cosmos DB-database.
services: cosmos-db
keywords: de unieke key-beperking, schending van de unieke key-beperking
author: rafats
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: rafats
ms.openlocfilehash: 5811cb1e08ed5d02038da2a4460ae4b63580833b
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696136"
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Unieke sleutels in Azure Cosmos DB

Unieke sleutels bieden ontwikkelaars de mogelijkheid om een laag van de integriteit van gegevens toevoegen aan de database. Door het maken van een unieke sleutel beleid wanneer u een container maakt, zorgt u ervoor dat een of meer waarden per [partitiesleutel](partition-data.md). Nadat een container is gemaakt met een unieke sleutel beleid, voorkomt u dat het maken van een nieuwe of bijgewerkte items met waarden dat er dubbele waarden die zijn opgegeven door de unieke key-beperking.   

> [!NOTE]
> Unieke sleutels worden ondersteund door de nieuwste versies van de [.NET](sql-api-sdk-dotnet.md) en [.NET Core](sql-api-sdk-dotnet-core.md) SQL SDK's, en de [MongoDB-API](mongodb-feature-support.md#unique-indexes). De tabel-API en de Gremlin-API bieden geen ondersteuning voor unieke sleutels op dit moment. 
> 
>

## <a name="use-case"></a>Use-case

Als voorbeeld gaan we kijken hoe een gebruikersdatabase die zijn gekoppeld aan een [sociale toepassing](use-cases.md#web-and-mobile-applications) kan profiteren van een unieke sleutel beleid dat op e-mailadressen. Door het maken van de gebruiker e-mailadres dat een unieke sleutel, zorgt u ervoor elke record een unieke e-mailadres heeft en er zijn geen nieuwe records kunnen worden gemaakt met dubbele e-mailadressen. 

Als u gebruikers mogelijk te maken wilt meerdere records met dezelfde e-mailadres, maar niet de dezelfde voornaam en achternaam en e-mailadres, u kunt andere paden toevoegen aan het beleid voor unieke sleutels. U kunt dus in plaats van het maken van een unieke sleutel op basis van een e-mailadres, een unieke sleutel die is een combinatie van de voornaam, achternaam en e-mailadres maken. In dit geval elke unieke combinatie van de drie paden is toegestaan, zodat de database kan items met het volgende pad-waarden bevatten. Elk van deze records zou slagen voor het beleid voor unieke sleutels.  

**Toegestane waarden voor unieke sleutel van de voornaam, achternaam en e-mailadres**

|Voornaam|Achternaam|E-mailadres|
|---|---|---|
|Gaby|Duperre|gaby@contoso.com |
|Gaby|Duperre|gaby@fabrikam.com|
|Ivan|Duperre|gaby@fabrikam.com|
|    |Duperre|gaby@fabrikam.com|
|    |       |gaby@fabraikam.com|

Als u een andere record met een van de combinaties die worden vermeld in de bovenstaande tabel invoegen probeert, ontvangt u een foutbericht dat aangeeft dat de unieke key-beperking niet is voldaan. De Azure Cosmos DB geretourneerde fout is "Resource met de opgegeven id of naam bestaat al." of "Resource met de opgegeven id, naam of unieke index al bestaat." 

## <a name="using-unique-keys"></a>Met behulp van unieke sleutels

Unieke sleutels moeten worden gedefinieerd als de container is gemaakt en de unieke sleutel is afgestemd op de partitiesleutel. Als u wilt maken op het vorige voorbeeld, als u op basis van postcode partitioneren, hebt u kunnen de records uit de tabel gedupliceerd in elke partitie.

U kunt bestaande container voor het gebruik van unieke sleutels niet bijwerken.

Nadat een container met een unieke sleutel beleid is gemaakt, kan het beleid kan niet worden gewijzigd, tenzij u de container opnieuw maken. Als u bestaande gegevens die u implementeren unieke sleutels wilt op hebt, de nieuwe container maken en gebruik vervolgens de juiste hulpprogramma voor gegevensmigratie de om gegevens te verplaatsen naar de nieuwe container. Voor SQL-containers, gebruikt u de [hulpprogramma voor gegevensmigratie](import-data.md). Gebruik voor MongoDB-containers, [mongoimport.exe of mongorestore.exe](mongodb-migrate.md).

Maximaal 16 pad waarden (bijvoorbeeld /firstName, /lastName, /address/zipCode, enzovoort) kan worden opgenomen in elke unieke sleutel. 

Het beleid voor elke unieke sleutels kan maximaal 10 unique key-beperkingen of combinaties en de gecombineerde paden voor alle eigenschappen van de unieke index mag niet groter zijn dan 60 tekens. Dus het eerdere voorbeeld die gebruikmaakt van voornaam, achternaam, e-mailadres is slechts één beperking en deze maakt gebruik van drie van de 16 mogelijke paden die beschikbaar zijn. 

Aanvragen eenheid die in rekening worden gebracht voor het maken, bijwerken en verwijderen van een item zijn een enigszins hogere wanneer er een unieke sleutel beleid op de container. 

Sparse unieke sleutels worden niet ondersteund. Als er waarden voor enkele unieke paden ontbreken, worden ze behandeld als een speciale null-waarde deel aan de beperking voor uniekheid neemt.

## <a name="sql-api-sample"></a>SQL-API-voorbeeld

Het volgende codevoorbeeld laat zien hoe u een nieuwe SQL-container maken met twee unique key-beperkingen. De eerste beperking is de voornaam, achternaam, e-beperking die wordt beschreven in het vorige voorbeeld. De tweede beperking is de gebruikers-adres/postcode. Een voorbeeld-JSON-bestand die gebruikmaakt van de paden in dit beleid met unieke sleutels volgt de voorbeeldcode. 

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
                    new UniqueKey { Paths = new Collection<string> { "/address/zipcode" } },
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
    "pk": "1234",
    "firstName": "Gaby",
    "lastName": "Duperre",
    "email": "gaby@contoso.com",
    "address": 
        {            
            "line1": "100 Some Street",
            "line2": "Unit 1",
            "city": "Seattle",
            "state": "WA",
            "zipcode": 98012
        }
    
}
```
> [!NOTE]
> Neem is notitie unieke sleutelnaam hoofdlettergevoelig. Zoals weergegeven in bovenstaande voorbeeld, is de unieke naam voor /address/zipcode ingesteld. Als uw gegevens postcode, wordt klikt u vervolgens ingevoegd unieke sleutel ' null' als postcode niet gelijk aan postcode is. En omdat deze hoofdlettergevoeligheid alle records met ZipCode niet kunnen worden ingevoegd als dubbele 'null' strijdig zijn met de unieke key-beperking.

## <a name="mongodb-api-sample"></a>MongoDB-API-voorbeeld

De volgende opdracht-voorbeeld laat zien hoe een unieke index voor de voornaam, achternaam en e-mailbericht velden van de Gebruikersverzameling voor de MongoDB-API maken. Dit zorgt ervoor dat voor een combinatie van alle drie de velden op alle documenten in de verzameling. Voor MongoDB-API-verzamelingen, wordt de unieke index gemaakt nadat de verzameling is gemaakt, maar voordat het invullen van de verzameling.

> [!NOTE]
> De indeling van de unieke voor MongoDB-API-accounts is die afwijkt van de SQL API-accounts, waarvan u niet hebben om op te geven van de backslash (/) voordat u de veldnaam. 

```
db.users.createIndex( { firstName: 1, lastName: 1, email: 1 }, { unique: true } )
```
## <a name="configure-unique-keys-by-using-azure-portal"></a>Unieke sleutels met behulp van Azure Portal configureren

In de secties boven vindt voorbeelden van code die wordt weergegeven hoe u unique key-beperkingen kunt definiëren als een verzameling is gemaakt met behulp van de SQL-API of de MongoDB-API zijn. Maar het is ook mogelijk om te unieke sleutels definiëren wanneer u een verzameling via de web-UI in Azure portal maakt. 

- Navigeer naar de **Data Explorer** in uw Cosmos DB-account
- Klik op **nieuwe verzameling**
- In de sectie unieke sleutels, ** u kunt de gewenste beperkingen voor unieke sleutels toevoegen door te klikken op **unieke sleutel toevoegen**

![Unieke sleutels definiëren in Data Explorer](./media/unique-keys/unique-keys-azure-portal.png)

- Als u maken van een unieke key-beperking voor het pad lastName wilt, die u toevoegt `/lastName`.
- Als u maken van een unieke key-beperking voor de combinatie van de firstName lastName wilt, toevoegen `/lastName,/firstName`

Klik wanneer klaar **OK** om de verzameling te maken.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u unieke sleutels voor items in een database maken. Als u een container voor het eerst maakt, lees dan [partitioneren van gegevens in Azure Cosmos DB](partition-data.md) als unieke sleutels en partitiesleutels zijn afhankelijk van elkaar. 


