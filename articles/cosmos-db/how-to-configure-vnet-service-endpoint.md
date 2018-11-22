---
title: Virtueel netwerk- en toegangsbeheer op basis van een subnet voor uw Azure Cosmos-account configureren
description: Dit document beschrijft de vereiste stappen voor het service-eindpunt voor virtuele netwerk van Azure Cosmos DB setup.
author: kanshiG
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: 16cd959a83850a3bc940803cd23e7542e34825c8
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52283209"
---
# <a name="how-to-access-azure-cosmos-db-resources-from-virtual-networks"></a>Toegang tot Azure Cosmos DB-resources van virtuele netwerken

Azure cosmos DB-accounts kunnen worden geconfigureerd zodat toegang alleen vanaf een specifiek subnet van het Virtueelnetwerk van Azure. Er zijn twee stappen die nodig zijn om toegang te beperken tot Azure Cosmos-account met verbindingen van een subnet in een virtueel netwerk (VNET).
 
1. Schakel in het subnet voor het verzenden van het subnet en de VNET-identiteit met Azure Cosmos DB. U kunt dit doen door het inschakelen van service-eindpunt voor Azure Cosmos DB op de specifieke subnet.

1. Voeg een regel toe in het subnet op te geven als een bron waaruit, Azure Cosmos-account het account kan worden geopend.

