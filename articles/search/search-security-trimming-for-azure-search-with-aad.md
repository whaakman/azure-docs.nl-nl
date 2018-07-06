---
title: Beveiligingsfilters voor Azure Search-resultaten met behulp van Active Directory-identiteiten trimming | Microsoft Docs
description: Toegangsbeheer voor Azure Search-inhoud met beveiligingsfilters en Active Directory-identiteiten.
author: revitalbarletz
manager: jlembicz
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/07/2017
ms.author: revitalb
ms.openlocfilehash: 75017a1a3a400ca5390210225f26a6c5f3bb7c47
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856161"
---
# <a name="security-filters-for-trimming-azure-search-results-using-active-directory-identities"></a>Beveiligingsfilters voor Azure Search-resultaten met behulp van Active Directory-identiteiten bijsnijden

In dit artikel laat zien hoe u Azure Active Directory (AAD) om beveiligingsidentiteiten te gebruiken, samen met de filters in Azure Search zoekresultaten op basis van groepslidmaatschap van de gebruiker knippen.

Dit artikel behandelt de volgende taken:
> [!div class="checklist"]
- AAD-groepen en gebruikers maken
- De gebruiker koppelen aan de groep die u hebt gemaakt
- De nieuwe groep in de cache
- Documenten van de index met de bijbehorende groepen
- Een zoekaanvraag probleem met het filter voor groep-id 's

>[!NOTE]
> De codefragmenten voorbeeld in dit artikel worden geschreven in C#. U vindt de volledige broncode [op GitHub](http://aka.ms/search-dotnet-howto). 

## <a name="prerequisites"></a>Vereisten

Uw Azure Search-index moet hebben een [gebied van beveiliging](search-security-trimming-for-azure-search.md) voor het opslaan van de lijst van groep-id's met leestoegang tot het document. Deze use case wordt ervan uitgegaan dat een-op-eencorrespondentie tussen een beveiligbaar item (zoals een persoon college toepassing) en een gebied van beveiliging op te geven wie toegang heeft tot dat item (Admission personeel).

AAD-beheerdersmachtigingen vereist in dit scenario voor het maken van gebruikers, groepen en koppelingen in AAD, moet u hebben.

Uw toepassing moet ook worden geregistreerd bij AAD, zoals beschreven in de volgende procedure.

### <a name="register-your-application-with-aad"></a>Uw toepassing registreren bij AAD

Deze stap kan uw toepassing met AAD ten behoeve van accepteer aanmeldingen van gebruikers- en groepsaccounts worden geïntegreerd. Als u geen een AAD-beheerder in uw organisatie bent, moet u mogelijk [een nieuwe tenant maken](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) om uit te voeren van de volgende stappen uit.

