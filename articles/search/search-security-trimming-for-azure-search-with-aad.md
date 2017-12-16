---
title: Beveiligingsfilters voor Azure Search-resultaten gebruik van Active Directory-identiteiten bijsnijden | Microsoft Docs
description: Toegangsbeheer voor Azure Search-inhoud met behulp van de beveiligingsfilters en Active Directory-identiteiten.
services: search
author: revitalbarletz
manager: jlembicz
ms.service: search
ms.topic: article
ms.date: 11/07/2017
ms.author: revitalb
ms.openlocfilehash: 2113b59d6fec15067acbef8b4d4c1fc34c141e62
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="security-filters-for-trimming-azure-search-results-using-active-directory-identities"></a>Beveiligingsfilters voor het gebruik van Active Directory-identiteiten Azure Search-resultaten bijsnijden

In dit artikel laat zien hoe Azure Active Directory (AAD) beveiligingsidentiteiten samen met filters gebruikt in Azure Search zoekresultaten op basis van lidmaatschap van gebruikersgroepen knippen.

In dit artikel bevat informatie over de volgende taken:
> [!div class="checklist"]
- AAD-groepen en gebruikers maken
- De gebruiker koppelen aan de groep die u hebt gemaakt
- De nieuwe groep in de cache
- Documenten van de index met de bijbehorende groepen
- Een zoekopdracht met filter beveiligingsgroep-id's uitgeven

>[!NOTE]
> De codefragmenten voorbeeld in dit artikel worden geschreven in C#. U vindt de volledige broncode [op GitHub](http://aka.ms/search-dotnet-howto). 

## <a name="prerequisites"></a>Vereisten

Uw Azure Search-index moet hebben een [gebied van beveiliging](search-security-trimming-for-azure-search.md) voor het opslaan van de lijst met de identiteit van groep met leestoegang tot het document. Deze gebruiksvoorbeeld wordt ervan uitgegaan dat een-op-een overeenkomst is tussen een beveiligbare item (zoals een persoon universiteit bent toepassing) en een gebied van beveiliging opgeven wie toegang heeft tot dat het item (Admission personeel).

U moet beheerdersmachtigingen AAD, vereist in dit scenario voor het maken van gebruikers, groepen en koppelingen in AAD.

Uw toepassing moet ook zijn geregistreerd bij AAD, zoals beschreven in de volgende procedure.

### <a name="register-your-application-with-aad"></a>Uw toepassing registreren bij AAD

Deze stap integreert uw toepassing met AAD omwille van de aanmeldingen van gebruikers- en groepsaccounts accepteren. Als u niet een AAD-beheerder in uw organisatie bent, moet u mogelijk [maken van een nieuwe tenant](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) de volgende stappen uit te voeren.

