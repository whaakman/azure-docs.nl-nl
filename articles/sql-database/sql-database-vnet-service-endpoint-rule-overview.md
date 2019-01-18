---
title: Virtual Network-service-eindpunten en regels voor Azure SQL Database en SQL Data Warehouse | Microsoft Docs
description: Een subnet markeren als een service-eindpunt voor Virtueelnetwerk. Vervolgens het eindpunt als een virtueel netwerk-regel om de ACL uw Azure SQL Database. U-SQL-Database aanvaardt vervolgens communicatie van alle virtuele machines en andere knooppunten in het subnet.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: vanto, genemi
manager: craigg
ms.date: 01/17/2019
ms.openlocfilehash: 0a0a5a046bd1afefe3f4c72e713a0dafe0c856e4
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54390398"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-sql"></a>Gebruik Virtual Network-service-eindpunten en regels voor Azure SQL

*Regels voor virtueel netwerk* zijn een functie van de firewall beveiliging die bepaalt of uw Azure [SQL-Database](sql-database-technical-overview.md) of [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) accepteert communicatie die zijn verzonden vanaf server specifieke subnetten in virtuele netwerken. In dit artikel wordt uitgelegd waarom de functie van de regel virtueel netwerk is het soms de beste optie voor het veilig toestaan van communicatie met uw Azure SQL Database en SQL Data Warehouse.

> [!IMPORTANT]
> Dit onderwerp is van toepassing op Azure SQL-servers en op SQL Database- en SQL Data Warehouse-databases die op deze Azure SQL-servers worden gemaakt. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse. In dit artikel biedt *niet* zijn van toepassing op **Azure SQL Database Managed Instance**.

Het maken van een regel voor virtuele netwerken, er moet eerst worden een [service-eindpunt voor virtueel netwerk] [ vm-virtual-network-service-endpoints-overview-649d] voor de regel om te verwijzen naar.

## <a name="how-to-create-a-virtual-network-rule"></a>Over het maken van een regel voor virtuele netwerken

