---
title: API-versie profielen gebruiken met Ruby in Azure-Stack | Microsoft Docs
description: Meer informatie over API-versie profielen gebruiken met Ruby in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: B82E4979-FB78-4522-B9A1-84222D4F854B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 98d285bf8ec92e7b6baf709d4796ce2f4420c4f2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="use-api-version-profiles-with-ruby-in-azure-stack"></a>API-versie profielen gebruiken met Ruby in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

## <a name="ruby-and-api-version-profiles"></a>Profielen voor Ruby en API-versie

De SDK voor Ruby voor Azure Stack van Resource Manager biedt hulpprogramma's waarmee u kunt ontwikkelen en beheren van uw infrastructuur. Resourceproviders in de SDK bevatten compute, virtuele netwerken en opslag, waarbij de Ruby taal. API-profielen in de SDK voor Ruby inschakelen hybride cloud ontwikkeling doordat u schakelen tussen globale Azure bronnen en bronnen op Azure-Stack.

Een API-profiel is een combinatie van resourceproviders en -versies. Een API-profiel kunt u verschillende brontypen combineren.

 - Om te maken van de nieuwste versies van alle services, gebruikt u de **nieuwste** profiel van de Azure SDK rollup-gem.
 - U kunt de services die compatibel zijn met de Azure-Stack gebruiken de **V2017_03_09** profiel van de Azure SDK rollup-gem.
 - U kunt de nieuwste api-versie van een service, gebruiken de **nieuwste** profiel van de specifieke gem. Bijvoorbeeld: als u de nieuwste api-versie van de compute-service alleen gebruiken wilt, gebruiken de **nieuwste** profiel van de **Compute** gem.
 - Als u specifieke api-versie voor een service, gebruikt u de specifieke API-versies binnen de gem gedefinieerd.

> [!note] 
> U kunt alle van de opties in dezelfde toepassing combineren.

