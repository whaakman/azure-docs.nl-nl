---
title: "Azure Active Directory gebruiken om te verifiëren van Azure Batch serviceoplossingen | Microsoft Docs"
description: Batch ondersteunt Azure AD voor de verificatie van de Batch-service.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/20/2017
ms.author: tamram
ms.openlocfilehash: 9c03bde919c46cd301229255c0b12ee69dda6f78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a>Verificatie van oplossingen voor Batch-service met Active Directory

Azure Batch biedt ondersteuning voor verificatie met [Azure Active Directory] [ aad_about] (Azure AD). Azure AD is Microsoft multitenant-cloudgebaseerde adreslijst en identity management-service. Azure zelf Azure AD gebruikt voor verificatie van zijn klanten, servicebeheerders en organisatie-gebruikers.

Als u Azure AD-verificatie met Azure Batch, kunt u verifiëren op twee manieren:

- Met behulp van **geïntegreerde verificatie** een gebruiker die de interactie is met de toepassing te verifiëren. Een toepassing met geïntegreerde verificatie van de gebruiker referenties moeten worden verzameld en gebruikt deze referenties voor het verifiëren van toegang tot de Batch-resources.
- Met behulp van een **service-principal** om te verifiëren van een toepassing zonder toezicht. Een service-principal definieert het beleid en de machtigingen voor een toepassing om de toepassing te vertegenwoordigen wanneer toegang tot bronnen tijdens runtime.