Als u alleen een regel voor virtuele netwerken maakt, kunt u verder naar de stappen en uitleg [verderop in dit artikel](#anchor-how-to-by-using-firewall-portal-59j).

<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologie en beschrijving

**Virtueel netwerk:** U kunt virtuele netwerken die zijn gekoppeld aan uw Azure-abonnement hebben.

**Subnet:** Een virtueel netwerk bevat **subnetten**. Een Azure virtual machines (VM's) waarvoor u zijn toegewezen aan subnetten. Één subnet kan meerdere virtuele machines of andere compute-knooppunten bevatten. COMPUTE-knooppunten die zich buiten het virtuele netwerk heeft geen toegang tot uw virtuele netwerk, tenzij u de beveiliging voor toegang configureren.

**Virtual Network service-eindpunt:** Een [service-eindpunt voor Virtueelnetwerk] [ vm-virtual-network-service-endpoints-overview-649d] is een subnet met eigenschappen die een of meer formele Azure-service typenamen bevatten. In dit artikel zijn we geïnteresseerd in de typenaam van **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database.

**Regel voor virtuele netwerken:** Een regel voor virtuele netwerken voor uw SQL Database-server is een subnet dat wordt weergegeven in de toegangsbeheerlijst (ACL) van uw SQL Database-server. Als u in de ACL voor uw SQL-Database, het subnet moet bevatten de **Microsoft.Sql** typenaam.

Een regel voor virtuele netwerken wordt aan uw SQL Database-server om communicatie van elk knooppunt dat zich op het subnet te accepteren.

<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Voordelen van een regel voor virtuele netwerken

Totdat u actie onderneemt, wordt de virtuele machines op de subnetten van het niet kunnen communiceren met uw SQL-Database. Een actie die de communicatie is het maken van een regel voor virtuele netwerken. De logica voor het kiezen van de aanpak van de VNet-regel wordt een vergelijken en contrast bespreking met betrekking tot de concurrerende beveiligingsopties die worden aangeboden door de firewall.

### <a name="a-allow-access-to-azure-services"></a>A. Toegang tot Azure-services toestaan

Het deelvenster firewall heeft een **aan/uit** knop die is aangeduid als **toegang tot Azure-services toestaan**. De **ON** instelling kan communicatie van alle Azure-IP-adressen en subnetten voor alle Azure. Deze Azure-IP-adressen of subnetten worden niet beheerd door u. Dit **ON** instelling is waarschijnlijk meer open dan u wilt dat uw SQL-Database te zijn. De functie van de regel virtueel netwerk maakt veel betere resultaten nauwkeurig beheer mogelijk.

### <a name="b-ip-rules"></a>B. IP-regels

De SQL Database-firewall kunt u IP-adresbereiken waaruit communicatie naar SQL Database worden geaccepteerd. Deze aanpak is prima voor stabiele IP-adressen die zich buiten het particuliere netwerk van Azure. Maar veel knooppunten in het particuliere netwerk van Azure zijn geconfigureerd met *dynamische* IP-adressen. Dynamische IP-adressen kunnen wijzigen, zoals wanneer uw virtuele machine opnieuw wordt opgestart. Het zou fraai om op te geven van een dynamisch IP-adres in een firewallregel in een productieomgeving zijn.

U kunt de IP-optie restwaarde door het verkrijgen van een *statische* IP-adres voor uw virtuele machine. Zie voor meer informatie, [privé IP-adressen voor een virtuele machine configureren met behulp van de Azure-portal][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Echter, de statische IP-aanpak kan het lastig om te beheren, en is kostbare wanneer u klaar bent op schaal. Virtual network-regels zijn gemakkelijker te maken en te beheren.

> [!NOTE]
> U kunt de SQL-Database nog geen op een subnet. Als uw Azure SQL Database-server een knooppunt in een subnet van het virtuele netwerk is, worden alle knooppunten in het virtuele netwerk kunnen communiceren met uw SQL-Database. In dit geval kunnen uw virtuele machines communiceren met de SQL-Database zonder dat u hoeft geen virtueel netwerkregels of IP-regels.

Vanaf September 2017, de Azure SQL Database-service is echter niet nog tussen de services die kunnen worden toegewezen aan een subnet.

<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Meer informatie over virtual network-regels

Deze sectie beschrijft de verschillende details over virtual network-regels.

### <a name="only-one-geographic-region"></a>Slechts één geografische regio

Alle service-eindpunten voor Virtueelnetwerk is van toepassing op slechts één Azure-regio. Het eindpunt is niet ingeschakeld voor andere regio's communicatie accepteert van het subnet.

Een regel voor virtuele netwerken is beperkt tot de regio waarin het onderliggende eindpunt is van toepassing op.

### <a name="server-level-not-database-level"></a>Niveau van de server, niet op databaseniveau

Elke regel van het virtuele netwerk is van toepassing op uw hele Azure SQL Database-server, niet alleen op een bepaalde database op de server. Met andere woorden, virtueel netwerk-regel wordt toegepast op de server-niveau, niet op niveau van de database.

- IP-regels kunnen daarentegen van toepassing op het niveau van een van beide.

### <a name="security-administration-roles"></a>Beveiligingsrollen voor beheer

Er is een scheiding van beveiligingsrollen in het beheer van service-eindpunten. Actie is vereist van elk van de volgende rollen:

- **De beheerder van het netwerk:** &nbsp; Schakel op het eindpunt.
- **De beheerder van de database:** &nbsp; Werk de toegangsbeheerlijst (ACL) aan het opgegeven subnet toevoegen aan de SQL-Database-server.

*RBAC alternatieve:*

De functies van netwerk-beheerder en beheerder van de Database beschikken over meer mogelijkheden dan nodig zijn voor het beheren van virtuele netwerkregels. Er is slechts een subset van hun mogelijkheden nodig.

U hebt de mogelijkheid van het gebruik van [op rollen gebaseerd toegangsbeheer (RBAC)] [ rbac-what-is-813s] in Azure een één aangepaste rol waarvoor alleen de benodigde subset van de mogelijkheden van maken. De aangepaste rol kan worden gebruikt in plaats van met betrekking tot de netwerk-beheerder of de Database-beheerder. De surface area van blootstelling van de beveiliging is lager als u een gebruiker aan een aangepaste rol toevoegen, ten opzichte van de gebruiker toe te voegen aan de andere twee belangrijke beheerdersrollen.

> [!NOTE]
> In sommige gevallen worden de Azure SQL Database en het VNet-subnet in verschillende abonnementen zijn. In dergelijke gevallen moet u ervoor zorgen dat de volgende configuraties:
> - Beide abonnementen moeten zich in dezelfde Azure Active Directory-tenant.
> - De gebruiker heeft de vereiste machtigingen voor het initiëren van bewerkingen, zoals service-eindpunten inschakelen en een VNet-subnet aan de opgegeven Server toe te voegen.
> - Beide abonnementen beschikken over de Microsoft.Sql-provider geregistreerd.

## <a name="limitations"></a>Beperkingen

De functie van de regels voor virtueel netwerk heeft voor Azure SQL Database, de volgende beperkingen:

- Een Web-App kunnen worden toegewezen aan een privé IP-adres in een VNet/subnet. Zelfs als de service-eindpunten van het opgegeven VNet/subnet zijn ingeschakeld, worden verbindingen van de Web-App naar de server een Azure openbare IP-bron niet de bron van een VNet/subnet hebben. Als u wilt verbinding hebben met een Web-App naar een server met VNet-firewallregels inschakelen, moet u **kunnen Azure-services tot server** op de server.

- In de firewall voor uw SQL-Database, is elke regel van het virtuele netwerk verwijst naar een subnet. Alle deze subnetten waarnaar wordt verwezen, moeten worden gehost in dezelfde geografische regio die als host fungeert voor de SQL-Database.

- Elke Azure SQL Database-server kan maximaal 128 ACL-vermeldingen voor een bepaald virtueel netwerk hebben.

- Virtual network-regels gelden alleen voor virtuele netwerken van Azure Resource Manager; en niet op [klassieke implementatiemodel] [ arm-deployment-model-568f] netwerken.

- Inschakelen van service-eindpunten naar Azure SQL Database kunt ook de eindpunten voor de MySQL en PostgreSQL-Azure-services. Met eindpunten op kunnen pogingen tot verbinding maken tussen de eindpunten en uw MySQL- of PostgreSQL-instanties echter mislukken.
  - De onderliggende reden is dat MySQL en PostgreSQL waarschijnlijk geen een regel voor virtuele netwerken geconfigureerd. U moet een regel voor virtuele netwerken configureren voor Azure Database voor MySQL en PostgreSQL en de verbinding slaagt.

- Op de firewall, IP-adresbereiken zijn van toepassing op de volgende items voor netwerken, maar regels voor virtueel netwerk dat niet doen:
  - [Site-naar-Site (S2S) virtueel particulier netwerk (VPN)][vpn-gateway-indexmd-608y]
  - On-premises via [ExpressRoute][expressroute-indexmd-744v]

### <a name="considerations-when-using-service-endpoints"></a>Overwegingen bij het gebruik van Service-eindpunten

Wanneer u service-eindpunten voor Azure SQL Database, bekijk de volgende punten:

- **Uitgaand naar Azure SQL Database openbare IP-adressen is vereist**: Netwerkbeveiligingsgroepen (nsg's) moeten worden geopend op Azure SQL Database IP's om toe te staan verbinding. U kunt dit doen met behulp van NSG [servicetags](../virtual-network/security-overview.md#service-tags) voor Azure SQL Database.

### <a name="expressroute"></a>ExpressRoute

Als u [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) gebruikt vanuit uw on-premises netwerk voor openbare peering of Microsoft-peering, moet u de NAT IP-adressen opgeven die worden gebruikt. Voor openbare peering gebruikt elk ExpressRoute-circuit standaard twee NAT IP-adressen. Deze worden toegepast op Azure-serviceverkeer wanneer het verkeer het Microsoft Azure-backbone-netwerk binnenkomt. Voor Microsoft-peering worden de NAT IP-adressen die worden gebruikt opgegeven door de klant of de serviceprovider. Voor toegang tot uw serviceresources moet u deze openbare IP-adressen toestaan in de instelling voor IP-firewall voor de resource. Wanneer u op zoek bent naar de IP-adressen van uw ExpressRoute-circuit, opent u [een ondersteuningsticket met ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) via de Azure Portal. Meer informatie over [NAT voor openbare peering en Microsoft-peering met ExpressRoute.](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering)
  
Als u wilt toestaan dat communicatie tussen uw circuit en Azure SQL Database, moet u IP-netwerkregels voor openbare IP-adressen van uw NAT bevinden.

<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## <a name="impact-of-removing-allow-azure-services-to-access-server"></a>Gevolgen van het toestaan van Azure-services voor toegang tot server verwijderen

Veel gebruikers wilt verwijderen **kunnen Azure-services tot server** van hun Azure SQL-Servers en vervang deze door een VNet-firewallregel.
Echter verwijderen van dit van invloed op de volgende functies:

### <a name="import-export-service"></a>Import Export-Service

Azure SQL-Database importeren exporteren Service wordt uitgevoerd op virtuele machines in Azure. Deze VM's zijn niet in uw VNet en daarom een Azure-IP-adres ophalen bij het verbinden met uw database. Over het verwijderen van **kunnen Azure-services tot server** deze VM's niet mogelijk toegang tot uw databases.
U kunt het probleem omzeilen. Uitvoeren van het BACPAC-importeren of exporteren rechtstreeks in uw code met behulp van de DACFx-API. Zorg ervoor dat deze is geïmplementeerd in een virtuele machine die zich in de VNet-subnet waarvoor u de firewallregel hebt ingesteld.

### <a name="sql-database-query-editor"></a>SQL Database Query-Editor

De Azure SQL Database Query-Editor wordt geïmplementeerd op virtuele machines in Azure. Deze VM's zijn niet in uw VNet. Daarom wordt bij de virtuele machines een Azure IP-adres ophalen bij het verbinden met uw database. Over het verwijderen van **kunnen Azure-services tot server**, deze VM's niet mogelijk toegang tot uw databases.

### <a name="table-auditing"></a>Tabel controleren

Op dit moment zijn er twee manieren om te controleren voor uw SQL-Database inschakelen. Tabelcontrole niet lukt nadat u service-eindpunten op uw Azure SQL-Server hebt ingeschakeld. Risicobeperking hier is om te verplaatsen naar de controlefunctie voor blobs.

### <a name="impact-on-data-sync"></a>Gevolgen voor gegevenssynchronisatie

Azure SQL-Database heeft de gegevenssynchronisatie-functie die verbinding maakt met uw databases met behulp van Azure-IP-adressen. Wanneer u service-eindpunten gebruikt, is het waarschijnlijk dat u wordt uitgeschakeld **kunnen Azure-services tot server** toegang tot uw logische server. Hierdoor wordt de functie Data Sync.

## <a name="impact-of-using-vnet-service-endpoints-with-azure-storage"></a>Gevolgen van het gebruik van VNet-Service-eindpunten met Azure storage

Azure Storage is de dezelfde functie waarmee u verbinding met uw Azure Storage-account beperken geïmplementeerd. Als u ervoor kiest deze functie wilt gebruiken met een Azure Storage-account dat wordt gebruikt door Azure SQL-Server, kunt u problemen ondervindt. Vervolgens wordt een lijst met en een beschrijving van de Azure SQL Database en Azure SQL Data Warehouse-functies die worden beïnvloed door dit.

### <a name="azure-sql-data-warehouse-polybase"></a>Azure SQL Data Warehouse PolyBase

PolyBase wordt meestal gebruikt om gegevens te laden in Azure SQL Data Warehouse vanuit Azure Storage-accounts. Als de Azure Storage-account dat u het laden van gegevens van alleen toegang tot een set van VNet-subnetten limieten, wordt verbinding hebben met PolyBase aan het Account verbroken. Voor het inschakelen van beide PolyBase importeren en exporteren van scenario's met Azure SQL Data Warehouse verbinding met Azure Storage die wordt beveiligd met VNet, volgt u de stappen die hieronder worden beschreven:

#### <a name="prerequisites"></a>Vereisten
1.  Installeer Azure PowerShell volgens dit [handleiding](https://docs.microsoft.com/powershell/azure/install-az-ps).
2.  Als u een voor algemeen gebruik v1- of blob storage-account hebt, moet u eerst upgraden naar algemeen gebruik v2 met behulp van dit [handleiding](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade).
3.  U moet hebben **vertrouwde Microsoft-services voor toegang tot dit storage-account toestaan** onder Azure Storage-account ingeschakeld **Firewalls en virtuele netwerken** instellingenmenu. Verwijzen naar dit [handleiding](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) voor meer informatie.
 
#### <a name="steps"></a>Stappen
1.  In PowerShell **de logische SQL-Server registreren** met Azure Active Directory (AAD):

    ```powershell
    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId your-subscriptionId
    Set-AzureRmSqlServer -ResourceGroupName your-logical-server-resourceGroup -ServerName your-logical-servername -AssignIdentity
    ```
    
 1. Maak een **voor algemeen gebruik v2-Opslagaccount** met behulp van dit [handleiding](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).

    > [!NOTE]
    > - Als u een voor algemeen gebruik v1- of blob storage-account hebt, moet u **eerst upgraden naar v2** met behulp van dit [handleiding](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade).
    > - Voor bekende problemen met Azure Data Lake Storage Gen2 Raadpleeg dit [handleiding](https://docs.microsoft.com/azure/storage/data-lake-storage/known-issues).
    
1.  Onder uw storage-account, gaat u naar **Access Control (IAM)**, en klikt u op **roltoewijzing toevoegen**. Toewijzen **Gegevensbijdrager voor Blob (Preview)** RBAC-rol met de logische SQL-Server.

    > [!NOTE] 
    > Alleen leden met de eigenaar van bevoegdheden kunnen deze stap uitvoeren. Voor de verschillende ingebouwde rollen voor Azure-resources, verwijzen naar dit [handleiding](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).
  
1.  **Polybase-verbinding met de Azure Storage-account:**

    1. Maak een database **[hoofdsleutel](https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql?view=sql-server-2017)** als u een eerder hebt gemaakt:
        ```SQL
        CREATE MASTER KEY [ENCRYPTION BY PASSWORD = 'somepassword'];
        ```
    
    1. Maken van database-scoped referentie met **id = 'Beheerde Service-identiteit'**:

        ```SQL
        CREATE DATABASE SCOPED CREDENTIAL msi_cred WITH IDENTITY = 'Managed Service Identity';
        ```
        > [!NOTE] 
        > - Er is niet nodig om op te geven van GEHEIM met de Azure Storage-toegangssleutel omdat dit mechanisme gebruikt [beheerde identiteit](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) op de achtergrond.
        > - Identiteitsnaam van de moet **'Beheerde Service-identiteit'** voor PolyBase-verbinding met het werken met Azure Storage-account is beveiligd met VNet.    
    
    1. Externe gegevensbron maken met abfss: / / kleurenschema voor de verbinding met uw opslagaccount voor algemeen gebruik v2 met PolyBase:

        ```SQL
        CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss WITH (TYPE = hadoop, LOCATION = 'abfss://myfile@mystorageaccount.dfs.core.windows.net', CREDENTIAL = msi_cred);
        ```
        > [!NOTE] 
        > - Als u al externe tabellen die zijn gekoppeld aan voor algemeen gebruik v1- of blob storage-account hebt, moet u eerst verwijderen die externe tabellen en zet de bijbehorende externe gegevensbron. Maak vervolgens de externe gegevensbron met abfss: / / verbinding maken met het opslagaccount voor algemeen gebruik v2 als hierboven-schema en de externe tabellen met behulp van deze nieuwe externe gegevensbron opnieuw te maken. U kunt [genereren en Scripts Wizard publiceren](https://docs.microsoft.com/sql/ssms/scripting/generate-and-publish-scripts-wizard?view=sql-server-2017) maken-scripts voor de externe tabellen voor een eenvoudige genereren.
        > - Voor meer informatie over abfss: / / -schema, verwijzen naar dit [handleiding](https://docs.microsoft.com/azure/storage/data-lake-storage/introduction-abfs-uri).
        > - Voor meer informatie over CREATE EXTERNAL DATA SOURCE verwijzen naar dit [handleiding](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql).
        
    1. Query als normale [externe tabellen](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql).

### <a name="azure-sql-database-blob-auditing"></a>Controlefunctie voor Azure SQL Database

Auditlogboeken controlefunctie voor BLOBs worden verstuurd naar uw eigen opslagaccount. Connectiviteit van Azure SQL Database naar het opslagaccount dat wordt verbroken als u dit storage-account maakt gebruik van de functie voor de VNet-Service-eindpunten.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Een VNet-firewallregel toevoegen aan uw server zonder in te schakelen op VNet-Service-eindpunten

Lang geleden, voordat dit onderdeel is verbeterd, moest u VNet-service-eindpunten inschakelen voordat u een live VNet-regel in de Firewall kan implementeren. De eindpunten die betrekking hebben een bepaald VNet-subnet naar een Azure SQL-Database. Maar nu vanaf januari 2018, u kunt omzeilen deze vereiste door in te stellen de **IgnoreMissingVNetServiceEndpoint** vlag.

Alleen instelling een firewallregel niet om de server beveiligen. U moet ook VNet-service-eindpunten op inschakelen om de beveiliging van kracht. Wanneer u service-eindpunten inschakelen, ondervindt uw VNet-subnet uitvaltijd totdat de bewerking is de overgang van uitgeschakeld naar op. Dit geldt met name in de context van grote VNets. U kunt de **IgnoreMissingVNetServiceEndpoint** vlag te verlagen of elimineren van de uitvaltijd tijdens de overgang.

U kunt instellen dat de **IgnoreMissingVNetServiceEndpoint** vlag met behulp van PowerShell. Zie voor meer informatie, [PowerShell voor het maken van een service-eindpunt voor Virtueelnetwerk en een regel voor Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

## <a name="errors-40914-and-40615"></a>Fouten 40914 en 40615

Er is een verbindingsfout 40914 gekoppeld aan *virtuele netwerkregels*, zoals opgegeven in het deelvenster Firewall in Azure portal. Fout 40615 is vergelijkbaar, behalve deze zich tot verhoudt *IP-adresregels* op de Firewall.

### <a name="error-40914"></a>Fout 40914

*Tekst van:* Server kan niet worden geopend '*[servernaam]* is aangevraagd door de aanmelding. Client is niet toegestaan voor toegang tot de server.

*Foutbeschrijving:* De client zich in een subnet met de eindpunten van virtual network-server. Maar de Azure SQL Database-server heeft geen regel voor virtuele netwerken die verleent aan het subnet van het recht om te communiceren met de SQL-Database.

*Fout-oplossing:* De Firewall in het deelvenster van de Azure-portal, gebruiken het besturingselement in virtueel netwerk regels op [een regel voor het virtuele netwerk toevoegen](#anchor-how-to-by-using-firewall-portal-59j) voor het subnet.

### <a name="error-40615"></a>Fout 40615

*Tekst van:* Server kan niet worden geopend '{0}' aangevraagd door de aanmelding. De client met IP-adres{1}' is niet toegestaan voor toegang tot de server.

*Foutbeschrijving:* De client probeert verbinding maken vanaf een IP-adres dat is niet geautoriseerd voor verbinding met de Azure SQL Database-server. De firewall van de server heeft geen IP-adres-regel waarmee een client voor de communicatie van de opgegeven IP-adres met de SQL-Database.

*Fout-oplossing:* Voer het IP-adres van de client als een IP-regel. Dit doen met behulp van het deelvenster Firewall in Azure portal.

Een lijst met foutberichten voor verschillende SQL-Database wordt gedocumenteerd [hier][sql-database-develop-error-messages-419g].

<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="portal-can-create-a-virtual-network-rule"></a>Portal kunt maken van een regel voor virtuele netwerken

Deze sectie wordt beschreven hoe u kunt de [Azure-portal] [ http-azure-portal-link-ref-477t] maken een *regel voor virtuele netwerken* in uw Azure SQL Database. De regel wordt de SQL-Database communicatie accepteert van een bepaald subnet dat is gemarkeerd als een *service-eindpunt voor Virtueelnetwerk*.

> [!NOTE]
> Als u van plan bent een service-eindpunt toevoegen aan de VNet-firewallregels van uw Azure SQL Database-server, eerst voor zorgen dat service eindpunten zijn ingeschakeld voor het subnet.
>
> Als service-eindpunten zijn niet ingeschakeld voor het subnet, de portal u wordt gevraagd in te schakelen. Klik op de **inschakelen** knop op de dezelfde blade waarop u de regel toevoegen.

## <a name="powershell-alternative"></a>Alternatieve van PowerShell

Een PowerShell-script kunt ook regels voor virtueel netwerk maken. De essentiële cmdlet **New-azurermsqlservervirtualnetworkrule toegevoegd om**. Als u nodig hebt, Zie [PowerShell voor het maken van een service-eindpunt voor Virtueelnetwerk en een regel voor Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

## <a name="rest-api-alternative"></a>Alternatieve REST-API

Intern maakt aanroept de PowerShell-cmdlets voor acties van de SQL-VNet REST API's. U kunt de REST API's rechtstreeks aanroepen.

- [Regels voor virtueel netwerk: Operations][rest-api-virtual-network-rules-operations-862r]

## <a name="prerequisites"></a>Vereisten

U moet al een subnet dat is gemarkeerd met het specifieke service-eindpunt voor Virtueelnetwerk hebben *typenaam* relevant zijn voor Azure SQL Database.

- De naam van het relevante eindpunt is **Microsoft.Sql**.
- Als uw subnet kan niet worden gelabeld met de naam van het type, Zie [controleren of uw subnet is een eindpunt][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

## <a name="azure-portal-steps"></a>Stappen voor Azure portal

1. Meld u aan bij [Azure Portal][http-azure-portal-link-ref-477t].

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

7. Zie de regel voor de resulterende virtuele netwerken in het deelvenster firewall.

    ![Zie de nieuwe regel in het deelvenster firewall.][image-portal-firewall-vnet-result-rule-30-png]

> [!NOTE]
> De volgende statussen of statussen van toepassing op de regels:
> - **Gereed:** Geeft aan dat de bewerking die u hebt gestart is voltooid.
> - **Is mislukt:** Geeft aan dat de bewerking die u hebt gestart, is mislukt.
> - **Verwijderd:** Alleen van toepassing op de Delete-bewerking, en geeft aan dat de regel is verwijderd en niet langer van toepassing.
> - **InProgress:** Geeft aan dat de bewerking uitgevoerd wordt. De oude regel van toepassing is terwijl de bewerking in deze status is.

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

<!-- Link references, to text, Within this same GitHub repo. -->

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

<!-- Link references, to text, Outside this GitHub repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/

[rest-api-virtual-network-rules-operations-862r]: https://docs.microsoft.com/rest/api/sql/virtualnetworkrules

<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->
