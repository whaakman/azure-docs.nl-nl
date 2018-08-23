---
title: Met behulp van API-versieprofielen met GO in Azure Stack | Microsoft Docs
description: Meer informatie over het gebruik van API-versieprofielen met GO in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 9ad4402098e938f72cf4b8c61cce8d0d46b5a147
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42058686"
---
# <a name="use-api-version-profiles-with-go-in-azure-stack"></a>API-versieprofielen met Go in Azure Stack gebruiken

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

## <a name="go-and-version-profiles"></a>Go en versie-profielen

Een profiel is een combinatie van verschillende resourcetypen met verschillende versies van verschillende services. Met behulp van een profiel kunt u combineren en matchen tussen verschillende resourcetypen. Profielen kunnen opgeven:

 - De stabiliteit voor uw toepassing met een vergrendeling aan specifieke API-versies.
 - Compatibiliteit voor uw toepassing met Azure Stack en regionale Azure-datacenters.

In de Go SDK profielen zijn beschikbaar onder de profielen / pad met de versie in de **jjjj-MM-DD** indeling. Nu de meest recente Azure Stack Profielversie is **2017-03-09**. Voor het importeren van een bepaalde service van een profiel, moet u de bijbehorende module importeren uit het profiel. Bijvoorbeeld, om te importeren **Compute** service **2017-03-09** profiel:

````go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/compute/mgmt/compute" 
````

