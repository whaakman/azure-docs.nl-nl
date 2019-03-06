---
title: Azure-SSIS integratieruntime toevoegen aan een virtueel netwerk | Microsoft Docs
description: Leer hoe u de Azure-SSIS integratieruntime toevoegen aan een Azure-netwerk.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/08/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 24e2d7be0dfc32e499bc864f2fdf7e2b1c108969
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57440205"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Een Azure-SSIS integratieruntime toevoegen aan een virtueel netwerk
Uw Azure-SSIS integratieruntime (IR) toevoegen aan een Azure-netwerk in de volgende scenario's: 

- U wilt verbinding maken met on-premises gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd in een Azure SSIS Integration Runtime. 

- Als u beheert de SQL Server Integration Services (SSIS)-catalogusdatabase in Azure SQL Database met virtual network service-eindpunten/beheerd exemplaar. 

 Azure Data Factory kunt u uw Azure-SSIS integratieruntime toevoegen aan een virtueel netwerk die zijn gemaakt via het klassieke implementatiemodel of het implementatiemodel Azure Resource Manager. 

> [!IMPORTANT]
> Het klassieke virtuele netwerk is op dit moment wordt afgeschaft, dus gebruik de Azure Resource Manager-virtuele netwerk in plaats daarvan.  Als u al het klassieke virtuele netwerk, ga dan voor het gebruik van het virtuele netwerk van Azure Resource Manager zo snel mogelijk.

## <a name="access-to-on-premises-data-stores"></a>Toegang tot on-premises gegevensopslagexemplaren
Als de SSIS-pakketten toegang tot gegevensarchieven enige openbare cloud, moet u niet de Azure-SSIS IR toevoegen aan een virtueel netwerk. Als de SSIS-pakketten toegang krijgen tot on-premises gegevensopslagexemplaren, moet u de Azure-SSIS IR toevoegen aan een virtueel netwerk dat is verbonden met de on-premises netwerk. 

Hier volgen enkele belangrijke punten om te weten: 

- Als er geen bestaande virtuele netwerk is verbonden met uw on-premises netwerk, maakt u eerst een [virtueel netwerk van Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) of een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-integratie Runtime om toe te voegen. Configureer vervolgens een site-naar-site [VPN-gatewayverbinding](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) of [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) verbinding van dit virtuele netwerk naar uw on-premises netwerk. 

- Als er een bestaande Azure Resource Manager of klassiek virtueel netwerk verbonden met uw on-premises netwerk op dezelfde locatie bevinden als uw Azure-SSIS-IR, kunt u de IR toevoegen aan dit virtuele netwerk. 

- Als er een bestaand klassiek virtueel netwerk verbonden met uw on-premises netwerk in een andere locatie van uw Azure-SSIS IR is, kunt u eerst maken een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS IR om toe te voegen. Configureer vervolgens een [klassieke naar Klassiek virtueel netwerk](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) verbinding. U kunt ook maken een [virtueel netwerk van Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) voor uw Azure-SSIS integratieruntime om toe te voegen. Configureer vervolgens een [virtueel netwerk van klassiek naar Azure resourcemanager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) verbinding. 
 
