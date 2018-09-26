---
title: API-versieprofielen gebruiken met .NET SDK in Azure Stack | Microsoft Docs
description: Meer informatie over het gebruik van API-versieprofielen met .NET in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 35329468ee01d5b70d654c1eb4a908db9d3fcb5d
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184783"
---
# <a name="use-api-version-profiles-with-net-in-azure-stack"></a>API-versieprofielen gebruiken met .NET in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De .NET-SDK voor Azure Stack Resource Manager biedt hulpprogramma's waarmee u kunt bouwen en beheren van uw infrastructuur. Resourceproviders in de SDK zijn onder andere compute, networking, storage, app-services, en [KeyVault](../../key-vault/key-vault-whatis.md). De .NET-SDK bevat 14 NuGet-pakketten dat moeten worden gedownload naar uw projectoplossing telkens wanneer die gebruikmaken van de profielgegevens. Echter, kunt u specifiek welke resourceprovider downloaden om het optimaliseren van het geheugen voor uw toepassing wordt gebruikt voor de 2018-03-01-hybride of 2017-03-09-profiel. Elk pakket bestaat uit een resourceprovider, de bijbehorende API-versie en de API-profiel waarvan deze deel uitmaakt. API-profielen in de .NET SDK inschakelen ontwikkeling van hybride cloud, doordat u schakelen tussen de globale Azure-resources en resources in Azure Stack.

## <a name="net-and-api-version-profiles"></a>.NET en API-versieprofielen

Een API-profiel is een combinatie van resourceproviders en -API-versies. U kunt een API-profiel gebruiken om op te halen van de laatste, meest stabiele versie van elk resourcetype in een resource provider-pakket.

-   Als u wilt gebruiken van de meest recente versies van alle services, gebruikt u de **nieuwste** profiel van de pakketten. Dit profiel maakt deel uit van de **Microsoft.Azure.Management** NuGet-pakket.

-   Als u de services die compatibel zijn met Azure Stack, gebruikt de **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg** of **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg** pakketten.

    -   Er zijn twee pakketten voor elke resourceprovider voor elk profiel.

    -   Zorg ervoor dat de **ResourceProvider** gedeelte van het bovenstaande NuGet-pakket wordt gewijzigd naar de juiste provider.

-   Als u de meest recente API-versie van een service, gebruikt de **nieuwste** profiel van de specifieke NuGet-pakket. Bijvoorbeeld, als u wilt gebruiken de **nieuwste API** versie van de compute-service alleen, gebruik de **nieuwste** profiel van de **compute** pakket. De **nieuwste** profiel maakt deel uit van de **Microsoft.Azure.Management** NuGet-pakket.

-   Als u specifieke API-versies voor een resourcetype in een specifieke resourceprovider, gebruikt u de specifieke API-versies zoals gedefinieerd in het pakket.

Houd er rekening mee dat u alle van de opties in dezelfde toepassing kunt combineren.

## <a name="install-the-azure-net-sdk"></a>De Azure .NET SDK installeren

1.  Git installeren. Zie voor instructies [aan de slag - installeren van Git][].

2.  Zie voor het installeren van de juiste NuGet-pakketten, [zoeken en installeren van een pakket][].

3.  De pakketten die moeten worden geïnstalleerd, is afhankelijk van de Profielversie die u wilt gebruiken. De naam van het pakket voor de profielversies zijn:

    1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**

    2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**

4.  Voor het installeren van de juiste NuGet-pakketten voor Visual Studio Code, Zie de volgende koppeling voor het downloaden van de [NuGet-Pakketbeheer-instructies][].

5.  Als niet beschikbaar is, maakt u een abonnement en opslaan van de abonnements-ID moet later worden gebruikt. Zie voor instructies voor het maken van een abonnement, [abonnementen voor aanbiedingen maken in Azure Stack][].

6.  Een service-principal maken en opslaan van de Client-ID en het Clientgeheim. Zie voor instructies over het maken van een service-principal voor Azure Stack [toepassingen toegang verlenen tot Azure Stack][]. Houd er rekening mee dat de Client-ID ook wel bekend als de toepassings-ID wordt bij het maken van een service-principal.

