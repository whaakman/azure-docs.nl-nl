---
title: Aan de slag met Microsoft Azure-beveiliging | Microsoft Docs
description: Dit artikel bevat een overzicht van Microsoft Azure-beveiligingsmogelijkheden en algemene overwegingen voor organisaties die hun activa naar een cloudprovider migreert.
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 8d8a0088-c85a-48e7-bd04-2bc7b78b0691
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: yurid
ms.openlocfilehash: eb53ed852b6175fbc7faea44a243e8c7d5ce1753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-microsoft-azure-security"></a>Aan de slag met de beveiliging in Microsoft Azure
Wanneer u bouwen of IT-middelen naar een cloudprovider migreren, worden de mogelijkheden van die organisatie aan uw toepassingen en gegevens beschermen met de services en de besturingselementen die ze bieden voor het beheren van de beveiliging van uw assets cloud-gebaseerde afhankelijk.

De infrastructuur van Azure is zo ontworpen dat toepassingen miljoenen klanten tegelijkertijd kunnen hosten en er daarnaast een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan hun beveiligingsbehoeften. Bovendien biedt Azure u een scala aan configureerbare beveiligingsopties en de mogelijkheid om deze te beheren, zodat u de beveiliging kunt afstemmen op de unieke vereisten van uw implementaties.

In dit overzichtsartikel over de beveiliging in Azure komen de volgende onderwerpen aan de orde:

* Azure-services en functies die u gebruiken kunt om uw services en de gegevens in Azure te beveiligen.
* Microsoft beveiligt hoe de Azure-infrastructuur ter bescherming van uw gegevens en toepassingen.

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer
Het controleren van de toegang tot de IT-infrastructuur, gegevens en toepassingen is van essentieel belang. Microsoft Azure biedt deze mogelijkheden door services zoals Azure Active Directory (Azure AD), Azure Storage en biedt ondersteuning voor meerdere standaarden en API's.

