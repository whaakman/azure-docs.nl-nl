---
title: Virtueel netwerk service-eindpunten en regels voor Azure SQL Database | Microsoft Docs
description: Een subnet markeren als een service-eindpunt van het virtuele netwerk. Vervolgens het eindpunt als een virtueel netwerk regel aan de ACL van uw Azure SQL Database. U-SQL-Database vervolgens accepteert communicatie van alle virtuele machines en andere knooppunten in het subnet.
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: VNet Service endpoints
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 01/23/2018
ms.author: genemi
ms.openlocfilehash: 6294216568e1d4c50ef6e6b6d2348a2a221406b0
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-sql-database"></a>Gebruik Virtual Network service-eindpunten en regels voor Azure SQL Database

*Virtueel netwerk regels* zijn een functie van de firewall beveiliging die bepaalt of de server van uw Azure SQL Database accepteert communicaties die van bepaalde subnetten in virtuele netwerken worden verzonden. Dit artikel wordt uitgelegd waarom de functie van de regel virtueel netwerk is het soms de beste optie voor het toestaan van veilige communicatie met uw Azure SQL Database.

Een regel virtueel netwerk maken eerst moet er een [virtueel netwerk service-eindpunt] [ vm-virtual-network-service-endpoints-overview-649d] voor de regel om te verwijzen.


> [!NOTE]
> Deze functie is beschikbaar in het voorbeeld in alle regio's van openbare Azure-cloud voor Azure SQL Database.

#### <a name="how-to-create-a-virtual-network-rule"></a>De regel van een virtueel netwerk maken

