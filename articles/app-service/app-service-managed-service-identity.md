---
title: Beheerde Service-identiteit in App Service en Azure Functions | Microsoft Docs
description: Conceptuele verwijzing en setup-handleiding voor ondersteuning van beheerde Service-identiteit in Azure App Service en Azure Functions
services: app-service
author: mattchenderson
manager: cfowler
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2018
ms.author: mahender
ms.openlocfilehash: ed2db5fd48c60601b90fc7ffb1094b8d89573b1f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-use-azure-managed-service-identity-public-preview-in-app-service-and-azure-functions"></a>Het gebruik van Azure Managed Service-identiteit (openbare preview) in App Service en Azure Functions

> [!NOTE] 
> Beheerde Service-identiteit voor App Service en Azure Functions is momenteel in preview. App-Service op Linux- en Web-App voor Containers worden momenteel niet ondersteund.


> [!Important] 
> Beheerde Service-identiteit voor App Service en Azure Functions wordt niet uitgevoerd zoals verwacht als uw app wordt gemigreerd in abonnementen/tenants. De app moet verkrijgen van een nieuwe identiteit en de identiteit van de bestaande goed zonder te verwijderen van de site zelf kan niet worden verwijderd. Uw app moet opnieuw worden gemaakt met een nieuwe identiteit en downstream resources moet toegangsbeleid bijgewerkt voor het gebruik van de nieuwe identiteit hebben.


In dit onderwerp leest u hoe u de identiteit van een beheerde app voor toepassingen van App Service en Azure Functions maakt en het gebruik ervan voor toegang tot andere resources. Een beheerde service-identiteit van Azure Active Directory kan uw app eenvoudig toegang tot andere AAD beveiligde bronnen zoals Azure Sleutelkluis. De identiteit wordt beheerd door de Azure-platform en vereist niet dat u in te richten of geen geheimen draaien. Zie voor meer informatie over Service-identiteit beheerd, de [overzicht van de Service-identiteit beheerd](../active-directory/managed-service-identity/overview.md).

## <a name="creating-an-app-with-an-identity"></a>Een app maken met een identiteit

Een app maken met een identiteit, moet een extra eigenschap worden ingesteld voor de toepassing.

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

Als u een beheerde service-identiteit in de portal instelt, u eerst een toepassing als normale maken en vervolgens de functie inschakelen.

1. Zoals u dat gewend bent, kunt u een app maken in de portal. Ga naar het in de portal.

2. Als u een functie-app gebruikt, gaat u naar **platformfuncties**. Voor andere typen app Schuif omlaag naar de **instellingen** groep in de linkernavigatiebalk.

3. Selecteer **beheerde service-identiteit**.

4. Switch **geregistreerd bij Azure Active Directory** naar **op**. Klik op **Opslaan**.

![Beheerde Service-identiteit in App Service](media/app-service-managed-service-identity/msi-blade.png)

### <a name="using-the-azure-cli"></a>Azure CLI gebruiken

Als u een beheerde service-identiteit met de Azure CLI instelt, moet u gebruiken de `az webapp identity assign` opdracht op basis van een bestaande toepassing. Hebt u drie opties voor het uitvoeren van de voorbeelden in deze sectie:

- Gebruik [Azure Cloud Shell](../cloud-shell/overview.md) vanuit de Azure-portal.
- Gebruik de ingesloten Azure Cloud Shell via de 'Deze probeer' knop, zich in de rechterbovenhoek van elk codeblok hieronder.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.31 of hoger) als u liever een lokale CLI-console te gebruiken. 

De volgende stappen wordt beschreven hoe u een web-app maken en toewijzen van een identiteit met behulp van de CLI:

1. Als u de Azure CLI in een lokale console, eerst aanmelden bij het gebruik van Azure [az aanmelding](/cli/azure/reference-index#az_login). Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt de toepassing implementeren:

    ```azurecli-interactive
    az login
    ```
2. Maak een webtoepassing met behulp van de CLI. Zie voor meer voorbeelden van hoe de CLI gebruiken met App Service [App Service CLI voorbeelden](../app-service/app-service-cli-samples.md):

    ```azurecli-interactive
    az group create --name myResourceGroup --location westus
    az appservice plan create --name myPlan --resource-group myResourceGroup --sku S1
    az webapp create --name myApp --resource-group myResourceGroup --plan myPlan
    ```

3. Voer de `identity assign` opdracht voor het maken van de identiteit voor deze toepassing:

    ```azurecli-interactive
    az webapp identity assign --name myApp --resource-group myResourceGroup
    ```

### <a name="using-an-azure-resource-manager-template"></a>Met behulp van een Azure Resource Manager-sjabloon

Een Azure Resource Manager-sjabloon kan worden gebruikt voor het automatiseren van de implementatie van uw Azure-resources. Zie voor meer informatie over het implementeren van App Service en-functies, [automatiseren resources implementeren in App Service](../app-service/app-service-deploy-complex-application-predictably.md) en [automatiseren resources implementeren in Azure Functions](../azure-functions/functions-infrastructure-as-code.md).

Een resource van het type `Microsoft.Web/sites` kunnen worden gemaakt met een identiteit door de volgende eigenschap in de resourcedefinitie, waaronder:
```json
"identity": {
    "type": "SystemAssigned"
}    
```

Dit vertelt Azure maken en beheren van de identiteit voor uw toepassing.

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

Wanneer de site is gemaakt, heeft deze de volgende aanvullende eigenschappen:
```json
"identity": {
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Waar `<TENANTID>` en `<PRINCIPALID>` worden vervangen door de GUID's. De eigenschap tenantId identificeert welke de toepassing bij de behoort AAD-tenant. De principalId is een unieke id voor de nieuwe identiteit van de toepassing. In AAD heeft de toepassing dezelfde naam die u hebt gegeven tot uw App Service- of Azure Functions-exemplaar.

## <a name="obtaining-tokens-for-azure-resources"></a>Het verkrijgen van tokens voor Azure-resources

Een app kan de identiteit gebruiken voor het ophalen van tokens, voor andere bronnen worden beveiligd door AAD, zoals Azure Sleutelkluis. Deze tokens vertegenwoordigen de toepassing toegang tot de bron en geen specifieke gebruiker van de toepassing. 

> [!IMPORTANT]
> Mogelijk moet u de doelresource voor toegang van uw toepassing configureren. Bijvoorbeeld, als u een token voor Sleutelkluis aanvraagt, moet u om ervoor te zorgen dat u een toegangsbeleid met de identiteit van uw toepassing hebt toegevoegd. Anders wordt wordt de aanroepen van Sleutelkluis geweigerd, zelfs als ze het token. Zie voor meer ondersteuning voor meer over welke resources beheerd Service-identiteit tokens [Azure-services die ondersteuning voor Azure AD authentication](../active-directory/managed-service-identity/overview.md#which-azure-services-support-managed-service-identity).

Er is een eenvoudige REST-protocol voor het verkrijgen van een token in App Service en Azure Functions. Voor .NET-toepassingen, de Microsoft.Azure.Services.AppAuthentication-bibliotheek biedt een abstractie via dit protocol en biedt ondersteuning voor een lokale ontwikkeling biedt.

### <a name="asal"></a>Met behulp van de Microsoft.Azure.Services.AppAuthentication-bibliotheek voor .NET

Voor .NET-toepassingen en -functies is de eenvoudigste manier om te werken met een beheerde service-identiteit via het Microsoft.Azure.Services.AppAuthentication-pakket. Deze bibliotheek, kunt u voor het testen van uw code lokaal op uw ontwikkelcomputer met uw gebruikersaccount vanuit Visual Studio de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), of geïntegreerde verificatie van Active Directory. Zie voor meer informatie over opties voor lokale ontwikkeling met deze bibliotheek, de [Microsoft.Azure.Services.AppAuthentication verwijzing]. Deze sectie leest u hoe aan de slag met de bibliotheek in uw code.

1. Voeg verwijzingen naar de [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) en [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet-pakketten voor uw toepassing.

2.  De volgende code toevoegen aan uw toepassing:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.KeyVault;
// ...
var azureServiceTokenProvider = new AzureServiceTokenProvider();
string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");
// OR
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
```

Zie voor meer informatie over Microsoft.Azure.Services.AppAuthentication en de bewerkingen getoond, de [Microsoft.Azure.Services.AppAuthentication verwijzing] en de [App Service- en KeyVault met MSI .NET voorbeeld](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

### <a name="using-the-rest-protocol"></a>Met behulp van de REST-protocol

Een app met een beheerde service-identiteit heeft twee omgevingsvariabelen gedefinieerd:
- MSI_ENDPOINT
- MSI_SECRET

De **MSI_ENDPOINT** is een lokale URL waaruit uw app tokens kan aanvragen. Als u een token voor een resource, moet u een HTTP GET-aanvraag aan dit eindpunt, met inbegrip van de volgende parameters:

> [!div class="mx-tdBreakAll"]
> |Parameternaam|in|Beschrijving|
> |-----|-----|-----|
> |Bron|Query’s uitvoeren|De AAD-bron-URI van de bron voor een token moet worden opgehaald.|
> |API-versie|Query’s uitvoeren|De versie van de token API moet worden gebruikt. '2017-09-01' is momenteel de enige versie ondersteund.|
> |geheim|Koptekst|De waarde van de omgevingsvariabele MSI_SECRET.|


Een geslaagde 200 OK reactie bevat een JSON-hoofdtekst met de volgende eigenschappen:

> [!div class="mx-tdBreakAll"]
> |De naam van eigenschap|Beschrijving|
> |-------------|----------|
> |access_token|Het aangevraagde toegangstoken. De webservice aanroepen kunt dit token voor verificatie aan de ontvangende webservice gebruiken.|
> |expires_on|De tijd wanneer het toegangstoken is verlopen. De datum die wordt weergegeven als het aantal seconden van 1970-01-01T0:0:0Z UTC totdat de verlooptijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache.|
> |Bron|De App ID URI van de ontvangende webservice.|
> |token_type|Geeft de waarde van het type token. Het enige type dat ondersteuning biedt voor Azure AD is Bearer. Zie voor meer informatie over bearer-tokens [het OAuth 2.0 autorisatie Framework: Bearer-Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).|


Deze reactie is hetzelfde als de [antwoord voor de tokenaanvraag voor AAD-service-naar-service toegang](../active-directory/develop/active-directory-protocols-oauth-service-to-service.md#service-to-service-access-token-response).

> [!NOTE] 
> Omgevingsvariabelen worden ingesteld als het proces wordt gestart, dus nadat de Service-identiteit beheerd is ingeschakeld voor uw toepassing u moet mogelijk uw toepassing opnieuw hebt gestart of voordat de code implementeren `MSI_ENDPOINT` en `MSI_SECRET` beschikbaar zijn voor uw code.

### <a name="rest-protocol-examples"></a>Voorbeelden van REST-protocol
Een voorbeeld van de aanvraag kan er als volgt uitzien:
```
GET /MSI/token?resource=https://vault.azure.net&api-version=2017-09-01 HTTP/1.1
Host: localhost:4141
Secret: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```
En een voorbeeldantwoord kan er als volgt:
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

### <a name="code-examples"></a>Codevoorbeelden
Om deze aanvraag in C#:
```csharp
public static async Task<HttpResponseMessage> GetToken(string resource, string apiversion)  {
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Add("Secret", Environment.GetEnvironmentVariable("MSI_SECRET"));
    return await client.GetAsync(String.Format("{0}/?resource={1}&api-version={2}", Environment.GetEnvironmentVariable("MSI_ENDPOINT"), resource, apiversion));
}
```
> [!TIP]
> Voor .NET-talen, kunt u ook gebruiken [Microsoft.Azure.Services.AppAuthentication](#asal) aanvragen in plaats van het samenstellen van dit zelf.

In Node.JS:
```javascript
const rp = require('request-promise');
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

In PowerShell:
```powershell
$apiVersion = "2017-09-01"
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:MSI_ENDPOINT + "?resource=$resourceURI&api-version=$apiVersion"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"Secret"="$env:MSI_SECRET"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Toegang tot SQL-Database veilig met managed service-identiteit](app-service-web-tutorial-connect-msi.md)

[Microsoft.Azure.Services.AppAuthentication verwijzing]: https://go.microsoft.com/fwlink/p/?linkid=862452
