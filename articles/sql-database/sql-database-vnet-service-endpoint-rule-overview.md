---
title: Virtual Network-service-eindpunten en regels voor Azure SQL Database | Microsoft Docs
description: Een subnet markeren als een service-eindpunt voor Virtueelnetwerk. Vervolgens het eindpunt als een virtueel netwerk-regel om de ACL uw Azure SQL Database. U-SQL-Database aanvaardt vervolgens communicatie van alle virtuele machines en andere knooppunten in het subnet.
services: sql-database
ms.service: sql-database
ms.prod_service: sql-database, sql-data-warehouse
author: DhruvMsft
manager: craigg
ms.custom: VNet Service endpoints
ms.topic: conceptual
ms.date: 07/18/2018
ms.reviewer: carlrab
ms.author: dmalik
ms.openlocfilehash: cdf067839c73f9da40d03628ff1c9920764e2219
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39127588"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-sql-database-and-sql-data-warehouse"></a>Gebruik Virtual Network-service-eindpunten en regels voor Azure SQL Database en SQL Data Warehouse

*Regels voor virtueel netwerk* zijn een functie van de firewall beveiliging die bepaalt of uw Azure [SQL-Database](sql-database-technical-overview.md) of [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) accepteert communicatie die zijn verzonden vanaf server specifieke subnetten in virtuele netwerken. In dit artikel wordt uitgelegd waarom de functie van de regel virtueel netwerk is het soms de beste optie voor het veilig toestaan van communicatie met uw Azure SQL-Database.

> [!NOTE]
> Dit onderwerp is van toepassing op Azure SQL-servers en op SQL Database- en SQL Data Warehouse-databases die op deze Azure SQL-servers worden gemaakt. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.

Het maken van een regel voor virtuele netwerken, er moet eerst worden een [service-eindpunt voor virtueel netwerk] [ vm-virtual-network-service-endpoints-overview-649d] voor de regel om te verwijzen naar.

#### <a name="how-to-create-a-virtual-network-rule"></a>Over het maken van een regel voor virtuele netwerken

