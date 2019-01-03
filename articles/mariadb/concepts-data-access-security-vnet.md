---
title: Azure Database voor MariaDB Server VNet eindpunt Serviceoverzicht | Microsoft Docs
description: Beschrijving van de werking van VNet-service-eindpunten voor uw Azure Database voor MariaDB-server.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/19/2018
ms.openlocfilehash: 82b72b45f9577c83e2c6619220b26e26d696c4f1
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540237"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-database-for-mariadb"></a>Gebruik Virtual Network-service-eindpunten en regels voor Azure Database voor MariaDB

> [!IMPORTANT]
> De VNet-service-eindpunten-functie is beschikbaar als openbare preview.

*Regels voor virtueel netwerk* één firewall beveiligingsfunctie die bepaalt of uw Azure Database voor MariaDB-server accepteert berichten die worden verzonden op basis van specifieke subnetten in virtuele netwerken zijn. In dit artikel wordt uitgelegd waarom de functie van de regel virtueel netwerk is het soms de beste optie voor het veilig toestaan van communicatie met uw Azure Database voor MariaDB-server.

Het maken van een regel voor virtuele netwerken, er moet eerst worden een [virtueel netwerk] [ vm-virtual-network-overview] (VNet) en een [service-eindpunt voor virtueel netwerk] [ vm-virtual-network-service-endpoints-overview-649d] voor de regel-verwijzing. De volgende afbeelding ziet u hoe een service-eindpunt voor Virtueelnetwerk werkt met Azure Database voor MariaDB:

![Voorbeeld van de werking van een VNet-Service-eindpunt](media/concepts-data-access-security-vnet/vnet-concept.png)

> [!NOTE]
> Deze functie is beschikbaar in alle regio's van Azure waarop Azure Database voor MariaDB wordt geïmplementeerd voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.

<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologie en beschrijving

**Virtueel netwerk:** U kunt virtuele netwerken die zijn gekoppeld aan uw Azure-abonnement hebben.

