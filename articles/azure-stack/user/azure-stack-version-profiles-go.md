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
ms.date: 01/19/2019
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 3c5a261eb5216c2dca93243d11b5d21b1d92846f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55224153"
---
# <a name="use-api-version-profiles-with-go-in-azure-stack"></a>API-versieprofielen met Go in Azure Stack gebruiken

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

## <a name="go-and-version-profiles"></a>Go en versie-profielen

Een profiel is een combinatie van verschillende resourcetypen met verschillende versies van verschillende services. Met behulp van een profiel, kunt u combineren en matchen tussen verschillende resourcetypen. Profielen kunnen de volgende voordelen bieden:

- De stabiliteit voor uw toepassing met een vergrendeling aan specifieke API-versies.
- Compatibiliteit voor uw toepassing met Azure Stack en regionale Azure-datacenters.

In de Go SDK profielen zijn beschikbaar onder het pad profielen met de versie in de **jjjj-MM-DD** indeling. Op dit moment, is de meest recente versie van de API van Azure Stack-profiel **2017-03-09**. Voor het importeren van een bepaalde service van een profiel, de bijbehorende module te importeren uit het profiel. Bijvoorbeeld, om te importeren **Compute** service **2017-03-09** profiel, gebruikt u de volgende code:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/compute/mgmt/compute"
```

## <a name="install-azure-sdk-for-go"></a>Azure SDK voor Go installeren

1. Git installeren. Zie voor instructies [aan de slag - installeren van Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2. Installeer de [programmeertaal Go](https://golang.org/dl). Profielen van de API voor Azure is Go versie 1.9 of hoger vereist.
3. De Azure-SDK voor Go en de bijbehorende afhankelijkheden installeren met de volgende bash-opdracht:

   ```bash
   go get -u -d github.com/Azure/azure-sdk-for-go/...
   ```

### <a name="the-go-sdk"></a>De Go SDK

U vindt meer informatie over de SDK van Azure gaat u op de volgende koppelingen:

- De Azure-SDK op Ga [installatie van de Azure SDK voor Go](/go/azure/azure-sdk-go-install).
- De Go SDK van Azure is openbaar beschikbaar op GitHub in de [azure sdk voor go](https://github.com/Azure/azure-sdk-for-go) opslagplaats.

### <a name="go-autorest-dependencies"></a>Go-AutoRest-afhankelijkheden

De Go SDK is afhankelijk van de Azure **Go-AutoRest** modules REST-aanvragen verzenden naar Azure Resource Manager-eindpunten. U moet de Azure importeren **Go-AutoRest** module-afhankelijkheden van [Azure Go-AutoRest op GitHub](https://github.com/Azure/go-autorest). U vindt de installatie van bash-opdrachten in de **installeren** sectie.

## <a name="how-to-use-go-sdk-profiles-on-azure-stack"></a>Go SDK profielen gebruiken in Azure Stack

Als u wilt een voorbeeld van Go-code uitvoeren in Azure Stack, de volgende stappen uit:

1. Azure SDK voor Go en de bijbehorende afhankelijkheden installeren. Zie voor instructies de vorige sectie [Azure SDK voor Go installeren](#install-azure-sdk-for-go).
2. Informatie over de metagegevens van het Resource Manager-eindpunt ophalen. Het eindpunt retourneert een JSON-bestand met de informatie die is vereist voor het uitvoeren van uw Go-code.

   > [!NOTE]  
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

3. Als niet beschikbaar is, maakt u een abonnement en opslaan van de abonnements-ID moet later worden gebruikt. Zie voor meer informatie over het maken van een abonnement [abonnementen voor aanbiedingen maken in Azure Stack](../azure-stack-subscribe-plan-provision-vm.md).

4. Een service-principal met maken **abonnement** bereik en **eigenaar** rol. De service-principal-ID en geheim opslaan. Zie voor meer informatie over het maken van een service-principal voor Azure Stack [service-principal maken](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad). Uw Azure Stack-omgeving is nu ingesteld.

5. Een servicemodule importeren van het profiel voor Go SDK in uw code. De huidige versie van Azure Stack-profiel is **2017-03-09**. Bijvoorbeeld, voor het importeren van de module op basis van netwerk **2017-03-09** profieltype, gebruikt u de volgende code:

   ```go
   package main
    import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
   ```

6. In de functie maken en verifiëren van een client met een **nieuw** aanroep van de client-functie. Voor het maken van een virtueel netwerk-client, kunt u de volgende code:  

   ```go
   package main

   import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"

   func main() {
      vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
      vnetClient.Authorizer = autorest.NewBearerAuthorizer(token)
   ```

   Stel `<baseURI>` naar de **ResourceManagerUrl** waarde die wordt gebruikt in stap 2. Stel `<subscriptionID>` naar de **SubscriptionID** waarde genoteerd in stap 3.

   Zie de volgende sectie voor het maken van het token.  

7. API-methoden aanroepen met behulp van de client die u in de vorige stap hebt gemaakt. Bijvoorbeeld, voor het maken van een virtueel netwerk met behulp van de client uit de vorige stap, Zie het volgende voorbeeld:

   ```go
   package main

   import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
   func main() {
   vnetClient := network.NewVirtualNetworksClientWithBaseURI("<baseURI>", "(subscriptionID>")
   vnetClient .Authorizer = autorest.NewBearerAuthorizer(token)

   vnetClient .CreateOrUpdate( )
   ```

Zie voor een compleet voorbeeld van het maken van een virtueel netwerk in Azure Stack met behulp van de Go SDK-profiel, de [voorbeeld](#example).

## <a name="authentication"></a>Verificatie

Aan de **Authorizer** eigenschap uit Azure Active Directory met behulp van de Go SDK installeren de **Go-AutoRest** modules. Deze modules moeten is al geïnstalleerd hebben met de installatie van de 'SDK gaan'; Als dit niet het geval is, installeert de [verificatiepakket vanuit GitHub](https://github.com/Azure/go-autorest/tree/master/autorest/adal).

De Authorizer moet worden ingesteld als de authorizer voor de resource-client. Er zijn verschillende manieren authorizer tokens ophalen in Azure Stack met behulp van clientreferenties:

1. Als een service-principal met de rol van eigenaar van het abonnement beschikbaar is, moet u deze stap overslaan. Anders maakt u een [service-principal](azure-stack-create-service-principals.md) en hieraan een rol 'eigenaar' [binnen het bereik van uw abonnement](azure-stack-create-service-principals.md#assign-the-service-principal-to-a-role). De service principal toepassings-ID en geheim opslaan.

2. Importeren van de **adal** van Go-AutoRest-pakket in uw code.

   ```go
   package main
   import "github.com/Azure/go-autorest/autorest/adal"
   ```

3. Maak een **oauthConfig** met behulp van de methode NewOAuthConfig van **adal** module.

   ```go
   package main

   import "github.com/Azure/go-autorest/autorest/ada1"

   func CreateToken() (adal.OAuthTokenProvider, error) {
      var token adal.OAuthTokenProvider
      oauthConfig, err := adal.NewOAuthConfig(activeDirectoryEndpoint, tenantID)
   }
   ```

   Stel `<activeDirectoryEndpoint>` aan de waarde van de `loginEndpoint` eigenschap uit de `ResourceManagerUrl` metagegevens opgehaald op de vorige sectie van dit document. Stel de `<tenantID>` waarde aan uw Azure Stack-tenant-ID.

4. Ten slotte de token van een service-principal maken met behulp van de `NewServicePrincipalToken` methode van de **adal** module:

   ```go
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
   ```

    Stel `<activeDirectoryResourceID>` op een van de waarden in de lijst 'doelgroep' uit de **ResourceManagerUrl** metagegevens opgehaald in de vorige sectie van dit artikel.
    Stel `<clientID>` naar de service-principal toepassing ID opgeslagen bij het service-principal is gemaakt in de vorige sectie van dit artikel.
    Stel `<clientSecret>` naar de service principal toepassingsgeheim opgeslagen wanneer de service-principal is gemaakt in de vorige sectie van dit artikel.

## <a name="example"></a>Voorbeeld

In dit voorbeeld wordt een voorbeeld van Go-code die u een virtueel netwerk in Azure Stack maakt. Zie voor volledige voorbeelden van de Go SDK, de [opslagplaats met Azure-SDK voor Go-voorbeelden](https://github.com/Azure-Samples/azure-sdk-for-go-samples). Azure Stack-voorbeelden zijn beschikbaar onder het pad van de hybride binnen de servicemappen van de opslagplaats.

> [!NOTE]  
> Controleer of het abonnement dat u gebruikt is voor het uitvoeren van de code in dit voorbeeld de **netwerk** -resourceprovider als vermeld **geregistreerde**. Om te controleren, zoekt u het abonnement in de Azure Stack-portal en selecteer **resourceproviders.**

1. Importeer de vereiste pakketten in uw code. Gebruik de meest recente profiel in Azure Stack om de netwerk-module te importeren:

   ```go
   package main

   import (
       "context"
       "fmt"
       "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/network/mgmt/network"
       "github.com/Azure/go-autorest/autorest"
       "github.com/Azure/go-autorest/autorest/adal"
       "github.com/Azure/go-autorest/autorest/to"
   )
   ```

2. Definieer de omgevingsvariabelen. Voor het maken van een virtueel netwerk, moet u een resourcegroep hebt.

   ```go
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
   ```

3. Nu dat u de omgevingsvariabelen hebt gedefinieerd, voeg de methode voor het maken van een toegangstoken voor verificatie met behulp van de **adal** pakket. Zie de vorige sectie voor meer informatie over verificatie.

   ```go
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
   ```

4. Voeg de `main` methode. De `main` methode wordt eerst een token opgehaald met behulp van de methode die is gedefinieerd in de vorige stap. Vervolgens wordt een client met behulp van een netwerk-module van het profiel. Ten slotte wordt een virtueel netwerk gemaakt.

   ```go
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
   ```

## <a name="next-steps"></a>Volgende stappen

- [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
- [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md)  