## <a name="install-the-azure-ruby-sdk"></a>Installeer de Azure SDK voor Ruby

 - Officiële instructies voor het installeren van [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
 - Officiële instructies voor het installeren van [Ruby](https://www.ruby-lang.org/en/documentation/installation/).
    - Tijdens het installeren kiezen **Ruby toevoegen voor de padvariabele**
    - Developer kit installeren tijdens de installatie van Ruby wanneer u wordt gevraagd.
    - Vervolgens installeert u bundler met de volgende opdracht:  
      `Gem install bundler`
 - Als niet beschikbaar is, een abonnement maken en opslaan van de abonnements-ID moet later worden gebruikt. Instructies voor het maken van een abonnement worden [hier](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
 - Maak een service-principal en sla de ID en het geheim. Instructies voor het maken van een service-principal voor Azure-Stack zijn [hier](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals). 
 - Zorg ervoor dat uw service-principal heeft de rol van Inzender/eigenaar op uw abonnement. Instructies voor de rol toewijzen aan de service-principal zijn [hier](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

## <a name="install-the-rubygem-packages"></a>De pakketten rubygem installeren

U kunt de pakketten azure rubygem rechtstreeks installeren.

````Ruby  
gem install azure_mgmt_compute
gem install azure_mgmt_storage
gem install azure_mgmt_resources
gem install azure_mgmt_network
Or use them in your Gemfile.
gem 'azure_mgmt_storage'
gem 'azure_mgmt_compute'
gem 'azure_mgmt_resources'
gem 'azure_mgmt_network'
````

Vergeet niet de SDK van Azure Resource Manager Ruby Preview-versie en moet waarschijnlijk afbreken van wijzigingen in de gebruikersinterface in toekomstige releases. Grote wijzigingen kan duiden op een hogere waarde in secundaire versie.

## <a name="usage-of-the-azuresdk-gem"></a>Informatie over het gebruik van de gem azure_sdk

De gem, azure_sdk, is een combinatie van alle ondersteunde gems in de SDK voor Ruby. Deze gem bestaat uit een **nieuwste** profiel die ondersteuning biedt voor de nieuwste versie van alle services. Het profiel voor een samengestelde geeft **V2017_03_09** profiel, dat is gebouwd voor Azure-Stack.

U kunt het updatepakket azure_sdk gem installeren met de volgende opdracht:  

````Ruby  
  gem install 'azure_sdk
````

## <a name="prerequisite"></a>Vereiste

Om te kunnen Ruby Azure SDK met de Azure-Stack gebruiken, moet u de volgende waarden opgeven en vervolgens waarden met omgevingsvariabelen instellen. Zie de instructies na de tabel voor uw besturingssysteem voor het instellen van de omgevingsvariabelen. 

| Waarde | Omgevingsvariabelen | Beschrijving | 
| --- | --- | --- | --- |
| Tenant-id | AZURE_TENANT_ID | De waarde van uw Azure-Stack [tenant-ID](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-overview). |
| Client-id | AZURE_CLIENT_ID | De service principal toepassings-ID opgeslagen wanneer de service-principal is gemaakt op de vorige sectie van dit document.  |
| Abonnements-id | AZURE_SUBSCRIPTION_ID | De [abonnements-ID](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) is hoe u toegang hebben tot aanbiedingen in Azure-Stack. |
| Clientgeheim | AZURE_CLIENT_SECRET | De principal servicetoepassing geheim opgeslagen bij het service-principal is gemaakt. |
| Resource Manager-eindpunt | ARM_ENDPOINT | Zie [Stack van het Azure resource manager endpoin](#The-azure-stack-resource-manager-endpoint).  |

### <a name="the-azure-stack-resource-manager-endpoint"></a>De Stack van Azure resource manager-eindpunt

De Microsoft Azure Resource Manager is een raamwerk waarmee beheerders te implementeren, beheren en bewaken van de Azure-resources. Azure Resource Manager kunt verwerken van deze taken als een groep, in plaats van afzonderlijk in één bewerking.

U kunt informatie over de metagegevens ophalen van het Resource Manager-eindpunt. Het eindpunt retourneert een JSON-bestand met de informatie die is vereist voor het uitvoeren van uw code.
  > [!note]  
  > De **ResourceManagerUrl** in Azure Stack Development Kit (ASDK) is: `https://management.local.azurestack.external/`  
  > De **ResourceManagerUrl** in geïntegreerde systemen is: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
  > Voor het ophalen van de metagegevens die vereist zijn: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
  Voorbeeld van JSON-bestand:

  ```json
  { "galleryEndpoint": "https://portal.local.azurestack.external:30015/",  
    "graphEndpoint": "https://graph.windows.net/",  
    "portal Endpoint": "https://portal.local.azurestack.external/", 
    "authentication": {
      "loginEndpoint": "https://login.windows.net/", 
      "audiences": ["https://management.<yourtenant>.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
    }
  }
  ```

### <a name="set-environmental-variables"></a>De omgevingsvariabelen instellen

**Microsoft Windows**  
Om in te stellen de omgevingsvariabelen in het Windows-opdrachtprompt, gebruik de volgende notatie:  
`set AZURE_TENANT_ID=<YOUR_TENANT_ID>`

**Mac OS, Linux en Unix-systemen**  
U kunt de opdracht zoals gebruiken in op basis van Unix-systemen:  
`export AZURE_TENANT_ID=<YOUR_TENANT_ID>`

## <a name="existing-api-profiles"></a>Bestaande API-profielen

Het updatepakket azure_sdk gem heeft de volgende twee profielen:

1. **V2017_03_09**  
  Het profiel is gebouwd voor Azure-Stack. Gebruik dit profiel voor services moet compatibel zijn met de Azure-Stack.
2. **meest recente**  
  Profiel bestaat uit de nieuwste versies van alle services. Gebruik de nieuwste versies van alle services.

Zie voor meer informatie over Azure-Stack en API-profielen, een [samenvatting van de API-profielen](azure-stack-version-profiles.md#summary-of-api-profiles).

## <a name="azure-ruby-sdk-api-profile-usage"></a>Gebruik van Azure SDK-API voor Ruby-profiel

De volgende regels moeten worden gebruikt bij het instantiëren van een client profiel. Deze parameter is alleen vereist voor Azure-Stack of andere persoonlijke clouds. Globale Azure heeft al deze instellingen standaard.

````Ruby  
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
````

De profiel-client kan worden gebruikt voor toegang tot afzonderlijke resourceproviders, zoals de berekenings-, opslag- en netwerk.

````Ruby  
# To access the operations associated with Compute
profile_client.compute.virtual_machines.get 'RESOURCE_GROUP_NAME', 'VIRTUAL_MACHINE_NAME'

# Option 1: To access the models associated with Compute
purchase_plan_obj = profile_client.compute.model_classes.purchase_plan.new

# Option 2: To access the models associated with Compute
# Notice Namespace: Azure::Profiles::<Profile Name>::<Service Name>::Mgmt::Models::<Model Name>
purchase_plan_obj = Azure::Profiles::V2017_03_09::Compute::Mgmt::Models::PurchasePlan.new
````

## <a name="define-azurestack-environment-setting-functions"></a>Definieer AzureStack omgeving instelling functies

Verificatie van de service-principal in de Azure-Stack-omgeving definiëren de eindpunten met **get_active_directory_settings()**. Deze methode maakt gebruik van de **ARM_Endpoint** omgevingsvariabele die u bij het maken van de omgevingsvariabelen instellen.

````Ruby  
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
````

## <a name="samples-using-api-profiles"></a>Voorbeelden weergegeven met behulp van API-profielen

U kunt de in de volgende voorbeelden gevonden in GitHub repositoreis als uitgangspunt om oplossingen te maken met Ruby en Stack-API van Azure-profielen:

 - [Azure-resources en -resourcegroepen beheren met Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)
 - [Virtuele machines beheren met Ruby](https://github.com/Azure-Samples/compute-ruby-manage-vm/tree/master/Hybrid)
 - [Een SSH implementeren met een sjabloon in Ruby virtuele machine ingeschakeld](https://github.com/Azure-Samples/resource-manager-ruby-template-deployment/tree/master/Hybrid)

### <a name="sample-resource-manager-and-groups"></a>Voorbeeld Resource Manager en groepen

Voor het uitvoeren van de steekproef, zorg ervoor dat u Ruby hebt geïnstalleerd. Als u van Visual Studio Code gebruikmaakt, download u de SDK voor Ruby als ook een uitbreiding. 

> [!note]  
> U kunt de opslagplaats ophalen voor het voorbeeld op '[beheren Azure-resources en resourcegroepen met Ruby](https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups/tree/master/Hybrid)'.

1. Kloon de opslagplaats.

    ````Bash
    git clone https://github.com/Azure-Samples/resource-manager-ruby-resources-and-groups.git
    ````

2. Installeer de bundel met afhankelijkheden.

    ````Bash
    cd resource-manager-ruby-resources-and-groups\Hybrid\
    bundle install
    ````

3. Maak een Azure-Service-Principal met behulp van PowerShell en ophalen van de waarden die nodig zijn. 

  Zie voor instructies over het maken van een service-principal [gebruik Azure PowerShell een service-principal maken met een certificaat](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

  Waarden die nodig zijn:
  - Tenant-id
  - Client-id
  - Clientgeheim
  - Abonnements-id
  - Resource Manager-eindpunt

  Stel de volgende omgevingsvariabelen met behulp van de informatie die u hebt opgehaald uit de Service-Principal u hebt gemaakt.

  - exporteren van AZURE_TENANT_ID = {uw tenant-id}
  - exporteren van AZURE_CLIENT_ID = {uw client-id}
  - exporteren van AZURE_CLIENT_SECRET = {{het clientgeheim}
  - exporteren van AZURE_SUBSCRIPTION_ID = {uw abonnements-id}
  - exporteren van ARM_ENDPOINT = {uw AzureStack Resource manager-url}

  > [!note]  
  > In Windows, set te gebruiken in plaats van de export.

4. Zorg ervoor dat de locatie-variabele is ingesteld op uw locatie AzureStack. Bijvoorbeeld lokale = "local"

5. In de volgende regel code toevoegen als u Azure-Stack of andere persoonlijke clouds toe te passen, de juiste active directory-eindpunten.

  ````Ruby  
  active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])
  ````

6. Binnen de variabele opties toevoegen de active directory-instellingen en de basis-URL voor gebruik met Azure-Stack. 

  ````Ruby  
  options = {
    credentials: credentials,
    subscription_id: subscription_id,
    active_directory_settings: active_directory_settings,
    base_url: ENV['ARM_ENDPOINT']
  }
  ````

7. Profiel-client die gericht is op het Azure-Stack-profiel maken:

  ````Ruby  
    client = Azure::Resources::Profiles::V2017_03_09::Mgmt::Client.new(options)
  ````

8. Verificatie van de service-principal met de Azure-Stack de eindpunten moeten worden gedefinieerd met behulp van **get_active_directory_settings()**. Deze methode maakt gebruik van de **ARM_Endpoint** omgevingsvariabele die u bij het maken van de omgevingsvariabelen instellen.

  ````Ruby  
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
  ````

9. Voet het voorbeeld uit.

  ````Ruby
    bundle exec ruby example.rb
  ````

## 

## <a name="next-steps"></a>Volgende stappen

* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md)  
