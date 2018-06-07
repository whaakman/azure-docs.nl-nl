---
title: Azure-Database voor MySQL-Server vnet eindpunt Serviceoverzicht | Microsoft Docs
description: Beschrijft hoe service-eindpunten vnet werken voor uw Azure-Database voor de MySQL-server.
services: mysql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.topic: conceptual
ms.date: 5/29/2018
ms.openlocfilehash: 652657c8891f2320c026251ffa32e4787028ee18
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655183"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-database-for-mysql"></a>Het gebruik van virtueel netwerk service-eindpunten en regels voor Azure-Database voor MySQL

*Virtueel netwerk regels* zijn een functie van de firewall beveiliging die bepaalt of uw Azure-Database voor de MySQL-server accepteert communicaties die van bepaalde subnetten in virtuele netwerken worden verzonden. Dit artikel wordt uitgelegd waarom de functie van de regel virtueel netwerk is het soms de beste optie voor het toestaan van veilige communicatie met uw Azure-Database voor de MySQL-server.

Een regel virtueel netwerk maken eerst moet er een [virtueel netwerk] [ vm-virtual-network-overview] (VNet) en een [virtueel netwerk service-eindpunt] [ vm-virtual-network-service-endpoints-overview-649d] voor de regel om te verwijzen naar. De volgende afbeelding ziet u hoe een service-eindpunt van het virtuele netwerk werkt met Azure Database voor MySQL:

![Voorbeeld van de werking van een VNet-Service-eindpunt](media/concepts-data-access-and-security-vnet/vnet-concept.png)

> [!NOTE]
> Deze functie is beschikbaar in public preview in alle regio's van Azure openbare cloud waarop Azure voor MySQL-Database wordt geïmplementeerd voor Azure-Database voor MySQL.

<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologie en -beschrijving

**Virtueel netwerk:** kunt u virtuele netwerken die zijn gekoppeld aan uw Azure-abonnement hebt.

