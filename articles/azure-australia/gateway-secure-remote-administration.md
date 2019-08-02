---
title: Extern beheer van uw gateway in azure Australia beveiligen
description: Richt lijnen voor het configureren van veilig extern beheer binnen de Australische regio's om te voldoen aan de specifieke vereisten van het beleid voor de Australische overheid, de regelgeving en de wetgeving.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 827dffc1c7544d9373b5f8d4426ea8c448fa25ab
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571599"
---
# <a name="secure-remote-administration-of-your-gateway-in-azure-australia"></a>Extern beheer van uw gateway in azure Australia beveiligen

Het is essentieel voor de beschik baarheid en integriteit van elk systeem dat administratieve activiteiten veilig worden uitgevoerd en worden beheerd. Beheer activiteiten moeten worden uitgevoerd vanaf een beveiligd apparaat, via een beveiligde verbinding en worden ondersteund door krachtige processen voor verificatie en autorisatie. Beveiligd extern beheer zorgt ervoor dat alleen geautoriseerde acties worden uitgevoerd en alleen door geautoriseerde beheerders.

In dit artikel vindt u informatie over het implementeren van een veilige externe beheer functie voor een Internet toegankelijk systeem dat wordt gehost in Azure en dat wordt afgestemd op de ACSC-richt lijnen van het Australische Cyber Security Center (de gebruikers handleiding) en de bedoeling van de informatie van de ACSC Beveiligings handboek (ISM).

## <a name="australian-cyber-security-centre-acsc-requirements"></a>Vereisten voor het Australische Cyber Security Center (ACSC)

