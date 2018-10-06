---
title: Azure Active Directory gebruiken voor verificatie van Azure Batch-service-oplossingen | Microsoft Docs
description: Batch biedt ondersteuning voor Azure AD voor verificatie van de Batch-service.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/18/2018
ms.author: danlep
ms.openlocfilehash: 1347d3aac01c62040c164eee22c7a905b07e8236
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816956"
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a>Verificatie van oplossingen van Batch-service met Active Directory

Azure Batch biedt ondersteuning voor verificatie met [Azure Active Directory] [ aad_about] (Azure AD). Azure AD is van Microsoft met meerdere tenants cloudgebaseerde directory en identity management-service. Azure zelf maakt gebruik van Azure AD om de klanten, servicebeheerders en organisatie-gebruikers te verifiëren.

Wanneer u Azure AD-verificatie met Azure Batch gebruikt, kunt u verifiëren op twee manieren:

- Met behulp van **geïntegreerde verificatie** om een gebruiker die wordt interactie met de toepassing te verifiëren. Een toepassing met behulp van geïntegreerde verificatie van de referenties van een gebruiker worden verzameld en gebruikt deze referenties voor het verifiëren van toegang tot de Batch-resources.
- Met behulp van een **service-principal** om te verifiëren van een toepassing zonder toezicht. Een service-principal definieert het beleid en de machtigingen voor een toepassing zodat de toepassing bij het openen van bronnen tijdens runtime.

