---
title: Isolatie in de openbare Cloud van Azure | Microsoft Docs
description: Meer informatie over cloud-gebaseerde computing services met een ruime keuze aan rekenprocessen en -services die u omhoog en omlaag automatisch schalen kunnen om te voldoen aan de behoeften van uw toepassing of enterprise.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 5710ebc1c52737e27aafa88eef5e9ae402f8e53f
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579836"
---
# <a name="isolation-in-the-azure-public-cloud"></a>Isolatie in de openbare Cloud van Azure
##  <a name="introduction"></a>Inleiding
### <a name="overview"></a>Overzicht
Bij de huidige en toekomstige Azure klanten te begrijpen en gebruikmaken van de verschillende beveiligingsgerelateerde mogelijkheden die beschikbaar zijn in en rondom het Azure-platform, Microsoft heeft ontwikkeld die een reeks White Papers, overzichten van de beveiliging, aanbevolen procedures en Controlelijsten voor.
De onderwerpen in termen van diepgaande en brede toepassing variëren en worden regelmatig bijgewerkt. Dit document is onderdeel van de reeks zoals samengevat in de volgende sectie Abstract.

### <a name="azure-platform"></a>Azure-Platform
Azure is een open en flexibel cloudserviceplatform die ondersteuning biedt voor het breedste arsenaal aan besturingssystemen, programmeertalen programmeertalen, frameworks, tools, databases en apparaten. U kunt bijvoorbeeld:
- Voer Linux-containers met Docker-integratie;
- Bouw apps met JavaScript, Python, .NET, PHP, Java en Node.js; en
- Bouw back-ends voor iOS, Android en Windows apparaten.

Microsoft Azure ondersteunt dezelfde technologieën waar miljoenen ontwikkelaars en IT-professionals die al afhankelijk zijn van en vertrouwen.

Wanneer u baseren of IT-activa te migreren, een openbare cloud serviceprovider, u, vertrouwen al op de beveiligingmogelijkheden die aan uw toepassingen en gegevens beschermen met de services en de besturingselementen die ze bieden voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

De infrastructuur van Azure is zo ontworpen dat toepassingen miljoenen klanten tegelijkertijd kunnen hosten en er daarnaast een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan hun beveiligingsbehoeften. Bovendien biedt Azure u een scala aan configureerbare beveiligingsopties en de mogelijkheid om deze te beheren, zodat u de beveiliging kunt afstemmen op de unieke vereisten van uw implementaties. Dit document helpt u aan deze vereisten voldoen.

### <a name="abstract"></a>Samenvatting