- Als er een bestaande Azure Resource Manager virtueel netwerk met uw on-premises netwerk in een andere locatie van uw Azure-SSIS IR verbonden is, kunt u eerst maken een [virtueel netwerk van Azure Resource Manager](../virtual-network/quick-create-portal.md##create-a-virtual-network) voor uw Azure-SSIS IR om toe te voegen. Configureer vervolgens een Azure Resource Manager-naar-Azure Resource Manager-verbinding voor virtueel netwerk. U kunt ook maken een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS IR om toe te voegen. Configureer vervolgens een [virtueel netwerk van klassiek naar Azure resourcemanager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) verbinding. 

## <a name="host-the-ssis-catalog-database-in-azure-sql-database-with-virtual-network-service-endpointsmanaged-instance"></a>Host de SSIS-catalogusdatabase in Azure SQL Database met virtual network service endpoints/beheerd exemplaar
Als de SSIS-catalogus wordt gehost in Azure SQL Database met virtual network-service-eindpunten of Managed Instance, u kunt deelnemen aan uw Azure-SSIS IR aan: 

- Hetzelfde virtuele netwerk 
- Een ander virtueel netwerk met een netwerk-naar-netwerkverbinding met de optie die wordt gebruikt voor het beheerde exemplaar 

Als u uw SSIS-catalogus in Azure SQL Database met virtual network-service-eindpunten host, ervoor zorgen dat u uw Azure-SSIS IR aan hetzelfde virtuele netwerk en subnet toevoegen.

Als u uw Azure-SSIS IR aan hetzelfde virtuele netwerk bevinden als het beheerde exemplaar toevoegen, zorg ervoor dat de Azure-SSIS IR in een ander subnet dan het beheerde exemplaar is. Als u uw Azure-SSIS IR aan een ander virtueel netwerk dan het beheerde exemplaar toevoegen, raden wij peering op virtueel netwerk (dit is beperkt tot dezelfde regio bevinden) of een virtueel netwerk met virtuele netwerk. Zie [verbinding maken met uw toepassing naar Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-connect-app.md).

In alle gevallen moet worden het virtuele netwerk alleen geïmplementeerd via het Azure Resource Manager-implementatiemodel.

De volgende secties vindt u meer informatie. 

## <a name="requirements-for-virtual-network-configuration"></a>Vereisten voor virtuele-netwerkconfiguratie
-   Zorg ervoor dat `Microsoft.Batch` is een geregistreerde provider onder het abonnement van uw virtuele netwerksubnet dat als host fungeert voor de Azure-SSIS-IR. Als u een klassiek virtueel netwerk gebruikt, ook toevoegen `MicrosoftAzureBatch` aan de rol Inzender voor klassieke virtuele machines voor dit virtuele netwerk. 

-   Zorg ervoor dat u beschikt over de vereiste machtigingen. Zie [vereiste machtigingen](#perms).

-   Selecteer het juiste subnet voor het hosten van de Azure-SSIS-IR. Zie [selecteert u het subnet](#subnet). 

-   Als u uw eigen server Services DNS (Domain Name) op het virtuele netwerk, Zie [Domain Name Services server](#dns_server). 

-   Als u een Netwerkbeveiligingsgroep (NSG) gebruikt op het subnet, Zie [netwerkbeveiligingsgroep](#nsg) 

-   Als u met behulp van Azure Express Route of configureren van de gebruiker gedefinieerde Route (UDR), raadpleegt u [gebruik Azure ExpressRoute of door de gebruiker gedefinieerde Route](#route). 

-   Zorg ervoor dat de resourcegroep van het virtuele netwerk kunt maken en verwijderen van bepaalde Azure-netwerkbronnen. Zie [vereisten voor de resourcegroep](#resource-group). 

Hier volgt een diagram van de vereiste verbindingen voor uw Azure-SSIS IR:

![Azure-SSIS IR](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir.png)

### <a name="perms"></a> Vereiste machtigingen

De gebruiker die de Azure-SSIS Integration Runtime maakt, moet de volgende machtigingen hebben:

- Als u uw SSIS-IR aan een virtueel netwerk van Azure Resource Manager toevoegen bent, hebt u twee opties:

  - Gebruik de ingebouwde *Inzender voor netwerken* rol. Deze rol wordt geleverd met de _Microsoft.Network/\*_  machtiging een veel groter bereik dan nodig heeft.

  - Een aangepaste rol maken die alleen de benodigde bevat _Microsoft.Network/virtualNetworks/\*/join/actie_ machtiging. 

- Als u uw SSIS-IR aan een klassiek virtueel netwerk toevoegen bent, raden wij aan dat u de ingebouwde *Inzender voor klassieke virtuele machines* rol. Anders moet u een aangepaste rol met de machtigingen om lid van het virtuele netwerk te definiëren.

### <a name="subnet"></a> Het subnet selecteren
-   Selecteer het GatewaySubnet voor het implementeren van een Azure-SSIS Integration Runtime, niet omdat deze is specifiek voor virtuele netwerkgateways. 

-   Zorg ervoor dat het subnet dat u voldoende beschikbare adresruimte voor Azure-SSIS IR te gebruiken. Laat u ten minste 2 * knooppuntnummer IR in beschikbare IP-adressen. Azure reserveert bepaalde IP-adressen binnen elk subnet, en deze adressen kunnen niet worden gebruikt. De eerste en laatste IP-adressen van de subnetten zijn gereserveerd voor protocolconformiteit en drie adressen voor Azure-services. Zie voor meer informatie, [gelden er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets). 

-   Gebruik niet een subnet dat uitsluitend wordt ingenomen door andere Azure-services (bijvoorbeeld SQL Database Managed Instance, App Service, enzovoort). 

### <a name="dns_server"></a> Domain Name Services server 
Als u nodig hebt voor gebruik met uw eigen Services DNS (Domain Name)-server in een virtueel netwerk dat is gekoppeld met uw Azure-SSIS integratieruntime, zorg ervoor dat het openbare Azure-hostnamen kan omzetten (bijvoorbeeld een naam voor Azure Storage blob `<your storage account>.blob.core.windows.net`). 

De volgende stappen worden aanbevolen: 

-   Configureren van aangepaste DNS voor het doorsturen van aanvragen naar Azure DNS. U kunt niet-omgezette DNS-records naar het IP-adres van Azure van recursieve resolvers (168.63.129.16) op uw eigen DNS-server doorsturen. 

-   Instellen van de aangepaste DNS als primaire en de Azure DNS als secundaire voor het virtuele netwerk. Het IP-adres van Azure recursieve resolvers (168.63.129.16) als een secundaire DNS-server registreren in het geval uw eigen DNS-server niet beschikbaar is. 

Zie voor meer informatie, [naamomzetting die gebruikmaakt van uw eigen DNS-server](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a> Netwerkbeveiligingsgroep
Als u nodig hebt voor het implementeren van een netwerkbeveiligingsgroep (NSG) voor het subnet dat wordt gebruikt door uw Azure-SSIS integratieruntime, kunt u binnenkomend en uitgaand verkeer via de volgende poorten: 

| Richting | Transportprotocol | Bron | Poortbereik van bron | Doel | Poortbereik van doel | Opmerkingen |
|---|---|---|---|---|---|---|
| Inkomend | TCP | AzureCloud<br/>(of een groter bereik, zoals Internet) | * | VirtualNetwork | 29876, 29877 (als u de IR aan een virtueel netwerk van Azure Resource Manager toevoegen) <br/><br/>10100, 20100, 30100 (als u de IR aan een klassiek virtueel netwerk toevoegen)| De Data Factory-service gebruikt deze poorten om te communiceren met de knooppunten van uw Azure-SSIS integratieruntime in het virtuele netwerk. <br/><br/> Of u een NSG op subnetniveau of niet maakt, Data Factory altijd Hiermee configureert u een NSG op het niveau van de netwerkinterfacekaarten (NIC's) die is gekoppeld aan de virtuele machines die als host fungeren van de Azure-SSIS-IR. Alleen inkomend verkeer van Data Factory-IP-adressen op de opgegeven poorten is toegestaan door deze NSG NIC-niveau. Zelfs als u deze poorten voor verkeer van Internet op het subnetniveau opent, wordt verkeer van IP-adressen die geen Data Factory-IP-adressen wordt geblokkeerd op NIC-niveau. |
| Uitgaand | TCP | VirtualNetwork | * | AzureCloud<br/>(of een groter bereik, zoals Internet) | 443 | De knooppunten van uw Azure-SSIS integratieruntime in het virtuele netwerk gebruiken deze poort voor toegang tot Azure-services, zoals Azure Storage en Azure Event Hubs. |
| Uitgaand | TCP | VirtualNetwork | * | Internet | 80 | De knooppunten van uw Azure-SSIS integratieruntime in het virtuele netwerk gebruikt deze poort certificaatintrekkingslijst downloaden van Internet. |
| Uitgaand | TCP | VirtualNetwork | * | SQL<br/>(of een groter bereik, zoals Internet) | 1433, 11000-11999, 14000-14999 | De knooppunten van uw Azure-SSIS integratieruntime in het virtuele netwerkgebruik deze poorten voor toegang tot SSISDB wordt gehost door uw Azure SQL Database-server, dit doel is niet van toepassing op SSISDB wordt gehost door beheerde exemplaar. |
||||||||

### <a name="route"></a> Gebruik Azure ExpressRoute of de gebruiker gedefinieerde Route
U kunt verbinding maken met een [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) circuit met de infrastructuur van uw virtuele netwerk naar uw on-premises netwerk uitbreiden naar Azure. 

Een veelvoorkomende configuratie is om het gebruik van geforceerde tunnels (een BGP-route 0.0.0.0/0 aan het virtuele netwerk adverteren) die ervoor zorgt dat het uitgaande internetverkeer vanuit de stroom van het virtuele netwerk naar on-premises netwerkapparaat voor controle en logboekregistratie. Deze verkeersstroom verbroken verbinding tussen de Azure-SSIS-IR in het virtuele netwerk met afhankelijke Azure Data Factory-services. De oplossing is voor het definiëren van een (of meer) [gebruiker gedefinieerde routes (udr's)](../virtual-network/virtual-networks-udr-overview.md) op het subnet waarin de Azure-SSIS-IR. Een UDR definieert de subnet-specifieke routes die worden herkend in plaats van de BGP-route. 

Of u kunt de gebruiker gedefinieerde routes (udr's) om af te dwingen van het uitgaande internetverkeer vanuit het subnet dat als host fungeert voor de Azure-SSIS IR aan een ander subnet, dat als host fungeert voor een virtueel netwerkapparaat als een firewall of een DMZ definiëren voor controle en logboekregistratie. 

In beide gevallen wordt een route 0.0.0.0/0 van toepassing met het volgende hoptype als **Internet** op het subnet dat als host fungeert voor de Azure-SSIS-IR, zodat de communicatie tussen de Data Factory-service en de Azure-SSIS IR IS kan slagen. 

![Een route toevoegen](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Als u zich zorgen maakt over de mogelijkheid om te controleren van uitgaand internetverkeer op dat subnet te verliezen, kunt u ook een NSG-regel toevoegen op het subnet om te beperken van uitgaande bestemmingen [Azure Datacenter IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653). 

Zie [dit PowerShell-script](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c) voor een voorbeeld. U moet het script wekelijks uitvoeren om de lijst met IP-adressen voor Azure data center up-to-date te houden. 

### <a name="resource-group"></a> Vereisten voor de resourcegroep
-   De Azure-SSIS-IR moet bepaalde netwerkresources in dezelfde resourcegroep bevinden als het virtuele netwerk maken. Deze resources omvatten het volgende:
    -   Een Azure load balancer, met de naam van de  *<Guid>- azurebatch cloudserviceloadbalancer*.
    -   Een Azure openbare IP-adres met de naam van de  *<Guid>- azurebatch cloudservicepublicip*.
    -   Een werk netwerkbeveiligingsgroep, met de naam van de  *<Guid>- azurebatch cloudservicenetworksecuritygroup*. 

-   Zorg ervoor dat u alle resourcevergrendeling voor de resourcegroep of abonnement waartoe het virtuele netwerk behoort geen hebt. Als u een alleen-lezen vergrendeld of een verwijderingsvergrendeling configureert, kunnen starten en stoppen van de IR mislukken of vastlopen. 

-   Zorg ervoor dat u een Azure-beleid waarmee wordt voorkomen dat de volgende bronnen worden gemaakt onder de resourcegroep of abonnement waartoe het virtuele netwerk behoort niet hebt: 
    -   Microsoft.Network/LoadBalancers 
    -   Microsoft.Network/NetworkSecurityGroups 
    -   Microsoft.Network/PublicIPAddresses 

## <a name="azure-portal-data-factory-ui"></a>Azure-portal (Data Factory-UI)
Deze sectie leest u hoe u een bestaande Azure-SSIS runtime toevoegen aan een virtueel netwerk (klassiek of Azure resourcemanager) met behulp van de Azure-portal en de gebruikersinterface van Data Factory. Eerst moet u het virtuele netwerk op de juiste wijze configureren voordat u uw Azure-SSIS IR toevoegen aan deze. Ga via een van de volgende twee secties op basis van het type van het virtuele netwerk (klassiek of Azure resourcemanager). Ga vervolgens door met de derde sectie naar uw Azure-SSIS IR toevoegen aan het virtuele netwerk. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Gebruik de portal voor het configureren van een virtueel netwerk van Azure Resource Manager
U moet een virtueel netwerk configureren voordat u een Azure-SSIS IR aan deze toevoegen kunt. 

1. Start Microsoft Edge of Google Chrome. Op dit moment wordt de Data Factory-UI alleen ondersteund in deze webbrowsers. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

1. Selecteer **meer services**. Filter en selecteer **virtuele netwerken**. 

1. Filter en selecteer het virtuele netwerk in de lijst. 

1. Op de **virtueel netwerk** weergeeft, schakelt **eigenschappen**. 

1. Selecteer de knop kopiëren voor **RESOURCE-ID** naar de resource-ID voor het virtuele netwerk naar het Klembord kopiëren. Sla de ID van het Klembord in OneNote of in een bestand. 

1. Selecteer **subnetten** in het menu links. Zorg ervoor dat het aantal **beschikbare adressen** groter is dan de knooppunten in uw Azure-SSIS integratieruntime. 

1. Controleer of dat de Azure Batch-provider is geregistreerd in de Azure-abonnement met het virtuele netwerk. Of de Azure Batch-provider hebt geregistreerd. Als u al een Azure Batch-account in uw abonnement hebt, is klikt u vervolgens uw abonnement geregistreerd voor Azure Batch. (Als u de Azure-SSIS-IR in de portal voor Data Factory maakt, de Azure Batch-provider wordt automatisch voor u geregistreerd.) 

   a. Selecteer in Azure portal, **abonnementen** in het menu links. 

   b. Selecteer uw abonnement. 

   c. Selecteer **resourceproviders** aan de linkerkant en Bevestig dat **Microsoft.Batch** is een geregistreerde provider. 

   ![Bevestiging van de status "Geregistreerd"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Als er geen **Microsoft.Batch** in de lijst om te registreren, [lege Azure Batch-account maken](../batch/batch-account-create-portal.md) in uw abonnement. U kunt deze later verwijderen. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Gebruik de portal voor het configureren van een klassiek virtueel netwerk
U moet een virtueel netwerk configureren voordat u een Azure-SSIS IR aan deze toevoegen kunt. 

1. Start Microsoft Edge of Google Chrome. De Data Factory-UI wordt momenteel alleen in deze browsers ondersteund. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

1. Selecteer **meer services**. Filter en selecteer **virtuele netwerken (klassiek)**. 

1. Filter en selecteer het virtuele netwerk in de lijst. 

1. Op de **virtueel netwerk (klassiek)** weergeeft, schakelt **eigenschappen**. 

   ![Klassiek virtueel netwerk resource-ID](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Selecteer de knop kopiëren voor **RESOURCE-ID** naar de resource-ID voor het klassieke virtuele netwerk naar het Klembord kopiëren. Sla de ID van het Klembord in OneNote of in een bestand. 

1. Selecteer **subnetten** in het menu links. Zorg ervoor dat het aantal **beschikbare adressen** groter is dan de knooppunten in uw Azure-SSIS integratieruntime. 

   ![Aantal beschikbare adressen in het virtuele netwerk](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. Deelnemen aan **MicrosoftAzureBatch** naar de **Inzender voor klassieke virtuele machines** rol voor het virtuele netwerk. 

    a. Selecteer **toegangsbeheer (IAM)** op het menu aan de linkerkant en selecteer de **roltoewijzingen** tabblad. 

    !['Toegangsbeheer' en 'Toevoegen' knoppen](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Selecteer **roltoewijzing toevoegen**.

    c. Op de **roltoewijzing toevoegen** weergeeft, schakelt **Inzender voor klassieke virtuele machines** voor **rol**. Plakken **ddbf3205-c6bd-46ae-8127-60eb93363864** in de **Selecteer** vak en selecteer vervolgens **Microsoft Azure Batch** uit de lijst met zoekresultaten. 

    ![Doorzoek resultaten op pagina 'Roltoewijzing toevoegen'](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    d. Selecteer **opslaan** de instellingen op te slaan en te sluiten van de pagina. 

    ![Opslaan van instellingen voor toegang](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    e. Controleer of **Microsoft Azure Batch** in de lijst met inzenders. 

    ![Azure Batch toegang bevestigen](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Controleer of dat de Azure Batch-provider is geregistreerd in de Azure-abonnement met het virtuele netwerk. Of de Azure Batch-provider hebt geregistreerd. Als u al een Azure Batch-account in uw abonnement hebt, is klikt u vervolgens uw abonnement geregistreerd voor Azure Batch. (Als u de Azure-SSIS-IR in de portal voor Data Factory maakt, de Azure Batch-provider wordt automatisch voor u geregistreerd.) 

   a. Selecteer in Azure portal, **abonnementen** in het menu links. 

   b. Selecteer uw abonnement. 

   c. Selecteer **resourceproviders** aan de linkerkant en Bevestig dat **Microsoft.Batch** is een geregistreerde provider. 

   ![Bevestiging van de status "Geregistreerd"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Als er geen **Microsoft.Batch** in de lijst om te registreren, [lege Azure Batch-account maken](../batch/batch-account-create-portal.md) in uw abonnement. U kunt deze later verwijderen. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>De Azure-SSIS IR toevoegen aan een virtueel netwerk
1. Start Microsoft Edge of Google Chrome. Op dit moment wordt de Data Factory-UI alleen ondersteund in deze webbrowsers. 

1. In de [Azure-portal](https://portal.azure.com), selecteer **Data Factory's** in het menu links. Als er geen **Data Factory's** selecteren in het menu **meer services**, en selecteer **Data Factory's** in de **INTELLIGENCE en analyse**sectie. 

   ![Lijst met data factory 's](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Selecteer uw gegevensfactory met de Azure-SSIS integratieruntime in de lijst. U ziet de startpagina van uw gegevensfactory. Selecteer de **maken en implementeren** tegel. U ziet de gebruikersinterface van Data Factory op een afzonderlijk tabblad. 

   ![Startpagina van de gegevensfactory](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. Ga in de gebruikersinterface van Data Factory, naar de **bewerken** tabblad **verbindingen**, en schakel over naar de **Integratieruntimes** tabblad. 

   ![Tabblad 'integratieruntimes'](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Als uw Azure-SSIS IR wordt uitgevoerd, in de lijst met integration runtime, selecteert u de **stoppen** knop in de **acties** kolom voor uw Azure-SSIS-IR. U kunt een IR niet bewerken, totdat u het stopt. 

   ![Stoppen van de IR](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. Selecteer in de lijst met integration runtime, de **bewerken** knop in de **acties** kolom voor uw Azure-SSIS-IR. 

   ![De integratieruntime bewerken](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. Op de **algemene instellingen** pagina van de **installatie van Integratieruntime** venster **volgende**. 

   ![Algemene instellingen voor de installatie van de IR](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)

1. Op de **SQL-instellingen** pagina, het administrator-wachtwoord invoeren en selecteer **volgende**. 

   ![SQL Server-instellingen voor de installatie van de IR](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)

1. Op de **geavanceerde instellingen** pagina, de volgende acties uitvoeren: 

   a. Schakel het selectievakje voor **selecteert u een VNet-naar-VNet-machtigingen /-instellingen configureren voor uw Azure-SSIS Integration Runtime te koppelen en om Azure-services toestaan**. 

   b. Voor **Type**, selecteert of het virtuele netwerk een klassiek virtueel netwerk of een virtueel netwerk van Azure Resource Manager is. 

   c. Voor **VNet-naam**, selecteer het virtuele netwerk. 

   d. Voor **subnetnaam**, selecteert u uw subnet in het virtuele netwerk. 

   e. Klik op **VNet validatie** en als dit lukt, klikt u op **Update**. 

   ![Geavanceerde instellingen voor de installatie van de IR](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

1. U kunt nu de IR starten met behulp van de **Start** knop in de **acties** kolom voor uw Azure-SSIS-IR. Het duurt ongeveer 20-30 minuten om te starten van een Azure-SSIS IR. 

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="configure-a-virtual-network"></a>Een virtueel netwerk configureren
U moet een virtueel netwerk configureren voordat u uw Azure-SSIS IR aan deze toevoegen kunt. Toevoegen als u wilt configureren automatisch machtigingen/instellingen van virtuele netwerken voor uw Azure-SSIS integratieruntime voor deelname aan het virtuele netwerk, het volgende script:

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Een Azure-SSIS IR maken en voeg deze toe aan een virtueel netwerk
U kunt een Azure-SSIS IR maken en koppel deze aan een virtueel netwerk op hetzelfde moment. Zie voor het volledige script en instructies [maken van een Azure-SSIS integratieruntime](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Een bestaande Azure-SSIS IR toevoegen aan een virtueel netwerk
Het script in de [maken van een Azure-SSIS integratieruntime](create-azure-ssis-integration-runtime.md) artikel leest u hoe een Azure-SSIS IR maken en voeg deze toe aan een virtueel netwerk in hetzelfde script. Als u een bestaande Azure-SSIS-IR, voer de volgende stappen uit als u wilt toevoegen aan het virtuele netwerk hebt: 
1. Stoppen van de Azure-SSIS-IR. 
1. De Azure-SSIS IR om lid van het virtuele netwerk te configureren. 
1. Start de Azure-SSIS-IR. 

### <a name="define-the-variables"></a>De variabelen definiëren
```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>De Azure-SSIS-IR stoppen
De Azure-SSIS integratieruntime te stoppen voordat u deze naar een virtueel netwerk kan worden toegevoegd. Met deze opdracht alle knooppunten worden vrijgegeven en stopt facturering:

```powershell
Stop-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Virtueel netwerk configureren voor de Azure-SSIS IR om toe te voegen
```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>De Azure-SSIS IR configureren
Voor het configureren van de Azure-SSIS integratieruntime om deel te nemen van het virtuele netwerk, voer de `Set-AzDataFactoryV2IntegrationRuntime` opdracht: 

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Start de Azure-SSIS-IR
Voer de volgende opdracht voor het starten van de Azure-SSIS integratieruntime: 

```powershell
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Met deze opdracht duurt 20-30 minuten om te voltooien.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over de Azure-SSIS runtime: 
- [Azure-SSIS-integratieruntime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat algemene informatie over integratieruntimes in het algemeen, met inbegrip van de Azure-SSIS-IR. 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md). In dit artikel vindt u stapsgewijze instructies voor het maken van een Azure-SSIS IR. Het maakt gebruik van Azure SQL Database voor het hosten van de SSIS-catalogus. 
- [Een Azure-SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). In dit artikel gaat verder in op de zelfstudie en bevat instructies over het gebruik van Azure SQL Database met virtual network service-eindpunten/beheerd exemplaar voor het hosten van de SSIS-catalogus en toevoegen van de IR aan een virtueel netwerk. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Deze ook wordt beschreven hoe u uw Azure-SSIS IR uitschalen door knooppunten toe te voegen. 