**Subnet:** een virtueel netwerk bevat **subnetten**. Eventuele Azure virtuele machines (VM's) die u hebt toegewezen aan subnetten. Eén subnet kan meerdere virtuele machines of andere rekenknooppunten bevatten. COMPUTE knooppunten die zich buiten het virtuele netwerk geen toegang tot het virtuele netwerk tenzij u de beveiliging voor toegang configureren.

**Virtueel netwerk service-eindpunt:** A [Virtual Network service-eindpunt] [ vm-virtual-network-service-endpoints-overview-649d] een subnet waarvan de eigenschapwaarden een of meer namen van formele Azure-service bevatten. In dit artikel we geïnteresseerd bent in de typenaam van **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database. Deze servicetag geldt ook voor de Azure-Database voor MySQL en PostgreSQL-services. Het is belangrijk te weten bij het toepassen van de **Microsoft.Sql** tag service met een VNet-service-eindpunt wordt het netwerkverkeer voor de service-eindpunt configureren voor Azure SQL Database, Azure-Database voor MySQL en Azure-Database voor PostgreSQL-servers op het subnet. 

**Virtueel netwerk regel:** een regel voor uw Azure-Database voor de MySQL-server het virtuele netwerk is een subnet op dat wordt vermeld in de toegangsbeheerlijst (ACL) van uw Azure-Database voor de MySQL-server. Als u in de ACL voor uw Azure-Database voor de MySQL-server, het subnet moet bevatten de **Microsoft.Sql** typenaam.

Een virtueel netwerk regel geeft aan dat uw Azure-Database voor de MySQL-server te accepteren van communicatie van elk knooppunt in het subnet.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Voordelen van een virtueel netwerk-regel

Totdat u actie onderneemt, wordt de virtuele machines op de subnetten kunnen niet communiceren met uw Azure-Database voor de MySQL-server. Een actie die de communicatie is het maken van een regel van het virtuele netwerk. De logica voor het kiezen van de aanpak van de regel VNet vereist een vergelijken en contrast discussie met betrekking tot de concurrerende beveiligingsopties die worden aangeboden door de firewall.

### <a name="a-allow-access-to-azure-services"></a>A. Toegang tot Azure-services toestaan

Het deelvenster verbinding beveiliging heeft een **aan/uit** knop met de naam **toegang tot Azure-services toestaan**. De **ON** instelling kan communicatie van alle Azure-IP-adressen en alle Azure-subnetten. Deze Azure-IP-adressen of subnetten mogelijk geen eigendom van u. Dit **ON** instelling openstaat waarschijnlijk meer dan u wilt dat uw Azure-Database voor de MySQL-Database te zijn. De functie van de regel virtueel netwerk biedt veel betere resultaten gedetailleerde controle.

### <a name="b-ip-rules"></a>B. IP-regels

De Azure-Database voor MySQL firewall kunt u IP-adresbereiken waarvan communicatie worden geaccepteerd in de Azure-Database voor de MySQL-Database opgeven. Deze methode werkt goed voor stabiele IP-adressen die zich buiten het particuliere netwerk van Azure. Maar veel knooppunten in het persoonlijke Azure-netwerk zijn geconfigureerd met *dynamische* IP-adressen. Dynamische IP-adressen kunnen wijzigen, zoals wanneer uw virtuele machine opnieuw is opgestart. Het zou fraai om op te geven van een dynamische IP-adres in een firewallregel in een productieomgeving zijn.

U kunt de IP-optie restwaarde door het verkrijgen van een *statische* IP-adres voor uw virtuele machine. Zie voor meer informatie [privé IP-adressen voor een virtuele machine configureren via de Azure-portal][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Echter, de statische IP-benadering kan moeilijk te beheren, en is kostbaar wanneer u klaar bent op grote schaal. Virtueel netwerk regels zijn gemakkelijker te maken en te beheren.

### <a name="c-cannot-yet-have-azure-database-for-mysql-on-a-subnet-without-defining-a-service-endpoint"></a>C. Nog geen Azure-Database voor MySQL in een subnet zonder te definiëren van een service-eindpunt

Als uw **Microsoft.Sql** server is een knooppunt in een subnet van het virtuele netwerk, alle knooppunten in het virtuele netwerk kunnen communiceren met uw Azure-Database voor de MySQL-server. In dit geval kunnen uw virtuele machines communiceren met Azure-Database voor MySQL zonder een virtueel netwerk regels of IP-regels.

Echter vanaf mei 2018, de Azure-Database voor de MySQL-service nog niet tussen de services die kunnen worden toegewezen rechtstreeks aan een subnet.

<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Meer informatie over de regels voor virtueel netwerk

Deze sectie beschrijft de verschillende details over de regels voor virtueel netwerk.

### <a name="only-one-geographic-region"></a>Slechts één geografische regio

Elk virtueel netwerk service-eindpunt is van toepassing op slechts één Azure-regio. Het eindpunt is niet ingeschakeld voor andere regio's communicatie accepteert van het subnet.

Elke regel van het virtuele netwerk is beperkt tot de regio die het onderliggende eindpunt is van toepassing op.

### <a name="server-level-not-database-level"></a>Niveau van de server, niet databaseniveau

Elke regel van het virtuele netwerk is van toepassing op uw hele Azure-Database voor de MySQL-server, niet alleen op een bepaalde database op de server. Met andere woorden, virtueel netwerk regel wordt toegepast op de server-niveau, niet op het database-niveau.

### <a name="security-administration-roles"></a>Beveiligingsrollen voor beheer

Er is een scheiding van beveiligingsrollen in het beheer van virtueel netwerk service-eindpunten. Actie is vereist van elk van de volgende rollen:

- **Netwerk-Admin:** &nbsp; schakelt u het eindpunt.
- **Database-Admin:** &nbsp; bijwerken van de toegangsbeheerlijst (ACL) bepaald subnet toevoegen aan de Azure-Database voor de MySQL-server.

*RBAC alternatief:*

De functies van netwerk-beheerder en Database Admin beschikken over meer mogelijkheden dan nodig zijn voor het beheren van virtueel netwerk regels. Er is slechts een subset van hun mogelijkheden nodig.

U hebt de optie voor het gebruik van [op rollen gebaseerde toegangsbeheer (RBAC)] [ rbac-what-is-813s] in Azure maken van een enkele aangepaste functie die alleen de benodigde subset van de mogelijkheden heeft. De aangepaste rol kan worden gebruikt in plaats van met betrekking tot de netwerk-beheerder of de Database-beheerder. Het serveroppervlaktegebied van uw beveiligingsrisico is lager als u een gebruiker aan een aangepaste beveiligingsrol toevoegt, ten opzichte van de gebruiker toe te voegen aan de andere twee belangrijke beheerdersrollen.

> [!NOTE]
> In sommige gevallen zijn de Azure-Database voor MySQL en het VNet-subnet in verschillende abonnementen behoren. In dergelijke gevallen moet u ervoor zorgen dat de volgende configuraties:
> - Beide abonnementen moeten in dezelfde Azure Active Directory-tenant.
> - De gebruiker heeft de vereiste machtigingen voor het initiëren van bewerkingen, zoals service-eindpunten inschakelen en een VNet-subnet aan de opgegeven Server toe te voegen.

## <a name="limitations"></a>Beperkingen

De functie van de regels voor virtueel netwerk heeft voor de Azure-Database voor MySQL, de volgende beperkingen:

- In de firewall voor uw Azure-Database voor MySQL verwijst elke regel van het virtuele netwerk naar een subnet. Alle deze subnetten waarnaar wordt verwezen, moeten worden gehost in dezelfde geografische regio die als host fungeert voor de Azure-Database voor MySQL.

- Elke Azure-Database voor de MySQL-server kan maximaal 128 ACL-vermeldingen voor een bepaald virtueel netwerk hebben.

- Virtueel netwerk regels alleen van toepassing op virtuele netwerken van Azure Resource Manager; en niet op [klassieke implementatiemodel] [ arm-deployment-model-568f] netwerken.

- Inschakelen van virtueel netwerk service-eindpunten met Azure-Database voor het gebruik van MySQL de **Microsoft.Sql** code van de service kan ook de eindpunten voor alle Azure-Database-services: Azure Database voor MySQL, Azure-Database voor PostgreSQL, Azure SQL Database en Azure SQL datawarehouse.

- Er is geen ondersteuning voor VNet verplaatsen bewerkingen op het moment van de openbare preview. Als u de regel van een virtueel netwerk, wilt verwijderen en maak deze opnieuw.

- Ondersteuning voor VNet-service-eindpunten is alleen voor algemene doeleinden en geoptimaliseerd voor geheugen-servers.

- Op de firewall IP-adresbereiken zijn van toepassing op de volgende items voor netwerken, maar niet het virtuele netwerk regels:
    - [Site-naar-Site (S2S) virtueel particulier netwerk (VPN)][vpn-gateway-indexmd-608y]
    - On-premises via [ExpressRoute][expressroute-indexmd-744v]

## <a name="expressroute"></a>ExpressRoute

Als uw netwerk is verbonden met het Azure-netwerk door het gebruik van [ExpressRoute][expressroute-indexmd-744v], elk circuit is geconfigureerd met twee openbare IP-adressen op de Microsoft Edge. De twee IP-adressen worden gebruikt om verbinding met Microsoft-Services, zoals naar Azure Storage via openbare Azure-Peering.

Communicatie wilt toestaan van uw circuit met Azure-Database voor MySQL, moet u IP-netwerk regels voor het openbare IP-adressen van uw circuits maken. Om te zoeken naar het openbare IP-adressen van uw ExpressRoute-circuit, moet u een ondersteuningsticket opent met ExpressRoute met behulp van de Azure-portal.

## <a name="related-articles"></a>Verwante artikelen:
- [Virtuele netwerken in Azure][vm-virtual-network-overview]
- [Virtueel netwerk van Azure service-eindpunten][vm-virtual-network-service-endpoints-overview-649d]

## <a name="next-steps"></a>Volgende stappen
Zie voor artikelen over het maken van de VNet-regels:
- [Maken en beheren van Azure-Database voor MySQL VNet regels met de Azure portal](howto-manage-vnet-using-portal.md)
- [Maken en beheren van Azure-Database voor MySQL VNet regels met Azure CLI](howto-manage-vnet-using-cli.md)

<!-- Link references, to text, Within this same Github repo. -->
[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[vm-virtual-network-overview]: ../virtual-network/virtual-networks-overview.md

[vm-virtual-network-service-endpoints-overview-649d]: ../virtual-network/virtual-network-service-endpoints-overview.md

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

[expressroute-indexmd-744v]: ../expressroute/index.yml