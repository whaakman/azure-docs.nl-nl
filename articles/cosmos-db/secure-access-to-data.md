---
title: Meer informatie over het beveiligen van toegang tot gegevens in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over concepten voor toegangsbeheer in Azure Cosmos DB, met inbegrip van hoofdsleutels, alleen-lezensleutels, gebruikers en machtigingen.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2017
ms.author: sngun
ms.openlocfilehash: eddfce08711043f81cee0b1c8d7ee8c6c02f6a45
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858735"
---
# <a name="securing-access-to-azure-cosmos-db-data"></a>Toegang tot Azure Cosmos DB-gegevens beveiligen
In dit artikel biedt een overzicht van het beveiligen van toegang tot gegevens die zijn opgeslagen in [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Azure Cosmos DB gebruikt twee typen sleutels bij het verifiëren van gebruikers en toegang bieden tot de gegevens en resources. 

|Sleuteltype|Resources|
|---|---|
|[Hoofdsleutels](#master-keys) |Gebruikt voor administratieve resources: database-accounts, databases, gebruikers en machtigingen|
|[Resource-tokens](#resource-tokens)|Gebruikt voor toepassingsresources: containers, documenten, bijlagen, opgeslagen procedures, triggers en UDF's|

<a id="master-keys"></a>

## <a name="master-keys"></a>Hoofdsleutels 

Hoofdsleutels bieden toegang tot de alle administratieve resources voor het account van de database. Hoofdsleutels:  
- Toegang tot accounts, databases, gebruikers en machtigingen verlenen. 
- Kan niet worden gebruikt om gedetailleerde toegang bieden tot containers en documenten.
- Tijdens het maken van een account zijn gemaakt.
- Op elk gewenst moment kunnen worden hersteld.

Elk account bestaat uit twee hoofdsleutels: een primaire en secundaire sleutel. Het doel van dubbele sleutels is zodat u kunt opnieuw genereren of samenvouwen van sleutels, continue toegang tot uw account en gegevens. 

Naast de twee hoofdsleutels voor de Cosmos DB-account zijn er twee sleutels voor alleen-lezen. Deze sleutels alleen-lezen is alleen toegestaan voor leesbewerkingen voor het account. Alleen-lezensleutels geven geen toegang tot resources van de machtigingen lezen.

Primair, secundair, alleen-lezen en sleutels voor lezen / schrijven master kan worden opgehaald en wordt opnieuw gegenereerd met behulp van de Azure portal. Zie voor instructies [weergeven, kopiëren en opnieuw genereren toegangssleutel](manage-account.md#keys).

![Toegangsbeheer (IAM) in Azure portal - beveiliging voor NoSQL-database aan te tonen](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

Het proces voor roulatie van uw hoofdsleutel is eenvoudig. Navigeer naar de Azure-portal naar uw secundaire sleutel ophalen en vervolgens uw primaire sleutel vervangen door uw secundaire sleutel in uw toepassing en draaien de primaire sleutel in Azure portal.

![Rotatie van de hoofdsleutel in de Azure portal - beveiliging voor NoSQL-database aan te tonen](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-to-use-a-master-key"></a>Voorbeeld van code dat een hoofdsleutel gebruiken

Het volgende codevoorbeeld ziet u hoe u een eindpunt van de Cosmos DB-account en de hoofdsleutel gebruiken om te maken van een DocumentClient en maak een database. 

```csharp
//Read the Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from the Azure portal on the Azure Cosmos DB account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access to your DocDB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly SecureString authorizationKey = ToSecureString(ConfigurationManager.AppSettings["AuthorizationKey"]);

client = new DocumentClient(new Uri(endpointUrl), authorizationKey);

// Create Database
Database database = await client.CreateDatabaseAsync(
    new Database
    {
        Id = databaseName
    });
```

<a id="resource-tokens"></a>

## <a name="resource-tokens"></a>Resource-tokens

Brontokens bieden toegang tot de toepassingsresources in een database. Resource-tokens:
- Toegang bieden tot specifieke containers, partitiesleutels, documenten, bijlagen, opgeslagen procedures, triggers en UDF's.
- Worden gemaakt wanneer een [gebruiker](#users) wordt verleend [machtigingen](#permissions) op een specifieke bron.
- Worden opnieuw gemaakt wanneer een resource machtiging wordt gereageerd op door POST, GET of PUT-aanroep.
- Gebruik een hash-resourcetoken speciaal gebouwd voor de gebruiker, de resource en de machtiging.
- Zijn verbonden met een aanpasbare geldigheidsperiode mogelijk is. De standaard geldige tijdsduur is één uur. Levensduur van tokens, maar kan expliciet worden opgegeven, tot een maximum van vijf uur.
- Geef een veilige alternatief voor het geven van de hoofdsleutel. 
- Zodat clients kunnen lezen, schrijven en verwijderen van resources in de Cosmos DB-account volgens de machtigingen die ze hebben gekregen.

U kunt een resourcetoken (door het maken van Cosmos DB-gebruikers en machtigingen) als u wilt toegang bieden tot bronnen in uw Cosmos DB-account naar een client die niet kan vertrouwd met de hoofdsleutel worden.  

Cosmos DB-brontokens bieden een veilige alternatief waarmee clients om te lezen, schrijven en verwijderen van resources in uw Cosmos DB-account volgens de machtigingen die u hebt opgegeven, en zonder de noodzaak voor een model of alleen belangrijke lezen.

Hier volgt een typische ontwerppatroon waarbij brontokens kunnen worden aangevraagd, gegenereerd en die worden geleverd aan clients:

1. Een halverwege tier-service is ingesteld voor het bieden van een mobiele App aan de gebruiker foto's delen. 
2. De middelste laag service beschikt over de hoofdsleutel van de Cosmos DB-account.
3. De foto-app is geïnstalleerd op mobiele apparaten van eindgebruikers. 
4. Op de aanmelding wordt de foto-app de identiteit van de gebruiker met de middelste laag service. Dit mechanisme van inrichting van de identiteit is puur aan de toepassing.
5. Zodra de identiteit van de tot stand is gebracht, vraagt de middelste laag service machtigingen op basis van de identiteit.
6. De middelste laag service verzendt een resourcetoken terug naar de telefoon-app.
7. De telefoon-app kan echter ook doorgaan met het resourcetoken voor rechtstreekse toegang tot Cosmos DB-resources met de machtigingen die zijn gedefinieerd door de resourcetoken en voor het interval dat is toegestaan door de resourcetoken. 
8. Wanneer de resourcetoken is verlopen, wordt elke volgende keer dat een 401-niet-geautoriseerde uitzondering.  Op dit moment de telefoon-app opnieuw wordt de identiteit en een nieuwe resourcetoken aanvraagt.

    ![Azure Cosmos DB-resource-tokens werkstroom](./media/secure-access-to-data/resourcekeyworkflow.png)

Token genereren van bronnen en het beheer wordt verwerkt door de systeemeigen clientbibliotheken voor Cosmos DB; Als u REST gebruiken moet u de aanvraag-/ verificatieheaders samenstellen. Zie voor meer informatie over het maken van verificatieheaders rest [toegangsbeheer op Cosmos DB-Resources](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources) of de [broncode voor onze SDK's](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).

Zie voor een voorbeeld van een middelste laag service gebruikt voor het genereren of brontokens broker, de [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

<a id="users"></a>

## <a name="users"></a>Gebruikers
Cosmos DB-gebruikers zijn gekoppeld aan een Cosmos DB-database.  Elke database kan nul of meer Cosmos DB-gebruikers bevatten.  Het volgende codevoorbeeld laat zien hoe een Cosmos DB gebruiker een resource maken.

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> Elke gebruiker Cosmos DB heeft een eigenschap PermissionsLink die kan worden gebruikt om op te halen van de lijst met [machtigingen](#permissions) die zijn gekoppeld aan de gebruiker.
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a>Machtigingen
Een machtiging Cosmos DB-resource is gekoppeld aan een Cosmos DB-gebruiker.  Elke gebruiker kan nul of meer Cosmos DB-machtigingen bevatten.  Een resource machtiging biedt toegang tot een beveiligingstoken dat de gebruiker moet bij het openen van de bron van een specifieke toepassing.
Er zijn twee toegangsniveaus die kunnen worden geleverd door een resource machtiging:

* Alle: De gebruiker heeft volledige machtigingen voor de resource.
* Lezen: De gebruiker de inhoud van de resource kan alleen worden gelezen, maar kan schrijven, update of delete-bewerkingen op de resource niet uitvoeren.

> [!NOTE]
> Opgeslagen procedures die de gebruiker moet de machtiging All hebben voor de container op waarin de opgeslagen procedure wordt uitgevoerd om uit te voeren van Cosmos DB.
> 
> 

### <a name="code-sample-to-create-permission"></a>Codevoorbeeld om machtiging te maken

Het volgende codevoorbeeld laat zien hoe u een machtiging-resource maken en koppelen van de machtigingen met het resourcetoken van de resource van de machtiging lezen de [gebruiker](#users) hierboven hebt gemaakt.

```csharp
// Create a permission.
Permission docPermission = new Permission
{
    PermissionMode = PermissionMode.Read,
    ResourceLink = UriFactory.CreateDocumentCollectionUri("db", "collection"),
    Id = "readperm"
};
  
docPermission = await client.CreatePermissionAsync(UriFactory.CreateUserUri("db", "user"), docPermission);
Console.WriteLine(docPermission.Id + " has token of: " + docPermission.Token);
```

Als u een partitiesleutel voor uw verzameling hebt opgegeven, moet klikt u vervolgens de machtiging voor de verzameling, document en bronnen van de bijlage ook de ResourcePartitionKey naast de ResourceLink.

### <a name="code-sample-to-read-permissions-for-user"></a>Codevoorbeeld om te lezen-machtigingen voor gebruiker

Eenvoudig alle om toestemming te krijgen resources die zijn gekoppeld aan een bepaalde gebruiker, Cosmos DB beschikbaar stelt een machtiging voor elk gebruikersobject-feed.  Het volgende codefragment toont hoe u ophalen van de machtiging die is gekoppeld aan de gebruiker die eerder is gemaakt, een lijst met machtigingen, en het instantiëren van een nieuwe DocumentClient namens de gebruiker.

```csharp
//Read a permission feed.
FeedResponse<Permission> permFeed = await client.ReadPermissionFeedAsync(
  UriFactory.CreateUserUri("db", "myUser"));
 List<Permission> permList = new List<Permission>();

foreach (Permission perm in permFeed)
{
    permList.Add(perm);
}

DocumentClient userClient = new DocumentClient(new Uri(endpointUrl), permList);
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de beveiliging van de Cosmos DB-database, [Cosmos DB: Database security](database-security.md).
* Zie voor meer informatie over het beheren van hoofd- en alleen-lezensleutels, [over het beheren van een Azure Cosmos DB-account](manage-account.md#keys).
* Zie voor meer het maken van Azure Cosmos DB-autorisatietokens [toegangsbeheer op Azure Cosmos DB-Resources](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources).