Zie voor meer informatie over Azure AD, de [Azure Active Directory-documentatie](https://docs.microsoft.com/azure/active-directory/).

## <a name="authentication-and-pool-allocation-mode"></a>Verificatie- en groep toewijzing-modus

Wanneer u een Batch-account maakt, kunt u opgeven waar de groepen die zijn gemaakt voor dat account moeten worden toegewezen. U kunt groepen in het standaard Batch-service-abonnement of in een gebruikerabonnement toegewezen. Uw keuze is van invloed op hoe u toegang tot bronnen in het account te verifiëren.

- **Batch-service-abonnement**. Batch-pools worden standaard toegewezen in een Batch-service-abonnement. Als u deze optie kiest, u toegang tot bronnen in het account kunt verifiëren met [gedeelde sleutel](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) of met Azure AD.
- **Gebruikerabonnement.** U kunt kiezen om toe te wijzen Batch-pools in een gebruikerabonnement die u opgeeft. Als u deze optie kiest, moet u verifiëren met Azure AD.

## <a name="endpoints-for-authentication"></a>Eindpunten voor verificatie

Om te verifiëren Batch-toepassingen met Azure AD, moet u een aantal bekende eindpunten opnemen in uw code.

### <a name="azure-ad-endpoint"></a>Azure AD-eindpunt

De base Azure AD authority-eindpunt is:

`https://login.microsoftonline.com/`

Om te verifiëren met Azure AD, moet u dit eindpunt samen met de tenant-ID (directory-ID) gebruiken. De tenant-ID identificeert de Azure AD-tenant wilt gebruiken voor verificatie. Volg de stappen in voor het ophalen van de tenant-ID, [de tenant-ID niet ophalen voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> Het eindpunt tenantspecifieke is vereist wanneer u verifiëren met behulp van een service-principal. 
> 
> Het eindpunt tenantspecifieke is optioneel wanneer u verifiëren met behulp van geïntegreerde verificatie, maar aanbevolen. U kunt echter ook het Azure AD gemeenschappelijk eindpunt gebruiken. Het algemene eindpunt biedt een algemene referentie interface verzamelen als een specifieke tenant is niet opgegeven. Het eindpunt van de algemene heeft `https://login.microsoftonline.com/common`.
>
>

Zie voor meer informatie over Azure AD-eindpunten [verificatie scenario's voor Azure AD][aad_auth_scenarios].

### <a name="batch-resource-endpoint"></a>Eindpunt voor batch-resource

Gebruik de **endpoint van Azure Batch-resource** te verkrijgen van een token voor het verifiëren van aanvragen voor de Batch-service:

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a>Uw toepassing registreren met een tenant

De eerste stap bij het gebruik van Azure AD om te verifiëren, is uw toepassing registreren in een Azure AD-tenant. Registreren van uw toepassing, kunt u de Azure aanroepen [Active Directory Authentication Library] [ aad_adal] (ADAL) vanuit uw code. De ADAL biedt een API voor de verificatie met Azure AD van uw toepassing. Registreren van uw toepassing is vereist of u van plan bent om geïntegreerde verificatie of een service-principal te gebruiken.

Wanneer u uw toepassing registreert, kunt u informatie opgeven over uw toepassing naar Azure AD. Vervolgens Azure AD levert een toepassings-ID waarmee u uw toepassing koppelen aan Azure AD tijdens runtime. Zie voor meer informatie over de toepassings-ID, [toepassing en service-principal objecten in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).

Voor het registreren van uw Batch-toepassing, volg de stappen in de [een toepassing toe te voegen](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) in sectie [toepassingen integreren met Azure Active Directory][aad_integrate]. Als u uw toepassing als een systeemeigen toepassing registreren, kunt u een geldige URI voor de **omleidings-URI**. Dit hoeft niet te worden van een echte-eindpunt.

Nadat u uw toepassing hebt geregistreerd, ziet u de toepassings-ID:

![De Batch-toepassing registreren met Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

Zie voor meer informatie over het registreren van een toepassing met Azure AD [verificatie scenario's voor Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).

## <a name="get-the-tenant-id-for-your-active-directory"></a>De tenant-ID niet ophalen voor uw Active Directory

De tenant-ID identificeert de Azure AD-tenant waarmee verificatieservices voor uw toepassing. Als u de tenant-ID, de volgende stappen uit:

1. Selecteer uw Active Directory in de Azure-portal.
2. Klik op **Eigenschappen**.
3. Kopieer de GUID-waarde opgegeven voor de map-ID. Deze waarde wordt ook aangeroepen voor de tenant-ID.

![Kopieer de map-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a>Gebruik van geïntegreerde verificatie

Om te verifiëren met geïntegreerde verificatie, moet u de machtigingen van uw toepassing verbinding maken met de API van Batch-service. Deze stap kunt uw toepassing om te verifiëren van aanroepen naar de API van Batch-service met Azure AD.

Zodra u hebt [geregistreerd van uw toepassing](#register-your-application-with-an-azure-ad-tenant), als volgt te werk in de Azure portal naar het toegang geven tot de Batch-service:

1. Kies in het navigatiedeelvenster links van de Azure portal, **meer Services**, klikt u op **App registraties**.
2. Zoeken naar de naam van uw toepassing in de lijst van app-rapporten:

    ![Zoeken naar de toepassingsnaam van uw](./media/batch-aad-auth/search-app-registration.png)

3. Open de **instellingen** blade voor uw toepassing. In de **API-toegang** sectie **vereist machtigingen**.
4. In de **vereist machtigingen** blade, klikt u op de **toevoegen** knop.
5. In stap 1, zoekt u de Batch-API. Zoek deze tekenreeksen totdat u de API hebt gevonden:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Nieuwere Azure AD-tenants kunnen deze naam gebruiken.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** is de id voor de Batch-API. 
6. Als u de Batch-API hebt gevonden, selecteert u deze en klikt u op de **Selecteer** knop.
6. In stap 2, schakel het selectievakje in naast **Access Azure Batch-Service** en klik op de **Selecteer** knop.
7. Klik op de **gedaan** knop.

De **Required Permissions** blade nu service bevat die uw Azure AD-toepassing toegang tot zowel ADAL als de Batch heeft-API. Machtigingen worden verleend aan ADAL automatisch wanneer u uw app eerst bij Azure AD registreren.

![Machtigingen verlenen API](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a>Gebruik van een service-principal 

Verificatie van een toepassing die wordt uitgevoerd zonder toezicht, moet u een service-principal gebruiken. Nadat u uw toepassing hebt geregistreerd, volg deze stappen in de Azure portal voor het configureren van een service-principal:

1. Vraagt u een geheime sleutel voor uw toepassing.
2. RBAC-rol toewijzen aan uw toepassing.

### <a name="request-a-secret-key-for-your-application"></a>Vraagt u een geheime sleutel voor uw toepassing

Als uw toepassing zich met een service-principal verifieert, worden zowel de toepassings-ID en een geheime sleutel naar Azure AD. U moet maken en kopieer de geheime sleutel van uw code te gebruiken.

Volg deze stappen in de Azure portal:

1. Kies in het navigatiedeelvenster links van de Azure portal, **meer Services**, klikt u op **App registraties**.
2. Zoek de naam van uw toepassing in de lijst van app-registraties.
3. Weergave de **instellingen** blade. In de **API-toegang** sectie **sleutels**.
4. Voer een beschrijving voor de sleutel voor het maken van een sleutel. Selecteer vervolgens een duur voor de sleutel van één of twee jaar. 
5. Klik op de **opslaan** knop maken en weergeven van de sleutel. De sleutelwaarde kopiëren naar een veilige plaats als het niet mogelijk toegang tot het opnieuw nadat u de blade laat. 

    ![Maken van een geheime sleutel](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-to-your-application"></a>Een RBAC-rol toewijzen aan uw toepassing

Om te verifiëren met een service-principal, moet u een RBAC-rol toewijzen aan uw toepassing. Volg deze stappen:

1. Ga in de Azure-portal naar het Batch-account wordt gebruikt door uw toepassing.
2. In de **instellingen** blade voor het Batch-account, selecteer **Access Control (IAM)**.
3. Klik op de **toevoegen** knop. 
4. Van de **rol** vervolgkeuzelijst, kiest u de _Inzender_ of _lezer_ rol voor uw toepassing. Zie voor meer informatie over deze rollen [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../active-directory/role-based-access-control-what-is.md).  
5. In de **Selecteer** en voer de naam van uw toepassing. Selecteer uw toepassing uit de lijst en klikt u op **opslaan**.

Uw toepassing wordt nu weergegeven in de instellingen voor toegangsbeheer met een RBAC-rol die is toegewezen. 

![Een RBAC-rol toewijzen aan uw toepassing](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>De tenant-ID niet ophalen voor uw Azure Active Directory

De tenant-ID identificeert de Azure AD-tenant waarmee verificatieservices voor uw toepassing. Als u de tenant-ID, de volgende stappen uit:

1. Selecteer uw Active Directory in de Azure-portal.
2. Klik op **Eigenschappen**.
3. Kopieer de GUID-waarde opgegeven voor de map-ID. Deze waarde wordt ook aangeroepen voor de tenant-ID.

![Kopieer de map-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a>Codevoorbeelden

De codevoorbeelden in deze sectie laten zien hoe om te verifiëren met Azure AD dat gebruikmaakt van geïntegreerde verificatie en met een service-principal. Deze codevoorbeelden .NET gebruiken, maar de concepten zijn vergelijkbaar voor andere talen.

> [!NOTE]
> Een Azure AD authentication-token verloopt na één uur. Wanneer u een lange levensduur **BatchClient** object, het is raadzaam dat u een token van ADAL bij elke aanvraag om te controleren of er altijd een geldig token ophalen. 
>
>
> Om dit te bereiken in .NET, schrijven van een methode die het token opgehaald uit Azure AD en geeft deze methode een **BatchTokenCredentials** object als een gemachtigde. De gemachtigdenmethode wordt aangeroepen bij elke aanvraag naar de Batch-service om ervoor te zorgen dat een geldig token moet worden verstrekt. Standaard ADAL in de cache opgeslagen tokens, zodat een nieuw token is opgehaald uit Azure AD alleen indien nodig. Zie voor meer informatie over tokens in Azure AD [verificatie scenario's voor Azure AD][aad_auth_scenarios].
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a>Voorbeeld: met behulp van Azure AD geïntegreerde verificatie met Batch .NET

Om te verifiëren met geïntegreerde verificatie van de Batch .NET, verwijst naar de [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) pakket en de [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) pakket.

De volgende `using` instructies in uw code:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Verwijzing naar de Azure AD-eindpunt in uw code, met inbegrip van de tenant-ID. Volg de stappen in voor het ophalen van de tenant-ID, [de tenant-ID niet ophalen voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Verwijzen naar het eindpunt van Batch-service resource:

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Verwijzen naar uw Batch-account:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Geef de toepassings-ID (client-ID) voor uw toepassing. De toepassings-ID is beschikbaar via de registratie van uw app in de Azure-portal:

```csharp
private const string ClientId = "<application-id>";
```

De omleidings-URI die u hebt opgegeven tijdens het registratieproces ook kopiëren. Omleidings-URI in uw code opgegeven de moet overeenkomen met de omleidings-URI die u hebt opgegeven bij de registratie van de toepassing:

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

Schrijven van een callback-methode te verkrijgen van het verificatietoken van Azure AD. De **GetAuthenticationTokenAsync** retouraanroepmethode aanroepen ADAL voor verificatie van een gebruiker die de interactie is met de toepassing hier weergegeven. De **AcquireTokenAsync** methode geleverd door ADAL vraagt de gebruiker om hun referenties en de toepassing voortgezet nadat de gebruiker deze heeft (tenzij deze al in de cache referenties opgeslagen is):

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire the authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

Samen een **BatchTokenCredentials** object waarmee de gemachtigde als parameter. Deze referenties gebruiken om te openen een **BatchClient** object. U kunt die **BatchClient** object voor verdere bewerkingen op basis van de Batch-service:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a>Voorbeeld: met behulp van een Azure AD-service-principal met Batch .NET

Om te verifiëren met een service-principal in Batch .NET, verwijst naar de [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) pakket en de [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) pakket.

De volgende `using` instructies in uw code:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Verwijzing naar de Azure AD-eindpunt in uw code, met inbegrip van de tenant-ID. Wanneer u een service-principal gebruikt, moet u een eindpunt tenantspecifieke opgeven. Volg de stappen in voor het ophalen van de tenant-ID, [de tenant-ID niet ophalen voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Verwijzen naar het eindpunt van Batch-service resource:  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Verwijzen naar uw Batch-account:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Geef de toepassings-ID (client-ID) voor uw toepassing. De toepassings-ID is beschikbaar via de registratie van uw app in de Azure-portal:

```csharp
private const string ClientId = "<application-id>";
```

Geef de geheime sleutel die u hebt gekopieerd uit de Azure-portal:

```csharp
private const string ClientKey = "<secret-key>";
```

Schrijven van een callback-methode te verkrijgen van het verificatietoken van Azure AD. De **GetAuthenticationTokenAsync** retouraanroepmethode aanroepen ADAL voor verificatie zonder toezicht hier weergegeven:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

Samen een **BatchTokenCredentials** object waarmee de gemachtigde als parameter. Deze referenties gebruiken om te openen een **BatchClient** object. Vervolgens kunt u die **BatchClient** object voor verdere bewerkingen op basis van de Batch-service:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure AD, de [Azure Active Directory-documentatie](https://docs.microsoft.com/azure/active-directory/). Gedetailleerde voorbeelden ziet u het gebruik van ADAL zijn beschikbaar in de [Azure codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotheek.

Zie voor meer informatie over service-principals, [toepassing en service-principal objecten in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md). Zie het maken van een service-principal met de Azure portal [portal gebruik maken van Active Directory-toepassing en service-principal die toegang bronnen tot](../resource-group-create-service-principal-portal.md). U kunt ook een service-principal maken met PowerShell of Azure CLI. 

Om te verifiëren met behulp van Azure AD Batch beheertoepassingen, Zie [oplossingen voor het beheer van Batch verifiëren met Active Directory](batch-aad-auth-management.md). 

[aad_about]: ../active-directory/active-directory-whatis.md "Wat is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Verificatie-scenario's voor Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Toepassingen integreren met Azure Active Directory"
[azure_portal]: http://portal.azure.com
