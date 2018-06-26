---
title: Azure-SSIS-integratie runtime toevoegen aan een virtueel netwerk | Microsoft Docs
description: Informatie over hoe de Azure-SSIS-integratie runtime koppelen aan een virtuele Azure-netwerk.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/24/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: aca67ceff2650a5470b1c08b20c21d71f00bae62
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751527"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Een Azure-SSIS-integratie runtime toevoegen aan een virtueel netwerk
Aanmelden bij uw Azure-SSIS-integratie runtime (IR) naar een Azure-netwerk in de volgende scenario's: 

- U wilt verbinding maken met on-premises gegevensarchieven vanuit SSIS-pakketten die worden uitgevoerd in een Azure SSIS Integration Runtime. 

- Als u beheert de catalogusdatabase van de SQL Server Integration Services (SSIS)-in Azure SQL Database met een virtueel netwerk service-eindpunten/beheerde exemplaar (Preview). 

 Azure Data Factory versie 2 (Preview) kunt u uw Azure-SSIS-integratie runtime toevoegen aan een virtueel netwerk gemaakt met behulp van het klassieke implementatiemodel of het Azure Resource Manager-implementatiemodel. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u gebruikmaakt van versie 1 van de Data Factory-service is in het algemeen beschikbaarheid (GA), raadpleegt u de [Data Factory-documentatie voor versie 1](v1/data-factory-introduction.md). 

## <a name="access-to-on-premises-data-stores"></a>Toegang tot on-premises gegevensopslagexemplaren
Als SSIS-pakketten toegang krijgen de gegevensarchieven alleen openbare cloud tot, moet u niet de Azure-SSIS-IR toevoegen aan een virtueel netwerk. Als SSIS-pakketten toegang on-premises gegevensopslagexemplaren tot, moet u de Azure-SSIS-IR toevoegen aan een virtueel netwerk dat is verbonden met de on-premises netwerk. 

Hier volgen enkele belangrijke punten met: 

- Als er zijn geen virtuele netwerk is verbonden met uw on-premises netwerk, maakt u eerst een [virtueel netwerk van Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) of een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-integratie de runtime om toe te voegen. Configureer vervolgens een site-naar-site [VPN-gatewayverbinding](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) of [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) verbinding van dit virtuele netwerk naar uw on-premises netwerk. 

- Als er een klassiek virtueel netwerk is verbonden met uw on-premises netwerk in dezelfde locatie als uw Azure-SSIS-IR of een bestaande Azure Resource Manager, kunt u de IR kan toevoegen aan dit virtuele netwerk. 

- Als er een bestaand klassiek virtueel netwerk verbonden met uw on-premises netwerk in een andere locatie van uw Azure-SSIS-IR, kunt u eerst maken een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-IR om toe te voegen. Configureer vervolgens een [klassiek naar Klassiek virtueel netwerk](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) verbinding. Kunt u een [virtueel netwerk van Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) voor uw Azure-SSIS-integratie runtime om toe te voegen. Configureer vervolgens een [virtueel netwerk van klassiek naar Azure resourcemanager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) verbinding. 
 