[Azure AD](../active-directory/active-directory-whatis.md) is een identiteitsopslagplaats en de engine voor verificatie, autorisatie en toegangsbeheer voorziet in een organisatie gebruikers, groepen en objecten. Azure AD biedt ontwikkelaars bovendien een effectieve manier om identiteitsbeheer te integreren in hun toepassingen. Industriestandaard-protocollen, zoals [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx), en [OpenID Connect](http://openid.net/connect/) aanmelden mogelijk maken op platformen, zoals .NET, Java, Node.js en PHP.

De op REST gebaseerde Graph API stelt ontwikkelaars in staat om vanaf elk platform gegevens in de directory te lezen en schrijven. Dankzij de ondersteuning van [OAuth 2.0](http://oauth.net/2/), kunnen ontwikkelaars mobiele en webtoepassingen die zijn geïntegreerd met Microsoft en derden web-API's en bouwen van hun eigen beveiligde web-API's. Er zijn open source-clientbibliotheken beschikbaar voor .Net, Windows Store, iOS en Android, en er worden meer bibliotheken ontwikkeld.

### <a name="how-azure-enables-identity-and-access-management"></a>Identiteits- en toegangsbeheer in Azure
Azure AD kan worden gebruikt als een zelfstandige clouddirectory voor uw organisatie of als een geïntegreerde oplossing met uw bestaande on-premises Active Directory. Sommige integratiefuncties omvatten directory-synchronisatie en eenmalige aanmelding (SSO). Deze het bereik van uw bestaande on-premises identiteiten in de cloud uitbreiden en de beheerder en gebruiker ervaring te verbeteren.

Azure biedt daarnaast onder andere de volgende mogelijkheden voor identiteits- en toegangsbeheer:

* Azure AD maakt eenmalige aanmelding ([SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)) bij SaaS-toepassingen mogelijk, ongeacht waar ze worden gehost. Voor sommige toepassingen kan federatieve aanmelding worden gebruikt via Azure AD en voor andere toepassingen kan eenmalige aanmelding (SSO) met een wachtwoord worden gebruikt. Federatieve toepassingen kunnen ook ondersteuning bieden voor het inrichten van gebruikers en het bewaren van wachtwoorden in een wachtwoordkluis.
* De toegang tot gegevens in [Azure Storage](https://azure.microsoft.com/services/storage/) wordt geregeld via verificatie. Elk opslagaccount is een primaire sleutel ([opslagaccountsleutel](https://msdn.microsoft.com/library/azure/ee460785.aspx), of SAK) en een secundaire geheime sleutel (shared access signature voor, of SAS).
* Azure AD levert de identiteit als via de federation Service met behulp van [Active Directory Federation Services](../active-directory/fundamentals-identity.md), synchronisatie en replicatie met on-premises adreslijsten.
* [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) is van de multi-factor authentication-service waarbij gebruikers aanmeldingen verifiëren met behulp van een mobiele app, telefonische oproep of een tekstbericht. Het kan worden gebruikt met Azure AD om veilige on-premises netwerkbronnen met de Azure multi-factor Authentication-server en aangepaste toepassingen en mappen met de SDK.
* [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kunt u virtuele Azure-machines toevoegen aan een domein zonder het implementeren van domeincontrollers. U kunt aanmelden bij deze virtuele machines met uw bedrijfsreferenties in Active Directory en domein virtuele machines beheren met behulp van Groepsbeleid af te dwingen beveiligingsbasislijnen op uw Azure virtuele machines.
* [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) biedt een maximaal beschikbare globale-identity management-service voor consumententoepassingen voor honderden miljoenen identiteiten. De service kan worden geïntegreerd in zowel mobiele als webplatforms. Uw consumenten zich aanmelden bij uw toepassingen via aanpasbare met behulp van hun bestaande sociale accounts of maken van nieuwe referenties.

## <a name="data-access-control-and-encryption"></a>Gegevenstoegangsbeheer en versleuteling
Bij alle Azure-bewerkingen worden door Microsoft de principes van een strikte scheiding van taken en het toekennen van [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) toegepast. Als Azure-ondersteuningspersoneel toegang wil tot uw gegevens, moet u daar expliciet toestemming voor geven. De toegang wordt verleend op een 'just-in-time'-basis en wordt in een logboek vastgelegd en gecontroleerd en vervolgens weer ingetrokken nadat de werkzaamheden van het ondersteuningspersoneel zijn voltooid.

Azure biedt ook meerdere mogelijkheden voor het beveiligen van gegevens in rust en onderweg. Dit omvat de versleuteling van gegevens, bestanden, toepassingen, services, communicatie en stations. U kunt versleutelen voordat deze in Azure plaatst en sleutels ook opslaan in uw lokale datacenters.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig1.PNG)

### <a name="azure-encryption-technologies"></a>Versleutelingstechnologieën van Azure
U kunt gegevens verzamelen over beheerderstoegang tot uw abonnementsomgeving via [Azure AD-rapportage](../active-directory/active-directory-reporting-audit-events.md). U kunt configureren [BitLocker-stationsversleuteling](https://technet.microsoft.com/library/cc732774.aspx) op VHD's met gevoelige gegevens in Azure.

Daarnaast hebt u onder andere de volgende mogelijkheden om uw gegevens beter te beveiligen in Azure:

* Ontwikkelaars kunnen bouwen versleuteling in de toepassingen die ze in Azure implementeren met behulp van de Windows [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) en .NET Framework.
* Volledig beheer de sleutels met versleuteling aan clientzijde voor Azure Blob-opslag. De sleutels worden nooit bekend bij de opslagsservice en kunnen ook niet worden ontsleuteld in de opslagservice.
* [Azure Rights Management (Azure RMS)](https://technet.microsoft.com/library/jj585026.aspx) (met de [RMS SDK](https://msdn.microsoft.com/library/dn758244.aspx)) biedt bestands- en gegevensniveau codering en gegevenslek preventie tot toegangsbeheer op basis van beleid.
* Azure ondersteunt [op tabelniveau en op kolomniveau codering (TDE/wissen)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) in virtuele machines van SQL Server en het derde ondersteunt lokale Sleutelbeheer servers in datacentra.
* Storage Account Keys, Shared Access Signatures, beheercertificaten en andere sleutels zijn uniek voor elke Azure-tenant.
* Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) hybride opslag versleutelt gegevens via een 128-bits openbaar/persoonlijk sleutelpaar voordat u dit uploadt naar Azure Storage.
* Azure ondersteunt en talrijke versleuteling mechanismen, met inbegrip van SSL/TLS, IPsec en AES, afhankelijk van de gegevenstypen, containers en -transporten gebruikt.

## <a name="virtualization"></a>Virtualisatie
Voor het Azure-platform wordt gebruikgemaakt van een gevirtualiseerde omgeving. Gebruikersexemplaren werkt als zelfstandige virtuele machines die geen toegang tot een fysieke host-server en deze isolatie wordt afgedwongen met behulp van fysieke [processor (ring-0-en ring 3) bevoegdheidsniveaus](https://en.wikipedia.org/wiki/Protection_ring).

Ring 0 is het niveau met de meeste bevoegdheden en ring 3 het niveau met de minste bevoegdheden. Het gastbesturingssysteem wordt uitgevoerd in een minder bevoegdheden Ring 1 en toepassingen worden uitgevoerd in de laagst mogelijke Ring 3. Deze virtualisatie van fysieke resources leidt tot een duidelijke scheiding tussen het gastbesturingssysteem en de hypervisor, wat zorgt voor een extra veiligheidsbarrière tussen de twee.

De Azure-hypervisor fungeert als een micro-kernel en geeft alle hardware toegangsaanvragen van virtuele machines voor gasten op de host voor de verwerking met behulp van een gedeeld geheugen interface VMBus aangeroepen. Dit voorkomt dat gebruikers onbeperkt toegang krijgen tot het systeem om gegevens te lezen en te schrijven of programma’s uit te voeren en vermindert de risico’s die kleven aan het delen van systeemresources.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig2.PNG)

### <a name="how-azure-implements-virtualization"></a>De implementatie van virtualisatie in Azure
Azure maakt gebruik van een firewall hypervisor (pakketfilter) die is geïmplementeerd in de hypervisor en geconfigureerd door een agent fabric-controller. Hiermee worden tenants beter beveiligd tegen onbevoegde toegang. Standaard al het verkeer wordt geblokkeerd als een virtuele machine wordt gemaakt en vervolgens de agent van de domeincontroller fabric configureert voor het pakketfilter toevoegen *regels en uitzonderingen* geautoriseerde verkeer toestaan.

Er zijn twee categorieën regels die hier worden geprogrammeerd:

* **Configuratie of infrastructuur regels machine**: standaard alle communicatie is geblokkeerd. Er zijn uitzonderingen op een virtuele machine te verzenden en ontvangen van DHCP en DNS-verkeer toestaan. Virtuele machines ook verkeer verzenden met het internet 'openbare' en verkeer naar de andere virtuele machines binnen het cluster en de OS-activeringsserver verzenden. De virtuele machines lijst met toegestane uitgaande bestemmingen omvat geen Azure-router subnetten, Azure management back-end en andere eigenschappen van Microsoft.
* **Configuratiebestand van de rol**: Hiermee definieert u de inkomende toegangsbeheerlijsten (ACL's) op basis van de tenant-ServiceModel. Bijvoorbeeld, als een tenant een webfront-end op poort 80 op een bepaalde virtuele machine heeft, Azure opent vervolgens TCP-poort 80 voor alle IP-adressen als u bij het configureren van een eindpunt in de [Azure klassieke implementatiemodel](../azure-resource-manager/resource-manager-deployment-model.md). Als de virtuele machine een back-end- of worker-rol die zijn uitgevoerd heeft, vervolgens wordt de werkrol alleen voor de virtuele machine binnen dezelfde tenant.

## <a name="isolation"></a>Isolatie
Een ander belangrijk cloud beveiligingsvereiste is scheiding om te voorkomen dat onbevoegde en onbedoelde overdracht van gegevens tussen implementaties in een gedeelde architectuur met meerdere tenants.

Azure implementeert [toegangsbeheer netwerk](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) en scheiding via VLAN-isolatie, ACL's, load balancers en IP-filters. Het externe verkeer beperkt inkomende poorten en protocollen op uw virtuele machines die u definieert. Azure netwerk filteren om te voorkomen dat vervalste verkeer en binnenkomende en uitgaande verkeer te beperken tot vertrouwde platform-onderdelen implementeert. Er zijn verkeersstroombeleidsregels geïmplementeerd voor grensbeveiligingsapparaten die het verkeer standaard weigeren.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig3.PNG)

NAT (Network Address Translation) wordt gebruikt om het interne netwerkverkeer te scheiden van het externe verkeer. Intern verkeer is niet extern routeerbaar. [Virtuele IP-adressen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) die extern routeerbaar zijn, worden omgezet in [interne dynamische IP](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx)-adressen die alleen routeerbaar zijn binnen Azure.

Het externe verkeer naar virtuele machines in Azure is met firewall via de ACL's op routers, load balancers, en Layer 3-switches. Alleen specifieke bekende protocollen zijn toegestaan. ACL's voor het beperken van verkeer van virtuele machines voor gasten op andere VLAN's gebruikt voor beheer. Verkeer gefilterd via IP-filters op de host die OS verder beperkt bovendien het verkeer op beide lagen van koppelings- en gegevens.

### <a name="how-azure-implements-isolation"></a>De implementatie van isolatie in Azure
De Azure-Infrastructuurcontroller is verantwoordelijk voor het toewijzen van bronnen van de infrastructuur voor tenant-werkbelastingen en beheert Unidirectioneel communicatie tussen de host en virtuele machines. De Azure hypervisor afgedwongen geheugen en proces scheiding tussen virtuele machines en het veilig routeert netwerkverkeer Gast OS tenants. Azure implementeert ook isolatie voor tenants, opslag en virtuele netwerken.

* Elke Azure AD-tenant is logisch geïsoleerd met behulp van beveiligingsgrenzen.
* Azure storage-accounts zijn uniek voor elk abonnement en toegang moet worden geverifieerd met behulp van de sleutel van een opslagaccount.
* Virtuele netwerken zijn logisch is geïsoleerd door een combinatie van unieke privé IP-adressen, firewalls en IP-ACL's. Load balancers routeren het verkeer naar de juiste tenants op basis van eindpuntdefinities.

## <a name="virtual-networks-and-firewalls"></a>Virtuele netwerken en firewalls
De [gedistribueerde en virtuele netwerken](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) in de help van Azure ervoor dat uw particuliere netwerkverkeer logisch is geïsoleerd van verkeer op andere virtuele netwerken in Azure.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig4.PNG)

Uw abonnement kunt bevatten meerdere geïsoleerde particuliere netwerken (en zijn de firewall, taakverdeling en netwerkadresomzetting).

Azure biedt drie primaire niveaus van netwerkscheiding in elk Azure-cluster om verkeer op een logische manier te scheiden. [Virtual local area network](https://azure.microsoft.com/services/virtual-network/) (VLAN's) worden gebruikt voor het scheiden van klantverkeer van de rest van het Azure-netwerk. De toegang tot het Azure-netwerk van buiten het cluster wordt beperkt door load balancers.

Netwerkverkeer naar en van virtuele machines moet doorgeven aan de virtuele switch van de hypervisor. De IP-filter-component in de hoofdmap OS isoleert de basis-virtuele machine van de virtuele machines voor gasten en de virtuele machines voor gasten van elkaar. Hiermee worden uitgevoerd voor het filteren van verkeer te beperken van communicatie tussen knooppunten van een tenant en het openbare Internet (op basis van de serviceconfiguratie van de klant), ze te scheiden van andere tenants.

Het IP-filter helpt voorkomen dat virtuele machines voor gasten van:

* Vervalste verkeer wordt gegenereerd.
* Verkeer dat niet is geadresseerd aan deze ontvangt.
* Het routeren van verkeer naar beveiligde infrastructuur eindpunten.
* Verzenden of ontvangen van ongeschikte broadcast-verkeer.

U kunt uw virtuele machines naar plaatsen [virtuele netwerken van Azure](https://azure.microsoft.com/documentation/services/virtual-network/). Deze virtuele netwerken zijn vergelijkbaar met de netwerken die u configureert in on-premises omgevingen, waarin ze doorgaans worden gekoppeld aan een virtuele switch. Virtuele machines die zijn verbonden met hetzelfde virtuele netwerk kunnen communiceren met elkaar zonder aanvullende configuratie. U kunt ook verschillende subnetten configureren in uw virtuele netwerk.

U kunt de volgende Azure Virtual Network-technologieën om beveiligde communicatie in het virtuele netwerk te gebruiken:

* [**Netwerkbeveiligingsgroepen (nsg's)**](../virtual-network/virtual-networks-nsg.md). U kunt een NSG aan verkeer voor beheer op een of meer exemplaren van virtuele machine gebruiken in uw virtuele netwerk. Een netwerkbeveiligingsgroep bevat toegangsbeheerregels waarmee verkeer wordt toegestaan of geweigerd op basis van verkeersrichting, protocol, bronadres en poort en doeladres en poort.
* [**Gebruiker gedefinieerde routering**](../virtual-network/virtual-networks-udr-overview.md). U kunt bepalen de routering van pakketten via een virtueel apparaat door maken gebruiker gedefinieerde routes die de volgende hop voor pakketten naar een specifiek subnet naar een virtueel netwerkapparaat beveiliging opgeeft.
* [**Doorsturen via IP**](../virtual-network/virtual-networks-udr-overview.md). Een apparaat voor de beveiliging van een virtueel netwerk moet in staat zijn om binnenkomend verkeer te ontvangen dat niet aan dat netwerk zelf is geadresseerd. Als u een virtuele machine voor het ontvangen verkeer dat is geadresseerd aan andere bestemmingen, schakelt u doorsturen via IP voor de virtuele machine.
* [**Geforceerde tunneling**](../vpn-gateway/vpn-gateway-about-forced-tunneling.md). Geforceerde tunneling kunt u omleiden of alle internetverkeer gegenereerd door uw virtuele machines in een virtueel netwerk ' force' terug naar uw on-premises locatie via een site-naar-site VPN-tunnel voor inspectie en controle
* [**Eindpunt-ACL's**](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). U kunt bepalen welke computers zijn toegestaan binnenkomende verbindingen via Internet aan een virtuele machine op het virtuele netwerk met het eindpunt-ACL's definiëren.
* [**Beveiligingsoplossingen voor partnernetwerken**](https://azure.microsoft.com/marketplace/). Er zijn een aantal beveiligingsoplossingen van partners netwerk die u vanuit Azure Marketplace openen kunt.

### <a name="how-azure-implements-virtual-networks-and-firewalls"></a>Hoe Azure implementeert virtuele netwerken en firewalls
Azure implementeert filteren van netwerkpakketten firewalls op alle host- en gastverkeer virtuele machines standaard. Installatiekopieën van het Windows-besturingssysteem vanuit Azure Marketplace hebben ook Windows Firewall standaard ingeschakeld. Load balancers in het perimeternetwerk van de communicatie van netwerken in openbare Azure-beheer op basis van IP-ACL's worden beheerd door beheerders van de klant.

Als in Azure als onderdeel van normale bewerkingen of tijdens een noodgeval gegevens van de klant worden verplaatst, gebeurt dat via private, versleutelde communicatiekanalen. Andere mogelijkheden die Azure moet worden gebruikt in virtuele netwerken en firewalls in dienst zijn:

* **Systeemeigen hostfirewall**: Azure Service Fabric en Azure Storage worden uitgevoerd op een eigen besturingssysteem dat geen hypervisor heeft. Daarom worden de windows firewall is geconfigureerd met de vorige twee sets met regels. De Azure-opslag wordt op een eigen besturingssysteem uitgevoerd om de prestaties te optimaliseren.
* **Hostfirewall**: de hostfirewall is het beschermen van het besturingssysteem van de host waarop de hypervisor. De regels worden geprogrammeerd toestaan alleen de Service Fabric-controller en gaan uit om te communiceren met het hostbesturingssysteem op een specifieke poort. De overige uitzonderingen zijn bedoeld om DHCP-reacties en DNS-antwoorden toe te staan. Azure maakt gebruik van een machine-configuratiebestand met de sjabloon van de firewall-regels voor het gastbesturingssysteem. De host zelf is aanvallen van buitenaf beveiligd door een Windows firewall zodanig geconfigureerd dat alleen communicatie van bekende, geverifieerde bronnen.
* **Gast firewall**: de regels in de virtuele machine Switch pakketfilters maar geprogrammeerde in verschillende software (zoals de Windows Firewall-onderdeel van het gastbesturingssysteem) worden gerepliceerd. De virtuele machine Gast firewall kan worden geconfigureerd om communicatie te beperken of naar de virtuele gastmachine, zelfs als de communicatie wordt toegestaan door de configuraties op de host-IP-Filter. U kunt bijvoorbeeld de firewall van de virtuele machine Gast gebruiken voor communicatie tussen twee van de VNets die zijn geconfigureerd om verbinding met elkaar te beperken.
* **Opslag-firewall (FW)**: de firewall op de front-end opslag filtert verkeer alleen op de poorten 80/443 en andere benodigde hulpprogramma-poorten. De firewall op de back-end voor opslag beperkt communicaties afkomstig zijn alleen van opslag-front-endservers.
* **Virtuele netwerkgateway**: de [Azure virtuele netwerkgateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) fungeert als de cross-premises gateway uw workloads in Azure Virtual Network verbinding te maken met uw lokale sites. Dit is vereist voor het verbinding maken met lokale sites via [IPSec-site-naar-site VPN-tunnels](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), of via [ExpressRoute](../expressroute/expressroute-introduction.md) circuits. Voor IPsec/IKE-VPN-tunnels, de gateways IKE handshakes uitvoeren en de IPsec S2S VPN-tunnels tussen virtuele netwerken en lokale sites tot stand brengen. Virtuele netwerkgateways ook beëindigen [punt-naar-site VPN-verbindingen](../vpn-gateway/vpn-gateway-point-to-site-create.md).

## <a name="secure-remote-access"></a>Veilige externe toegang
Voor gegevens die zijn opgeslagen in de cloud, moeten er voldoende veiligheidsvoorzieningen zijn aangebracht om aanvallen te voorkomen en de vertrouwelijkheid en integriteit van de gegevens te bewaren terwijl ze worden verzonden. Hiertoe behoren ook netwerkbeheervoorzieningen die aansluiten bij de op beleid gebaseerde, controleerbare identiteits- en toegangsbeheermechanismen van een organisatie.

Ingebouwde cryptografische technologie stelt u in staat de communicatie te versleutelen binnen en tussen implementaties, tussen Azure-regio's en van Azure naar on-premises datacenters. Beheerderstoegang tot virtuele machines via [extern bureaublad-sessies](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), [externe Windows PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx), en de Azure-portal wordt altijd versleuteld.

Als u wilt uitbreiden veilig uw on-premises datacentrum in de cloud, biedt Azure [site-naar-site VPN](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) en [punt-naar-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md), plus specifieke verbindingen met [ExpressRoute](../expressroute/expressroute-introduction.md) (virtuele netwerken van Azure via VPN worden versleuteld).

### <a name="how-azure-implements-secure-remote-access"></a>De implementatie van veilige externe toegang in Azure
Verbindingen met de Azure-portal moeten altijd worden geverifieerd en ze SSL/TLS nodig hebben. U kunt beheercertificaten configureren om veilig beheer mogelijk te maken. Beveiliging van industriestandaard-protocollen, zoals [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) en [IPsec](https://en.wikipedia.org/wiki/IPsec) worden volledig ondersteund.

Met [Azure ExpressRoute](../expressroute/expressroute-introduction.md) kunt u particuliere verbindingen maken tussen Azure-datacenters en -infrastructuur binnen uw onderneming of in een co-locatie-omgeving. ExpressRoute-verbindingen gaan niet via het openbare internet. Ze bieden meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan typische op Internet gebaseerde koppelingen. In sommige gevallen overbrengen van gegevens tussen on-premises locaties en Azure met behulp van ExpressRoute-verbindingen kan ook aanzienlijke kostenvoordelen opleveren.

## <a name="logging-and-monitoring"></a>Logboekregistratie en bewaking
Azure biedt geverifieerde logboekregistratie van beveiliging relevante gebeurtenissen die een audittrail genereren en het is zodanig ontworpen zijn tegen knoeien. Dit omvat systeeminformatie, zoals het beveiligingslogboek in virtuele machines van Azure-infrastructuur en Azure AD. Gebeurtenis beveiligingsbewaking omvat het verzamelen van gebeurtenissen, zoals wijzigingen in de DHCP- of DNS-server IP-adressen; Er is geprobeerd toegang tot de poorten, protocollen of IP-adressen die zijn geblokkeerd door ontwerp; wijzigingen in beleids- of firewall beveiligingsinstellingen; maken van het account of groep; en onverwachte processen of stuurprogramma-installatiebestand.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started/sec-azgsfig5.PNG)

Controlelogboeken waarin activiteiten van bevoegde gebruikers, geautoriseerde en niet-geautoriseerde pogingen om toegang te krijgen, systeemuitzonderingen en informatiebeveiligingsgebeurtenissen worden geregistreerd, worden slechts een bepaalde tijd bewaard. De bewaartermijn van uw logboekbestanden kunt u zelf bepalen omdat u het verzamelen en bewaren van logboekgegevens naar eigen behoefte kunt configureren.

### <a name="how-azure-implements-logging-and-monitoring"></a>De implementatie van logboekregistratie en bewaking in Azure
In Azure worden MA- (Management Agents) en ASM-agents (Azure Security Monitor) gedistribueerd naar elk reken-, opslag- of infrastructuurknooppunt dat wordt beheerd, ongeacht of het gaat om een systeemeigen of virtueel knooppunt. Elke beheeragent is geconfigureerd om zich te verifiëren bij een serviceteam-opslagaccount met een certificaat dat is verkregen uit de Azure-certificaatopslag en om vooraf geconfigureerde diagnostische en gebeurtenisgegevens door te sturen naar het opslagaccount. Deze agenten worden niet geïmplementeerd op virtuele machines van klanten.

Azure-beheerders krijgen toegang tot de logboeken via een webportal voor geverifieerde en beheerde toegang tot deze logboeken. Een beheerder kan logboeken parseren, filteren, vergelijken en analyseren. De opslagaccounts van het Azure-serviceteam voor logboeken zijn beveiligd tegen directe toegang door de beheerder om te voorkomen dat er onrechtmatige wijzigingen in de logboeken worden aangebracht.

Microsoft verzamelt logboeken van netwerkapparaten met behulp van de Syslog-protocol en van de host-servers met behulp van Microsoft Audit Collection Services (ACS). Deze logboeken worden geplaatst in een logboekdatabase waarin waarschuwingen naar verdachte gebeurtenissen worden gegenereerd. De beheerder kan deze logboeken openen en analyseren.

[Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx) is een functie van Azure waarmee u diagnostische gegevens kunt verzamelen van een toepassing die in Azure wordt uitgevoerd. Dit zijn diagnostische gegevens voor foutopsporing en het oplossen van problemen, meten van de prestaties, Resourcegebruik, analyse van het netwerkverkeer, capaciteitsplanning bewaking en controle. Nadat de diagnostische gegevens zijn verzameld, kunt u deze overbrengen naar een Azure-opslagaccount voor persistentie. Overdrachten kunnen ofwel worden gepland of op aanvraag.

## <a name="threat-mitigation"></a>Threat risicobeperking
Naast isolatie, versleuteling en filtering biedt Azure een aantal risicobeperkende mechanismen en processen om de infrastructuur en services te beschermen. Hiertoe behoren interne controles en technologieën die worden gebruikt om geavanceerde bedreigingen, zoals DDoS, de escalatie van bevoegdheden en de [OWASP Top 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), te detecteren en onschadelijk te maken.

De beveiligingsmechanismen en risicobeheerprocessen die door Microsoft worden gebruikt voor het beveiligen van zijn cloudinfrastructuur, verminderen het risico van beveiligingsincidenten. In het geval van een incident voordoet, is het team Security Incident Management (SIM) binnen het team van Microsoft-Services voor Online beveiliging en naleving (OSSC) gereed om te reageren op elk gewenst moment.

### <a name="how-azure-implements-threat-mitigation"></a>De implementatie van risicobeperking in Azure
Azure heeft beveiligingsmechanismen geïmplementeerd voor het implementeren van risicobeperking threat en om mogelijke bedreigingen in hun omgeving kunnen klanten te helpen. De volgende lijst bevat een overzicht van de threat risicobeperking mogelijkheden die worden aangeboden door Azure:

* [Azure Antimalware](azure-security-antimalware.md) is standaard ingeschakeld op alle infrastructuurservers. U kunt deze desgewenst inschakelen in uw eigen virtuele machines.
* Microsoft bewaakt servers, netwerken en toepassingen doorlopend om bedreigingen te detecteren en aanvallen te voorkomen. Met geautomatiseerde waarschuwingen worden beheerders op de hoogte gebracht van afwijkend gedrag, waardoor ze maatregelen kunnen nemen tegen interne en externe bedreigingen.
* U kunt oplossingen van derden implementeren binnen uw abonnementen, zoals web application firewalls van [Barracuda](https://techlib.barracuda.com/ng54/deployonazure).
* Aanpak van Microsoft voor het testen van binnendringen bevat '[rood teams](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf), ' waarop de medewerkers van Microsoft security is een aanval op (niet-klant) live productiesystemen van Azure voor het testen van beveiliging op basis van echte, omvat geavanceerde, permanente bedreigingen.
* Met geïntegreerde implementatiesystemen worden de distributie en installatie van beveiligingspatches beheerd op het hele Azure-platform.

## <a name="next-steps"></a>Volgende stappen
[Vertrouwenscentrum van Azure](https://azure.microsoft.com/support/trust-center/)

[Blog van het Azure-beveiligingsteam](http://blogs.msdn.com/b/azuresecurity/)

[Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

[Active Directory-blog](http://blogs.technet.com/b/ad/)
