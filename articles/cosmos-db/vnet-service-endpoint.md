---
title: Toegang tot een Azure Cosmos DB-account beveiligen met behulp van Azure Virtual Network service-eindpunt | Microsoft Docs
description: Dit document beschrijft de vereiste stappen voor het service-eindpunt voor virtuele netwerk van Azure Cosmos DB setup.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: govindk
ms.openlocfilehash: cc64bc1af6eee505ccf3d38e214c49ffb7e3304c
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404457"
---
# <a name="secure-access-to-an-azure-cosmos-db-account-by-using-azure-virtual-network-service-endpoint"></a>Toegang tot een Azure Cosmos DB-account beveiligen met behulp van Azure Virtual Network service-eindpunt

Azure cosmos DB-accounts kunnen worden geconfigureerd zodat toegang alleen vanaf een specifiek subnet van het Virtueelnetwerk van Azure. Doordat een [Service-eindpunt](../virtual-network/virtual-network-service-endpoints-overview.md) voor Azure cosmos DB van een Virtueelnetwerk en een subnet, verkeer een route optimaal en veilig naar de Azure Cosmos DB is gewaarborgd.  

Azure Cosmos DB is een wereldwijd gedistribueerde, multi-model databaseservice. U kunt de gegevens aanwezig zijn in Azure Cosmos DB-account in meerdere regio's repliceren. Wanneer Azure Cosmos DB is geconfigureerd met een service-eindpunt voor virtueel netwerk, kan elk virtueel netwerk toegang van IP-adressen die deel uitmaken van specicifc subnet. De volgende afbeelding toont een voorbeeld van een Azure Cosmos DB met virtual network service-eindpunt is ingeschakeld:

![virtuele netwerkarchitectuur van eindpunt](./media/vnet-service-endpoint/vnet-service-endpoint-architecture.png)

Nadat een Azure Cosmos DB-account is geconfigureerd met een service-eindpunt voor virtueel netwerk, wordt deze alleen uit het opgegeven subnet kunnen worden geopend, wordt alle toegang tot het openbare/internet verwijderd. Om te leren op een gedetailleerde over service-eindpunten, verwijzen naar de Azure [overzicht van Virtual network service endpoints](../virtual-network/virtual-network-service-endpoints-overview.md) artikel.

## <a name="configure-service-endpoint-by-using-azure-portal"></a>Service-eindpunt configureren met behulp van Azure portal
### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Service-eindpunt voor een bestaande Azure-netwerk en subnet configureren

1. Van **alle resources** blade zoeken naar het virtuele netwerk die u wilt configureren van service-eindpunt voor Azure Cosmos DB. Navigeer naar de **Service-eindpunten** blade en zorg ervoor dat het subnet van het virtuele netwerk is ingeschakeld voor de service-eindpunt voor 'Azure.CosmosDB'.  

   ![Controleer of de service-eindpunt ingeschakeld](./media/vnet-service-endpoint/confirm-service-endpoint-enabled.png)

2. Van **alle resources** blade zoeken naar de Azure Cosmos DB account dat u wilt beveiligen.  

3. Voordat u service-eindpunt voor virtueel netwerk is ingeschakeld, kopieert u de IP-firewall-informatie die is gekoppeld aan uw Azure Cosmos DB-account voor toekomstig gebruik. U kunt IP-firewall opnieuw inschakelen na de configuratie van service-eindpunt.  

4. Selecteer **Firewalls en virtuele netwerken** van instellingenmenu en kies zodat toegang vanaf **geselecteerde netwerken**.  

3. Als u wilt toegang verlenen tot een bestaand virtueel netwerk subnet, onder virtuele netwerken, selecteer **toevoegen van bestaande Azure-netwerk**.  

