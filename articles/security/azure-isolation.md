---
title: Isolatie in de Azure openbare Cloud | Microsoft Docs
description: Meer informatie over cloud-gebaseerde computers onder meer services als een groot aantal compute-exemplaren en services die kunnen worden geschaald omhoog en omlaag automatisch om te voldoen aan de behoeften van uw toepassing of enterprise.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: a153d70e077ad63a042e76d0c4ae40e3cc067a2a
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="isolation-in-the-azure-public-cloud"></a>Isolatie van opslagprestaties in de openbare Azure-Cloud
##  <a name="introduction"></a>Inleiding
### <a name="overview"></a>Overzicht
Bij de huidige en toekomstige Azure klanten begrijpen en gebruiken van de verschillende betrekking hebben op beveiliging mogelijkheden beschikbaar in en rond de Azure-platform, Microsoft heeft ontwikkeld die een reeks whitepapers, overzichten van de beveiliging, Best Practices en Controlelijsten.
De onderwerpen in termen van breedte en diepte liggen en regelmatig worden bijgewerkt. Dit document is onderdeel van de reeks zoals samengevat in de volgende sectie Abstract.

### <a name="azure-platform"></a>Azure-Platform
Azure is een open en flexibele cloud service-platform die ondersteuning biedt voor de breedste selectie van besturingssystemen, programmeertalen, frameworks, hulpprogramma's, databases, en apparaten. U kunt bijvoorbeeld:
- Linux-containers worden uitgevoerd met Docker-integratie;
- Bouwen van apps met JavaScript, Python, .NET, PHP, Java en Node.js; en
- Build back-ends voor iOS, Android en Windows apparaten.

Microsoft Azure ondersteunt dezelfde technologieën miljoenen ontwikkelaars en IT-professionals die al zijn afhankelijk van en vertrouwt.

Wanneer u bouwen op of IT-activa te migreren, een openbare cloud serviceprovider, u zijn afhankelijk van die organisatie mogelijkheden voor uw toepassingen en gegevens beschermen met de services en de besturingselementen die ze bieden voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

De infrastructuur van Azure is zo ontworpen dat toepassingen miljoenen klanten tegelijkertijd kunnen hosten en er daarnaast een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan hun beveiligingsbehoeften. Bovendien biedt Azure u een scala aan configureerbare beveiligingsopties en de mogelijkheid om deze te beheren, zodat u de beveiliging kunt afstemmen op de unieke vereisten van uw implementaties. Dit document helpt u aan deze vereisten voldoet.

### <a name="abstract"></a>Abstracte

