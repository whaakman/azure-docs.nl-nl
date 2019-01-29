---
title: Hybride identiteit ontwerp - strategie voor ingebruikname Azure | Microsoft Docs
description: Azure Active Directory controleert met voorwaardelijk toegangsbeheer, de specifieke voorwaarden die u bij het verifiëren van de gebruiker en voordat de toegang tot de toepassing kiezen. Als deze voorwaarden is voldaan, wordt de gebruiker geverifieerd en toegang hebben tot de toepassing.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: b92fa5a9-c04c-4692-b495-ff64d023792c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: d58f789461b761431bd86dce60b2cd059ca8df53
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164329"
---
# <a name="define-a-hybrid-identity-adoption-strategy"></a>Een strategie voor hybride identiteit ingebruikname definiëren
In deze taak definieert u de acceptatiestrategie voor hybride identiteit voor uw oplossing voor hybride identiteit om te voldoen aan de zakelijke vereisten die zijn beschreven in:

* [Bedrijfsbehoeften vaststellen](plan-hybrid-identity-design-considerations-business-needs.md)
* [Bepaal de vereisten voor directory-synchronisatie](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)
* [Bepaal de vereisten voor meervoudige verificatie](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Strategie voor zakelijke behoeften definiëren
De eerste taak adressen bepalen van het bedrijf organisaties moet.  Dit kan zijn zeer brede en bereik kneep kan optreden als u geen zorgvuldig.  In het begin, houd het eenvoudig, maar vergeet om te plannen voor een ontwerp dat wordt aangepast aan en veranderingen in de toekomst.  Ongeacht of het ontwerp voor een eenvoudige of een zeer complex zijn, Azure Active Directory is de Microsoft Identity-platform die ondersteuning biedt voor Office 365, Microsoft Online Services en cloud bewuste toepassingen.

## <a name="define-an-integration-strategy"></a>Een Integratiestrategie voor definiëren
Microsoft heeft drie belangrijkste integratiescenario's die cloud-identiteiten, gesynchroniseerde identiteiten en federatieve identiteiten zijn.  U moet plannen op overstappen op een van deze integratiestrategieën.  De strategie die u kiest kan variëren en de beslissingen bij het kiezen van een kunnen bevatten, wat voor soort gebruikerservaring die u bieden wilt, hebt u een bestaande infrastructuur en wat is de meest efficiënte.  

![](./media/plan-hybrid-identity-design-considerations/integration-scenarios.png)

De scenario's die zijn gedefinieerd in de bovenstaande afbeelding zijn:

* **Cloud-identiteiten**: dit zijn identiteiten die bestaan enkel in de cloud.  In het geval van Azure AD, zouden ze zich bevinden in het bijzonder in uw Azure AD-directory.
* **Gesynchroniseerd**: dit identiteiten die on-premises aanwezig zijn en in de cloud.  Met Azure AD Connect, worden deze gebruikers gemaakt of samengevoegd met de bestaande Azure AD-accounts.  Wachtwoord-hash van de gebruiker is gesynchroniseerd vanuit de on-premises-omgeving naar de cloud in wat een wachtwoord-hash wordt genoemd.  Wanneer met behulp van gesynchroniseerd is het een voorbehoud dat als een gebruiker is uitgeschakeld in de on-premises-omgeving, het tot drie uur voor de status van dat account duurt worden weergegeven in Azure AD.  Dit komt door het synchronisatie-interval.
* **Federatieve**: deze identiteiten bestaat zowel on-premises en in de cloud.  Met Azure AD Connect, worden deze gebruikers gemaakt of samengevoegd met de bestaande Azure AD-accounts.  

> [!NOTE]
> Lees voor meer informatie over de synchronisatie-opties, [uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md).
> 
> 

De volgende tabel helpt bij het bepalen van de voordelen en nadelen van elk van de volgende strategieën:

| Strategie | Voordelen | Nadelen |
| --- | --- | --- |
| **Cloud-identiteiten** |Gemakkelijker te beheren voor kleine organisatie. <br> Er is niets naar on-premises installeert. Er zijn geen extra hardware nodig is<br>Eenvoudig uitgeschakeld als de gebruiker het bedrijf verlaat |Gebruikers moeten zich aanmelden bij het openen van workloads in de cloud <br> Wachtwoorden al kunnen dan niet hetzelfde zijn voor de cloud en on-premises identiteiten |
| **Gesynchroniseerd** |On-premises wachtwoord wordt geverifieerd voor zowel on-premises en cloud mappen <br>Gemakkelijker te beheren voor kleine, middelgrote of grote organisaties <br>Gebruikers kunnen hebben eenmalige aanmelding (SSO) voor bepaalde resources <br> Microsoft primaire methode voor synchronisatie <br> Gemakkelijker te beheren |Sommige klanten mogelijk hun mappen synchroniseren met de cloud vanwege specifiek bedrijf politie terughoudend |
| **Federatieve** |Gebruikers kunnen hebben eenmalige aanmelding (SSO) <br>Als een gebruiker wordt beëindigd of verlaat, het account kan direct worden uitgeschakeld en de toegang ingetrokken,<br> Ondersteunt geavanceerde scenario's die niet kunnen worden bewerkstelligd met gesynchroniseerd |Meer stappen voor het instellen en configureren <br> Hogere onderhoud <br> Extra hardware nodig voor de STS-infrastructuur <br> Extra hardware voor het installeren van de federation-server nodig. Aanvullende software is vereist als AD FS wordt gebruikt <br> Uitgebreide instellingen voor eenmalige aanmelding vereisen <br> Kritieke point of failure als de federation-server niet actief is, gebruikers niet mogelijk om te verifiëren |

### <a name="client-experience"></a>Clientervaring
De strategie die u gebruikt worden bepaald door de gebruiker aanmelden ervaring.  De volgende tabellen bieden u informatie over wat de gebruikers verwachten hun aanmeldervaring dat kunnen te zijn.  Niet alle federatieve id-providers ondersteunt eenmalige aanmelding in alle scenario's.

**Lid is van een domein en privé netwerktoepassingen**:

|  | Gesynchroniseerde identiteiten | Federatieve identiteit |
| --- | --- | --- |
| Webbrowsers |Verificatie op basis van formulieren |eenmalige aanmelding, soms ook vereist voor de levering van organisatie-ID |
| Outlook |Prompt voor referenties |Prompt voor referenties |
| Skype voor bedrijven (Lync) |Prompt voor referenties |eenmalige aanmelding voor Lync, dit wordt gevraagd referenties voor Exchange |
| OneDrive voor Bedrijven |Prompt voor referenties |Eenmalige aanmelding |
| Office Professional Plus abonnement |Prompt voor referenties |Eenmalige aanmelding |

**Externe of niet-vertrouwde bronnen**:

|  | Gesynchroniseerde identiteiten | Federatieve identiteit |
| --- | --- | --- |
| Webbrowsers |Verificatie op basis van formulieren |Verificatie op basis van formulieren |
| Outlook, Skype voor bedrijven (Lync), OneDrive voor bedrijven, Office-abonnement |Prompt voor referenties |Prompt voor referenties |
| Exchange ActiveSync |Prompt voor referenties |eenmalige aanmelding voor Lync, dit wordt gevraagd referenties voor Exchange |
| Mobiele apps |Prompt voor referenties |Prompt voor referenties |

Als u hebt vastgesteld dat u in taak 1 dat u een externe id-provider of zijn te gaan naar een gebruiken voor federatie met Azure AD hebt, moet u rekening houden met de volgende ondersteunde mogelijkheden:

* Een SAML 2.0-provider dat compatibel is voor de SP-Lite-profiel kan ondersteuning voor verificatie met Azure AD en toepassingen die zijn gekoppeld
* Biedt ondersteuning voor passieve verificatie, waarbij verificatie OWA, SPO, enzovoort.
* Exchange Online-clients kunnen worden ondersteund via het SAML 2.0 uitgebreide Client profiel (ECP)

U moet ook rekening houden met welke mogelijkheden niet meer beschikbaar:

* Zonder ondersteuning voor WS-Trust/Federation verbreken alle actieve clients
  * Dit betekent dat er geen Lync-client, de client voor OneDrive, Office-abonnement, Office Mobile vóór Office 2016
* Overgang van Office op passieve verificatie kan ter ondersteuning van pure SAML 2.0 IDP's, maar ondersteuning is nog steeds op basis van de client door de client

> [!NOTE]
> Voor de meest recente lijst Lees het artikel [federatiecompatibiliteitslijst van Azure AD](how-to-connect-fed-compatibility.md).
> 
> 

## <a name="define-synchronization-strategy"></a>Synchronisatie-strategie definiëren
In deze taak definieert u de hulpprogramma's die worden gebruikt om te synchroniseren van de organisatie on-premises gegevens naar de cloud en welke topologie u moet gebruiken.  Omdat de meeste organisaties Active Directory gebruikt, wordt informatie over het gebruik van Azure AD Connect voor het adres van de bovenstaande vragen vindt u in bepaalde mate van detail.  Er is informatie over het gebruik van FIM 2010 R2- of MIM 2016 om u te helpen bij het plannen van deze strategie voor omgevingen waarvoor geen Active Directory.  Echter toekomstige versies van Azure AD Connect biedt ondersteuning voor LDAP-adreslijsten, dus is afhankelijk van de tijdlijn, deze informatie kan mogelijk helpen.

### <a name="synchronization-tools"></a>Hulpprogramma's voor synchronisatie
De afgelopen jaren hebben verschillende hulpprogramma's voor synchronisatie bestond en gebruikt voor verschillende scenario's.  Azure AD Connect is momenteel de gaat u naar het hulpprogramma naar keuze voor alle ondersteunde scenario's.  AAD Sync en DirSync zijn ook nog steeds rond en zelfs nu aanwezig zijn in uw omgeving kan worden. 

> [!NOTE]
> Lees voor de meest recente informatie over de ondersteunde functies van elk hulpprogramma, [vergelijking van hulpprogramma's voor Directory-integratie](plan-hybrid-identity-design-considerations-tools-comparison.md) artikel.  
> 
> 

### <a name="supported-topologies"></a>Ondersteunde topologieën
Bij het definiëren van een strategie voor synchronisatie, kunnen de topologie die wordt gebruikt, moet worden bepaald. Afhankelijk van de informatie die is vastgesteld in stap 2 u kunt vaststellen welke topologie de beste is om te gebruiken. De één forest, één Azure AD-topologie is de meest voorkomende en bestaat uit één Active Directory-forest en één exemplaar van Azure AD.  Dit zal worden gebruikt in een meerderheid van de scenario's en de verwachte topologie is bij het gebruik van Azure AD verbinding maken met Express-installatie, zoals wordt weergegeven in de afbeelding hieronder.

![](./media/plan-hybrid-identity-design-considerations/single-forest.png) Scenario voor één Forest het is gebruikelijk voor kleine en zelfs grote organisaties hebben meerdere forests, zoals wordt weergegeven in afbeelding 5.

> [!NOTE]
> Voor meer informatie over de verschillende on-premises en Azure AD-topologieën met Azure AD Connect sync Lees het artikel [topologieën voor Azure AD Connect](plan-connect-topologies.md).
> 
> 

![](./media/plan-hybrid-identity-design-considerations/multi-forest.png) 

Scenario voor meerdere forests

Als dit het geval is, en vervolgens de meerdere forests één Azure AD-topologie moet worden beschouwd als de volgende items voldaan wordt:

* Gebruikers hebben slechts 1 identiteit in alle forests: de unieke id gebruikerssectie hieronder wordt dit beschreven in meer detail.
* De gebruiker wordt geverifieerd op het forest waarin hun identiteit zich bevindt
* UPN en Bronanker (onveranderbare id), zijn afkomstig van dit forest
* Alle forests zijn ook toegankelijk via Azure AD Connect: dit betekent dat het hoeft niet te worden van domein toegevoegd en kan worden geplaatst in een perimeternetwerk bevindt, als dit dit is mogelijk.
* Gebruikers hebben slechts één postvak
* Het forest dat als host fungeert voor het postvak van gebruikers is de beste kwaliteit van de gegevens voor kenmerken die zichtbaar zijn in de Exchange globale adreslijst (GAL)
* Als er geen postvak op de gebruiker, kan klikt u vervolgens elk forest worden gebruikt om bij te dragen van deze waarden
* Hebt u een gekoppeld postvak, wordt er ook een ander account in een ander forest dat is gebruikt voor aanmelding bij.

> [!NOTE]
> Objecten die zijn opgenomen in zowel on-premises en in de cloud zijn 'verbonden' via een unieke id. In de context van Directory-synchronisatie, worden deze unieke id wordt de SourceAnchor genoemd. In de context van Single Sign-On, dit de ImmutableId genoemd. [Ontwerpconcepten voor Azure AD Connect](plan-connect-design-concepts.md#sourceanchor) voor meer overwegingen met betrekking tot het gebruik van SourceAnchor.
> 
> 

Als de bovenstaande niet waar zijn en u meer dan één actieve account of meer dan één postvak hebt, wordt Azure AD Connect Kies een en de andere negeren.  Als u postvakken, maar er is geen andere account hebt gekoppeld, wordt deze accounts worden niet geëxporteerd naar Azure AD en wordt die gebruiker zich niet lid is van een groep.  Dit wijkt af van hoe het is in het verleden met DirSync en is bedoeld voor een beter ondersteuning van deze scenario's voor meerdere forests. Een scenario met meerdere forests wordt weergegeven in de afbeelding hieronder.

![](./media/plan-hybrid-identity-design-considerations/multiforest-multipleAzureAD.png) 

**Meerdere forests meerdere Azure AD-scenario**

Het wordt aanbevolen dat slechts één map in Azure AD voor een organisatie, maar dit wordt ondersteund deze een 1:1-relatie tussen een Azure AD Connect-synchronisatieserver en een Azure AD-directory wordt bewaard.  Voor elk exemplaar van Azure AD moet u een installatie van Azure AD Connect.  Ook Azure AD standaard is geïsoleerd en gebruikers in een exemplaar van Azure AD niet mogelijk om te zien van gebruikers in een ander exemplaar.

Het is mogelijk en worden ondersteund in een on-premises exemplaar van Active Directory verbinden met meerdere Azure AD-mappen zoals weergegeven in de afbeelding hieronder:

![](./media/plan-hybrid-identity-design-considerations/single-forest-flitering.png) 

**Filters gebruiken om één forest-scenario**

U doet dit door het volgende moet waar zijn:

* Azure AD Connect-synchronisatieservers moeten worden geconfigureerd voor het filteren, zodat ze elk een sluiten elkaar wederzijds uit set van objecten hebben.  Dit doen, bijvoorbeeld door het bereik van elke server aan een bepaald domein of organisatie-eenheid.
* Een DNS-domein kan alleen worden geregistreerd in één Azure AD-directory zodat de UPN van de gebruikers in de on-premises AD afzonderlijke naamruimten moet gebruiken
* Gebruikers in een exemplaar van Azure AD worden alleen gebruikers uit hun-exemplaar.  Ze is niet mogelijk om te zien van gebruikers in de andere exemplaren
* Alleen een van de Azure AD-mappen hybride oplossing met de on-premises Exchange kunt inschakelen AD
* Wederzijdse exclusiviteit geldt ook voor write-back.  Hierdoor kunt sommige niet ondersteund met deze topologie omdat deze wordt ervan uitgegaan een configuratie met één on-premises dat write-back-functies.  Dit omvat:
  * Groep terugschrijven met standaardconfiguratie
  * Write-back van apparaat

Het volgende wordt niet ondersteund en niet als een van de implementatie moet worden gekozen:

* Hebt meerdere Azure AD Connect-synchronisatieservers verbinding maken met de dezelfde Azure AD-directory, zelfs als ze zijn geconfigureerd om te synchroniseren sluiten elkaar wederzijds uit set object wordt niet ondersteund
* Het wordt niet ondersteund voor het synchroniseren van de gebruiker aan meerdere Azure AD-directory's. 
* Dit wordt ook niet ondersteund als u wilt maken van een configuratiewijziging hebt doorgevoerd zodat gebruikers in een Azure AD worden weergegeven als contactpersonen in een andere Azure AD-directory. 
* Het is ook niet-ondersteunde Azure AD Connect-synchronisatie verbinding maken met meerdere Azure AD-mappen wijzigen.
* Azure AD-directory's standaard geïsoleerd. Het wordt niet ondersteund om de configuratie van Azure AD Connect-synchronisatie gegevens lezen uit een andere Azure AD-directory in een poging om het bouwen van een gemeenschappelijke en geïntegreerde GAL tussen de mappen te wijzigen. Dit wordt ook niet ondersteund voor het exporteren van gebruikers als contactpersonen aan een andere on-premises AD met behulp van Azure AD Connect-synchronisatie.

> [!NOTE]
> Als uw organisatie voorkomt computers in uw netwerk verbinding maken met Internet dat, in dit artikel bevat de eindpunten (FQDN's, IPv4 en IPv6-adresbereiken) die u moet opnemen in uw uitgaande toestaan lijsten en Zone van Internet Explorer vertrouwde Sites van client computers om te controleren of uw computers kunnen Office 365 kunnen gebruiken. Lees voor meer informatie [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
> 
> 

## <a name="define-multi-factor-authentication-strategy"></a>Multi-factor authentication-strategie definiëren
In deze taak definieert u de strategie voor meervoudige verificatie te gebruiken.  Azure multi-factor Authentication wordt geleverd in twee verschillende versies.  Een cloud-gebaseerd is en de andere is on-premises gebaseerde via de Azure MFA-Server.  Op basis van de evaluatie boven kunt u bepalen welke oplossing is de juiste oplossing voor uw strategie voor.  Gebruik de onderstaande tabel om te bepalen welke ontwerpoptie het beste voldoet aan vereisten van uw bedrijf beveiligingsvereiste:

Ontwerpopties van meerdere factoren:

| Asset om te beveiligen | MFA in de cloud | MFA on-premises |
| --- | --- | --- |
| Microsoft-apps |ja |ja |
| SaaS-apps in de app-galerie |ja |ja |
| IIS-toepassingen die zijn gepubliceerd via toepassingsproxy van Azure AD |ja |ja |
| IIS-toepassingen die niet zijn gepubliceerd via de Azure AD App Proxy |nee |ja |
| Externe toegang als VPN, RDG |nee |ja |

Hoewel u mogelijk hebt gekozen voor een oplossing voor uw strategie voor, moet u nog steeds gebruik van de evaluatie van boven op waar uw gebruikers zich bevinden.  Dit kan leiden tot de oplossing te wijzigen.  Gebruik de onderstaande tabel om te helpen u dit te bepalen:

| Gebruikerslocatie | Ontwerp van voorkeur optie |
| --- | --- |
| Azure Active Directory |Multi-FactorAuthentication in de cloud |
| Azure AD en on-premises AD dat gebruikmaakt van federatie met AD FS |Beide |
| Azure AD en on-premises AD met behulp van Azure AD Connect-geen Wachtwoordsynchronisatie |Beide |
| Azure AD en on-premises met Azure AD Connect met Wachtwoordsynchronisatie |Beide |
| On-premises AD |Multi-Factor Authentication-server |

> [!NOTE]
> U moet er ook voor zorgen dat de functies die vereist voor uw ontwerp zijn biedt ondersteuning voor de optie voor het ontwerp van multi-factor authentication die u hebt geselecteerd.  Lees voor meer informatie [kiest u de multi-factor-beveiligingsoplossing voor u](../authentication/concept-mfa-whichversion.md#what-am-i-trying-to-secure).
> 
> 

## <a name="multi-factor-auth-provider"></a>Multi-Factor Auth Provider
Multi-factor authentication is standaard beschikbaar voor hoofdbeheerders met een Azure Active Directory-tenant. Echter, als u wilt met meervoudige verificatie uitbreiden naar alle gebruikers en/of uw globale beheerders kunnen gebruik functies zoals de beheerportal, aangepaste begroetingen en rapporten uitvoeren wilt, klikt u vervolgens moet u kopen en configureren Multi-factor Authentication-Provider.

> [!NOTE]
> U moet er ook voor zorgen dat de functies die vereist voor uw ontwerp zijn biedt ondersteuning voor de optie voor het ontwerp van multi-factor authentication die u hebt geselecteerd. 
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Bepalen van de beveiligingsvereisten voor gegevens](plan-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)