Zie voor meer informatie over Azure Active Directory, de [documentatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="endpoints-for-authentication"></a>Eindpunten voor verificatie

Als u wilt verifiëren Batch-toepassingen met Azure AD, moet u enkele bekende eindpunten in uw code opnemen.

### <a name="azure-ad-endpoint"></a>Azure AD-eindpunt

De base Azure AD-instantie-eindpunt:

`https://login.microsoftonline.com/`

Als u wilt verifiëren met Azure AD, moet u dit eindpunt, samen met de tenant-ID (directory-ID) gebruiken. De tenant-ID geeft de Azure AD-tenant te gebruiken voor verificatie. Als u wilt ophalen van de tenant-ID, volg de stappen [ophalen van de tenant-ID voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> Het eindpunt van de tenant-specifieke is vereist wanneer u verifieert met behulp van een service-principal. 
> 
> Het eindpunt van de tenant-specifieke is optioneel wanneer u verifieert met behulp van geïntegreerde verificatie, maar wordt aanbevolen. U kunt echter ook de algemene Azure AD-eindpunt gebruiken. Het eindpunt van de algemene biedt een algemene referentie interface verzamelen wanneer een specifieke tenant is niet opgegeven. Het eindpunt van de algemene is `https://login.microsoftonline.com/common`.
>
>

Zie voor meer informatie over Azure AD-eindpunten [Verificatiescenario's voor Azure AD][aad_auth_scenarios].

### <a name="batch-resource-endpoint"></a>Batch-resource-eindpunt

Gebruik de **Azure Batch-resource-eindpunt** om een token voor het verifiëren van aanvragen voor de Batch-service te verkrijgen:

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a>Uw toepassing registreren bij een tenant

De eerste stap bij het gebruik van Azure AD om te verifiëren, is uw toepassing registreren in een Azure AD-tenant. Registreren van uw toepassing, kunt u voor het aanroepen van de Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) vanuit uw code. De ADAL-bibliotheek biedt een API voor verificatie met Azure AD van uw toepassing. Registreren van uw toepassing is vereist of u van plan bent om geïntegreerde verificatie of een service-principal te gebruiken.

Als u uw toepassing registreert, kunt u informatie opgeven over uw toepassing naar Azure AD. Vervolgens Azure AD biedt een toepassings-ID (ook wel een *client-ID*) waarmee u kunt uw toepassing koppelen aan Azure AD tijdens runtime. Zie voor meer informatie over de toepassings-ID, [toepassing en service-principalobjecten in Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md).

Voor het registreren van uw Batch-toepassing, volg de stappen in de [toevoegen van een toepassing](../active-directory/develop/quickstart-v1-add-azure-ad-app.md) in sectie [toepassingen integreren met Azure Active Directory][aad_integrate]. Als u uw toepassing als een systeemeigen toepassing registreert, kunt u een geldige URI voor de **omleidings-URI**. Het hoeft niet te worden geen echt eindpunt.

Nadat u uw toepassing hebt geregistreerd, ziet u de toepassings-ID:

![Uw Batch-toepassing registreren bij Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

Zie voor meer informatie over het registreren van een toepassing met Azure AD [Verificatiescenario's voor Azure AD](../active-directory/develop/authentication-scenarios.md).

## <a name="get-the-tenant-id-for-your-active-directory"></a>Haal de tenant-ID voor uw Active Directory

De tenant-ID geeft de Azure AD-tenant waarmee verificatieservices aan uw toepassing. Als u de tenant-ID, de volgende stappen uit:

1. Selecteer uw Active Directory in de Azure-portal.
2. Klik op **Eigenschappen**.
3. Kopieer de GUID-waarde opgegeven voor de **map-ID**. Deze waarde wordt ook aangeroepen voor de tenant-ID.

![Kopieer de map-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a>Gebruikmaken van geïntegreerde verificatie

Als u wilt verifiëren met geïntegreerde verificatie, moet u uw toepassing machtigen voor het verbinding maken met de Batch-service-API. Deze stap kan uw toepassing aanroepen naar de Batch-service-API met Azure AD verifiëren.

Nadat u hebt [uw toepassing geregistreerd](#register-your-application-with-an-azure-ad-tenant), als volgt te werk in de Azure portal om het toegang geven tot de Batch-service:

1. Kies in het linker navigatiedeelvenster van de Azure-portal **alle services**. Klik op **App-registraties**.
2. Zoeken naar de naam van uw toepassing in de lijst van app-registraties:

    ![Zoek de toepassingsnaam van uw](./media/batch-aad-auth/search-app-registration.png)

3. Klik op de toepassing en op **instellingen**. In de **API-toegang** sectie, selecteer **vereiste machtigingen**.
4. In de **vereiste machtigingen** blade, klikt u op de **toevoegen** knop.
5. In **Select an API**, zoek naar de Batch-API. Zoek deze tekenreeksen totdat u de API hebt gevonden:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Nieuwere Azure AD-tenants kunnen deze naam gebruiken.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** is de id voor de Batch-API. 
6. Als u de Batch-API hebt gevonden, selecteert u deze en klikt u op **Selecteer**.
7. In **machtigingen selecteren**, schakel het selectievakje in naast **Access Azure Batch-Service** en klikt u op **Selecteer**.
8. Klik op **Gereed**.

De **vereiste machtigingen** windows nu wordt weergegeven dat uw Azure AD-toepassing toegang tot zowel ADAL en de Batch heeft-service-API. Machtigingen worden toegekend aan ADAL automatisch wanneer u uw app eerst bij Azure AD registreren.

![Machtigingen verlenen API](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a>Een service-principal gebruiken 

Als u wilt verifiëren van een toepassing die wordt uitgevoerd zonder toezicht, moet u een service-principal gebruiken. Nadat u uw toepassing hebt geregistreerd, voert u de volgende stappen uit in de Azure portal voor het configureren van een service-principal:

1. Vraag een geheime sleutel voor uw toepassing.
2. Een RBAC-rol toewijzen aan uw toepassing.

### <a name="request-a-secret-key-for-your-application"></a>Aanvragen van een geheime sleutel voor uw toepassing

Wanneer uw toepassing met een service-principal verifieert, stuurt het zowel de toepassings-ID en een geheime sleutel naar Azure AD. U moet maken en kopieer de geheime sleutel van uw code te gebruiken.

Volg deze stappen in de Azure-portal:

1. Kies in het linker navigatiedeelvenster van de Azure-portal **alle services**. Klik op **App-registraties**.
2. Zoeken naar de naam van uw toepassing in de lijst van app-registraties.
3. Klik op de toepassing en op **instellingen**. In de **API-toegang** sectie, selecteer **sleutels**.
4. Voer een beschrijving voor de sleutel voor het maken van een sleutel. Selecteer een duur van de sleutel van één of twee jaar. 
5. Klik op de **opslaan** knop maken en weergeven van de sleutel. Kopieer de sleutelwaarde op een veilige plaats als u het niet mogelijk toegang tot het opnieuw nadat u de blade hebt verlaten. 

    ![Maken van een geheime sleutel](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-to-your-application"></a>Een RBAC-rol toewijzen aan uw toepassing

Als u wilt verifiëren met een service-principal, moet u een RBAC-rol toewijzen aan uw toepassing. Volg deze stappen:

1. Navigeer naar de Batch-account wordt gebruikt door uw toepassing in de Azure-portal.
2. In de **instellingen** blade voor het Batch-account, selecteer **Access Control (IAM)**.
3. Klik op de knop **Toevoegen**. 
4. Uit de **rol** vervolgkeuzelijst, kiest u de _Inzender_ of _lezer_ rol voor uw toepassing. Zie voor meer informatie over deze rollen [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../role-based-access-control/overview.md).  
5. In de **Selecteer** en voer de naam van uw toepassing. Selecteer uw toepassing in de lijst en klikt u op **opslaan**.

Uw toepassing wordt nu weergegeven in de instellingen voor toegangsbeheer met een RBAC-rol die is toegewezen. 

![Een RBAC-rol toewijzen aan uw toepassing](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>De tenant-ID niet ophalen voor uw Azure Active Directory

De tenant-ID geeft de Azure AD-tenant waarmee verificatieservices aan uw toepassing. Als u de tenant-ID, de volgende stappen uit:

1. Selecteer uw Active Directory in de Azure-portal.
2. Klik op **Eigenschappen**.
3. Kopieer de GUID-waarde opgegeven voor de **map-ID**. Deze waarde wordt ook aangeroepen voor de tenant-ID.

![Kopieer de map-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a>Voorbeelden van code

De codevoorbeelden in deze sectie laten zien hoe om te verifiëren met Azure AD dat gebruikmaakt van geïntegreerde verificatie en met een service-principal. De meeste van deze codevoorbeelden .NET gebruiken, maar de concepten zijn vergelijkbaar voor andere talen.

> [!NOTE]
> Een Azure AD-verificatietoken is verlopen na een uur. Wanneer u een lange levensduur hebben **BatchClient** -object, raden wij aan dat u een token uit ADAL bij elke aanvraag om ervoor te zorgen er altijd een geldig token opgehaald. 
>
>
> Om dit te doen in .NET, schrijven van een methode die het token opgehaald uit Azure AD en doorgeven van deze methode een **BatchTokenCredentials** object als een gemachtigde. De gemachtigdenmethode wordt aangeroepen bij elke aanvraag naar de Batch-service om ervoor te zorgen dat een geldig token wordt geleverd. Standaard ADAL tokens, caches, zodat een nieuw token is opgehaald uit Azure AD alleen indien nodig. Zie voor meer informatie over de tokens in Azure AD, [Verificatiescenario's voor Azure AD][aad_auth_scenarios].
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a>Codevoorbeeld: met behulp van Azure AD geïntegreerde verificatie met Batch .NET

Als u wilt verifiëren met geïntegreerde verificatie van de Batch .NET, verwijzen naar de [Azure Batch .NET](https://www.nuget.org/packages/Microsoft.Azure.Batch/) pakket en de [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) pakket.

De volgende `using` instructies in uw code:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Verwijzen naar de Azure AD-eindpunt in uw code, met inbegrip van de tenant-ID. Als u wilt ophalen van de tenant-ID, volg de stappen [ophalen van de tenant-ID voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Verwijzen naar de resource-eindpunt van de Batch-service:

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Verwijzen naar uw Batch-account:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Geef de toepassings-ID (client-ID) voor uw toepassing. De toepassings-ID is beschikbaar vanuit uw app-registratie in Azure portal:

```csharp
private const string ClientId = "<application-id>";
```

Kopieer de omleidings-URI die u hebt opgegeven, ook als u uw toepassing geregistreerd als een systeemeigen toepassing. De URI die is opgegeven in uw code omleiding moet overeenkomen met de omleidings-URI die u hebt opgegeven bij de registratie van de toepassing:

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

Schrijven van een retouraanroepmethode om de verificatietoken van Azure AD te verkrijgen. De **GetAuthenticationTokenAsync** terugbelmethode hier aanroepen ADAL voor verificatie van een gebruiker die wordt interactie met de toepassing wordt weergegeven. De **AcquireTokenAsync** methode geleverd door ADAL vraagt de gebruiker om hun referenties en de opbrengst van toepassing wanneer de gebruiker deze geeft (tenzij deze al in de cache referenties opgeslagen is):

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

Maken van een **BatchTokenCredentials** -object dat de gemachtigde als een parameter nodig. Deze referenties gebruiken om te openen een **BatchClient** object. U kunt gebruiken die **BatchClient** -object voor verdere bewerkingen op basis van de Batch-service:

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

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a>Codevoorbeeld: met behulp van een service-principal voor Azure AD met Batch .NET

Als u wilt verifiëren met een service-principal van Batch .NET, verwijzen naar de [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) pakket en de [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) pakket.

De volgende `using` instructies in uw code:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Verwijzen naar de Azure AD-eindpunt in uw code, met inbegrip van de tenant-ID. Wanneer u een service-principal gebruikt, kunt u een tenant-specifieke eindpunt moet opgeven. Als u wilt ophalen van de tenant-ID, volg de stappen [ophalen van de tenant-ID voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Verwijzen naar de resource-eindpunt van de Batch-service:  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Verwijzen naar uw Batch-account:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Geef de toepassings-ID (client-ID) voor uw toepassing. De toepassings-ID is beschikbaar vanuit uw app-registratie in Azure portal:

```csharp
private const string ClientId = "<application-id>";
```

Geef de geheime sleutel die u hebt gekopieerd uit de Azure-portal:

```csharp
private const string ClientKey = "<secret-key>";
```

Schrijven van een retouraanroepmethode om de verificatietoken van Azure AD te verkrijgen. De **GetAuthenticationTokenAsync** terugbelmethode hier aanroepen ADAL voor verificatie zonder toezicht wordt weergegeven:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

Maken van een **BatchTokenCredentials** -object dat de gemachtigde als een parameter nodig. Deze referenties gebruiken om te openen een **BatchClient** object. Gebruik vervolgens die **BatchClient** -object voor verdere bewerkingen op basis van de Batch-service:

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
### <a name="code-example-using-an-azure-ad-service-principal-with-batch-python"></a>Codevoorbeeld: met behulp van een service-principal voor Azure AD met Batch Python

Als u wilt verifiëren met een service-principal van Batch Python, installeren en verwijzen naar de [azure batch](https://pypi.org/project/azure-batch/) en [azure gemeenschappelijke](https://pypi.org/project/azure-common/) modules.


```python
from azure.batch import BatchServiceClient
from azure.common.credentials import ServicePrincipalCredentials
```

Wanneer u een service-principal gebruikt, moet u de tenant-ID. Als u wilt ophalen van de tenant-ID, volg de stappen [ophalen van de tenant-ID voor uw Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```python
TENANT_ID = "<tenant-id>";
```

Verwijzen naar de resource-eindpunt van de Batch-service:  

```python
RESOURCE = "https://batch.core.windows.net/";
```

Verwijzen naar uw Batch-account:

```python
BATCH_ACCOUNT_URL = "https://myaccount.mylocation.batch.azure.com";
```

Geef de toepassings-ID (client-ID) voor uw toepassing. De toepassings-ID is beschikbaar vanuit uw app-registratie in Azure portal:

```python
CLIENT_ID = "<application-id>";
```

Geef de geheime sleutel die u hebt gekopieerd uit de Azure-portal:

```python
SECRET = "<secret-key>";
```

Maak een **ServicePrincipalCredentials** object:

```python
credentials = ServicePrincipalCredentials(
    client_id=CLIENT_ID,
    secret=SECRET,
    tenant=TENANT_ID,
    resource=RESOURCE
)
```

De referenties voor de service-principal gebruiken om te openen een **BatchServiceClient** object. Gebruik vervolgens die **BatchServiceClient** -object voor verdere bewerkingen op basis van de Batch-service.

```python
    batch_client = BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL
)
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Azure Active Directory, de [documentatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Gedetailleerde voorbeelden die laten zien hoe u het gebruik van ADAL zijn beschikbaar in de [Azure-codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotheek.

* Zie voor meer informatie over service-principals, [toepassing en service-principalobjecten in Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md). Zie voor het maken van een service-principal met behulp van de Azure portal, [portal gebruiken voor het maken van Active Directory toepassing en service-principal die toegang hebben tot resources](../resource-group-create-service-principal-portal.md). U kunt ook een service-principal maken met PowerShell of Azure CLI.

* Als u wilt verifiëren Batch Management-toepassingen met behulp van Azure AD, Zie [verifiëren Batch Management-oplossingen met Active Directory](batch-aad-auth-management.md).

* Zie voor een Python-voorbeeld van het maken van een batchclient die is geverifieerd met behulp van een Azure AD-token, de [Azure Batch aangepaste installatiekopie implementeren met een Python-Script](https://github.com/azurebigcompute/recipes/blob/master/Azure%20Batch/CustomImages/CustomImagePython.md) voorbeeld.

[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Wat is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Verificatiescenario's voor Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Toepassingen integreren met Azure Active Directory"
[azure_portal]: http://portal.azure.com