7.  Zorg ervoor dat uw service-principal de rol van inzender of eigenaar heeft op uw abonnement. Zie voor instructies over hoe u een rol toewijzen aan service-principal [toepassingen toegang verlenen tot Azure Stack][].

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van de Azure-SDK voor .NET met Azure Stack, moet u de volgende waarden opgeven en vervolgens waarden met omgevingsvariabelen instellen. Als u wilt de omgevingsvariabelen instellen, raadpleegt u de instructies onder de tabel voor uw besturingssysteem.

| Waarde                     | Omgevingsvariabelen   | Beschrijving                                                                                                             |
|---------------------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Tenant-id                 | AZURE_TENANT_ID       | De waarde van uw Azure Stack [ *tenant-ID*][].                                                                          |
| Client-id                 | AZURE_CLIENT_ID       | De service principal toepassings-ID opgeslagen wanneer de service-principal is gemaakt in de vorige sectie van dit artikel. |
| Abonnements-id           | AZURE_SUBSCRIPTION_ID | De [ *abonnements-ID* ][] is hoe u toegang hebben tot aanbiedingen in Azure Stack.                                                      |
| Clientgeheim             | AZURE_CLIENT_SECRET   | De service principal toepassingsgeheim opgeslagen wanneer de service-principal is gemaakt.                                      |
| Resource Manager-eindpunt | ARM_ENDPOINT           | Zie [ *de Azure Stack resource manager-eindpunt*][].                                                                    |

Volg de instructies om de Tenant-ID voor uw Azure Stack, [hier](../azure-stack-csp-ref-operations.md). Om in te stellen de omgevingsvariabelen, het volgende doen:

### <a name="microsoft-windows"></a>Microsoft Windows

De omgevingsvariabelen instellen in een Windows-opdrachtprompt, gebruik de volgende indeling:

```shell
Set Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="macos-linux-and-unix-based-systems"></a>macOS, Linux en Unix-systemen

In op basis van Unix-systemen kunt u de volgende opdracht uit:

```shell
Export Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="the-azure-stack-resource-manager-endpoint"></a>De Azure Stack resource manager-eindpunt

De Microsoft Azure Resource Manager is een raamwerk waarmee beheerders te implementeren, beheren en bewaken van Azure-resources. Deze taken kunnen worden verwerkt in Azure Resource Manager als een groep, in plaats van afzonderlijk, in één bewerking.

U krijgt informatie over de metagegevens van het Resource Manager-eindpunt. Het eindpunt retourneert een JSON-bestand met de vereiste informatie op uw code wordt uitgevoerd.

Houd rekening met de volgende overwegingen:

- De **ResourceManagerUrl** is in de Azure Stack Development Kit (ASDK): https://management.local.azurestack.external/

