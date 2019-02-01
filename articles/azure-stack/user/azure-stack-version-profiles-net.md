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
ms.date: 12/07/2018
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 12/07/2018
ms.openlocfilehash: e335f4cb112c0f029768ccf050f888e00a6efdc1
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250611"
---
# <a name="use-api-version-profiles-with-net-in-azure-stack"></a>API-versieprofielen gebruiken met .NET in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De .NET-SDK voor Azure Stack Resource Manager biedt hulpprogramma's waarmee u kunt bouwen en beheren van uw infrastructuur. Resourceproviders in de SDK zijn onder andere compute, networking, storage, app-services, en [KeyVault](../../key-vault/key-vault-whatis.md). De .NET-SDK bevat 14 NuGet-pakketten. Deze pakketten moeten worden gedownload naar uw project-oplossing telkens waarin de profielgegevens. Echter, kunt u specifiek welke resourceprovider downloaden om het optimaliseren van het geheugen voor uw toepassing wordt gebruikt voor de 2018-03-01-hybride of 2017-03-09-profiel. Elk pakket bestaat uit een resourceprovider, de bijbehorende API-versie en de API-profiel waarvan deze deel uitmaakt. API-profielen in de .NET SDK inschakelen ontwikkeling van hybride cloud, doordat u schakelen tussen de globale Azure-resources en resources in Azure Stack.

## <a name="net-and-api-version-profiles"></a>.NET en API-versieprofielen

Een API-profiel is een combinatie van resourceproviders en -API-versies. U kunt een API-profiel gebruiken om op te halen van de laatste, meest stabiele versie van elk resourcetype in een resource provider-pakket.

-   Als u wilt gebruiken van de meest recente versies van alle services, gebruikt u de **nieuwste** profiel van de pakketten. Dit profiel maakt deel uit van de **Microsoft.Azure.Management** NuGet-pakket.

-   Als u de services die compatibel zijn met Azure Stack, gebruikt de **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg** of **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg** pakketten.

    -   Er zijn twee pakketten voor elke resourceprovider voor elk profiel.

    -   Zorg ervoor dat de **ResourceProvider** gedeelte van het bovenstaande NuGet-pakket wordt gewijzigd naar de juiste provider.

-   Als u de meest recente API-versie van een service, gebruikt de **nieuwste** profiel van de specifieke NuGet-pakket. Bijvoorbeeld, als u wilt gebruiken de **nieuwste API** versie van de compute-service alleen, gebruik de **nieuwste** profiel van de **compute** pakket. De **nieuwste** profiel maakt deel uit van de **Microsoft.Azure.Management** NuGet-pakket.

-   Als u specifieke API-versies voor een resourcetype in een specifieke resourceprovider, gebruikt u de specifieke API-versies zoals gedefinieerd in het pakket.

U kunt alle van de opties in dezelfde toepassing combineren.

## <a name="install-the-azure-net-sdk"></a>De Azure .NET SDK installeren

1.  Git installeren. Zie voor instructies [aan de slag - installeren van Git][].

2.  Zie voor het installeren van de juiste NuGet-pakketten, [zoeken en installeren van een pakket][].

3.  De pakketten die moeten worden geïnstalleerd, is afhankelijk van de Profielversie die u wilt gebruiken. De Pakketnamen van het voor de profielversies zijn:

    1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01. *ResourceProvider*. 0.9.0-preview.nupkg**

    2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09. *ResourceProvider*. 0.9.0-preview.nupkg**

4.  Voor het installeren van de juiste NuGet-pakketten voor Visual Studio Code, Zie de volgende koppeling voor het downloaden van de [NuGet-Pakketbeheer-instructies][].

5.  Als niet beschikbaar is, maakt u een abonnement en opslaan van de abonnements-ID moet later worden gebruikt. Zie voor instructies voor het maken van een abonnement, [abonnementen voor aanbiedingen maken in Azure Stack][].

6.  Een service-principal maken en opslaan van de Client-ID en het Clientgeheim. Zie voor instructies over het maken van een service-principal voor Azure Stack [toepassingen toegang verlenen tot Azure Stack][]. De Client-ID wordt ook wel bekend als de toepassings-ID als een service-principal maken.

7.  Zorg ervoor dat uw service-principal de rol van inzender of eigenaar heeft op uw abonnement. Zie voor instructies over hoe u een rol toewijzen aan service-principal [toepassingen toegang verlenen tot Azure Stack][].

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van de Azure-SDK voor .NET met Azure Stack, moet u de volgende waarden opgeven en vervolgens waarden met omgevingsvariabelen instellen. Als u wilt de omgevingsvariabelen instellen, raadpleegt u de instructies onder de tabel voor uw besturingssysteem.

| Waarde                     | Omgevingsvariabelen   | Description                                                                                                             |
|---------------------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Tenant-id                 | AZURE_TENANT_ID       | De waarde van uw Azure Stack [ *tenant-ID*][].                                                                          |
| Client-id                 | AZURE_CLIENT_ID       | De service principal toepassings-ID opgeslagen wanneer de service-principal is gemaakt in de vorige sectie van dit artikel. |
| Abonnements-id           | AZURE_SUBSCRIPTION_ID | De [ *abonnements-ID* ][] is hoe u toegang hebben tot aanbiedingen in Azure Stack.                                                      |
| Clientgeheim             | AZURE_CLIENT_SECRET   | De service principal toepassingsgeheim opgeslagen wanneer de service-principal is gemaakt.                                      |
| Resource Manager-eindpunt | ARM_ENDPOINT           | Zie [ *het Azure Stack Resource Manager-eindpunt*][].                                                                    |
| Locatie                  | RESOURCE_LOCATION     | Locatie voor Azure Stack.

