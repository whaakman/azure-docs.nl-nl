---
title: Beheerde identiteiten in App Service en Azure Functions | Microsoft Docs
description: Conceptuele verwijzing en setup-handleiding voor het beheerde identiteiten in Azure App Service en Azure Functions
services: app-service
author: mattchenderson
manager: cfowler
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/20/2018
ms.author: mahender
ms.openlocfilehash: 801dddd3379d3c9c375ab883e98f346c69068033
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834414"
---
# <a name="how-to-use-managed-identities-for-app-service-and-azure-functions"></a>Over het gebruik van beheerde identiteiten voor App Service en Azure Functions

> [!NOTE] 
> Beheerde identiteit ondersteuning voor App Service op Linux- en Web App for Containers is momenteel in preview.

> [!Important] 
> Beheerde identiteiten voor App Service en Azure Functions wordt niet meer zoals verwacht als uw app voor abonnementen/tenants wordt gemigreerd. De app moet ophalen van een nieuwe identiteit, die kan worden uitgevoerd door te schakelen en de functie opnieuw in te schakelen. Zie [verwijderen van een identiteit](#remove) hieronder. Downstream resources moet ook zijn bijgewerkt voor het gebruik van de nieuwe identiteit toegangsbeleid.

In dit onderwerp leest u hoe u een beheerde identiteit voor toepassingen van App Service en Azure Functions maken en te gebruiken voor toegang tot andere resources. Een beheerde identiteit uit Azure Active Directory kan uw app eenvoudig toegang tot andere AAD beveiligde bronnen, zoals Azure Key Vault. De identiteit wordt beheerd door het Azure-platform en vereist niet dat u in te richten of er geheimen draaien. Zie voor meer informatie over beheerde identiteiten in AAD, [beheerde identiteiten voor een Azure-resources](../active-directory/managed-identities-azure-resources/overview.md).

Uw toepassing kan twee soorten identiteiten worden verleend: 
- Een **systeem toegewezen identiteit** is gekoppeld aan uw toepassing en wordt verwijderd als uw app wordt verwijderd. Een app kan slechts één systeem toegewezen identiteit hebben. Ondersteuning voor het systeem toegewezen identiteit is algemeen beschikbaar voor Windows-apps. 
- Een **gebruiker toegewezen identiteit** is een afzonderlijke Azure-resource die kan worden toegewezen aan uw app. Een app kan meerdere gebruiker toegewezen identiteiten hebben. Er is ondersteuning voor de gebruiker toegewezen identiteiten Preview-versie voor alle app-typen.

## <a name="adding-a-system-assigned-identity"></a>Toevoegen van een systeem toegewezen identiteit

Het maken van een app met een door het systeem toegewezen identiteit, moet een extra eigenschap worden ingesteld voor de toepassing.

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

Als u een beheerde identiteit in de portal instelt, wordt u eerst maken van een toepassing als normale en schakel vervolgens de functie.

1. Zoals u gewend bent, kunt u een app maken in de portal. Navigeren in de portal.

2. Als u een functie-app gebruikt, gaat u naar **platformfuncties**. Voor andere typen, schuif omlaag naar de **instellingen** groep in het linkernavigatievenster.

3. Selecteer **beheerde identiteit**.

4. Binnen de **systeem toegewezen** tabblad **Status** naar **op**. Klik op **Opslaan**.

![Beheerde identiteit in App Service](media/app-service-managed-service-identity/msi-blade-system.png)

### <a name="using-the-azure-cli"></a>Azure CLI gebruiken

Als u een beheerde identiteit met de Azure CLI instelt, moet u gebruiken de `az webapp identity assign` opdracht op basis van een bestaande toepassing. Hebt u drie opties voor het uitvoeren van de voorbeelden in deze sectie:

- Gebruik [Azure Cloud Shell](../cloud-shell/overview.md) vanuit Azure portal.
- Gebruik de ingesloten Azure Cloud Shell via het 'Try It' de knop, zich in de rechterbovenhoek van de onderstaande codeblok.
- [Installeer de nieuwste versie van Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.31 of hoger) als u liever een lokale CLI-console. 

De volgende stappen helpen u bij het maken van een web-app en het toewijzen van een identiteit met behulp van de CLI:

1. Als u de Azure CLI in een lokale console gebruikt, meldt u zich eerst aan bij Azure met [az login](/cli/azure/reference-index#az-login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarin u wilt de toepassing implementeren:

    ```azurecli-interactive
    az login
    ```
2. Maak een web-App met behulp van de CLI. Zie voor meer voorbeelden van hoe u kunt de CLI gebruiken met App Service, [App Service CLI-voorbeelden](../app-service/app-service-cli-samples.md):

    ```azurecli-interactive
    az group create --name myResourceGroup --location westus
    az appservice plan create --name myPlan --resource-group myResourceGroup --sku S1
    az webapp create --name myApp --resource-group myResourceGroup --plan myPlan
    ```

3. Voer de `identity assign` opdracht voor het maken van de identiteit voor deze toepassing:

    ```azurecli-interactive
    az webapp identity assign --name myApp --resource-group myResourceGroup
    ```

### <a name="using-azure-powershell"></a>Azure PowerShell gebruiken

De volgende stappen helpen u bij het maken van een web-app en het toewijzen van een identiteit met Azure PowerShell:

1. Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](/powershell/azure/overview) en voer vervolgens `Login-AzureRmAccount` uit om verbinding te maken met Azure.

2. Maak een webtoepassing met Azure PowerShell. Zie voor meer voorbeelden van hoe u Azure PowerShell gebruiken met App Service [voorbeelden van App Service PowerShell](../app-service/app-service-powershell-samples.md):

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzureRmResourceGroup -Name myResourceGroup -Location $location
    
    # Create an App Service plan in Free tier.
    New-AzureRmAppServicePlan -Name $webappname -Location $location -ResourceGroupName myResourceGroup -Tier Free
    
    # Create a web app.
    New-AzureRmWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName myResourceGroup
    ```

3. Voer de `Set-AzureRmWebApp -AssignIdentity` opdracht voor het maken van de identiteit voor deze toepassing:

    ```azurepowershell-interactive
    Set-AzureRmWebApp -AssignIdentity $true -Name $webappname -ResourceGroupName myResourceGroup 
    ```

### <a name="using-an-azure-resource-manager-template"></a>Met behulp van een Azure Resource Manager-sjabloon

Een Azure Resource Manager-sjabloon kan worden gebruikt om de implementatie van uw Azure-resources te automatiseren. Zie voor meer informatie over implementeren naar App Service en Functions [automatiseren van resource-implementatie in App Service](../app-service/app-service-deploy-complex-application-predictably.md) en [automatiseren van resource-implementatie in Azure Functions](../azure-functions/functions-infrastructure-as-code.md).

Een resource van het type `Microsoft.Web/sites` kunnen worden gemaakt met een identiteit die door de volgende eigenschap te nemen in de resourcedefinitie:
```json
"identity": {
    "type": "SystemAssigned"
}    
```

> [!NOTE] 
> Een toepassing kan zowel door het systeem toegewezen als de gebruiker toegewezen identiteiten hebben op hetzelfde moment. In dit geval de `type` eigenschap `SystemAssigned,UserAssigned`

Toevoegen van het type systeem toegewezen weet Azure maken en beheren van de identiteit voor uw toepassing.

Een web-app kan er bijvoorbeeld als volgt uitzien:
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
    ]
}
```

Wanneer de site wordt gemaakt, heeft de volgende aanvullende eigenschappen:
```json
"identity": {
    "type": "SystemAssigned",
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Waar `<TENANTID>` en `<PRINCIPALID>` zijn vervangen door de GUID's. De eigenschap tenantId identificeert welke deel uitmaakt van de identiteit aan AAD-tenant. De principalId is een unieke id voor de nieuwe identiteit van de toepassing. In AAD heeft de service-principal dezelfde naam die u hebt gegeven tot uw App Service of Azure Functions-exemplaar.


## <a name="adding-a-user-assigned-identity-preview"></a>Toevoegen van een gebruiker toegewezen identiteit (preview)

> [!NOTE] 
> Gebruiker toegewezen identiteiten zijn momenteel in preview. Sovreign clouds zijn nog niet ondersteund.

Het maken van een app met een gebruiker toegewezen identiteit is vereist dat u de identiteit maken en vervolgens de resource-id aan uw app-configuratie toevoegen.

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

> [!NOTE] 
> Deze portal-ervaring wordt geïmplementeerd en kan nog niet beschikbaar in alle regio's.

U moet eerst een resource door de gebruiker toegewezen identiteit maken.

1. Maak een beheerde identiteit gebruiker toegewezen resource volgens [deze instructies](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

2. Zoals u gewend bent, kunt u een app maken in de portal. Navigeren in de portal.

3. Als u een functie-app gebruikt, gaat u naar **platformfuncties**. Voor andere typen, schuif omlaag naar de **instellingen** groep in het linkernavigatievenster.

4. Selecteer **beheerde identiteit**.

5. Binnen de **gebruiker toegewezen (preview)** tabblad **toevoegen**.

6. Zoek de identiteit die u eerder hebt gemaakt en selecteer deze. Klik op **Add**.

![Beheerde identiteit in App Service](media/app-service-managed-service-identity/msi-blade-user.png)

### <a name="using-an-azure-resource-manager-template"></a>Met behulp van een Azure Resource Manager-sjabloon

Een Azure Resource Manager-sjabloon kan worden gebruikt om de implementatie van uw Azure-resources te automatiseren. Zie voor meer informatie over implementeren naar App Service en Functions [automatiseren van resource-implementatie in App Service](../app-service/app-service-deploy-complex-application-predictably.md) en [automatiseren van resource-implementatie in Azure Functions](../azure-functions/functions-infrastructure-as-code.md).

Een resource van het type `Microsoft.Web/sites` kunnen worden gemaakt met een identiteit die door het volgende blok in de resourcedefinitie vervangen `<RESOURCEID>` met de resource-ID van de gewenste identiteit:
```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {}
    }
}    
```

> [!NOTE] 
> Een toepassing kan zowel door het systeem toegewezen als de gebruiker toegewezen identiteiten hebben op hetzelfde moment. In dit geval de `type` eigenschap `SystemAssigned,UserAssigned`

Toevoegen van het type gebruiker toegewezen en een cotells van Azure maken en beheren van de identiteit voor uw toepassing.

Een web-app kan er bijvoorbeeld als volgt uitzien:
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]": {}
        }
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
    ]
}
```