- Als er een bestaande Azure Resource Manager virtueel netwerk met uw on-premises netwerk in een andere locatie van uw Azure-SSIS-IR verbonden, kunt u eerst maken een [virtueel netwerk van Azure Resource Manager](../virtual-network/quick-create-portal.md##create-a-virtual-network) voor uw Azure-SSIS IR om toe te voegen. Configureer vervolgens de verbinding van een Azure Resource Manager naar Azure Resource Manager virtueel netwerk. U kunt ook maken een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure-SSIS-IR om toe te voegen. Configureer vervolgens een [virtueel netwerk van klassiek naar Azure resourcemanager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) verbinding. 

## <a name="host-the-ssis-catalog-database-in-azure-sql-database-with-virtual-network-service-endpointsmanaged-instance-preview"></a>Host de catalogus SSIS-database in Azure SQL Database met virtueel netwerk service-eindpunten/beheerde exemplaar (Preview)
Als de SSIS-catalogus wordt gehost in Azure SQL Database met de service-eindpunten virtueel netwerk of beheerd exemplaar (Preview), kunt u aanmelden bij uw Azure-SSIS-IR naar: 

- Hetzelfde virtuele netwerk 
- Een ander virtueel netwerk met een netwerk-naar-netwerkverbinding met de versie die wordt gebruikt voor Azure SQL Database met een virtueel netwerk service-eindpunten/beheerde exemplaar (Preview) 

Als u uw Azure-SSIS-IR aan hetzelfde virtuele netwerk als de instantie worden beheerd toevoegen, zorg ervoor dat de Azure-SSIS-IR zich in een ander subnet dan het exemplaar worden beheerd. Als u de Azure-SSIS-IR aan een ander virtueel netwerk dan het exemplaar worden beheerd koppelt, raden wij virtueel netwerk peering (dit is beperkt tot dezelfde regio) of een virtueel netwerk met virtuele netwerk. Zie [verbinding maken met uw toepassing met Azure SQL Database beheerd exemplaar](../sql-database/sql-database-managed-instance-connect-app.md).

Het virtuele netwerk kan worden geïmplementeerd via het klassieke implementatiemodel of het Azure Resource Manager-implementatiemodel.

De volgende secties vindt u meer informatie. 

## <a name="requirements-for-virtual-network-configuration"></a>Vereisten voor de configuratie van virtueel netwerk
-   Zorg ervoor dat `Microsoft.Batch` is een geregistreerde provider onder het abonnement van het subnet van uw virtuele netwerk die als host fungeert voor de Azure-SSIS-IR Als u een klassiek virtueel netwerk gebruikt, ook koppelen `MicrosoftAzureBatch` aan de rol Inzender van klassieke virtuele Machine voor het virtuele netwerk. 

-   Selecteer het juiste subnet voor het hosten van de Azure-SSIS-IR Zie [het subnet selecteert](#subnet). 

-   Als u uw eigen server Services DNS (Domain Name) op het virtuele netwerk gebruikt, raadpleegt u [domeinnaamservices server](#dns_server). 

-   Als u van een Netwerkbeveiligingsgroep (NSG) op het subnet gebruikmaakt, Zie [netwerkbeveiligingsgroep](#nsg) 

-   Als u met behulp van Azure Express Route of de gebruiker gedefinieerde Route (UDR) configureren, Zie [gebruik Azure ExpressRoute of door de gebruiker gedefinieerde Route](#route). 

-   Zorg ervoor dat de brongroep van het virtuele netwerk kunt maken en verwijderen van bepaalde resources van een Azure-netwerk. Zie [vereisten voor de resourcegroep](#resource-group). 

### <a name="subnet"></a> Het subnet selecteren
-   Selecteer het GatewaySubnet voor het implementeren van een Azure-SSIS-integratie-Runtime niet omdat deze is specifiek voor virtuele netwerkgateways. 

-   Zorg ervoor dat het subnet dat u voldoende beschikbare adresruimte voor Azure SSIS-IR te gebruiken. Laat ten minste 2 * knooppuntnummer IR in beschikbare IP-adressen. Azure bepaalde IP-adressen binnen elk subnet gereserveerd en deze adressen kunnen niet worden gebruikt. De eerste en laatste IP-adressen van de subnetten die zijn gereserveerd voor protocol overeenstemming, samen met drie meer adressen voor Azure-services gebruikt. Zie voor meer informatie [zijn er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets). 

-   Gebruik geen een subnet die uitsluitend wordt ingenomen door andere Azure-services (bijvoorbeeld beheerde exemplaar van SQL Database (Preview), App Service, enzovoort). 

### <a name="dns_server"></a> Domain Name Services-server 
Als u uw eigen server Services DNS (Domain Name) te gebruiken in een virtueel netwerk die worden toegevoegd door de runtime van uw Azure-SSIS-integratie wilt, controleert u of het openbare Azure-hostnamen kan omzetten (bijvoorbeeld een Azure Storage blob-naam, `<your storage account>.blob.core.windows.net`). 

De volgende stappen worden aanbevolen: 

-   Configureer aangepaste DNS voor het doorsturen van aanvragen naar Azure DNS. U kunt niet-omgezette DNS-records in de IP-adres van de Azure recursieve resolvers (168.63.129.16) doorsturen op uw eigen DNS-server. 

-   De aangepaste DNS als primaire en de Azure DNS als secundaire voor het virtuele netwerk instellen. Het IP-adres van de Azure recursieve resolvers (168.63.129.16) als een secundaire DNS-server registreren als uw eigen DNS-server niet beschikbaar is. 

Zie voor meer informatie [naamomzetting die gebruikmaakt van uw eigen DNS-server](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a> Netwerkbeveiligingsgroep
Als u nodig hebt voor het implementeren van een netwerkbeveiligingsgroep (NSG) in een virtueel netwerk die worden toegevoegd door de runtime van uw Azure-SSIS-integratie, kunt u binnenkomend en uitgaand verkeer via de volgende poorten: 

| Richting | Protocol-transport | Bron | Poortbereik van bron | Bestemming | Poortbereik van doel | Opmerkingen |
|---|---|---|---|---|---|---|
| Inkomend | TCP | Internet | * | VirtualNetwork | 29876, 29877 (als u de IR aan een virtueel netwerk van Azure Resource Manager koppelt) <br/><br/>10100, 20100, 30100 (als u de IR aan een klassiek virtueel netwerk koppelt)| De Data Factory-service gebruikt deze poorten om te communiceren met de knooppunten van de runtime van uw Azure-SSIS-integratie in het virtuele netwerk. <br/><br/> Of u een NSG of niet opgeeft, Data Factory altijd configureert u een NSG op het niveau van de netwerkinterfacekaarten (NIC's) gekoppeld aan de virtuele machines die als host fungeren van de Azure-SSIS-IR Alleen inkomend verkeer van Data Factory-IP-adressen is toegestaan. Zelfs als u deze poorten om internetverkeer te openen, is het verkeer van IP-adressen die geen Data Factory-IP-adressen op het niveau van de NIC geblokkeerd. |
| Uitgaand | TCP | VirtualNetwork | * | Internet | 443 | De knooppunten van de runtime van uw Azure-SSIS-integratie in het virtuele netwerk deze poort gebruiken voor toegang tot Azure-services, zoals Azure Storage en Azure Event Hubs. |
| Uitgaand | TCP | VirtualNetwork | * | Internet- of Sql | 1433, 11000 11999, 14000 14999 | De knooppunten van de runtime van uw Azure-SSIS-integratie in het virtuele netwerkgebruik deze poorten voor toegang tot SSISDB door de server van uw Azure SQL Database gehost - dit doel is niet van toepassing op SSISDB gehost door beheerde exemplaar (Preview). |
||||||||

### <a name="route"></a> Azure ExpressRoute gebruiken of door de gebruiker gedefinieerde Route
U kunt verbinding maken met een [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) circuit met de infrastructuur van uw virtuele netwerk naar uw on-premises netwerk uitbreiden naar Azure. 

Een algemene configuratie-instellingen is het gebruik van geforceerde tunneling (adverteren een BGP-route 0.0.0.0/0 aan het virtuele netwerk) waardoor uitgaand internetverkeer van de overdracht van het virtuele netwerk naar lokale netwerkapparaat voor controle en logboekregistratie. Dit netwerkverkeer verbreekt de connectiviteit tussen de Azure-SSIS-IR in het virtuele netwerk met afhankelijke Azure Data Factory-services. De oplossing is voor het definiëren van een (of meer) [gebruiker gedefinieerde routes (udr's)](../virtual-network/virtual-networks-udr-overview.md) op het subnet waarin de Azure-SSIS-IR Een UDR definieert subnet-specifieke routes die worden gehonoreerd in plaats van de BGP-route. 

Of u kunt de gebruiker gedefinieerde routes (udr's) om af te dwingen uitgaand internetverkeer vanuit het subnet die als host fungeert voor de Azure-SSIS-IR naar een ander subnet, die als host fungeert voor een virtueel netwerkapparaat als een firewall of een DMZ host definiëren voor controle en logboekregistratie. 

In beide gevallen moet een 0.0.0.0/0 route van toepassing met het volgende hoptype als **Internet** op het subnet die als host fungeert voor de Azure-SSIS-IR, zodat de communicatie tussen de Data Factory-service en de Azure-SSIS IS IR kan slagen. 

![Een route toevoegen](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Als u zich zorgen maakt over de mogelijkheid te verliezen uitgaand internetverkeer van dat subnet controleren, kunt u ook een NSG-regel toevoegen op het subnet om te beperken uitgaande bestemmingen [IP-adressen van Azure-datacentrum](https://www.microsoft.com/download/details.aspx?id=41653). 

Zie [dit PowerShell-script](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c) voor een voorbeeld. U moet uitvoeren van het script wekelijks up-to-date te houden de lijst met Azure data center IP-adres. 

### <a name="resource-group"></a> Vereisten voor resourcegroep
De Azure-SSIS-IR moet maken van bepaalde netwerkbronnen onder dezelfde resourcegroep bevinden als het virtuele netwerk, met inbegrip van een Azure load balancer, een Azure openbare IP-adres en een netwerkbeveiligingsgroep voor werk. 

-   Zorg ervoor dat er geen vergrendeling resource van de resourcegroep of abonnement waartoe het virtuele netwerk behoort. Als u een vergrendeling van alleen-lezen of een vergrendeling verwijderen configureert, kunnen starten en stoppen van de IR mislukken of vastlopen. 

-   Zorg ervoor dat er een Azure-beleid waarmee wordt voorkomen dat de volgende bronnen worden gemaakt onder de resourcegroep of abonnement waartoe het virtuele netwerk behoort geen: 
    -   Microsoft.Network/LoadBalancers 
    -   Microsoft.Network/NetworkSecurityGroups 
    -   Microsoft.Network/PublicIPAddresses 

## <a name="azure-portal-data-factory-ui"></a>Azure-portal (Data Factory-UI)
Deze sectie wordt beschreven hoe u een bestaande Azure SSIS-runtime toevoegen aan een virtueel netwerk (klassiek of Azure resourcemanager) met behulp van de Azure-portal en de Data Factory-gebruikersinterface. Eerst moet u het virtuele netwerk op de juiste wijze configureren voordat u uw Azure-SSIS-IR toevoegt aan het. Ga via een van de volgende twee secties op basis van het type van het virtuele netwerk (klassiek of Azure resourcemanager). Ga vervolgens verder met de derde sectie uw Azure-SSIS-IR koppelen aan het virtuele netwerk. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>De portal gebruiken voor het configureren van een virtueel netwerk van Azure Resource Manager
U moet een virtueel netwerk configureren voordat u kunt een Azure-SSIS-IR deelnemen aan. 

1. Start Microsoft Edge of Google Chrome. Op dit moment wordt wordt de Data Factory-gebruikersinterface alleen ondersteund in deze webbrowsers. 

2. Meld u aan bij [Azure Portal](https://portal.azure.com). 

3. Selecteer **meer services**. Filteren op en selecteer **virtuele netwerken**. 

4. Filteren op en selecteert u het virtuele netwerk in de lijst. 

5. Op de **virtueel netwerk** pagina **eigenschappen**. 

6. Selecteer de knop kopiëren voor **RESOURCE-ID** de resource-ID voor het virtuele netwerk naar het Klembord kopiëren. Sla de ID van het Klembord in OneNote of een bestand. 

7. Selecteer **subnetten** in het menu links. Zorg ervoor dat het aantal **beschikbare adressen** groter is dan de knooppunten in uw Azure-SSIS-integratie-runtime. 

8. Controleer of de Azure Batch-provider is geregistreerd in de Azure-abonnement dat het virtuele netwerk heeft. Of de Azure Batch-provider geregistreerd. Als u al een Azure Batch-account in uw abonnement hebt, wordt uw abonnement geregistreerd voor Azure Batch. (Als u de Azure-SSIS-IR in de Data Factory-portal maakt, de Azure Batch-provider wordt automatisch voor u geregistreerd.) 

   a. Selecteer in Azure portal **abonnementen** in het menu links. 

   b. Selecteer uw abonnement. 

   c. Selecteer **resourceproviders** aan de linkerkant en Bevestig dat **Microsoft.Batch** is een geregistreerde provider. 

   ![Bevestiging van de status 'Geregistreerd'](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Als er geen **Microsoft.Batch** in de lijst om te registreren, [leeg Azure Batch-account maken](../batch/batch-account-create-portal.md) in uw abonnement. U kunt deze later verwijderen. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>De portal gebruiken voor het configureren van een klassiek virtueel netwerk
U moet een virtueel netwerk configureren voordat u kunt een Azure-SSIS-IR deelnemen aan. 

1. Start Microsoft Edge of Google Chrome. Op dit moment wordt wordt de Data Factory-gebruikersinterface alleen ondersteund in deze browsers. 

2. Meld u aan bij [Azure Portal](https://portal.azure.com). 

3. Selecteer **meer services**. Filteren op en selecteer **virtuele netwerken (klassiek)**. 

4. Filteren op en selecteert u het virtuele netwerk in de lijst. 

5. Op de **virtuele netwerk (klassiek)** pagina **eigenschappen**. 

   ![De resource-ID klassiek virtueel netwerk](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

6. Selecteer de knop kopiëren voor **RESOURCE-ID** de resource-ID voor het klassieke netwerk naar het Klembord kopiëren. Sla de ID van het Klembord in OneNote of een bestand. 

7. Selecteer **subnetten** in het menu links. Zorg ervoor dat het aantal **beschikbare adressen** groter is dan de knooppunten in uw Azure-SSIS-integratie-runtime. 

   ![Aantal beschikbare adressen in het virtuele netwerk](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

8. Deelnemen aan **MicrosoftAzureBatch** naar de **klassieke Virtual Machine Contributor** rol voor het virtuele netwerk. 

    a. Selecteer **toegangsbeheer (IAM)** op het menu aan de linkerkant en selecteer **toevoegen** op de werkbalk. 

    !['Toegangsbeheer' en 'Add' knoppen](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Op de **machtigingen toevoegen** pagina **klassieke Virtual Machine Contributor** voor **rol**. Plakken **ddbf3205-c6bd-46ae-8127-60eb93363864** in de **Selecteer** vak en selecteer vervolgens **Microsoft Azure Batch** uit de lijst met zoekresultaten. 

    ![Zoekresultaten op de pagina 'Machtigingen toevoegen'](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    c. Selecteer **opslaan** de instellingen op te slaan en te sluiten van de pagina. 

    ![Opslaan van instellingen voor toegang](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    d. Controleer of u **Microsoft Azure Batch** in de lijst met inzenders. 

    ![Azure Batch toegang bevestigen](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

9. Controleer of de Azure Batch-provider is geregistreerd in de Azure-abonnement dat het virtuele netwerk heeft. Of de Azure Batch-provider geregistreerd. Als u al een Azure Batch-account in uw abonnement hebt, wordt uw abonnement geregistreerd voor Azure Batch. (Als u de Azure-SSIS-IR in de Data Factory-portal maakt, de Azure Batch-provider wordt automatisch voor u geregistreerd.) 

   a. Selecteer in Azure portal **abonnementen** in het menu links. 

   b. Selecteer uw abonnement. 

   c. Selecteer **resourceproviders** aan de linkerkant en Bevestig dat **Microsoft.Batch** is een geregistreerde provider. 

   ![Bevestiging van de status 'Geregistreerd'](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Als er geen **Microsoft.Batch** in de lijst om te registreren, [leeg Azure Batch-account maken](../batch/batch-account-create-portal.md) in uw abonnement. U kunt deze later verwijderen. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>De Azure-SSIS-IR toevoegen aan een virtueel netwerk
1. Start Microsoft Edge of Google Chrome. Op dit moment wordt wordt de Data Factory-gebruikersinterface alleen ondersteund in deze webbrowsers. 

2. In de [Azure-portal](https://portal.azure.com), selecteer **gegevensfactory** in het menu links. Als er geen **gegevensfactory** selecteren in het menu **meer services**, en selecteer **gegevensfactory** in de **INTELLIGENCE en analyse**sectie. 

   ![Lijst met gegevensfactory 's](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

3. Selecteer uw gegevensfactory met de Azure-SSIS-integratie runtime in de lijst. U Zie de startpagina van uw gegevensfactory. Selecteer de **auteur & implementeren** tegel. Op een afzonderlijke tabblad ziet u de Data Factory-gebruikersinterface. 

   ![Startpagina van de gegevensfactory](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

4. In de Data Factory-gebruikersinterface overschakelen naar de **bewerken** tabblad **verbindingen**, en schakel over naar de **integratie Runtimes** tabblad. 

   ![Tabblad 'Integratie runtimes'](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

5. Als uw Azure-SSIS-IR wordt uitgevoerd in de lijst van de runtime integratie selecteert u de **stoppen** knop in de **acties** kolom voor uw Azure-SSIS-IR U kunt een IR niet bewerken, totdat u het stopt. 

   ![De IR stoppen](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

6. Selecteer in de lijst van de runtime integratie de **bewerken** knop in de **acties** kolom voor uw Azure-SSIS-IR 

   ![De runtime integratie bewerken](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

7. Op de **algemene instellingen** pagina van de **integratie Runtime-instellingen** Selecteer **volgende**. 

   ![Algemene instellingen voor IR setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)

8. Op de **SQL-instellingen** pagina, voer het wachtwoord in en selecteer **volgende**. 

   ![SQL Server-instellingen voor IR setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)

9. Op de **geavanceerde instellingen** pagina, de volgende acties uitvoeren: 

   a. Schakel het selectievakje voor **selecteert u een VNet-naar-VNet-machtigingen/instellingen configureren voor uw Azure-SSIS-integratie-Runtime voor het koppelen van Azure-services toestaan**. 

   b. Voor **Type**, selecteer of het virtuele netwerk een klassiek virtueel netwerk of een virtueel netwerk van Azure Resource Manager. 

   c. Voor **VNet-naam**, selecteert u het virtuele netwerk. 

   d. Voor **subnetnaam**, selecteert u uw subnet in het virtuele netwerk. 

   e. Klik op **VNet validatie** en als dit lukt, klikt u op **Update**. 

   ![Geavanceerde instellingen voor IR setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

10. U kunt nu de IR starten met behulp van de **Start** knop in de **acties** kolom voor uw Azure-SSIS-IR Het duurt ongeveer 20-30 minuten een Azure-SSIS-IR starten 

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="configure-a-virtual-network"></a>Een virtueel netwerk configureren
U moet een virtueel netwerk configureren voordat u kunt uw Azure-SSIS-IR deelnemen aan. Toevoegen voor het automatisch configureren van virtueel netwerk machtigingsinstellingen voor uw Azure-SSIS-integratie runtime lid worden van het virtuele netwerk, het volgende script:

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Een Azure-SSIS-IR maken en toevoegen aan een virtueel netwerk
U kunt een Azure-SSIS-IR maken en toevoegen aan een virtueel netwerk op hetzelfde moment. Zie voor de volledige script en instructies [maken van een Azure-SSIS-integratie runtime](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Een bestaande Azure SSIS-IR toevoegen aan een virtueel netwerk
Het script in de [maken van een Azure-SSIS-integratie runtime](create-azure-ssis-integration-runtime.md) artikel laat zien hoe een Azure-SSIS-IR maken en toevoegen aan een virtueel netwerk in hetzelfde script. Als u een bestaande Azure SSIS-IR hebt, voert u de volgende stappen uit als u wilt toevoegen aan het virtuele netwerk: 
1. De Azure-SSIS-IR stoppen 
2. De Azure-SSIS-IR om lid van het virtuele netwerk te configureren. 
3. De Azure-SSIS-IR starten 

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
De runtime Azure SSIS-integratie te stoppen voordat u deze met een virtueel netwerk kan worden toegevoegd. Met deze opdracht alle knooppunten worden vrijgegeven en stopt facturering:

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Instellingen voor virtueel netwerk voor de Azure-SSIS-IR om lid te configureren
```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>De Azure-SSIS-IR configureren
Voor het configureren van de Azure-SSIS-integratie runtime als u wilt deelnemen aan het virtuele netwerk, voer de `Set-AzureRmDataFactoryV2IntegrationRuntime` opdracht: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>De Azure-SSIS-IR starten
Voer de volgende opdracht voor het starten van de runtime Azure SSIS-integratie: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Deze opdracht genereert 20-30 minuten duren.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de Azure-SSIS-runtime, in de volgende onderwerpen: 
- [Azure-SSIS-integratie runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel bevat conceptuele informatie over de integratie runtimes in het algemeen, met inbegrip van de Azure-SSIS-IR 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md). In dit artikel vindt stapsgewijze instructies voor het maken van een Azure-SSIS-IR Azure SQL Database wordt gebruikt voor het hosten van de SSIS-catalogus. 
- [Een Azure-SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). Dit artikel wordt op de zelfstudie wordt uitgebreid en bevat instructies over het gebruik van Azure SQL Database met een virtueel netwerk service-eindpunten/beheerde exemplaar (Preview) voor het hosten van de SSIS-catalogus en u de IR toevoegt aan een virtueel netwerk. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). In dit artikel leest u hoe u informatie over een Azure-SSIS IR ophaalt. Daarnaast bevat het artikel beschrijvingen van statuswaarden die worden gebruikt in de geretourneerde informatie. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u een Azure-SSIS IR stopt, start of verwijdert. Deze ook wordt beschreven hoe u uw Azure-SSIS-IR uitbreiden door knooppunten toe te voegen. 