Microsoft Azure kunt u toepassingen en virtuele machines (VM's) op gedeelde fysieke infrastructuur worden uitgevoerd. Een van de voornaamste economische redenen voor het uitvoeren van toepassingen in een cloudomgeving is de mogelijkheid voor het distribueren van de kosten van gedeelde resources tussen meerdere klanten. Met deze procedure van multitenancy verbetert de efficiëntie door multiplexing resources onder verschillende klanten tegen lage kosten. Helaas introduceert het ook het risico van het delen van fysieke servers en andere infrastructuurresources om uit te voeren van uw gevoelige toepassingen en virtuele machines die deel van een willekeurige en mogelijk kwaadwillende gebruiker uitmaken mogelijk.

In dit artikel bevat een overzicht van hoe Microsoft Azure biedt isolatie tegen schadelijke en niet-kwaadwillende gebruikers en fungeert als een handleiding voor cloud-oplossingen door verschillende isolatie-opties biedt voor architecten te ontwerpen. Dit technische document richt zich op de technologie van Azure-platform en klantgerichte beveiligingsmaatregelen en probeert niet te adres Sla's, prijzen modellen en overwegingen met betrekking tot DevOps-praktijken.

## <a name="tenant-level-isolation"></a>Niveau isolatie van tenants
Een van de belangrijkste voordelen van cloud computing concept van een gedeelde, gemeenschappelijke infrastructuur tegelijkertijd in meerdere klanten is, leidt tot schaalvoordelen. Dit concept wordt multitenancy genoemd. Microsoft werkt continu om ervoor te zorgen dat de architectuur met meerdere tenants van Azure voor Microsoft Cloud security, vertrouwelijkheid, privacy, integriteit en beschikbaarheid standaarden ondersteunt.

Met betrekking tot werklocaties in de cloud kan een tenant worden gedefinieerd als een client of organisatie die een bepaald exemplaar van de gebruikte cloudservice in eigendom heeft en beheert. Met het identiteitsplatform dat wordt geleverd door Microsoft Azure, is een tenant gewoon een toegewezen exemplaar van Azure Active Directory (Azure AD) die uw organisatie ontvangt en eigenaar is wanneer deze zich aanmeldt voor een Microsoft-cloudservice.

Elke Azure AD-directory is uniek en werkt afzonderlijk van andere Azure AD-directory’s. Net zoals een kantoorgebouw een beveiligd bedrijfsmiddel is van uw organisatie, is ook een Azure AD-directory ontworpen als een beveiligd bedrijfsmiddel dat alleen door uw organisatie kan worden gebruikt. De Azure AD-architectuur isoleert klant- en identiteitsgegevens, zodat deze niet door elkaar worden gehaald. Dit betekent dat gebruikers en beheerders van een Azure AD-directory niet per ongeluk of opzettelijk gegevens in een andere directory kunnen openen.

### <a name="azure-tenancy"></a>Azure-Tenants
Azure tenants (Azure-abonnement) verwijst naar een ' klant/facturering'-relatie en moet een uniek [tenant](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) in [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). Niveau isolatie van tenants in Microsoft Azure wordt bereikt met Azure Active Directory en [besturingselementen op basis van rollen](https://docs.microsoft.com/azure/role-based-access-control/overview) die worden aangeboden door het. Elk Azure-abonnement is gekoppeld aan een directory van Azure Active Directory (AD).

Gebruikers, groepen en toepassingen uit die map kunnen resources in het Azure-abonnement te beheren. U kunt deze toegangsrechten met behulp van de Azure portal, opdrachtregelprogramma's van Azure en Azure Management-API's. Een Azure AD-tenant wordt logisch geïsoleerd met beveiligingsgrenzen zodat klanten geen toegang tot of inbreuk CO-tenants, per ongeluk of opzettelijk. Azure AD wordt uitgevoerd op 'bare-metalcomputers' servers geïsoleerd in een gescheiden netwerksegment waar filteren van netwerkpakketten hostniveau en Windows Firewall blokkeert ongewenste verbindingen en het verkeer.

- Toegang tot gegevens in Azure AD is vereist verificatie van de gebruiker via een beveiligingstokenservice (STS). Informatie over het bestaan van de gebruiker, de ingeschakelde status en de rol wordt gebruikt door het autorisatiesysteem om te bepalen of de aangevraagde toegang tot de doel-tenant voor deze gebruiker is toegestaan in deze sessie.

![Azure-Tenants](./media/azure-isolation/azure-isolation-fig1.png)


- Tenants zijn afzonderlijke containers en er is geen relatie tussen deze.

- Geen toegang voor tenants, tenzij de tenantbeheerder verleend via federatie of het inrichten van gebruikersaccounts van andere tenants.

- Fysieke toegang tot servers die deel uitmaken van de Azure AD-service en directe toegang tot Azure AD back-endsystemen, is beperkt.

- Azure AD-gebruikers hebben geen toegang tot activa fysieke of een andere locatie en het is daarom niet mogelijk om het overslaan van de logische RBAC beleidscontroles vermeld te volgen.

Voor diagnostische gegevens en onderhoudsbehoeften, is een operationeel model met een bevoegdheden just-in-time-elevationsysteem vereist en gebruikt. Azure AD Privileged Identity Management (PIM) introduceert het concept van een in aanmerking komende beheerder. [In aanmerking komende beheerders](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) moeten gebruikers die bevoegdheden moeten toegang tot nu en dan maar niet elke dag. De rol is inactief totdat gebruikers toegang nodig hebben. Op dat moment voeren ze een activeringsproces uit en zijn ze gedurende een vooraf bepaalde hoeveelheid tijd een actieve beheerder.

![Azure AD Privileged Identity Management](./media/azure-isolation/azure-isolation-fig2.png)

Azure Active Directory als host fungeert voor elke tenant in een eigen beveiligde container, met beleidsregels en machtigingen en binnen de container uitsluitend eigendom van en worden beheerd door de tenant.

Het concept van tenant containers is diep is in de directoryservice op alle lagen van de portals helemaal tot aan permanente opslag.

Zelfs wanneer de metagegevens van meerdere tenants van Azure Active Directory worden opgeslagen op dezelfde fysieke schijf, is er geen relatie tussen de containers dan wat wordt gedefinieerd door de directoryservice, die op zijn beurt wordt bepaald door de tenantbeheerder.

### <a name="azure-role-based-access-control-rbac"></a>Azure Role-Based Access Control (RBAC)
[Azure Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) helpt u bij het delen van verschillende onderdelen die beschikbaar zijn binnen een Azure-abonnement door te geven over Geavanceerd toegangsbeheer voor Azure. Azure RBAC kunt u taken binnen uw organisatie scheiden en toegang op basis van wat gebruikers nodig om hun werk te kunnen verlenen. In plaats van zodat iedereen onbeperkte machtigingen in Azure-abonnement of resources, kunt u alleen bepaalde acties toestaan.

Azure RBAC heeft drie fundamentele rollen die betrekking hebben op alle resourcetypen:

- **De eigenaar van** heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren.

- **Inzender** kunt maken en beheren van alle typen Azure-resources, maar kan geen toegang om anderen te verlenen.

- **Lezer** bestaande Azure-resources kunt bekijken.

![Op rollen gebaseerde toegangsbeheer van Azure](./media/azure-isolation/azure-isolation-fig3.png)

De rest van de RBAC-rollen in Azure kunt beheer van specifieke Azure-resources. De rol Inzender voor virtuele machines kan bijvoorbeeld de gebruiker te maken en beheren van virtuele machines. Het is niet zodat ze toegang tot het Azure-netwerk of het subnet waarmee de virtuele machine verbinding maakt.

[Ingebouwde RBAC-rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) vermeld de rollen die beschikbaar zijn in Azure. Het geeft de operations- en scope die elke ingebouwde rol aan gebruikers verleent. Als u op zoek bent voor het definiëren van uw eigen rollen voor nog meer controle over het bouwen van Zie [aangepaste rollen in Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/custom-roles).

Bepaalde andere functies voor Azure Active Directory zijn onder andere:
- Azure AD kunt eenmalige aanmelding voor SaaS-toepassingen, ongeacht waar ze worden gehost. Voor sommige toepassingen kan federatieve aanmelding worden gebruikt via Azure AD en voor andere toepassingen kan eenmalige aanmelding (SSO) met een wachtwoord worden gebruikt. Federatieve toepassingen kunnen ook ondersteuning voor het inrichten van gebruikers en [wachtwoordkluis](https://www.techopedia.com/definition/31415/password-vault).

- De toegang tot gegevens in [Azure Storage](https://azure.microsoft.com/services/storage/) wordt geregeld via verificatie. Elk opslagaccount heeft een primaire sleutel ([opslagaccountsleutel](https://docs.microsoft.com/azure/storage/storage-create-storage-account), of SAK) en een secundaire geheime sleutel (de handtekening voor gedeelde toegang, of SAS).

- Azure AD biedt identiteit als een Service via federatie met behulp van [Active Directory Federation Services](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs), synchronisatie en replicatie met on-premises adreslijsten.

- [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) is van de multi-factor authentication-service die vereist dat gebruikers om te controleren of aanmeldingen via een mobiele app, telefonische oproep of SMS-bericht. Het kan worden gebruikt met Azure AD voor de veilige on-premises resources met de Azure multi-factor Authentication-server, en ook met aangepaste toepassingen en mappen met behulp van de SDK.

- [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kunt u virtuele machines van Azure toevoegen aan een Active Directory-domein zonder domeincontrollers te implementeren. U kunt zich aanmelden bij deze virtuele machines met uw bedrijfsreferenties van Active Directory en domein virtuele machines beheren met behulp van Groepsbeleid om af te dwingen basisbeveiliging voor uw Azure virtual machines.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) voorziet in een maximaal beschikbare globale-identity management-service voor consumentgerichte toepassingen die kan worden opgeschaald naar honderden miljoenen identiteiten. De service kan worden geïntegreerd in zowel mobiele als webplatforms. Uw consumenten kunnen al uw toepassingen aanpasbare manier aanmelden met hun bestaande sociale accounts of door referenties te maken.

### <a name="isolation-from-microsoft-administrators--data-deletion"></a>Isolatie van Microsoft-beheerders & gegevens verwijderen
Microsoft hecht sterke maatregelen om uw gegevens beschermen tegen ongeoorloofde toegang of gebruik door onbevoegden. Deze operationele processen en bediening worden ondersteund door de [voorwaarden voor onlineservices](http://aka.ms/Online-Services-Terms), die contractuele verplichtingen die toegang tot uw gegevens bieden bieden.

-   Microsoft-technici hebt geen standaardtoegang tot uw gegevens in de cloud. In plaats daarvan, krijgen zij toegang onder beheer van toezicht, alleen indien nodig. Die de toegang is zorgvuldig beheerd en geregistreerd en ingetrokken wanneer deze niet meer nodig is.

-   Microsoft kan huren andere bedrijven in beperkte diensten te verlenen namens haar. Onderaannemers kunnen toegang hebben tot klantgegevens alleen om te leveren die de services waarvoor we ze hebben ingehuurd en zij hebben geen toestemming voor andere doeleinden te gebruiken. Bovendien zijn ze contractueel gebonden aan het onderhouden van de vertrouwelijkheid van gegevens van onze klanten.

Zakelijke services met gecontroleerde certificeringen zoals ISO/IEC 27001 worden regelmatig gecontroleerd door geaccrediteerde auditbedrijven die voeren proefaudits om te bevestigen dat toegang alleen voor legitieme zakelijke doeleinden uit en Microsoft. U kunt altijd toegang tot uw eigen klantgegevens op elk gewenst moment en om elke reden.

Als u alle gegevens verwijdert, worden de gegevens, met inbegrip van alle exemplaren in de cache of back-up verwijderd in Microsoft Azure. Voor de services binnen de regeling vallen, die verwijderen wordt uitgevoerd om binnen 90 dagen na het einde van de bewaarperiode. (In-scope services zijn gedefinieerd in de sectie gegevensverwerking voorwaarden van onze [voorwaarden voor onlineservices](http://aka.ms/Online-Services-Terms).)

Als een schijf gebruikt voor de opslag leidt dit tot slechtere een hardwarestoring optreedt, is het veilig [worden gewist of vernietigd](https://microsoft.com/en-us/trustcenter/privacy/you-own-your-data) voordat Microsoft wordt geretourneerd voor vervanging of reparatie van de fabrikant. Om ervoor te zorgen dat de gegevens kunnen niet worden hersteld door middel van de gegevens op de schijf overschreven.

## <a name="compute-isolation"></a>COMPUTE-isolatie
Microsoft Azure biedt verschillende cloud-gebaseerde computing services met een ruime keuze aan rekenprocessen en -services die u omhoog en omlaag automatisch schalen kunnen om te voldoen aan de behoeften van uw toepassing of enterprise. Deze compute-instantie en de service bieden isolatie op meerdere niveaus om gegevens te beveiligen zonder dat dit ten koste gaat van de flexibiliteit in de configuratie die vraag van klanten.

### <a name="isolated-virtual-machine-sizes"></a>Geïsoleerde VM-grootten
Azure Compute biedt formaten virtuele machines die zijn geïsoleerd voor een specifiek hardwaretype en bedoeld voor een enkele klant.  Deze formaten virtuele machines zijn ideaal voor workloads waarvoor een hoge mate van isolatie van andere klanten vereist is en voor workloads waarbij elementen als naleving en wettelijke vereisten een rol spelen.  Klanten kunnen ook kiezen voor het verder onderverdelen de resources van deze geïsoleerde virtuele machines met behulp van [Azure-ondersteuning voor geneste virtuele machines](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).

Met behulp van de grootte van een geïsoleerde zorgt ervoor dat uw virtuele machine worden slechts één actief in die specifieke server-exemplaar.  De huidige aanbiedingen met geïsoleerde virtuele machine zijn onder andere:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2
* Standard_D15_v2

U kunt meer informatie over elke beschikbare geïsoleerde grootte [hier](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory).

### <a name="hyper-v--root-os-isolation-between-root-vm--guest-vms"></a>Hyper-V & basis OS isolatie tussen de basis-VM & Gast-VM 's
Het Azure compute-platform is gebaseerd op machine netwerkvirtualisatie, wat wil zeggen dat alle code van de klant wordt uitgevoerd in een Hyper-V virtuele machine. Voor elke Azure-knooppunt (of netwerkeindpunt) is er een Hypervisor die wordt uitgevoerd boven op de hardware en verdeelt een knooppunt in het nummer van een variabele van de Gast virtuele Machines (VM's).


![Hyper-V & basis OS isolatie tussen de basis-VM & Gast-VM 's](./media/azure-isolation/azure-isolation-fig4.jpg)


Elk knooppunt heeft ook een speciale basis-VM, die het Host-besturingssysteem wordt uitgevoerd. De grens van een kritieke is de isolatie van de basis-VM op basis van de Gast-VM's en de Gast-VM's van elkaar, beheerd door de hypervisor en de basis-besturingssysteem. De hypervisor/root OS koppeling maakt gebruik van Microsofts tientallen jaren ervaring besturingssysteem en meer recente learning van Microsoft Hyper-V voor sterke isolatie van Gast-VM's.

Voor het Azure-platform wordt gebruikgemaakt van een gevirtualiseerde omgeving. Gebruikersexemplaren functioneren als zelfstandige virtuele machines die geen toegang tot een fysieke host-server en deze isolatie wordt afgedwongen met behulp van de fysieke processor-bevoegdheidsniveaus (ring-0/ring 3).

Ring 0 is het niveau met de meeste bevoegdheden en ring 3 het niveau met de minste bevoegdheden. Het gastbesturingssysteem wordt uitgevoerd in een Ring 1 met minder bevoegdheden en toepassingen worden uitgevoerd in de minste bevoegdheden Ring 3. Deze virtualisatie van fysieke resources leidt tot een duidelijke scheiding tussen het gastbesturingssysteem en de hypervisor, wat zorgt voor een extra veiligheidsbarrière tussen de twee.

De Azure-hypervisor fungeert als een microkernel en geeft de aanvragen voor alle hardware van virtuele machines voor gasten op de host voor verwerking met behulp van een gedeeld-Geheugeninterface VMBus genoemd. Dit voorkomt dat gebruikers onbeperkt toegang krijgen tot het systeem om gegevens te lezen en te schrijven of programma’s uit te voeren en vermindert de risico’s die kleven aan het delen van systeemresources.

### <a name="advanced-vm-placement-algorithm--protection-from-side-channel-attacks"></a>Geavanceerde algoritmen voor VM-plaatsing en bescherming tegen aanvallen van side-channel
Een cross-VM-aanval bestaat uit twee stappen: een virtuele machine van kwaadwillende persoon worden beheerd op dezelfde host als een van de virtuele machines van het slachtoffer te plaatsen en vervolgens overtredingen van de scheiding tussen slachtoffer vertrouwelijke gegevens stelen of invloed hebben op de prestaties voor greed of vandalisme. Microsoft Azure biedt bescherming bij beide stappen met behulp van een geavanceerd algoritme voor VM-plaatsing en bescherming tegen alle bekende kant kanaal aanvallen inclusief ruis VM's.

### <a name="the-azure-fabric-controller"></a>De Azure-Infrastructuurcontroller
De Azure-Infrastructuurcontroller is verantwoordelijk voor het toewijzen van infrastructuurresources aan tenant-workloads en beheert Unidirectioneel communicatie van de host naar virtuele machines. De virtuele machine brengen algoritme van de Azure-infrastructuurcontroller is uiterst geavanceerde en nagenoeg onmogelijk is om te voorspellen als fysieke hostniveau.

![De Azure-Infrastructuurcontroller](./media/azure-isolation/azure-isolation-fig5.png)

De Azure-hypervisor dwingt geheugen en het processcheiding tussen virtuele machines en deze veilig netwerkverkeer omgeleid naar gast OS tenants. Dit elimineert de mogelijkheid om en aan clientzijde kanaal aanval op het niveau van de virtuele machine.

In Azure, de root-VM is bijzonder: het besturingssysteem wordt uitgevoerd een beperkt de hoofdmap OS met de naam die als host fungeert voor een fabric-agent (m). FAs zijn binnen de gastbesturingssystemen op klant virtuele machines op zijn beurt gebruikt voor het beheren van gastagents (GA). FAs ook beheren storage-knooppunten.

De verzameling van Azure-hypervisor, hoofd-OS/FA en klant virtuele machines/GAs bestaat uit een rekenknooppunt. FAs worden beheerd door een infrastructuurcontroller (FC), die buiten de reken- en knooppunten (reken- en -clusters worden beheerd door afzonderlijke FCs) bestaat. Als een klant van hun toepassing configuratiebestand bijwerkt terwijl deze wordt uitgevoerd, wordt de FC communiceert met de VA, dit neemt vervolgens contact op met GAs, de toepassing van de wijziging in de configuratie die op de hoogte stellen. In het geval van een hardwarestoring optreedt, de FC automatisch vinden van de beschikbare hardware en opnieuw opstarten van de virtuele machine er.

![Azure-Infrastructuurcontroller](./media/azure-isolation/azure-isolation-fig6.jpg)

Communicatie van een Infrastructuurcontroller naar een agent is Unidirectioneel. De agent implementeert een beveiligd met een SSL-service die alleen op aanvragen van de netwerkcontroller reageert. Deze kan niet verbindingen met de domeincontroller of andere bevoegde interne knooppunten starten. De FC worden alle antwoorden behandeld alsof ze zich niet-vertrouwd.


![Infrastructuurcontroller](./media/azure-isolation/azure-isolation-fig7.png)

Isolatie is een uitbreiding van de basis-VM van Gast-VM's en de Gast-VM's van elkaar. COMPUTE-knooppunten zijn ook geïsoleerd van de storage-knooppunten voor een betere bescherming.


De hypervisor en de host die OS netwerkpakket - filters om u te helpen ervoor te zorgen bieden dat niet-vertrouwde virtuele machines kan platformonderdelen genereren of verkeer niet wordt besproken, ontvangen verkeer naar beveiligde infrastructuur-eindpunten of verzenden/ontvangen ongepast broadcastverkeer.


### <a name="additional-rules-configured-by-fabric-controller-agent-to-isolate-vm"></a>Aanvullende regels die zijn geconfigureerd door de Infrastructuurcontroller-Agent voor het isoleren van de virtuele machine
Standaard worden alle verkeer wordt geblokkeerd als een virtuele machine is gemaakt en vervolgens de infrastructuurcontroller-agent het pakketfilter om toe te voegen regels en uitzonderingen waarmee geautoriseerd verkeer geconfigureerd.

Er zijn twee categorieën regels die zijn geprogrammeerd:

-   **Regels van configuratie of de infrastructuur van de computer:** standaard wordt alle communicatie geblokkeerd. Er zijn uitzonderingen om toe te staan een virtuele machine te verzenden en ontvangen van DHCP en DNS-verkeer. Virtuele machines kunnen ook verkeer verzenden naar het 'openbare' internet en verkeer verzenden naar andere virtuele machines binnen hetzelfde Azure-netwerk en de activeringsserver van het besturingssysteem. De virtuele machines lijst met toegestane uitgaande bestemmingen bevat geen Azure-routersubnetten, Azure-beheer en andere eigenschappen van Microsoft.

-   **Configuratiebestand van de rol:** Hiermee definieert u de binnenkomende toegangsbeheerlijsten (ACL's) op basis van het servicemodel van de tenant.

### <a name="vlan-isolation"></a>VLAN-isolatie
Er zijn drie VLAN's in elke cluster:

![VLAN-isolatie](./media/azure-isolation/azure-isolation-fig8.jpg)


-   Het belangrijkste VLAN – koppelt klant niet-vertrouwde knooppunten

-   De VLAN FC-bevat vertrouwde FCs en ondersteunende systemen

-   Het apparaat VLAN-bevat vertrouwd netwerk en andere infrastructuurapparaten

Communicatie met de belangrijkste VLAN van de FC-VLAN is toegestaan, maar met de FC-VLAN van de belangrijkste VLAN kan niet worden geïnitieerd. Communicatie met het apparaat VLAN ook geblokkeerd van de belangrijkste VLAN. Dit zorgt ervoor dat deze knooppunten op de FC of het apparaat VLAN's kan niet, zelfs als een knooppunt met klantcode ermee is geknoeid aanvallen.

## <a name="storage-isolation"></a>Opslag-isolatie
### <a name="logical-isolation-between-compute-and-storage"></a>Logische isolatie tussen rekencapaciteit en opslag
Als onderdeel van het ontwerp van de fundamentele scheidt Microsoft Azure berekening op basis van een virtuele machine uit de opslag. Dankzij deze scheiding kunt gegevensverwerking en opslag onafhankelijk schalen waardoor het gemakkelijker wordt om meerdere tenants en isolatie te bieden.

Daarom Azure Storage wordt uitgevoerd op afzonderlijke hardware met geen verbinding met het netwerk naar het Azure Compute, behalve logisch. [Dit](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf) betekent dat wanneer een virtuele schijf wordt gemaakt, wordt schijfruimte niet voor de volledige capaciteit toegewezen. In plaats daarvan wordt een tabel die is toegewezen adressen op de virtuele schijf aan sectoren op de fysieke schijf gemaakt en die tabel in eerste instantie leeg is. **De eerste keer dat een klant Hiermee worden gegevens op de virtuele schijf geschreven ruimte op de fysieke schijf wordt toegewezen, en een verwijzing naar deze wordt geplaatst in de tabel.**
### <a name="isolation-using-storage-access-control"></a>Isolatie met behulp van Storage Access control
**Toegangsbeheer in Azure Storage** heeft een eenvoudig model voor toegangsbeheer. Elk Azure-abonnement kan een of meer Opslagaccounts maken. Elk Opslagaccount heeft een enkel geheime sleutel die wordt gebruikt voor het beheren van toegang tot alle gegevens in dat Opslagaccount.

![Isolatie met behulp van Storage Access control](./media/azure-isolation/azure-isolation-fig9.png)

**Toegang tot Azure Storage-gegevens (inclusief tabellen)** kan worden beheerd via een [SAS (Shared Access Signature)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) token, welke verleent toegang binnen het bereik. De SAS is gemaakt via een querysjabloon (URL), ondertekend met de [SAK (Opslagaccountsleutel)](https://msdn.microsoft.com/library/azure/ee460785.aspx). Die [ondertekend URL](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) kunt krijgen tot een ander proces (dat wordt gedelegeerd), die u kunt vervolgens Vul de details van de query en het maken van de aanvraag van de storage-service. Een SAS kunt u op basis van tijd om toegang te verlenen aan clients zonder te onthullen geheime sleutel van het storage-account.

De SAS betekent dat we een client beperkte machtigingen voor objecten in onze storage-account voor een bepaalde periode en met een opgegeven set machtigingen kunt verlenen. We kunnen deze beperkte machtigingen verlenen zonder dat u hoeft voor het delen van de toegangssleutels van uw account.

### <a name="ip-level-storage-isolation"></a>IP-opslag op isolatie
U kunt tot stand brengen firewalls en een IP-adresbereik definiëren voor uw vertrouwde clients. Een IP-adresbereik, alleen clients met een IP-adres binnen het gedefinieerde bereik kunnen verbinding maken met [Azure Storage](https://docs.microsoft.com/azure/storage/storage-security-guide).

IP-opslaggegevens kunnen worden beveiligd tegen onbevoegde gebruikers via een netwerk mechanisme dat wordt gebruikt voor het toewijzen van een toegewezen rekenknooppunten of toegewezen tunnel van verkeer naar de IP-opslag.

### <a name="encryption"></a>Versleuteling
Azure biedt de volgende typen versleuteling om gegevens te beveiligen:
-   Versleuteling tijdens overdracht

-   Versleuteling 'at rest'

#### <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht
Versleuteling tijdens overdracht is een mechanisme om gegevens te beveiligen wanneer deze worden verzonden tussen netwerken. Met Azure Storage, kunt u beveiligen met behulp van gegevens:

-   [Versleuteling op transportniveau](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), zoals HTTPS wanneer u gegevens naar of uit Azure Storage overbrengen.

-   [Wire-versleuteling](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), zoals SMB 3.0-versleuteling voor Azure-bestandsshares.

-   [Clientversleuteling](https://docs.microsoft.com/azure/storage/storage-security-guide#using-client-side-encryption-to-secure-data-that-you-send-to-storage), voor het versleutelen van de gegevens voordat deze wordt overgedragen naar de opslag en de gegevens nadat deze zijn overgebracht geen opslagruimte meer te ontsleutelen.

#### <a name="encryption-at-rest"></a>Versleuteling-at-Rest
Voor veel organisaties [gegevensversleuteling in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is een verplichte stap voor privacy, naleving en onafhankelijkheid van gegevens. Er zijn drie Azure-functies die bieden van versleuteling van gegevens die zich 'in rust':

-   [Storage-Serviceversleuteling](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-at-rest) Hiermee kunt u aanvragen dat de storage-service gegevens automatisch versleuteld bij het schrijven van deze naar Azure Storage.

-   [Clientversleuteling](https://docs.microsoft.com/azure/storage/storage-security-guide#client-side-encryption) biedt ook de functie van versleuteling-at-rest.

-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) kunt u voor het versleutelen van de besturingssysteemschijven en gegevensschijven die zijn gebruikt door een IaaS-virtuele machine.

#### <a name="azure-disk-encryption"></a>Azure Disk Encryption
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) voor virtuele machines (VM's) kunt u adres organisatie beveiligings- en nalevingsvereisten voldoet door het versleutelen van de VM-schijven (met inbegrip van de opstart- en schijven) met sleutels en beleidsregels die u beheert in [Azure-sleutel Kluis](https://azure.microsoft.com/services/key-vault/).

Het Disk Encryption-oplossing voor Windows is gebaseerd op [Microsoft BitLocker-stationsversleuteling](https://technet.microsoft.com/library/cc732774.aspx), en de Linux-oplossing is gebaseerd op [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

De oplossing ondersteunt de volgende scenario's voor IaaS-VM's als ze zijn ingeschakeld in Microsoft Azure:
-   Integratie met Azure Key Vault

-   Standard-laag virtuele machines: A, D, DS, G, GS, enzovoort, reeks IaaS-VM's

-   Inschakelen van versleuteling op Windows en Linux IaaS-VM 's

-   Uitschakelen van versleuteling op besturingssysteem en schijven voor Windows IaaS-VM 's

-   Codering op gegevensstations voor Linux IaaS-VM's uitschakelen

-   Inschakelen van versleuteling op IaaS-VM's met Windows-clientbesturingssysteem

-   Versleuteling voor volumes met koppelpunt paden is ingeschakeld

-   Inschakelen van versleuteling op Linux-VM's die zijn geconfigureerd met schijf striping (RAID) met behulp van [mdadm](https://en.wikipedia.org/wiki/Mdadm)

-   Inschakelen van versleuteling op Linux-VM's met behulp van [LVM (Logical Volume Manager)](https://msdn.microsoft.com/library/windows/desktop/bb540532) voor gegevensschijven

-   Inschakelen van versleuteling op Windows-VM's die zijn geconfigureerd met behulp van opslagruimten

-   Alle Azure openbare regio's worden ondersteund

De oplossing ondersteunt niet de volgende scenario's, functies en -technologie in de release:

-   Basic-laag IaaS-VM 's

-   Uitschakelen van versleuteling op een station van het besturingssysteem voor Linux IaaS-VM 's

-   IaaS-VM's die zijn gemaakt met behulp van de klassieke methode voor het maken van de virtuele machine

-   Integratie met uw on-premises Key Management Service

-   Azure Files (gedeelde bestandssysteem), Network File System (NFS), dynamische volumes en virtuele machines van Windows die zijn geconfigureerd met software gebaseerde RAID-systemen

## <a name="sql-azure-database-isolation"></a>SQL Azure Database-isolatie
SQL Database is een relationele database-service in de Microsoft Cloud op basis van de toonaangevende Microsoft SQL Server-engine en is in staat bedrijfskritieke workloads af te handelen. SQL Database biedt voorspelbare gegevensisolatie op accountniveau, Geografie / de regio op basis van en in het netwerk, allemaal met praktisch zonder beheer.

### <a name="sql-azure-application-model"></a>SQL Azure-toepassingsmodel

[Microsoft SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started) Database is een cloud-gebaseerde relationele database-service die is gebouwd op SQL Server-technologieën. Het biedt een maximaal beschikbare, schaalbare, multitenant databaseservice die wordt gehost door Microsoft in de cloud.

Vanuit het perspectief van een toepassing SQL Azure biedt de volgende hiërarchie: elk niveau heeft een-op-veel-containment van onderliggende niveaus.

![SQL Azure-toepassingsmodel](./media/azure-isolation/azure-isolation-fig10.png)

Het account en het abonnement zijn Microsoft Azure-platform concepten om facturering en beheer te koppelen.

Logische servers en databases zijn SQL Azure-specifieke concepten en worden beheerd met behulp van SQL Azure, de opgegeven OData- en TSQL-interfaces of via SQL Azure-portal die geïntegreerd in Azure portal.

SQL Azure-servers zijn niet fysiek of VM-exemplaren, in plaats daarvan zijn verzamelingen van databases, delen van beleidsregels voor beheer en beveiliging, die zijn opgeslagen in een zogenaamde 'logische hoofddatabase' database.

![SQL Azure](./media/azure-isolation/azure-isolation-fig11.png)

Logische-hoofddatabases zijn onder andere:

-   SQL-aanmeldingen die zijn gebruikt voor verbinding met de server

-   Firewall-regels

Facturering en gebruik gerelateerde informatie voor SQL Azure-databases van dezelfde logische server niet gegarandeerd worden op dezelfde fysieke instantie in SQL Azure-cluster, in plaats daarvan toepassingen moet de naam van doeldatabase opgeven wanneer u verbinding maakt.

Vanuit het oogpunt van de klant een logische server gemaakt in een geo-grafische regio, terwijl de werkelijke het maken van de server in een van de clusters in de regio gebeurt.

### <a name="isolation-through-network-topology"></a>Isolatie door middel van de topologie van netwerk

Wanneer een logische server is gemaakt en de DNS-naam is geregistreerd, de DNS-naam verwijst naar het zogenaamde 'VIP Gateway'-adres in het specifieke Datacenter waar de server is geplaatst.

Achter het VIP (virtuele IP-adres) hebben we een verzameling van staatloze gateway-services. Gateways ophalen in het algemeen betrokken wanneer er coördinatie tussen meerdere gegevensbronnen (master-database, gebruiker-database, enzovoort). Gateway-services implementeren de volgende:
-   **TDS-verbinding via een proxy.** Dit omvat gebruikersdatabase vinden in het back-end-cluster, de aanmeldingssequentie implementeren en stuurt de TDS-pakketten naar de back-end en terug.

-   **Database-beheer.** Dit omvat het implementeren van een verzameling van werkstromen CREATE/ALTER/DROP databasebewerkingen uitvoeren. De databasebewerkingen kunnen worden aangeroepen door sniffing TDS-pakketten of expliciete OData APIs.

-   CREATE/ALTER/DROP login/gebruiker

-   Logische server beheerbewerkingen via OData-API

![Isolatie door middel van de topologie van netwerk](./media/azure-isolation/azure-isolation-fig12.png)

De laag achter de gateways heet 'back-end'. Dit is waar alle gegevens worden opgeslagen in een maximaal beschikbare manier. Elke hoeveelheid gegevens wordt gezegd dat deel uitmaken van een 'partitie' of 'failover-eenheid', elk met ten minste drie replica's. Replica's worden opgeslagen en gerepliceerd door SQL Server-engine en beheerd door een failover-systeem vaak aangeduid als 'infrastructuur'.

Over het algemeen communiceert de back-endsysteem niet uitgaand naar andere systemen als veiligheidsmaatregel. Dit is gereserveerd voor de systemen in de laag front-end (gateway). De laag gatewaymachines hebben beperkte rechten op de back-end-machines om de kwetsbaarheid voor aanvallen als een mechanisme in ingrijpende minimaliseren.

### <a name="isolation-by-machine-function-and-access"></a>Isolatie van Machine-functie- en toegangsbeheer
SQL Azure (is samengesteld uit de services die worden uitgevoerd op een andere computer functies. SQL Azure is onderverdeeld in 'back-end'-Database in de Cloud als 'front-end' (Gateway/Management)-omgevingen, met het algemene principe van verkeer alleen gaan in de back-end en niet uit. De front-end-omgeving kan communiceren met de buitenwereld van andere services en in het algemeen alleen beperkte machtigingen heeft in de back-end (voldoende voor het aanroepen van de toegangspunten nodig om aan te roepen).

## <a name="networking-isolation"></a>Netwerken isolatie
Azure-implementatie heeft meerdere lagen van netwerkisolatie. Het volgende diagram toont de verschillende lagen van Azure klanten biedt netwerkisolatie. Deze lagen zijn zowel systeemeigen in de Azure-platform zelf wordt geboden door de klant gedefinieerde functies. Inkomende via Internet, Azure DDoS biedt isolatie op basis van grootschalige aanvallen op Azure. De volgende isolatielaag is door de klant gedefinieerde openbare IP-adressen (eindpunten), die worden gebruikt om te bepalen welk verkeer via de service in de cloud kunt doorgeven aan het virtuele netwerk. Systeemeigen Azure virtuele netwerkisolatie garandeert volledige isolatie van alle andere netwerken, en dat verkeer alleen via paden van de gebruiker die is geconfigureerd en methoden stromen. Deze paden en methoden zijn de volgende laag, waar de nsg's, UDR en virtuele netwerkapparaten kunnen worden gebruikt om te maken van isolatiegrenzen ter bescherming van de implementaties van toepassingen in het beveiligde netwerk.

![Netwerken isolatie](./media/azure-isolation/azure-isolation-fig13.png)

**Isolatie van verkeer:** A [virtueel netwerk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) is de verkeersisolatiegrens op het Azure-platform. Virtuele machines (VM's) in één virtueel netwerk kan niet communiceren rechtstreeks op virtuele machines in een ander virtueel netwerk, zelfs als beide virtuele netwerken worden gemaakt van dezelfde klant. Isolatie is een belangrijke eigenschap die ervoor zorgt dat de klant-VM's en communicatie blijft privé binnen een virtueel netwerk.

[Subnet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#subnets) biedt een extra isolatielaag met in het virtuele netwerk op basis van IP-bereik. IP-adressen in het virtuele netwerk, kunt u een virtueel netwerk verdelen in meerdere subnetten voor organisatie- en beveiligingsdoeleinden. Tussen VM's en PaaS-rolexemplaren die in (dezelfde of verschillende) subnetten in een VNET zijn geïmplementeerd, is communicatie mogelijk zonder extra configuratie. U kunt ook configureren [netwerkbeveiligingsgroep (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#network-security-groups-nsg) wilt toestaan of weigeren van netwerkverkeer naar een VM-exemplaar op basis van regels die zijn geconfigureerd in de toegangsbeheerlijst (ACL) van de NSG. NSG's kunnen worden gekoppeld aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Als een NSG is gekoppeld aan een subnet, zijn de ACL-regels van toepassing op alle VM-exemplaren in dat subnet.

## <a name="next-steps"></a>Volgende stappen

- [Opties voor netwerkisolatie voor Machines in Windows Azure-netwerken](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/)

Dit omvat het klassieke front-end en back-end scenario waar de machines in een specifieke back-endnetwerk of subnetwerk kunnen alleen toestaan bepaalde clients of andere computers verbinding maken met een bepaald eindpunt op basis van een lijst met toegestane IP-adressen.

- [COMPUTE-isolatie](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure biedt een verschillende cloud-gebaseerde computing services met een ruime keuze aan rekenprocessen en -services die u omhoog en omlaag automatisch schalen kunnen om te voldoen aan de behoeften van uw toepassing of enterprise.

- [Opslag-isolatie](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure scheidt klant berekening op basis van een virtuele machine uit de opslag. Dankzij deze scheiding kunt gegevensverwerking en opslag onafhankelijk schalen waardoor het gemakkelijker wordt om meerdere tenants en isolatie te bieden. Daarom Azure Storage wordt uitgevoerd op afzonderlijke hardware met geen verbinding met het netwerk naar het Azure Compute, behalve logisch. Alle aanvragen uitgevoerd via HTTP of HTTPS op basis van de keuze van de klant.

