---
title: API-versieprofielen gebruiken met Ruby in Azure Stack | Microsoft Docs
description: Meer informatie over het gebruik van API-versieprofielen met Ruby in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: B82E4979-FB78-4522-B9A1-84222D4F854B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 0e7d624c77447f537e6d47ea19d9054c1117d742
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263646"
---
# <a name="use-api-version-profiles-with-ruby-in-azure-stack"></a>API-versieprofielen gebruiken met Ruby in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

## <a name="ruby-and-api-version-profiles"></a>Ruby- en API-versieprofielen

De Ruby-SDK voor Azure Stack Resource Manager biedt hulpprogramma's waarmee u kunt bouwen en beheren van uw infrastructuur. Resourceproviders in de SDK zijn Reken-, virtuele netwerken, en met de Ruby-taal. API-profielen in de Ruby-SDK inschakelen ontwikkeling van hybride cloud, doordat u schakelen tussen de globale Azure-resources en resources in Azure Stack.

Een API-profiel is een combinatie van resourceproviders en -versies. Een API-profiel kunt u verschillende resourcetypen combineren.

- Als u de nieuwste versies van alle services, gebruikt de **nieuwste** profiel van de Azure SDK rollup-gem.
- Als u de services die compatibel zijn met de Azure Stack, gebruikt de **V2017_03_09** profiel van de Azure SDK rollup-gem.
- Gebruik van de meest recente **api-versie** van een service, gebruikt u de **nieuwste** profiel van de specifieke gem. Bijvoorbeeld, als u wilt gebruikmaken van de laatste **api-versie** van compute-service alleen, gebruikt u de **nieuwste** profiel van de **Compute** gem.
- Gebruik van een specifieke **api-versie** voor een service, gebruikt u de specifieke API-versies zoals gedefinieerd in de gem.

> [!NOTE]
> U kunt alle van de opties in dezelfde toepassing combineren.

## <a name="install-the-azure-ruby-sdk"></a>Installeer de Azure SDK voor Ruby

