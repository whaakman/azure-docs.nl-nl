---
title: Azure-SSIS-integratie-runtime toevoegen aan een virtueel netwerk | Microsoft Docs
description: Meer informatie over hoe u de Azure-SSIS Integration runtime kunt koppelen aan een virtueel Azure-netwerk.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 3a1e272fa332c0bf0ee4e5ececa3edd83aec1d46
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543132"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Een Azure-SSIS-integratie-runtime toevoegen aan een virtueel netwerk
Wanneer u SQL Server Integration Services (SSIS) in Azure Data Factory (ADF) gebruikt, moet u uw Azure-SSIS Integration Runtime (IR) toevoegen aan een virtueel Azure-netwerk in de volgende scenario's: 

- U wilt verbinding maken met on-premises gegevens archieven van SSIS-pakketten die worden uitgevoerd op uw Azure-SSIS-IR zonder zelf-Hostende IR te configureren/beheren als proxy. 

- U wilt verbinding maken met Azure-service bronnen die worden ondersteund door service-eind punten van het virtuele netwerk van SSIS-pakketten die worden uitgevoerd op uw Azure-SSIS-IR.

- U gaat de SSIS-catalogus database (SSISDB) hosten in Azure SQL Database met virtuele netwerk service-eind punten/beheerde exemplaren in een virtueel netwerk. 

Met ADF kunt u uw Azure-SSIS IR toevoegen aan een virtueel netwerk dat is gemaakt via het klassieke implementatie model of het Azure Resource Manager-implementatie model. 

> [!IMPORTANT]
> Het klassieke virtuele netwerk wordt momenteel afgeschaft. gebruik in plaats daarvan het Azure Resource Manager virtuele netwerk.  Als u het klassieke virtuele netwerk al gebruikt, kunt u zo snel mogelijk overschakelen op het gebruik van het Azure Resource Manager virtuele netwerk.

## <a name="access-to-on-premises-data-stores"></a>Toegang tot on-premises gegevens archieven
Als uw SSIS-pakketten toegang hebben tot on-premises gegevens opslag, kunt u uw Azure-SSIS IR toevoegen aan een virtueel netwerk dat is verbonden met het on-premises netwerk of een zelf-Hostende IR configureren/beheren als proxy voor uw Azure-SSIS-IR. Zie [zelf-hostende IR configureren als een proxy voor het Azure-SSIS-IR-](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) artikel. Wanneer u uw Azure-SSIS-IR wilt toevoegen aan een virtueel netwerk, moet u rekening houden met enkele belang rijke punten: 