Volg de instructies om de Tenant-ID voor uw Azure Stack, [hier](../azure-stack-csp-ref-operations.md). Als u wilt de omgevingsvariabelen instellen, moet u de volgende stappen uitvoeren:

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

### <a name="the-azure-stack-resource-manager-endpoint"></a>Het Azure Stack Resource Manager-eindpunt

De Microsoft Azure Resource Manager is een raamwerk waarmee beheerders te implementeren, beheren en bewaken van Azure-resources. Deze taken kunnen worden verwerkt in Azure Resource Manager als een groep, in plaats van afzonderlijk, in één bewerking.

U krijgt informatie over de metagegevens van het Resource Manager-eindpunt. Het eindpunt retourneert een JSON-bestand met de vereiste informatie op uw code wordt uitgevoerd.

Houd rekening met de volgende overwegingen:

- De **ResourceManagerUrl** is in de Azure Stack Development Kit (ASDK): https://management.local.azurestack.external/

- De **ResourceManagerUrl** in geïntegreerde systemen is: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` Om op te halen de metagegevens die vereist zijn: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`

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

1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01.*ResourceProvider*.0.9.0-preview.nupkg**: Meest recente profiel die is gebouwd voor Azure Stack. Gebruik dit profiel voor services meest compatibel is met Azure Stack, zolang er op het stempel 1808 of verdere.

2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09.*ResourceProvider*.0.9.0-preview.nupkg**: Als u van een tijdstempel die lager is dan de build 1808 gebruikmaakt, moet u dit profiel gebruiken.

3.  **Laatste**: Profiel met de nieuwste versies van alle services. Gebruik de nieuwste versies van alle services. Dit profiel maakt deel uit van de **Microsoft.Azure.Management** NuGet-pakket.

Zie voor meer informatie over Azure Stack- en API-profielen, een [Overzicht van de API-profielen][].

## <a name="azure-net-sdk-api-profile-usage"></a>Gebruik van Azure .NET SDK API-profiel

De volgende code moet worden gebruikt voor het starten van een resource management-client. Vergelijkbare code kan worden gebruikt voor het starten van de andere resourceprovider (zoals compute, netwerk en opslag) clients. 

```csharp
var client = new ResourceManagementClient(armEndpoint, credentials)
{
    SubscriptionId = subscriptionId
};
```

De `credentials` parameter in de bovenstaande code is vereist voor het starten van een client. De volgende code genereert een verificatietoken door de tenant-ID en de service-principal.

```csharp
var azureStackSettings = getActiveDirectoryServiceSettings(armEndpoint);
var credentials = ApplicationTokenProvider.LoginSilentAsync(tenantId, servicePrincipalId, servicePrincipalSecret, azureStackSettings).GetAwaiter().GetResult();
```
De `getActiveDirectoryServiceSettings` in de code haalt u Azure Stack-eindpunten van het metagegevenseindpunt. Stelt de omgevingsvariabelen van de aanroep die is gemaakt: 

```csharp
public static ActiveDirectoryServiceSettings getActiveDirectoryServiceSettings(string armEndpoint)
{
    var settings = new ActiveDirectoryServiceSettings();
    try
    {
        var request = (HttpWebRequest)HttpWebRequest.Create(string.Format("{0}/metadata/endpoints?api-version=1.0", armEndpoint));
        request.Method = "GET";
        request.UserAgent = ComponentName;
        request.Accept = "application/xml";
        using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
        {
            using (StreamReader sr = new StreamReader(response.GetResponseStream()))
            {
                var rawResponse = sr.ReadToEnd();
                var deserialized = JObject.Parse(rawResponse);
                var authenticationObj = deserialized.GetValue("authentication").Value<JObject>();
                var loginEndpoint = authenticationObj.GetValue("loginEndpoint").Value<string>();
                var audiencesObj = authenticationObj.GetValue("audiences").Value<JArray>();
                settings.AuthenticationEndpoint = new Uri(loginEndpoint);
                settings.TokenAudience = new Uri(audiencesObj[0].Value<string>());
                settings.ValidateAuthority = loginEndpoint.TrimEnd('/').EndsWith("/adfs", StringComparison.OrdinalIgnoreCase) ? false : true;
            }
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine(String.Format("Could not get AD service settings. Exception: {0}", ex.Message));
    }
    return settings;
}
```
Hierdoor kunt u de API-profiel NuGet-pakketten gebruiken om uw toepassing is op Azure Stack te implementeren.

## <a name="samples-using-api-profiles"></a>Voorbeelden met behulp van API-profielen

De volgende voorbeelden kunnen worden gebruikt als uitgangspunt voor het maken van oplossingen met .NET en Azure Stack-API-profielen.
- [Resourcegroepen beheren](https://github.com/Azure-Samples/hybrid-resources-dotnet-manage-resource-group)
- [Storage-Accounts beheren](https://github.com/Azure-Samples/hybird-storage-dotnet-manage-storage-accounts)
- [Een virtuele Machine beheren](https://github.com/Azure-Samples/hybrid-compute-dotnet-manage-vm)

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
  [* de Azure Stack Resource Manager-eindpunt *]: ../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint
  [Overzicht van de API-profielen]: ../user/azure-stack-version-profiles.md#summary-of-api-profiles
  [Test Project to Virtual Machine, vNet, resource groups, and storage account]: https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject
  [Use Azure PowerShell to create a service principal with a certificate]: ../azure-stack-create-service-principals.md
  [Run unit tests with Test Explorer.]: /visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2017