## <a name="install-azure-sdk-for-go"></a>Azure SDK voor Go installeren

  1. Git installeren. Zie voor instructies [aan de slag - installeren van Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
  2. Installeer de [programmeertaal Go](https://golang.org/dl).  
  Profielen van de API voor Azure is Go versie 1.9 of hoger vereist.
  3. De Azure-SDK voor Go en de bijbehorende afhankelijkheden installeren met de volgende bash-opdracht:
  ```
    go get -u -d github.com/Azure/azure-sdk-for-go/...
  ```

### <a name="the-go-sdk"></a>De GO-SDK

U vindt meer informatie over de Azure SDK gaat u op:
- De Azure-SDK op Ga [installatie van de Azure SDK voor Go](https://docs.microsoft.com/go/azure/azure-sdk-go-install).
- De Go SDK van Azure is openbaar beschikbaar op GitHub op [azure sdk voor go](https://github.com/Azure/azure-sdk-for-go).

### <a name="go-autorest-dependencies"></a>Go-AutoRest-afhankelijkheden

De GO SDK is afhankelijk van de Azure-Ga-AutoRest-modules REST-aanvragen verzenden naar Azure Resource Manager-eindpunten. U moet voor het importeren van de Azure-Ga-AutoRest-module-afhankelijkheden van [Azure Go-AutoRest op GitHub](https://github.com/Azure/go-autorest). U vindt de installatie van bash-opdrachten in de **installeren** sectie.

## <a name="how-to-use-go-sdk-profiles-on-azure-stack"></a>GO SDK profielen gebruiken in Azure Stack

Een voorbeeld van Go-code uitvoeren in Azure Stack:
  1. Azure SDK voor Go en de bijbehorende afhankelijkheden installeren. Zie voor instructies voor de vorige sectie, [Azure SDK voor Go installeren](#install-azure-sdk-for-go).
  2. Informatie over de metagegevens van het Resource Manager-eindpunt ophalen. Het eindpunt retourneert een JSON-bestand met de informatie die is vereist voor het uitvoeren van uw Go-code.

  > [!Note]  
  > De **ResourceManagerUrl** is in de Azure Stack Development Kit (ASDK): `https://management.local.azurestack.external/`  
  > De **ResourceManagerUrl** in geïntegreerde systemen is: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`  
  > Om op te halen de metagegevens die vereist zijn: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`
  
  JSON-voorbeeldbestand:

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

  3. Als niet beschikbaar is, maakt u een abonnement en opslaan van de abonnements-ID moet later worden gebruikt. Zie voor meer informatie over het maken van een abonnement [abonnementen voor aanbiedingen maken in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm). 
  4. Een service-principal maken met 'Abonnement' bereik en **eigenaar** rol. Sla de service-principals ID en -geheim op. Zie voor meer informatie over het maken van een service-principal voor Azure Stack [service-principal maken](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#create-service-principal-for-azure-ad). Uw Azure Stack-omgeving is ingesteld.
  5. Een servicemodule importeren van het profiel van de Go SDK in uw code. De huidige versie van Azure Stack-profiel is **2017-03-09**. Bijvoorbeeld, voor het importeren van de module op basis van netwerk **2017-03-09** profieltype: 

  ````go
    package main 
    import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
  ````
  
  6. In de functie maken en verifiëren van een client met een **nieuw** aanroep van de Client-functie. Voor het maken van een virtueel netwerk-client, kunt u de volgende code:  

  ````go
  package main 

  import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 

  func main() { 
      vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
  ````

  Stel `<baseURI>` naar de **ResourceManagerUrl** waarde die wordt gebruikt in stap 2.
  Stel `<subscriptionID>` naar de **SubscriptionID** waarde genoteerd in stap 3.
  Zie voor het maken van tokens, verificatie in onderstaande sectie.  

  7. API-methoden aanroepen met behulp van de client die u in de vorige stap hebt gemaakt. Als u bijvoorbeeld het maken van een virtueel netwerk met behulp van onze client uit de vorige stap: 
  
````go
package main

import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network" 
func main() { 
  vnetC1ient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
  vnetClient .Authorizer = autorest.NewBearerAuthorizer(token)

  vnetClient .CreateOrUpdate( ) 
````
  
  Zie voor een compleet voorbeeld van het maken van een virtueel netwerk in Azure Stack met behulp van Go SDK-profiel, [voorbeeld](#example).

## <a name="authentication"></a>Verificatie

Als u de eigenschap Authorizer uit Azure Active Directory met behulp van Go SDK, de Go-AutoRest-modules te installeren. Deze modules moeten is al geïnstalleerd hebben met de installatie van de 'SDK gaan'; Als dit niet het geval is, installeert de [verificatiepakket op GitHub](https://github.com/Azure/go-autorest/tree/master/autorest/adal).

De Authorizer moet worden ingesteld als de authorizer voor de resource-client. Er zijn verschillende methoden voor het ophalen van een Authorizer; voor een volledige lijst Zie hier.

In deze sectie geeft een veelgebruikte manier authorizer tokens ophalen in Azure Stack met behulp van clientreferenties:

  1. Als een service-principal met de rol van eigenaar van het abonnement beschikbaar is, moet u deze stap overslaan. Anders maakt u een service-principal [instructies]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) en toe te wijzen een 'eigenaar'-rol binnen het bereik van uw abonnement [instructies]( https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal). De service principal toepassings-ID en geheim opslaan. 

  2. Importeren **adal** van Go-AutoRest-pakket in uw code. 
  
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
   
  Stel `<activeDirectoryEndpoint>` aan de waarde van "loginEndpoint" eigenschap uit de ResourceManagerUrl metagegevens opgehaald op de vorige sectie van dit document.
  Stel `<tenantID>` waarde aan uw Azure Stack-tenant-ID. 

  4. Maak ten slotte de token van een service-principal met behulp van de methode NewServicePrincipalToken uit adal-module. 

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
  Stel `<clientID>` naar de service-principal toepassing ID opgeslagen wanneer service-principal is gemaakt op de vorige sectie van dit document.  
  Stel `<clientSecret>` naar de service-principal toepassing geheim opgeslagen wanneer service-principal is gemaakt op de vorige sectie van dit document.  

## <a name="example"></a>Voorbeeld

In deze sectie wordt een voorbeeld van Go-code voor het virtueel netwerk maken in Azure Stack. Voor volledige voorbeelden van Go SDK, Zie [opslagplaats met Azure-SDk voor Go-voorbeelden](https://github.com/Azure-Samples/azure-sdk-for-go-samples). Azure Stack-voorbeelden zijn beschikbaar onder hybride / pad binnen de servicemappen van de opslagplaats.

> [!Note]  
> Controleer of het abonnement dat u gebruikt is voor het uitvoeren van de code in dit voorbeeld **netwerk** -resourceprovider als vermeld **geregistreerde**. Om deze te verifiëren, zoekt u het abonnement in de Azure Stack-portal en klikt u op **resourceproviders.**

1. Importeer de vereiste pakketten in uw code. U moet de meest recente profiel in Azure Stack gebruiken om de netwerk-module te importeren. 
  
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

2. Definieer de omgevingsvariabelen. Voor het maken van een virtueel netwerk moet u een resourcegroep hebt. 

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

3. Nu dat u de omgevingsvariabelen hebt gedefinieerd, Voeg een methode voor het verificatietoken te maken met behulp van **adal** pakket. Zie meer informatie over verificatie in de vorige sectie.
  
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

4. De belangrijkste methode toevoegen. De belangrijkste methode wordt eerst een token opgehaald met behulp van de methode die is gedefinieerd in de vorige stap. Vervolgens wordt een client met behulp van netwerk-module van het profiel. Ten slotte wordt een virtueel netwerk gemaakt. 
  
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
* [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md)  
