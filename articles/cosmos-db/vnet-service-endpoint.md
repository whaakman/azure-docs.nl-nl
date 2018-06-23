---
title: Beveiligde toegang tot een Cosmos-DB Azure-account met behulp van Azure Virtual Network service-eindpunt | Microsoft Docs
description: Dit document beschrijft de vereiste stappen voor het service-eindpunt voor setup Azure Cosmos DB virtueel netwerk.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: govindk
ms.openlocfilehash: de52521824c146f63fb16e2690e2a24167ae2efe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333909"
---
# <a name="secure-access-to-an-azure-cosmos-db-account-by-using-azure-virtual-network-service-endpoint"></a>Veilige toegang tot een Cosmos-DB Azure-account met behulp van Azure Virtual Network service-eindpunt

Azure CosmosDB accounts kunnen worden geconfigureerd om alleen toegankelijk is vanaf specifiek subnet van het virtuele netwerk van Azure. Doordat een [Service-eindpunt](../virtual-network/virtual-network-service-endpoints-overview.md) voor Azure CosmosDB van een virtueel netwerk en een subnet, verkeer een route optimaal en veilig naar de Cosmos Azure DB is gewaarborgd.  

Azure Cosmos DB is een wereldwijd gedistribueerde, multi-model databaseservice. U kunt gegevens in Azure DB die Cosmos-account repliceren naar meerdere regio's. Wanneer Azure Cosmos DB is geconfigureerd met een service-eindpunt van het virtuele netwerk, kan elke virtuele netwerk toegang van IP-adressen die deel uitmaken van specicifc subnet. De volgende afbeelding toont een voorbeeld van een Azure Cosmos-database met de service-eindpunt van een virtueel netwerk ingeschakeld:

![virtuele netwerkarchitectuur van eindpunt](./media/vnet-service-endpoint/vnet-service-endpoint-architecture.png)

Zodra een Cosmos-DB Azure-account is geconfigureerd met een service-eindpunt van het virtuele netwerk, wordt deze toegankelijk is alleen van het opgegeven subnet, wordt alle toegang tot het openbare/internet verwijderd. Voor meer gedetailleerde over service-eindpunten die verwijzen naar de Azure [Virtual network service-eindpunten overzicht](../virtual-network/virtual-network-service-endpoints-overview.md) artikel.

> [!NOTE]
> Virtual Network service-eindpunten kunnen op dit moment worden geconfigureerd voor SQL-API van Azure Cosmos DB of Mongo-API-accounts. Mogelijkheid voor het service-eindpunten configureren voor andere API's en soevereine clouds zoals Duitse Azure of Azure Government zijn binnenkort beschikbaar. Als u een bestaande IP-firewall is geconfigureerd voor uw Azure DB die Cosmos-account hebt, houd er rekening mee configuratie van de firewall, verwijdert u de firewall van het IP- en configureer vervolgens het service-eindpunt-ACL. Nadat u service-eindpunt hebt geconfigureerd, kunt u de IP-firewall weer inschakelen indien nodig.

## <a name="configure-service-endpoint-by-using-azure-portal"></a>Service-eindpunt configureren met behulp van Azure-portal
### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Service-eindpunt voor een bestaande virtuele Azure-netwerk en subnet configureren

1. Van **alle resources** blade zoeken naar het virtuele netwerk die u wilt het service-eindpunt configureren voor Azure Cosmos DB. Navigeer naar de **Service-eindpunten** blade en zorg ervoor dat het subnet van het virtuele netwerk is ingeschakeld voor de service-eindpunt 'Azure.CosmosDB'.  

   ![Controleer de service-eindpunt is ingeschakeld](./media/vnet-service-endpoint/confirm-service-endpoint-enabled.png)

2. Van **alle resources** blade zoeken naar de database van de Cosmos Azure account u wilt beveiligen.  

3. Voordat u service-eindpunt voor virtueel netwerk, kopieert u de IP-firewall-informatie die is gekoppeld aan uw Azure DB die Cosmos-account voor toekomstig gebruik. U kunt IP-firewall opnieuw inschakelen na het configureren van de service-eindpunt.  

4. Selecteer **Firewalls en virtuele netwerken** vanuit het menu instellingen en kies toegang toestaan via **netwerken geselecteerd**.  

3. Selecteer om toegang tot het subnet een bestaand virtueel netwerk onder virtuele netwerken **bestaande virtuele Azure-netwerk toevoegen**.  

