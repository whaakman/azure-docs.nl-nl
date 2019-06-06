---
title: Azure AD-Cloud voor On-Premises Workloads - Azure geregeld
description: Dit onderwerp worden beschreven gelden voor cloudbeheer voor on-premises werkbelastingen.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b7e54f6acfe1cbbe6e46fe92d132ebdaa91ff33
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742342"
---
# <a name="how-azure-ad-delivers-cloud-governed-management-for-on-premises-workloads"></a>Hoe Azure AD biedt Cloud gelden voor beheer van On-Premises Workloads

Azure Active Directory (Azure AD) is een uitgebreide identity & als een (IDaaS)-serviceoplossing die worden gebruikt door miljoenen organisaties die alle aspecten van identiteit, toegangsbeheer en beveiliging omvatten. Azure AD bevat meer dan een miljard gebruikers-id's en kan gebruikers zich aanmelden en veilig toegang krijgen tot beide:

* Externe bronnen, zoals Microsoft Office 365, Azure portal en duizenden andere Software-as-a-Service (SaaS)-toepassingen.
* Interne resources, zoals toepassingen op het bedrijfsnetwerk en het intranet, samen met alle cloudtoepassingen die zijn ontwikkeld door de organisatie van een organisatie.

Organisaties kunnen Azure AD gebruiken als ze zijn 'pure cloud', of als een 'hybride' implementatie als ze beschikken over on-premises werkbelastingen. Een hybride implementatie van Azure AD kan deel uitmaken van een strategie voor een organisatie voor het migreren van de IT-activa naar de cloud of door te gaan naar het integreren van bestaande on-premises infrastructuur en nieuwe cloudservices.

In het verleden 'hybride' organisaties die Azure AD hebt gezien, als een uitbreiding van hun bestaande on-premises infrastructuur. In deze implementaties, het beheer van on-premises identity governance, Windows Server Active Directory of andere interne directory-systemen, worden de controlepunten en gebruikers en groepen zijn gesynchroniseerd vanaf deze systemen met een clouddirectory, zoals Azure AD. Zodra de identiteiten in de cloud, kunnen ze worden beschikbaar gesteld aan Office 365, Azure en andere toepassingen.