Microsoft Azure kunt u toepassingen en virtuele machines (VM's) op gedeelde fysieke infrastructuur worden uitgevoerd. Een van de voornaamste economische motivaties voor het uitvoeren van toepassingen in een cloudomgeving is de mogelijkheid voor het distribueren van de kosten van gedeelde resources tussen meerdere klanten. Deze oefening van multi-tenancymodus verbetert de efficiëntie door multiplexing resources onder de verschillende klanten tegen lage kosten. Helaas kleven er ook het risico van het delen van fysieke servers en andere bronnen infrastructuur om uit te voeren voor uw gevoelige toepassingen en virtuele machines die deel van een willekeurige en kwaadwillende gebruikers uitmaken mogelijk.

In dit artikel bevat een overzicht van hoe Microsoft Azure biedt isolatie tegen schadelijke en niet kwaadwillende gebruikers en fungeert als een handleiding voor cloudoplossingen door verschillende isolatie-opties biedt voor architecten worden veranderd. Dit document richt zich op de technologie van Azure-platform en klantgerichte beveiligingsmechanismen en probeert niet te adres Sla's prijzen modellen en DevOps practice overwegingen.

## <a name="tenant-level-isolation"></a>Niveau isolatie van tenants
Een van de belangrijkste voordelen van cloud computing concept van een gedeelde gemeenschappelijke infrastructuur gelijktijdig op meerdere klanten is, wat leidt tot schaalvoordelen. Dit concept wordt multi-tenancymodus genoemd. Microsoft werkt continu om ervoor te zorgen dat de multitenant-architectuur van Microsoft Cloud Azure beveiliging, vertrouwelijkheid, privacy, integriteit en beschikbaarheid standaarden ondersteunt.

Met betrekking tot werklocaties in de cloud kan een tenant worden gedefinieerd als een client of organisatie die een bepaald exemplaar van de gebruikte cloudservice in eigendom heeft en beheert. Met het identiteitsplatform dat wordt geleverd door Microsoft Azure, is een tenant het eigen exemplaar van Azure Active Directory (Azure AD) die uw organisatie ontvangt en de eigenaar is wanneer deze zich voor een Microsoft-cloudservice aanmeldt.

Elke Azure AD-directory is uniek en werkt afzonderlijk van andere Azure AD-directory’s. Net zoals een kantoorgebouw een beveiligd bedrijfsmiddel is van uw organisatie, is ook een Azure AD-directory ontworpen als een beveiligd bedrijfsmiddel dat alleen door uw organisatie kan worden gebruikt. De Azure AD-architectuur isoleert klant- en identiteitsgegevens, zodat deze niet door elkaar worden gehaald. Dit betekent dat gebruikers en beheerders van een Azure AD-directory niet per ongeluk of opzettelijk gegevens in een andere directory kunnen openen.

### <a name="azure-tenancy"></a>Azure-Tenancymodus
Azure-tenancymodus (Azure-abonnement) verwijst naar een relatie 'klant ' / facturering en een unieke [tenant](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) in [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). Niveau isolatie van tenants in Microsoft Azure wordt bereikt met Azure Active Directory en [besturingselementen op basis van rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) die worden aangeboden door het. Elk Azure-abonnement is gekoppeld aan één map op Azure Active Directory (AD).

Gebruikers, groepen en toepassingen van die directory kunnen resources in het Azure-abonnement te beheren. U kunt deze toegangsrechten met behulp van de Azure-portal, Azure-opdrachtregelprogramma's en Azure Management-API's. Een Azure AD-tenant is logisch is geïsoleerd met behulp van beveiligingsgrenzen zodat klanten geen toegang hebt of mede-tenants, met kwaadaardige bedoelingen of per ongeluk in gevaar brengen. Azure AD wordt uitgevoerd op bare-metalcomputer'-servers die zijn geïsoleerd in een netwerksegment gescheiden, waarbij hostniveau pakketfilters en Windows Firewall blokkeren ongewenste verbindingen en verkeer.

- Toegang tot gegevens in Azure AD vereist verificatie van de gebruiker via een beveiligingstokenservice (STS). Informatie over de bestaan, de ingeschakelde status en de rol van de gebruiker wordt gebruikt door het autorisatiesysteem om te bepalen of de aangevraagde toegang tot de doel-tenant voor deze gebruiker is gemachtigd in deze sessie.

![Azure-Tenancymodus](./media/azure-isolation/azure-isolation-fig1.png)


- Tenants discrete containers zijn en er is geen relatie tussen deze.

- Geen toegang via tenants tenzij tenantbeheerder verleend via federatie of het inrichten van gebruikersaccounts van andere tenants.

- Fysieke toegang tot servers die bestaan uit de Azure AD-service en directe toegang tot Azure AD-back-end-systemen, is beperkt.

- Azure AD-gebruikers hebben geen toegang tot fysieke activa of een andere locatie en het is daarom niet mogelijk ze voor het overslaan van de logische RBAC beleid controles vermeld te volgen.

Een operationeel model die gebruikmaakt van een systeem van de uitbreiding van bevoegdheden just-in-time-bevoegdheden is vereist en gebruikt voor diagnostische gegevens en onderhoudsbehoeften. Azure AD Privileged Identity Management (PIM) introduceert het concept van een in aanmerking komende-beheerder. [In aanmerking komende beheerders](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) moeten gebruikers die de juiste rechten moeten toegang af en toe, maar niet elke dag. De rol is niet actief totdat de gebruiker toegang, moet vervolgens zij een activering te voltooien en een actieve beheerder voor een vooraf bepaalde hoeveelheid tijd zijn geworden.

![Azure AD Privileged Identity Management](./media/azure-isolation/azure-isolation-fig2.png)

Azure Active Directory als host fungeert voor elke tenant in de eigen beveiligde container, beleidsregels en machtigingen voor en in de container uitsluitend eigendom van en beheerd door de tenant.

Het concept van tenant containers is diep is in Active Directory op alle lagen van de portals helemaal tot aan de permanente opslag.

Zelfs wanneer de metagegevens van meerdere tenants van Azure Active Directory op dezelfde fysieke schijf is opgeslagen, is er geen relatie tussen de containers dan wat wordt gedefinieerd door de directoryservice, die op zijn beurt wordt bepaald door de tenantbeheerder.

### <a name="azure-role-based-access-control-rbac"></a>Azure op rollen gebaseerde toegangsbeheer (RBAC)
[Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) helpt u bij het delen van verschillende onderdelen die beschikbaar zijn in een Azure-abonnement dankzij Geavanceerd toegangsbeheer voor Azure. Azure RBAC kunt u taken binnen uw organisatie scheiden en het verlenen van toegang op basis van de gebruikers moeten uitvoeren van hun taken. In plaats van iedereen geven onbeperkte machtigingen in Azure-abonnement of bronnen, kunt u alleen bepaalde acties toestaan.

Azure RBAC heeft drie elementaire functies die van toepassing op alle brontypen:

- **Eigenaar** heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren.

- **Inzender** kunt maken en beheren van alle soorten Azure-resources, maar kan geen toegang tot de overige verlenen.

- **Lezer** bestaande Azure-resources kunt weergeven.

![Op rollen gebaseerde toegangsbeheer van Azure](./media/azure-isolation/azure-isolation-fig3.png)

De rest van de RBAC-rollen in Azure toestaan van beheer van specifieke Azure-resources. De rol Inzender van de virtuele Machine kan bijvoorbeeld de gebruiker te maken en beheren van virtuele machines. Het biedt ze geen toegang geven tot het virtuele Azure-netwerk of het subnet dat de virtuele machine verbinding met maakt.

[Ingebouwde RBAC-rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) vermeld de rollen die beschikbaar zijn in Azure. De Hiermee geeft u de bewerkingen en het bereik dat elke ingebouwde rol aan gebruikers verleent. Als u op zoek bent om te definiëren van uw eigen rollen voor nog meer informatie over het bouwen [aangepaste rollen in Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

Bepaalde andere functies voor Azure Active Directory zijn onder andere:
- Azure AD kunt eenmalige aanmelding voor SaaS-toepassingen, ongeacht waar ze worden gehost. Voor sommige toepassingen kan federatieve aanmelding worden gebruikt via Azure AD en voor andere toepassingen kan eenmalige aanmelding (SSO) met een wachtwoord worden gebruikt. Federatieve toepassingen kunnen bieden ook ondersteuning voor gebruikers inrichten en [wachtwoordkluizen](https://www.techopedia.com/definition/31415/password-vault).

- De toegang tot gegevens in [Azure Storage](https://azure.microsoft.com/services/storage/) wordt geregeld via verificatie. Elk opslagaccount is een primaire sleutel ([opslagaccountsleutel](https://docs.microsoft.com/azure/storage/storage-create-storage-account), of SAK) en een secundaire geheime sleutel (shared access signature voor, of SAS).

- Azure AD levert de identiteit als via de federation Service met behulp van [Active Directory Federation Services](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs), synchronisatie en replicatie met on-premises adreslijsten.

- [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) is van de multi-factor authentication-service waarbij gebruikers aanmeldingen verifiëren met behulp van een mobiele app, telefonische oproep of een tekstbericht. Het kan worden gebruikt met Azure AD om veilige on-premises netwerkbronnen met de Azure multi-factor Authentication-server en aangepaste toepassingen en mappen met de SDK.

- [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kunt u toevoegen aan virtuele machines in Azure Active Directory-domein zonder het implementeren van domeincontrollers. U kunt aanmelden bij deze virtuele machines met uw bedrijfsreferenties in Active Directory en domein virtuele machines beheren met behulp van Groepsbeleid af te dwingen beveiligingsbasislijnen op uw Azure virtuele machines.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) biedt een maximaal beschikbare globale-identity management-service voor consumententoepassingen voor honderden miljoenen identiteiten. De service kan worden geïntegreerd in zowel mobiele als webplatforms. Uw consumenten zich aanmelden bij uw toepassingen via aanpasbare met behulp van hun bestaande sociale accounts of referenties maken.

### <a name="isolation-from-microsoft-administrators--data-deletion"></a>Isolatie van Microsoft-beheerders & gegevens verwijderen
Microsoft hecht sterk maatregelen om uw gegevens beschermen tegen ongeoorloofde toegang, of gebruik door onbevoegden. Deze operationele processen en -besturingselementen worden ondersteund door de [Online Services-voorwaarden](http://aka.ms/Online-Services-Terms), welke aanbieding contractueel verplichtingen die voor de toegang tot uw gegevens.

-   Microsoft engineers hoeft geen standaardtoegang tot uw gegevens in de cloud. In plaats daarvan ze toegang, onder beheer toezicht, alleen indien nodig. Dat toegang is zorgvuldig beheerd en geregistreerd en ingetrokken wanneer het niet langer nodig is.

-   Microsoft mogelijk gebruik van andere bedrijven beperkte services te bieden voor zijn rekening. Onderaannemers mogelijk toegang tot klantgegevens alleen voor het leveren van die de services waarvoor we deze hebben ingehuurd om en zij hebben geen toestemming voor andere doeleinden te gebruiken. Bovendien zijn ze contractueel gebonden aan het onderhouden van de vertrouwelijkheid van gegevens voor onze klanten.

Zakelijke services met gecontroleerde certificeringen zoals ISO/IEC 27001 regelmatig gecontroleerd door Microsoft en erkende audit bedrijven, die voorbeeld audits om te verklaren die toegang, alleen voor legitieme bedrijfsdoeleinden uitvoeren. U kunt altijd toegang hebt tot uw eigen gegevens van de klant op elk gewenst moment en voor welke reden dan ook.

Als u geen gegevens verwijdert, worden de gegevens, inclusief alle kopieën in de cache of back-up in Microsoft Azure verwijderd. Voor de services-scope, die verwijdering binnen 90 dagen na het einde van de bewaarperiode vallen. (In de scope-services zijn gedefinieerd in de sectie gegevensverwerking voorwaarden van onze [Online Services-voorwaarden](http://aka.ms/Online-Services-Terms).)

Als een schijf die wordt gebruikt voor de opslag van een hardware storing, is het veilig [verwijderd of vernietigd](https://www.microsoft.com/trustcenter/Privacy/You-own-your-data) voordat Microsoft terug naar de fabrikant voor vervanging of herstel. Om ervoor te zorgen dat de gegevens kunnen niet worden hersteld door middel van de gegevens op de schijf overschreven.

## <a name="compute-isolation"></a>COMPUTE isolatie
Microsoft Azure biedt verschillende cloud-gebaseerde computers onder meer services als een groot aantal compute-exemplaren en services die kunnen worden geschaald omhoog en omlaag automatisch om te voldoen aan de behoeften van uw toepassing of enterprise. Deze compute-exemplaar en de service bieden isolatie op meerdere niveaus om gegevens te beveiligen zonder verlies van de flexibiliteit in de configuratie die vraag van klanten.

### <a name="hyper-v--root-os-isolation-between-root-vm--guest-vms"></a>Hyper-V & hoofdmap OS-isolatie tussen & hoofdmap VM-Gast-VM 's
Azure rekenplatform is gebaseerd op de machine virtualisatie: wil zeggen dat alle code van de klant in een Hyper-V virtuele machine wordt uitgevoerd. Voor elke Azure-knooppunt (of netwerkeindpunt) is er een Hypervisor die rechtstreeks via de hardware uitgevoerd en verdeelt van een knooppunt in het nummer van een variabele van de gast (virtuele Machines).


![Hyper-V & hoofdmap OS-isolatie tussen & hoofdmap VM-Gast-VM 's](./media/azure-isolation/azure-isolation-fig4.jpg)


Elk knooppunt heeft ook een speciale hoofdmap VM, die het Host-besturingssysteem wordt uitgevoerd. De grens van een kritieke is de isolatie van de basis-VM van de Gast-VM's en de Gast-VM's van elkaar, beheerd door de hypervisor en de basis-besturingssysteem. De hypervisor/root OS-koppeling maakt gebruik van Microsoft tientallen jaren besturingssysteem beveiliging ervaring, en meer recente leren van Microsoft Hyper-V, sterke isolatie van Gast-VM's te bieden.

Voor het Azure-platform wordt gebruikgemaakt van een gevirtualiseerde omgeving. Gebruikersexemplaren werkt als zelfstandige virtuele machines die geen toegang tot een fysieke host-server en deze isolatie wordt afgedwongen met behulp van de fysieke processor (ring-0-en ring 3) bevoegdheidsniveaus.

Ring 0 is het niveau met de meeste bevoegdheden en ring 3 het niveau met de minste bevoegdheden. Het gastbesturingssysteem wordt uitgevoerd in een minder bevoegdheden Ring 1 en toepassingen worden uitgevoerd in de laagst mogelijke Ring 3. Deze virtualisatie van fysieke resources leidt tot een duidelijke scheiding tussen het gastbesturingssysteem en de hypervisor, wat zorgt voor een extra veiligheidsbarrière tussen de twee.

De Azure-hypervisor fungeert als een micro-kernel en geeft alle hardware toegangsaanvragen van virtuele machines voor gasten op de host voor de verwerking met behulp van een gedeeld geheugen interface VMBus aangeroepen. Dit voorkomt dat gebruikers onbeperkt toegang krijgen tot het systeem om gegevens te lezen en te schrijven of programma’s uit te voeren en vermindert de risico’s die kleven aan het delen van systeemresources.

### <a name="advanced-vm-placement-algorithm--protection-from-side-channel-attacks"></a>Geavanceerde VM plaatsing algoritme & bescherming tegen kanaal aan clientzijde
Een aanval cross-VM bestaat uit twee stappen: een VM adversary beheerd op dezelfde host als een van de virtuele machines van het slachtoffer te plaatsen en vervolgens inbreuk op de isolatiegrens voor het slachtoffer gevoelige gegevens worden gestolen of de prestaties voor greed of vandalisme beïnvloeden. Microsoft Azure biedt bescherming op beide stappen met behulp van een geavanceerde VM plaatsing algoritme en beveiliging van alle bekende side kanaal aanvallen, met inbegrip van virtuele machines daarop.

### <a name="the-azure-fabric-controller"></a>De Azure-infrastructuur-Controller
De Azure-Infrastructuurcontroller is verantwoordelijk voor het toewijzen van bronnen van de infrastructuur voor tenant-werkbelastingen en beheert Unidirectioneel communicatie tussen de host en virtuele machines. De VM brengen algoritme van de Azure-infrastructuurcontroller is zeer geavanceerde en nagenoeg onmogelijk te voorspellen als fysieke hostniveau.

![De Azure-infrastructuur-Controller](./media/azure-isolation/azure-isolation-fig5.png)

De Azure hypervisor afgedwongen geheugen en proces scheiding tussen virtuele machines en het veilig routeert netwerkverkeer Gast OS tenants. Dit voorkomt kans en aan clientzijde kanaal aanval op het niveau van de virtuele machine.

In Azure, de hoofdmap VM is speciaal: een beperkte besturingssysteem de hoofdmap OS aangeroepen die als host fungeert voor een fabric-agent (VA) wordt uitgevoerd. Door zijn binnen gastbesturingssystemen van de klant virtuele machines op zijn beurt gebruikt voor het beheren van Gast-agents (GA). Door ook beheren opslagknooppunten.

De verzameling van Azure hypervisor root OS/VA en klant VM's / GAs bestaat uit een rekenknooppunt. VA worden beheerd door een fabric-controller (FC), die buiten de berekenings- en knooppunten (berekenings- en -clusters worden beheerd door afzonderlijke FCs) bestaat. Als een klant updates hun toepassingsconfiguratiebestand terwijl deze wordt uitgevoerd, wordt de FC communiceert met de VA, die vervolgens GAs contact, die de toepassing van de configuratiewijziging melden. In het geval van een hardwarestoring de FC automatisch zoeken naar beschikbare hardware en opnieuw opstarten van de virtuele machine er.

![Azure-Infrastructuurcontroller](./media/azure-isolation/azure-isolation-fig6.jpg)

Communicatie van een Infrastructuurcontroller naar een agent is Unidirectioneel. De agent wordt geïmplementeerd voor een service met SSL beveiligde die alleen op aanvragen van de domeincontroller reageert. Deze kan niet verbindingen met de domeincontroller of andere bevoorrechte interne knooppunten starten. De FC beschouwt alle antwoorden op dezelfde manier als niet-vertrouwd.


![Fabric-Controller](./media/azure-isolation/azure-isolation-fig7.png)

Isolatie van uitbreiding van de basis-VM van Gast-VM's en de Gast-VM's van elkaar. COMPUTE knooppunten zijn ook geïsoleerd van de configuratie voor opslagknooppunten voor een betere bescherming.


De hypervisor en de OS netwerkpakket - filters bieden om te zorgen dat niet-vertrouwde virtuele machines kan geen vervalste verkeer genereren of ontvangen van verkeer dat niet is geadresseerd aan deze host verkeer omleiden naar beveiligde infrastructuur eindpunten of verzenden/ontvangen ongeschikte broadcast-verkeer.


### <a name="additional-rules-configured-by-fabric-controller-agent-to-isolate-vm"></a>Extra regels die zijn geconfigureerd door de Fabric-Controller Agent VM isoleren
Standaard worden alle verkeer wordt geblokkeerd als een virtuele machine wordt gemaakt en vervolgens de fabric-controller agent configureert u het pakketfilter om toe te voegen regels en uitzonderingen geautoriseerde verkeer toestaan.

Er zijn twee categorieën van regels die zijn geprogrammeerd:

-   **Configuratie of infrastructuur regels machine:** standaard alle communicatie is geblokkeerd. Er zijn uitzonderingen op een virtuele machine te verzenden en ontvangen van DHCP en DNS-verkeer toestaan. Virtuele machines kunnen verkeer verzenden naar het 'openbare' internet en op andere virtuele machines in hetzelfde virtuele netwerk Azure en de OS-activeringsserver verkeer kunnen verzenden. De virtuele machines lijst met toegestane uitgaande bestemmingen omvat geen Azure-router subnetten, Azure management en andere Microsoft-eigenschappen.

-   **Configuratiebestand van de rol:** Hiermee definieert u de inkomende toegangsbeheerlijsten (ACL's) op basis van de tenant-ServiceModel.

### <a name="vlan-isolation"></a>VLAN-isolatie
Er zijn drie VLAN's in elke cluster:

![VLAN-isolatie](./media/azure-isolation/azure-isolation-fig8.jpg)


-   De belangrijkste VLAN-verbindingen niet-vertrouwde klant knooppunten

-   De VLAN FC – bevat vertrouwde FCs- en ondersteunende systemen

-   Het apparaat VLAN – bevat vertrouwd netwerk en andere infrastructuurapparaten

Communicatie met de belangrijkste VLAN van de FC-VLAN is toegestaan, maar het FC-VLAN van de belangrijkste VLAN kan niet worden gestart. Communicatie met het apparaat VLAN ook geblokkeerd van de belangrijkste VLAN. Dit zorgt ervoor dat deze knooppunten op de FC- of apparaat VLAN's kan niet zelfs als een knooppunt met de klantcode ermee is geknoeid aanvallen.

## <a name="storage-isolation"></a>Isolatie van opslag
### <a name="logical-isolation-between-compute-and-storage"></a>Logische isolatie tussen berekeningen en opslag
Als onderdeel van het ontwerp van de fundamentele scheidt u Microsoft Azure berekeningen uit de opslag op basis van VM. Deze scheiding kunt berekening en opslag kunnen worden geschaald, waardoor het gemakkelijker om multi-tenancymodus en isolatie te bieden.

Daarom Azure Storage wordt uitgevoerd op afzonderlijke hardware met geen netwerkverbinding naar Azure Compute, behalve logisch. [Dit](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf) betekent dat wanneer een virtuele schijf wordt gemaakt, niet voor de volledige capaciteit schijfruimte. In plaats daarvan een tabel die is toegewezen adressen op de virtuele schijf aan gebieden van de fysieke schijf wordt gemaakt en die tabel in eerste instantie leeg is. **De eerste keer dat een klant gegevens op de virtuele schijf schrijft ruimte op de fysieke schijf is toegewezen en een verwijzing naar deze in de tabel is geplaatst.**
### <a name="isolation-using-storage-access-control"></a>Isolatie met behulp van Storage toegangsbeheer
**Toegangsbeheer in Azure Storage** een eenvoudige model voor toegangsbeheer heeft. Elk Azure-abonnement kunt maken van een of meer Accounts voor opslag. Elk Opslagaccount heeft een geheim dat wordt gebruikt voor toegang tot alle gegevens in dit Opslagaccount.

![Isolatie met behulp van Storage toegangsbeheer](./media/azure-isolation/azure-isolation-fig9.png)

**Toegang tot Azure Storage-gegevens (inclusief tabellen)** kan worden beheerd via een [SAS (Shared Access Signature)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) token, welke verleent toegang binnen het bereik. De SAS is gemaakt via een querysjabloon (URL) zijn ondertekend met de [SAK (Opslagaccountsleutel)](https://msdn.microsoft.com/library/azure/ee460785.aspx). Die [ondertekend URL](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) aan een ander proces (dat is gedelegeerd), die u kunt vervolgens de details van de query opgeven en het indienen van de storage-service kan worden opgegeven. Een SAS kunt u op basis van tijd om toegang te verlenen aan clients zonder geheime sleutel van het storage-account weer te geven.

De SAS betekent dat er een client beperkte machtigingen voor objecten in onze storage-account gedurende een bepaalde tijd en met een opgegeven set machtigingen kunt verlenen. We kunnen deze beperkte rechten verlenen zonder te delen van de toegangssleutels van uw account.

### <a name="ip-level-storage-isolation"></a>IP-opslagniveau isolatie
U kunt tot stand brengen van firewalls en een IP-adresbereik opgeven voor uw vertrouwde clients. Met een IP-adresbereik alleen clients die een IP-adres binnen het gedefinieerde bereik hebben, kunnen verbinding maken met [Azure Storage](https://docs.microsoft.com/azure/storage/storage-security-guide).

IP-storage-gegevens kunnen worden beveiligd tegen onbevoegde gebruikers via een VPN-mechanisme dat wordt gebruikt voor het toewijzen van een toegewezen of speciale tunnel van verkeer naar de IP-opslag.

### <a name="encryption"></a>Versleuteling
Azure biedt volgende typen versleuteling om gegevens te beveiligen:
-   Codering in transit

-   Versleuteling 'at rest'

#### <a name="encryption-in-transit"></a>Codering in Transit
Versleuteling onderweg is een mechanisme om gegevens te beveiligen wanneer deze worden verzonden via netwerken. U kunt beveiligen met behulp van gegevens met Azure Storage:

-   [Versleuteling transportniveau](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), zoals HTTPS wanneer u gegevens van of naar Azure Storage overbrengen.

-   [Versleuteling Wire](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), zoals versleuteling door SMB 3.0 voor Azure-bestandsshares.

-   [Versleuteling aan clientzijde](https://docs.microsoft.com/azure/storage/storage-security-guide#using-client-side-encryption-to-secure-data-that-you-send-to-storage), voor het versleutelen van de gegevens voordat deze worden overgedragen naar de opslag en de gegevens te decoderen nadat deze zijn overgebracht buiten-opslag.

#### <a name="encryption-at-rest"></a>Codering in rust
Voor veel organisaties [gegevensversleuteling in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is een verplichte stap naar privacy, naleving en gegevens onafhankelijkheid van gegevens. Er zijn drie Azure-functies waarmee de codering van gegevens die zich 'in rust':

-   [Versleuteling van de opslagruimte](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-at-rest) Hiermee kunt u aanvragen dat de storage-service gegevens automatisch coderen wanneer het schrijven naar Azure Storage.

-   [Versleuteling aan clientzijde](https://docs.microsoft.com/azure/storage/storage-security-guide#client-side-encryption) biedt ook de functie van versleuteling in rust.

-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) kunt u voor het versleutelen van de OS-schijven en gegevensschijven die wordt gebruikt door een virtuele machine voor IaaS.

#### <a name="azure-disk-encryption"></a>Azure Disk Encryption
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) voor virtuele machines (VM's) kunt u bedrijfsbeveiligingsbeleid adres- en nalevingsvereisten door uw VM-schijven (inclusief opstart- en gegevensschijven) te versleutelen met sleutels en beleidsregels die u beheert in [Azure-sleutel Kluis](https://azure.microsoft.com/services/key-vault/).

De oplossing schijfversleuteling voor Windows is gebaseerd op [Microsoft BitLocker-stationsversleuteling](https://technet.microsoft.com/library/cc732774.aspx), en de Linux-oplossing is gebaseerd op [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

De oplossing ondersteunt de volgende scenario's voor IaaS VM's wanneer ze zijn ingeschakeld in Microsoft Azure:
-   Integratie met Azure Sleutelkluis

-   Standard-laag VMs: A, D, DS, G, GS, enzovoort, reeks IaaS VM's

-   Codering voor Windows en Linux IaaS VM's inschakelen

-   Het uitschakelen van versleuteling op besturingssysteem en schijven voor Windows IaaS VM 's

-   Codering op gegevensstations voor Linux IaaS VM's uitschakelen

-   IaaS VM's waarop Windows client-OS-codering inschakelen

-   Versleuteling voor volumes met koppelpunten paden inschakelen

-   Inschakelen van versleuteling op Linux-virtuele machines die zijn geconfigureerd met de schijf (RAID) te verwijderen met behulp van [mdadm](https://en.wikipedia.org/wiki/Mdadm)

-   Inschakelen van versleuteling op Linux VM's met behulp van [LVM (beheer van logische volumes)](https://msdn.microsoft.com/library/windows/desktop/bb540532) voor gegevensschijven

-   Op Windows-virtuele machines die zijn geconfigureerd met behulp van opslagruimten-codering inschakelen

-   Alle openbare Azure-regio worden ondersteund

De oplossing ondersteunt niet de volgende scenario's, functies en -technologie in de release:

-   Basisstaffel IaaS VM 's

-   Het uitschakelen van Linux IaaS VM's op een station OS versleuteling

-   IaaS VM's die zijn gemaakt met behulp van de methode voor het maken van klassieke VM

-   Integratie met uw on-premises Key Management Service

-   Azure Files (gedeelde bestandssysteem), Network File System (NFS), dynamische volumes en virtuele machines van Windows die zijn geconfigureerd met software gebaseerde RAID-systemen

## <a name="sql-azure-database-isolation"></a>SQL Azure Database isolatie
SQL Database is een relationele database-service in de Microsoft Cloud op basis van de toonaangevende Microsoft SQL Server-engine en is in staat bedrijfskritieke workloads af te handelen. SQL-Database biedt voorspelbare gegevensisolatie op niveau van de account, Geografie / regio op basis van en in het netwerk, met praktisch zonder beheer.

### <a name="sql-azure-application-model"></a>SQL Azure-toepassing-Model

[Microsoft SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started) Database is een relationele database cloud-gebaseerde service gebouwd op de SQL Server-technologieën. Er is een maximaal beschikbare, schaalbare, multitenant databaseservice gehost door Microsoft in de cloud.

Vanuit het perspectief van een toepassing SQL Azure biedt de volgende hiërarchie: elk niveau heeft een-op-veel containment van onderliggende niveaus.

![SQL Azure-toepassing-Model](./media/azure-isolation/azure-isolation-fig10.png)

Het account en abonnement zijn Microsoft Azure-platform concepten om facturering en beheer te koppelen.

Logische servers en databases SQL Azure-specifieke concepten en worden beheerd met behulp van SQL Azure opgegeven OData en TSQL-interfaces of via SQL Azure-portal geïntegreerd in de Azure-portal.

Azure SQL-servers zijn niet fysiek of VM-instanties, in plaats daarvan zijn verzamelingen van databases, beheer- en beveiligingsbeleid van die zijn opgeslagen in een zogenaamde 'logische master'-delen database.

![SQL Azure](./media/azure-isolation/azure-isolation-fig11.png)

Logische-hoofddatabases zijn onder andere:

-   SQL-aanmeldingen waarmee verbinding met de server

-   Firewall-regels

Facturering en gebruik-gerelateerde informatie voor Azure SQL databases van dezelfde logische server zijn niet gegarandeerd op hetzelfde fysieke exemplaar in Azure SQL-cluster, in plaats daarvan toepassingen moet de naam van de doel-database opgeven om verbinding te maken.

Vanuit het oogpunt van de klant van een logische server gemaakt in een geografisch grafische regio, terwijl de werkelijke maken van de server in een van de clusters in de regio gebeurt.

### <a name="isolation-through-network-topology"></a>Isolatie door middel van de netwerktopologie

Als een logische server wordt gemaakt en de DNS-naam is geregistreerd, de DNS-server een naam verwijst naar het zogenaamde 'Gateway VIP'-adres in het specifieke Datacenter waar de server is geplaatst.

We hebben een verzameling van staatloze gatewayservices achter het VIP (virtuele IP-adres). Gateways ophalen in het algemeen betrokken wanneer er coördinatie nodig tussen meerdere gegevensbronnen (hoofddatabase, gebruikersdatabase, enzovoort). Gatewayservices implementeren de volgende:
-   **TDS-verbinding via een proxy.** Dit omvat gebruikersdatabase zoeken in het back-end-cluster, de aanmeldingssequentie implementeren en vervolgens de TDS-pakketten naar de back-end en weer worden doorgestuurd.

-   **Database-beheer.** Dit omvat een verzameling van werkstromen voor CREATE/ALTER/DROP databasebewerkingen implementeren. De databasebewerkingen kunnen worden aangeroepen door sniffing TDS-pakketten of expliciete OData APIs.

-   CREATE/ALTER/DROP aanmeldingsgebruiker bewerkingen

-   Beheerbewerkingen logische server via de OData-API

![Isolatie door middel van de netwerktopologie](./media/azure-isolation/azure-isolation-fig12.png)

De laag achter de gateways wordt 'back-end' genoemd. Dit is waar de gegevens wordt opgeslagen in een maximaal beschikbare manier. Elk gegevensitem wordt gezegd te behoren tot een 'partitie' of 'failover-eenheid', elk met ten minste drie replica's. Replica's worden opgeslagen en gerepliceerd door de engine van SQL Server en beheerd door een failover-systeem vaak 'infrastructuur' genoemd.

In het algemeen communiceert de back-end-systeem niet met andere systemen uitgaande veiligheidsoverwegingen. Dit is gereserveerd voor de systemen in de laag front-end (gateway). De gateway-laagmachines hebben beperkte rechten op de back-end-machines te minimaliseren, de kwetsbaarheid voor aanvallen als een mechanisme defense-in-depth.

### <a name="isolation-by-machine-function-and-access"></a>Isolatie per Machine functie en toegang
SQL Azure (is samengesteld uit de services worden uitgevoerd op andere machine functies. SQL Azure is onderverdeeld in de back-end' Cloud-Database en de 'front-end' (/ gatewaybeheer)-omgevingen, met het algemene principe van verkeer alleen gaat naar de back-end en niet uit. De front-omgeving kan communiceren met de buitenwereld van andere services en in het algemeen heeft slechts beperkte machtigingen in de back-end (voldoende is voor het aanroepen van de toegangspunten nodig om aan te roepen).

## <a name="networking-isolation"></a>Isolatie van netwerken
Azure-implementatie bestaat uit meerdere lagen van netwerkisolatie. Het volgende diagram toont de verschillende lagen van Azure klanten biedt netwerkisolatie. Deze lagen zijn zowel systeemeigen in de Azure-platform zelf en de klant gedefinieerde functies. Inkomende vanaf Internet, biedt Azure DDoS isolatie op basis van grootschalige aanvallen op Azure. De volgende beveiligingslaag isolatie is door de klant gedefinieerde openbare IP-adressen (eindpunten) die worden gebruikt om te bepalen welk verkeer via de cloudservice kunt doorgeven aan het virtuele netwerk. Systeemeigen Azure virtuele netwerkisolatie garandeert volledige isolatie van alle andere netwerken, en dat alleen verkeersstromen via paden van de gebruiker die is geconfigureerd en methoden. Deze paden en methoden zijn de volgende laag, waar de nsg's, UDR en virtuele netwerkapparaten kunnen worden gebruikt isolatiegrenzen ter bescherming van de implementaties van toepassingen in het beveiligde netwerk maken.

![Isolatie van netwerken](./media/azure-isolation/azure-isolation-fig13.png)

**Isolatie van verkeer:** A [virtueel netwerk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) is de grens van de isolatie van verkeer op de Azure-platform. Virtuele machines (VM's) in één virtueel netwerk kan niet communiceren rechtstreeks met virtuele machines in een ander virtueel netwerk, zelfs als beide virtuele netwerken worden gemaakt van dezelfde klant. Isolatie is een kritieke eigenschap die ervoor zorgt dat de klant VM's en communicatie blijft persoonlijke binnen een virtueel netwerk.

[Subnet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#subnets) biedt een extra laag voor isolatie met in het virtuele netwerk op basis van IP-bereik. IP-adressen in het virtuele netwerk, kunt u een virtueel netwerk verdelen over meerdere subnetten voor organisatie en beveiliging. Tussen VM's en PaaS-rolexemplaren die in (dezelfde of verschillende) subnetten in een VNET zijn geïmplementeerd, is communicatie mogelijk zonder extra configuratie. U kunt ook configureren [netwerkbeveiligingsgroep (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#network-security-groups-nsg) wilt toestaan of weigeren van netwerkverkeer koppelen aan een VM-exemplaren op basis van regels die zijn geconfigureerd in de toegangsbeheerlijst (ACL) van het NSG. NSG's kunnen worden gekoppeld aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Als een NSG is gekoppeld aan een subnet, zijn de ACL-regels van toepassing op alle VM-exemplaren in dat subnet.

## <a name="next-steps"></a>Volgende stappen

- [Opties voor netwerkisolatie voor Machines in Windows Azure virtuele netwerken](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/)

Dit omvat het klassieke front-end en back-end scenario waar machines in een bepaalde back-endnetwerk of subnetwerk kunnen alleen toestaan dat bepaalde clients of andere computers verbinding maken met een bepaald eindpunt op basis van een lijst met geaccepteerde IP-adressen.

- [COMPUTE isolatie](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure biedt een verschillende cloud-gebaseerde computers onder meer services als een groot aantal compute-exemplaren en services die kunnen worden geschaald omhoog en omlaag automatisch om te voldoen aan de behoeften van uw toepassing of enterprise.

- [Isolatie van opslag](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure scheidt klant op basis van VM berekeningen uit de opslag. Deze scheiding kunt berekening en opslag kunnen worden geschaald, waardoor het gemakkelijker om multi-tenancymodus en isolatie te bieden. Daarom Azure Storage wordt uitgevoerd op afzonderlijke hardware met geen netwerkverbinding naar Azure Compute, behalve logisch. Alle aanvragen uitgevoerd via HTTP of HTTPS op basis van de keuze van de klant.