Als u alleen een regel voor virtuele netwerken maakt, kunt u verder naar de stappen en uitleg [verderop in dit artikel](#anchor-how-to-by-using-firewall-portal-59j).






<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologie en beschrijving

**Virtueel netwerk:** kunt u virtuele netwerken die zijn gekoppeld aan uw Azure-abonnement hebt.

**Subnet:** een virtueel netwerk bevat **subnetten**. Een Azure virtual machines (VM's) waarvoor u zijn toegewezen aan subnetten. Één subnet kan meerdere virtuele machines of andere compute-knooppunten bevatten. COMPUTE-knooppunten die zich buiten het virtuele netwerk heeft geen toegang tot uw virtuele netwerk, tenzij u de beveiliging voor toegang configureren.

**Service-eindpunt voor virtueel netwerk:** A [service-eindpunt voor Virtueelnetwerk] [ vm-virtual-network-service-endpoints-overview-649d] is een subnet met eigenschappen die een of meer formele Azure-service typenamen bevatten. In dit artikel zijn we geïnteresseerd in de typenaam van **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database.

**Regel voor virtuele netwerken:** een regel voor virtuele netwerken voor uw SQL Database-server is een subnet dat wordt weergegeven in de toegangsbeheerlijst (ACL) van uw SQL Database-server. Als u in de ACL voor uw SQL-Database, het subnet moet bevatten de **Microsoft.Sql** typenaam.

Een regel voor virtuele netwerken wordt aan uw SQL Database-server om communicatie van elk knooppunt dat zich op het subnet te accepteren.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Voordelen van een regel voor virtuele netwerken

Totdat u actie onderneemt, wordt de virtuele machines op de subnetten van het niet kunnen communiceren met uw SQL-Database. Een actie die de communicatie is het maken van een regel voor virtuele netwerken. De logica voor het kiezen van de aanpak van de VNet-regel wordt een vergelijken en contrast bespreking met betrekking tot de concurrerende beveiligingsopties die worden aangeboden door de firewall.

#### <a name="a-allow-access-to-azure-services"></a>A. Toegang tot Azure-services toestaan

Het deelvenster firewall heeft een **aan/uit** knop die is aangeduid als **toegang tot Azure-services toestaan**. De **ON** instelling kan communicatie van alle Azure-IP-adressen en subnetten voor alle Azure. Deze Azure-IP-adressen of subnetten worden niet beheerd door u. Dit **ON** instelling is waarschijnlijk meer open dan u wilt dat uw SQL-Database te zijn. De functie van de regel virtueel netwerk maakt veel betere resultaten nauwkeurig beheer mogelijk.

#### <a name="b-ip-rules"></a>B. IP-regels

De SQL Database-firewall kunt u IP-adresbereiken waaruit communicatie naar SQL Database worden geaccepteerd. Deze aanpak is prima voor stabiele IP-adressen die zich buiten het particuliere netwerk van Azure. Maar veel knooppunten in het particuliere netwerk van Azure zijn geconfigureerd met *dynamische* IP-adressen. Dynamische IP-adressen kunnen wijzigen, zoals wanneer uw virtuele machine opnieuw wordt opgestart. Het zou fraai om op te geven van een dynamisch IP-adres in een firewallregel in een productieomgeving zijn.

U kunt de IP-optie restwaarde door het verkrijgen van een *statische* IP-adres voor uw virtuele machine. Zie voor meer informatie, [privé IP-adressen voor een virtuele machine configureren met behulp van de Azure-portal][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Echter, de statische IP-aanpak kan het lastig om te beheren, en is kostbare wanneer u klaar bent op schaal. Virtual network-regels zijn gemakkelijker te maken en te beheren.

#### <a name="c-cannot-yet-have-sql-database-on-a-subnet"></a>C. Kan niet nog SQL-Database op een subnet

Als uw Azure SQL Database-server een knooppunt in een subnet van het virtuele netwerk is, worden alle knooppunten in het virtuele netwerk kunnen communiceren met uw SQL-Database. In dit geval kunnen uw virtuele machines communiceren met de SQL-Database zonder dat u hoeft geen virtueel netwerkregels of IP-regels.

Vanaf September 2017, de Azure SQL Database-service is echter niet nog tussen de services die kunnen worden toegewezen aan een subnet.






<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Meer informatie over virtual network-regels

Deze sectie beschrijft de verschillende details over virtual network-regels.

#### <a name="only-one-geographic-region"></a>Slechts één geografische regio

Alle service-eindpunten voor Virtueelnetwerk is van toepassing op slechts één Azure-regio. Het eindpunt is niet ingeschakeld voor andere regio's communicatie accepteert van het subnet.

Een regel voor virtuele netwerken is beperkt tot de regio waarin het onderliggende eindpunt is van toepassing op.

#### <a name="server-level-not-database-level"></a>Niveau van de server, niet op databaseniveau

Elke regel van het virtuele netwerk is van toepassing op uw hele Azure SQL Database-server, niet alleen op een bepaalde database op de server. Met andere woorden, virtueel netwerk-regel wordt toegepast op de server-niveau, niet op niveau van de database.

- IP-regels kunnen daarentegen van toepassing op het niveau van een van beide.

#### <a name="security-administration-roles"></a>Beveiligingsrollen voor beheer

Er is een scheiding van beveiligingsrollen in het beheer van service-eindpunten. Actie is vereist van elk van de volgende rollen:

- **De beheerder van het netwerk:** &nbsp; inschakelen op het eindpunt.
- **De beheerder van de database:** &nbsp; bijwerken van de toegangsbeheerlijst (ACL) aan het opgegeven subnet toevoegen aan de SQL-Database-server.

*RBAC alternatieve:*

De functies van netwerk-beheerder en beheerder van de Database beschikken over meer mogelijkheden dan nodig zijn voor het beheren van virtuele netwerkregels. Er is slechts een subset van hun mogelijkheden nodig.

U hebt de mogelijkheid van het gebruik van [op rollen gebaseerd toegangsbeheer (RBAC)] [ rbac-what-is-813s] in Azure een één aangepaste rol waarvoor alleen de benodigde subset van de mogelijkheden van maken. De aangepaste rol kan worden gebruikt in plaats van met betrekking tot de netwerk-beheerder of de Database-beheerder. De surface area van blootstelling van de beveiliging is lager als u een gebruiker aan een aangepaste rol toevoegen, ten opzichte van de gebruiker toe te voegen aan de andere twee belangrijke beheerdersrollen.

> [!NOTE]
> In sommige gevallen worden de Azure SQL Database en het VNet-subnet in verschillende abonnementen zijn. In dergelijke gevallen moet u ervoor zorgen dat de volgende configuraties:
> - Beide abonnementen moeten zich in dezelfde Azure Active Directory-tenant.
> - De gebruiker heeft de vereiste machtigingen voor het initiëren van bewerkingen, zoals service-eindpunten inschakelen en een VNet-subnet aan de opgegeven Server toe te voegen.

## <a name="limitations"></a>Beperkingen

De functie van de regels voor virtueel netwerk heeft voor Azure SQL Database, de volgende beperkingen:

- Een Web-App kunnen worden toegewezen aan een privé IP-adres in een VNet/subnet. Zelfs als de service-eindpunten van het opgegeven VNet/subnet zijn ingeschakeld, worden verbindingen van de Web-App naar de server een Azure openbare IP-bron niet de bron van een VNet/subnet hebben. Als u wilt verbinding hebben met een Web-App naar een server met VNet-firewallregels inschakelen, moet u **alle Azure-services toestaan** op de server.

- In de firewall voor uw SQL-Database, is elke regel van het virtuele netwerk verwijst naar een subnet. Alle deze subnetten waarnaar wordt verwezen, moeten worden gehost in dezelfde geografische regio die als host fungeert voor de SQL-Database.

- Elke Azure SQL Database-server kan maximaal 128 ACL-vermeldingen voor een bepaald virtueel netwerk hebben.

- Virtual network-regels gelden alleen voor virtuele netwerken van Azure Resource Manager; en niet op [klassieke implementatiemodel] [ arm-deployment-model-568f] netwerken.

- Inschakelen van service-eindpunten naar Azure SQL Database kunt ook de eindpunten voor de MySQL en PostgreSQL-Azure-services. Echter met eindpunten op verbinding maken tussen de eindpunten en uw MySQL- of PostgreSQL-instanties, mislukken pogingen tot.
    - De onderliggende reden is dat MySQL en PostgreSQL momenteel geen ACLing ondersteunen.

- Op de firewall, IP-adresbereiken zijn van toepassing op de volgende items voor netwerken, maar regels voor virtueel netwerk dat niet doen:
    - [Site-naar-Site (S2S) virtueel particulier netwerk (VPN)][vpn-gateway-indexmd-608y]
    - On-premises via [ExpressRoute][expressroute-indexmd-744v]

#### <a name="considerations-when-using-service-endpoints"></a>Overwegingen bij het gebruik van Service-eindpunten
Wanneer u service-eindpunten voor Azure SQL Database, bekijk de volgende punten:

- **Uitgaand naar Azure SQL Database openbare IP-adressen is vereist**: Netwerkbeveiligingsgroepen (nsg's) naar Azure SQL Database IPs waarmee verbinding moet worden geopend. U kunt dit doen met behulp van NSG [servicetags](../virtual-network/security-overview.md#service-tags) voor Azure SQL Database.

#### <a name="expressroute"></a>ExpressRoute

Als uw netwerk is verbonden met het Azure-netwerk door het gebruik van [ExpressRoute][expressroute-indexmd-744v], elk circuit is geconfigureerd met twee openbare IP-adressen op de Microsoft Edge. De twee IP-adressen worden gebruikt om verbinding maken met Microsoft-Services, zoals aan Azure Storage met behulp van openbare Azure-Peering.

Als u wilt toestaan dat communicatie tussen uw circuit en Azure SQL Database, moet u IP-netwerkregels voor openbare IP-adressen van uw circuits maken. Om te zoeken in de openbare IP-adressen van uw ExpressRoute-circuit, opent u een ondersteuningsticket met ExpressRoute met behulp van de Azure-portal.


<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## <a name="impact-of-removing-allow-all-azure-services"></a>Gevolgen van het verwijderen van 'Alle Azure-Services toestaan'

Veel gebruikers wilt verwijderen **alle Azure-Services toestaan** van hun Azure SQL-Servers en vervang deze door een VNet-firewallregel.
Echter verwijderen van dit van invloed op de volgende Azure-SQLDB-functies:

#### <a name="import-export-service"></a>Import Export-Service
Azure SQLDB Import Export-Service wordt uitgevoerd op virtuele machines in Azure. Deze VM's zijn niet in uw VNet en daarom een Azure-IP-adres ophalen bij het verbinden met uw database. Over het verwijderen van **alle Azure-Services toestaan** deze VM's niet mogelijk toegang tot uw databases.
U kunt het probleem omzeilen. Uitvoeren van het BACPAC-importeren of exporteren rechtstreeks in uw code met behulp van de DACFx-API. Zorg ervoor dat deze is geïmplementeerd in een virtuele machine die zich in de VNet-subnet waarvoor u de firewallregel hebt ingesteld.

#### <a name="sql-database-query-editor"></a>SQL Database Query-Editor
De Azure SQL Database Query-Editor wordt geïmplementeerd op virtuele machines in Azure. Deze VM's zijn niet in uw VNet. Daarom wordt bij de virtuele machines een Azure IP-adres ophalen bij het verbinden met uw database. Over het verwijderen van **alle Azure-Services toestaan**, deze VM's niet mogelijk toegang tot uw databases.

#### <a name="table-auditing"></a>Tabel controleren
Op dit moment zijn er twee manieren om te controleren voor uw SQL-Database inschakelen. Tabelcontrole niet lukt nadat u service-eindpunten op uw Azure SQL-Server hebt ingeschakeld. Risicobeperking hier is om te verplaatsen naar de controlefunctie voor blobs.

#### <a name="impact-on-data-sync"></a>Gevolgen voor gegevenssynchronisatie
Azure SQLDB heeft de gegevenssynchronisatie-functie die verbinding maakt met uw databases met behulp van Azure-IP-adressen. Wanneer u service-eindpunten gebruikt, is het waarschijnlijk dat u wordt uitgeschakeld **alle Azure-Services toestaan** toegang tot uw logische server. Hierdoor wordt de functie Data Sync.

## <a name="impact-of-using-vnet-service-endpoints-with-azure-storage"></a>Gevolgen van het gebruik van VNet-Service-eindpunten met Azure storage

Azure Storage is de dezelfde functie waarmee u verbinding met uw Storage-account beperken geïmplementeerd.
Als u ervoor kiest deze functie wilt gebruiken met een opslagaccount dat wordt gebruikt door een Azure SQL-Server, kunt u problemen ondervindt. Vervolgens wordt een lijst met en een beschrijving van de Azure-SQLDB-functies die worden beïnvloed door dit.

#### <a name="azure-sqldw-polybase"></a>Azure RESOURCEKLASSE PolyBase
PolyBase wordt meestal gebruikt om gegevens te laden in Azure RESOURCEKLASSE van Storage-accounts. Als het opslagaccount dat u het laden van gegevens van alleen toegang tot een set van VNet-subnetten limieten, wordt verbinding hebben met PolyBase aan het Account verbroken. Er is een beperking voor deze, en u kunt contact opnemen met *dmalik@microsoft.com* voor meer informatie.

#### <a name="azure-sqldb-blob-auditing"></a>Azure SQLDB Blob Auditing
Auditlogboeken controlefunctie voor BLOBs worden verstuurd naar uw eigen opslagaccount. Connectiviteit van Azure SQLDB naar het opslagaccount dat wordt verbroken als u dit storage-account maakt gebruik van de functie voor hergebruik Service-eindpunten.


## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Een VNET-firewallregel toevoegen aan uw server zonder in te schakelen op VNET-Service-eindpunten

Lang geleden, voordat dit onderdeel is verbeterd, moest u VNet-service-eindpunten inschakelen voordat u een live VNet-regel in de Firewall kan implementeren. De eindpunten die betrekking hebben een bepaald VNet-subnet naar een Azure SQL-Database. Maar nu vanaf januari 2018, u kunt omzeilen deze vereiste door in te stellen de **IgnoreMissingServiceEndpoint** vlag.

Alleen instelling een firewallregel niet om de server beveiligen. U moet ook VNet-service-eindpunten op inschakelen om de beveiliging van kracht. Wanneer u service-eindpunten inschakelen, ondervindt uw VNet-subnet uitvaltijd totdat de bewerking is de overgang van uitgeschakeld naar op. Dit geldt met name in de context van grote VNets. U kunt de **IgnoreMissingServiceEndpoint** vlag te verlagen of elimineren van de uitvaltijd tijdens de overgang.

U kunt instellen dat de **IgnoreMissingServiceEndpoint** vlag met behulp van PowerShell. Zie voor meer informatie, [PowerShell voor het maken van een service-eindpunt voor Virtueelnetwerk en een regel voor Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].


## <a name="errors-40914-and-40615"></a>Fouten 40914 en 40615

Er is een verbindingsfout 40914 gekoppeld aan *virtuele netwerkregels*, zoals opgegeven in het deelvenster Firewall in Azure portal. Fout 40615 is vergelijkbaar, behalve deze zich tot verhoudt *IP-adresregels* op de Firewall.

#### <a name="error-40914"></a>Fout 40914

*Berichttekst:* server kan niet worden geopend '*[servernaam]* is aangevraagd door de aanmelding. Client is niet toegestaan voor toegang tot de server.

*Foutbeschrijving:* de client zich in een subnet met de eindpunten van virtual network-server. Maar de Azure SQL Database-server heeft geen regel voor virtuele netwerken die verleent aan het subnet van het recht om te communiceren met de SQL-Database.

*Fout bij het probleem zou moeten:* op de Firewall in de Azure portal, gebruik invloed hebben op de regels voor virtueel netwerk aan [een regel voor het virtuele netwerk toevoegen](#anchor-how-to-by-using-firewall-portal-59j) voor het subnet.

#### <a name="error-40615"></a>Fout 40615

*Berichttekst:* server kan niet worden geopend '{0}' aangevraagd door de aanmelding. De client met IP-adres{1}' is niet toegestaan voor toegang tot de server.

*Foutbeschrijving:* de client verbinding probeert te maken van een IP-adres dat is niet geautoriseerd voor verbinding met de Azure SQL Database-server. De firewall van de server heeft geen IP-adres-regel waarmee een client voor de communicatie van de opgegeven IP-adres met de SQL-Database.

*Fout bij het probleem zou moeten:* IP-adres van de client opgeven als een IP-regel. Dit doen met behulp van het deelvenster Firewall in Azure portal.


Een lijst met foutberichten voor verschillende SQL-Database wordt gedocumenteerd [hier][sql-database-develop-error-messages-419g].





<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="portal-can-create-a-virtual-network-rule"></a>Portal kunt maken van een regel voor virtuele netwerken

Deze sectie wordt beschreven hoe u kunt de [Azure-portal] [ http-azure-portal-link-ref-477t] maken een *regel voor virtuele netwerken* in uw Azure SQL Database. De regel wordt de SQL-Database communicatie accepteert van een bepaald subnet dat is gemarkeerd als een *service-eindpunt voor Virtueelnetwerk*.

> [!NOTE]
> Als u van plan bent een service-eindpunt toevoegen aan de VNet-firewallregels van uw Azure SQL Database-server, eerst voor zorgen dat service eindpunten zijn ingeschakeld voor het subnet.
>
> Als service-eindpunten zijn niet ingeschakeld voor het subnet, de portal u wordt gevraagd in te schakelen. Klik op de **inschakelen** knop op de dezelfde blade waarop u de regel toevoegen.

#### <a name="powershell-alternative"></a>Alternatieve van PowerShell

Een PowerShell-script kunt ook regels voor virtueel netwerk maken. De essentiële cmdlet **New-azurermsqlservervirtualnetworkrule toegevoegd om**. Als u nodig hebt, Zie [PowerShell voor het maken van een service-eindpunt voor Virtueelnetwerk en een regel voor Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

#### <a name="rest-api-alternative"></a>Alternatieve REST-API

Intern maakt aanroept de PowerShell-cmdlets voor acties van de SQL-VNet REST API's. U kunt de REST API's rechtstreeks aanroepen.

- [Regels voor virtueel netwerk: bewerkingen][rest-api-virtual-network-rules-operations-862r]

#### <a name="prerequisites"></a>Vereisten

U moet al een subnet dat is gemarkeerd met het specifieke service-eindpunt voor Virtueelnetwerk hebben *typenaam* relevant zijn voor Azure SQL Database.

- De naam van het relevante eindpunt is **Microsoft.Sql**.
- Als uw subnet kan niet worden gelabeld met de naam van het type, Zie [controleren of uw subnet is een eindpunt][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

### <a name="azure-portal-steps"></a>Stappen voor Azure portal

1. Meld u aan bij de [Azure-portal][http-azure-portal-link-ref-477t].

2. Navigeer in de portal **SQL-servers** &gt; **Firewall / virtuele netwerken**.

3. Stel de **toegang tot Azure-services toestaan** besturingselement in op OFF.

    > [!IMPORTANT]
    > Als u het besturingselement instellen op ON laat, accepteert uw Azure SQL Database-server-communicatie van elk subnet. Het besturingselement instellen op ON verlaten mogelijk overmatige toegang vanuit het oogpunt van een security. De functie Microsoft Azure Virtual Network-service-eindpunt, in combinatie met de functie van de regel virtueel netwerk van de SQL-Database, kunt samen het gebied van beveiliging beperken.

4. Klik op de **+ toevoegen bestaande** beheren in de **virtuele netwerken** sectie.

    ![Klik op toevoegen aan bestaande (als een SQL-regel subnet eindpunt).][image-portal-firewall-vnet-add-existing-10-png]

5. In de nieuwe **Create/Update** deelvenster, vult u in de besturingselementen met de namen van uw Azure-resources.

    > [!TIP]
    > U moet de juiste bevatten **adresvoorvoegsel** voor uw subnet. U vindt de waarde in de portal.
    > Navigeer **alle resources** &gt; **alle typen** &gt; **virtuele netwerken**. Het filter wordt uw virtuele netwerken. Klik op het virtuele netwerk en klik vervolgens op **subnetten**. De **ADRESBEREIK** kolom heeft het adresvoorvoegsel dat u nodig hebt.

    ![Vul de velden voor nieuwe regel.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Klik op de **OK** knop aan de onderkant van het deelvenster.

7.  Zie de regel voor de resulterende virtuele netwerken in het deelvenster firewall.

    ![Zie de nieuwe regel in het deelvenster firewall.][image-portal-firewall-vnet-result-rule-30-png]


> [!NOTE]
> De volgende statussen of statussen van toepassing op de regels:
> - **Gereed:** geeft aan dat de bewerking die u hebt gestart is voltooid.
> - **Is mislukt:** geeft aan dat de bewerking die u hebt gestart is mislukt.
> - **Verwijderd:** alleen van toepassing op de Delete-bewerking, en geeft aan dat de regel is verwijderd en niet langer van toepassing.
> - **InProgress:** geeft aan dat de bewerking uitgevoerd wordt. De oude regel van toepassing is terwijl de bewerking in deze status is.


<a name="anchor-how-to-links-60h" />

## <a name="related-articles"></a>Verwante artikelen:

- [Azure virtual network-service-eindpunten][vm-virtual-network-service-endpoints-overview-649d]
- [Azure SQL-Database op serverniveau en databaseniveau firewall-regels][sql-db-firewall-rules-config-715d]

De functie van de regel virtueel netwerk voor Azure SQL Database is beschikbaar in latere September 2017 geworden.

## <a name="next-steps"></a>Volgende stappen

- [PowerShell gebruiken voor het maken van een service-eindpunt voor virtueel netwerk en vervolgens een regel voor virtuele netwerken voor Azure SQL Database.][sql-db-vnet-service-endpoint-rule-powershell-md-52d]
- [Regels voor virtueel netwerk: Bewerkingen] [ rest-api-virtual-network-rules-operations-862r] met REST-API's



<!-- Link references, to images. -->

[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png

[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png

[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png



<!-- Link references, to text, Within this same Github repo. -->

[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[expressroute-indexmd-744v]: ../expressroute/index.yml

[rbac-what-is-813s]:../role-based-access-control/overview.md

[sql-db-firewall-rules-config-715d]: sql-database-firewall-configure.md

[sql-database-develop-error-messages-419g]: sql-database-develop-error-messages.md

[sql-db-vnet-service-endpoint-rule-powershell-md-52d]: sql-database-vnet-service-endpoint-rule-powershell.md

[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]: sql-database-vnet-service-endpoint-rule-powershell.md#a-verify-subnet-is-endpoint-ps-100

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[vm-virtual-network-service-endpoints-overview-649d]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml



<!-- Link references, to text, Outside this Github repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/

[rest-api-virtual-network-rules-operations-862r]: https://docs.microsoft.com/rest/api/sql/virtualnetworkrules



<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->
