---
title: Azure identity & access best practices voor beveiliging | Microsoft Docs
description: In dit artikel biedt een set van aanbevolen procedures voor het identiteitsbeheer van en toegang beheren met ingebouwde mogelijkheden van Azure.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2019
ms.author: barclayn
ms.openlocfilehash: 2a669f5b46db4d5de7d1d6863b94e6c117667aee
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153241"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure-identiteitsbeheer en toegangsbeheer best practices voor beveiliging
In dit artikel wordt besproken hoe een verzameling Azure-identiteitsbeheer en aanbevolen procedures voor de beveiliging van access control. Deze aanbevolen procedures zijn afgeleid van onze ervaring met [Azure AD](../active-directory/fundamentals/active-directory-whatis.md) en de ervaringen van klanten, zoals zelf.

Voor elke best practice wordt we het volgende uitgelegd:

* Wat de beste manier is
* Waarom u deze wilt inschakelen, die best practices
* Wat kan het resultaat zijn als u een failover naar de aanbevolen procedure inschakelen
* Mogelijke alternatieven voor de aanbevolen procedure
* Hoe u meer informatie kunt krijgen om in te schakelen van de aanbevolen procedure

Deze Azure identiteits- en toegangsbeveiliging aanbevolen procedures voor is gebaseerd op een consensus advies en mogelijkheden van Azure-platform en functiesets, zoals deze bestaan in de tijd in dit artikel is geschreven. Adviezen en -technologieën na verloop van tijd worden gewijzigd en regelmatig op basis van deze wijzigingen in dit artikel wordt bijgewerkt.

Azure-identiteit en besturingselement best practices voor beveiliging die in dit artikel worden besproken omvatten:

* Identiteit behandelen als de primaire beveiligingsperimeter
* Centraliseer het identiteitsbeheer
* Verbonden tenants beheren
* Eenmalige aanmelding inschakelen
* Voorwaardelijke toegang inschakelen
* Wachtwoordbeheer inschakelen
* Meervoudige verificatie voor gebruikers afdwingen
* Op rollen gebaseerd toegangsbeheer gebruiken
* Lagere blootstelling van bevoegde accounts
* Besturingselement locaties waar resources zich bevinden
* Azure AD gebruiken voor verificatie van opslag

## <a name="treat-identity-as-the-primary-security-perimeter"></a>Identiteit behandelen als de primaire beveiligingsperimeter