1. Ga naar de [ **Portal voor Appregistratie**](https://apps.dev.microsoft.com) >  **geconvergeerde app** > **een app toevoegen**.
2. Voer een naam in voor uw toepassing en klik vervolgens op **maken**. 
3. Selecteer de zojuist geregistreerde toepassing in de pagina Mijn toepassingen.
4. Op de pagina van de registratie van toepassingen > **Platforms** > **Platform toevoegen**, kiest u **Web-API**.
5. Nog steeds op de registratiepagina van de toepassing, gaat u naar > **machtigingen voor Microsoft Graph** > **toevoegen**.
6. Selecteer machtigingen toevoegen de volgende overgedragen machtigingen en klik vervolgens op **OK**:

   + **Directory.ReadWrite.All**
   + **Group.ReadWrite.All**
   + **User.ReadWrite.All**

Microsoft Graph biedt een API die programmatische toegang tot de AAD via een REST-API. De voorbeeldcode voor dit scenario gebruikt de machtigingen voor het aanroepen van de Microsoft Graph-API voor het maken van groepen, gebruikers en koppelingen. De API's worden ook gebruikt om de cache groeps-id's voor het filteren van sneller.

## <a name="create-users-and-groups"></a>Gebruikers en groepen maken

Als u zoeken naar een bestaande toepassing toevoegt, wellicht u bestaande gebruikers- en groeps-id's in AAD. In dit geval kunt u de volgende drie stappen overslaan. 

Als u geen bestaande gebruikers hebt, kunt u echter Microsoft Graph-API's gebruiken de beveiligings-principals maken. De volgende codefragmenten laten zien hoe u id's die waarden voor het gebied van beveiliging in uw Azure Search-index worden te genereren. In onze hypothetische college Admission toepassing, zou dit de beveiligings-id's voor Admission personeel.

Het is mogelijk dat gebruiker en groepslidmaatschap zeer vloeiende, met name in grote organisaties. Code die gebruiker en groep-id's moet vaak genoeg om op te halen wijzigingen in het lidmaatschap van de organisatie worden uitgevoerd. Uw Azure Search-index moet ook een vergelijkbare bijwerken van de planning in overeenstemming met de huidige status van de toegestane gebruikers en bronnen.

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

### <a name="step-4-cache-the-groups-identifiers"></a>Stap 4: De id's van groepen in de Cache
(Optioneel) als u wilt de netwerklatentie beperken, kunt u de cache de gebruikersgroep koppelingen zodat wanneer u een zoekaanvraag verzendt, groepen worden geretourneerd uit de cache opslaan van een retour naar AAD. U kunt gebruiken (AAD Batch-API) [https://developer.microsoft.com/graph/docs/concepts/json_batching] voor het verzenden van een afzonderlijke Http-aanvraag met meerdere gebruikers en het bouwen van de cache.

Microsoft Graph is ontworpen voor het verwerken van een groot aantal aanvragen. Als een groot aantal aanvragen optreedt, wordt in Microsoft Graph de aanvraag met de statuscode HTTP 429 mislukt. Zie voor meer informatie, [Microsoft Graph beperking](https://developer.microsoft.com/graph/docs/concepts/throttling).

## <a name="index-document-with-their-permitted-groups"></a>Index-document met hun toegestane groepen

Querybewerkingen in Azure Search worden uitgevoerd via een Azure Search-index. In deze stap maakt importeert een indexeringsbewerking doorzoekbare gegevens in een index, met inbegrip van de id's die worden gebruikt als beveiligingsfilters voor. 

Azure Search niet verifiëren van gebruikers-id's, of geef logica voor het vaststellen van welke inhoud van een gebruiker is gemachtigd om weer te geven. De use-case voor security trimming wordt geregeld wordt ervan uitgegaan dat u de koppeling tussen een vertrouwelijk document en de toegang tot het document, intact geïmporteerd in een search-index-id opgeven. 

In het voorbeeld hypothetische omvat de hoofdtekst van de PUT-aanvraag voor een Azure Search-index van een aanvrager college Open of transcript samen met de groeps-id dat de machtiging om deze inhoud weer te geven. 

In het algemene voorbeeld in het codevoorbeeld gebruikt voor dit scenario wordt de index-actie kan er als volgt uitzien:

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

Om veiligheidsredenen inkorten zijn de waarden in het gebied van beveiliging in de index statische waarden gebruikt voor het opnemen of uitsluiten van documenten in de zoekresultaten. Bijvoorbeeld, als de groeps-id voor Admission "A11B22C33D44 E55F66G77 H88I99JKK" is, zijn alle documenten in een Azure Search-index met deze id in de beveiliging opgeslagen opgenomen (of uitgesloten) in de lijst met zoekresultaten verzonden terug naar de aanvrager.

Als u wilt filteren geretourneerde documenten in de zoekresultaten op basis van groepen van de gebruiker de aanvraag uitgeven, bekijk de volgende stappen.

### <a name="step-1-retrieve-users-group-identifiers"></a>Stap 1: Ophalen van de gebruiker groeps-id 's

Als de gebruikersgroepen zijn niet al in de cache of de cache is verlopen, geven de [groepen](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/directoryobject_getmembergroups) aanvraag
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

### <a name="step-2-compose-the-search-request"></a>Stap 2: De search-aanvraag opstellen

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
### <a name="step-3-handle-the-results"></a>Stap 3: De resultaten worden gestuurd

Het antwoord bevat een gefilterde lijst van documenten, die bestaan uit die de gebruiker is gemachtigd om weer te geven. Afhankelijk van hoe u de pagina met zoekresultaten samenstelt, is het raadzaam om op te nemen van visuele aanwijzingen om de gefilterde resultatenset weer te geven.

## <a name="conclusion"></a>Conclusie

In dit scenario, hebt u geleerd technieken voor het gebruik van AAD-aanmeldingen om documenten in Azure Search-resultaten te filteren bijsnijden van de resultaten van de documenten die niet overeenkomen met het filter dat is opgegeven in de aanvraag.

## <a name="see-also"></a>Zie ook

+ [Identiteit gebaseerd toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search.md)
+ [Filters in Azure Search](search-filters.md)
+ [Beveiligings- en toegangsbeheer in Azure Search-bewerkingen](search-security-overview.md)
