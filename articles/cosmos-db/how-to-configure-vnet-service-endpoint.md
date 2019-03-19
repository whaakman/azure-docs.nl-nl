---
title: Virtueel netwerk- en toegangsbeheer op basis van een subnet voor uw Azure Cosmos DB-account configureren
description: Dit document beschrijft de stappen voor het instellen van een service-eindpunt voor virtueel netwerk voor Azure Cosmos DB.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: efe85e017d883ca82414fe5ff10b5c2932366e48
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58101122"
---
# <a name="configure-access-from-virtual-networks-vnet"></a>De toegang van virtuele netwerken (VNet) configureren

U kunt Azure Cosmos DB-accounts voor toegang van alleen een specifiek subnet van een Azure-netwerk configureren. Om toegang te beperken tot een Azure Cosmos DB-account met verbindingen van een subnet in een virtueel netwerk:
 
1. Schakel in het subnet voor het verzenden van het subnet en de identiteit van virtueel netwerk met Azure Cosmos DB. U kunt dit doen door in te schakelen van een service-eindpunt voor Azure Cosmos DB op de specifieke subnet.

1. Voeg een regel toe in de Azure Cosmos DB-account om op te geven van het subnet als een bron waaruit de account kan worden geopend.

> [!NOTE]
> Wanneer een service-eindpunt voor uw Azure Cosmos DB-account is ingeschakeld op een subnet, schakelt u de bron van het verkeer die geschikt is voor Azure Cosmos DB uit een openbare IP-adres aan een virtueel netwerk en subnet. Het verkeer overschakelen geldt voor alle Azure Cosmos DB-account dat wordt geopend vanuit dit subnet. Als uw Azure Cosmos DB-accounts hebt een IP-gebaseerde firewall om toe te staan van dit subnet, aanvragen van het subnet service-functionaliteit niet meer overeen met de IP-firewall-regels en ze zijn geweigerd. 
>
> Zie voor meer informatie, de stappen die worden beschreven de [migreren van een IP-firewallregel naar een virtueel netwerk access control list](#migrate-from-firewall-to-vnet) sectie van dit artikel. 

De volgende secties wordt beschreven hoe u een service-eindpunt voor virtueel netwerk voor een Azure Cosmos DB-account configureren.

## <a id="configure-using-portal"></a>Een service-eindpunt configureren met behulp van de Azure-portal

### <a name="configure-a-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Een service-eindpunt voor een bestaande Azure-netwerk en subnet configureren

1. Uit de **alle resources** blade, zoeken naar de Azure Cosmos DB-account dat u wilt beveiligen.

1. Selecteer **Firewalls en virtuele netwerken** in het instellingenmenu en kies zodat toegang vanaf **geselecteerde netwerken**.

1. Om toegang te verlenen aan een bestaand virtueel netwerk subnet, onder **virtuele netwerken**, selecteer **toevoegen van bestaande Azure-netwerk**.

1. Selecteer de **abonnement** uit die u wilt toevoegen van een Azure-netwerk. Selecteer het Azure **virtuele netwerken** en **subnetten** dat u wilt toegang bieden tot uw Azure Cosmos DB-account. Selecteer vervolgens **inschakelen** voor het inschakelen van geselecteerde netwerken met service-eindpunten voor 'Microsoft.AzureCosmosDB'. Wanneer deze voltooid is, selecteert u **toevoegen**. 

   ![Selecteer het virtuele netwerk en subnet](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png)


1. Nadat de Azure Cosmos DB-account voor toegang vanuit een virtueel netwerk is ingeschakeld, kunnen er verkeer van alleen dit gekozen subnet worden. Het virtuele netwerk en subnet die u hebt toegevoegd moeten worden weergegeven zoals in de volgende schermafbeelding:

   ![virtueel netwerk en subnet is geconfigureerd](./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

> [!NOTE]
> Om in te schakelen service-eindpunten voor virtueel netwerk, moet u de volgende abonnementsmachtigingen:
>   * Abonnement met een virtueel netwerk: Inzender voor netwerken
>   * Abonnement met Azure Cosmos DB-account: Inzender voor DocumentDB-account
>   * Als uw virtuele netwerk en Azure Cosmos DB-account zich in verschillende abonnementen, zorg ervoor dat het abonnement dat is het virtuele netwerk ook heeft `Microsoft.DocumentDB` -resourceprovider geregistreerd. Zie voor het registreren van een resourceprovider [Azure resourceproviders en typen](../azure-resource-manager/resource-manager-supported-services.md) artikel. 

Hier vindt u de aanwijzingen voor het abonnement bij de resourceprovider registreren.

### <a name="configure-a-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Een service-eindpunt voor een nieuwe Azure-netwerk en subnet configureren

1. Uit de **alle resources** blade, zoeken naar de Azure Cosmos DB-account dat u wilt beveiligen.  

1. Selecteer **Firewalls en virtuele netwerken van Azure** in het instellingenmenu en kies zodat toegang vanaf **geselecteerde netwerken**.  

1. Om toegang te verlenen met een nieuwe Azure-netwerk, onder **virtuele netwerken**, selecteer **nieuw virtueel netwerk toevoegen**.  

1. Geef de details die vereist voor het maken van een nieuw virtueel netwerk en selecteer vervolgens **maken**. Het subnet wordt gemaakt met een service-eindpunt voor 'Microsoft.AzureCosmosDB' ingeschakeld.

   ![Selecteer een virtueel netwerk en subnet voor een nieuw virtueel netwerk](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

Als uw Azure Cosmos DB-account wordt gebruikt door andere Azure-services zoals Azure Search of toegankelijk is vanuit de Stream analytics of Power BI, u toegang toestaan door het selecteren van **verbindingen aanvaarden van in de openbare Azure-datacenters**.

Om ervoor te zorgen dat u toegang tot Azure Cosmos DB metrische gegevens van de portal hebt, moet u inschakelen **zodat toegang vanaf Azure portal** opties. Zie voor meer informatie over deze opties, de [een IP-firewall configureren](how-to-configure-firewall.md) artikel. Nadat u toegang wilt inschakelen, selecteert u **opslaan** de instellingen op te slaan.

## <a id="remove-vnet-or-subnet"></a>Een virtueel netwerk of subnet verwijderen 

1. Uit de **alle resources** blade, zoeken naar de Azure Cosmosdb-account waarvoor u service-eindpunten is toegewezen.  

2. Selecteer **Firewalls en virtuele netwerken** in het instellingenmenu.  

3. Als u wilt verwijderen van een virtueel netwerk of subnet regel, selecteer **...**  naast het virtuele netwerk of subnet, en selecteer **verwijderen**.

   ![Een virtueel netwerk verwijderen](./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png)

4.  Selecteer **opslaan** de wijzigingen worden toegepast.

## <a id="configure-using-powershell"></a>Een service-eindpunt configureren met behulp van Azure PowerShell

> [!NOTE]
> Wanneer u PowerShell of Azure CLI gebruikt, moet u de volledige lijst met IP-filters en virtueel netwerk ACL's opgeven in de parameters, niet alleen degene die moeten worden toegevoegd.

Gebruik de volgende stappen uit om te configureren van een service-eindpunt voor een Azure Cosmos DB-account met behulp van Azure PowerShell:  

1. Installeer [Azure PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) en [aanmelden](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  

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

1. Ophalen van informatie over virtuele netwerken.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName `
     -ResourceGroupName $rgName
   ```

1. Eigenschappen van het Azure Cosmos DB-account ophalen door de volgende cmdlet:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

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

1. Eigenschappen van Azure Cosmos DB-account bijwerken met de nieuwe configuratie door het uitvoeren van de volgende cmdlets: 

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

1. Voer de volgende opdracht om te controleren of uw Azure Cosmos DB-account is bijgewerkt met de service-eindpunt voor virtueel netwerk die u hebt geconfigureerd in de vorige stap:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a id="configure-using-cli"></a>Een service-eindpunt configureren met behulp van de Azure CLI 

1. De service-eindpunt voor een subnet in een virtueel netwerk inschakelen.

1. De bestaande Azure Cosmos DB-account bijwerken met subnet toegangsbeheerlijsten (ACL's).

   ```azurecli-interactive

   name="<Azure Cosmos DB account name>"
   resourceGroupName="<Resource group name>"

   az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/frontend"
   ```

1. Maak een nieuw Azure Cosmos DB-account met subnet ACL's.

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

## <a id="migrate-from-firewall-to-vnet"></a>Migreren van een IP-firewallregel naar een virtueel netwerk ACL 

Gebruik de volgende stappen alleen voor Azure Cosmos DB-accounts met bestaande IP-firewallregels waarmee een subnet, wanneer u een virtueel netwerk en subnet op basis van ACL's gebruiken in plaats van een IP-firewallregel wilt maken.

Nadat een service-eindpunt voor een Azure Cosmos DB-account is ingeschakeld voor een subnet, worden de aanvragen verzonden met de bron met virtueel netwerk en subnet informatie in plaats van een openbaar IP-adres. Deze aanvragen niet overeen met een IP-filter. Deze switch bron gebeurt voor alle Azure Cosmos DB-accounts die vanuit het subnet met een service-eindpunt ingeschakeld. Om te voorkomen uitvaltijd, gebruikt u de volgende stappen uit:

1. Eigenschappen van het Azure Cosmos DB-account ophalen door de volgende cmdlet:

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. De variabelen voor het gebruik ervan later opnieuw worden geïnitialiseerd. Alle variabelen instellen vanuit de definitie van de bestaande account. Het virtuele netwerk ACL toevoegen aan alle Azure Cosmos DB-accounts die toegankelijk zijn vanuit het subnet met de `ignoreMissingVNetServiceEndpoint` vlag.

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

1. Eigenschappen van Azure Cosmos DB-account bijwerken met de nieuwe configuratie door het uitvoeren van de volgende cmdlets:

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

1. Herhaal stappen 1-3 voor alle Azure Cosmos DB-accounts die toegankelijk zijn vanuit het subnet.

1.  Wacht 15 minuten en werk vervolgens het subnet zodat het service-eindpunt.

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

1. Verwijder de IP-firewallregel voor het subnet.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het configureren van een firewall voor Azure Cosmos DB de [Firewall-ondersteuning](firewall-support.md) artikel.