Als u alleen de regel van een virtueel netwerk maakt, kunt u verder gaan naar de stappen en uitleg [verderop in dit artikel](#anchor-how-to-by-using-firewall-portal-59j).






<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologie en -beschrijving

**Virtueel netwerk:** kunt u virtuele netwerken die zijn gekoppeld aan uw Azure-abonnement hebt.

**Subnet:** een virtueel netwerk bevat **subnetten**. Eventuele Azure virtuele machines (VM's) die u hebt toegewezen aan subnetten. Eén subnet kan meerdere virtuele machines of andere rekenknooppunten bevatten. COMPUTE knooppunten die zich buiten het virtuele netwerk geen toegang tot het virtuele netwerk tenzij u de beveiliging voor toegang configureren.

**Virtueel netwerk service-eindpunt:** A [Virtual Network service-eindpunt] [ vm-virtual-network-service-endpoints-overview-649d] een subnet waarvan de eigenschapwaarden een of meer namen van formele Azure-service bevatten. In dit artikel we geïnteresseerd bent in de typenaam van **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database.

**Virtueel netwerk regel:** een virtueel netwerk-regel voor de SQL-Database-server is een subnet op dat wordt vermeld in de toegangsbeheerlijst (ACL) van uw SQL-Database-server. Als u in de ACL voor de SQL-Database, het subnet moet bevatten de **Microsoft.Sql** typenaam.

Een virtueel netwerk regel geeft aan dat uw SQL Database-server te accepteren van communicatie van elk knooppunt in het subnet.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Voordelen van een virtueel netwerk-regel

Totdat u actie onderneemt, wordt de virtuele machines op de subnetten kunnen niet communiceren met uw SQL-Database. Een actie die de communicatie is het maken van een regel van het virtuele netwerk. De logica voor het kiezen van de aanpak van de regel VNet vereist een vergelijken en contrast discussie met betrekking tot de concurrerende beveiligingsopties die worden aangeboden door de firewall.

#### <a name="a-allow-access-to-azure-services"></a>A. Toegang tot Azure-services toestaan

Het deelvenster firewall heeft een **aan/uit** knop met de naam **toegang tot Azure-services toestaan**. De **ON** instelling kan communicatie van alle Azure-IP-adressen en alle Azure-subnetten. Deze Azure-IP-adressen of subnetten mogelijk geen eigendom van u. Dit **ON** instelling openstaat waarschijnlijk meer dan u wilt dat de SQL-Database te zijn. De functie van de regel virtueel netwerk biedt veel betere resultaten gedetailleerde controle.

#### <a name="b-ip-rules"></a>B. IP-regels

De SQL-Database-firewall kunt u IP-adresbereiken van waaruit de communicatie worden geaccepteerd in SQL-Database opgeven. Deze methode werkt goed voor stabiele IP-adressen die zich buiten het particuliere netwerk van Azure. Maar veel knooppunten in het persoonlijke Azure-netwerk zijn geconfigureerd met *dynamische* IP-adressen. Dynamische IP-adressen kunnen wijzigen, zoals wanneer uw virtuele machine opnieuw is opgestart. Het zou fraai om op te geven van een dynamische IP-adres in een firewallregel in een productieomgeving zijn.

U kunt de IP-optie restwaarde door het verkrijgen van een *statische* IP-adres voor uw virtuele machine. Zie voor meer informatie [privé IP-adressen voor een virtuele machine configureren via de Azure-portal][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Echter, de statische IP-benadering kan moeilijk te beheren, en is kostbaar wanneer u klaar bent op grote schaal. Virtueel netwerk regels zijn gemakkelijker te maken en te beheren.

#### <a name="c-cannot-yet-have-sql-database-on-a-subnet"></a>C. Kan niet nog SQL-Database op een subnet

Als uw Azure SQL Database-server een knooppunt in een subnet van het virtuele netwerk is, worden alle knooppunten in het virtuele netwerk kunnen communiceren met uw SQL-Database. In dit geval kunnen uw virtuele machines communiceren met de SQL-Database zonder een virtueel netwerk regels of IP-regels.

Echter vanaf September 2017, de Azure SQL Database-service nog niet tussen de services die kunnen worden toegewezen aan een subnet.






<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Meer informatie over de regels voor virtueel netwerk

Deze sectie beschrijft de verschillende details over de regels voor virtueel netwerk.

#### <a name="only-one-geographic-region"></a>Slechts één geografische regio

Elk virtueel netwerk service-eindpunt is van toepassing op slechts één Azure-regio. Het eindpunt is niet ingeschakeld voor andere regio's communicatie accepteert van het subnet.

Elke regel van het virtuele netwerk is beperkt tot de regio die het onderliggende eindpunt is van toepassing op.

#### <a name="server-level-not-database-level"></a>Niveau van de server, niet databaseniveau

Elke regel van het virtuele netwerk geldt voor uw hele Azure SQL Database-server, niet alleen op een bepaalde database op de server. Met andere woorden, virtueel netwerk regel wordt toegepast op de server-niveau, niet op het database-niveau.

- IP-regels kunnen daarentegen van toepassing op beide niveaus.

#### <a name="security-administration-roles"></a>Beveiligingsrollen voor beheer

Er is een scheiding van beveiligingsrollen in het beheer van virtueel netwerk service-eindpunten. Actie is vereist van elk van de volgende rollen:

- **Netwerk-Admin:** &nbsp; schakelt u het eindpunt.
- **Database-Admin:** &nbsp; bijwerken van de toegangsbeheerlijst (ACL) aan het opgegeven subnet toevoegen aan de SQL-Database-server.

*RBAC alternatief:*

De functies van netwerk-beheerder en Database Admin beschikken over meer mogelijkheden dan nodig zijn voor het beheren van virtueel netwerk regels. Er is slechts een subset van hun mogelijkheden nodig.

U hebt de optie voor het gebruik van [op rollen gebaseerde toegangsbeheer (RBAC)] [ rbac-what-is-813s] in Azure maken van een enkele aangepaste functie die alleen de benodigde subset van de mogelijkheden heeft. De aangepaste rol kan worden gebruikt in plaats van met betrekking tot de netwerk-beheerder of de Database-beheerder. Het serveroppervlaktegebied van uw beveiligingsrisico is lager als u een gebruiker aan een aangepaste beveiligingsrol toevoegt, ten opzichte van de gebruiker toe te voegen aan de andere twee belangrijke beheerdersrollen.

> [!NOTE]
> In sommige gevallen zijn de Azure SQL Database en het VNet-subnet in verschillende abonnementen behoren. In dergelijke gevallen moet u ervoor zorgen dat de volgende configuraties:
> - Beide abonnementen moeten in dezelfde Azure Active Directory-tenant.
> - De gebruiker heeft de vereiste machtigingen voor het initiëren van bewerkingen, zoals service-eindpunten inschakelen en een VNet-subnet aan de opgegeven Server toe te voegen.

## <a name="limitations"></a>Beperkingen

De functie van de regels voor virtueel netwerk heeft voor Azure SQL Database, de volgende beperkingen:

- Op dit moment, een Azure-Web-App in een subnet die heeft **Service-eindpunten** ingeschakeld heeft geen nog functie zoals verwacht. We werken aan deze functionaliteit in te schakelen.
    - Tot deze functie volledig is geïmplementeerd, wordt u aangeraden dat u uw Web-App hebt verplaatst naar een ander subnet waarop nog geen service-eindpunten die zijn ingeschakeld voor SQL.

- In de firewall voor de SQL-Database, is elke regel virtueel netwerk verwijst naar een subnet. Alle deze subnetten waarnaar wordt verwezen, moeten worden gehost in dezelfde geografische regio die als host fungeert voor de SQL-Database.

- Elke Azure SQL Database-server kan maximaal 128 ACL-vermeldingen voor een bepaald virtueel netwerk hebben.

- Virtueel netwerk regels alleen van toepassing op virtuele netwerken van Azure Resource Manager; en niet op [klassieke implementatiemodel] [ arm-deployment-model-568f] netwerken.

- Inschakelen van virtueel netwerk service-eindpunten met Azure SQL Database kunt ook de eindpunten voor de MySQL en PostGres Azure-services. Met de eindpunten van mislukt verbindingspogingen met uw MySQL of Postgres exemplaren van de eindpunten echter.
    - De onderliggende reden is dat MySQL en PostGres bieden momenteel geen ondersteuning voor ACLing.

- Op de firewall IP-adresbereiken zijn van toepassing op de volgende items voor netwerken, maar niet het virtuele netwerk regels:
    - [Site-naar-Site (S2S) virtueel particulier netwerk (VPN)][vpn-gateway-indexmd-608y]
    - On-premises via [ExpressRoute][expressroute-indexmd-744v]

#### <a name="expressroute"></a>ExpressRoute

Als uw netwerk is verbonden met het Azure-netwerk door het gebruik van [ExpressRoute][expressroute-indexmd-744v], elk circuit is geconfigureerd met twee openbare IP-adressen op de Microsoft Edge. De twee IP-adressen worden gebruikt om verbinding met Microsoft-Services, zoals naar Azure Storage via openbare Azure-Peering.

Als u communicatie van uw circuit naar Azure SQL Database, moet u IP-netwerk regels voor het openbare IP-adressen van uw circuits maken. Om te zoeken naar het openbare IP-adressen van uw ExpressRoute-circuit, moet u een ondersteuningsticket opent met ExpressRoute met behulp van de Azure-portal.


<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## <a name="impact-of-removing-allow-all-azure-services"></a>Gevolgen van het verwijderen van 'Alle Azure-Services toestaan'

Veel gebruikers wilt verwijderen **alle Azure-Services toestaan** van hun Azure SQL-Servers en vervang deze door een firewallregel VNet.
Echter verwijderen van dit van invloed op de volgende Azure SQLDB functies:

#### <a name="import-export-service"></a>De Export-Service importeren
Azure SQLDB importeren exporteren Service wordt uitgevoerd op virtuele machines in Azure. Deze virtuele machines zijn niet in uw VNet en daarom een Azure IP-adres ophalen bij het verbinden met uw database. Over het verwijderen van **alle Azure-Services toestaan** deze virtuele machines kan niet worden voor toegang tot uw databases.
U kunt het probleem omzeilen. Uitvoeren van het Bacpac-importeren of exporteren rechtstreeks in uw code met behulp van de API DACFx. Zorg ervoor dat dit wordt geïmplementeerd in een virtuele machine die zich in de VNet-subnet waarvoor u de firewallregel hebt ingesteld.

#### <a name="sql-database-query-editor"></a>SQL Database-Query-Editor
De Query-Editor van Azure SQL Database wordt geïmplementeerd op virtuele machines in Azure. Deze virtuele machines zijn niet in uw VNet. De virtuele machines u daarom een Azure IP-adres om verbinding te maken met uw database. Over het verwijderen van **alle Azure-Services toestaan**, deze virtuele machines kan niet worden voor toegang tot uw databases.

#### <a name="table-auditing"></a>Controle van de tabel
Op dit moment zijn er twee manieren inschakelen van controle op de SQL-Database. Tabel controle mislukt nadat u service-eindpunten hebt ingeschakeld op uw Azure SQL-Server. Risicobeperking hier is te verplaatsen naar Auditingfunctie voor blobs.


## <a name="impact-of-using-vnet-service-endpoints-with-azure-storage"></a>Gevolgen van het Service-eindpunten VNet gebruiken met Azure storage

Azure-opslag is geïmplementeerd de dezelfde functie waarmee u verbinding met uw opslagaccount te beperken.
Als u deze functie wilt gebruiken met een opslagaccount die wordt gebruikt door een Azure SQL Server kiest, kunt u uitvoeren in de problemen. Vervolgens vindt u een beschrijving van de Azure-SQLDB-functies die worden beïnvloed door dit.

#### <a name="azure-sqldw-polybase"></a>Azure SQLDW PolyBase
PolyBase wordt meestal gebruikt om gegevens te laden in Azure SQLDW van Storage-accounts. Als het opslagaccount dat u bij het laden van gegevens van alleen toegang tot een set VNet-subnetten limieten, verbreekt de verbinding tussen PolyBase en het Account.

#### <a name="azure-sqldb-blob-auditing"></a>Azure SQLDB Blob controle
Controlelogboeken auditingfunctie voor BLOBs worden verstuurd naar uw eigen opslagaccount. Verbinding tussen Azure SQLDB en het opslagaccount worden verbroken als dit opslagaccount gebruikmaakt van de functie VENTILATOR Service-eindpunten.


## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Een VNET-firewallregel toevoegen aan een server zonder in te schakelen op VNET Service-eindpunten

Lang geleden voordat dit onderdeel is verbeterd, u vereiste zou u VNet service-eindpunten op voordat u een live VNet regel in de Firewall kan implementeren. De eindpunten gerelateerd een bepaald VNet subnet met een Azure SQL-Database. Maar nu vanaf januari 2018, u kunt omzeilen deze vereiste door in te stellen de **IgnoreMissingServiceEndpoint** vlag.

Alleen instelling een firewallregel niet helpt de server beveiligen. U moet de service-eindpunten VNet ook inschakelen voor de beveiligingsupdate van kracht te laten. Wanneer u de service-eindpunten inschakelt, wordt in uw VNet-subnet uitvaltijd ervaringen totdat de overgang van uitgeschakeld naar is voltooid op. Dit geldt met name in de context van grote VNets. U kunt de **IgnoreMissingServiceEndpoint** vlag wilt verlagen of elimineren van de uitvaltijd tijdens de overgang.

U kunt instellen de **IgnoreMissingServiceEndpoint** vlag met behulp van PowerShell. Zie voor meer informatie [PowerShell voor het maken van een virtueel netwerk service-eindpunt en een regel voor Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].


## <a name="errors-40914-and-40615"></a>Fouten 40914 en 40615

Er is een verbindingsfout 40914 gekoppeld aan *virtueel netwerk regels*, zoals opgegeven in het deelvenster Firewall in de Azure-portal. Fout 40615 lijkt, maar deze zich tot verhoudt *IP-adresregels* op de Firewall.

#### <a name="error-40914"></a>Error 40914

*Berichttekst:* server kan niet worden geopend '*[servernaam]*is aangevraagd door de aanmelding. Client is niet toegestaan voor toegang tot de server.

*Foutbeschrijving:* de client zich in een subnet met de server-eindpunten in virtueel netwerk. Maar de Azure SQL Database-server heeft geen virtueel netwerk-regel waarmee het recht om te communiceren met de SQL-Database wordt verleend aan het subnet.

*Fout oplossen:* deelvenster van de Azure portal, de regels van het virtuele netwerk moet worden besturingselement gebruikt op de Firewall [een regel voor het virtuele netwerk toevoegen](#anchor-how-to-by-using-firewall-portal-59j) voor het subnet.

#### <a name="error-40615"></a>Fout 40615

*Berichttekst:* server '{0}' is aangevraagd door de aanmelding kan niet worden geopend. Client met IP-adres '{1}' is niet toegestaan voor toegang tot de server.

*Foutbeschrijving:* de client verbinding probeert te maken van een IP-adres dat niet is geautoriseerd voor verbinding met de Azure SQL Database-server. De firewall van de server heeft geen IP-adres-regel waarmee een client van het opgegeven IP-adres communiceren met de SQL-Database.

*Fout oplossen:* IP-adres van de client als een IP-regel invoeren. Dit doen met behulp van het deelvenster Firewall in de Azure-portal.


Een lijst met verschillende foutberichten voor SQL-Database is gedocumenteerd [hier][sql-database-develop-error-messages-419g].





<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="portal-can-create-a-virtual-network-rule"></a>Portal kunt een regel virtueel netwerk maken

Deze sectie wordt beschreven hoe u kunt de [Azure-portal] [ http-azure-portal-link-ref-477t] maken een *virtueel netwerk regel* in uw Azure SQL Database. De regel geeft aan dat de SQL-Database communicatie accepteert van een bepaald subnet die zijn gelabeld als een *Virtual Network service-eindpunt*.

#### <a name="powershell-alternative"></a>Alternatieve PowerShell

Een PowerShell-script kunt ook regels virtueel netwerk maken. De cmdlet cruciaal **nieuw AzureRmSqlServerVirtualNetworkRule**. Als u geïnteresseerd zijn, raadpleegt u [PowerShell voor het maken van een virtueel netwerk service-eindpunt en een regel voor Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

#### <a name="prerequisites"></a>Vereisten

U moet al een subnet op dat is gemarkeerd met het desbetreffende Virtual Network service-eindpunt hebben *typenaam* relevant zijn voor Azure SQL Database.

- De typenaam van de relevante eindpunt **Microsoft.Sql**.
- Als uw subnet kan niet worden gemarkeerd met de naam van het type, Zie [controleren of uw subnet is een eindpunt][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

### <a name="azure-portal-steps"></a>Stappen voor Azure portal

1. Meld u aan bij de [Azure-portal][http-azure-portal-link-ref-477t].

2. Navigeert u vervolgens de portal voor **SQL-servers** &gt; **Firewall / virtuele netwerken**.

3. Stel de **toegang tot Azure-services toestaan** besturingselement op OFF.

    > [!IMPORTANT]
    > Als u het besturingselement dat is ingesteld op ON laat, accepteert enkel subnet communicatie van uw Azure SQL Database-server. Als u het besturingselement dat is ingesteld op ON is mogelijk overmatige toegang vanuit een oogpunt van beveiliging. De functie Microsoft Azure Virtual Network service-eindpunt, in coördinatie met de functie van de regel virtueel netwerk van de SQL-Database, kan samen het serveroppervlaktegebied van de beveiliging verminderen.

4. Klik op de **+ toevoegen bestaande** beheren in de **virtuele netwerken** sectie.

    ![Klik op toevoegen aan bestaande (als een SQL-regel subnet eindpunt).][image-portal-firewall-vnet-add-existing-10-png]

5. In het nieuwe **Create/Update** deelvenster vulling in de besturingselementen die met de namen van uw Azure-resources.

    > [!TIP]
    > U moet de juiste opnemen **adresvoorvoegsel** voor uw subnet. U vindt de waarde in de portal.
    > Navigeer **alle resources** &gt; **alle typen** &gt; **virtuele netwerken**. Uw virtuele netwerken worden weergegeven. Klik op het virtuele netwerk en klik vervolgens op **subnetten**. De **ADRESBEREIK** kolom heeft het adresvoorvoegsel dat u nodig hebt.

    ![Vul de velden voor nieuwe regel.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Klik op de **OK** knop aan de onderkant van het deelvenster.

7.  Zie de resulterende virtuele netwerk regel in het deelvenster firewall.

    ![Zie de nieuwe regel in het deelvenster firewall.][image-portal-firewall-vnet-result-rule-30-png]


> [!NOTE]
> De volgende statussen of statussen van toepassing op de regels:
> - **Ready:** geeft aan dat de bewerking die u hebt gestart is voltooid.
> - **Is mislukt:** geeft aan dat de bewerking die u hebt gestart is mislukt.
> - **Verwijderd:** alleen toegepast op de bewerking Delete, en geeft aan dat de regel is verwijderd en niet langer van toepassing.
> - **InProgress:** geeft aan dat de bewerking uitgevoerd wordt. De oude regel van toepassing tijdens de bewerking in deze status is.


<a name="anchor-how-to-links-60h" />

## <a name="related-articles"></a>Verwante artikelen:

- [Virtueel netwerk van Azure service-eindpunten][vm-virtual-network-service-endpoints-overview-649d]
- [Azure SQL Database-server- en databaseniveau firewall-regels][sql-db-firewall-rules-config-715d]

De functie van de regel virtueel netwerk voor Azure SQL Database is beschikbaar in latere September 2017 geworden.

## <a name="next-steps"></a>Volgende stappen

- [PowerShell gebruiken voor het maken van een service-eindpunt van het virtuele netwerk en de regel van een virtueel netwerk voor Azure SQL Database.][sql-db-vnet-service-endpoint-rule-powershell-md-52d]


<!-- Link references, to images. -->

[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png

[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png

[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png



<!-- Link references, to text, Within this same Github repo. -->

[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[expressroute-indexmd-744v]: ../expressroute/index.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[sql-db-firewall-rules-config-715d]: sql-database-firewall-configure.md

[sql-database-develop-error-messages-419g]: sql-database-develop-error-messages.md

[sql-db-vnet-service-endpoint-rule-powershell-md-52d]: sql-database-vnet-service-endpoint-rule-powershell.md

[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]: sql-database-vnet-service-endpoint-rule-powershell.md#a-verify-subnet-is-endpoint-ps-100

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[vm-virtual-network-service-endpoints-overview-649d]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.md



<!-- Link references, to text, Outside this Github repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/




<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->