Wanneer de site wordt gemaakt, heeft de volgende aanvullende eigenschappen:
```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "<RESOURCEID>": {
            "principalId": "<PRINCIPALID>",
            "clientId": "<CLIENTID>"
        }
    }
}
```

Waar `<PRINCIPALID>` en `<CLIENTID>` zijn vervangen door de GUID's. De principalId is een unieke id voor de identiteit die wordt gebruikt voor het beheer van AAD. De clientId is een unieke id voor de nieuwe identiteit van de toepassing die wordt gebruikt voor het op te geven welke identiteit moet worden gebruikt tijdens runtime-aanroepen.


## <a name="obtaining-tokens-for-azure-resources"></a>Het verkrijgen van tokens voor Azure-resources

Een app kunt u de identiteit gebruiken voor het ophalen van tokens, voor andere bronnen worden beveiligd door AAD, zoals Azure Key Vault. Deze tokens vertegenwoordigt de toepassing toegang tot de resource en niet een specifieke gebruiker van de toepassing. 

> [!IMPORTANT]
> Mogelijk moet u de doelresource voor toegang vanuit uw toepassing configureren. Bijvoorbeeld, als u een token voor Key Vault aanvraagt, moet u om te controleren of dat u een toegangsbeleid met de identiteit van uw toepassing hebt toegevoegd. Anders wordt de aanroepen van Key Vault geweigerd, zelfs als ze het token bevatten. Zie voor meer informatie over welke resources Azure Active Directory-tokens worden ondersteund, [Azure-services die ondersteuning voor Azure AD-verificatie](../active-directory/managed-identities-azure-resources/services-support-msi.md#azure-services-that-support-azure-ad-authentication).

Er is een eenvoudige REST-protocol voor het verkrijgen van een token in App Service en Azure Functions. Voor .NET-toepassingen, de Microsoft.Azure.Services.AppAuthentication-bibliotheek biedt een abstractie via dit protocol en biedt ondersteuning voor een lokale ontwikkeling-ervaring.

### <a name="asal"></a>Met behulp van de Microsoft.Azure.Services.AppAuthentication-bibliotheek voor .NET

Voor .NET-toepassingen en -functies is de eenvoudigste manier om te werken met een beheerde identiteit via de Microsoft.Azure.Services.AppAuthentication-pakket. Deze bibliotheek wordt u ook de mogelijkheid voor het testen van uw code lokaal op uw ontwikkelcomputer, met behulp van uw gebruikersaccount vanuit Visual Studio, de [Azure CLI](/cli/azure), of geïntegreerde verificatie van Active Directory. Zie voor meer informatie over opties voor lokale ontwikkeling met deze bibliotheek, de [Microsoft.Azure.Services.AppAuthentication verwijzing]. Deze sectie leest u hoe u aan de slag met de bibliotheek in uw code.

1. Voeg verwijzingen toe aan de [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) en andere benodigde NuGet-pakketten aan uw toepassing. Het onderstaande voorbeeld gebruikt u ook [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

2.  Voeg de volgende code aan uw toepassing, wijzigt u de juiste doelresource. In dit voorbeeld ziet u twee manieren om te werken met Azure Key Vault:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.KeyVault;
// ...
var azureServiceTokenProvider = new AzureServiceTokenProvider();
string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
// OR
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
```

Zie voor meer informatie over Microsoft.Azure.Services.AppAuthentication en de bewerkingen die beschikbaar worden gemaakt, de [Microsoft.Azure.Services.AppAuthentication verwijzing] en de [App Service en de Key Vault met MSI-.NET voorbeeld](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

### <a name="using-the-rest-protocol"></a>Met behulp van de REST-protocol

Een app met een beheerde identiteit heeft twee omgevingsvariabelen gedefinieerd:
- MSI_ENDPOINT
- MSI_SECRET

De **MSI_ENDPOINT** is een lokale URL waaruit uw app tokens kan aanvragen. Als u een token voor een resource, moet u een HTTP GET-aanvraag voor dit eindpunt, met inbegrip van de volgende parameters:

> [!div class="mx-tdBreakAll"]
> |Parameternaam|In|Beschrijving|
> |-----|-----|-----|
> |Bron|Query’s uitvoeren|De AAD-resource-URI van de resource voor een token moet worden opgehaald. Dit wordt mogelijk een van de [Azure-services die ondersteuning voor Azure AD-verificatie](../active-directory/managed-identities-azure-resources/services-support-msi.md#azure-services-that-support-azure-ad-authentication) of een andere resource-URI.|
> |API-versie|Query’s uitvoeren|De versie van de token API moet worden gebruikt. '2017-09-01' is momenteel de enige versie die wordt ondersteund.|
> |geheim|Header|De waarde van de omgevingsvariabele MSI_SECRET.|
> |clientid|Query’s uitvoeren|(Optioneel) De ID van de gebruiker toegewezen identiteit moet worden gebruikt. Als u dit weglaat, wordt het systeem toegewezen identiteit wordt gebruikt.|


Een geslaagde respons met 200 OK bevat een JSON-hoofdtekst met de volgende eigenschappen:

> [!div class="mx-tdBreakAll"]
> |Naam van eigenschap|Beschrijving|
> |-------------|----------|
> |access_token|Het aangevraagde toegangstoken. De aanroepende webservice kan dit token gebruiken om te verifiëren bij de ontvangende webservice.|
> |expires_on|De tijd wanneer het toegangstoken is verlopen. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de vervaltijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache.|
> |Bron|De App-ID-URI van de ontvangende webservice.|
> |token_type|Geeft aan dat de waarde van het token. Het enige type die ondersteuning biedt voor Azure AD is Bearer. Zie voor meer informatie over het bearer-tokens [Framework van de OAuth 2.0 machtiging: Bearer Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).|


Deze reactie is hetzelfde als de [antwoord voor de AAD-service-naar-service toegang-tokenaanvraag](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#service-to-service-access-token-response).

> [!NOTE] 
> Omgevingsvariabelen worden ingesteld als het proces voor het eerst start, zodat na een beheerde identiteit voor uw toepassing is ingeschakeld, moet u mogelijk uw toepassing opnieuw hebt gestart of voordat de code implementeren `MSI_ENDPOINT` en `MSI_SECRET` beschikbaar zijn voor uw code.

### <a name="rest-protocol-examples"></a>Voorbeelden van REST-protocol
Een van de voorbeeldaanvraag kan er als volgt uitzien:
```
GET /MSI/token?resource=https://vault.azure.net&api-version=2017-09-01 HTTP/1.1
Host: localhost:4141
Secret: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```
En een voorbeeldantwoord ziet er als volgt uit:
```
HTTP/1.1 200 OK
Content-Type: application/json
 
{
    "access_token": "eyJ0eXAi…",
    "expires_on": "09/14/2017 00:00:00 PM +00:00",
    "resource": "https://vault.azure.net",
    "token_type": "Bearer"
}
```

### <a name="code-examples"></a>Voorbeelden van code
<a name="token-csharp"></a>Om in C# maken voor deze aanvraag:
```csharp
public static async Task<HttpResponseMessage> GetToken(string resource, string apiversion)  {
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Add("Secret", Environment.GetEnvironmentVariable("MSI_SECRET"));
    return await client.GetAsync(String.Format("{0}/?resource={1}&api-version={2}", Environment.GetEnvironmentVariable("MSI_ENDPOINT"), resource, apiversion));
}
```
> [!TIP]
> Voor .NET-talen, kunt u ook gebruiken [Microsoft.Azure.Services.AppAuthentication](#asal) in plaats van het samenstellen van deze aanvraag zelf.

<a name="token-js"></a>In Node.JS:
```javascript
const rp = require('request-promise');
const getToken = function(resource, apiver, cb) {
    var options = {
        uri: `${process.env["MSI_ENDPOINT"]}/?resource=${resource}&api-version=${apiver}`,
        headers: {
            'Secret': process.env["MSI_SECRET"]
        }
    };
    rp(options)
        .then(cb);
}
```

<a name="token-powershell"></a>In PowerShell:
```powershell
$apiVersion = "2017-09-01"
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:MSI_ENDPOINT + "?resource=$resourceURI&api-version=$apiVersion"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"Secret"="$env:MSI_SECRET"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```

## <a name="remove"></a>Een identiteit die is verwijderd

Een systeem toegewezen identiteit kan worden verwijderd door het uitschakelen van de functie met behulp van de portal, PowerShell of CLI op dezelfde manier waarop deze is gemaakt. Gebruiker toegewezen identiteiten kunnen afzonderlijk worden verwijderd. Als u wilt verwijderen van alle identiteiten in het protocol REST/ARM-sjabloon, wordt dit gedaan door het instellen van het type 'Geen':

```json
"identity": {
    "type": "None"
}    
```

Verwijderen van een systeem toegewezen identiteit op deze manier, wordt deze ook verwijderd uit AAD. Het systeem toegewezen identiteit worden ook automatisch verwijderd uit AAD wanneer de app-resource wordt verwijderd.

> [!NOTE] 
> Er is ook een toepassingsinstelling die kan worden ingesteld, WEBSITE_DISABLE_MSI, waardoor alleen de lokale token-service is uitgeschakeld. Echter, laat u de identiteit in plaats en hulpprogramma's worden nog steeds de beheerde identiteit als 'aan' of 'ingeschakeld'. Als gevolg hiervan is gebruik van deze instelling niet aanbevolen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Toegang tot SQL Database veilig met een beheerde identiteit](app-service-web-tutorial-connect-msi.md)

[Microsoft.Azure.Services.AppAuthentication verwijzing]: https://go.microsoft.com/fwlink/p/?linkid=862452