- Als er geen bestaand virtueel netwerk is verbonden met uw on-premises netwerk, maakt u eerst een [Azure Resource Manager virtueel netwerk](../virtual-network/quick-create-portal.md#create-a-virtual-network) of een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) voor uw Azure SSIS Integration runtime om samen te voegen. Configureer vervolgens een site-naar-site [-VPN-gateway verbinding](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) of een [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) -verbinding van het virtuele netwerk naar uw on-premises netwerk. 

- Als er een bestaand Azure Resource Manager of een klassiek virtueel netwerk is verbonden met uw on-premises netwerk op dezelfde locatie als uw Azure-SSIS-IR, kunt u de IR toevoegen aan dat virtuele netwerk. 

- Als er een bestaand klassiek virtueel netwerk is verbonden met uw on-premises netwerk op een andere locatie dan uw Azure-SSIS-IR, kunt u eerst een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) maken om uw Azure-SSIS-IR te koppelen. Configureer vervolgens een [klassieke-naar-klassieke virtuele netwerk](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) verbinding. U kunt ook een [Azure Resource Manager virtueel netwerk](../virtual-network/quick-create-portal.md#create-a-virtual-network) maken voor uw Azure SSIS Integration runtime om samen te voegen. Configureer vervolgens een [klassieke-naar-Azure Resource Manager virtuele netwerk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) verbinding. 
 
- Als er een bestaand Azure Resource Manager virtuele netwerk is verbonden met uw on-premises netwerk op een andere locatie dan uw Azure-SSIS-IR, kunt u eerst een [Azure Resource Manager virtueel netwerk](../virtual-network/quick-create-portal.md##create-a-virtual-network) maken voor uw Azure-SSIS-IR om deel te nemen. Configureer vervolgens een Azure Resource Manager-to-Azure Resource Manager virtuele netwerk verbinding. U kunt ook een [klassiek virtueel netwerk](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) maken voor uw Azure-SSIS-IR om samen te voegen. Configureer vervolgens een [klassieke-naar-Azure Resource Manager virtuele netwerk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) verbinding. 

## <a name="access-to-azure-services-with-virtual-network-service-endpoints"></a>Toegang tot Azure-Services met virtuele-netwerk service-eind punten
Als uw SSIS-pakketten toegang krijgen tot Azure-service bronnen die worden ondersteund door [service-eind punten van virtuele netwerken](../virtual-network/virtual-network-service-endpoints-overview.md) en u deze bronnen wilt beveiligen met Azure-SSIS IR, kunt u uw Azure-SSIS IR toevoegen aan het subnet van het virtuele netwerk dat is geconfigureerd met het virtuele netwerk Service-eind punt en ondertussen virtuele-netwerk regel toevoegen aan Azure-service resource om toegang vanaf hetzelfde subnet toe te staan.

## <a name="host-the-ssis-catalog-database-in-azure-sql-database-with-virtual-network-service-endpointsmanaged-instance"></a>De SSIS-catalogus database hosten in Azure SQL Database met virtuele netwerk service-eind punten/beheerde exemplaren
Als u uw SSIS-catalogus in Azure SQL Database met virtuele netwerk service-eind punten host, moet u ervoor zorgen dat u uw Azure-SSIS IR aan hetzelfde virtuele netwerk en subnet koppelt.

Als u uw Azure-SSIS IR aan hetzelfde virtuele netwerk als het beheerde exemplaar koppelt, moet u ervoor zorgen dat de Azure-SSIS IR zich in een ander subnet bevindt dan het beheerde exemplaar. Als u uw Azure-SSIS IR koppelt aan een ander virtueel netwerk dan het beheerde exemplaar, raden we u aan virtuele netwerk peering (die beperkt is tot dezelfde regio) of een virtueel netwerk te koppelen aan een virtuele netwerk verbinding. Zie [uw toepassing verbinden met Azure SQL database beheerde instantie](../sql-database/sql-database-managed-instance-connect-app.md).

In alle gevallen kan het virtuele netwerk alleen worden geïmplementeerd via het Azure Resource Manager-implementatie model.

In de volgende secties vindt u meer informatie. 

## <a name="requirements-for-virtual-network-configuration"></a>Vereisten voor de configuratie van virtuele netwerken
-   Zorg ervoor dat `Microsoft.Batch` u een geregistreerde provider hebt onder het abonnement van het subnet van het virtuele netwerk dat als host fungeert voor de Azure-SSIS IR. Als u een klassiek virtueel netwerk gebruikt, voegt `MicrosoftAzureBatch` u ook toe aan de rol van de klassieke virtuele machine voor dat virtuele netwerk. 

-   Zorg ervoor dat u over de vereiste machtigingen beschikt. Bekijk de [vereiste machtigingen](#perms).

-   Selecteer het juiste subnet om de Azure-SSIS IR te hosten. Zie [het subnet selecteren](#subnet). 

-   Als u uw eigen DNS-server (Domain Name Services) in het virtuele netwerk gebruikt, raadpleegt u [Domain Name Services server](#dns_server). 

-   Als u een netwerk beveiligings groep (NSG) in het subnet gebruikt, raadpleegt u [netwerk beveiligings groep](#nsg) 

-   Als u Azure Express route gebruikt of een door de gebruiker gedefinieerde route (UDR) configureert, raadpleegt u [Azure ExpressRoute of door de gebruiker gedefinieerde route gebruiken](#route). 

-   Zorg ervoor dat de resource groep van het virtuele netwerk bepaalde Azure-netwerk resources kan maken en verwijderen. Zie de [vereisten voor de resource groep](#resource-group). 

-   Als u uw Azure-SSIS-IR aanpast zoals beschreven in het artikel [aangepaste installatie voor Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) , worden er voor uw Azure-SSIS-IR-knoop punten particuliere IP-adressen van een vooraf gedefinieerd bereik van 172.16.0.0-172.31.255.255 toegewezen. Zorg er dus voor dat het privé-IP-adres adresbereiken van uw virtuele/on-premises netwerken veroorzaken geen conflicten met dit bereik.

Hier volgt een diagram met de vereiste verbindingen voor uw Azure-SSIS-IR:

![Azure-SSIS IR](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir.png)

### <a name="perms"></a>Vereiste machtigingen

De gebruiker die de Azure-SSIS Integration Runtime maakt, moet de volgende machtigingen hebben:

- Als u uw SSIS-IR koppelt aan een Azure Resource Manager virtueel netwerk, hebt u twee opties:

  - Gebruik de ingebouwde rol *netwerk bijdrager* . Deze rol wordt geleverd met het _micro soft.\* Network/-_ machtiging, met een veel groter bereik dan nodig is.

  - Maak een aangepaste rol die alleen de benodigde machtigingen voor _micro soft. Network/\*virtualNetworks//join/Action_ bevat. 

- Als u uw SSIS-IR aan een klassiek virtueel netwerk koppelt, wordt u aangeraden de ingebouwde rol van de *klassieke virtuele machine* te gebruiken. Anders moet u een aangepaste rol definiëren die de machtiging bevat om lid te worden van het virtuele netwerk.

### <a name="subnet"></a>Het subnet selecteren
-   Selecteer niet de GatewaySubnet voor het implementeren van een Azure-SSIS Integration Runtime, omdat deze is toegewezen aan virtuele netwerk gateways. 

-   Zorg ervoor dat het door u geselecteerde subnet voldoende beschik bare adres ruimte heeft voor het gebruik van Azure-SSIS IR. Plaats ten minste 2 * IR-knooppunt nummer in beschik bare IP-adressen. Azure reserveert een aantal IP-adressen binnen elk subnet en deze adressen kunnen niet worden gebruikt. Het eerste en laatste IP-adres van de subnetten zijn gereserveerd voor protocol conformiteit, samen met drie meer adressen die worden gebruikt voor Azure-Services. Zie [zijn er beperkingen voor het gebruik van IP-adressen in deze subnetten?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)voor meer informatie. 

-   Gebruik geen subnet dat uitsluitend wordt ingen Omen door andere Azure-Services (bijvoorbeeld SQL Database beheerde instantie, App Service enz.). 

### <a name="dns_server"></a>Domain Name Services-server 
Als u uw eigen DNS-server (Domain Name Services) wilt gebruiken in een virtueel netwerk dat is gekoppeld aan uw Azure SSIS Integration runtime, moet u ervoor zorgen dat de globale namen van Azure-hosts worden omgezet (bijvoorbeeld een `<your storage account>.blob.core.windows.net`Azure Storage BLOB-naam). 

De volgende stappen worden aanbevolen: 

-   Aangepaste DNS configureren voor het door sturen van aanvragen naar Azure DNS. U kunt niet-omgezette DNS-records door sturen naar het IP-adres van de recursieve resolvers van Azure (168.63.129.16) op uw eigen DNS-server. 

-   Stel de aangepaste DNS in als primair en Azure DNS als secundair voor het virtuele netwerk. Registreer het IP-adres van recursieve resolvers (168.63.129.16) van Azure als secundaire DNS-server voor het geval uw eigen DNS-server niet beschikbaar is. 

Zie [naam omzetting die gebruikmaakt van uw eigen DNS-server](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)voor meer informatie. 

### <a name="nsg"></a>Netwerk beveiligings groep
Als u een netwerk beveiligings groep (NSG) moet implementeren voor het subnet dat door uw Azure SSIS Integration runtime wordt gebruikt, kunt u inkomend/uitgaand verkeer via de volgende poorten toestaan: 

| Direction | Transport Protocol | Source | Bron poort bereik | Bestemming | Doel poort bereik | Opmerkingen |
|---|---|---|---|---|---|---|
| Inkomend | TCP | BatchNodeManagement | * | VirtualNetwork | 29876, 29877 (als u de IR koppelt aan een Azure Resource Manager virtueel netwerk) <br/><br/>10100, 20100, 30100 (als u de IR koppelt aan een klassiek virtueel netwerk)| De Data Factory-service gebruikt deze poorten om te communiceren met de knoop punten van uw Azure SSIS Integration runtime in het virtuele netwerk. <br/><br/> Of u nu een NSG op subnetniveau maakt, Data Factory altijd een NSG configureert op het niveau van de netwerk interface kaarten (Nic's) die zijn gekoppeld aan de virtuele machines die de Azure-SSIS-IR hosten. Alleen binnenkomend verkeer van Data Factory IP-adressen op de opgegeven poorten is toegestaan door deze NSG op NIC-niveau. Zelfs als u deze poorten opent op het Internet verkeer op subnetniveau, wordt verkeer van IP-adressen die niet Data Factory IP-adressen worden geblokkeerd op het niveau van de NIC. |
| Uitgaande | TCP | VirtualNetwork | * | AzureCloud | 443 | De knoop punten van uw Azure SSIS Integration runtime in het virtuele netwerk gebruiken deze poort voor toegang tot Azure-Services, zoals Azure Storage en Azure Event Hubs. |
| Uitgaande | TCP | VirtualNetwork | * | Internet | 80 | De knoop punten van uw Azure SSIS Integration runtime in het virtuele netwerk gebruiken deze poort voor het downloaden van de intrekkings lijst voor certificaten van Internet. |
| Uitgaande | TCP | VirtualNetwork | * | SQL | 1433, 11000-11999 | De knoop punten van uw Azure SSIS Integration runtime in het virtuele netwerk gebruiken deze poorten om toegang te krijgen tot de SSISDB die door uw Azure SQL Database-Server worden gehost. Als uw Azure SQL Database Server-verbindings beleid is ingesteld op **proxy** in plaats van de omleiding, is alleen poort 1433 vereist. Deze uitgaande beveiligings regel is niet van toepassing op SSISDB die worden gehost door uw beheerde instantie in het virtuele netwerk. |
||||||||

### <a name="route"></a>Azure ExpressRoute of door de gebruiker gedefinieerde route gebruiken
Wanneer u een [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) -circuit verbindt met uw virtuele netwerk infrastructuur om uw on-premises netwerk naar Azure uit te breiden, is een gemeen schappelijke configuratie het gebruik van geforceerde tunneling (adverteren van een BGP-route, 0.0.0.0/0 naar het virtuele netwerk). Dit forceert uitgaand Internet verkeer van de virtuele netwerk stroom naar een on-premises netwerk apparaat voor inspectie en logboek registratie. 
 
Of u kunt door de [gebruiker gedefinieerde routes (udr's)](../virtual-network/virtual-networks-udr-overview.md) definiëren om uitgaand Internet verkeer te forceren van het subnet dat als host fungeert voor de Azure-SSIS IR naar een ander subnet, die als host fungeert voor een Virtual Network apparaat (NVA) als een firewall of Azure Firewall voor inspectie en logboek registratie.
 
In beide gevallen verbreekt de verkeers route de vereiste binnenkomende verbindingen van afhankelijke Azure Data Factory Services (Azure Batch beheer Services specifiek) naar Azure-SSIS IR in het virtuele netwerk. 
 
De oplossing is het definiëren van een (of meer) door de gebruiker gedefinieerde routes (Udr's) op het subnet dat de Azure-SSIS IR bevat. 

- U kunt de route 0.0.0.0/0 Toep assen met het type van de volgende hop als **Internet** op het subnet dat als host fungeert voor de Azure-SSIS IR in het Azure ExpressRoute-scenario of bestaande% 0-route wijzigen van het volgende hop-type als **virtueel apparaat** op **Internet** in NVA omstandigheden.

![Een route toevoegen](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)
- Als u zich zorgen maakt dat de mogelijkheid om uitgaand Internet verkeer vanuit dat subnet te controleren, kwijtraakt. U kunt specifieke Udr's definiëren om alleen verkeer tussen Azure Batch-beheer Services en Azure-SSIS IR te routeren met het type van de volgende hop als **Internet**.
Bijvoorbeeld Als uw Azure-SSIS-IR zich op `UK South`heeft, moet u een lijst met IP-adres bereik van `BatchNodeManagement.UKSouth` de service Tags ophalen uit de [Download koppeling voor het IP-adres bereik](https://www.microsoft.com/en-us/download/details.aspx?id=56519) of via de [service tag discovery-API](https://aka.ms/discoveryapi). Daarna worden de volgende Udr's van gerelateerde IP-bereik route toegepast met het type volgende hop als **Internet**.

![AzureBatch UDR-instellingen](media/join-azure-ssis-integration-runtime-virtual-network/azurebatch-udr-settings.png)
> [!NOTE]
> Er zijn extra onderhouds kosten van deze aanpak die u regel matig moet controleren en het nieuwe IP-adres bereik kunt toevoegen aan uw UDR om te voor komen dat Azure-SSIS IR wordt bereikt. Wanneer er een nieuw IP-adres wordt weer gegeven in het label service, duurt het een andere maand voordat een nieuw IP-adres van kracht wordt. Daarom wordt u aangeraden om het IP-bereik maandelijks te controleren. 

### <a name="resource-group"></a>Vereisten voor de resource groep
-   De Azure-SSIS IR moet bepaalde netwerk bronnen maken onder dezelfde resource groep als het virtuele netwerk. Deze resources omvatten het volgende:
    -   Een Azure-Load Balancer, met de naam  *\<GUID >-azurebatch-cloudserviceloadbalancer*.
    -   Een openbaar IP-adres van Azure, met de naam  *\<GUID >-azurebatch-cloudservicepublicip*.
    -   Een netwerk beveiligings groep, met de naam  *\<GUID >-azurebatch-cloudservicenetworksecuritygroup*. 

    Deze resources worden gemaakt wanneer IR wordt gestart en verwijderd wanneer IR wordt gestopt. U kunt ze niet opnieuw gebruiken in uw andere resources, anders wordt IR-stop geblokkeerd. 

-   Zorg ervoor dat u geen resource vergrendeling hebt voor de resource groep of het abonnement waartoe het virtuele netwerk behoort. Als u een alleen-lezen vergrendeling of een verwijderings vergrendeling configureert, kan het starten en stoppen van de IR mislukken of reageert niet meer. 

-   Zorg ervoor dat u geen Azure-beleid hebt waarmee wordt voor komen dat de volgende resources worden gemaakt onder de resource groep of het abonnement waartoe het virtuele netwerk behoort: 
    -   Microsoft.Network/LoadBalancers 
    -   Microsoft.Network/NetworkSecurityGroups 
    -   Microsoft.Network/PublicIPAddresses 

### <a name="faq"></a>FAQ

- Hoe wordt het open bare IP-adres dat wordt weer gegeven op Azure-SSIS IR voor een binnenkomende verbinding beschermd? En is het mogelijk om het open bare IP-adres te verwijderen?
 
    Op dit moment wordt het open bare IP-adres automatisch gemaakt wanneer Azure-SSIS IR samenvoegt VNet. We hebben NSG op NIC-niveau nodig om alleen Azure Batch-beheer Services toe te staan om binnenkomende verbindingen met Azure-SSIS IR te maken en u kunt ook NSG op subnetniveau opgeven voor inkomende beveiliging.

    Als u het open bare IP-adres niet wilt weer gegeven, kunt u overwegen om [zelf-hostende IR te configureren als proxy voor Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) in plaats van VNet als deze wordt toegepast op uw scenario.
 
- Is er een statisch IP-adres van Azure-SSIS IR dat in een lijst met firewalls voor toegang tot de gegevens bron kan worden geplaatst?
 
    - Als uw gegevens bron on-premises is, nadat u een virtueel netwerk hebt verbonden met uw on-premises netwerk en uw Azure-SSIS-IR toevoegt aan het subnet van het virtuele netwerk, kunt u het IP-adres bereik van dat subnet in de lijst met toegestane bronnen plaatsen.
    - Als uw gegevens bron wordt ondersteund door de Azure-service met het service-eind punt van de virtuele netwerk, kunt u het service punt van het virtuele netwerk in het virtuele netwerk configureren en uw Azure-SSIS-IR toevoegen aan het subnet van het virtuele netwerk. vervolgens kunt u de regel voor het virtuele netwerk gebruiken van Azure-Services in plaats van IP-bereik om toegang toe te staan.
    - Als uw gegevens bron andere gegevens bronnen in de Cloud is, kunt u UDR gebruiken om uitgaand verkeer van Azure-SSIS IR te routeren naar NVA of Azure Firewall met een statisch openbaar IP-adres, zodat u dit open bare IP-adres van NVA of Azure-firewall in de lijst met toegestane adressen kunt plaatsen.
    - Als u geen van de bovenstaande vereisten hebt, kunt u evalueren of uw toegang tot de gegevens bron kan worden uitgevoerd via [zelf-hostende IR configureren als proxy voor Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). vervolgens kunt u het IP-adres van de computer waarop zelf-HOSTende IR wordt gehost, in plaats van Azure-SSIS IR toevoegen aan VNet.

## <a name="azure-portal-data-factory-ui"></a>Azure Portal (Data Factory gebruikers interface)
In deze sectie wordt beschreven hoe u een bestaande Azure-SSIS-runtime kunt koppelen aan een virtueel netwerk (klassiek of Azure Resource Manager) met behulp van de Azure Portal en Data Factory gebruikers interface. Eerst moet u het virtuele netwerk op de juiste wijze configureren voordat u uw Azure-SSIS-IR eraan koppelt. Ga naar een van de volgende twee secties op basis van het type van uw virtuele netwerk (klassiek of Azure Resource Manager). Ga vervolgens verder met het derde gedeelte om uw Azure-SSIS-IR toe te voegen aan het virtuele netwerk. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>De portal gebruiken om een Azure Resource Manager virtueel netwerk te configureren
U moet een virtueel netwerk configureren voordat u hieraan een Azure-SSIS-IR kunt toevoegen. 

1. Start micro soft Edge of Google Chrome. Op dit moment wordt de Data Factory gebruikers interface alleen ondersteund in deze webbrowsers. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

1. Selecteer **meer services**. Filter voor en selecteer **virtuele netwerken**. 

1. Filter en selecteer uw virtuele netwerk in de lijst. 

1. Selecteer op de pagina **virtueel netwerk** **Eigenschappen**. 

1. Selecteer de knop kopiëren voor de **resource-id** om de resource-id voor het virtuele netwerk naar het klem bord te kopiëren. Sla de ID op in het klem bord in OneNote of in een bestand. 

1. Selecteer **subnetten** in het menu links. Zorg ervoor dat het aantal **beschik bare adressen** groter is dan de knoop punten in uw Azure SSIS Integration runtime. 

1. Controleer of de Azure Batch provider is geregistreerd in het Azure-abonnement met het virtuele netwerk. U kunt ook de Azure Batch provider registreren. Als u al een Azure Batch account in uw abonnement hebt, wordt uw abonnement geregistreerd voor Azure Batch. (Als u Azure-SSIS IR maakt in de Data Factory Portal, wordt de Azure Batch provider automatisch voor u geregistreerd.) 

   a. Selecteer in Azure Portal de optie **abonnementen** in het menu links. 

   b. Selecteer uw abonnement. 

   c. Selecteer **resource providers** aan de linkerkant en controleer of **micro soft. batch** een geregistreerde provider is. 

   ![Bevestiging van de status ' geregistreerd '](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Als u **micro soft. batch** niet in de lijst ziet, [maakt u een leeg Azure batch account](../batch/batch-account-create-portal.md) in uw abonnement om het te registreren. U kunt deze later verwijderen. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>De portal gebruiken om een klassiek virtueel netwerk te configureren
U moet een virtueel netwerk configureren voordat u hieraan een Azure-SSIS-IR kunt toevoegen. 

1. Start micro soft Edge of Google Chrome. Op dit moment wordt de Data Factory gebruikers interface alleen ondersteund in deze webbrowsers. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

1. Selecteer **meer services**. Filter voor en selecteer **virtuele netwerken (klassiek)** . 

1. Filter en selecteer uw virtuele netwerk in de lijst. 

1. Selecteer op de pagina **virtueel netwerk (klassiek)** de optie **Eigenschappen**. 

   ![Resource-ID van klassiek virtueel netwerk](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Selecteer de knop kopiëren voor **resource-id** om de resource-id voor het klassieke netwerk naar het klem bord te kopiëren. Sla de ID op in het klem bord in OneNote of in een bestand. 

1. Selecteer **subnetten** in het menu links. Zorg ervoor dat het aantal **beschik bare adressen** groter is dan de knoop punten in uw Azure SSIS Integration runtime. 

   ![Aantal beschik bare adressen in het virtuele netwerk](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. Voeg **MicrosoftAzureBatch** toe aan de rol **Inzender voor klassieke virtuele machines** voor het virtuele netwerk. 

    a. Selecteer **toegangs beheer (IAM)** in het menu links en selecteer het tabblad **roltoewijzingen** . 

    ![De knoppen toegangs beheer en toevoegen](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Selecteer **roltoewijzing toevoegen**.

    c. Selecteer op de pagina **roltoewijzing toevoegen** de optie **klassieke Inzender voor virtuele machines** voor **rol**. Plak **ddbf3205-c6bd-46ae-8127-60eb93363864** in het **selectie** vakje en selecteer vervolgens **Microsoft Azure batch** in de lijst met zoek resultaten. 

    ![Zoek resultaten op de pagina functie toewijzing toevoegen](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    d. Selecteer **Opslaan** om de instellingen op te slaan en de pagina te sluiten. 

    ![Toegangs instellingen opslaan](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    e. Controleer of **Microsoft Azure batch** wordt weer geven in de lijst met inzenders. 

    ![Azure Batch toegang bevestigen](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Controleer of de Azure Batch provider is geregistreerd in het Azure-abonnement met het virtuele netwerk. U kunt ook de Azure Batch provider registreren. Als u al een Azure Batch account in uw abonnement hebt, wordt uw abonnement geregistreerd voor Azure Batch. (Als u Azure-SSIS IR maakt in de Data Factory Portal, wordt de Azure Batch provider automatisch voor u geregistreerd.) 

   a. Selecteer in Azure Portal de optie **abonnementen** in het menu links. 

   b. Selecteer uw abonnement. 

   c. Selecteer **resource providers** aan de linkerkant en controleer of **micro soft. batch** een geregistreerde provider is. 

   ![Bevestiging van de status ' geregistreerd '](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Als u **micro soft. batch** niet in de lijst ziet, [maakt u een leeg Azure batch account](../batch/batch-account-create-portal.md) in uw abonnement om het te registreren. U kunt deze later verwijderen. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>De Azure-SSIS IR toevoegen aan een virtueel netwerk
1. Start micro soft Edge of Google Chrome. Op dit moment wordt de Data Factory gebruikers interface alleen ondersteund in deze webbrowsers. 

1. Selecteer in het [Azure Portal](https://portal.azure.com) **gegevens fabrieken** in het menu links. Als er geen **gegevens fabrieken** worden weer gegeven in het menu, selecteert u **meer services**en de sectie **gegevens fabrieken** selecteren in het gedeelte **Intelligence en analyse** . 

   ![Lijst met gegevens fabrieken](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Selecteer uw data factory met de Azure SSIS Integration runtime in de lijst. U ziet de start pagina voor uw data factory. Selecteer de tegel **auteur & implementeren** . U ziet de Data Factory gebruikers interface op een afzonderlijk tabblad. 

   ![Startpagina van de gegevensfactory](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. Ga in de Data Factory-gebruikers interface naar het tabblad **bewerken** , selecteer **verbindingen**en schakel over naar het tabblad **Integration Runtimes** . 

   ![Tabblad Integration Runtimes](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Als uw Azure-SSIS IR wordt uitgevoerd, selecteert u in de lijst Integration runtime de knop **stoppen** in de kolom **acties** voor uw Azure-SSIS IR. U kunt een IR pas bewerken nadat u deze hebt gestopt. 

   ![De IR stoppen](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. Selecteer in de lijst Integration runtime de knop **bewerken** in de kolom **acties** voor uw Azure-SSIS-IR. 

   ![De Integration runtime bewerken](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. Ga in **het configuratie** venster van Integration runtime naar de pagina **algemene instellingen** en **SQL-instellingen** en klik op de knop **volgende** . 

1. Voer op de pagina **Geavanceerde instellingen** de volgende acties uit: 

   a. Schakel het selectie vakje **een VNet... selecteren** in. 

   b. Selecteer bij **abonnement**het Azure-abonnement waarmee u een bestaand virtueel netwerk kunt selecteren. 
  
   c. Selecteer voor **VNet-naam**het virtuele netwerk. 

   d. Selecteer bij **subnet naam**het subnet in het virtuele netwerk. 

   e. Als u ook een zelf-Hostende IR wilt configureren/beheren als proxy voor uw Azure-SSIS-IR, schakelt u het selectie vakje **zelf gehost...** in en raadpleegt u zelf-Hostende [IR configureren als proxy voor het Azure-SSIS IR-](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) artikel.

   f. Klik op de knop **VNet-validatie** en klik vervolgens op de knop **volgende** . 

   ![Geavanceerde instellingen voor IR-installatie](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

1. Controleer op de pagina **samen vatting** alle instellingen voor uw Azure-SSIS-IR en klik op de knop **bijwerken** .

1. U kunt nu uw Azure-SSIS-IR starten door te klikken op de knop **Start** in de kolom **acties** voor uw Azure-SSIS-IR. Het duurt ongeveer 20 tot 30 minuten om uw Azure-SSIS-IR te starten die wordt gekoppeld aan een virtueel netwerk. 

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="configure-a-virtual-network"></a>Een virtueel netwerk configureren
U moet een virtueel netwerk configureren voordat u de Azure-SSIS-IR eraan kunt toevoegen. Voeg het volgende script toe om de machtigingen/instellingen van het virtuele netwerk voor uw Azure-SSIS Integration runtime automatisch te configureren om lid te worden van het virtuele netwerk:

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

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Een Azure-SSIS-IR maken en toevoegen aan een virtueel netwerk
U kunt een Azure-SSIS-IR maken en deze tegelijkertijd toevoegen aan een virtueel netwerk. Zie [een Azure SSIS Integration runtime maken](create-azure-ssis-integration-runtime.md#azure-powershell)voor het volledige script en de instructies.

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Een bestaande Azure-SSIS-IR toevoegen aan een virtueel netwerk
Het script in het artikel [een Azure-SSIS Integration runtime maken](create-azure-ssis-integration-runtime.md) laat zien hoe u een Azure-SSIS-IR maakt en aan een virtueel netwerk in hetzelfde script koppelt. Als u een bestaande Azure-SSIS-IR hebt, voert u de volgende stappen uit om deze toe te voegen aan het virtuele netwerk: 
1. Stop de Azure-SSIS IR. 
1. Configureer de Azure-SSIS IR om lid te worden van het virtuele netwerk. 
1. Start de Azure-SSIS IR. 

### <a name="define-the-variables"></a>De variabelen definiëren
```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>De Azure-SSIS IR stoppen
Stop de Azure SSIS Integration runtime voordat u deze kunt koppelen aan een virtueel netwerk. Met deze opdracht worden alle knoop punten vrijgegeven en wordt de facturering gestopt:

```powershell
Stop-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Configureer de instellingen van het virtuele netwerk voor de Azure-SSIS IR om samen te voegen
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
Als u de Azure-SSIS Integration runtime wilt configureren om lid te worden van het `Set-AzDataFactoryV2IntegrationRuntime` virtuele netwerk, voert u de volgende opdracht uit: 

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Start de Azure-SSIS IR
Voer de volgende opdracht uit om de Azure-SSIS Integration runtime te starten: 

```powershell
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Het volt ooien van deze opdracht duurt 20 tot 30 minuten.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over Azure-SSIS Integration runtime: 
- [Azure-SSIS Integration runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime). Dit artikel bevat conceptuele informatie over Integration Runtimes in het algemeen, inclusief Azure-SSIS IR. 
- [Zelfstudie: SSIS-pakketten implementeren in Azure](tutorial-create-azure-ssis-runtime-portal.md). Deze zelf studie bevat stapsgewijze instructies voor het maken van uw Azure-SSIS-IR. Er wordt gebruikgemaakt van Azure SQL Database voor het hosten van de SSIS-catalogus. 
- [Een Azure-SSIS Integration Runtime maken](create-azure-ssis-integration-runtime.md). In dit artikel wordt de zelf studie uitgebreid en vindt u instructies over het gebruik van Azure SQL Database met virtuele netwerk service-eind punten/beheerde exemplaren in een virtueel netwerk om de SSIS-catalogus te hosten en uw Azure-SSIS-IR toe te voegen aan een virtueel netwerk. 
- [Een Azure-SSIS IR controleren](monitor-integration-runtime.md#azure-ssis-integration-runtime). Dit artikel laat u zien hoe u informatie kunt ophalen over uw Azure-SSIS-IR en status beschrijvingen in de geretourneerde informatie verstrekt. 
- [Een Azure-SSIS IR beheren](manage-azure-ssis-integration-runtime.md). In dit artikel leest u hoe u uw Azure-SSIS-IR kunt stoppen, starten of verwijderen. U ziet ook hoe u uw Azure-SSIS-IR kunt schalen door knoop punten toe te voegen.