Veel Houd rekening met de identiteit van de primaire perimeter voor beveiliging. Dit is een overstap van de traditionele focus op de beveiliging van het netwerk. Netwerkverbindingen steeds meer poreuze, en dat defense perimeter kan niet worden zo effectief was voordat de explosie van [BYOD](https://aka.ms/byodcg) apparaten en toepassingen.

[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) de Azure-oplossing voor identiteits-en toegang. Azure AD is een multitenant, cloudgebaseerde directory en identity management-service van Microsoft. Het combineert belangrijke directoryservices, toegangsbeheer voor toepassingen en identiteitsbeveiliging in één oplossing.

De volgende secties worden aanbevolen procedures voor beveiliging identiteits- en toegangsbeheer op basis van Azure AD.

## <a name="centralize-identity-management"></a>Centraliseer het identiteitsbeheer

In een [hybride identiteit](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?) scenario wordt aangeraden dat u uw on-premises integreren en mappen in de cloud. Met de integratie kunt uw IT-team om accounts te beheren vanaf één locatie, ongeacht waar een account wordt gemaakt. Integratie van helpt ook bij uw gebruikers productiever werken door te geven van een gemeenschappelijke identiteit voor toegang tot zowel cloud en on-premises resources.

**Beste**: Tot stand brengen van één Azure AD-exemplaar. Consistentie en een enkele gezaghebbende bronnen helderheid en beveiligingsrisico's verkleinen van menselijke fouten en complexiteit van de configuratie.
**Details**: Aanwijzen van één Azure AD directory als de gezaghebbende bron voor bedrijfs- en organisatie-accounts.

**Beste**: Uw on-premises directory's integreren met Azure AD.  
**Details**: Gebruik [Azure AD Connect](../active-directory/connect/active-directory-aadconnect.md) om te synchroniseren van uw on-premises directory met uw clouddirectory.

> [!Note]
> Er zijn [factoren die invloed hebben op de prestaties van Azure AD Connect](../active-directory/hybrid/plan-connect-performance-factors.md). Zorg ervoor dat Azure AD Connect heeft onvoldoende capaciteit om u te houden attenderen systemen van belemmeren beveiliging en productiviteit. Groot of complex organisaties (organisaties meer dan 100.000 objecten inrichting) diende de [aanbevelingen](../active-directory/hybrid/whatis-hybrid-identity.md) het optimaliseren van hun Azure AD Connect-implementatie.

**Beste**: Niet synchroniseren met Azure AD-accounts die maximale bevoegdheden in uw bestaande Active Directory-exemplaar hebben.
**Details**: De standaardwaarde niet te wijzigen [Azure AD Connect-configuratie](../active-directory/hybrid/how-to-connect-sync-configure-filtering.md) die deze accounts filtert. Deze configuratie vermindert het risico van aanvallers draaien van cloud naar on-premises activa (dit kunnen een grote incident maken).

**Beste**: Wachtwoord-hashsynchronisatie inschakelen.  
**Details**: Wachtwoord-hashsynchronisatie is een functie die wordt gebruikt voor het synchroniseren van de gebruiker wachtwoord-hashes van een on-premises Active Directory-exemplaar naar een cloud-gebaseerde Azure AD-exemplaar. Deze synchronisatie helpt te beschermen tegen de referenties zijn gelekt opnieuw wordt afgespeeld vanaf vorige aanvallen.

Zelfs als u besluit het gebruik van Federatie met Active Directory Federation Services (AD FS) of andere id-providers, kunt u eventueel instellen van wachtwoord-hashsynchronisatie als een back-up in het geval uw on-premises servers mislukt of tijdelijk niet beschikbaar zijn. Deze synchronisatie kan gebruikers zich aanmeldt bij de service met behulp van het wachtwoord dat ze zich aanmelden bij hun on-premises Active Directory-exemplaar gebruiken. Daarnaast kunt u Identity Protection voor het detecteren van verdachte referenties door het vergelijken van gesynchroniseerde wachtwoord-hashes met wachtwoorden die bekend is dat inbreuk worden gepleegd, als een gebruiker heeft de hetzelfde e-mailadres en het wachtwoord gebruikt voor andere services die niet zijn verbonden met Azure AD.

Zie voor meer informatie, [implementeren u wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie](../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).

**Beste**: Voor het ontwikkelen van nieuwe toepassingen, Azure AD voor de verificatie te gebruiken.
**Details**: Gebruik de juiste mogelijkheden om verificatie te ondersteunen:

  - Azure AD voor werknemers
  - [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/) voor gastgebruikers en externe partners
  - [Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/) om te bepalen hoe klanten zich registreren, aanmelden en hun profielen beheren wanneer ze uw toepassingen

Organisaties die hun on-premises-identiteit niet met hun cloudidentiteit integreren kunnen meer overhead bij het beheren van accounts hebben. Deze overhead verhoogt de kans op fouten en schendingen van de beveiliging.

> [!Note]
> U moet kiezen welke mappen kritieke accounts bevindt zich in en of het beheerwerkstation die gebruikt wordt beheerd door de nieuwe cloudservices of bestaande verwerkt. Met behulp van bestaande management en processen het inrichten van identiteiten risico's kunt verlagen, maar ook het risico dat een aanvaller inbreuk op een on-premises-account en draaien naar de cloud kunt maken. Kunt u met een andere strategie voor verschillende rollen (bijvoorbeeld, IT-beheerders versus business unit beheerders). U hebt twee opties. Eerste optie is het maken van Azure AD-Accounts die zijn niet gesynchroniseerd met uw on-premises Active Directory-exemplaar. Voeg uw beheerwerkstation toe aan Azure AD, die u kunt beheren en hierop patches toepassen met behulp van Microsoft Intune. Tweede optie is het gebruik van bestaande beheerdersaccounts door deze te synchroniseren met uw on-premises Active Directory-exemplaar. Gebruik bestaande werkstations in uw Active Directory-domein voor het beheer en beveiliging.

## <a name="manage-connected-tenants"></a>Verbonden tenants beheren
Uw organisatie beveiliging moet zichtbaarheid op risico's te beoordelen en om te bepalen of het beleid van uw organisatie, en eventuele regelgeving wordt nageleefd. U moet ervoor zorgen dat uw organisatie beveiliging inzicht in alle abonnementen die zijn verbonden met de productie-omgeving en het netwerk heeft (via [Azure ExpressRoute](../expressroute/expressroute-introduction.md) of [site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)). Een [globale beheerder/bedrijfsbeheerder](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) in Azure AD kunt verhogen toegang krijgen tot de [Administrator voor gebruikerstoegang](../role-based-access-control/built-in-roles.md#user-access-administrator) rol en alle abonnementen en beheerde groepen die zijn verbonden met uw omgeving zien.

Zie [toegangsrechten voor het beheren van alle Azure-abonnementen en beheergroepen](../role-based-access-control/elevate-access-global-admin.md) om ervoor te zorgen dat u en uw beveiligingsgroep alle abonnementen of beheergroepen kunnen bekijken die is verbonden met uw omgeving. Nadat u de risico's hebt geëvalueerd, moet u deze toegang met verhoogde bevoegdheid verwijderen.

## <a name="enable-single-sign-on"></a>Eenmalige aanmelding inschakelen

In een wereld mobiel-mobiliteit en de cloud die u wilt inschakelen eenmalige aanmelding (SSO) aan apparaten, apps en services vanaf elke locatie, zodat uw gebruikers productief waar en wanneer zijn kunnen. Wanneer u meerdere id-oplossingen voor het beheren van hebt, wordt dit een probleem met beheerdersrechten niet alleen voor IT, maar ook voor gebruikers die hebben tot meerdere wachtwoorden onthouden.

Met behulp van dezelfde identiteitsoplossing voor voor al uw apps en resources, kunt u eenmalige aanmelding kunt bereiken. En uw gebruikers kunnen dezelfde set referenties gebruiken om te melden en toegang tot de bronnen die ze nodig hebben, ongeacht of de resources zich op locatie bevinden of in de cloud.

**Beste**: SSO inschakelen.  
**Details**: Azure AD [uitbreiding van on-premises Active Directory](../active-directory/connect/active-directory-aadconnect.md) naar de cloud. Gebruikers kunnen hun primaire werk of school-account gebruiken voor hun domein apparaten, resources van het bedrijf en alle van de web- en SaaS-toepassingen die ze nodig hebben om hun werk te doen. Gebruikers hoeven te onthouden meerdere sets met gebruikersnamen en wachtwoorden, en de toegang tot hun toepassingen kan automatisch worden ingericht (of de inrichting ongedaan gemaakt) op basis van hun groepslidmaatschappen van de organisatie en hun status als een werknemer. En u kunt deze toegang beheren voor galerie-apps of voor uw eigen on-premises apps die u hebt ontwikkeld en gepubliceerd via de [Azure AD-toepassingsproxy](../active-directory/active-directory-application-proxy-get-started.md).

Eenmalige aanmelding gebruiken waarmee gebruikers toegang krijgen tot hun [SaaS-toepassingen](../active-directory/active-directory-appssoaccess-whatis.md) op basis van hun account voor werk of school in Azure AD. Dit is van toepassing niet alleen voor Microsoft SaaS-apps, maar ook met andere apps, zoals [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) en [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). U kunt uw toepassing configureren voor gebruik van Azure AD als een [SAML gebaseerde identiteit](../active-directory/fundamentals-identity.md) provider. Als een besturingselement voor beveiliging, Azure AD niet uitgeven van een token waarmee gebruikers zich aanmelden bij de toepassing, tenzij ze de toegang via Azure AD hebt gekregen. U kunt toegang verlenen rechtstreeks of via een groep dat gebruikers lid zijn van zijn.

Organisaties die geen een gemeenschappelijke identiteit maakt voor het maken van eenmalige aanmelding voor hun gebruikers en toepassingen worden meer blootgesteld aan scenario's waarin gebruikers meerdere wachtwoorden hebben. Deze scenario's vergroot de kans dat gebruikers wachtwoorden opnieuw gebruiken of met behulp van zwakke wachtwoorden.

## <a name="turn-on-conditional-access"></a>Voorwaardelijke toegang inschakelen

Gebruikers kunnen toegang krijgen tot bronnen van uw organisatie met behulp van verschillende apparaten en apps vanaf elke locatie. Als IT-beheerder, die u wilt ervoor zorgen dat deze apparaten aan uw normen voor beveiliging en naleving. Alleen richten op wie toegang heeft tot een resource is niet voldoende meer.

Voor beveiliging en productiviteit in balans brengen, moet u echter nadenken over hoe u een resource wordt geopend voordat u een beslissing over toegangsbeheer kunt maken. Met voorwaardelijke toegang van Azure AD kunt u deze vereiste aanpakken. Met voorwaardelijke toegang, kunt u automatisch toegang tot het toegangsbeheer op basis van voorwaarden voor toegang tot uw cloud-apps.

**Beste**: Beheren en toegang tot bedrijfsresources beheren.  
**Details**: Azure AD configureren [voorwaardelijke toegang](../active-directory/active-directory-conditional-access-azure-portal.md) op basis van een groep, de locatie en de gevoeligheid van een toepassing voor SaaS-apps en Azure AD verbonden apps.

**Beste**: Verouderde verificatieprotocollen blokkeren.
**Details**: Aanvallers zwakke punten in oudere protocollen elke dag, met name voor wachtwoord spray aanvallen. Configureer voorwaardelijke toegang voor het blokkeren van verouderde protocollen. Zie de video [Azure AD: Taken en niet moet doen](https://www.youtube.com/watch?v=wGk0J4z90GI) voor meer informatie.

## <a name="enable-password-management"></a>Wachtwoordbeheer inschakelen

Als u meerdere tenants hebt of als u wilt dat gebruikers kunnen [hun eigen wachtwoorden opnieuw instellen](../active-directory/active-directory-passwords-update-your-own-password.md), is het belangrijk dat u passende beveiligingsbeleidsregels gebruiken om misbruik te voorkomen.

**Beste**: Selfservice voor wachtwoordherstel (SSPR) voor uw gebruikers instellen.  
**Details**: Gebruik de Azure AD [Self-service voor wachtwoord opnieuw instellen](../active-directory-b2c/active-directory-b2c-reference-sspr.md) functie.

**Beste**: Monitor hoe of als SSPR daadwerkelijk wordt gebruikt.  
**Details**: Bewaak de gebruikers die zijn geregistreerd met behulp van de Azure AD [activiteitenrapport voor wachtwoord opnieuw instellen van inschrijving](../active-directory/active-directory-passwords-get-insights.md). De rapportagefunctie die Azure AD biedt vindt u antwoorden op vragen met behulp van vooraf gedefinieerde rapporten. Als u op de juiste wijze in licentie gegeven bent, kunt u ook aangepaste query's maken.

**Beste**: Cloud-gebaseerde wachtwoord-beleid op uw on-premises infrastructuur uitbreiden.
**Details**: Wachtwoordbeleid in uw organisatie verbeteren door het uitvoeren van controles voor on-premises wachtwoord te wijzigen, zoals u dat wel voor cloud-gebaseerde wachtwoord te wijzigen doet. Installeer [Azure AD-wachtwoordbeveiliging](../active-directory/authentication/concept-password-ban-bad.md) voor Windows Server Active Directory-agents on-premises lijsten met uitgesloten wachtwoorden met uw bestaande infrastructuur uitbreiden. Gebruikers en beheerders die wijzigt, instellen of opnieuw instellen van wachtwoorden on-premises vereist zijn om te voldoen aan het beleid voor dezelfde als gebruikers alleen in de cloud.

## <a name="enforce-multi-factor-verification-for-users"></a>Meervoudige verificatie voor gebruikers afdwingen

Het is raadzaam dat u verificatie in twee stappen voor alle gebruikers vereisen. Dit omvat beheerders en anderen in uw organisatie die een aanzienlijke invloed hebben kan als hun account is aangetast (bijvoorbeeld financiële officers).

Er zijn meerdere opties voor het vereisen van verificatie in twee stappen. De beste optie voor u, is afhankelijk van uw doelstellingen, de Azure AD-editie die u uitvoert en uw licentieprogramma. Zie [hoe u verificatie in twee stappen vereist voor een gebruiker](../active-directory/authentication/howto-mfa-userstates.md) om te bepalen welke optie het beste voor u. Zie de [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) en [Azure multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) prijspagina's voor meer informatie over licenties en prijzen.

Hieronder vindt u opties en prestaties voor het inschakelen van verificatie in twee stappen:

**Optie 1**: [Multi-factor Authentication inschakelen door het veranderen van gebruikersstatus](../active-directory/authentication/howto-mfa-userstates.md).   
**Voordeel**: Dit is de traditionele methode voor het vereisen van verificatie in twee stappen. Het werkt met beide [Azure multi-factor Authentication in de cloud en Azure multi-factor Authentication-Server](../active-directory/authentication/concept-mfa-whichversion.md). Met deze methode vereist dat gebruikers verificatie uitvoeren telkens wanneer ze zich aanmelden en vervangt beleid voor voorwaardelijke toegang.

Om te bepalen waar multi-factor Authentication moet worden ingeschakeld, Zie [welke versie van Azure MFA geschikt is voor mijn organisatie?](../active-directory/authentication/concept-mfa-whichversion.md).

**Optie 2**: [Multi-factor Authentication met beleid voor voorwaardelijke toegang inschakelen](../active-directory/authentication/howto-mfa-getstarted.md).
**Voordeel**: Deze optie kunt u te vragen om verificatie in twee stappen onder bepaalde omstandigheden, met behulp van [voorwaardelijke toegang](../active-directory/active-directory-conditional-access-azure-portal.md). Specifieke voorwaarden kunnen aanmelden van gebruikers vanuit verschillende locaties, niet-vertrouwde apparaten of toepassingen die u beschouwt als riskant zijn. Voor het definiëren van specifieke voorwaarden waar u verificatie in twee stappen vereist, kunt u voorkomen constante vragen om uw gebruikers, dit kunnen een onaangename gebruikerservaring.

Dit is de meest flexibele manier om in te schakelen van verificatie in twee stappen voor uw gebruikers. Als u een beleid voor voorwaardelijke toegang werkt alleen voor Azure multi-factor Authentication in de cloud en is een premium-functie van Azure AD. U vindt meer informatie over deze methode in [implementeren van cloud-gebaseerde Azure multi-factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md).

**Optie 3**: Multi-factor Authentication met beleid voor voorwaardelijke toegang inschakelen door het evalueren van de gebruiker en meld u risico's van [Azure AD Identity Protection](../active-directory/authentication/tutorial-risk-based-sspr-mfa.md).   
**Voordeel**: Deze optie kunt u naar:

- Detecteren van mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie.
- Automatische antwoorden om gedetecteerde verdachte activiteit die betrekking op de identiteiten van uw organisatie hebben te configureren.
- Verdachte incidenten onderzoeken en onderneem gepaste actie op te lossen.

Deze methode maakt gebruik van de risico-evaluatie van de Azure AD Identity Protection om te bepalen of verificatie in twee stappen vereist is op basis van gebruiker en aanmeldingsrisico voor alle cloudtoepassingen. Deze methode is een Azure Active Directory P2-licentie vereist. U vindt meer informatie over deze methode in [Azure Active Directory Identity Protection](../active-directory/identity-protection/overview.md).

> [!Note]
> Optie 1, meervoudige verificatie inschakelen door het veranderen van de status van de gebruiker, overschrijft beleid voor voorwaardelijke toegang. Aangezien de opties 2 en 3 gebruikt beleidsregels voor voorwaardelijke toegang, kunt u de optie 1 met hen niet gebruiken.

Organisaties die geen extra lagen van identiteitsbeveiliging, zoals verificatie, toevoegen zijn gevoeliger voor referentie diefstal aanval. Een referentie diefstal aanval kan leiden tot inbreuk op gegevens.

## <a name="use-role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer gebruiken
Toegangsbeheer voor cloudresources is essentieel voor een organisatie die gebruikmaakt van de cloud. [Op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md) kunt u beheren wie toegang heeft tot de Azure-resources, wat ze kunnen doen met deze resources en welke gebieden ze toegang hebben.

Het toewijzen van groepen of afzonderlijke functies die verantwoordelijk zijn voor specifieke functies in Azure, kunt verwarring die tot menselijke leiden kan en automatiseringsfouten die beveiligingsrisico's voorkomen. Toegang beperken op basis van de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes is van cruciaal belang voor organisaties die willen beveiligingsbeleid voor toegang tot gegevens afdwingen.

Uw beveiligingsteam moet inzicht in uw Azure-resources om te beoordelen en risico's te herstellen. Als het beveiligingsteam operationele verantwoordelijkheden heeft, moeten aanvullende machtigingen om hun werk te doen.

U kunt [RBAC](../role-based-access-control/overview.md) machtigingen toewijzen aan gebruikers, groepen en toepassingen met een bepaald bereik. Het bereik van een roltoewijzing kan een abonnement, een resourcegroep of één resource zijn.

**Beste**: Scheiden van taken in uw team en alleen de mate van toegang verlenen aan gebruikers die ze nodig hebben om hun werk te kunnen. Onbeperkte machtigingen in uw Azure-abonnement of resources, toestaan in plaats van iedereen zodat alleen bepaalde acties op een bepaald bereik.
**Details**: Gebruik [ingebouwde RBAC-rollen](../role-based-access-control/built-in-roles.md) in Azure machtigingen toewijzen aan gebruikers.

> [!Note]
> Specifieke machtigingen maken onnodig complex en verwarring, samenvoegen in een 'legacy'-configuratie die moeilijk zijn om op te lossen zonder angst van iets te verbreken. Resource-specifieke machtigingen voorkomen. Gebruik in plaats daarvan beheergroepen voor bedrijfsbrede machtigingen en -resourcegroepen voor machtigingen in abonnementen. Voorkomen dat machtigingen voor specifieke gebruikers. In plaats daarvan toegang toewijzen aan groepen in Azure AD.

**Beste**: Verleen beveiligingsteams met de verantwoordelijkheden van Azure toegang tot Azure-resources bekijken, zodat ze kunnen beoordelen en risico's verhelpen.
**Details**: Het RBAC voor security teams verlenen [Beveiligingslezer](../role-based-access-control/built-in-roles.md#security-reader) rol. U kunt de root management-groep of de segment-beheergroep, afhankelijk van het bereik van taken:

- **Hoofdmap beheergroep** voor teams die verantwoordelijk is voor alle enterprise-resources
- **Beheergroep segment** voor teams met een beperkt bereik (meestal vanwege regelgeving of andere Bedrijfsgrenzen)

**Beste**: De juiste machtigingen aan beveiligingsteams die direct operationele verantwoordelijkheden.
**Details**: Bekijk de ingebouwde RBAC-rollen voor de toewijzing van juiste rol. Als de ingebouwde rollen niet voldoen aan de specifieke behoeften van uw organisatie, kunt u [aangepaste rollen voor Azure-resources](../role-based-access-control/custom-roles.md). Als kunt ingebouwde rollen, u aangepaste rollen toewijzen aan gebruikers, groepen en service-principals op abonnement, resourcegroep en resource-bereiken.

**Aanbevolen procedures**: Toegang verlenen Azure Security Center voor beveiligingsrollen die ze nodig hebt. Security Center kunt beveiligingsteams om snel te identificeren en risico's verhelpen.
**Details**: Beveiligingsteams aan deze behoeften voor toevoegen aan de RBAC [beveiligingsbeheerder](../role-based-access-control/built-in-roles.md#security-admin) rol zodat ze kunnen weergeven van beleidsregels voor veiligheid, security-status weergeven, beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen bewerken en negeren van waarschuwingen en aanbevelingen. U kunt dit doen met behulp van de root management-groep of de segment-beheergroep, afhankelijk van het bereik van taken.

Organisaties die geen gegevenstoegangsbeheer afdwingen met behulp van mogelijkheden zoals RBAC mogelijk meer bevoegdheden dan nodig is om hun gebruikers geven. Dit kan leiden tot inbreuk op gegevens door gebruikers in staat toegang tot verschillende soorten gegevens (bijvoorbeeld hoge impact op bedrijf) die ze al dan niet mogen hebben.

## <a name="lower-exposure-of-privileged-accounts"></a>Lagere blootstelling van bevoegde accounts

Het beveiligen van bevoegde is toegang een kritieke eerste stap in de bescherming van uw bedrijfsactiva. Door het aantal gebruikers die toegang hebben tot het beveiligen van gegevens of resources vermindert de kans dat een kwaadwillende gebruiker de toegang of een geautoriseerde gebruiker per ongeluk die betrekking hebben op een gevoelige resource.

Bevoegde accounts zijn accounts waarmee IT-systemen worden beheerd. Cyber aanvallers richt u op deze accounts om toegang tot de gegevens en systemen van een organisatie te krijgen. Als u wilt beveiligen van bevoegde toegang, moet u de accounts en systemen van het risico wordt blootgesteld aan een kwaadwillende gebruiker isoleren.

U wordt aangeraden dat u ontwikkelt en gaat u als een schema volgt voor het beveiligen van bevoegde toegang tegen cyberaanvallen aanvallers. Voor informatie over het maken van een gedetailleerde roadmap voor het beveiligen van identiteiten en toegang die worden beheerd of gemeld in Azure AD, Microsoft Azure, Office 365 en andere cloudservices, Bekijk [beveiligen van bevoegde toegang voor hybride en cloud-implementaties in Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

Het volgende bevat een overzicht van de aanbevolen procedures die zijn gevonden in [beveiligen van bevoegde toegang voor hybride en cloud-implementaties in Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md):

**Beste**: Beheren, controleren en bewaken van toegang tot beschermde accounts.   
**Details**: Schakel [Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/active-directory-securing-privileged-access.md). Nadat u Privileged Identity Management inschakelen, ontvangt u de melding e-mailberichten voor bevoegde toegang rol wijzigingen. Deze meldingen bieden vroegtijdig waarschuwen wanneer andere gebruikers worden toegevoegd aan maximaal bevoorrechte rollen in uw directory.

**Beste**: Zorg ervoor dat alle essentiële beheerdersaccounts worden beheerd Azure AD-accounts.
**Details**: Elke consument-accounts verwijderen van kritieke-beheerdersrollen (bijvoorbeeld Microsoft-accounts, zoals hotmail.com, live.com en outlook.com).

**Beste**: Zorg ervoor dat alle essentiële-beheerdersrollen hebben voor een afzonderlijk account voor beheertaken phishing en andere aanvallen om inbreuk van Administrator-bevoegdheden te voorkomen.
**Details**: Maak een afzonderlijk beheerdersaccount dat toegewezen de bevoegdheden die nodig zijn om uit te voeren van de administratieve taken. Blokkeer het gebruik van deze beheerdersaccounts voor dagelijkse productiviteitsprogramma's, zoals Microsoft Office 365-e-mailadres of willekeurige het web te surfen.

**Beste**: Identificeren en categoriseren accounts in maximaal bevoorrechte rollen.   
**Details**: Na het inschakelen van Azure AD Privileged Identity Management, bekijk de gebruikers die zich in de globale beheerder, beheerder met bevoorrechte rol en andere maximaal bevoorrechte rollen. Verwijderen van accounts die niet langer nodig zijn in deze rollen en de resterende accounts die zijn toegewezen aan beheerdersrollen categoriseren:

- Afzonderlijk toegewezen aan gebruikers met beheerdersrechten en kan worden gebruikt voor niet-administratieve doeleinden (bijvoorbeeld persoonlijke e-mailadres)
- Afzonderlijk toegewezen aan gebruikers met beheerdersrechten en aangewezen voor administratieve doeleinden alleen
- Gedeeld door meerdere gebruikers
- Voor scenario's voor toegang in noodgevallen
- Voor geautomatiseerde scripts
- Voor externe gebruikers

**Beste**: Implementeren 'just-in-time' toegang verder verlagen de blootstellingstijd van bevoegdheden en vergroot uw inzicht in het gebruik van accounts met bevoegdheden (JIT).   
**Details**: Azure AD Privileged Identity Management kunt u:

- Gebruikers beperken tot alleen nemen over hun JIT-bevoegdheden.
- Rollen toewijzen voor een kortere duur met vertrouwen dat de bevoegdheden automatisch worden ingetrokken.

**Beste**: Ten minste twee accounts voor toegang in noodgevallen definiëren.   
**Details**: Accounts voor toegang in noodgevallen kunnen organisaties bevoorrechte toegang in een bestaand Azure Active Directory-omgeving beperken. Deze accounts over uitgebreide beheerdersmogelijkheden en niet zijn toegewezen aan specifieke personen. Accounts voor toegang in noodgevallen zijn beperkt tot scenario's waarin de normale beheerdersaccounts kunnen niet worden gebruikt. Organisaties moeten van het noodgeval account gebruik om alleen de benodigde hoeveelheid tijd te beperken.

Evalueer de accounts die toegewezen of die in aanmerking komen voor de rol globale beheerder zijn. Als er geen accounts alleen in de cloud met behulp van de `*.onmicrosoft.com` domein (die bestemd zijn voor toegang in noodgevallen), maken ze. Zie voor meer informatie, [beheerdersaccounts voor Noodtoegang beheren in Azure AD](../active-directory/users-groups-roles/directory-emergency-access.md).

**Beste**: Zorg ervoor dat een proces "verbreken om" in het geval van nood.
**Details**: Volg de stappen in [beveiligen van bevoegde toegang voor hybride en cloud-implementaties in Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

**Beste**: Alle kritieke beheerdersaccounts worden zonder wachtwoord nodig (bij voorkeur) of meervoudige verificatie vereisen.
**Details**: Gebruik de [Microsoft Authenticator-app](../active-directory/authentication/howto-authentication-phone-sign-in.md) zich aanmeldt bij een Azure AD-account zonder een wachtwoord te gebruiken. Zoals [Windows Hello voor bedrijven](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification), de Microsoft Authenticator gebruikmaakt van verificatie op basis van een sleutel waarmee de referenties van een gebruiker die gekoppeld aan een apparaat en maakt gebruik van biometrische verificatie of een PINCODE.

Azure multi-factor Authentication vereisen bij aanmelding voor alle afzonderlijke gebruikers die permanent zijn toegewezen aan een of meer van de Azure AD-beheerdersrollen: Globale beheerder, beheerder met bevoorrechte rol, beheerder van Exchange Online en SharePoint Online-beheerder. Schakel [multi-factor Authentication voor uw beheerdersaccounts](../active-directory/authentication/howto-mfa-userstates.md) en zorg ervoor dat gebruikers van admin-account hebt geregistreerd.

**Beste**: Voor kritieke beheerdersaccounts, hebt u een beheerwerkstation waar de taken zijn niet toegestaan (voor bijvoorbeeld surfen en de e-mailadres). Dit wordt uw beheerdersaccounts beveiligen tegen aanvalsvectoren die gebruikmaken van bladeren en e-mailbericht en aanzienlijk beperkt u het risico van een grote incident.
**Details**: Gebruik een beheerwerkstation. Kies een beveiligingsniveau of werkstation:

- Zeer veilige productiviteit apparaten bieden geavanceerde beveiliging voor het bladeren door en andere productiviteitstaken.
- [Privileged Access Workstations (paw's)](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations) bieden een speciaal besturingssysteem die beschermd tegen aanvallen via internet en dreigingsvectoren voor gevoelige taken.

**Beste**: De inrichting beheerdersaccounts wanneer werknemers uw organisatie verlaten.
**Details**: Beschikken over een proces dat wordt uitgeschakeld of verwijderd van beheerdersaccounts wanneer werknemers uw organisatie verlaten.

**Beste**: Regelmatig beheerdersaccounts testen met behulp van de huidige aanvalstechnieken.
**Details**: Office 365-aanval Simulator of een derde partij, bieden voor het uitvoeren van realistische aanvalsscenario's in uw organisatie gebruiken. Dit kan helpen u kwetsbaar gebruikers voordat een echte aanval plaatsvindt.

**Beste**: Stappen om de meest gebruikte aangevallen technieken.  
**Details**: [Microsoft-accounts in een beheerderrol die moet worden overgeschakeld naar de werk- of schoolaccount identificeren](../active-directory/users-groups-roles/directory-admin-roles-secure.md#identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts)  

[Zorg ervoor dat afzonderlijke gebruikersaccounts en doorsturen voor accounts van globale beheerders van e-mail](../active-directory/users-groups-roles/directory-admin-roles-secure.md)  

[Zorg ervoor dat de wachtwoorden van beheerdersaccounts onlangs hebt gewijzigd](../active-directory/users-groups-roles/directory-admin-roles-secure.md#ensure-the-passwords-of-administrative-accounts-have-recently-changed)  

[Wachtwoord-hashsynchronisatie inschakelen](../active-directory/users-groups-roles/directory-admin-roles-secure.md#turn-on-password-hash-synchronization)  

[Meervoudige verificatie vereisen voor gebruikers in alle bevoorrechte rollen, evenals de beschikbaar gemaakte gebruikers](../active-directory/users-groups-roles/directory-admin-roles-secure.md#require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users)  

[Uw Office 365 beveiligen Score ophalen (als u Office 365)](../active-directory/users-groups-roles/directory-admin-roles-secure.md#obtain-your-office-365-secure-score-if-using-office-365)  

[Bekijk de Office 365-beveiligings- en -richtlijnen (als u Office 365)](../active-directory/users-groups-roles/directory-admin-roles-secure.md#review-the-office-365-security-and-compliance-guidance-if-using-office-365)  

[Office 365 Activiteitenbewaking te configureren (als u Office 365)](../active-directory/users-groups-roles/directory-admin-roles-secure.md#configure-office-365-activity-monitoring-if-using-office-365)  

[Eigenaren van incident/emergency response-plan maken](../active-directory/users-groups-roles/directory-admin-roles-secure.md#establish-incidentemergency-response-plan-owners)  

[Veilige on-premises bevoegde beheerdersaccounts](../active-directory/users-groups-roles/directory-admin-roles-secure.md#turn-on-password-hash-synchronization)

Als u geen uitgebreide toegang beveiligen, vindt u mogelijk dat u te veel gebruikers in maximaal bevoorrechte rollen hebben en kwetsbaar voor aanvallen zijn. Kwaadwillende actoren, met inbegrip van cyber aanvallers, vaak target-beheerdersaccounts en andere elementen van bevoegde toegang om toegang te krijgen tot gevoelige gegevens en systemen met behulp van diefstal van referenties.

## <a name="control-locations-where-resources-are-created"></a>Besturingselement locaties waar resources worden gemaakt

Het inschakelen van cloudoperators voor het uitvoeren van taken bij het voorkomen dat belangrijke conventies die nodig zijn om de resources van uw organisatie te beheren is erg belangrijk. Organisaties die willen bepalen de locaties waar resources zijn gemaakt moeten harde code voor deze locaties.

U kunt [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) te maken van beveiligingsbeleid waarvan definities beschrijven de acties of resources die specifiek worden geweigerd. U toewijzen die beleidsdefinities op het gewenste bereik, zoals het abonnement, de resourcegroep of een afzonderlijke resource.

> [!NOTE]
> Beleidsregels voor veiligheid zijn niet gelijk zijn aan RBAC. Ze daadwerkelijk RBAC gebruiken om te autoriseren van gebruikers om deze resources te maken.
>
>

Organisaties die niet zijn bepalen hoe resources worden gemaakt, zijn vatbaar voor gebruikers die misbruik van de service maken kunnen door het maken van meer resources dan ze nodig hebben. Beperking van het proces voor het maken van resources is een belangrijke stap voor het beveiligen van een scenario voor meerdere tenants.

## <a name="actively-monitor-for-suspicious-activities"></a>Controleren voor verdachte activiteiten

Een actieve identiteit bewakingssysteem kan snel verdacht gedrag detecteren en activeren van een waarschuwing voor verder onderzoek. De volgende tabel bevat twee Azure AD-mogelijkheden waarmee organisaties hun identiteit te controleren:

**Beste**: Beschikken over een methode om te identificeren:

- Probeert aan te melden bij [zonder worden getraceerd](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md).
- [Brute force](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) aanvallen tegen een bepaald account.
- Probeert aan te melden bij vanaf meerdere locaties.
- Aanmeldingen vanaf [geïnfecteerde apparaten](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md).
- Verdachte IP-adressen.

**Details**: Gebruik Azure AD Premium [anomaliedetectie rapporten](../active-directory/active-directory-view-access-usage-reports.md). Zorg ervoor dat processen en procedures voor IT-beheerders om uit te voeren van deze rapporten dagelijks of op aanvraag (meestal in een reactie op incidenten scenario).

**Beste**: Beschikken over een actieve bewaking systeem dat op de hoogte gebracht van de risico's en risiconiveau (hoog, Gemiddeld of laag) voor uw zakelijke vereisten kunt aanpassen.   
**Details**: Gebruik [Azure AD Identity Protection](../active-directory/active-directory-identityprotection.md), die de huidige vlaggen risico's op een eigen dashboard en dagelijkse samenvatting meldingen via e-mail worden verzonden. Ter bescherming van identiteiten in uw organisatie, kunt u risico's gebaseerde beleidsregels die automatisch op gedetecteerde problemen reageren na het verstrijken van een opgegeven risiconiveau configureren.

Organisaties die hun identiteitssystemen niet controleren zijn op gebruiker referenties zijn aangetast. Medeweten verdachte activiteiten zijn plaatsvinden via deze referenties kunnen niet organisaties dit type bedreiging beperken.

## <a name="use-azure-ad-for-storage-authentication"></a>Azure AD gebruiken voor verificatie van opslag
[Azure Storage](../storage/common/storage-auth-aad.md) biedt ondersteuning voor verificatie en autorisatie met Azure AD voor Blob storage en Queue storage. Met Azure AD-verificatie, kunt u de op rollen gebaseerd toegangsbeheer in Azure voor het specifieke machtigingen verlenen aan gebruikers, groepen en toepassingen naar het bereik van een afzonderlijke blob-container of de wachtrij.

Het is raadzaam dat u [Azure AD voor het verifiëren van toegang tot opslag](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/).

## <a name="next-step"></a>Volgende stap

Zie [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md) voor meer best practices voor beveiliging moeten worden gebruikt wanneer bent u het ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.