> [!NOTE]
> Eenmaal service-eindpunt voor uw Azure-Cosmos-account is ingeschakeld op een subnet, de bron van het bereiken van Azure Cosmos DB verkeer verandert van openbaar IP-adres in een VNET en subnet. Het verkeer overschakelen geldt voor alle Azure-Cosmos-account om dit subnet wordt geopend. Als uw Azure Cosmos-account (s) op basis van IP-firewall om toe te staan van dit subnet, vraagt van de service is ingeschakeld subnet zou niet meer overeen met de IP-firewall-regels en ze worden geweigerd. Zie voor meer informatie, de stappen in [migreren van IP-firewallregel naar VNET Access Control List](#migrate-from-firewall-to-vnet) sectie van dit artikel. 

De volgende secties wordt beschreven hoe u VNET-service-eindpunt voor een Azure Cosmos-account configureren.

## <a id="configure-using-portal"></a>Service-eindpunt configureren met behulp van Azure portal

### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Service-eindpunt voor een bestaande Azure-netwerk en subnet configureren

1. Van **alle resources** blade zoeken naar de Azure-Cosmos account dat u wilt beveiligen.

1. Selecteer **Firewalls en virtuele netwerken** van instellingenmenu en kies zodat toegang vanaf **geselecteerde netwerken**.

1. Om toegang te verlenen aan een bestaand virtueel netwerk subnet, onder **virtuele netwerken**, selecteer **toevoegen van bestaande Azure-netwerk**.

1. Selecteer de **abonnement** uit die u wilt toevoegen van virtuele Azure-netwerk. Selecteer het Azure **virtuele netwerken** en **subnetten** die u wilt toegang bieden tot uw Azure Cosmos-account. Selecteert u vervolgens **inschakelen** voor het inschakelen van geselecteerde netwerken met service-eindpunten voor 'Microsoft.AzureCosmosDB'. Wanneer deze voltooid is, selecteert u **toevoegen**. 

   ![Selecteer het virtuele netwerk en subnet](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png)


1. Nadat de Azure Cosmos-account voor toegang tot vanuit een virtueel netwerk is ingeschakeld, kunnen er alleen verkeer van dit gekozen subnet worden. Het virtuele netwerk en subnet dat u hebt toegevoegd moeten worden weergegeven zoals in de volgende schermafbeelding:

   ![virtueel netwerk en subnet is geconfigureerd](./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

> [!NOTE]
> Om in te schakelen service-eindpunten voor virtueel netwerk, moet u de volgende abonnementsmachtigingen:
  * Abonnement met een VNET: Inzender voor netwerken
  * Abonnement met Azure Cosmos-account: Inzender voor DocumentDB-Account

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Service-eindpunt voor een nieuwe Azure-netwerk en subnet configureren

1. Van **alle resources** blade zoeken naar de Azure-Cosmos account dat u wilt beveiligen.

1. Selecteer **Firewalls en virtuele netwerken van Azure** van instellingenmenu en kies zodat toegang vanaf **geselecteerde netwerken**.  

1. Als u wilt toegang verlenen tot een nieuwe Azure-netwerk vindt onder virtuele netwerken, selecteer **nieuw virtueel netwerk toevoegen**.  

1. Geef de details die vereist voor het maken van een nieuw virtueel netwerk en selecteer vervolgens maken. Het subnet wordt gemaakt met een service-eindpunt voor 'Microsoft.AzureCosmosDB' ingeschakeld.

   ![Selecteer het virtuele netwerk en subnet voor nieuw virtueel netwerk](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

Als uw Azure Cosmos-account wordt gebruikt door andere Azure-services zoals Azure Search, of Stream analytics of Power BI vanuit, u toegang toestaan door het controleren van **verbindingen aanvaarden van in de openbare Azure-datacenters**.

Om te controleren of u toegang hebt tot Azure Cosmos DB metrische gegevens van de portal, moet u inschakelen **toegang toestaan vanuit Azure Portal** opties. Zie voor meer informatie over deze opties, aanvragen van Azure portal en aanvragen van het Azure PaaS-services secties van de configureren [IP-firewall](how-to-configure-firewall.md) artikel. Na het selecteren van toegang, selecteer **opslaan** de instellingen op te slaan.

## <a id="remove-vnet-or-subnet"></a>Een virtueel netwerk of subnet verwijderen 

1. Van **alle resources** blade vinden van het Azure Cosmos-account waarvoor u service-eindpunten is toegewezen.  

2. Selecteer **Firewalls en virtuele netwerken** van instellingenmenu.  

3. Als u wilt verwijderen van een virtueel netwerk of subnet regel, selecteer "…" naast het virtuele netwerk of subnet en selecteer **verwijderen**.

   ![Een virtueel netwerk verwijderen](./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png)

4.  Klik op **opslaan** de wijzigingen worden toegepast.

## <a id="configure-using-powershell"></a>Service-eindpunt configureren met behulp van Azure PowerShell

> [!NOTE]
> Wanneer u PowerShell of CLI, moet u de volledige lijst van VNET-ACL's en het IP-filters opgeven in de parameters, niet alleen degene die moeten worden toegevoegd.

Gebruik de volgende stappen uit service-eindpunt voor een Azure Cosmos-account configureren met behulp van Azure PowerShell:  

1. Installeer de meest recente [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) en [aanmelding](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  

1. Schakel de service-eindpunt voor een bestaand subnet van een virtueel netwerk.  

   ```powershell
   $rgname = "<Resource group name>"
   $vnName = "<Virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname  `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. VNET-informatie ophalen.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName `
     -ResourceGroupName $rgName
   ```

1. Eigenschappen van Azure Cosmos-account ophalen door de volgende cmdlet:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos account name>"

   $cosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Initialiseren van de variabelen voor later gebruik. Alle variabelen instellen vanuit de definitie van de bestaande account.

   ```powershell
   $locations = @()

   foreach ($readLocation in $cosmosDBConfiguration.Properties.readLocations) {
      $locations += , @{
         locationName     = $readLocation.locationName;
         failoverPriority = $readLocation.failoverPriority;
      }
   }

   $virtualNetworkRules = @(@{
      id = "$($vnProp.Id)/subnets/$sname";
   })

   if ($cosmosDBConfiguration.Properties.isVirtualNetworkFilterEnabled) {
      $virtualNetworkRules = $cosmosDBConfiguration.Properties.virtualNetworkRules + $virtualNetworkRules
   }
   ```

1. Eigenschappen van Azure Cosmos-account bijwerken met de nieuwe configuratie door het uitvoeren van de volgende cmdlets: 

   ```powershell
   $cosmosDBProperties = @{
      databaseAccountOfferType      = $cosmosDBConfiguration.Properties.databaseAccountOfferType;
      consistencyPolicy             = $cosmosDBConfiguration.Properties.consistencyPolicy;
      ipRangeFilter                 = $cosmosDBConfiguration.Properties.ipRangeFilter;
      locations                     = $locations;
      virtualNetworkRules           = $virtualNetworkRules;
      isVirtualNetworkFilterEnabled = $True;
   }

   Set-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName `
     -Properties $CosmosDBProperties
   ```

1. Voer de volgende opdracht om te controleren of uw Azure Cosmos-account is bijgewerkt met de service-eindpunt voor virtueel netwerk die u hebt geconfigureerd in de vorige stap:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a id="configure-using-cli"></a>Service-eindpunt configureren met behulp van Azure CLI 

1. De service-eindpunt voor een subnet in een virtueel netwerk inschakelen.

1. Bestaande Azure-Cosmos-account bijwerken met subnet ACL 's

   ```azurecli-interactive

   name="<Azure Cosmos account name>"
   resourceGroupName="<Resource group name>"

   az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/frontend"
   ```

1. Maak een nieuwe Azure Cosmos-account met subnet ACL 's

   ```azurecli-interactive
   az cosmosdb create \
    --name $name \
    --kind GlobalDocumentDB \
    --resource-group $resourceGroupName \
    --max-interval 10 \
    --max-staleness-prefix 200 \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/default"
   ```

## <a id="migrate-from-firewall-to-vnet"></a>Migreren van IP-firewallregel VNET-ACL 

De volgende stappen zijn alleen nodig voor Azure Cosmos-accounts met bestaande IP-firewallregels, zodat een subnet en u een VNET en subnet op basis van ACL's gebruiken in plaats van IP-firewallregel wilt maken.

Zodra de service-eindpunt voor Azure Cosmos-account is ingeschakeld voor een subnet, worden de aanvragen worden verzonden met bron met informatie over de VNET en subnet in plaats van een openbaar IP-adres. Dergelijke aanvragen kunnen daarom niet overeen met een IP-filter. Deze switch bron gebeurt voor alle Azure Cosmos-accounts die vanuit het subnet met de service-eindpunt ingeschakeld. Om te voorkomen dat uitvaltijd, gebruikt u de volgende stappen uit:

1. Eigenschappen van Azure Cosmos-account ophalen door de volgende cmdlet:

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Cosmos account name>"

   $cosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. De variabelen voor het gebruik ervan later opnieuw worden geïnitialiseerd. Alle variabelen instellen vanuit de definitie van de bestaande account. Voeg de VNET-ACL voor alle Azure-Cosmos-accounts worden geopend vanuit het subnet met de `ignoreMissingVNetServiceEndpoint` vlag.

   ```powershell
   $locations = @()

   foreach ($readLocation in $cosmosDBConfiguration.Properties.readLocations) {
      $locations += , @{
         locationName     = $readLocation.locationName;
         failoverPriority = $readLocation.failoverPriority;
      }
   }

   $subnetID = "Subnet ARM URL" e.g "/subscriptions/f7ddba26-ab7b-4a36-a2fa-7d01778da30b/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/subnet1"

   $virtualNetworkRules = @(@{
      id = $subnetID;
      ignoreMissingVNetServiceEndpoint = "True";
   })

   if ($cosmosDBConfiguration.Properties.isVirtualNetworkFilterEnabled) {
      $virtualNetworkRules = $cosmosDBConfiguration.Properties.virtualNetworkRules + $virtualNetworkRules
   }
   ```

1. Eigenschappen van Azure Cosmos-account bijwerken met de nieuwe configuratie door het uitvoeren van de volgende cmdlets:

   ```powershell
   $cosmosDBProperties = @{
      databaseAccountOfferType      = $cosmosDBConfiguration.Properties.databaseAccountOfferType;
      consistencyPolicy             = $cosmosDBConfiguration.Properties.consistencyPolicy;
      ipRangeFilter                 = $cosmosDBConfiguration.Properties.ipRangeFilter;
      locations                     = $locations;
      virtualNetworkRules           = $virtualNetworkRules;
      isVirtualNetworkFilterEnabled = $True;
   }

   Set-AzureRmResource `
      -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
      -ApiVersion $apiVersion `
      -ResourceGroupName $rgName `
      -Name $acctName `
      -Properties $CosmosDBProperties
   ```

1. Herhaal stappen 1-3 voor alle Azure Cosmos-accounts die u vanuit het subnet opent.

1.  Wacht 15 minuten en werk vervolgens het subnet van service-eindpunt inschakelen.

1.  Schakel de service-eindpunt voor een bestaand subnet van een virtueel netwerk.

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
      -ResourceGroupName $rgname `
      -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
      -Name $sname `
      -AddressPrefix $subnetPrefix `
      -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. IP-firewallregel voor het subnet verwijderen.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het configureren van een firewall voor Azure Cosmos DB [firewall-ondersteuning](firewall-support.md) artikel.