4. Selecteer de **abonnement** uit die u wilt toevoegen van virtuele Azure-netwerk. Selecteer het Azure **virtuele netwerken** en **subnetten** die u wilt toegang bieden tot uw Azure Cosmos DB-account. Selecteert u vervolgens **inschakelen** voor het inschakelen van geselecteerde netwerken met service-eindpunten voor 'Microsoft.AzureCosmosDB'. Wanneer deze voltooid is, selecteert u **toevoegen**.  

   ![Selecteer het virtuele netwerk en subnet](./media/vnet-service-endpoint/choose-subnet-and-vnet.png)

   > [!NOTE]
   > Als service-eindpunt voor Azure Cosmos DB is niet eerder hebt geconfigureerd voor de geselecteerde Azure-netwerken en subnetten, kan deze worden geconfigureerd als onderdeel van deze bewerking. Inschakelen van toegang, duurt tot vijftien minuten om te voltooien. Het is heel belangrijk dat u de IP-firewall uitschakelen na noteert de inhoud van de firewall ACL voor renabling ze later opnieuw. 

   ![virtueel netwerk en subnet is geconfigureerd](./media/vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

Uw Azure Cosmos DB-account kunnen nu alleen verkeer van dit subnet gekozen. Als u IP-firewall eerder was ingeschakeld, opnieuw Schakel ze met behulp van oudere gegevens.

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Service-eindpunt voor een nieuwe Azure-netwerk en subnet configureren

1. Van **alle resources** blade zoeken naar de Azure Cosmos DB account dat u wilt beveiligen.  

   > [!NOTE]
   > Als u een bestaande IP-firewall is geconfigureerd voor uw Azure Cosmos DB-account hebt, houd er rekening mee de firewallconfiguratie, verwijderen van de IP-firewall en schakel vervolgens het Service-eindpunt. Als u het Service-eindpunt zonder disbling de firewall inschakelt, wordt het verkeer van deze ip-adresbereik de identiteit van de virtuele IP-gaan verloren en wordt deze verwijderd met een foutbericht van de IP-filter. Dus u moet de firewall-regels altijd uitschakelen om te voorkomen dat deze fout, kopiëren, service-eindpunt van het subnet en ten slotte op het subnet van Cosmos DB-ACL inschakelen. Nadat u service-eindpunt configureren en de ACL toevoegen kunt u opnieuw de IP-firewall opnieuw inschakelen indien nodig.

2. Voordat u service-eindpunt voor virtueel netwerk is ingeschakeld, kopieert u de IP-firewall-informatie die is gekoppeld aan uw Azure Cosmos DB-account voor toekomstig gebruik. U kunt IP-firewall opnieuw inschakelen na de configuratie van service-eindpunt.  

3. Selecteer **Firewalls en virtuele netwerken van Azure** van instellingenmenu en kies zodat toegang vanaf **geselecteerde netwerken**.  

4. Als u wilt toegang verlenen tot een nieuwe Azure-netwerk vindt onder virtuele netwerken, selecteer **nieuw virtueel netwerk toevoegen**.  

5. Geef de details die vereist voor het maken van een nieuw virtueel netwerk en selecteer vervolgens maken. Het subnet wordt gemaakt met een service-eindpunt voor 'Microsoft.AzureCosmosDB' ingeschakeld.

   ![Selecteer het virtuele netwerk en subnet voor nieuw virtueel netwerk](./media/vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

## <a name="allow-access-from-azure-portal"></a>Toegang vanuit Azure portal toestaan

Nadat Azure Virtual Network service-eindpunten zijn ingeschakeld voor het account van uw Azure Cosmos DB-database, wordt toegang vanuit de portal of andere Azure-services is standaard uitgeschakeld. Toegang tot uw Azure Cosmos DB-databaseaccount van computers buiten het geconfigureerde subnet worden geblokkeerd met inbegrip van toegang via de portal.

![Toegang vanuit de portal toestaan](./media/vnet-service-endpoint/allow-access-from-portal.png)

Als uw Azure Cosmos DB-account wordt gebruikt door andere Azure-services zoals Azure Search, of Stream analytics of Power BI vanuit, u toegang toestaan door het controleren van **toegang tot Azure-Services toestaan**.

Om te controleren of u toegang hebt tot Azure Cosmos DB metrische gegevens van de portal, moet u inschakelen **toestaan van toegang tot Azure portal** opties. Zie voor meer informatie over deze opties, [verbindingen van Azure portal](firewall-support.md#connections-from-the-azure-portal) en [verbindingen van Azure PaaS-services](firewall-support.md#connections-from-global-azure-datacenters-or-azure-paas-services) secties. Na het selecteren van toegang, selecteer **opslaan** de instellingen op te slaan.

## <a name="remove-a-virtual-network-or-subnet"></a>Een virtueel netwerk of subnet verwijderen 

1. Van **alle resources** blade, zoeken naar de Azure Cosmosdb-account waarvoor u service-eindpunten is toegewezen.  

2. Selecteer **Firewalls en virtuele netwerken** van instellingenmenu.  

3. Als u wilt verwijderen van een virtueel netwerk of subnet regel, selecteer "…" naast het virtuele netwerk of subnet en selecteer **verwijderen**.

   ![Een virtueel netwerk verwijderen](./media/vnet-service-endpoint/remove-a-vnet.png)

4.  Klik op **opslaan** de wijzigingen worden toegepast.

## <a name="configure-service-endpoint-by-using-azure-powershell"></a>Service-eindpunt configureren met behulp van Azure PowerShell 

Gebruik de volgende stappen uit Service-eindpunt voor een Azure Cosmos DB-account configureren met behulp van Azure PowerShell:  

1. Installeer de meest recente [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) en [aanmelding](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  Zorg ervoor dat u Houd er rekening mee de IP-firewall-instellingen en de IP-firewall verwijderen voordat u Service-eindpunt voor het account inschakelt.

  > [!NOTE]
  > Als u een bestaande IP-firewall is geconfigureerd voor uw Azure Cosmos DB-account hebt, houd er rekening mee de firewallconfiguratie, verwijderen van de IP-firewall en schakel vervolgens het Service-eindpunt. Als u het Service-eindpunt zonder disbling de firewall inschakelt, wordt het verkeer van deze ip-adresbereik de identiteit van de virtuele IP-gaan verloren en wordt deze verwijderd met een foutbericht van de IP-filter. Dus u moet de firewall-regels altijd uitschakelen om te voorkomen dat deze fout, kopiëren, service-eindpunt van het subnet en ten slotte op het subnet van Cosmos DB-ACL inschakelen. Nadat u service-eindpunt configureren en de ACL toevoegen kunt u opnieuw de IP-firewall opnieuw inschakelen indien nodig.

2. Voordat u service-eindpunt voor virtueel netwerk is ingeschakeld, kopieert u de IP-firewall-informatie die is gekoppeld aan uw Azure Cosmos DB-account voor toekomstig gebruik. U kunt IP-firewall opnieuw na het configureren van service-eindpunt.  

3. Schakel de service-eindpunt voor een bestaand subnet van een virtueel netwerk.  

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname  `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

4. Bereid u voor de activering van ACL op de cosmos DB-Account door ervoor te zorgen dat het virtuele netwerk en subnet service-eindpunt ingeschakeld voor Azure Cosmos DB hebben.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName  -ResourceGroupName $rgName
   ```

5. Eigenschappen van Azure Cosmos DB-account ophalen door de volgende cmdlet:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

6. Initialiseren van de variabelen voor later gebruik. Instellen van alle variabelen van de bestaande definitie van de account als u meerdere locaties hebt, moet u hen toevoegen als onderdeel van de matrix. In deze stap configureert u service-eindpunt voor virtueel netwerk ook door in te stellen van de variabele 'accountVNETFilterEnabled' op 'True'. Deze waarde wordt later opnieuw toegewezen aan de parameter 'isVirtualNetworkFilterEnabled'.  

   ```powershell
   $locations = @(@{})

   <# If you have read regions in addition to a write region, use the following code to set the $locations variable instead.

   $locations = @(@{"locationName"="<Write location>"; 
                 "failoverPriority"=0}, 
               @{"locationName"="<Read location>"; 
                  "failoverPriority"=1}) #>

   $consistencyPolicy = @{}
   $cosmosDBProperties = @{}

   $locations[0]['failoverPriority'] = $cosmosDBConfiguration.Properties.failoverPolicies.failoverPriority
   $locations[0]['locationName'] = $cosmosDBConfiguration.Properties.failoverPolicies.locationName

   $consistencyPolicy = $cosmosDBConfiguration.Properties.consistencyPolicy

   $accountVNETFilterEnabled = $True
   $subnetID = $vnProp.Id+"/subnets/" + $sname  
   $virtualNetworkRules = @(@{"id"=$subnetID})
   $databaseAccountOfferType = $cosmosDBConfiguration.Properties.databaseAccountOfferType
   ```

7. Eigenschappen van Azure Cosmos DB-account bijwerken met de nieuwe configuratie door het uitvoeren van de volgende cmdlets: 

   ```powershell
   $cosmosDBProperties['databaseAccountOfferType'] = $databaseAccountOfferType
   $cosmosDBProperties['locations'] = $locations
   $cosmosDBProperties['consistencyPolicy'] = $consistencyPolicy
   $cosmosDBProperties['virtualNetworkRules'] = $virtualNetworkRules
   $cosmosDBProperties['isVirtualNetworkFilterEnabled'] = $accountVNETFilterEnabled

   Set-AzureRmResource `
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName -Properties $CosmosDBProperties
   ```

8. Voer de volgende opdracht om te controleren of uw Azure Cosmos DB-account is bijgewerkt met de service-eindpunt voor virtueel netwerk die u hebt geconfigureerd in de vorige stap:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a name="add-virtual-network-service-endpoint-for-an-azure-cosmos-db-account-that-has-ip-firewall-enabled"></a>Toevoegen van service-eindpunt voor virtueel netwerk voor een Azure Cosmos DB-account met IP-firewall is ingeschakeld

1. Schakel eerst uit het IP-firewall de toegang tot Azure Cosmos DB-account.  

2. Eindpunt voor virtueel netwerk naar het Azure Cosmos DB-account inschakelen met behulp van een van de methoden die worden beschreven in de vorige secties.  

3. De IP-firewalltoegang opnieuw inschakelen. 

## <a name="test-the-access"></a>Test de toegang

Als u wilt controleren als de service-eindpunten voor Azure Cosmos DB zijn geconfigureerd zoals verwacht, gebruikt u de volgende stappen uit:

* Twee subnetten in een virtueel netwerk instellen als de front-end en back-end, Cosmos DB-service-eindpunt voor de back endsubnet inschakelen.  

* Toegang inschakelen in de Cosmos DB-account voor de back endsubnet.  

* Maak twee virtuele machines-een in back endsubnet en de andere in het front-endsubnet.  

* Probeer toegang tot de Azure Cosmos DB-account van beide virtuele machines. U moet geen verbinding maken van virtuele machine is gemaakt in het back endsubnet, maar niet uit werd aangemaakt in frontend-subnet. De aanvraag wordt fout met 404-fout wanneer u probeert verbinding te maken vanaf het frontendsubnet waarin wordt bevestigd dat de toegang tot Azure Cosmos DB is beveiligd met behulp van de service-eindpunt van het virtuele netwerk. De computer in het back endsubnet zich goed werken.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-happens-when-you-access-an-azure-cosmos-db-account-that-has-virtual-network-access-control-list-acl-enabled"></a>Wat gebeurt er wanneer u een Azure Cosmos DB-account dat is het virtuele netwerk lijst met ACL (Access Control) ingeschakeld?  

HTTP 404 fout wordt geretourneerd.  

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-cosmos-db-account-in-another-region-for-example-if-azure-cosmos-db-account-is-in-west-us-or-east-us-and-virtual-networks-are-in-multiple-regions-can-the-virtual-network-access-azure-cosmos-db"></a>Zijn de subnetten van een virtueel netwerk hebt gemaakt in verschillende regio's die toegang hebben tot een Azure Cosmos DB-account in een andere regio? Bijvoorbeeld, als Azure Cosmos DB-account in VS-West of VS-Oost en van het virtuele netwerk is zijn in meerdere regio's, het virtuele netwerk toegang tot Azure Cosmos DB?  

Ja, virtuele netwerken die in verschillende regio's zijn gemaakt door de nieuwe functie toegankelijk. 

### <a name="can-an-azure-cosmos-db-account-have-both-virtual-network-service-endpoint-and-a-firewall"></a>Kan een Azure Cosmos DB-account hebben zowel Service-eindpunt voor virtueel netwerk en een firewall?  

Ja, Virtual Network-Service-eindpunt en een firewall kunnen naast elkaar bestaan. In het algemeen moet u ervoor zorgen dat toegang tot de portal is altijd ingeschakeld voor het configureren van een service-eindpunt van het virtuele netwerk waarmee u kunt de metrische gegevens die zijn gekoppeld aan de container weergeven.

### <a name="can-i-accept-connections-from-within-public-azure-datacenters-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Kan ik 'Verbindingen accepteren van in de openbare Azure-datacenters' wanneer toegang tot de service-eindpunt is ingeschakeld voor Azure Cosmos DB?  

Dit is vereist alleen als u wilt dat uw Azure Cosmos DB-account om te worden geopend door andere Azure eerste partij services zoals Azure Data factory, een Azure Search of een service die wordt geïmplementeerd in de opgegeven Azure-regio.

### <a name="how-many-virtual-network-service-endpoints-are-allowed-for-azure-cosmos-db"></a>Het aantal virtual network-service-eindpunten zijn toegestaan voor Azure Cosmos DB?  

service-eindpunten van 64 virtuele netwerken zijn toegestaan voor een Azure Cosmos DB-account.

### <a name="what-is-the-relationship-between-service-endpoint-and-network-security-group-nsg-rules"></a>Wat is de relatie tussen regels voor het Service-eindpunt en een Netwerkbeveiligingsgroep (NSG)?  

NSG-regels in Azure Cosmos DB kunnen u toegang te beperken tot specifieke Azure Cosmos DB-IP-adresbereik. Als u wilt toestaan dat toegang tot een Azure Cosmos DB-exemplaar dat is opgenomen in een specifieke [regio](https://azure.microsoft.com/global-infrastructure/regions/), kunt u de regio in de volgende indeling: 

    AzureCosmosDB.<region name>

Voor meer informatie over NSG Zie tags [virtueel netwerk servicetags](../virtual-network/security-overview.md#service-tags) artikel. 
  
### <a name="what-is-relationship-between-an-ip-firewall-and-virtual-network-service-endpoint-capability"></a>Wat is de relatie tussen een IP-firewall en Virtual Network-service-eindpunt mogelijkheid?  

Deze twee functies vullen elkaar om te garanderen van de isolatie van Azure Cosmos DB-assets en ze zijn beveiligd. Met IP-garandeert firewall dat statische IP-adressen toegang Azure Cosmos DB-account tot krijgen.  

### <a name="can-an-on-premises-devices-ip-address-that-is-connected-through-azure-virtual-network-gatewayvpn-or-express-route-gateway-access-azure-cosmos-db-account"></a>Kan IP-adres van een on-premises apparaat dat is verbonden via Azure Virtual Network gateway(VPN) of Express route-gateway toegang tot Azure Cosmosdb-account?  

De IP-adres of IP-adresbereik van de on-premises-apparaat moet worden toegevoegd aan de lijst met statische IP-adressen voor toegang tot de Azure Cosmos DB-account.  

### <a name="what-happens-if-you-delete-a-virtual-network-that-has-service-endpoint-setup-for-azure-cosmos-db"></a>Wat gebeurt er als u een virtueel netwerk met setup van service-eindpunt voor Azure Cosmos DB verwijderen?  

Wanneer een virtueel netwerk wordt verwijderd, de ACL-gegevens die zijn gekoppeld aan dat Azure Cosmos DB is verwijderd en de interactie tussen een virtueel netwerk en het Azure Cosmos DB-account wordt verwijderd. 

### <a name="what-happens-if-an-azure-cosmos-db-account-that-has-virtual-network-service-endpoint-enabled-is-deleted"></a>Wat gebeurt er als een Azure Cosmos DB-account met service-eindpunt van een virtueel netwerk ingeschakeld is verwijderd?

De metagegevens die zijn gekoppeld aan de specifieke Azure Cosmos DB-account wordt verwijderd uit het subnet. En het is de verantwoordelijkheid van de eindgebruiker te verwijderen van het subnet en het virtuele netwerk dat wordt gebruikt.

### <a name="can-i-use-a-peered-virtual-network-to-create-service-endpoint-for-azure-cosmos-db"></a>Kan ik een gekoppeld virtueel netwerk gebruiken voor het maken van service-eindpunt voor Azure Cosmos DB?  

Nee, alleen direct virtueel netwerk en de subnetten service-eindpunten van Azure Cosmos DB kunnen maken.

### <a name="what-happens-to-the-source-ip-address-of-resource-like-virtual-machine-in-the-subnet-that-has-azure-cosmos-db-service-endpoint-enabled"></a>Wat gebeurt er met de bron-IP-adres van bron, zoals virtuele machine in het subnet waarvoor Azure Cosmos DB-service-eindpunt ingeschakeld?

Wanneer service-eindpunten zijn ingeschakeld, wordt de bron-IP-adressen van resources in uw virtuele netwerk subnet over van het gebruik van openbare IPV4-adressen in Azure Virtual Network van privé-adressen voor verkeer naar Azure Cosmos DB.

### <a name="does-azure-cosmos-db-reside-in-the-azure-virtual-network--provided-by-the-customer"></a>Azure Cosmos DB is bevindt zich in de Azure-netwerk dat is opgegeven door de klant?  

Azure Cosmos DB is een service met meerdere tenants met een openbaar IP-adres. Wanneer u de toegang tot een subnet van een virtueel Azure-netwerk beperken met behulp van de functie van de service-eindpunt, zijn toegang is niet toegestaan voor uw Azure Cosmos DB-account via het opgegeven Azure-netwerk en het subnet ervan bevindt.  Azure Cosmos DB-account bevindt zich niet in dit Virtueelnetwerk van Azure. 

### <a name="what-if-anything-will-be-logged-in-log-analytics-if-it-is-enabled"></a>Wat gebeurt er als worden geregistreerd in Log Analytics als deze is ingeschakeld?  

Azure Cosmos DB kunt u logboeken met IP-adres (zonder het laatste achttal werd) met de status 403 voor aanvraag geblokkeerd door de ACL verzenden.  

## <a name="next-steps"></a>Volgende stappen
Een firewall configureren voor Azure Cosmos DB Zie [firewall-ondersteuning](firewall-support.md) artikel.