**Subnet:** Een virtueel netwerk bevat **subnetten**. Een Azure virtual machines (VM's) waarvoor u zijn toegewezen aan subnetten. Één subnet kan meerdere virtuele machines of andere compute-knooppunten bevatten. COMPUTE-knooppunten die zich buiten het virtuele netwerk heeft geen toegang tot uw virtuele netwerk, tenzij u de beveiliging voor toegang configureren.

**Virtual Network service-eindpunt:** Een [service-eindpunt voor Virtueelnetwerk] [ vm-virtual-network-service-endpoints-overview-649d] is een subnet met eigenschappen die een of meer formele Azure-service typenamen bevatten. In dit artikel zijn we geïnteresseerd in de typenaam van **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database. Deze servicetag geldt ook voor de Azure Database voor MariaDB, MySQL en PostgreSQL-services. Het is belangrijk te weten bij het toepassen van de **Microsoft.Sql** servicetag naar een VNet-service-eindpunt wordt het verkeer van de service-eindpunt configureren voor alle Azure SQL Database, Azure Database voor MariaDB, Azure Database voor MySQL en Azure De database voor PostgreSQL-servers op het subnet.

**Regel voor virtuele netwerken:** Een regel voor virtuele netwerken voor uw Azure Database voor MariaDB-server is een subnet dat wordt weergegeven in de toegangsbeheerlijst (ACL) van uw Azure Database voor MariaDB-server. Als u in de ACL voor uw Azure Database voor MariaDB-server, het subnet moet bevatten de **Microsoft.Sql** typenaam.

Een regel voor virtuele netwerken wordt aan uw Azure-Database voor MariaDB-server voor het accepteren van communicatie van elk knooppunt dat zich in het subnet.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Voordelen van een regel voor virtuele netwerken

Totdat u actie onderneemt, wordt de virtuele machines op de subnetten van het niet kunnen communiceren met uw Azure Database voor MariaDB-server. Een actie die de communicatie is het maken van een regel voor virtuele netwerken. De logica voor het kiezen van de aanpak van de VNet-regel wordt een vergelijken en contrast bespreking met betrekking tot de concurrerende beveiligingsopties die worden aangeboden door de firewall.

### <a name="a-allow-access-to-azure-services"></a>A. Toegang tot Azure-services toestaan

Het deelvenster verbinding beveiliging heeft een **aan/uit** knop die is aangeduid als **toegang tot Azure-services toestaan**. De **ON** instelling kan communicatie van alle Azure-IP-adressen en subnetten voor alle Azure. Deze Azure-IP-adressen of subnetten worden niet beheerd door u. Dit **ON** instelling is waarschijnlijk meer open dan u wilt dat uw Azure Database voor MariaDB die Database. De functie van de regel virtueel netwerk maakt veel betere resultaten nauwkeurig beheer mogelijk.

### <a name="b-ip-rules"></a>B. IP-regels

De Azure Database voor MariaDB firewall kunt u IP-adresbereiken waaruit berichten worden geaccepteerd in de Azure Database voor MariaDB-server opgeven. Deze aanpak is prima voor stabiele IP-adressen die zich buiten het particuliere netwerk van Azure. Maar veel knooppunten in het particuliere netwerk van Azure zijn geconfigureerd met *dynamische* IP-adressen. Dynamische IP-adressen kunnen wijzigen, zoals wanneer uw virtuele machine opnieuw wordt opgestart. Het zou fraai om op te geven van een dynamisch IP-adres in een firewallregel in een productieomgeving zijn.

U kunt de IP-optie restwaarde door het verkrijgen van een *statische* IP-adres voor uw virtuele machine. Zie voor meer informatie, [privé IP-adressen voor een virtuele machine configureren met behulp van de Azure-portal][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Echter, de statische IP-aanpak kan het lastig om te beheren, en is kostbare wanneer u klaar bent op schaal. Virtual network-regels zijn gemakkelijker te maken en te beheren.

### <a name="c-cannot-yet-have-azure-database-for-mariadb-on-a-subnet-without-defining-a-service-endpoint"></a>C. Nog geen Azure Database voor MariaDB op een subnet zonder het definiëren van een service-eindpunt

Als uw **Microsoft.Sql** server is een knooppunt in een subnet van het virtuele netwerk, alle knooppunten in het virtuele netwerk kunnen communiceren met uw Azure Database voor MariaDB-server. In dit geval kunnen uw virtuele machines communiceren met Azure Database voor MariaDB zonder alle regels voor virtueel netwerk of het IP-regels.

Vanaf augustus 2018 is de Azure Database voor MariaDB-service is echter niet nog tussen de services die kunnen worden toegewezen rechtstreeks aan een subnet.

<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Meer informatie over virtual network-regels

Deze sectie beschrijft de verschillende details over virtual network-regels.

### <a name="only-one-geographic-region"></a>Slechts één geografische regio

Alle service-eindpunten voor Virtueelnetwerk is van toepassing op slechts één Azure-regio. Het eindpunt is niet ingeschakeld voor andere regio's communicatie accepteert van het subnet.

Een regel voor virtuele netwerken is beperkt tot de regio waarin het onderliggende eindpunt is van toepassing op.

### <a name="server-level-not-database-level"></a>Niveau van de server, niet op databaseniveau

Elk virtueel netwerk-regel van toepassing is op uw gehele Azure-Database voor MariaDB-server, niet alleen op een bepaalde database op de server. Met andere woorden, virtueel netwerk-regel wordt toegepast op de server-niveau, niet op niveau van de database.

### <a name="security-administration-roles"></a>Beveiligingsrollen voor beheer

Er is een scheiding van beveiligingsrollen in het beheer van service-eindpunten. Actie is vereist van elk van de volgende rollen:

- **De beheerder van het netwerk:** &nbsp; Schakel op het eindpunt.
- **De beheerder van de database:** &nbsp; Werk de toegangsbeheerlijst (ACL) om toe te voegen van het opgegeven subnet met de Azure Database voor MariaDB-server.

*RBAC alternatieve:*

De functies van netwerk-beheerder en beheerder van de Database beschikken over meer mogelijkheden dan nodig zijn voor het beheren van virtuele netwerkregels. Er is slechts een subset van hun mogelijkheden nodig.

U hebt de mogelijkheid van het gebruik van [op rollen gebaseerd toegangsbeheer (RBAC)] [ rbac-what-is-813s] in Azure een één aangepaste rol waarvoor alleen de benodigde subset van de mogelijkheden van maken. De aangepaste rol kan worden gebruikt in plaats van met betrekking tot de netwerk-beheerder of de Database-beheerder. De surface area van blootstelling van de beveiliging is lager als u een gebruiker aan een aangepaste rol toevoegen, ten opzichte van de gebruiker toe te voegen aan de andere twee belangrijke beheerdersrollen.

> [!NOTE]
> In sommige gevallen zijn de Azure Database voor MariaDB en het VNet-subnet in verschillende abonnementen. In dergelijke gevallen moet u ervoor zorgen dat de volgende configuraties:
> - Beide abonnementen moeten zich in dezelfde Azure Active Directory-tenant.
> - De gebruiker heeft de vereiste machtigingen voor het initiëren van bewerkingen, zoals service-eindpunten inschakelen en een VNet-subnet aan de opgegeven Server toe te voegen.

## <a name="limitations"></a>Beperkingen

Voor Azure Database voor MariaDB heeft de functie van de regels voor virtueel netwerk in de volgende beperkingen:

- In de firewall voor uw Azure Database voor MariaDB verwijst elke regel van het virtuele netwerk naar een subnet. Alle deze subnetten waarnaar wordt verwezen, moeten worden gehost in dezelfde geografische regio die als host fungeert voor de Azure Database voor MariaDB.

- Elke Azure-Database voor MariaDB-server kan maximaal 128 ACL-vermeldingen voor een bepaald virtueel netwerk hebben.

- Virtual network-regels gelden alleen voor virtuele netwerken van Azure Resource Manager; en niet op [klassieke implementatiemodel] [ resource-manager-deployment-model-568f] netwerken.

- Inschakelen op service-eindpunten met Azure Database voor MariaDB met behulp van de **Microsoft.Sql** servicetag kunt ook de eindpunten voor alle Azure-Database-services: Azure Database voor MariaDB, Azure Database voor MySQL, Azure Database voor PostgreSQL, Azure SQL Database en Azure SQL datawarehouse.

- Ondersteuning voor VNet-service-eindpunten is alleen voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.

- Op de firewall, IP-adresbereiken zijn van toepassing op de volgende items voor netwerken, maar regels voor virtueel netwerk dat niet doen:
    - [Site-naar-Site (S2S) virtueel particulier netwerk (VPN)][vpn-gateway-indexmd-608y]
    - On-premises via [ExpressRoute][expressroute-indexmd-744v]

## <a name="expressroute"></a>ExpressRoute

Als uw netwerk is verbonden met het Azure-netwerk door het gebruik van [ExpressRoute][expressroute-indexmd-744v], elk circuit is geconfigureerd met twee openbare IP-adressen op de Microsoft Edge. De twee IP-adressen worden gebruikt om verbinding maken met Microsoft-Services, zoals aan Azure Storage met behulp van openbare Azure-Peering.

Als u wilt toestaan dat communicatie tussen uw circuit en Azure Database voor MariaDB, moet u IP-netwerkregels voor openbare IP-adressen van uw circuits maken. Om te zoeken in de openbare IP-adressen van uw ExpressRoute-circuit, opent u een ondersteuningsticket met ExpressRoute met behulp van de Azure-portal.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Een VNET-firewallregel toevoegen aan uw server zonder in te schakelen op VNET-Service-eindpunten

Alleen instelling een firewallregel niet om de server met het VNet beveiligen. U moet ook de VNet-service-eindpunten inschakelen **op** voor de beveiliging pas van kracht. Wanneer u service-eindpunten inschakelen **op**, uw VNet-subnet ondervindt uitvaltijd totdat de bewerking is de overgang van **uit** naar **op**. Dit geldt met name in de context van grote VNets. U kunt de **IgnoreMissingServiceEndpoint** vlag te verlagen of elimineren van de uitvaltijd tijdens de overgang.

U kunt instellen dat de **IgnoreMissingServiceEndpoint** vlag met behulp van de Azure CLI of de portal.

## <a name="related-articles"></a>Verwante artikelen:
- [Virtuele netwerken van Azure][vm-virtual-network-overview]
- [Azure virtual network-service-eindpunten][vm-virtual-network-service-endpoints-overview-649d]

## <a name="next-steps"></a>Volgende stappen
Zie voor artikelen over het maken van VNet-regels:
- [Maken en beheren van Azure Database voor MariaDB-VNet-regels met behulp van de Azure portal](howto-manage-vnet-portal.md)
 
<!--
- [Create and manage Azure Database for MariaDB VNet rules using Azure CLI](howto-manage-vnet-using-cli.md)
-->

<!-- Link references, to text, Within this same GitHub repo. -->
[resource-manager-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[vm-virtual-network-overview]: ../virtual-network/virtual-networks-overview.md

[vm-virtual-network-service-endpoints-overview-649d]: ../virtual-network/virtual-network-service-endpoints-overview.md

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

[expressroute-indexmd-744v]: ../expressroute/index.yml
