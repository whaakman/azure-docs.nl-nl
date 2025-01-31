---
title: Overzicht van groepen van Azure-beveiliging
titlesuffix: Azure Virtual Network
description: Meer informatie over netwerk- en toepassingsbeveiligingsgroepen. Beveiligingsgroepen helpen u bij het filteren van netwerkverkeer tussen Azure-resources.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2018
ms.author: malop;kumud
ms.openlocfilehash: 99a55d0cd06e6f1a92a70b20447d300dbc05eee1
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709541"
---
# <a name="security-groups"></a>Beveiligingsgroepen
<a name="network-security-groups"></a>

U kunt netwerkverkeer naar en van Azure-resources in een [virtueel Azure-netwerk](virtual-networks-overview.md) filteren met een netwerkbeveiligingsgroep. Een netwerkbeveiligingsgroep bevat [beveiligingsregels](#security-rules) waarmee binnenkomend netwerkverkeer naar, of uitgaand netwerkverkeer van diverse typen Azure-resources kan worden toegestaan of geweigerd. Zie [Integratie van virtuele netwerken voor Azure-services](virtual-network-for-azure-services.md) voor meer informatie over de Azure-resources die in een virtueel netwerk kunnen worden geïmplementeerd en waaraan netwerkbeveiligingsgroepen kunnen worden gekoppeld. Voor elke regel kunt u de bron en het doel, de poort en het protocol opgeven.

In dit artikel worden de concepten van netwerkbeveiligingsgroepen uitgelegd, zodat u ze effectief kunt gebruiken. Als u nog nooit een netwerkbeveiligingsgroep hebt gemaakt, kunt u een snelle [zelfstudie](tutorial-filter-network-traffic.md) uitvoeren om enige ervaring met het maken van een netwerkbeveiligingsgroep op te doen. Zie [Een netwerkbeveiligingsgroep maken, wijzigen of verwijderen](manage-network-security-group.md) als u bekend bent met het netwerkbeveiligingsgroepen en u ze wilt beheren. Zie [Een probleem met een netwerkverkeersfilter in een virtuele machine vaststellen](diagnose-network-traffic-filter-problem.md) als u communicatieproblemen hebt en problemen met netwerkbeveiligingsgroepen wilt oplossen. U kunt [logboekregistratie van stromen in netwerkbeveiligingsgroepen](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) inschakelen voor het [analyseren van netwerkverkeer](../network-watcher/traffic-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) naar en van resources waaraan een netwerkbeveiligingsgroep is gekoppeld.

## <a name="security-rules"></a>Beveiligingsregels

Een netwerkbeveiligingsgroep bevat nul regels of zoveel regels als u wilt binnen de [limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) van uw Azure-abonnement. Elke regel geeft de volgende eigenschappen aan:

|Eigenschap  |Uitleg  |
|---------|---------|
|Name|Een unieke naam binnen de netwerkbeveiligingsgroep.|
|Prioriteit | Een getal tussen 100 en 4096. Regels worden verwerkt in volgorde van prioriteit, waarbij lagere getallen worden verwerkt vóór hogere getallen omdat lagere getallen een hogere prioriteit hebben. Zodra het verkeer overeenkomt met een regel, wordt de verwerking beëindigd. Daardoor worden regels met een lagere prioriteit (een hoger getal) die dezelfde kenmerken hebben als regels met een hogere prioriteit, niet verwerkt.|
|Bron of doel| Een IP-adres, CIDR-blok (bijvoorbeeld 10.0.0.0/24), [servicetag](#service-tags) of [toepassingsbeveiligingsgroep](#application-security-groups). Als u een adres opgeeft voor een Azure-resource, geeft u het privé-IP-adres op dat aan de resource is toegewezen. Netwerkbeveiligingsgroepen worden verwerkt nadat Azure een openbaar IP-adres vertaalt naar een privé-IP-adres voor binnenkomend verkeer en voordat Azure een privé-IP-adres naar een openbaar IP-adres voor uitgaand verkeer vertaalt. Meer informatie over de [IP-adressen](virtual-network-ip-addresses-overview-arm.md) van Azure. Als u een bereik, servicetag of toepassingsbeveiligingsgroep opgeeft, hoeft u minder beveiligingsregels te maken. De mogelijkheid om in een regel meerdere afzonderlijke IP-adressen en -bereiken op te geven (u kunt niet meerdere servicetags of toepassingsgroepen opgeven) wordt aangeduid met de term [uitgebreide beveiligingsregels](#augmented-security-rules). Uitgebreide beveiligingsregels kunnen alleen worden gemaakt in netwerkbeveiligingsgroepen die zijn gemaakt via het Resource Manager-implementatiemodel. U kunt niet meerdere IP-adressen en IP-adresbereiken opgeven in netwerkbeveiligingsgroepen die zijn gemaakt via het klassieke implementatiemodel. Meer informatie over [implementatiemodellen van Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|Protocol     | TCP, UDP of alle, waaronder (maar niet beperkt tot) TCP, UDP en ICMP. U kunt niet alleen ICMP opgeven, dus als u ICMP vereist, gebruikt u Alle. |
|Direction| Hiermee wordt aangegeven of de regel van toepassing is op binnenkomend of uitgaand verkeer.|
|Poortbereik     |U kunt één poort of een poortbereik opgeven. U kunt bijvoorbeeld 80 of 10000-10005 opgeven. Als u bereiken opgeeft, hoeft u minder beveiligingsregels te maken. Uitgebreide beveiligingsregels kunnen alleen worden gemaakt in netwerkbeveiligingsgroepen die zijn gemaakt via het Resource Manager-implementatiemodel. U kunt niet meerdere poorten of poortbereiken opgeven in dezelfde beveiligingsregel in netwerkbeveiligingsgroepen die zijn gemaakt via het klassieke implementatiemodel.   |
|Action     | Toestaan of weigeren        |

Beveiligingsregels voor netwerkbeveiligingsgroepen worden op prioriteit geëvalueerd op basis van de 5 tuple-gegevens (bron, bronpoort, doel, doelpoort en protocol) voor het toestaan of weigeren van het verkeer. Voor bestaande verbindingen wordt een stroomrecord gemaakt. Communicatie wordt toegestaan of geweigerd op basis van de verbindingsstatus van de stroomrecord. Met de stroomrecord wordt een netwerkbeveiligingsgroep toegestaan stateful te zijn. Als u bijvoorbeeld een beveiligingsregel voor uitgaand verkeer opgeeft voor elk adres via poort 80, hoeft u geen beveiligingsregel voor binnenkomend verkeer op te geven voor de reacties op het uitgaande verkeer. U hoeft alleen een beveiligingsregel voor binnenkomend verkeer op te geven als de communicatie extern is gestart. Het omgekeerde geldt ook. Als binnenkomend verkeer via een poort is toegestaan, is het niet nodig om een beveiligingsregel voor uitgaand verkeer op te geven om te reageren op verkeer via die poort.
Bestaande verbindingen kunnen niet worden onderbroken wanneer u een beveiligingsregel verwijdert die de stroom mogelijk heeft gemaakt. Verkeersstromen worden onderbroken wanneer er verbindingen worden gestopt en er gedurende minstens een paar minuten in beide richtingen geen verkeer stroomt.

Er gelden beperkingen voor het aantal beveiligingsregels dat u in een netwerkbeveiligingsgroep kunt maken. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

## <a name="augmented-security-rules"></a>Uitgebreide beveiligingsregels

Uitgebreide beveiligingsregels vereenvoudigen de beveiligingsdefinitie voor virtuele netwerken, zodat u een uitgebreider en complexer netwerkbeveiligingsbeleid kunt definiëren met minder regels. U kunt meerdere poorten en meerdere expliciete IP-adressen en -bereiken combineren tot één gemakkelijk te begrijpen beveiligingsregel. U kunt uitgebreide regels gebruiken in de velden voor bron, doel en poort van een regel. Om het onderhoud van de definitie van uw beveiligingsregel te vereenvoudigen, kunt u uitgebreide beveiligingsregels combineren met [servicetags](#service-tags) of [toepassingsbeveiligingsgroepen](#application-security-groups). Er gelden beperkingen voor het aantal adressen, bereiken en poorten die u in een regel opgeven kunt. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

## <a name="service-tags"></a>Servicetags

Een servicetag vertegenwoordigt een groep IP-adresvoorvoegsels die het maken van beveiligingsregel vereenvoudigt. U kunt niet uw eigen servicetag maken en ook niet opgeven welke IP-adressen zijn opgenomen in een tag. Microsoft beheert de adresvoorvoegsels die de servicetag omvat en werkt de servicetag automatisch bij wanneer adressen veranderen. U kunt servicetags gebruiken in plaats van specifieke IP-adressen wanneer u beveiligingsregels maakt. 

De volgende servicetags zijn beschikbaar voor gebruik in [regels voor netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules). Servicetags met sterretje aan het einde (dat wil zeggen AzureCloud *) kunnen ook worden gebruikt [netwerk van Azure firewallregels](https://docs.microsoft.com/azure/firewall/service-tags). 

* **VirtualNetwork** (Resource Manager) (**VIRTUAL_NETWORK** voor de klassieke versie): Deze tag omvat de adresruimte van het virtuele netwerk (alle CIDR-bereiken die voor het virtuele netwerk zijn gedefinieerd), alle verbonden on-premises adresruimten, [gekoppeld](virtual-network-peering-overview.md) virtuele netwerken of virtuele netwerken die zijn verbonden met een [virtuele netwerkgateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en los deze voorvoegsels op [gebruiker gedefinieerde routes](virtual-networks-udr-overview.md). Let erop dat deze tag standaardroute kan bevatten. 
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** voor de klassieke versie): Met deze tag wordt de load balancer voor de infrastructuur van Azure aangeduid. De tag wordt omgezet in het [Virtuele IP-adres van de host](security-overview.md#azure-platform-considerations) (168.63.129.16) van waaruit statuscontroles van Azure worden uitgevoerd. Als u de load balancer van Azure niet gebruikt, kunt u deze regel onderdrukken.
* **Internet** (Resource Manager) (**INTERNET** voor de klassieke versie): Met deze tag wordt de IP-adresruimte aangeduid die zich buiten het virtuele netwerk bevindt en bereikbaar is via internet. Dit adresbereik omvat ook de [openbare IP-adresruimte van Azure](https://www.microsoft.com/download/details.aspx?id=41653).
* **AzureCloud*** (alleen Resource Manager): Met deze tag wordt de IP-adresruimte voor Azure aangeduid, inclusief alle [openbare IP-adressen van het datacenter](https://www.microsoft.com/download/details.aspx?id=41653). Als u *AzureCloud* opgeeft als waarde, wordt verkeer naar AzureCloud toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot AzureCloud in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling AzureCloud. [ regionaam]. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **AzureTrafficManager*** (alleen Resource Manager): Met deze tag wordt de IP-adresruimte voor de test-IP-adressen van Azure Traffic Manager aangeduid. Meer informatie over de test-IP-adressen van Traffic Manager vindt u in de [Veelgestelde vragen over Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs). Deze tag wordt aanbevolen voor de beveiligingsregel voor binnenkomend verkeer.  
* **Opslag*** (alleen Resource Manager): Met deze tag wordt de IP-adresruimte voor de service Azure Storage aangeduid. Als u *Storage* opgeeft als waarde, wordt verkeer naar de opslag toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot opslag in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling opslag. [ regionaam]. De tag vertegenwoordigt de service, maar geen specifieke exemplaren van de service. De tag vertegenwoordigt bijvoorbeeld de service Azure Storage, maar geen specifiek Azure Storage-account. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **SQL*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de Azure SQL Database, Azure Database for MySQL, Azure Database for PostgreSQL, en services van Azure SQL Data Warehouse. Als u *Sql* opgeeft als waarde, wordt verkeer naar Sql toegestaan of geweigerd. Als u alleen wilt toestaan toegang tot Sql in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling Sql. [regio name]. De tag vertegenwoordigt de service, maar geen specifieke exemplaren van de service. De tag vertegenwoordigt bijvoorbeeld de service Azure SQL Database, maar geen specifieke SQL-database of -server. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **AzureCosmosDB*** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de Azure Cosmos Database-service aangeduid. Als u *AzureCosmosDB* opgeeft als waarde, wordt verkeer naar AzureCosmosDB toegestaan of geweigerd. Als u alleen toegang wilt toestaan tot AzureCosmosDB in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling AzureCosmosDB.[regionaam] specificeren. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **AzureKeyVault*** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Key Vault aangeduid. Als u *AzureKeyVault* opgeeft als waarde, wordt verkeer naar AzureKeyVault toegestaan of geweigerd. Als u alleen toegang wilt toestaan tot AzureKeyVault in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling AzureKeyVault.[regionaam] specificeren. Deze tag heeft afhankelijkheid van de **AzureActiveDirectory** tag. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel.  
* **Event hub*** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Event Hub aangeduid. Als u *EventHub* opgeeft als waarde, wordt verkeer naar EventHub toegestaan of geweigerd. Als u alleen toegang wilt toestaan tot EventHub in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling EventHub.[regionaam] specificeren. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **Service Bus*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de Azure Service Bus-service met behulp van de Premium-servicelaag. Als u *ServiceBus* opgeeft als waarde, wordt verkeer naar ServiceBus toegestaan of geweigerd. Als u alleen toegang wilt toestaan tot ServiceBus in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling ServiceBus.[regionaam] specificeren. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **MicrosoftContainerRegistry*** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Microsoft-containerregister aangeduid. Als u *MicrosoftContainerRegistry* opgeeft als waarde, wordt verkeer naar MicrosoftContainerRegistry toegestaan of geweigerd. Als u alleen toegang wilt toestaan tot MicrosoftContainerRegistry in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling MicrosoftContainerRegistry.[regionaam] specificeren. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **AzureContainerRegistry*** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Container Registry aangeduid. Als u *AzureContainerRegistry* opgeeft als waarde, wordt verkeer naar AzureContainerRegistry toegestaan of geweigerd. Als u alleen toegang wilt toestaan tot AzureContainerRegistry in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling AzureContainerRegistry.[regionaam] specificeren. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **AppService*** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure App Service aangeduid. Als u *AppService* opgeeft als waarde, wordt verkeer naar AppService toegestaan of geweigerd. Als u alleen toegang wilt toestaan tot AppService in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling AppService.[regionaam] specificeren. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel voor Web-Apps-front-ends.  
* **AppServiceManagement*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van het beheer van verkeer voor implementaties van App Service Environment is toegewezen. Als u *AppServiceManagement* opgeeft als waarde, wordt verkeer naar AppServiceManagement toegestaan of geweigerd. Deze tag wordt aanbevolen voor de beveiligingsregel voor binnenkomend en uitgaand. 
* **ApiManagement*** (alleen Resource Manager): Deze tag geeft dat de adresvoorvoegsels van het beheer van verkeer voor APIM toegewezen implementaties. Als u *ApiManagement* opgeeft als waarde, wordt verkeer naar ApiManagement toegestaan of geweigerd. Deze tag wordt aanbevolen voor de beveiligingsregel voor binnenkomend en uitgaand. 
* **AzureConnectors*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de Logic Apps-connectors voor test/back-end-verbindingen. Als u *AzureConnectors* opgeeft als waarde, wordt verkeer naar AzureConnectors toegestaan of geweigerd. Als u alleen toegang wilt toestaan tot AzureConnectors in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling AzureConnectors.[regionaam] specificeren. Deze tag wordt aanbevolen voor de beveiligingsregel voor binnenkomend verkeer. 
* **GatewayManager** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van het beheer van verkeer voor VPN-/ App Gateways toegewezen implementaties. Als u *GatewayManager* opgeeft als waarde, wordt verkeer naar GatewayManager toegestaan of geweigerd. Deze tag wordt aanbevolen voor de beveiligingsregel voor binnenkomend verkeer. 
* **AzureDataLake*** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Data Lake aangeduid. Als u *AzureDataLake* opgeeft als waarde, wordt verkeer naar AzureDataLake toegestaan of geweigerd. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **AzureActiveDirectory*** (alleen Resource Manager): Met deze tag worden de adresvoorvoegsels van de service Azure Active Directory aangeduid. Als u *AzureActiveDirectory* opgeeft als waarde, wordt verkeer naar AzureActiveDirectory toegestaan of geweigerd. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel.
* **AzureMonitor*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de Log Analytics, Application Insights, AzMon en aangepaste metrische gegevens (GB-eindpunten). Als u opgeeft *AzureMonitor* voor de waarde moet verkeer is toegestaan of geweigerd voor AzureMonitor. Voor Log Analytics, heeft deze tag afhankelijkheid van de **opslag** tag. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel.
* **Service fabric*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de service fabric-service. Als u opgeeft *ServiceFabric* voor de waarde moet verkeer is toegestaan of geweigerd voor service fabric. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **AzureMachineLearning*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de service AzureMachineLearning. Als u opgeeft *AzureMachineLearning* voor de waarde moet verkeer is toegestaan of geweigerd voor AzureMachineLearning. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **BatchNodeManagement*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van het beheer van verkeer voor implementaties van Azure Batch is toegewezen. Als u opgeeft *BatchNodeManagement* voor de waarde moet verkeer is toegestaan of geweigerd vanuit de Batch-service om knooppunten te berekenen. Deze tag wordt aanbevolen voor de beveiligingsregel voor binnenkomend en uitgaand. 
* **Met AzureBackup*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de AzureBackup-service. Als u opgeeft *AzureBackup* voor de waarde moet verkeer is toegestaan of geweigerd voor Backup van Azure. Deze tag heeft afhankelijkheid van de **opslag** en **AzureActiveDirectory** tag. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel. 
* **AzureActiveDirectoryDomainServices*** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van het beheer van verkeer voor specifieke Azure Active Directory Domain Services-implementaties. Als u opgeeft *AzureActiveDirectoryDomainServices* voor de waarde moet verkeer is toegestaan of geweigerd voor AzureActiveDirectoryDomainServices. Deze tag wordt aanbevolen voor de beveiligingsregel voor binnenkomend en uitgaand.  
* **SqlManagement*** (alleen Resource Manager): Deze tag geeft dat de adresvoorvoegsels van het beheer van verkeer voor SQL toegewezen implementaties. Als u opgeeft *SqlManagement* voor de waarde moet verkeer is toegestaan of geweigerd voor SqlManagement. Deze tag wordt aanbevolen voor de beveiligingsregel voor binnenkomend en uitgaand. 
* **CognitiveServicesManagement** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van verkeer voor Cognitive Services. Als u opgeeft *CognitiveServicesManagement* voor de waarde moet verkeer is toegestaan of geweigerd voor CognitiveServicesManagement. Deze tag wordt aanbevolen voor uitgaande beveiligingsregel.  
* **Dynamics365ForMarketingEmail** (alleen Resource Manager): Deze tag geeft de adresvoorvoegsels van de marketing-e-mailservice van Dynamics 365. Als u opgeeft *Dynamics365ForMarketingEmail* voor de waarde moet verkeer is toegestaan of geweigerd voor Dynamics365ForMarketingEmail. Als u alleen wilt toestaan toegang tot Dynamics365ForMarketingEmail in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio in de volgende indeling Dynamics365ForMarketingEmail. [ regionaam].
* **AzurePlatformDNS** (alleen Resource Manager): Deze tag wordt de DNS is een basisinfrastructuur-service. Als u opgeeft *AzurePlatformDNS* waarde, kunt u de standaard uitschakelen [Azure-platform overweging](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) voor DNS. Neem voorzichtig bij het gebruik van deze tag. Testen wordt voordat u deze tag aanbevolen. 
* **AzurePlatformIMDS** (alleen Resource Manager): Deze tag geeft IMDS dit een basisinfrastructuur-service is. Als u opgeeft *AzurePlatformIMDS* waarde, kunt u de standaard uitschakelen [Azure-platform overweging](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) voor IMDS. Neem voorzichtig bij het gebruik van deze tag. Testen wordt voordat u deze tag aanbevolen. 
* **AzurePlatformLKM** (alleen Resource Manager): Deze tag wordt de Windows-licentieverlening of service voor sleutelbeheer. Als u opgeeft *AzurePlatformLKM* waarde, kunt u de standaard uitschakelen [Azure-platform overweging](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) voor licentieverlening. Neem voorzichtig bij het gebruik van deze tag. Testen wordt voordat u deze tag aanbevolen. 

> [!NOTE]
> Met servicetags van Azure-services worden de adresvoorvoegsels aangeduid van de specifieke cloud die wordt gebruikt. 

> [!NOTE]
> Als u een [service-eindpunt voor een virtueel netwerk](virtual-network-service-endpoints-overview.md) voor een service als Azure Storage of Azure SQL Database implementeert, wordt een [route](virtual-networks-udr-overview.md#optional-default-routes) naar een virtueel netwerksubnet voor de service toegevoegd. De adresvoorvoegsels in de route zijn dezelfde adresvoorvoegsels (of CIDR-bereiken) als de bijbehorende servicetag.

### <a name="service-tags-in-on-premises"></a>Servicetags on-premises  
U kunt downloaden en een on-premises firewall integreren in de lijst met servicetags met voorvoegsel informatie over de publicaties van de volgende wekelijkse voor Azure [openbare](https://www.microsoft.com/download/details.aspx?id=56519), [Amerikaanse overheid](https://www.microsoft.com/download/details.aspx?id=57063), [China ](https://www.microsoft.com/download/details.aspx?id=57062), en [Duitsland](https://www.microsoft.com/download/details.aspx?id=57064) clouds.

U kunt ook programmatisch ophalen voor deze informatie met behulp van de **API-Service Tag detectie** (openbare Preview) - [REST](https://aka.ms/discoveryapi_rest), [Azure PowerShell](https://aka.ms/discoveryapi_powershell), en [ Azure CLI](https://aka.ms/discoveryapi_cli). 

> [!NOTE]
> Volgende week publicaties (oude versie) voor Azure [openbare](https://www.microsoft.com/en-us/download/details.aspx?id=41653), [China](https://www.microsoft.com/en-us/download/details.aspx?id=42064), en [Duitsland](https://www.microsoft.com/en-us/download/details.aspx?id=54770) clouds met 30 juni 2020 worden afgeschaft. Start met behulp van de bijgewerkte publicaties, zoals hierboven beschreven. 

## <a name="default-security-rules"></a>Standaardbeveiligingsregels

Azure maakt de volgende standaardregels in elke netwerkbeveiligingsgroep die u maakt:

### <a name="inbound"></a>Inkomend

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Prioriteit|Source|Bronpoorten|Bestemming|Doelpoorten|Protocol|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Alle|Allow|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Prioriteit|Source|Bronpoorten|Bestemming|Doelpoorten|Protocol|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Alle|Allow|

#### <a name="denyallinbound"></a>DenyAllInbound

|Prioriteit|Source|Bronpoorten|Bestemming|Doelpoorten|Protocol|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Alle|Weigeren|

### <a name="outbound"></a>Uitgaande

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Prioriteit|Source|Bronpoorten| Bestemming | Doelpoorten | Protocol | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Alle | Allow |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Prioriteit|Source|Bronpoorten| Bestemming | Doelpoorten | Protocol | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Alle | Allow |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Prioriteit|Source|Bronpoorten| Bestemming | Doelpoorten | Protocol | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Alle | Weigeren |

In de kolommen **Bron** en **Doel** zijn *VirtualNetwork*, *AzureLoadBalancer* en *Internet* [servicetags](#service-tags) in plaats van IP-adressen. In de kolom protocol omvat **Alle** TCP, UDP en ICMP. Wanneer u een regel maakt, kunt u TCP, UDP of Alle opgeven, maar u kunt niet alleen ICMP opgeven. Als uw regel ICMP vereist, selecteert u daarom *Alle* voor protocol. *0.0.0.0/0* in de kolommen **Bron** en **Doel** vertegenwoordigt alle adressen. Clients zoals Azure portal, Azure CLI of Powershell kunt gebruiken * of een voor deze expressie.
 
U kunt de standaardregels niet verwijderen, maar u kunt ze negeren door regels te maken met een hogere prioriteit.

## <a name="application-security-groups"></a>Toepassingsbeveiligingsgroepen

Met behulp van toepassingsbeveiligingsgroepen kunt u netwerkbeveiliging configureren als een natuurlijk verlengstuk van de structuur van een toepassing, waarbij u virtuele machines kunt groeperen en netwerkbeveiligingsbeleid kunt definiëren op basis van die groepen. U kunt het beveiligingsbeleid op grote schaal opnieuw gebruiken zonder handmatig onderhoud van expliciete IP-adressen. Het platform verwerkt de complexiteit van expliciete IP-adressen en meerdere regelsets, zodat u zich kunt richten op uw bedrijfslogica. Kijk eens naar de volgende voorbeelden voor een beter begrip van toepassingsbeveiligingsgroepen:

![Toepassingsbeveiligingsgroepen](./media/security-groups/application-security-groups.png)

In de vorige afbeelding zijn *NIC1* en *NIC2* leden van de toepassingsbeveiligingsgroep *AsgWeb*. *NIC3* is een lid van de toepassingsbeveiligingsgroep *AsgLogic*. *NIC4* is een lid van de toepassingsbeveiligingsgroep *AsgDb*. Hoewel elke netwerkinterface in dit voorbeeld een lid is van slechts één toepassingsbeveiligingsgroep, kan een netwerkinterface lid zijn van meerdere toepassingsbeveiligingsgroepen, maar niet meer dan de opgegeven [Azure-limiet](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Aan geen van de netwerkinterfaces is een netwerkbeveiligingsgroep gekoppeld. *NSG1* is gekoppeld aan beide subnetten en bevat de volgende regels:

### <a name="allow-http-inbound-internet"></a>Allow-HTTP-Inbound-Internet

Deze regel is vereist om verkeer van internet naar de webservers te laten lopen. Binnenkomend verkeer van internet wordt geweigerd door de standaardbeveiligingsregel [DenyAllInbound](#denyallinbound). Daarom is er geen extra regel nodig voor de toepassingsbeveiligingsgroepen *AsgLogic* of *AsgDb*.

|Prioriteit|Source|Bronpoorten| Bestemming | Doelpoorten | Protocol | Access |
|---|---|---|---|---|---|---|
| 100 | Internet | * | AsgWeb | 80 | TCP | Allow |

### <a name="deny-database-all"></a>Deny-Database-All

De standaardbeveiligingsregel [AllowVNetInBound](#allowvnetinbound) staat communicatie toe tussen resources in hetzelfde virtuele netwerk. Daarom is deze regel vereist voor het weigeren van verkeer dat van een willekeurige resource afkomstig is.

|Prioriteit|Source|Bronpoorten| Bestemming | Doelpoorten | Protocol | Access |
|---|---|---|---|---|---|---|
| 120 | * | * | AsgDb | 1433 | Alle | Weigeren |

### <a name="allow-database-businesslogic"></a>Allow-Database-BusinessLogic

Deze regel staat verkeer toe van de toepassingsbeveiligingsgroep *AsgLogic* naar de toepassingsbeveiligingsgroep *AsgDb*. De prioriteit voor deze regel is hoger dan de prioriteit voor de regel *Deny-Database-All*. Als gevolg hiervan wordt deze regel verwerkt vóór de regel *Deny-Database-All*, zodat verkeer van de toepassingsbeveiligingsgroep *AsgLogic* wordt toegestaan, terwijl al het andere verkeer wordt geblokkeerd.

|Prioriteit|Source|Bronpoorten| Bestemming | Doelpoorten | Protocol | Access |
|---|---|---|---|---|---|---|
| 110 | AsgLogic | * | AsgDb | 1433 | TCP | Allow |

De regels die een toepassingsbeveiligingsgroep als bron of doel opgeven, worden alleen toegepast op netwerkinterfaces die lid van de toepassingsbeveiligingsgroep. Als de netwerkinterface geen lid is van een toepassingsbeveiligingsgroep, wordt de regel niet toegepast op de netwerkinterface, ook niet als de netwerkbeveiligingsgroep aan het subnet is gekoppeld.

Toepassingsbeveiligingsgroepen hebben de volgende beperkingen:

-   Er gelden beperkingen voor het aantal toepassingsbeveiligingsgroepen dat u met een abonnement kunt hebben. Er gelden ook andere beperkingen met betrekking tot toepassingsbeveiligingsgroepen. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.
- U kunt in een beveiligingsregel een en dezelfde toepassingsbeveiligingsgroep opgeven als de bron en het doel. Het is niet mogelijk om meerdere toepassingsbeveiligingsgroepen in de bron of het doel op te geven.
- Alle netwerkinterfaces die zijn toegewezen aan een toepassingsbeveiligingsgroep, moeten zich in hetzelfde virtuele netwerk bevinden als de eerste netwerkinterface die aan de toepassingsbeveiligingsgroep is toegewezen. Bijvoorbeeld, als de eerste netwerkinterface die aan een toepassingsbeveiligingsgroep met de naam *AsgWeb* is toegewezen, zich in het virtuele netwerk met de naam *VNet1* bevindt, moeten alle volgende netwerkinterfaces die zijn toegewezen aan *AsgWeb*, aanwezig zijn in *VNet1*. U kunt geen netwerkinterfaces van verschillende virtuele netwerken toevoegen aan dezelfde toepassingsbeveiligingsgroep.
- Als u in een beveiligingsregel een toepassingsbeveiligingsgroep als bron en doel opgeeft, moeten de netwerkinterfaces in beide toepassingsbeveiligingsgroepen zich in hetzelfde virtuele netwerk bevinden. Als *AsgLogic* bijvoorbeeld netwerkinterfaces van *VNet1* bevat en *AsgDb* netwerkinterfaces van *VNet2*, is het niet mogelijk om in een regel *AsgLogic* toe te wijzen als bron en *AsgDb* als doel. Alle netwerkinterfaces voor zowel de bron- als de doeltoepassingsbeveiligingsgroepen moeten aanwezig zijn in hetzelfde virtuele netwerk.

> [!TIP]
> Als u het gewenste aantal beveiligingsregels wilt verminderen en tevens het wijzigen van de regels wilt beperken, plant u de toepassingsbeveiligingsgroepen die u nodig hebt en maakt u, indien mogelijk, regels met behulp van servicetags of toepassingsbeveiligingsgroepen in plaats van afzonderlijke IP-adressen of -bereiken.

## <a name="how-traffic-is-evaluated"></a>Verkeer evalueren

U kunt resources van diverse Azure-services in een virtueel Azure-netwerk implementeren. Zie [Services die kunnen worden geïmplementeerd in een virtueel netwerk](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network) voor de volledige lijst. U kunt nul of één netwerkbeveiligingsgroep aan elk [subnet](virtual-network-manage-subnet.md#change-subnet-settings) van een virtueel netwerk en [netwerkinterface](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group) in een virtuele machine koppelen. Dezelfde netwerkbeveiligingsgroep kan aan zoveel subnetten en netwerkinterfaces worden gekoppeld als u wilt.

In de volgende afbeelding ziet u verschillende scenario's voor het implementeren van netwerkbeveiligingsgroepen om netwerkverkeer naar en van internet via TCP-poort 80 toe te staan:

![Verwerken van NSG's](./media/security-groups/nsg-interaction.png)

Raadpleeg de afbeelding hierboven in combinatie met de volgende tekst als u wilt weten hoe binnenkomende en uitgaande regels voor netwerkbeveiligingsgroepen worden verwerkt:

### <a name="inbound-traffic"></a>Binnenkomend verkeer

Voor binnenkomend verkeer worden eerst de regels verwerkt in een netwerkbeveiligingsgroep die aan een subnet is gekoppeld, indien aanwezig. Vervolgens worden de regels verwerkt die zijn gekoppeld aan een netwerkinterface, indien aanwezig.

- **VM1**: De beveiligingsregels in *NSG1* worden verwerkt, omdat deze is gekoppeld aan *Subnet1* en *VM1* zich in *Subnet1* bevindt. Tenzij u een regel hebt gemaakt waardoor poort 80 binnenkomend verkeer toestaat, wordt het verkeer geweigerd door de standaardbeveiligingsregel [DenyAllInbound](#denyallinbound) en nooit geëvalueerd door *NSG2*, omdat *NSG2* is gekoppeld aan de netwerkinterface. Als *NSG1* een beveiligingsregel bevat die poort 80 toestaat, wordt het verkeer door *NSG2* verwerkt. Als u poort 80 voor de virtuele machine wilt toestaan, moet zowel *NSG1* als *NSG2* een regel bevatten die verkeer vanaf internet via poort 80 toestaat.
- **VM2**: De regels in *NSG1* worden verwerkt omdat *VM2* zich ook in *Subnet1* bevindt. Omdat *VM2* geen netwerkinterface bevat die aan de bijbehorende netwerkinterface is gekoppeld, wordt hier alle verkeer ontvangen dat is toegestaan via *NSG1* of wordt alle verkeer geweigerd dat is geweigerd door *NSG1*. Verkeer wordt voor alle resources in hetzelfde subnet toegestaan of geweigerd als een netwerkbeveiligingsgroep aan een subnet is gekoppeld.
- **VM3**: Omdat er geen netwerkbeveiligingsgroep is gekoppeld aan *Subnet2*, wordt verkeer naar het subnet toegestaan en verwerkt door *NSG2*, omdat *NSG2* is gekoppeld aan de netwerkinterface die met *VM3* is verbonden.
- **VM4**: Verkeer naar *VM4* wordt toegestaan, omdat er geen netwerkbeveiligingsgroep is gekoppeld aan *Subnet3* of de netwerkinterface in de virtuele machine. Alle netwerkverkeer wordt toegestaan via een subnet en netwerkinterface als er geen netwerkbeveiligingsgroep aan is gekoppeld.

### <a name="outbound-traffic"></a>Uitgaand verkeer

Voor uitgaand verkeer worden eerst de regels verwerkt in een netwerkbeveiligingsgroep die aan een netwerkinterface is gekoppeld, indien aanwezig. Vervolgens worden de regels in een netwerkbeveiligingsgroep verwerkt die zijn gekoppeld aan het subnet, indien aanwezig.

- **VM1**: De beveiligingsregels in *NSG2* worden verwerkt. Tenzij u een beveiligingsregel maakt die uitgaand verkeer naar internet via poort 80 weigert, wordt in zowel *NSG1* als *NSG2* verkeer toegestaan door de standaardbeveiligingsregel [AllowInternetOutbound](#allowinternetoutbound). Als *NSG2* een beveiligingsregel bevat die poort 80 weigert, wordt het verkeer geweigerd en nooit door *NSG1* geëvalueerd. Als u binnenkomend verkeer van de virtuele machine via poort 80 wilt weigeren, moet een van de netwerkbeveiligingsgroepen, of beide, een regel bevatten die uitgaand verkeer naar internet via poort 80 weigert.
- **VM2**: Al het verkeer wordt via de netwerkinterface naar het subnet verzonden, omdat aan de netwerkinterface die met *VM2* is verbonden, geen netwerkbeveiligingsgroep is gekoppeld. De regels in *NSG1* worden verwerkt.
- **VM3**: Als *NSG2* een beveiligingsregel bevat die poort 80 weigert, wordt het verkeer geweigerd. Als *NSG2* een beveiligingsregel bevat die poort 80 toestaat, wordt via poort 80 uitgaand verkeer naar internet toegestaan, omdat er geen netwerkbeveiligingsgroep aan *Subnet2* is gekoppeld.
- **VM4**: Al het netwerkverkeer van *VM4* wordt toegestaan, omdat er geen netwerkbeveiligingsgroep is gekoppeld aan de netwerkinterface die met de virtuele machine is verbonden, of aan *Subnet3*.

U kunt eenvoudig de cumulatieve regels bekijken die op een netwerkinterface worden toegepast door de [effectieve beveiligingsregels](virtual-network-network-interface.md#view-effective-security-rules) voor een netwerkinterface te bekijken. U kunt ook de functie [IP-stroom controleren](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) in Azure Network Watcher gebruiken om te bepalen of de communicatie van of naar een netwerkinterface is toegestaan. IP-stroom controleren geeft aan of communicatie is toegestaan en welke netwerkbeveiligingsregel verkeer toestaat of weigert.

> [!NOTE]
> Netwerkbeveiligingsgroepen worden gekoppeld aan subnetten of virtuele machines en cloudservices die zijn geïmplementeerd in het klassieke implementatiemodel en aan subnetten of netwerkinterfaces in het Resource Manager-implementatiemodel. Zie [Over Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over Azure-implementatiemodellen.

> [!TIP]
> Het wordt aangeraden een netwerkbeveiligingsgroep aan hetzij een subnet, hetzij een netwerkinterface te koppelen, tenzij u een bepaalde reden hebt om ze aan beide te koppelen. Aangezien regels in een netwerkbeveiligingsgroep die aan een subnet is gekoppeld, kunnen conflicteren met regels in een netwerkbeveiligingsgroep die aan een netwerkinterface is gekoppeld, kunnen er onverwachte communicatieproblemen optreden waarvoor een probleemoplossing dient te worden uitgevoerd.

## <a name="azure-platform-considerations"></a>Overwegingen bij het Azure-platform

- **Virtueel IP-adres van het hostknooppunt**: Basisinfrastructuurservices zoals DHCP, DNS, IMDS en statuscontrole worden geleverd via de gevirtualiseerde host-IP-adressen 168.63.129.16 en 169.254.169.254. Deze IP-adressen zijn van Microsoft en zijn de enige gevirtualiseerde IP-adressen die in alle regio's voor dit doel worden gebruikt.
- **Licentieverlening (Key Management Service)** : Voor alle Windows installatiekopieën die op virtuele machines worden uitgevoerd, is een licentie vereist. Hiervoor wordt een licentieaanvraag verstuurd naar de Key Management Service-hostservers waarop dergelijke query's worden afgehandeld. De uitgaande aanvraag wordt gedaan via poort. 1688. Voor implementaties die gebruikmaken van een configuratie met de [standaardroute 0.0.0.0/0](virtual-networks-udr-overview.md#default-route), wordt deze platformregel uitgeschakeld.
- **Virtuele machines in groepen met gelijke taakverdeling**: De bronpoort en het bronadresbereik die worden toegepast, zijn die van de oorspronkelijke computer, niet van de load balancer. De doelpoort en het doeladresbereik zijn die van de doelcomputer, niet van de load balancer.
- **Azure-service-exemplaren**: Exemplaren van verschillende Azure-services, zoals HDInsight, toepassingsserviceomgevingen en Virtual Machine Scale Sets, worden geïmplementeerd in virtuele netwerksubnetten. Zie [Virtueel netwerk voor Azure-services](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network) voor een volledige lijst met services die u in virtuele netwerken kunt implementeren. Zorg ervoor dat u vertrouwd raakt met de poortvereisten voor elke service voordat u een netwerkbeveiligingsgroep toepast op het subnet waarin de resource is geïmplementeerd. Als u poorten weigert die vereist zijn voor de service, werkt de service niet naar behoren.
- **Uitgaande e-mail verzenden**: Microsoft raadt u aan om geverifieerde SMTP-relayservices te gebruiken (doorgaans verbonden via TCP-poort 587, maar ook vaak andere) om e-mail vanaf Azure Virtual Machines te verzenden. SMTP-relayservices leggen zich toe op de reputatie van de afzender, om zo de kans dat e-mailproviders van derden berichten weigeren, tot het uiterste terug te dringen. Dergelijke SMTP-relayservices omvatten, maar beperken zich niet tot Exchange Online Protection en SendGrid. Het gebruik van de SMTP-relayservices wordt in Azure op geen enkele wijze beperkt, ongeacht welk type abonnement u hebt. 

  Als u uw Azure-abonnement vóór 15 november 2017 hebt gemaakt, kunt u naast de mogelijkheid om gebruik te maken van SMTP-relayservices ook rechtstreeks e-mail verzenden via TCP-poort 25. Als u uw abonnement na 15 november 2017 hebt gemaakt kunt u mogelijk geen e-mail rechtstreeks via poort 25 verzenden. Hoe uitgaande communicatie via poort 25 verloopt, hangt als volgt samen met het type abonnement dat u hebt:

     - **Enterprise Overeenkomst**: Uitgaande communicatie via poort 25 is toegestaan. U kunt uitgaande e-mail rechtstreeks vanaf virtuele machines naar externe e-mailproviders verzenden, zonder dat daar beperkingen voor gelden op grond van het Azure-platform. 
     - **Betalen per gebruik**: Uitgaande communicatie via poort 25 is voor alle resources geblokkeerd. Als u e-mail vanaf een virtuele machine rechtstreeks naar de externe e-mailproviders wilt verzenden (niet via een geverifieerde SMTP-relay), kunt u een aanvraag indienen om de beperking op te heffen. Het is aan Microsoft om te bepalen of aanvragen worden gecontroleerd en goedgekeurd, en aanvragen worden alleen toegekend nadat er controles ter voorkoming van fraude zijn uitgevoerd. Als u een aanvraag wilt indienen, opent u een ondersteuningsaanvraag met het probleemtype *Technisch*, *Virtuele netwerkverbinding*, *Kan geen e-mail verzenden (SMTP/poort 25)* . Vermeld in uw ondersteuningsaanvraag informatie zoals waarom uw abonnement e-mail rechtstreeks naar e-mailproviders moet kunnen verzenden in plaats van via een geverifieerde SMTP-relay. Als de aanvraag voor uw abonnement wordt toegekend, kunnen alleen de virtuele machines die na de toekenningsdatum zijn gemaakt poort 25 voor uitgaande communicatie gebruiken.
     - **MSDN, Azure Pass, Azure in Open, Education, BizSpark en gratis proefversie**: Uitgaande communicatie via poort 25 is voor alle resources geblokkeerd. Er kunnen geen aanvragen worden ingediend voor het opheffen van de beperking, omdat zulke aanvragen niet worden toegekend. Als u e-mail moet verzenden vanaf uw virtuele machine, dient u een SMTP-relayservice te gebruiken.
     - **Cloudserviceprovider**: Klanten die Azure-resources via een cloudserviceprovider gebruiken, kunnen een ondersteuningsaanvraag maken bij hun cloudserviceprovider. Zij kunnen vervolgens aanvragen dat de provider namens hen een blokkering opheft, als er geen beveiligde SMTP-relay kan worden gebruikt.

  Als u in Azure e-mail kunt verzenden via poort 25, garandeert Microsoft niet dat e-mailproviders inkomende e-mail vanaf uw virtuele machine accepteren. Als een specifieke provider e-mail van uw virtuele machine weigert, dient u samen met de provider alle eventuele problemen met de aflevering van berichten of met de spamfilter op te lossen, of u moet een geverifieerde SMTP-relayservice gebruiken.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Een netwerkbeveiligingsgroep maken](tutorial-filter-network-traffic.md).
