---
title: API-versieprofielen gebruiken met Java in Azure Stack | Microsoft Docs
description: Meer informatie over het gebruik van API-versieprofielen met Java in Azure Stack.
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
ms.openlocfilehash: b5a876ea8b5cc70ee0ca0dcac8628c12dc2b009b
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413614"
---
# <a name="use-api-version-profiles-with-java-in-azure-stack"></a>API-versieprofielen gebruiken met Java in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De Java-SDK voor Azure Stack Resource Manager biedt hulpprogramma's waarmee u kunt bouwen en beheren van uw infrastructuur. Resourceproviders in de SDK zijn onder andere compute, networking, storage, app-services, en [KeyVault](../../key-vault/key-vault-whatis.md). De Java-SDK bevat API-profielen door afhankelijkheden op te nemen in het Pom.xml-bestand dat de juiste modules in het bestand startactiviteit wordt geladen. Echter, u kunt meerdere profielen als toevoegen afhankelijkheden, zoals de **2018-03-01-hybride**, of **nieuwste** als de Azure-profiel. Met behulp van deze afhankelijkheden, wordt de juiste module geladen zo in dat wanneer u het resourcetype maakt, u kunt selecteren welke API-versie van deze profielen die u wilt gebruiken. Hiermee kunt u het gebruik van de meest recente versies in Azure, tijdens het ontwikkelen van op basis van de meest recente versie van de API voor Azure Stack. Met de Java SDK, kunt een ervaring voor ontwikkelaars van echte hybride cloud. API-profielen in de Java SDK inschakelen ontwikkeling van hybride cloud, doordat u schakelen tussen de globale Azure-resources en resources in Azure Stack.

## <a name="java-and-api-version-profiles"></a>Java- en API-versieprofielen

Een API-profiel is een combinatie van resourceproviders en -API-versies. U kunt een API-profiel gebruiken om op te halen van de laatste, meest stabiele versie van elk resourcetype in een resource provider-pakket.

- Als u de nieuwste versies van alle services, gebruikt de **nieuwste** profiel als de afhankelijkheid.
    
   - Voor het gebruik van de meest recente profiel, de afhankelijkheid is **com.microsoft.azure**.

   - Als u de services die compatibel zijn met Azure Stack, gebruikt de **com.microsoft.azure.profile\_2018\_03\_01\_hybride** profiel.
    
      - Dit is om te worden opgegeven in het Pom.xml-bestand als een afhankelijkheid, die wordt automatisch modules geladen als u de juiste klasse in de vervolgkeuzelijst, kiezen zoals u zou met .NET doen.
        
          - Het begin van elke module wordt als volgt weergegeven:         
           `Import com.microsoft.azure.management.resources.v2018_03_01.ResourceGroup`
             

  - Afhankelijkheden er als volgt uitzien:
     ```xml
     <dependency>
     <groupId>com.microsoft.azure.profile_2018_03_01_hybrid</groupId>
     <artifactId>azure</artifactId>
     <version>1.0.0-beta</version>
     </dependency>
     ```

  - Gebruik voor het gebruik van specifieke API-versies voor een resourcetype in een specifieke resourceprovider, de specifieke API-versies met intellisense gedefinieerd.

Houd er rekening mee dat u alle van de opties in dezelfde toepassing kunt combineren.

## <a name="install-the-azure-java-sdk"></a>Installeer de Azure Java SDK

Gebruik de volgende stappen voor het installeren van de Java SDK:

1.  De officiële instructies om Git te installeren. Zie voor instructies [aan de slag - installeren van Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

2.  De officiële instructies voor het installeren van de [Java SDK](http://zulu.org/download/)) en [Maven](https://maven.apache.org/). De juiste versie is versie 8 van de Java Developer Kit. De juiste Apache Maven is versie 3.0 of hoger. De omgevingsvariabele JAVA_HOME moet worden ingesteld op de installatielocatie van de Java Development Kit om uit te voeren van de Quick Start. Zie voor meer informatie, [uw eerste functie maken met Java en Maven](../../azure-functions/functions-create-first-java-maven.md).

3.  De juiste om afhankelijkheidspakketten te installeren, opent u het bestand Pom.xml in uw Java-toepassing. Een afhankelijkheid toevoegen, zoals wordt weergegeven in de volgende code:

   ```xml  
   <dependency>
   <groupId>com.microsoft.azure.profile_2018_03_01_hybrid</groupId>
   <artifactId>azure</artifactId>
   <version>1.0.0-beta</version>
   </dependency>
   ```

4.  De pakketten die moeten worden geïnstalleerd, is afhankelijk van de Profielversie die u wilt gebruiken. De Pakketnamen van het voor de profielversies zijn:
    
   - **com.Microsoft.Azure.profile\_2018\_03\_01\_hybride**
   - **com.Microsoft.Azure**
      - **meest recente**

5.  Als niet beschikbaar is, maakt u een abonnement en de abonnements-ID voor later gebruik opslaan. Zie voor instructies over het maken van een abonnement [abonnementen voor aanbiedingen maken in Azure Stack](../azure-stack-subscribe-plan-provision-vm.md).

6.  Een service-principal maken en opslaan van de Client-ID en het Clientgeheim. Zie voor instructies over het maken van een service-principal voor Azure Stack [toepassingen toegang verlenen tot Azure Stack](../azure-stack-create-service-principals.md). Houd er rekening mee dat de client-ID ook wel bekend als de toepassings-ID wordt bij het maken van een service-principal.

7.  Zorg ervoor dat uw service-principal de rol van inzender of eigenaar heeft op uw abonnement. Zie voor instructies over hoe u een rol toewijzen aan service-principal [toepassingen toegang verlenen tot Azure Stack](../azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van de Azure-SDK voor .NET met Azure Stack, moet u de volgende waarden opgeven en vervolgens waarden met omgevingsvariabelen instellen. Als u wilt de omgevingsvariabelen instellen, raadpleegt u de instructies onder de tabel voor uw besturingssysteem.

| Waarde                     | Omgevingsvariabelen | Beschrijving                                                                                                                                                                                                          |
| ------------------------- | --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Tenant-id                 | TENANT_ID            | De waarde van uw Azure Stack [ <span class="underline">tenant-ID</span>](../azure-stack-identity-overview.md).                                                          |
| Client-id                 | CLIENT_ID             | De service principal toepassings-ID opgeslagen wanneer service-principal is gemaakt op de vorige sectie van dit document.                                                                                              |
| Abonnements-id           | ABONNEMENTS-ID      | De [ <span class="underline">abonnements-ID</span> ](../azure-stack-plan-offer-quota-overview.md#subscriptions) is hoe u toegang hebben tot aanbiedingen in Azure Stack.                |
| Clientgeheim             | WAARDE VOOR CLIENT_SECRET        | De service-principal toepassing geheim opgeslagen bij het service-principal is gemaakt.                                                                                                                                   |
| Resource Manager-eindpunt | EINDPUNT              | Zie [ <span class="underline">de Azure Stack resource manager-eindpunt</span>](../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint). |
| Locatie                  | RESOURCE_LOCATION    | Lokale voor Azure Stack                                                                                                                                                                                                |

Volg de instructies om de tenant-ID voor uw Azure Stack, [hier](../azure-stack-csp-ref-operations.md). Om in te stellen de omgevingsvariabelen, het volgende doen:

### <a name="microsoft-windows"></a>Microsoft Windows

De omgevingsvariabelen instellen in een Windows-opdrachtprompt, gebruik de volgende indeling:

```shell
Set Azure_Tenant_ID=<Your_Tenant_ID>
```

### <a name="macos-linux-and-unix-based-systems"></a>macOS, Linux en Unix-systemen

In op basis van Unix-systemen kunt u de volgende opdracht uit:

```shell
Export Azure_Tenant_ID=<Your_Tenant_ID>
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
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## <a name="existing-api-profiles"></a>Bestaande API-profielen

1.  **com.Microsoft.Azure.profile\_2018\_03\_01\_hybride**: nieuwste die is gebouwd voor Azure Stack-profiel. Gebruik dit profiel voor services meest compatibel is met Azure Stack, zolang er op het stempel 1808 of verdere.

2.  **com.Microsoft.Azure.profile\_2017\_03\_09\_profiel**: als u van een tijdstempel die lager is dan de build 1808 gebruikmaakt, dit profiel gebruiken.

3.  **com.Microsoft.Azure**: profiel met de nieuwste versies van alle services. Gebruik de nieuwste versies van alle services.

Zie voor meer informatie over Azure Stack- en API-profielen, de [samenvatting van de API-profielen](../user/azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-java-sdk-api-profile-usage"></a>Gebruik van Azure Java SDK API-profiel

De volgende code wordt geverifieerd door de service-principal in Azure Stack. Deze maakt u een token door de tenant-ID en de base verificatie, die specifiek is voor Azure Stack:

```java
AzureTokenCredentials credentials = new ApplicationTokenCredentials(client, tenant, key, AZURE_STACK)
                    .withDefaultSubscriptionId(subscriptionId);
            Azure azureStack = Azure.configure().withLogLevel(com.microsoft.rest.LogLevel.BASIC)
                    .authenticate(credentials, credentials.defaultSubscriptionId());
```

Hiermee kunt u de afhankelijkheden van API-profiel gebruiken om uw toepassing is op Azure Stack te implementeren.

## <a name="define-azure-stack-environment-setting-functions"></a>Azure Stack-omgeving instelling functies definiëren

Voor het registreren van de Azure Stack-cloud met de juiste eindpunten, gebruik de volgende code:

```java
AzureEnvironment AZURE_STACK = new AzureEnvironment(new HashMap<String, String>() {
                {
                    put("managementEndpointUrl", settings.get("audience"));
                    put("resourceManagerEndpointUrl", armEndpoint);
                    put("galleryEndpointUrl", settings.get("galleryEndpoint"));
                    put("activeDirectoryEndpointUrl", settings.get("login_endpoint"));
                    put("activeDirectoryResourceId", settings.get("audience"));
                    put("activeDirectoryGraphResourceId", settings.get("graphEndpoint"));
                    put("storageEndpointSuffix", armEndpoint.substring(armEndpoint.indexOf('.')));
                    put("keyVaultDnsSuffix", ".adminvault" + armEndpoint.substring(armEndpoint.indexOf('.')));
                }
            });
```

De `getActiveDirectorySettings` in de volgende code haalt u de eindpunten van de metagegevens-eindpunten. Stelt de omgevingsvariabelen van de aanroep die is gemaakt:

```java
public static HashMap<String, String>
getActiveDirectorySettings(String armEndpoint) {

HashMap<String, String> adSettings = new HashMap<String, String>();

try {

// create HTTP Client
HttpClient httpClient = HttpClientBuilder.create().build();

// Create new getRequest with below mentioned URL
HttpGet getRequest = new
HttpGet(String.format("%s/metadata/endpoints?api-version=1.0",
armEndpoint));

// Add additional header to getRequest which accepts application/xml
data
getRequest.addHeader("accept", "application/xml");

// Execute request and catch response
HttpResponse response = httpClient.execute(getRequest);
```

## <a name="samples-using-api-profiles"></a>Voorbeelden met behulp van API-profielen

U kunt de volgende GitHub-voorbeelden als verwijzingen gebruiken voor het maken van oplossingen met .NET en Azure Stack-API-profielen:

  - [Resourcegroepen beheren](https://github.com/viananth/resources-java-manage-resource-group/tree/stack/Hybrid)

  - [Storage-Accounts beheren](https://github.com/viananth/storage-java-manage-storage-accounts/tree/stack/Hybrid)

  - [Een virtuele Machine beheren](https://github.com/viananth/compute-java-manage-vm/tree/stack/Hybrid)

### <a name="sample-unit-test-project"></a>Voorbeeld van een Project voor de Test-eenheid 

1.  Kloon de opslagplaats met de volgende opdracht:
    
    `git clone https://github.com/viananth/resources-java-manage-resource-group/tree/stack/Hybrid`

2.  Een Azure-service-principal maken en toewijzen van een rol voor toegang tot het abonnement. Zie voor instructies over het maken van een service-principal [gebruik Azure PowerShell een service-principal maken met een certificaat](../azure-stack-create-service-principals.md).

3.  De volgende vereiste waarden ophalen:
    
   1.  Tenant-id
   2.  Client-id
   3.  Clientgeheim
   4.  Abonnements-id
   5.  Resource Manager-eindpunt
   6.  Resourcelocatie

4.  Stel de volgende omgevingsvariabelen met behulp van de gegevens die u hebt opgehaald uit de Service-Principal hebt gemaakt met behulp van de opdrachtprompt:
    
   1. exporteren van TENANT_ID = {uw tenant-id}
   2. exporteren van CLIENT_ID = {uw client-id}
   3. exporteren van de waarde voor CLIENT_SECRET = {uw clientgeheim}
   4. exporteren van de abonnements-id = {uw abonnements-id}
   5. exporteren van ARM_ENDPOINT = {uw Azure Stack Resource manager-URL}
   6. exporteren van RESOURCE_LOCATION = {locatie van Azure Stack}

   Gebruik in Windows, **ingesteld** in plaats van **exporteren**.

5.  Gebruik de `getactivedirectorysettings` code voor het ophalen van de arm-eindpunt voor metagegevens en de HTTP-client gebruiken om in te stellen van informatie over het eindpunt.

   ```java
   public static HashMap<String, String> getActiveDirectorySettings(String armEndpoint) {
   HashMap<String, String> adSettings = new HashMap<String,> String>();

   try {

   // create HTTP Client
   HttpClient httpClient = HttpClientBuilder.create().build();

   // Create new getRequest with below mentioned URL
   HttpGet getRequest = new
   HttpGet(String.format("%s/metadata/endpoints?api-version=1.0", armEndpoint));

   // Add additional header to getRequest which accepts application/xml data
   getRequest.addHeader("accept", "application/xml");

   // Execute request and catch response
   HttpResponse response = httpClient.execute(getRequest);
   ```

7.  In het pom.xml-bestand, moet u het hieronder om het profiel 2018-03-01-hybride gebruiken voor Azure Stack afhankelijkheid toevoegen. Met deze afhankelijkheid installeert de modules die zijn gekoppeld aan dit profiel voor de resourceproviders Compute, netwerken, opslag, KeyVault en App-Services.
    
   Houd er rekening mee dat u de meest recente profiel met doel-Azure kunt gebruiken:
        
   ```xml
   <dependency>
   <groupId>com.microsoft.azure.profile_2018_03_01_hybrid</groupId>
   <artifactId>azure</artifactId>
   <version>1.0.0-beta</version>
   </dependency>
   ```

8.  Voer de volgende regel in de opdrachtprompt die geopend is voor de omgevingsvariabelen worden ingesteld:
    
   ```shell
   mvn clean compile exec:java
   ```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over API-profielen:

- [Beheren van API-versieprofielen in Azure Stack](azure-stack-version-profiles.md)
- [Resource provider API-versies ondersteund door profielen](azure-stack-profiles-azure-resource-manager-versions.md)
