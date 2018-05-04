---
title: Ga in Azure-Stack van profielen voor API-versie met | Microsoft Docs
description: Meer informatie over het gebruik van API-versie profielen met gaat u in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 84ca616856f363e4d3d68ab1cc45b97f7c589185
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="use-api-version-profiles-with-go-in-azure-stack"></a>API-versie profielen met gaat u in Azure-Stack gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

## <a name="go-and-version-profiles"></a>Ga en versie-profielen

Een profiel is een combinatie van verschillende brontypen met verschillende versies van andere services. Een profiel kunt u meng en verschillende brontypen overeenstemmen. Profielen kunnen opgeven:

 - De stabiliteit voor uw toepassing door af te grendelen op specifieke API-versies.
 - Compatibiliteit voor uw toepassing met Azure-Stack en regionale Azure-datacenters.

In de SDK gaat profielen zijn beschikbaar onder de profielen / pad met de versie in de **jjjj-MM-DD** indeling. Nu direct de nieuwste Azure-Stack Profielversie is **2017-03-09**. Voor het importeren van een bepaalde service uit een profiel, moet u de bijbehorende module importeren uit het profiel. Om bijvoorbeeld te importeren **Compute** service **2017-03-09** profiel:

````go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/compute/mgmt/compute" 
````

## <a name="install-azure-sdk-for-go"></a>Installeer Azure SDK voor Ga

  1. Installeer Git. Zie voor instructies [aan de slag - installeren van Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
  2. Installeer de [gaat programmeertaal](https://golang.org/dl).  
  Profielen van de API voor Azure is Ga versie 1,9 of hoger vereist.
  3. Ga Azure SDK en de bijbehorende afhankelijkheden installeren met de volgende bash-opdracht:
  ```
    go get -u -d github.com/Azure/azure-sdk-for-go/...
  ```

### <a name="the-go-sdk"></a>De Ga SDK

U vindt meer informatie over de Azure SDK gaat op:
- De Azure SDK op Go [installeren van de Azure SDK voor Ga](https://docs.microsoft.com/go/azure/azure-sdk-go-install).
- De Azure SDK gaat is algemeen beschikbaar op GitHub op [azure sdk voor Ga](https://github.com/Azure/azure-sdk-for-go).

### <a name="go-autorest-dependencies"></a>Ga AutoRest-afhankelijkheden

De SDK gaan, is afhankelijk van de modules Azure Ga-AutoRest REST aanvragen verzenden naar Azure Resource Manager-eindpunten. U moet voor het importeren van de afhankelijkheden van de module Azure Ga-AutoRest van [Azure Ga-AutoRest op GitHub](https://github.com/Azure/go-autorest). U vindt de installatie bash-opdrachten in de **installeren** sectie.

## <a name="how-to-use-go-sdk-profiles-on-azure-stack"></a>Hoe gaat SDK profielen op Azure-Stack gebruiken

Een voorbeeld van code gaat uitvoeren op Azure-Stack:
  1. Azure SDK voor gaan en de bijbehorende afhankelijkheden installeren. Zie voor instructies voor de vorige sectie, [Azure SDK installeren voor Ga](#install-azure-sdk-for-go).
  2. Informatie over de metagegevens ophalen van het Resource Manager-eindpunt. Het eindpunt retourneert een JSON-bestand met de informatie die is vereist voor het uitvoeren van uw code gaat.
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

  3. Als niet beschikbaar is, een abonnement maken en opslaan van de abonnements-ID moet later worden gebruikt. Zie voor meer informatie over het maken van een abonnement [abonnementen op aanbiedingen maken in Azure-Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
  4. Een service-principal maken met ' abonnementsbereik ' en **eigenaar** rol. De service-principals Sla-ID en geheim. Zie voor meer informatie over het maken van een service-principal voor Azure-Stack [service-principal maken](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-azure-ad). Uw Azure-Stack-omgeving is ingesteld.
  5. Een servicemodule importeren uit profiel SDK gaat u in uw code. De huidige versie van Azure-Stack-profiel is **2017-03-09**. Bijvoorbeeld, netwerk-module van importeren **2017-03-09** profieltype: 

  ````go
    package main 
    import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
  ````
  
  6. In de functie maken en verifiëren van een client met een **nieuw** aanroep van de Client-functie. U kunt de volgende code gebruiken voor het maken van een virtueel netwerk-client:  

  ````go
  package main 

  import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 

  func main() { 
      vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
  ````

  Stel `<baseURI>` naar de **ResourceManagerUrl** waarde die wordt gebruikt in stap 2.
  Stel `<subscriptionID>` naar de **abonnements-id** waarde uit stap drie opgeslagen.
  Zie verificatie maken token onderstaande sectie.  

  7. API-methoden worden aangeroepen met behulp van de client die u in de vorige stap hebt gemaakt. Als u bijvoorbeeld een virtueel netwerk maken met behulp van onze client van de vorige stap: 
  
````go
package main

import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 
func main() { 
  vnetC1ient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
  vnetClient .Authorizer = autorest.NewBearerAuthorizer(token)

  vnetClient .CreateOrUpdate( ) 
````
  
  Zie voor een compleet voorbeeld van het maken van een virtueel netwerk op Azure-Stack met SDK gaat profiel [voorbeeld](#example).

## <a name="authentication"></a>Verificatie

Als u de eigenschap Authorizer van Azure Active Directory met behulp van SDK gaat, moet u de Ga-AutoRest-modules installeren. Deze modules moeten is al geïnstalleerd hebben met de installatie 'SDK gaan'; Als dit niet het geval is, installeert de [verificatiepakket op GitHub](https://github.com/Azure/go-autorest/tree/master/autorest/adal).

De Authorizer moet worden ingesteld als de authorizer voor de resource-client. Er zijn verschillende methoden om op te halen van een Authorizer; voor een volledige lijst Zie hier.

Deze sectie geeft een veelgebruikte manier voor het verkrijgen van authorizer tokens op Azure-Stack door middel van de clientreferenties:

  1. Als een service-principal met de rol van eigenaar voor het abonnement beschikbaar is, moet u deze stap overslaan. Anders maakt u een service-principal [instructies]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) en wijs deze schijf een 'eigenaar'-rol binnen het bereik van uw abonnement [instructies]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal). Sla het service-principal toepassings-ID en het geheim. 

  2. Importeren **adal** pakket van Ga AutoRest in uw code. 
  
  ````go
  package main
  import "github.com/Azure/go-autorest/autorest/adal" 
  ````

  3. Een oauthConfig maken met behulp van de methode NewOAuthConfig van **adal** module. 
  
  ````go
  package main 

  import "github.com/Azure/go-autorest/autorest/ada1" 

  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
  ````
   
  Stel `<activeDirectoryEndpoint>` aan de waarde van 'loginEndpoint' eigenschap uit de metagegevens van de ResourceManagerUrl opgehaald op de vorige sectie van dit document.
  Stel `<tenantID>` waarde aan uw Azure-Stack-tenant-ID. 

  4. Maak ten slotte de principal-token van een service met behulp van de methode NewServicePrincipalToken met adal-module. 

  ````go
  package main 

  import "github.com/Azure/go-autorest/autorest/adal" 

  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
  ````
  
  Stel `<activeDirectoryResourceID>` op een van de waarden in de 'doelgroep' lijst van de metagegevens van de ResourceManagerUrl opgehaald op de vorige sectie van dit document.  
  Stel `<clientID>` voor de servicetoepassing principal ID opgeslagen wanneer service-principal is gemaakt op de vorige sectie van dit document.  
  Stel `<clientSecret>` voor de servicetoepassing principal geheim opgeslagen wanneer service-principal is gemaakt op de vorige sectie van dit document.  

## <a name="example"></a>Voorbeeld

Deze sectie toont een voorbeeld van code Ga virtueel netwerk maken op Azure-Stack. Voor volledige voorbeelden van SDK gaat Zie [Ga-SDk van Azure-voorbeelden opslagplaats](https://github.com/Azure-Samples/azure-sdk-for-go-samples). Azure Stack-voorbeelden zijn beschikbaar onder hybride / pad in servicemappen van de opslagplaats.
> [!note]  
> Controleer of het abonnement dat u gebruikt is om de code in dit voorbeeld uitvoert, **netwerk** resourceprovider vermeld als **geregistreerde**. Om deze te verifiëren, zoekt u het abonnement in de Stack van Azure-portal en klik op **resourceproviders.**

1. Importeer de vereiste pakketten in uw code. U moet het meest recente profiel op Azure-Stack gebruiken de netwerk-module te importeren. 
  
  ````go
  package main

  import (
      "context"
      "fmt"
      "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
      "github.com/Azure/go-autorest/autorest"
      "github.com/Azure/go-autorest/autorest/adal"
      "github.com/Azure/go-autorest/autorest/to"
  )
  ````

2. Definieer de omgevingsvariabelen. Houd er rekening mee dat voor het maken van een virtueel netwerk moet u een resourcegroep hebt. 

  ````go
  var (
      activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
      tenantID = "yourAzureStackTenantID"
      clientID = "yourServicePrincipalApplicationID"
      clientSecret = "yourServicePrincipalSecret"
      activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
      subscriptionID = "yourSubscriptionID"
      baseURI = "yourResourceManagerURL"
      resourceGroupName = "existingResourceGroupName"
  )
  ````

3. Nadat u de omgevingsvariabelen van uw hebt gedefinieerd, voegt u toe een methode voor het verificatietoken te maken met behulp van **adal** pakket. Zie informatie over verificatie in de vorige sectie.
  
  ````go
  //CreateToken creates a service principal token
  func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
      token, err = adal.NewServicePrincipalToken(
          *oauthConfig,
          clientID,
          clientSecret,
          activeDirectoryResourceID)
      return token, err
  }
  ````

4. De belangrijkste methode toevoegen. De belangrijkste methode haalt eerst een token met behulp van de methode die is gedefinieerd in de vorige stap. Vervolgens wordt een client met behulp van netwerk-module van profiel gemaakt. Ten slotte wordt gemaakt van een virtueel netwerk. 
  
````go
package main

import (
    "context"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
    "github.com/Azure/go-autorest/autorest"
    "github.com/Azure/go-autorest/autorest/adal"
    "github.com/Azure/go-autorest/autorest/to"
)

var (
    activeDirectoryEndpoint = "yourLoginEndpointFromResourceManagerUrlMetadata"
    tenantID = "yourAzureStackTenantID"
    clientID = "yourServicePrincipalApplicationID"
    clientSecret = "yourServicePrincipalSecret"
    activeDirectoryResourceID = "yourAudienceFromResourceManagerUrlMetadata"
    subscriptionID = "yourSubscriptionID"
    baseURI = "yourResourceManagerURL"
    resourceGroupName = "existingResourceGroupName"
)

//CreateToken creates a service principal token
func CreateToken() (adal.OAuthTokenProvider, error) {
    var token adal.OAuthTokenProvider
    oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        clientID,
        clientSecret,
        activeDirectoryResourceID)
    return token, err
}

func main() {
    token, _ := CreateToken()
    vnetClient := network.NewVirtualNetworksClientWithBaseURI(baseURI, subscriptionID)
    vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
    future, _ := vnetClient.CreateOrUpdate(
        context.Background(),
        resourceGroupName,
        "sampleVnetName",
        network.VirtualNetwork{
            Location: to.StringPtr("local"),
            VirtualNetworkPropertiesFormat: &network.VirtualNetworkPropertiesFormat{
                AddressSpace: &network.AddressSpace{
                    AddressPrefixes: &[]string{"10.0.0.0/8"},
                },
                Subnets: &[]network.Subnet{
                    {
                        Name: to.StringPtr("subnetName"),
                        SubnetPropertiesFormat: &network.SubnetPropertiesFormat{
                            AddressPrefix: to.StringPtr("10.0.0.0/16"),
                        },
                    },
                },
            },
        })
    err := future.WaitForCompletion(context.Background(), vnetClient.Client)
    if err != nil {
        fmt.Printf(err.Error())
        return
    }
}

````

## <a name="next-steps"></a>Volgende stappen
* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md)  