![Levenscyclus van identiteit](media/cloud-governed-management-for-on-premises//image1.png)

Wanneer organisaties meer van hun IT-infrastructuur, samen met hun toepassingen naar de cloud, zoekt veel de verbeterde beveiliging en vereenvoudigd beheer-mogelijkheden van identity management als een service. De cloud geleverde IDaaS functies in Azure AD Versnel de overgang naar de cloud onder beheer door op te geven van de oplossingen en mogelijkheden waarmee organisaties snel vaststellen en meer van hun identiteitsbeheer van traditionele on-premises verplaatsen systemen naar Azure AD, maar blijft ondersteuning bieden voor bestaande, evenals nieuwe toepassingen.

In dit artikel bevat een overzicht van de strategie van Microsoft voor hybride IDaaS en beschrijft hoe organisaties Azure AD kunnen gebruiken voor hun bestaande toepassingen.

## <a name="the-azure-ad-approach-to-cloud-governed-identity-management"></a>De Azure AD-benadering tot cloud bestuurbaar identiteitsbeheer

Als organisaties overgang naar de cloud, moeten de zekerheid dat ze controle van de volledige omgeving - meer beveiliging en meer inzicht in activiteiten, die worden ondersteund door automation en proactief inzichten hebben. "**Cloud management geregeld**' wordt beschreven hoe organisaties inperken en hun gebruikers, toepassingen, groepen en apparaten vanuit de cloud.

Organisaties moeten kunnen effectief beheren op schaal, vanwege de verspreiding van SaaS-toepassingen en de toenemende rollen van samenwerking en externe identiteiten in deze moderne wereld. Het nieuwe landschap van risico's van de cloud, betekent dat een organisatie moet sneller te reageren: een schadelijke actor die een gebruiker van de cloud wordt aangetast invloed kan zijn op cloud en on-premises toepassingen.

In het bijzonder hybride organisaties moeten kunnen het overdragen van en het automatiseren van taken, die in het verleden IT handmatig hebt. Om taken te automatiseren, ze nodig hebben API's en bedrijfsprocessen waarvoor de levenscyclus van de andere identiteit gerelateerde resources (gebruikers, groepen, toepassingen, apparaten), zodat ze het dagelijkse beheer van deze resources aan meer personen buiten delegeren kunnen Core IT-personeel. Azure AD-adressen deze vereisten via beheer van gebruikersaccounts en systeemeigen verificatie voor gebruikers zonder on-premises infrastructuur voor identiteiten. Niet het bouwen van on-premises infrastructuur, kan organisaties die beschikken over nieuwe community's van gebruikers, zoals zakelijke partners, die niet afkomstig zijn uit hun on-premises directory, maar waarvan toegangsbeheer is van essentieel belang voor het bereiken van bedrijfsresultaten profiteren.

Bovendien management is niet compleet zonder governance--- en beheeracties in deze nieuwe omgeving is een geïntegreerd onderdeel van het identiteitssysteem in plaats van een invoegtoepassing. Identity governance biedt organisaties de mogelijkheid om te beheren van de identiteit en toegang tot de levenscyclus voor werknemers, zakelijke partners en leveranciers, en services en toepassingen.

Gebruikmaking van identiteitsbeheer gemakkelijker om in te schakelen van de organisatie om een overgang naar de cloud onder beheer, kan IT om te schalen, nieuwe uitdagingen met gasten en biedt een dieper inzicht en automation dan wat klanten met had on-premises infrastructuur. Governance in deze nieuwe wereld betekent dat de mogelijkheid voor een organisatie transparantie, zichtbaarheid en juiste besturingselementen op de toegang tot bronnen binnen de organisatie hebben. Met Azure AD beveiligingsbewerkingen en audit teams hebben zichtbaarheid van wie heeft--- en die moet hebben - toegang tot welke resources in de organisatie (op welke apparaten), wat gebruikers doen met die toegang, en of de organisatie heeft en juiste gebruikt besturingselementen voor het verwijderen of beperken van toegang in overeenstemming met het bedrijf of regelgeving beleid.

Het nieuwe model voor het beheer voordelen biedt voor organisaties met zowel SaaS- en line-of-business (LOB)-toepassingen, omdat ze eenvoudiger kunt beheren en beveiligen van toegang tot deze toepassingen zijn. Door toepassingen integreren met Azure AD, kunnen organisaties gebruiken en beheren van toegang op beide cloud en on-premises afkomstig identiteiten consistent is. Levensduurbeheer van toepassingen komt meer geautomatiseerde en Azure AD biedt uitgebreide inzichten in het gebruik van de toepassing die is niet eenvoudig haalbare in on-premises identiteitsbeheer. Via de Azure AD, Office 365-groepen en Teams selfservice-functies, kunnen organisaties eenvoudig groepen maken voor het beheer van toegang en samenwerking en toevoegen of verwijderen gebruikers in de cloud management vereisten en samenwerking.

Selecteer de juiste Azure AD-mogelijkheden voor het cloud bestuurbaar management is afhankelijk van de toepassingen die moeten worden gebruikt en hoe deze toepassingen wordt geïntegreerd met Azure AD. De volgende secties beschrijven de benaderingen voor AD-geïntegreerde toepassingen en toepassingen die gebruikmaken van federation-protocollen (bijvoorbeeld SAML, OAuth of OpenID Connect).

## <a name="cloud-governed-management-for-ad-integrated-applications"></a>Onder beheer van AD-geïntegreerde toepassingen in de cloud

Azure AD wordt verbeterd voor het beheer van een organisatie on-premises Active Directory geïntegreerde toepassingen via veilige externe toegang en voorwaardelijke toegang tot toepassingen. Bovendien biedt Azure AD ook levenscyclusbeheer voor uw account en beheer van referenties van de gebruiker bestaande AD-accounts, met inbegrip van:

* **Veilige externe toegang en voorwaardelijke toegang voor on-premises toepassingen**

Voor veel organisaties, de eerste stap bij het beheren van toegang vanuit de cloud voor on-premises AD-geïntegreerde web- en extern bureaublad-toepassingen te implementeren is het [toepassingsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) voor deze toepassingen voor beveiligen externe toegang.

Na een single sign-on bij Azure AD, kunnen gebruikers toegang tot van zowel cloud en on-premises toepassingen via een externe URL of een interne applicatie-portal. Application Proxy biedt bijvoorbeeld externe toegang en eenmalige aanmelding voor extern bureaublad, SharePoint, evenals apps, zoals Tableau en Qlik en line-of-business (LOB)-toepassingen. Beleid voor voorwaardelijke toegang kunnen bovendien bevatten weergeven van de [gebruiksvoorwaarden](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) en [ervoor te zorgen dat de gebruiker heeft ingestemd met deze](https://docs.microsoft.com/azure/active-directory/conditional-access/require-tou) voordat ze toegang kunnen krijgen tot een toepassing.

![App-Proxy-architectuur](media/cloud-governed-management-for-on-premises/image2.png)

* **Automatische levenscyclusbeheer voor Active Directory-accounts**

Identiteitsbeheer helpt organisaties bij een evenwicht tussen *productiviteit* ---hoe snel een persoon die toegang hebben tot de bronnen ze nodig hebben, zoals wanneer ze lid van de organisatie? --- en *security* ---hoe moet de toegang wijzigen gedurende een periode, zoals wanneer de werkstatus van die persoon verandert? Beheer van identiteitslevenscycli vormt de basis voor identiteitsbeheer en doeltreffend beheer op schaal vereist de infrastructuur voor identiteiten lifecycle management voor toepassingen moderniseren.

Levenscyclus van identiteit voor medewerkers is voor veel organisaties gekoppeld aan de weergave van die gebruiker in een systeem personeelsadministratie (HCM). Voor organisaties met behulp van Workday als hun HCM-systeem, Azure AD kunt controleren of de gebruikersaccounts in AD zijn [automatisch ingericht en de inrichting ongedaan gemaakt voor werknemers in Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial). Doet dit het geval is leidt tot verbeterde productiviteit door middel van automatisering van geboorterecht accounts en -risico's beheren door ervoor te zorgen dat de toepassing toegang wordt automatisch bijgewerkt wanneer een gebruiker rollen te wijzigen of de organisatie verlaat. De Workday gebaseerde gebruikersinrichting [implementatieplan](https://aka.ms/WorkdayDeploymentPlan) is een stapsgewijze handleiding laat zien hoe u organisaties door de aanbevolen procedures voor implementatie van Workday oplossing Active Directory-gebruikers inrichten in een proces vijf stappen.

Azure AD Premium biedt ook Microsoft Identity Manager, welke records uit andere on-premises HCM systemen importeren kunt, waaronder SAP, Oracle eBusiness en Oracle PeopleSoft.

Business-to-business samenwerking vereist steeds verleent toegang tot personen buiten uw organisatie. [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/) samenwerking kan organisaties veilig delen op hun toepassingen en services met gastgebruikers en externe partners behoud van controle over hun eigen zakelijke gegevens.

Azure AD kunt [accounts automatisch maken in AD voor gastgebruikers](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises) indien nodig, het inschakelen van zakelijke gasten voor toegang tot on-premises AD-geïntegreerde toepassingen zonder een ander wachtwoord. Organisaties kunnen instellen [multi-factor authentication (MFA)-beleid voor gastgebruiker](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)s zodat MFA controles worden uitgevoerd tijdens de application proxy-verificatie. Ook wordt elke [toegangsbeoordelingen](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews) die worden uitgevoerd op de cloud B2B gebruikers op de on-premises gebruikers toepassen. Bijvoorbeeld, als de gebruiker van de cloud via lifecycle management-beleid wordt verwijderd, wordt de on-premises-gebruiker ook verwijderd.

**Referentie management voor Active Directory-accounts** Azure AD-de selfservice voor wachtwoordherstel kan gebruikers die hun wachtwoorden gebruiken om te worden geverifieerd en hun wachtwoorden, met de gewijzigde wachtwoorden opnieuw instellen bent vergeten [geschreven naar on-premises Active Directory](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback). Het proces voor wachtwoordherstel kunt ook de on-premises Active Directory-wachtwoordbeleid gebruiken: Wanneer een gebruiker hun wachtwoord opnieuw instellen, wordt deze gecontroleerd om te controleren of deze voldoet aan het beleid van on-premises Active Directory voordat u deze doorvoert naar die map. De self-service voor wachtwoord opnieuw instellen [implementatieplan](https://aka.ms/deploymentplans/sspr) geeft een overzicht van aanbevolen procedures voor implementatie van selfservice voor wachtwoord opnieuw instellen voor gebruikers via de web- en geïntegreerde Windows-ervaringen.

![Azure AD SSPR-architectuur](media/cloud-governed-management-for-on-premises/image3.png)

Ten slotte voor organisaties die toestaan dat gebruikers hun wachtwoord moeten wijzigen in AD, AD kan worden geconfigureerd voor het gebruik van het wachtwoordbeleid met hetzelfde als de organisatie wordt gebruikt in Azure AD via de [beveiligingsfunctie van Azure AD-wachtwoord](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises), momenteel in openbare preview-versie.

Wanneer een organisatie klaar is voor een AD-geïntegreerde toepassing in de cloud verplaatsen door het verplaatsen van het besturingssysteem die als host fungeert voor de toepassing in Azure, [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) biedt compatibel is met AD domain services (zoals domeindeelname, Groepsbeleid, LDAP, en Kerberos/NTLM-verificatie). Azure AD Domain Services kan worden geïntegreerd met de organisatie bestaande Azure AD-tenant, waardoor het mogelijk dat gebruikers zich aanmelden met hun bedrijfsreferenties. Bovendien kunnen bestaande groepen en accounts voor gebruikers worden gebruikt voor het beveiligen van toegang tot resources, ervoor te zorgen dat een soepeler 'lift-and-shift' van de on-premises resources naar Azure infrastructuurservices.

![Azure AD Domain Services](media/cloud-governed-management-for-on-premises/image4.png)

## <a name="cloud-governed-management-for-on-premises-federation-based-applications"></a>Onder beheer van on-premises op basis van een federatieve toepassingen in de cloud

Voor een organisatie die al gebruikmaakt van een on-premises id-provider, kunt verplaatsen van toepassingen naar Azure AD u beter beveiligde toegang en eenvoudiger beheerdersoogpunt voor het federatiebeheer van. Azure AD kunt gedetailleerde per toepassing toegangsbeheer, met inbegrip van Azure multi-factor Authentication, met behulp van Azure AD voor voorwaardelijke toegang configureren. Azure AD biedt ondersteuning voor meer mogelijkheden, waaronder certificaten voor tokenondertekening toepassingsspecifieke en configureerbare vervaldatums. Deze mogelijkheden, hulpmiddelen en richtlijnen kunnen organisaties hun on-premises id-providers buiten gebruik stellen. Van het Microsoft IT, voor een voorbeeld is verplaatst 17,987 toepassingen van Microsoft's interne Active Directory Federation Services (AD FS) naar Azure AD.

![Azure AD-ontwikkeling](media/cloud-governed-management-for-on-premises/image5.png)

Om te beginnen met migreren federatieve toepassingen naar Azure AD als id-provider, verwijzen naar https://aka.ms/migrateapps die bevat koppelingen naar:

* Het technische document [uw toepassingen migreren naar Azure Active Directory](https://aka.ms/migrateapps/whitepaper), waarin de voordelen van de migratie worden weergegeven en wordt beschreven hoe u van plan bent voor migratie in vier fasen duidelijk beschreven: detectie, classificatie, migratie en voortdurend beheer. U wordt geholpen bij het denken over het proces en u uw project in stukken eenvoudig te gebruiken. In het hele document vindt u koppelingen naar belangrijke bronnen waarmee u weg.

* De handleiding [migreren toepassing verificatie van Active Directory Federation Services met Azure Active Directory](https://aka.ms/migrateapps/adfssolutionguide) verkent in meer detail hetzelfde vier fasen van het plannen en uitvoeren van een toepassing migration-project . In deze handleiding leert u hoe u deze fasen van toepassing op het specifieke doel van het verplaatsen van een toepassing van Active Directory Federation Services (AD FS) naar Azure AD.

* De [Active Directory Federation Services gereedheid Migratiescript](https://aka.ms/migrateapps/adfstools) op bestaande on-premises Active Directory Federation Services (AD FS)-servers om te bepalen van de gereedheid van toepassingen voor migratie naar Azure AD kan worden uitgevoerd.

## <a name="ongoing-access-management-across-cloud-and-on-premises-applications"></a>Beheer van de continue toegang voor cloud en on-premises toepassingen

Organisaties moeten een proces voor het beheren van toegang dat schaalbaar is. Gebruikers blijven verzamelen van de toegangsrechten en eindigen met meer dan wat in eerste instantie was ingericht voor deze. Enterprise-organisaties moeten bovendien efficiënt schalen om te ontwikkelen en af te dwingen toegangsbeleid en besturingselementen regelmatig.

Normaal gesproken IT delegeert toegang tot zakelijke beleidsmakers goedkeuring beslissingen nemen. Bovendien IT kan betrekking hebben op de gebruikers zelf. Gebruikers die toegang hebben tot vertrouwelijke klantgegevens in de handel brengen toepassing van een bedrijf in Europa moeten bijvoorbeeld weten beleidsregels van het bedrijf. Gastgebruikers ook mogelijk geen informatie over de vereisten voor de afhandeling van gegevens in een organisatie waaraan ze hebt uitgenodigd.

Organisaties kunnen het proces van de levenscyclus van toegang tot en met technologieën zoals automatiseren [dynamische groepen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership), verbonden met het inrichten van gebruikers naar [SaaS-toepassingen](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list), of [toepassingen geïntegreerd met behulp van het systeem voor meerdere domeinen Identity Management (SCIM](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)) standaard. Organisaties ook kunnen bepalen welke [gastgebruikers hebben toegang tot on-premises toepassingen](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises). Deze toegang krijgen tot rechten kan vervolgens worden regelmatig gecontroleerd met behulp van terugkerende [Azure AD-toegangsbeoordelingen](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview).

## <a name="future-directions"></a>De toekomst

In hybride omgevingen, de strategie van Microsoft bestaat uit het inschakelen van implementaties waarbij de **cloud is het besturingselement vlak voor de id van**, en on-premises adreslijsten en andere identiteitssystemen, zoals Active Directory en andere on-premises toepassingen, zijn het doel voor het inrichten van gebruikers die toegang heeft. Deze strategie blijven om te controleren of de rechten, identiteiten en toegang in deze toepassingen en werkbelastingen die afhankelijk van deze zijn. Met deze status end zich organisaties op station de productiviteit van eindgebruikers geheel vanuit de cloud.

![Azure AD-architectuur](media/cloud-governed-management-for-on-premises/image6.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hoe u aan de slag op deze reis, de Azure AD-implementatieplannen, dat zich bevindt in <https://aka.ms/deploymentplans> . Ze bieden end-to-end-richtlijnen over het implementeren van de mogelijkheden van Azure Active Directory (Azure AD). Elk plan wordt uitgelegd dat de waarde voor het bedrijf, planning van overwegingen, het ontwerp en operationele procedures die nodig zijn voor het implementeren van algemene Azure AD-mogelijkheden. Microsoft wordt voortdurend bijgewerkt van de implementatieplannen met aanbevolen procedures geleerd van implementaties van klanten en andere feedback wanneer we nieuwe mogelijkheden voor het beheren van de cloud met Azure AD toevoegt.