- De officiële instructies voor het installeren van [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
- De officiële instructies voor het installeren van [Ruby](https://www.ruby-lang.org/en/documentation/installation/).
  - Tijdens het installeren, kiest u **Ruby toevoegen aan variabele PATH**.
  - De development kit installeren tijdens de Ruby-installatie als u wordt gevraagd.
  - Vervolgens installeert u de bundler met de volgende opdracht:  
    `Gem install bundler`
- Als niet beschikbaar is, maakt u een abonnement en opslaan van de abonnements-ID moet later worden gebruikt. Instructies voor het maken van een abonnement zijn [hier](../azure-stack-subscribe-plan-provision-vm.md).
- Een service-principal maken en sla de ID en -geheim op. Instructies voor het maken van een service-principal voor Azure Stack zijn [hier](../azure-stack-create-service-principals.md).
- Zorg ervoor dat uw service-principal de rol van inzender of eigenaar heeft op uw abonnement. Instructies over het toewijzen van rollen aan service-principal zijn [hier](../azure-stack-create-service-principals.md).

## <a name="install-the-rubygem-packages"></a>De Rubygem-pakketten installeren

U kunt de Azure-Rubygem-pakketten rechtstreeks installeren.

```Ruby  
gem install azure_mgmt_compute
gem install azure_mgmt_storage
gem install azure_mgmt_resources
gem install azure_mgmt_network
Or use them in your Gemfile.
gem 'azure_mgmt_storage'
gem 'azure_mgmt_compute'
gem 'azure_mgmt_resources'
gem 'azure_mgmt_network'
```

De Ruby-SDK van Azure Resource Manager is in preview en heeft waarschijnlijk belangrijke wijzigingen in de gebruikersinterface in toekomstige releases. Belangrijke wijzigingen kan duiden op een hogere getal in de secundaire versie.

## <a name="use-the-azuresdk-gem"></a>Gebruik de gem azure_sdk

De gem **azure_sdk**, is een totaal van alle ondersteunde gems in de Ruby-SDK. Deze gem bestaat uit een **nieuwste** profiel, die ondersteuning biedt voor de meest recente versie van alle services. Het geeft een versienummer profiel **V2017_03_09** profiel, dat is gebouwd voor Azure Stack.

U kunt het updatepakket azure_sdk gem installeren met de volgende opdracht:  

```Ruby  
gem install 'azure_sdk
```

## <a name="prerequisites"></a>Vereisten

Als u wilt gebruiken de Ruby-SDK van Azure met Azure Stack, moet u de volgende waarden opgeven en stel de waarden met omgevingsvariabelen. Zie de instructies onder de tabel voor uw besturingssysteem voor het instellen van de omgevingsvariabelen.

| Waarde | Omgevingsvariabelen | Description |
| --- | --- | --- | --- |
| Tenant-id | AZURE_TENANT_ID | De waarde van uw Azure Stack [tenant-ID](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-overview). |
| Client-id | AZURE_CLIENT_ID | De service principal toepassings-ID opgeslagen wanneer service-principal is gemaakt op de vorige sectie van dit document.  |
| Abonnements-id | AZURE_SUBSCRIPTION_ID | De [abonnements-ID](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) is hoe u toegang hebben tot aanbiedingen in Azure Stack. |
| Clientgeheim | AZURE_CLIENT_SECRET | De service-principal toepassing geheim opgeslagen bij het service-principal is gemaakt. |
| Resource Manager-eindpunt | ARM_ENDPOINT | Zie [de Azure Stack resource manager-eindpunt](#The-azure-stack-resource-manager-endpoint).  |

### <a name="the-azure-stack-resource-manager-endpoint"></a>De Azure Stack resource manager-eindpunt

De Microsoft Azure Resource Manager is een raamwerk waarmee beheerders kunnen implementeren, beheren en controleren van Azure-resources. Deze taken kunnen worden verwerkt in Azure Resource Manager als een groep, in plaats van afzonderlijk, in één bewerking.

U krijgt informatie over de metagegevens van het Resource Manager-eindpunt. Het eindpunt retourneert een JSON-bestand met de vereiste informatie op uw code wordt uitgevoerd.

 > [!NOTE]  
 > De **ResourceManagerUrl** is in de Azure Stack Development Kit (ASDK): `https://management.local.azurestack.external/` De **ResourceManagerUrl** in geïntegreerde systemen is: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
 > Om op te halen de metagegevens die vereist zijn: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
 JSON-voorbeeldbestand:

 ```json
 {
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
   "graphEndpoint": "https://graph.windows.net/",  
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": {
     "loginEndpoint": "https://login.windows.net/",
     "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
   }
 }
```

### <a name="set-environmental-variables"></a>Omgevingsvariabelen instellen

**Microsoft Windows**  
De omgevingsvariabelen instellen in een Windows-opdrachtprompt, gebruik de volgende indeling:  
`set AZURE_TENANT_ID=<YOUR_TENANT_ID>`

**Mac OS, Linux, Unix-of Linux-systemen en** In Unix-systemen, kunt u de volgende opdracht uit:  
`export AZURE_TENANT_ID=<YOUR_TENANT_ID>`

## <a name="existing-api-profiles"></a>Bestaande API-profielen

Het updatepakket Azure_sdk gem heeft de volgende twee profielen:

1. **V2017_03_09**  
  Het profiel is die is gebouwd voor Azure Stack. Gebruik dit profiel voor services op het meest compatibel zijn met de Azure Stack.
2. **Latest**  
  Profiel bestaat uit de meest recente versies van alle services. Gebruik de nieuwste versies van alle services.

Zie voor meer informatie over Azure Stack- en API-profielen, de [samenvatting van de API-profielen](azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-ruby-sdk-api-profile-usage"></a>Gebruik van Azure Ruby-SDK-API-profiel

U kunt de volgende code gebruiken exemplaar maken van een profiel-client. Deze parameter is alleen vereist voor Azure Stack of andere persoonlijke clouds. Globale Azure al deze instellingen in standaard.

```Ruby  
active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])

provider = MsRestAzure::ApplicationTokenProvider.new(
  ENV['AZURE_TENANT_ID'],
  ENV['AZURE_CLIENT_ID'],
  ENV['AZURE_CLIENT_SECRET'],
  active_directory_settings
)
credentials = MsRest::TokenCredentials.new(provider)
options = {
  credentials: credentials,
  subscription_id: subscription_id,
  active_directory_settings: active_directory_settings,
  base_url: ENV['ARM_ENDPOINT']
}

# Target profile built for Azure Stack
client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
```

De profiel-client kan worden gebruikt voor toegang tot afzonderlijke resourceproviders, zoals rekentijd, opslag en netwerk:

```Ruby  
# To access the operations associated with Compute
profile_client.compute.virtual_machines.get 'RESOURCE_GROUP_NAME', 'VIRTUAL_MACHINE_NAME'

# Option 1: To access the models associated with Compute
purchase_plan_obj = profile_client.compute.model_classes.purchase_plan.new

# Option 2: To access the models associated with Compute
# Notice Namespace: Azure::Profiles::<Profile Name>::<Service Name>::Mgmt::Models::<Model Name>
purchase_plan_obj = Azure::Profiles::V2017_03_09::Compute::Mgmt::Models::PurchasePlan.new
```

## <a name="define-azure-stack-environment-setting-functions"></a>Azure Stack-omgeving instelling functies definiëren

Als u wilt verifiëren van de service-principal naar het Azure Stack-omgeving, definieert u de eindpunten met behulp van `get_active_directory_settings()`. Deze methode maakt gebruik van de **ARM_Endpoint** omgevingsvariabele die u bij het maken van uw omgevingsvariabelen instellen:

```Ruby  
# Get Authentication endpoints using Arm Metadata Endpoints
def get_active_directory_settings(armEndpoint)
  settings = MsRestAzure::ActiveDirectoryServiceSettings.new
  response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
  status_code = response.code
  response_content = response.body
  unless status_code == "200"
    error_model = JSON.load(response_content)
    fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
  end
  result = JSON.load(response_content)
  settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
  settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
  settings
end
```

## <a name="samples-using-api-profiles"></a>Voorbeelden met behulp van API-profielen

U kunt de volgende voorbeelden op GitHub als verwijzingen voor het maken van oplossingen met Ruby- en -API van Azure Stack-profielen gevonden:

- [Azure-resources en -resourcegroepen beheren met Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)
- [Virtuele machines beheren met Ruby](https://github.com/Azure-Samples/compute-ruby-manage-vm/tree/master/Hybrid)
- [Implementeren van een SSH-VM met een sjabloon in Ruby ingeschakeld](https://github.com/Azure-Samples/resource-manager-ruby-template-deployment/tree/master/Hybrid)

### <a name="sample-resource-manager-and-groups"></a>Voorbeeld van Resource Manager en groepen

De als voorbeeld wilt uitvoeren, moet u controleren of u Ruby hebt geïnstalleerd. Als u van Visual Studio Code gebruikmaakt, downloadt u de Ruby-SDK-extensie.

> [!NOTE]  
> Krijgt u de opslagplaats voor het voorbeeld op "[beheren Azure-resources en resourcegroepen met Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)'.

1. Kloon de opslagplaats:

   ```bash
   git clone https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups.git
   ```

2. De afhankelijkheden met behulp van de bundel installeren:

   ```Bash
   cd resource-manager-ruby-resources-and-groups\Hybrid\
   bundle install
   ```

3. Maak een Azure service-principal met behulp van PowerShell en ophalen van de waarden die nodig zijn.

   Zie voor instructies over het maken van een service-principal [gebruik Azure PowerShell een service-principal maken met een certificaat](../azure-stack-create-service-principals.md).

   Er zijn waarden die nodig zijn:
   - Tenant-id
   - Client-id
   - Clientgeheim
   - Abonnements-id
   - Resource Manager-eindpunt

   Stel de volgende omgevingsvariabelen die met behulp van de gegevens die u hebt opgehaald uit de Service-Principal u hebt gemaakt.

   - exporteren van AZURE_TENANT_ID = {uw tenant-id}
   - exporteren van AZURE_CLIENT_ID = {uw client-id}
   - exporteren van AZURE_CLIENT_SECRET = {uw clientgeheim}
   - exporteren van AZURE_SUBSCRIPTION_ID = {uw abonnements-id}
   - exporteren van ARM_ENDPOINT = {uw AzureStack Resource manager-url}

   > [!NOTE]  
   > Op Windows, set te gebruiken in plaats van de export.

4. Zorg ervoor dat de locatie-variabele is ingesteld op de locatie van uw Azure Stack; bijvoorbeeld, `LOCAL="local"`.

5. Voeg de volgende coderegel toe als u met Azure Stack of andere persoonlijke clouds voor de juiste active directory-eindpunten:

   ```Ruby  
   active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])
   ```

6. In de variabele opties moet u de Active Directory-instellingen en de basis-URL om te werken met Azure Stack toevoegen:

   ```ruby  
   options = {
   credentials: credentials,
   subscription_id: subscription_id,
   active_directory_settings: active_directory_settings,
   base_url: ENV['ARM_ENDPOINT']
   }
   ```

7. Een profiel-client die gericht is op het Azure Stack-profiel maken:

   ```ruby  
   client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
   ```

8. Als u wilt verifiëren van de service-principal met Azure Stack, de eindpunten moeten worden gedefinieerd met behulp van **get_active_directory_settings()**. Deze methode maakt gebruik van de **ARM_Endpoint** omgevingsvariabele die u bij het maken van uw omgevingsvariabelen instellen:

   ```ruby  
   def get_active_directory_settings(armEndpoint)
     settings = MsRestAzure::ActiveDirectoryServiceSettings.new
     response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
     status_code = response.code
     response_content = response.body
     unless status_code == "200"
       error_model = JSON.load(response_content)
       fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Metadata Endpoints", response, error_model)
     end
     result = JSON.load(response_content)
     settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
     settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
     settings
   end
   ```

9. Voet het voorbeeld uit.

   ```ruby
   bundle exec ruby example.rb
   ```

## <a name="next-steps"></a>Volgende stappen

- [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
- [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md)  