1. Ga naar de [ **toepassing Registratieportal**](https://apps.dev.microsoft.com) >  **geconvergeerde app** > **een app toevoegen**.
2. Voer een naam voor uw toepassing en klik vervolgens op **maken**. 
3. Selecteer de zojuist geregistreerde toepassing op de pagina Mijn toepassingen.
4. Op de registratiepagina van de toepassing > **Platforms** > **toevoegen Platform**, kies **Web API**.
5. Nog steeds op de registratiepagina van de toepassing, gaat u naar > **Microsoft Graph machtigingen** > **toevoegen**.
6. Voeg de volgende machtigingen beschikken in machtigingen selecteren en klik vervolgens op **OK**:

   + **Directory.ReadWrite.All**
   + **Group.ReadWrite.All**
   + **User.ReadWrite.All**

Microsoft Graph biedt een API die programmatische toegang tot AAD via een REST-API verleent. De voorbeeldcode voor dit scenario gebruikt de machtigingen voor de Microsoft Graph-API-aanroep voor het maken van groepen, gebruikers en koppelingen. De API's worden ook gebruikt voor cache groeps-id's voor het filteren van sneller.

## <a name="create-users-and-groups"></a>Gebruikers en groepen maken

Als u zoeken naar een bestaande toepassing toevoegt, mogelijk hebt u bestaande gebruikers- en groeps-id's in AAD. In dit geval kunt u de volgende drie stappen overslaan. 

Als u geen bestaande gebruikers hebt, kunt u Microsoft Graph-API's gebruiken de beveiligings-principals maken. De volgende codefragmenten laten zien hoe-id's die gegevenswaarden voor het gebied van beveiliging in uw Azure Search-index worden te genereren. In onze toepassing hypothetische universiteit bent Admission, zou dit beveiligings-id's voor Admission personeel.

Gebruiker en het groepslidmaatschap mogelijk zeer vloeiende, met name in grote organisaties. Code die gebruiker en groep-id's voortbouwt moet vaak genoeg uitvoeren om op te halen wijzigingen in het lidmaatschap van de organisatie. Ook bij uw Azure Search-index moet een updateplanning vergelijkbaar in overeenstemming met de huidige status van de toegestane gebruikers en bronnen.

### <a name="step-1-create-aad-grouphttpsdevelopermicrosoftcomgraphdocsapi-referencev10apigrouppostgroups"></a>Stap 1: Maak [AAD-groep](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/group_post_groups) 
```csharp
// Instantiate graph client 
GraphServiceClient graph = new GraphServiceClient(new DelegateAuthenticationProvider(...));
Group group = new Group()
{
    DisplayName = "My First Prog Group",
    SecurityEnabled = true,
    MailEnabled = false,
    MailNickname = "group1"
}; 
Group newGroup = await graph.Groups.Request().AddAsync(group);
```
   
### <a name="step-2-create-aad-userhttpsdevelopermicrosoftcomgraphdocsapi-referencev10apiuserpostusers"></a>Stap 2: Maak [AAD-gebruiker](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_post_users) 
```csharp
User user = new User()
{
    GivenName = "First User",
    Surname = "User1",
    MailNickname = "User1",
    DisplayName = "First User",
    UserPrincipalName = "User1@FirstUser.com",
    PasswordProfile = new PasswordProfile() { Password = "********" },
    AccountEnabled = true
};
User newUser = await graph.Users.Request().AddAsync(user);
```

### <a name="step-3-associate-user-and-group"></a>Stap 3: Gebruikers en groepen koppelen
```csharp
await graph.Groups[newGroup.Id].Members.References.Request().AddAsync(newUser);
```

### <a name="step-4-cache-the-groups-identifiers"></a>Stap 4: De groepen id's in de Cache
Eventueel om te beperken netwerklatentie, kunt u cache de gebruikersgroep koppelingen zodat wanneer een zoekaanvraag is uitgegeven, groepen worden geretourneerd uit de cache opslaan van een retour in AAD. U kunt gebruiken (AAD Batch-API) [https://developer.microsoft.com/graph/docs/concepts/json_batching] voor het verzenden van één HTTP-aanvraag met meerdere gebruikers en het bouwen van de cache.

Microsoft Graph is ontworpen voor het verwerken van een groot aantal aanvragen. Als een groot aantal aanvragen optreedt, wordt door Microsoft Graph de aanvraag met HTTP-statuscode 429 mislukt. Zie voor meer informatie [Microsoft Graph beperking](https://developer.microsoft.com/graph/docs/concepts/throttling).

## <a name="index-document-with-their-permitted-groups"></a>Index-document met de toegestane groepen

Querybewerkingen in Azure Search worden uitgevoerd via een Azure Search-index. In deze stap importeert een indexeringsbewerking doorzoekbare gegevens in een index, met inbegrip van de id's als beveiligingsfilters gebruikt. 

Azure Search biedt niet verifiëren van gebruikers-id's en logica voor het vaststellen van welke inhoud van een gebruiker is gemachtigd om weer te geven. Het gebruiksvoorbeeld voor beveiliging bijsnijden wordt ervan uitgegaan dat u de koppeling tussen een vertrouwelijk document en de toegang hebben tot dit document, intact geïmporteerd in een search-index-id opgeven. 

In het voorbeeld hypothetische zou de hoofdtekst van de PUT-aanvraag voor een Azure Search-index bevatten een aanvrager universiteit bent Open of de tekst van samen met de groeps-id dat de machtiging om deze inhoud weer te geven. 

In het algemene voorbeeld gebruikt in de voorbeeldcode voor dit scenario wordt de index-actie kan er als volgt uitzien:

```csharp
var actions = new IndexAction<SecuredFiles>[]
              {
                  IndexAction.Upload(
                  new SecuredFiles()
                  {
                      FileId = "1",
                      Name = "secured_file_a",
                      GroupIds = new[] { groups[0] }
                  }),
              ...
             };

var batch = IndexBatch.New(actions);

_indexClient.Documents.Index(batch);  
```

## <a name="issue-a-search-request"></a>Een aanvraag zoeken

Om veiligheidsredenen inkorten zijn de waarden in het gebied van beveiliging in de index statische waarden gebruikt voor het opnemen of uitsluiten van documenten in de zoekresultaten. Bijvoorbeeld, als de id voor Admission 'A11B22C33D44 E55F66G77 H88I99JKK' is, zijn alle documenten in een Azure Search-index met die id in de beveiliging gearchiveerd opgenomen (of uitgesloten) in de zoekresultaten terug naar de aanvrager verzonden.

Als u wilt filteren van documenten die worden geretourneerd in de zoekresultaten op basis van groepen van de gebruiker de aanvraag uitgeven, bekijk de volgende stappen.

### <a name="step-1-retrieve-users-group-identifiers"></a>Stap 1: Ophalen van de gebruiker groeps-id 's

Als de gebruikersgroepen zijn niet al in de cache of de cache is verlopen, geven de [groepen](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/directoryobject_getmembergroups) aanvraag
```csharp
private static void RefreshCacheIfRequired(string user)
{
    if (!_groupsCache.ContainsKey(user))
    {
        var groups = GetGroupIdsForUser(user).Result;
        _groupsCache[user] = groups;
    }
}

private static async Task<List<string>> GetGroupIdsForUser(string userPrincipalName)
{
    List<string> groups = new List<string>();
    var allUserGroupsRequest = graph.Users[userPrincipalName].GetMemberGroups(true).Request();

    while (allUserGroupsRequest != null) 
    {
        IDirectoryObjectGetMemberGroupsRequestBuilder allUserGroups = await allUserGroupsRequest.PostAsync();
        groups = allUserGroups.ToList();
        allUserGroupsRequest = allUserGroups.NextPageRequest;
    }
    return groups;
}
``` 

### <a name="step-2-compose-the-search-request"></a>Stap 2: De zoekaanvraag opstellen

Ervan uitgaande dat u het lidmaatschap van groepen van de gebruiker hebt, kunt u de zoekopdracht met de juiste filterwaarden uitgeven.

```csharp
string filter = String.Format("groupIds/any(p:search.in(p, '{0}'))", string.Join(",", groups.Select(g => g.ToString())));
SearchParameters parameters = new SearchParameters()
             {
                 Filter = filter,
                 Select = new[] { "application essays" }
             };

DocumentSearchResult<SecuredFiles> results = _indexClient.Documents.Search<SecuredFiles>("*", parameters);
```
### <a name="step-3-handle-the-results"></a>Stap 3: De resultaten verwerken

Het antwoord bevat een gefilterde lijst van documenten, die bestaan uit die de gebruiker is gemachtigd om weer te geven. Afhankelijk van hoe u de pagina met zoekresultaten samenstellen, is het raadzaam om op te nemen visuele aanwijzingen om de set gefilterde resultaten weer te geven.

## <a name="conclusion"></a>Conclusie

In dit scenario hebt u geleerd technieken voor het gebruik van AAD aanmeldingen om documenten in Azure Search-resultaten te filteren bijsnijden van de resultaten van documenten die niet overeenkomen met het filter dat is opgegeven voor de aanvraag.

## <a name="see-also"></a>Zie ook

+ [Op basis van identiteit toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search.md)
+ [Filters in Azure Search](search-filters.md)
+ [Beveiliging en toegang beheren in Azure Search-bewerkingen](search-security-overview.md)