De algemene beveiligings vereisten voor de Gemenebests systemen worden gedefinieerd in de ISM. Voor de ondersteuning van gemenebests entiteiten bij het bieden van beveiligd beheer [heeft de ACSC de ACSC-beveiliging gepubliceerd: Beveiligd beheer](https://www.acsc.gov.au/publications/protect/secure-administration.htm)

In dit document wordt het belang van veilig beheer besproken en wordt een methode voor het implementeren van een beveiligde beheer omgeving voorgesteld. In het document worden de elementen van een veilige beheer oplossing als volgt beschreven:

|Element   |Description   |
|---|---|
|Privileged Access Control   |Het beheren van de toegang tot geprivilegieerde accounts is een fundamenteel beveiligings beheer dat geprivilegieerde accounts van misbruik kan beveiligen. De toegangs beheer methodologie bevat de concepten van ' minste bevoegdheden ' en ' moet beschikken over ' evenals processen en procedures voor het beheren van service accounts en personeels bewegingen.   |
|Meervoudige verificatie   |Het implementeren van aanvullende factoren van verificatie naast gebruikers namen en wachtwoord zinnen, zoals fysieke tokens of Smart Cards, kunnen kritieke assets helpen beveiligen. Als een kwaadwillende persoon de referenties voor geprivilegieerde accounts beïnvloedt, kunnen de gevolgen voor het uitvoeren van een deel van de multi-factor Authentication in het begin worden verkleind.|
|Geprivilegieerde werk stations|Het gebruik van een beveiligde omgeving voor beheer taken kan leiden tot een minder risico op het netwerk dat wordt aangetast door de implementatie van aanvullende beveiligings controles.|
|Logboek registratie en controle   |Door het automatisch genereren, verzamelen en analyseren van beveiligings-en beheer gebeurtenissen op basis van werk stations, servers, netwerk apparaten en Jump boxen, kunnen inbreuken worden gedetecteerd en wordt geknoeid. Met Automation kunnen organisaties sneller reageren, waardoor de implicaties van een inbreuk worden verminderd.|
|Netwerk segmentatie en schei ding|Het segmenteren van een netwerk in logische zones, zoals verschillende beveiligings domeinen, en het scheiden van deze logische netwerken door de typen gegevens te beperken die van de ene zone naar een andere worden getransporteerd, beperkt de zijdelingse verplaatsing. Segmentatie voor komt dat een kwaadwillende persoon toegang krijgt tot extra resources.|
|Jump boxen|Een Jump box is een beveiligde RAS-server, die vaak gebruik maakt van de Extern bureaublad-services of SSH-software (Secure Shell) van micro soft. Jump boxes fungeren als stapsgewijze punt voor beheerders die toegang hebben tot essentiële systemen met alle beheer acties die worden uitgevoerd vanaf de toegewezen host.|
|

Dit artikel bevat een referentie architectuur voor de manier waarop de bovenstaande elementen kunnen worden gebruikt voor veilig beheer van systemen die zijn geïmplementeerd in Azure.

## <a name="architecture"></a>Architectuur

Het bieden van een veilige beheer mogelijkheid vereist meerdere onderdelen die samen werken om een samenhangende oplossing te vormen. In de beschik bare referentie architectuur worden de onderdelen toegewezen aan de elementen die [worden beschreven in ACSC beveiliging: Beveiligd beheer](https://www.acsc.gov.au/publications/protect/secure-administration.htm)

![Azure Secure Remote Administration-architectuur](media/remote-admin.png)

## <a name="components"></a>Onderdelen

De architectuur is ontworpen om ervoor te zorgen dat een bevoegd account alleen de benodigde machtigingen krijgt, veilig wordt geïdentificeerd en vervolgens toegang verleent tot beheer interfaces vanaf een goedgekeurd apparaat en via beveiligde communicatie mechanismen die worden beheerd en gecontroleerd.

|Oplossing| Onderdelen|Elementen|
|---|---|---|
|Beveiligde apparaten |<ul><li>Geprivilegieerd werk station</li><li>Mobiel apparaat</li><li>Microsoft Intune</li><li>Groepsbeleid</li><li>Jump server/bastion-host</li><li>Just-in-time-beheer (JIT)</li></ul> |<ul><li>Geprivilegieerde werk stations</li><li>Jump boxen</li></ul>|
|Beveiligde communicatie |<ul><li>Azure Portal</li><li>Azure VPN-gateway</li><li>Extern bureaublad-gateway (RD)</li><li>Netwerk beveiligings groepen (Nsg's)</li></ul> |<ul><li>Netwerk segmentatie en schei ding</li></ul>|
|Sterke verificatie |<ul><li>Domein controller (DC)</li><li>Azure Active Directory (Azure AD)</li><li>Network Policy Server (NPS)</li><li>Azure MFA</li></ul> |<ul><li>Meervoudige verificatie</li></ul> |
|Sterke autorisatie |<ul><li>Identiteits-en toegangs beheer (IAM)</li><li>Privileged Identity Management (PIM)</li><li>Voorwaardelijke toegang</li></ul>|<ul><li>Privileged Access Control</li></ul>|
|||

>[!NOTE]
>Voor meer informatie over het logboek registratie en controle raadpleegt u het artikel over de [logboek registratie, controle en zicht baarheid](gateway-log-audit-visibility.md) van de gateway

## <a name="administration-workflow"></a>Beheer werk stroom

Het beheren van systemen die zijn geïmplementeerd in azure, is onderverdeeld in twee afzonderlijke categorieën, waarbij de Azure-configuratie en het beheer van workloads die in azure zijn geïmplementeerd, worden beheerd. De Azure-configuratie wordt uitgevoerd via de Azure Portal en het beheer van de werk belasting wordt uitgevoerd via beheer mechanismen zoals Remote Desktop Protocol (RDP), Secure Shell (SSH) of voor PaaS-mogelijkheden met behulp van hulpprogram ma's zoals SQL Management Studio.

Toegang verkrijgen voor beheer is een proces met meerdere stappen voor de onderdelen die in de architectuur worden genoemd en die toegang nodig hebben tot de Azure Portal en de Azure-configuratie voordat toegang tot Azure-workloads kan worden gemaakt.

>[!NOTE]
> De stappen die hier worden beschreven, zijn het algemene proces met behulp van de grafische gebruikers interface (GUI)-onderdelen van Azure. Deze stappen kunnen ook worden uitgevoerd met andere interfaces, zoals Power shell.

### <a name="azure-configuration-and-azure-portal-access"></a>Toegang tot Azure-configuratie en-Azure Portal

|Stap |Description |
|---|---|
|Aanmelden bij geprivilegieerd werk station |De beheerder ondertekent het bevoegde werk station met beheerders referenties. Groepsbeleid besturings elementen voor komen dat niet-beheerders accounts worden geverifieerd bij het bevoegde werk station en voor komt u dat beheerders accounts verifiëren op werk stations die geen privileged zijn. Microsoft Intune beheert de naleving van het bevoegde werk station om ervoor te zorgen dat het up-to-date is met software patches, antimalware en andere nalevings vereisten. |
|Azure Portal aanmelden |De beheerder opent een webbrowser met de Azure Portal, die is versleuteld met behulp van Transport Layer Security (TLS), en meldt zich aan met beheerders referenties. De verificatie aanvraag wordt verwerkt via Azure Active Directory rechtstreeks of via verificatie mechanismen zoals Active Directory Federation Services (AD FS) of Pass Through-verificatie. |
|Azure MFA |Azure MFA verzendt een verificatie aanvraag naar het geregistreerde mobiele apparaat van het bevoegde account. Het mobiele apparaat wordt beheerd door intune om ervoor te zorgen dat aan de beveiligings vereisten wordt voldaan. De beheerder moet eerst worden geverifieerd bij het mobiele apparaat en vervolgens naar de Microsoft Authenticator-app met behulp van een pincode of biometrisch systeem voordat de verificatie poging is toegestaan voor Azure MFA. |
|Voorwaardelijke toegang |Beleid voor voorwaardelijke toegang Controleer de verificatie poging om ervoor te zorgen dat het voldoet aan de vereiste vereisten, zoals het IP-adres van de verbinding, het groepslid maatschap voor het bevoegde account en de beheer-en nalevings status van de geprivilegieerd werk station zoals gerapporteerd door intune. |
|Privileged Identity Management (PIM) |Via de Azure Portal kan de beheerder nu activering activeren of aanvragen voor de bevoegde rollen waarvoor ze een autorisatie via PIM hebben. PIM zorgt ervoor dat geprivilegieerde accounts geen permanente beheerders bevoegdheden hebben en dat alle aanvragen voor bevoegde toegang alleen gelden voor de tijd die nodig is om beheer taken uit te voeren. PIM biedt ook logboek registratie van alle aanvragen en activeringen voor controle doeleinden. |
|Identiteits- en toegangsbeheer|Zodra het bevoegde account veilig is geïdentificeerd en rollen geactiveerd, wordt de beheerder toegang verleend tot de Azure-abonnementen en-resources waaraan ze machtigingen hebben toegewezen via identiteits-en toegangs beheer.|
|

Zodra het bevoegde account de stappen heeft voltooid om beheerders toegang te krijgen tot de Azure Portal, kan de toegang tot de werk belastingen worden geconfigureerd en kunnen beheer verbindingen worden gemaakt.

### <a name="azure-workload-administration"></a>Beheer van Azure-workloads

|Stap |Description|
|---|---|
|Just-in-time-toegang|Om toegang te krijgen tot virtuele machines, gebruikt de beheerder JIT om vanaf de Jump server toegang aan te vragen bij de Jump server van het RD-gateway IP-adres en RDP of SSH van de betreffende virtuele werkbelasting machines.|
|Azure VPN-gateway|De beheerder brengt nu een punt-naar-site-VPN-verbinding tot stand vanaf hun bevoegde werk station naar de Azure-VPN Gateway, waarmee certificaat verificatie wordt uitgevoerd om de verbinding tot stand te brengen.|
|RD-gateway|De beheerder probeert nu een RDP-verbinding met de Jumpserver met de RD-gateway die in de Verbinding met extern bureaublad configuratie is opgegeven. De RD-gateway heeft een privé-IP-adres dat bereikbaar is via de Azure VPN Gateway-verbinding. Beleids regels op het RD-gateway bepalen of het bevoegde account toegang heeft tot de aangevraagde Jump server. De RD-gateway vraagt de beheerder om referenties en stuurt de verificatie aanvraag door naar de Network Policy Server (NPS).|
|Network Policy Server (NPS)|De NPS ontvangt de verificatie aanvraag van de RD-gateway en valideert de gebruikers naam en het wacht woord voor Active Directory voordat een aanvraag naar Azure Active Directory wordt verzonden om een Azure MFA-verificatie aanvraag te activeren.|
|Azure MFA|Azure MFA verzendt een verificatie aanvraag naar het geregistreerde mobiele apparaat van het bevoegde account. Het mobiele apparaat wordt beheerd door intune om ervoor te zorgen dat aan de beveiligings vereisten wordt voldaan. De beheerder moet eerst worden geverifieerd bij het mobiele apparaat en vervolgens naar de Microsoft Authenticator-app met behulp van een pincode of biometrisch systeem voordat de verificatie poging is toegestaan voor Azure MFA.|
|Jump-server|Nadat de verificatie is gelukt, wordt de RDP-verbinding met Transport Layer Security (TLS) versleuteld en vervolgens via de versleutelde IPSec-tunnel naar de Azure-VPN Gateway verzonden via de RD-gateway en de Jump-server. Vanaf de Jump-server kan de beheerder nu RDP of SSH uitvoeren voor virtuele workload-machines zoals opgegeven in de JIT-aanvraag.|
|

## <a name="general-guidance"></a>Algemene richtlijnen

Bij het implementeren van de onderdelen die in dit artikel worden vermeld, zijn de volgende algemene richt lijnen van toepassing:

* Valideer de beschik baarheid van de regio van services, zodat alle gegevens binnen de toegestane locaties blijven en op AU-centraal of AU-centraal 2 worden geïmplementeerd als de eerste voor keur voor beveiligde werk belastingen

* Raadpleeg het *Azure-ACSC-certificerings rapport-beveiligde 2018-* publicatie voor de certificerings status van afzonderlijke services en voer zelf beoordelingen uit op alle relevante onderdelen die niet in het rapport zijn opgenomen volgens de *ACSC-hand leiding voor gebruikers. Microsoft Azure op beveiligd*

* Controleer de netwerk verbinding en de benodigde proxy configuratie voor toegang tot de benodigde verificatie onderdelen, zoals Azure AD, ADFS en PTA

* Azure Policy gebruiken om naleving van vereisten te controleren en af te dwingen

* Zorg ervoor dat virtuele machines, met name Active Directory-domein-controllers, worden opgeslagen in een versleutelde opslag account en gebruik Azure Disk Encryption

* Maak en onderhoud robuuste identiteits-en beheer bevoegdheden beheer processen en governance om de technische controles te onderbouwen die in dit artikel worden vermeld

|Resource|URL|
|---|---|
|Documenten voor Australische regelgeving en beleids regels voor naleving|[Documenten voor Australische regelgeving en beleids regels voor naleving](https://aka.ms/au-irap)|
|Azure-producten: Australische regio's en niet-regionaal|[Azure-producten: Australische regio's en niet-regionaal](https://azure.microsoft.com/global-infrastructure/services/?regions=non-regional,australia-central,australia-central-2,australia-east,australia-southeast)|
|Strategieën voor het beperken van Cyber-beveiligings incidenten|[Strategieën voor het beperken van Cyber-beveiligings incidenten](https://acsc.gov.au/infosec/mitigationstrategies.htm)|
|ACSC beveiligen: Beveiligd beheer|[ACSC beveiligen: Beveiligd beheer](https://acsc.gov.au/publications/protect/secure-administration.htm)|
|Procedure: De infrastructuur van uw extern bureaublad-Gateway met behulp van de extensie voor Network Policy Server (NPS) en Azure AD integreren|[RD-gateway integreren met NPS en Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-nps-extension-rdg)|

## <a name="component-guidance"></a>Onderdeel richtlijnen

Deze sectie bevat informatie over het doel van elk onderdeel en de bijbehorende rol in de algehele beveiligde externe beheer architectuur. Aanvullende koppelingen worden gegeven voor toegang tot nuttige bronnen, zoals naslag documentatie, hand leidingen en zelf studies.

## <a name="secure-devices"></a>Beveiligde apparaten

De fysieke apparaten die door bevoegde gebruikers worden gebruikt om beheer functies uit te voeren, zijn waardevolle doelen voor kwaad aardige actors. Het behoud van de beveiliging en integriteit van de fysieke apparaten en ervoor te zorgen dat ze vrij zijn van schadelijke software en dat ze kunnen worden beschermd tegen inbreuken is een belang rijk onderdeel van het leveren van een veilige externe beheer mogelijkheid. Dit omvat een beveiligings configuratie met hoge prioriteit zoals beschreven in de essentiële acht strategieën van de ACSC om te voor komen dat Cyber beveiligings incidenten, zoals toepassings white list, patches voor toepassingen, toepassings beveiliging en patching van besturings systemen. Deze mogelijkheden moeten worden geïnstalleerd, geconfigureerd, gecontroleerd, gevalideerd en gerapporteerd op om ervoor te zorgen dat de status van een apparaat compatibel is met de organisatie vereisten.

### <a name="privileged-workstation"></a>Geprivilegieerd werk station

Het bevoegde werk station is een harde computer die kan worden gebruikt om beheer taken uit te voeren en is alleen toegankelijk voor beheerders accounts. Het bevoegde werk station moet beleid en configuratie hebben om de software die kan worden uitgevoerd te beperken. de toegang tot netwerk bronnen en Internet en referenties moet worden beschermd in het geval dat het apparaat wordt gestolen of geknoeid. |

|Resources|Koppelen|
|---|---|
|Overzicht van de architectuur van privileged Access workstations|[https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)|
|Naslag materiaal over bevoegde toegang beveiligen|[https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)|
|

### <a name="mobile-device"></a>Mobiel apparaat

Een mobiel apparaat is langer risico op onbedoeld verlies of dief stal als gevolg van de draag baarheid en grootte, en moet op de juiste manier worden beveiligd. Het mobiele apparaat biedt een sterke extra factor voor verificatie, omdat deze de mogelijkheid heeft om verificatie te afdwingen voor toegang tot het apparaat, traceer baarheid via locatie Services, versleutelings functies en de mogelijkheid om op afstand te wissen. Wanneer u een mobiel apparaat gebruikt als een extra verificatie factor voor Azure, moet het apparaat worden geconfigureerd voor het gebruik van de Microsoft Authenticator-app met pincode of biometrische verificatie en niet via telefoon gesprekken of SMS-berichten.

|Resources|Koppelen|
|---|---|
|Azure AD-verificatie methoden|[https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-methods](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-methods)|
|De Microsoft Authenticator-app gebruiken|[https://support.microsoft.com/help/4026727/microsoft-account-how-to-use-the-microsoft-authenticator-app](https://support.microsoft.com/help/4026727/microsoft-account-how-to-use-the-microsoft-authenticator-app)|
|

### <a name="microsoft-intune"></a>Microsoft Intune

InTune is het onderdeel van Enterprise Mobility + Security waarmee mobiele apparaten en apps worden beheerd. Het is nauw geïntegreerd met andere onderdelen, zoals Azure Active Directory voor identiteits-en toegangs beheer en Azure Information Protection voor gegevens bescherming. InTune biedt beleids regels voor werk stations en mobiele apparaten om nalevings vereisten in te stellen voor toegang tot resources en biedt rapportage-en controle mogelijkheden om inzicht te krijgen in de status van beheer apparaten.

|Resources|Koppelen|
|---|---|
|Documentatie over Microsoft Intune|[https://docs.microsoft.com/intune/](https://docs.microsoft.com/intune/)|
|Aan de slag met apparaatcompatibiliteit in intune|[https://docs.microsoft.com/intune/device-compliance-get-started](https://docs.microsoft.com/intune/device-compliance-get-started)|
|

### <a name="group-policy"></a>Groepsbeleid

Groepsbeleid wordt gebruikt om de configuratie van besturings systemen en toepassingen te beheren. Beveiligings beleid bepaalt de instellingen voor verificatie, autorisatie en controle van een systeem. Groepsbeleid wordt gebruikt voor het beveiligen van het bevoegde werk station, het beschermen van beheerders referenties en het beperken van niet-geautoriseerde accounts van toegang tot geprivilegieerde apparaten.

|Resources|Koppelen|
|---|---|
|Instelling voor lokaal aanmelden toestaan groepsbeleid|[https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/allow-log-on-locally](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/allow-log-on-locally)|
|

### <a name="jump-server--bastion-host"></a>Jump server/bastion-host

De Jump server/bastion-host is een gecentraliseerd punt voor beheer. Het bevat de hulpprogram ma's die nodig zijn voor het uitvoeren van beheer taken, maar heeft ook de netwerk toegang die nodig is om verbinding te maken met bronnen in beheer poorten. De Jump-server is het centrale punt voor het beheer van de werk belasting van virtuele machines in dit artikel, maar kan ook worden geconfigureerd als het toegelaten punt voor het beheren van PaaS-mogelijkheden (platform as a Service), zoals SQL. Toegang tot de PaaS-mogelijkheden kan worden beperkt per service basis met behulp van identiteits-en netwerk besturings elementen.

|Resources|Koppelen|
|---|---|
|Beveiligde beheer hosts implementeren|[https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/implementing-secure-administrative-hosts](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/implementing-secure-administrative-hosts)|
|

### <a name="just-in-time-jit-access"></a>Just-in-time-toegang

JIT is een Azure Security Center mogelijkheid om netwerk beveiligings groepen (Nsg's) te gebruiken om de toegang tot beheer protocollen zoals RDP en SSH op Virtual Machines te blok keren. Toepassingen die worden gehost op Virtual Machines blijven functioneren als normaal, maar voor het verkrijgen van beheerders toegang moet het verzoek alleen worden toegekend voor een bepaalde periode. Alle aanvragen worden geregistreerd voor controle doeleinden.

|Resources |Koppelen |
|---|---|
|Just-in-time-toegang beheren|[https://docs.microsoft.com/azure/security-center/security-center-just-in-time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)|
|Azure just-in-time-VM-toegang automatiseren|[https://blogs.technet.microsoft.com/motiba/2018/06/24/automating-azure-just-in-time-vm-access](https://blogs.technet.microsoft.com/motiba/2018/06/24/automating-azure-just-in-time-vm-access)|
|

## <a name="secure-communication"></a>Beveiligde communicatie

Communicatie verkeer voor beheer activiteiten kan uiterst gevoelige informatie bevatten, zoals beheerders referenties, en moet dan ook worden beheerd en beveiligd. Veilige communicatie biedt een betrouw bare versleutelings mogelijkheden om te voor komen dat de netwerk segmentatie en de beperkingen van het beheer verkeer naar toegestane eind punten worden beperkt en de verwerkings verplaatsing van een systeem wordt aangetast.

### <a name="azure-portal"></a>Azure Portal

De communicatie met de Azure Portal is versleuteld met behulp van Transport Layer Security (TLS) en het gebruik van de Azure Portal is gecertificeerd door de ACSC. Gemenebests entiteiten moeten de aanbevelingen volgen in de *ACSC-gebruikers handleiding* en hun webbrowsers configureren om ervoor te zorgen dat ze de meest recente versie van TLS gebruiken en met ondersteunde cryptografische algoritmen.

|Resources |Koppelen |
|---|---|
|Overzicht van Azure-versleuteling – versleuteling in transit|[https://docs.microsoft.com/azure/security/security-azure-encryption-overview#encryption-of-data-in-transit](https://docs.microsoft.com/azure/security/security-azure-encryption-overview#encryption-of-data-in-transit)|
|

### <a name="azure-vpn-gateway"></a>Azure VPN-gateway

De Azure-VPN Gateway biedt de beveiligde versleutelde verbinding van het bevoegde werk station naar Azure. De Azure-VPN Gateway is gecertificeerd door de ACSC voor het leveren van beveiligde IPSec-communicatie. Gemenebests entiteiten moeten de Azure-VPN Gateway configureren in overeenstemming met de ACSC-gebruikers handleiding, het ACSC-certificerings rapport en andere specifieke richt lijnen.

|Resources |Koppelen |
|---|---|
|Over punt-naar-site-verbindingen|[https://docs.microsoft.com/azure/vpn-gateway/point-to-site-about](https://docs.microsoft.com/azure/vpn-gateway/point-to-site-about)|
|Cryptografische Details van Azure VPN Gateway|[https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-compliance-crypto](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-compliance-crypto)|
|Configuratie van Azure VPN Gateway|[https://aka.ms/AzGovAUSecurity](https://aka.ms/AzGovAUSecurity)|
|

### <a name="remote-desktop-rd-gateway"></a>Extern bureaublad-gateway (RD)

RD-gateway is een veilig mechanisme voor het beheren en toelaten van RDP-verbindingen met systemen. Het werkt door RDP-verkeer in te kapselen in HyperText-overdrachts Protocol Secure (HTTPS) en versleuteld met behulp van TLS. TLS biedt een extra beveiligingslaag voor beheer verkeer.

|Resources |Koppelen |
|---|---|
|Extern bureaublad-services architectuur|[https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)|
|

### <a name="network-security-groups-nsgs"></a>Netwerk beveiligings groepen (Nsg's)

Nsg's functie als Access Control lijsten (Acl's) voor netwerk verkeer subnetten of virtuele machines binnenkomend of verlaten. Nsg's bieden netwerk segmentatie en bieden een mechanisme voor het beheren en beperken van de communicatie stromen die tussen systemen zijn toegestaan. Nsg's zijn een kern onderdeel van just-in-time-beheer (JIT) voor het toestaan of weigeren van toegang tot beheer protocollen.

|Resources |Koppelen |
|---|---|
|Overzicht van Azure-beveiligings groepen|[https://docs.microsoft.com/azure/virtual-network/security-overview](https://docs.microsoft.com/azure/virtual-network/security-overview)|
|Procedure: Virtuele netwerken plannen|[https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)|
|

## <a name="strong-authentication"></a>Strenge verificatie

Het is veilig om bevoegde gebruikers te identificeren voordat ze toegang krijgen tot systemen. Dit is een kern onderdeel van beveiligd beheer. Er moeten mechanismen zijn ingesteld voor het beveiligen van de referenties die zijn gekoppeld aan een bevoegd account en om te voor komen dat kwaad aardige actors toegang krijgen tot systemen via imitatie of dief stal van referenties.

### <a name="domain-controller-dc"></a>Domein controller (DC)

Op hoog niveau host een DC een kopie van de Active Directory-Data Base, die alle gebruikers, computers en groepen in een domein bevat. DCs voert verificatie uit voor gebruikers en computers. De Dc's in deze architectuur worden gehost als virtuele machines in Azure en bieden verificatie services voor geprivilegieerde accounts die verbinding maken met Jump servers en virtuele machines van werk belastingen.

|Resources |Koppelen |
|---|---|
|Overzicht van Active Directory Domain Services|[https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)|
|

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)

Azure AD is de verificatie service voor Azure. Deze bevat de Cloud

identiteiten en bieden verificatie en autorisatie voor een Azure-omgeving. Azure AD kan worden gesynchroniseren met Active Directory via Azure AD Connect en kunnen Federated Authentication via Active Directory Federation Services (AD FS) en Azure AD Connect bieden. Azure AD is een kern onderdeel van beveiligd beheer.

|Resources |Koppelen |
|---|---|
|Documentatie voor Azure Active Directory|[https://docs.microsoft.com/azure/active-directory](https://docs.microsoft.com/azure/active-directory)|
|Documentatie voor hybride identiteit|[https://docs.microsoft.com/azure/active-directory/hybrid](https://docs.microsoft.com/azure/active-directory/hybrid)|
|

### <a name="network-policy-server-nps"></a>Network Policy Server (NPS)

Een NPS is een verificatie-en beleids server die geavanceerde verificatie-en autorisatie processen biedt. De NPS-server in deze architectuur wordt verschaft om Azure MFA-verificatie te integreren met RD-gateway verificatie aanvragen. De NPS heeft een specifieke invoeg toepassing ter ondersteuning van integratie met Azure MFA in azure AD.

|Resources |Koppelen |
|---|---|
|Documentatie over Network Policy Server|[https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top)|
|

### <a name="azure-mfa"></a>Azure MFA

Azure MFA is een verificatie service in Azure Active Directory om verificatie aanvragen in te scha kelen die groter zijn dan een gebruikers naam en wacht woord voor toegang tot cloud resources zoals de Azure Portal. Azure MFA ondersteunt een reeks verificatie methoden en deze architectuur maakt gebruik van de Microsoft Authenticator-app voor verbeterde beveiliging en integratie met NPS.

|Resources |Koppelen |
|---|---|
|Hoe werkt het? Azure Multi-Factor Authentication|[https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)|
|Procedure: Azure Multi-Factor Authentication op basis van cloud implementeren|[https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)|
|

## <a name="strong-authorisation"></a>Sterke autorisatie

Zodra een bevoegd account veilig is geïdentificeerd, kan er toegang worden verleend tot resources. Autorisatie besturings elementen en beheert de bevoegdheden die aan een specifiek account zijn toegewezen. Sterke autorisatie processen worden uitgelijnd met de essentiële acht strategie van ACSC voor het beperken van Cyber-beveiligings incidenten van het beperken van beheerders bevoegdheden.

### <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

Toegang tot het uitvoeren van bevoegde acties binnen Azure is gebaseerd op rollen die aan dat account zijn toegewezen. Azure bevat een uitgebreid en gedetailleerd scala aan rollen met specifieke machtigingen voor het uitvoeren van specifieke taken. Deze rollen kunnen worden toegekend op meerdere niveaus, zoals een abonnement of resource groep. Functie toewijzing en machtigings beheer zijn gebaseerd op accounts en groepen in Azure Active Directory en wordt beheerd via Access Control (IAM) in Azure.

|Resources |Koppelen |
|---|---|
|Access Control op basis van Azure Role|[https://docs.microsoft.com/azure/role-based-access-control](https://docs.microsoft.com/azure/role-based-access-control)|
|Roldefinities begrijpen|[https://docs.microsoft.com/azure/role-based-access-control/role-definitions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions)|
|

### <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PIM)

PIM is een Azure Active Directory onderdeel waarmee de toegang tot bevoegde rollen wordt beheerd. Voor geprivilegieerde accounts is geen permanente of permanente privileged Access vereist, maar u kunt in plaats daarvan privileged Access voor een bepaalde tijd aanvragen om bevoegde activiteiten te volt ooien. PIM biedt extra besturings elementen voor het onderhouden en beperken van bevoegde toegang, evenals logboek registratie en controle om instanties van het gebruik van bevoegdheden bij te houden.

|Resources |Koppelen |
|---|---|
|Documentatie voor Privileged Identity Management (PIM)|[https://docs.microsoft.com/azure/active-directory/privileged-identity-management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management)|
|Beginnen met PIM|[https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-getting-started](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-getting-started)|
|

### <a name="conditional-access"></a>Voorwaardelijke toegang

Voorwaardelijke toegang is een onderdeel van Azure Active Directory waarmee de toegang tot resources op basis van voor waarden wordt toegestaan of geweigerd. Deze voor waarden kunnen een netwerk locatie zijn op basis van het type apparaat, de nalevings status, het groepslid maatschap en meer. Voorwaardelijke toegang wordt gebruikt om MFA, Apparaatbeheer en naleving af te dwingen via intune en groepslid maatschap van beheerders accounts.

|Resources |Koppelen |
|---|---|
|Documentatie over voorwaardelijke toegang|[https://docs.microsoft.com/azure/active-directory/conditional-access](https://docs.microsoft.com/azure/active-directory/conditional-access)|
|Procedure: Beheerde apparaten vereisen voor toegang tot Cloud-apps met voorwaardelijke toegang|[https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)|
|

## <a name="next-steps"></a>Volgende stappen

Raadpleeg het artikel over het beheer van de [Gateway ingang en het beheer](gateway-ingress-traffic.md) voor meer informatie over het beheren van verkeers stromen via uw gateway onderdelen in Azure.
