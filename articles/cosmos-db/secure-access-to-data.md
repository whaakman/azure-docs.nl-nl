---
title: Meer informatie over het beveiligen van toegang tot gegevens in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over de access control-concepten in Azure Cosmos DB, met inbegrip van hoofdsleutels, alleen-lezen-sleutels, gebruikers en machtigingen.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 8641225d-e839-4ba6-a6fd-d6314ae3a51c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.openlocfilehash: 383e04f91eec2f465b381ce30f2d6d24c488b731
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="securing-access-to-azure-cosmos-db-data"></a>Toegang tot Azure DB die Cosmos-gegevens beveiligen
In dit artikel biedt een overzicht van de beveiliging van toegang tot gegevens die zijn opgeslagen in [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Azure Cosmos DB gebruikt twee typen sleutels voor het verifiëren van gebruikers en toegang bieden tot de gegevens en bronnen. 

|sleuteltype|Resources|
|---|---|
|[Hoofdsleutels](#master-keys) |Gebruikt voor administratieve resources: database-accounts, databases, gebruikers en machtigingen|
|[Resource-tokens](#resource-tokens)|Voor toepassingsresources gebruikt: verzamelingen, documenten, bijlagen, opgeslagen procedures, triggers en UDF's|

<a id="master-keys"></a>

## <a name="master-keys"></a>Hoofdsleutels 

Hoofdsleutels bieden toegang tot de alle beheerdersrechten resources voor het account van de database. Hoofdsleutels:  
- Bieden toegang tot accounts, databases, gebruikers en machtigingen. 
- Kan niet worden gebruikt voor gedetailleerde toegang tot de verzamelingen en documenten.
- Tijdens het maken van een account zijn gemaakt.
- Op elk gewenst moment kunnen worden hersteld.

Elk account bestaat uit twee hoofdsleutels: een primaire en secundaire sleutel. Het doel van dubbele sleutels is zodat u kunt opnieuw genereren of sleutels draaien, continue toegang tot uw account en -gegevens. 

Naast de twee hoofdsleutels voor het account van de Cosmos-DB zijn er twee sleutels voor alleen-lezen. Deze sleutels alleen-lezen kunnen alleen leesbewerkingen op het account. Alleen-lezen sleutels bieden geen toegang tot bronnen van de machtigingen lezen.

Primaire, secundaire alleen-lezen en lezen-schrijven hoofdsleutels kunnen worden opgehaald en wordt opnieuw gegenereerd met de Azure portal. Zie voor instructies [weergeven, kopiëren en opnieuw genereren toegangstoetsen](manage-account.md#keys).

![Toegangsbeheer (IAM) in de Azure portal - beveiliging voor NoSQL-database te demonstreren](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

Het proces van het roteren van de hoofdsleutel is eenvoudig. Navigeer naar de Azure-portal en ophalen van de secundaire sleutel vervolgens uw primaire sleutel vervangen door de secundaire sleutel in uw toepassing vervolgens roteren van de primaire sleutel in de Azure-portal.

![Rotatie van de hoofdsleutel in de Azure portal - beveiliging voor NoSQL-database te demonstreren](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-to-use-a-master-key"></a>Voorbeeld van code een hoofdsleutel gebruiken

De volgende voorbeeldcode laat zien hoe een Cosmos-DB account eindpunt en de hoofdsleutel gebruiken om te instantiëren van een DocumentClient en een database maken. 

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

Resource-tokens bieden toegang tot de toepassingsresources in een database. Resource-tokens:
- Toegang tot specifieke verzamelingen, partitiesleutels, documenten, bijlagen, opgeslagen procedures, triggers en UDF's bieden.
- Worden gemaakt wanneer een [gebruiker](#users) is verleend [machtigingen](#permissions) op een specifieke bron.
- Worden opnieuw gemaakt wanneer een resource machtiging wordt gereageerd op door POST of GET PUT-aanroep.
- Een hash-resource-token die specifiek voor de gebruiker, de bron en de machtiging is gebouwd gebruiken.
- Tijdsgebonden met een geldigheidsperiode aanpasbaar zijn. De standaard geldige timespan is een uur. Levensduur van token, maar kan expliciet worden opgegeven, maximaal vijf uur.
- Geef een veilige alternatief voor het geven van de hoofdsleutel. 
- Inschakelen voor clients om te lezen, schrijven en verwijderen van resources in het account van de Cosmos-DB volgens de machtigingen die ze hebt gekregen.

U kunt een resource-token (door het maken van Cosmos-DB-gebruikers en machtigingen) als u wilt toegang bieden tot bronnen in uw account Cosmos-database naar een client die niet kan vertrouwd met de hoofdsleutel worden.  

Cosmos DB resource tokens bieden een veilige alternatief waarmee clients lezen, schrijven en verwijderen van resources in uw account Cosmos DB volgens de machtigingen die u hebt verleend en zonder dat nodig is voor een model- of alleen belangrijke lezen.

Hier volgt een typisch ontwerppatroon waarbij resource tokens kunnen worden aangevraagd, gegenereerd en aan clients geleverd:

1. Een service middelste laag is ingesteld voor het uitvoeren van een mobiele toepassing Gebruikersfoto delen. 
2. De middelste laag-service beschikt over de hoofdsleutel van de Cosmos-DB-account.
3. De foto-app is geïnstalleerd op mobiele apparaten van eindgebruikers. 
4. Aanmelden stelt u de app foto de identiteit van de gebruiker met de service voor de middelste laag. Dit mechanisme van tot stand brengen van de identiteit is uitsluitend tot de toepassing.
5. Wanneer de identiteit is gemaakt, serviceaanvragen de middelste laag machtigingen op basis van de identiteit.
6. De middelste laag service verzendt een resource-token terug naar de telefoonapp.
7. De telefoonapp kunt blijven gebruiken van de resource-token rechtstreeks toegang krijgen tot bronnen van de Cosmos-database met de machtigingen die zijn gedefinieerd door de resource-token en voor het interval dat is toegestaan door de resource-token. 
8. Wanneer de bron-token is verlopen, ontvangt de volgende aanvragen een 401-niet-geautoriseerde uitzondering.  Op dit moment de telefoonapp herstelt de identiteit en vraagt een token van een nieuwe resource.

    ![Azure DB Cosmos resource tokens werkstroom](./media/secure-access-to-data/resourcekeyworkflow.png)

Resource token genereren en beheren wordt verwerkt door de systeemeigen Cosmos DB clientbibliotheken; Als u gebruikmaakt van REST moet u de aanvraag-/ verificatieheaders opgeven. Zie voor meer informatie over het maken van verificatieheaders voor REST [toegangsbeheer voor bronnen voor Cosmos DB](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) of de [broncode voor onze SDK's](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).

Zie voor een voorbeeld van een van de middelste laag-service gebruikt voor het genereren of resource tokens broker de [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

<a id="users"></a>

## <a name="users"></a>Gebruikers
Cosmos DB gebruikers zijn gekoppeld aan een Cosmos-DB-database.  Elke database kan nul of meer Cosmos-DB-gebruikers bevatten.  De volgende voorbeeldcode laat zien hoe een Cosmos-DB gebruikersbron maken.

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> Elke gebruiker Cosmos DB heeft een PermissionsLink-eigenschap die kan worden gebruikt voor het ophalen van de lijst met [machtigingen](#permissions) die zijn gekoppeld aan de gebruiker.
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a>Machtigingen
Een Cosmos-DB machtiging-bron is gekoppeld aan een Cosmos-DB-gebruiker.  Elke gebruiker kan nul of meer Cosmos DB machtigingen bevatten.  Een machtiging resource biedt toegang tot een beveiligingstoken dat de gebruiker moet wanneer u probeert te krijgen tot de bron van een specifieke toepassing.
Er zijn twee beschikbare toegangsniveaus die door een resource machtiging worden verleend:

* Alle: De gebruiker heeft volledige machtigingen voor de bron.
* Lees: De gebruiker de inhoud van de resource kan alleen worden gelezen, maar schrijven, update of delete-bewerkingen op de bron kan niet worden uitgevoerd.

> [!NOTE]
> Opgeslagen procedures die de gebruiker moet de machtiging All hebben voor de verzameling waarin u de opgeslagen procedure wordt uitgevoerd om te kunnen uitvoeren van de Cosmos-DB.
> 
> 

### <a name="code-sample-to-create-permission"></a>Voorbeeld van code voor het maken van de machtiging

De volgende voorbeeldcode laat zien hoe een machtiging resource maken en koppelen van de machtigingen met het resource-token van de bron van de machtiging lezen de [gebruiker](#users) die eerder is gemaakt.

```csharp
// Create a permission.
Permission docPermission = new Permission
{
    PermissionMode = PermissionMode.Read,
    ResourceLink = documentCollection.SelfLink,
    Id = "readperm"
};
  
docPermission = await client.CreatePermissionAsync(UriFactory.CreateUserUri("db", "user"), docPermission);
Console.WriteLine(docPermission.Id + " has token of: " + docPermission.Token);
```

Als u een partitiesleutel voor uw verzameling hebt opgegeven, moet klikt u vervolgens de machtiging voor het verzamelen, document en bijlage resources ook de ResourcePartitionKey naast de ResourceLink.

### <a name="code-sample-to-read-permissions-for-user"></a>Voorbeeld van code lezen machtigingen voor gebruiker

Eenvoudig alle om toestemming te krijgen resources die zijn gekoppeld aan een bepaalde gebruiker, Cosmos DB beschikbaar stelt een machtiging voor elk gebruikersobject feed.  Het volgende codefragment laat zien hoe ophalen van de machtiging die is gekoppeld aan de gebruiker die eerder is gemaakt, maken een lijst met machtigingen en exemplaar maken van een nieuwe DocumentClient namens de gebruiker.

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
* Zie voor meer informatie over de beveiliging van de Cosmos-DB-database, [Cosmos DB: beveiliging van de Database](database-security.md).
* Zie voor meer informatie over het beheer van hoofd- en alleen-lezen-sleutels, [het beheren van een account voor Azure Cosmos DB](manage-account.md#keys).
* Zie voor meer informatie over het maken van Azure DB die Cosmos autorisatie tokens, [toegangsbeheer op Azure Cosmos DB bronnen](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).