4. Selecteer de **abonnement** waaruit u wilt toevoegen van virtuele Azure-netwerk. Selecteer de Azure **virtuele netwerken** en **subnetten** die u wilt toegang bieden tot uw Azure DB die Cosmos-account. Selecteert u vervolgens **inschakelen** geselecteerde netwerken met de service-eindpunten inschakelen voor 'Microsoft.AzureCosmosDB'. Wanneer deze voltooid is, selecteert u **toevoegen**.  

   ![Selecteer het virtueel netwerk en subnet](./media/vnet-service-endpoint/choose-subnet-and-vnet.png)

   > [!NOTE]
   > Als service-eindpunt voor Azure Cosmos DB eerder voor de geselecteerde virtuele Azure-netwerken en subnetten niet is geconfigureerd, kan worden geconfigureerd als onderdeel van deze bewerking. Het inschakelen van toegang, duurt maximaal 15 minuten duren. Het is belangrijk de IP-firewall uitschakelen na Let op de inhoud van de firewall ACL voor renabling ze later. 

   ![virtueel netwerk en subnet geconfigureerd](./media/vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

Nu worden uw Azure DB die Cosmos-account mag alleen verkeer vanaf deze gekozen subnet. Als u IP-firewall eerder was ingeschakeld, opnieuw Schakel ze met behulp van oudere gegevens.

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Service-eindpunt voor een nieuwe virtuele Azure-netwerk en subnet configureren

1. Van **alle resources** blade zoeken naar de database van de Cosmos Azure account u wilt beveiligen.  

> [!NOTE]
> Als u een bestaande IP-firewall is geconfigureerd voor uw Azure DB die Cosmos-account hebt, houd rekening met de configuratie van de firewall, verwijdert u de IP-firewall en schakel vervolgens het Service-eindpunt. Als u het Service-eindpunt zonder disbling de firewall inschakelt, wordt het verkeer van deze ip-adresbereik de identiteit van de virtuele IP-gaan verloren en wordt deze verwijderd met een foutbericht van de IP-filter. Dus om te voorkomen dat deze fout moet u altijd de firewallregels uitschakelt, kopiëren, service-eindpunt van het subnet en ten slotte ACL het subnet van de Cosmos-database inschakelen. Nadat u service-eindpunt configureren en de ACL toevoegen kunt u opnieuw de IP-firewall opnieuw inschakelen indien nodig.

2. Voordat u service-eindpunt voor virtueel netwerk, kopieert u de IP-firewall-informatie die is gekoppeld aan uw Azure DB die Cosmos-account voor toekomstig gebruik. U kunt IP-firewall opnieuw inschakelen na het configureren van de service-eindpunt.  

3. Selecteer **Firewalls en virtuele netwerken van Azure** vanuit het menu instellingen en kies toegang toestaan via **netwerken geselecteerd**.  

4. Selecteer om toegang tot een nieuwe Azure virtueel netwerk, onder virtuele netwerken **toevoegen nieuw virtueel netwerk**.  

5. Geef de details die zijn vereist voor het maken van een nieuw virtueel netwerk en selecteer maken. Het subnet wordt gemaakt met een service-eindpunt voor 'Microsoft.AzureCosmosDB' ingeschakeld.

   ![Selecteer het virtuele netwerk en subnet voor de nieuwe virtuele netwerk](./media/vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

## <a name="allow-access-from-azure-portal"></a>Toegang toestaan via Azure portal

Nadat u Azure Virtual Network service-eindpunten zijn ingeschakeld voor het account van uw Azure DB die Cosmos-database, wordt toegang via de portal of andere Azure-services standaard uitgeschakeld. Toegang tot uw Azure DB die Cosmos-databaseaccount vanaf computers buiten het geconfigureerde subnet worden geblokkeerd met inbegrip van toegang via de portal.

![Toegang toestaan via de portal](./media/vnet-service-endpoint/allow-access-from-portal.png)

Als uw Azure DB die Cosmos-account wordt gebruikt door andere Azure-services zoals Azure Search of toegankelijk is vanuit de Stream analytics of Power BI, u toegang toestaan door het controleren van **toegang tot Azure-Services toestaan**.

Om te controleren of u toegang hebt tot Azure Cosmos DB metrische gegevens vanuit de portal, moet u inschakelen **toegang tot Azure-portal** opties. Zie voor meer informatie over deze opties, [verbindingen van Azure-portal](firewall-support.md#connections-from-the-azure-portal) en [verbindingen van Azure PaaS services](firewall-support.md#connections-from-global-azure-datacenters-or-azure-paas-services) secties. Na het selecteren van toegang, selecteer **opslaan** de instellingen op te slaan.

## <a name="remove-a-virtual-network-or-subnet"></a>Een virtueel netwerk of subnet verwijderen 

1. Van **alle resources** blade, zoeken naar de Azure DB van de Cosmos-account waarvoor u de service-eindpunten toegewezen.  

2. Selecteer **Firewalls en virtuele netwerken** vanuit het menu instellingen.  

3. Als u wilt verwijderen van een virtueel netwerk of subnet regel, selecteer '...' naast het virtuele netwerk of subnet en selecteer **verwijderen**.

   ![Een virtueel netwerk verwijderen](./media/vnet-service-endpoint/remove-a-vnet.png)

4.  Klik op **opslaan** uw wijzigingen toe te passen.

## <a name="configure-service-endpoint-by-using-azure-powershell"></a>Service-eindpunt configureren met behulp van Azure PowerShell 

Gebruik de volgende stappen uit op een Cosmos-DB Azure-account het Service-eindpunt configureren met behulp van Azure PowerShell:  

1. Installeer de meest recente [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) en [aanmelding](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  Zorg ervoor dat u Let op de IP-firewall-instellingen en de IP-firewall verwijderen voordat u Service-eindpunt voor het account inschakelt.


> [!NOTE]
> Als u een bestaande IP-firewall is geconfigureerd voor uw Azure DB die Cosmos-account hebt, houd rekening met de configuratie van de firewall, verwijdert u de IP-firewall en schakel vervolgens het Service-eindpunt. Als u het Service-eindpunt zonder disbling de firewall inschakelt, wordt het verkeer van deze ip-adresbereik de identiteit van de virtuele IP-gaan verloren en wordt deze verwijderd met een foutbericht van de IP-filter. Dus om te voorkomen dat deze fout moet u altijd de firewallregels uitschakelt, kopiëren, service-eindpunt van het subnet en ten slotte ACL het subnet van de Cosmos-database inschakelen. Nadat u service-eindpunt configureren en de ACL toevoegen kunt u opnieuw de IP-firewall opnieuw inschakelen indien nodig.

2. Voordat u service-eindpunt voor virtueel netwerk, kopieert u de IP-firewall-informatie die is gekoppeld aan uw Azure DB die Cosmos-account voor toekomstig gebruik. U wordt IP-firewall opnieuw inschakelen na het configureren van de service-eindpunt.  

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

4. Bereid u voor het inschakelen van ACL voor het CosmosDB Account door ervoor te zorgen dat het virtuele netwerk en subnet ingeschakeld voor Azure Cosmos DB service-eindpunt hebt.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName  -ResourceGroupName $rgName
   ```

5. Eigenschappen van Azure DB die Cosmos-account ophalen door de volgende cmdlet:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

6. Initialiseren van de variabelen voor later gebruik. Instellen van alle variabelen van de bestaande definitie van de account als u meerdere locaties hebt, moet u hen toevoegen als onderdeel van de matrix. In deze stap maakt configureren u ook service-eindpunt voor virtueel netwerk door de variabele 'accountVNETFilterEnabled' op 'True'. Deze waarde is hoger toegewezen aan de parameter 'isVirtualNetworkFilterEnabled'.  

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

7. Eigenschappen van Azure DB die Cosmos-account met de nieuwe configuratie bijwerken door het uitvoeren van de volgende cmdlets: 

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

8. Voer de volgende opdracht om te controleren dat uw Azure DB die Cosmos-account is bijgewerkt met het service-eindpunt voor het virtuele netwerk die u in de vorige stap hebt geconfigureerd:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a name="add-virtual-network-service-endpoint-for-an-azure-cosmos-db-account-that-has-ip-firewall-enabled"></a>Service-eindpunt voor een Azure DB die Cosmos-account dat IP-Firewall is ingeschakeld op virtueel netwerk toevoegen

1. De IP-firewall-toegang tot Azure Cosmos DB account eerst uit te schakelen.  

2. Virtueel netwerkeindpunt aan het account voor Azure Cosmos DB inschakelen met behulp van een van de methoden die in de vorige secties beschreven.  

3. Toegang via de IP-firewall opnieuw inschakelen. 

## <a name="test-the-access"></a>De toegang testen

Als u wilt controleren of de service-eindpunten voor Azure Cosmos DB zijn geconfigureerd zoals verwacht, gebruikt u de volgende stappen uit:

* Twee subnetten in een virtueel netwerk instellen als de front-end- en back-end Cosmos DB service-eindpunt voor het subnet backend inschakelen.  

* Toegang inschakelen in de database van de Cosmos-account voor de back-end-subnet.  

* Maak twee virtuele machines-een in back-end-subnet en een andere in het front-end-subnet.  

* Probeer toegang tot de Azure DB die Cosmos-account vanaf beide virtuele machines. U moet uit de virtuele machine gemaakt in het subnet backend, maar niet op gemaakt in het subnet frontend verbinding kunnen maken. De aanvraag wordt een fout optreedt met 404 wanneer u probeert verbinding te maken van het subnet frontend die bevestigt dat de toegang tot Azure Cosmos DB is beveiligd met behulp van het service-eindpunt van het virtuele netwerk. De computer in het subnet backend zich te werken.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-happens-when-you-access-an-azure-cosmos-db-account-that-has-virtual-network-access-control-list-acl-enabled"></a>Wat gebeurt er wanneer u een account voor Azure Cosmos DB met virtuele netwerk lijst ACL (Access Control) ingeschakeld?  

HTTP 404-fout geretourneerd.  

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-cosmos-db-account-in-another-region-for-example-if-azure-cosmos-db-account-is-in-west-us-or-east-us-and-virtual-networks-are-in-multiple-regions-can-the-virtual-network-access-azure-cosmos-db"></a>Zijn de subnetten van een virtueel netwerk gemaakt in verschillende regio's die toegang mogen krijgen tot een Cosmos-DB Azure-account in een andere regio? Bijvoorbeeld, als Azure DB die Cosmos-account in VS-West of VS-Oost en van het virtuele netwerk is zijn in meerdere regio's, het virtuele netwerk toegang tot Azure Cosmos DB?  

Ja, toegankelijk door de nieuwe virtuele netwerken die zijn gemaakt in verschillende regio's. 

### <a name="can-an-azure-cosmos-db-account-have-both-virtual-network-service-endpoint-and-a-firewall"></a>Kan een Cosmos-DB Azure-account hebben zowel virtuele netwerk Service-eindpunt als een firewall?  

Ja, Virtual Network Service-eindpunt en een firewall kunnen naast elkaar bestaan. In het algemeen moet u ervoor zorgen dat toegang tot de portal is altijd ingeschakeld voordat u een service-eindpunt van het virtuele netwerk configureert zodat u kunt de metrische gegevens die zijn gekoppeld aan de container weergeven.

### <a name="can-i-allow-access-to-other-azure-services-from-a-given-azure-region-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Kan ik 'toestaan toegang tot andere Azure-services van een bepaald Azure-regio' wanneer toegang tot de service-eindpunt is ingeschakeld voor Azure Cosmos DB?  

Dit is vereist alleen als u wilt dat uw Azure DB die Cosmos-account toegankelijk door andere Azure eerste partij services zoals Azure Data factory, Azure Search of een service die wordt geïmplementeerd in de opgegeven Azure-regio.

### <a name="how-many-virtual-network-service-endpoints-are-allowed-for-azure-cosmos-db"></a>Hoeveel virtuele netwerk service-eindpunten zijn toegestaan voor Azure Cosmos DB?  

64 virtuele netwerk service-eindpunten zijn toegestaan voor een Azure DB die Cosmos-account.

### <a name="what-is-the-relationship-between-service-endpoint-and-network-security-group-nsg-rules"></a>Wat is de relatie tussen de regels voor Service-eindpunt en een Netwerkbeveiligingsgroep (NSG)?  

NSG-regels in Azure Cosmos DB kunnen u toegang te beperken tot specifieke Azure Cosmos DB IP-adresbereik. Als u wilt toestaan toegang tot een exemplaar van Azure DB die Cosmos die aanwezig is in een specifieke [regio](https://azure.microsoft.com/global-infrastructure/regions/), kunt u de regio in de volgende indeling: 

    AzureCosmosDB.<region name>

Voor meer informatie over het NSG Zie tags [virtueel netwerk service labels](../virtual-network/security-overview.md#service-tags) artikel. 
  
### <a name="what-is-relationship-between-an-ip-firewall-and-virtual-network-service-endpoint-capability"></a>Wat is de relatie tussen een IP-firewall en Virtual Network service-eindpunt mogelijkheid?  

Deze twee functies een aanvulling op elkaar om te garanderen isolatie van Azure DB die Cosmos activa en ze zijn beveiligd. Gebruik van IP-firewall zorgt ervoor dat statische IP-adressen toegang hebt tot Azure DB die Cosmos-account.  

### <a name="can-an-on-premise-devices-ip-address-that-is-connected-through-azure-virtual-network-gatewayvpn-or-express-route-gateway-access-azure-cosmos-db-account"></a>Een on-premises-apparaat IP-adres dat is verbonden via Azure Virtual Network gateway(VPN) of Express route-gateway toegang hebt tot Azure Cosmos DB account?  

Het IP-adres of de IP-adresbereik van het on-premises-apparaat moet worden toegevoegd aan de lijst met statische IP-adressen om toegang tot de Azure DB die Cosmos-account.  

### <a name="what-happens-if-you-delete-a-virtual-network-that-has-service-endpoint-setup-for-azure-cosmos-db"></a>Wat gebeurt er als u een virtueel netwerk met de installatie van de service-eindpunt voor Azure Cosmos DB verwijderen?  

Wanneer een virtueel netwerk wordt verwijderd, worden de ACL-gegevens gekoppeld aan dat Azure Cosmos DB wordt verwijderd en de interactie tussen het virtuele netwerk en de Azure DB die Cosmos-account wordt verwijderd. 

### <a name="what-happens-if-an-azure-cosmos-db-account-that-has-virtual-network-service-endpoint-enabled-is-deleted"></a>Wat gebeurt er als een Cosmos-DB Azure-account met service-eindpunt van een virtueel netwerk ingeschakeld is verwijderd?

De metagegevens gekoppeld aan de specifieke Azure DB die Cosmos-account wordt verwijderd uit het subnet. En is de verantwoordelijkheid van de eindgebruiker te verwijderen van het subnet en het virtuele netwerk dat wordt gebruikt.

### <a name="can-i-use-a-peered-virtual-network-to-create-service-endpoint-for-azure-cosmos-db"></a>Kan ik een peered virtueel netwerk gebruiken voor het maken van service-eindpunt voor Azure Cosmos-DB  

Nee, alleen directe virtueel netwerk en hun subnetten Azure Cosmos DB service-eindpunten kunnen maken.

### <a name="what-happens-to-the-source-ip-address-of-resource-like-virtual-machine-in-the-subnet-that-has-azure-cosmos-db-service-endpoint-enabled"></a>Wat gebeurt er met het bron-IP-adres van bron, zoals virtuele machine in het subnet met Azure Cosmos DB service-eindpunt is ingeschakeld?

Wanneer de service-eindpunten virtueel netwerk zijn ingeschakeld, wordt de bron-IP-adressen van resources in uw virtueel netwerksubnet overschakelen van het gebruik van openbare IPV4-adressen aan Azure Virtual Network particuliere adressen voor het verkeer naar Azure Cosmos DB.

### <a name="does-azure-cosmos-db-reside-in-the-azure-virtual-network--provided-by-the-customer"></a>Azure Cosmos DB bevindt zich in het Azure-netwerk dat is opgegeven door de klant?  

Azure Cosmos-database is een multitenant-service met een openbaar IP-adres. Wanneer u toegang tot een subnet van een virtueel Azure-netwerk met behulp van de functie van de service-eindpunt beperken, is toegang beperkt voor uw Azure DB die Cosmos-account via het opgegeven Azure-netwerk en een subnet.  Azure DB Cosmos-account zich niet in dit virtuele netwerk van Azure. 

### <a name="what-if-anything-will-be-logged-in-log-analyticsoms-if-it-is-enabled"></a>Wat gebeurt er als wordt vastgelegd in logboek Analytics/OMS als deze is ingeschakeld?  

Azure Cosmos DB zendt de logboeken met IP-adres (zonder de laatste octet) met de status 403 voor aanvraag wordt geblokkeerd door de ACL.  

## <a name="next-steps"></a>Volgende stappen
Voor een firewall configureren voor Azure Cosmos DB Zie [firewall-ondersteuning](firewall-support.md) artikel.