- De **ResourceManagerUrl** in geïntegreerde systemen is: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` om op te halen de metagegevens die vereist zijn: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

JSON-voorbeeldbestand:

```json
{ 
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",
   "graphEndpoint": "https://graph.windows.net/",
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": 
      {
      "loginEndpoint": "https://login.windows.net/",
      "audiences": ["https://management.yourtenant.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## <a name="existing-api-profiles"></a>Bestaande API-profielen

1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**: nieuwste profiel die is gebouwd voor Azure Stack. Gebruik dit profiel voor services meest compatibel is met Azure Stack, zolang er op het stempel 1808 of verdere.

2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**: als u van een tijdstempel die lager is dan de build 1808 gebruikmaakt, dit profiel gebruiken.

3.  **Meest recente**: profiel met de nieuwste versies van alle services. Gebruik de nieuwste versies van alle services. Dit profiel maakt deel uit van de **Microsoft.Azure.Management** NuGet-pakket.

Zie voor meer informatie over Azure Stack- en API-profielen, een [Overzicht van de API-profielen][].

## <a name="azure-net-sdk-api-profile-usage"></a>Gebruik van Azure .NET SDK API-profiel

De volgende code moet worden gebruikt voor het starten van een profiel-client. Deze parameter is alleen vereist voor Azure Stack of andere persoonlijke clouds. Globale Azure al deze instellingen in standaard.

De volgende code is vereist voor het verifiëren van de service-principal in Azure Stack. Het maakt een token door de tenant-ID en de base verificatie, die specifiek is voor Azure Stack.

```csharp
public class CustomLoginCredentials : ServiceClientCredentials
{
    private string clientId;
    private string clientSecret;
    private string resourceId;
    private string tenantId;

    private const string authenticationBase = "https://login.windows.net/{0}";

    public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
    {
        clientId = servicePrincipalId;
        clientSecret = servicePrincipalSecret;
        resourceId = azureEnvironmentResourceId;
        tenantId = azureEnvironmentTenandId;
    }
```

Hierdoor kunt u de API-profiel NuGet-pakketten gebruiken om uw toepassing is op Azure Stack te implementeren.

## <a name="define-azure-stack-environment-setting-functions"></a>Azure Stack-omgeving instelling functies definiëren

Gebruik de volgende code voor de verificatie van de service-principal naar het Azure Stack-omgeving heeft:

```csharp
private string AuthenticationToken { get; set; }
public override void InitializeServiceClient<T>(ServiceClient<T> client)
{
    var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
    var credential = new ClientCredential(clientId, clientSecret);
    var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
    clientCredential: credential).Result;
    if (result == null)
    {
        throw new InvalidOperationException("Failed to obtain the JWT token");
    }
    AuthenticationToken = result.AccessToken;
}
```

Dit heeft voorrang op de service-client initialiseren om te verifiëren met Azure Stack.

## <a name="samples-using-api-profiles"></a>Voorbeelden met behulp van API-profielen

U kunt de volgende voorbeelden gevonden in de GitHub-opslagplaatsen als uitgangspunt voor het maken van oplossingen met .NET en Azure Stack-API-profielen gebruiken.

-   [Project om te testen op virtuele Machine, vNet, resourcegroepen en storage-account][]
-   Virtuele machines beheren met .NET

### <a name="sample-unit-test-project"></a>Voorbeeld van een Project voor de Test-eenheid 

1.  Kloon de opslagplaats met de volgende opdracht:

    `git clone <https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject>`

2.  Een Azure-service-principal maken en toewijzen van een rol voor toegang tot het abonnement. Zie voor instructies over het maken van een service-principal [Azure PowerShell gebruiken voor het maken van een service-principal met een certificaat][].

3.  De volgende vereiste waarden ophalen:

    1.  Tenant-id
    2.  Client-id
    3.  Clientgeheim
    4.  Abonnements-id
    5.  Resource Manager-eindpunt

4.  Stel de volgende omgevingsvariabelen met behulp van de gegevens die u hebt opgehaald via de service-principal dat hebt gemaakt met behulp van de opdrachtprompt:

    1.  exporteren van AZURE_TENANT_ID = {uw tenant-id}
    2.  exporteren van AZURE_CLIENT_ID = {uw client-id}
    3.  exporteren van AZURE_CLIENT_SECRET = {uw clientgeheim}
    4.  exporteren van AZURE_SUBSCRIPTION_ID = {uw abonnements-id}
    5.  exporteren van ARM_ENDPOINT = {uw Azure Stack Resource manager-URL}

   Gebruik in Windows, **ingesteld** in plaats van **exporteren**.

5.  Zorg ervoor dat de locatie-variabele is ingesteld op de locatie van uw Azure Stack. Bijvoorbeeld, lokale = "local".

6.  Stel de aangepaste aanmeldingsreferenties waarmee u kunt om te verifiëren met Azure Stack. Houd er rekening mee dat dit deel van de code is opgenomen in dit voorbeeld in de autorisatie-map.

   ```csharp
   public class CustomLoginCredentials : ServiceClientCredentials
   {
       private string clientId;
       private string clientSecret;
       private string resourceId;
       private string tenantId;
       private const string authenticationBase = "https://login.windows.net/{0}";
       public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
       {
           clientId = servicePrincipalId;
           clientSecret = servicePrincipalSecret;
           resourceId = azureEnvironmentResourceId;
           tenantId = azureEnvironmentTenandId;
       }
   private string AuthenticationToken { get; set; }
   ```

7.  Voeg de volgende code toe als u Azure Stack voor de onderdrukking van de service-client initialiseren om te verifiëren met Azure Stack. Houd er rekening mee dat een deel van de code al in dit voorbeeld in de autorisatie-map opgenomen is.

   ```csharp
   public override void InitializeServiceClient<T>(ServiceClient<T> client)
   {
      var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
      var credential = new ClientCredential(clientId, clientSecret);
      var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
                clientCredential: credential).Result;
      if (result == null)
      {
          throw new InvalidOperationException("Failed to obtain the JWT token");
      }
      AuthenticationToken = result.AccessToken;
   }
   ```
 
8.  Met behulp van NuGet-Pakketbeheer, zoeken naar '2018-03-01-hybride' en installeer de pakketten die zijn gekoppeld aan dit profiel voor de resourceproviders Compute, netwerken, opslag, KeyVault en App-Services.

2.  Binnen elke taak in het bestand .cs, stel de parameters die nodig zijn voor het werken met Azure Stack. Een voorbeeld als volgt wordt weergegeven voor de taak `CreateResourceGroupTest`:

   ```csharp
   var location = Environment.GetEnvironmentVariable("AZURE_LOCATION");
   var baseUriString = Environment.GetEnvironmentVariable("AZURE_BASE_URL");
   var resourceGroupName = Environment.GetEnvironmentVariable("AZURE_RESOURCEGROUP");
   var servicePrincipalId = Environment.GetEnvironmentVariable("AZURE_CLIENT_ID");
   var servicePrincipalSecret = Environment.GetEnvironmentVariable("AZURE_CLIENT_SECRET");
   var azureResourceId = Environment.GetEnvironmentVariable("AZURE_RESOURCE_ID");
   var tenantId = Environment.GetEnvironmentVariable("AZURE_TENANT_ID");
   var subscriptionId = Environment.GetEnvironmentVariable("AZURE_SUBSCRIPTION_ID");
   var credentials = new CustomLoginCredentials(servicePrincipalId, servicePrincipalSecret, azureResourceId, tenantId);
   ```

1.  Klik met de rechtermuisknop op elke taak en selecteert u **test uitvoeren**.

    1.  Een waarschuwing de een groen vinkje in het deelvenster aan clientzijde dat elke taak is op basis van de opgegeven parameters is gemaakt. Controleer of uw Azure Stack-abonnement om te controleren of dat de resources zijn gemaakt.

    2.  Zie voor meer informatie over het uitvoeren van eenheidstests [eenheidstests met Test Explorer uitvoeren.][]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over API-profielen:

- [Beheren van API-versieprofielen in Azure Stack](azure-stack-version-profiles.md)
- [Resource provider API-versies ondersteund door profielen](azure-stack-profiles-azure-resource-manager-versions.md)

  [Aan de slag - installeren van Git]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
  [Zoeken en installeren van een pakket]: /nuget/tools/package-manager-ui
  [NuGet-Pakketbeheer-instructies]: https://marketplace.visualstudio.com/items?itemName=jmrog.vscode-nuget-package-manager
  [Abonnementen voor aanbiedingen maken in Azure Stack]: ../azure-stack-subscribe-plan-provision-vm.md
  [Toepassingen toegang verlenen tot Azure Stack]: ../azure-stack-create-service-principals.md
  [* tenant -ID *]: ../azure-stack-identity-overview.md
  [* abonnement -ID *]: ../azure-stack-plan-offer-quota-overview.md#subscriptions
  [* de Azure Stack resource manager-eindpunt *]: ../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint
  [Overzicht van de API-profielen]: ../user/azure-stack-version-profiles.md#summary-of-api-profiles
  [Project om te testen op virtuele Machine, vNet, resourcegroepen en storage-account]: https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject
  [Azure PowerShell gebruiken voor het maken van een service-principal met een certificaat]: ../azure-stack-create-service-principals.md
  [Eenheidstests met Test Explorer uitvoeren.]: /visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2017
