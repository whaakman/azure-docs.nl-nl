---
title: Azure identity & access best practices voor beveiliging | Microsoft Docs
description: In dit artikel biedt een set van aanbevolen procedures voor het identiteitsbeheer van en toegang beheren met ingebouwde mogelijkheden van Azure.
services: security
documentationcenter: na
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/17/2018
ms.author: barclayn
ms.openlocfilehash: 77bd95f036aec0cdaa351c44c0f1eafe9fc702d9
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294353"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure-identiteitsbeheer en toegangsbeheer best practices voor beveiliging

Veel Houd rekening met de identiteit van de nieuwe grens laag voor de beveiliging over die functie van het traditionele netwerk gerichte perspectief te nemen. Deze evolutie van het primaire pivot voor aandacht voor beveiliging en investeringen afkomstig zijn uit het feit dat netwerkverbindingen steeds poreuze zijn geworden en die defense perimeter mag niet zo effectief als ze eenmaal zijn voordat de explosie van [BYOD ](http://aka.ms/byodcg) apparaten en toepassingen.

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
* Eenmalige aanmelding inschakelen
* Voorwaardelijke toegang inschakelen
* Wachtwoordbeheer inschakelen
* Meervoudige verificatie voor gebruikers afdwingen
* Op rollen gebaseerd toegangsbeheer gebruiken
* Lagere blootstelling van bevoegde accounts
* Besturingselement locaties waar resources zich bevinden

## <a name="treat-identity-as-the-primary-security-perimeter"></a>Identiteit behandelen als de primaire beveiligingsperimeter
Veel Houd rekening met de identiteit van de primaire perimeter voor beveiliging. Dit is een overstap van de traditionele focus op de beveiliging van het netwerk. Netwerkverbindingen steeds meer poreuze, en dat defense perimeter kan niet worden zo effectief was voordat de explosie van [BYOD](http://aka.ms/byodcg) apparaten en toepassingen.
[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) de Azure-oplossing voor identiteits-en toegang. Azure AD is een multitenant, cloudgebaseerde directory en identity management-service van Microsoft. Het combineert belangrijke directoryservices, toegangsbeheer voor toepassingen en identiteitsbeveiliging in één oplossing.

De volgende secties worden aanbevolen procedures voor beveiliging identiteits- en toegangsbeheer op basis van Azure AD.

<<<<<<< Hiervoor HEAD naar [hybride identiteit](../active-directory/hybrid/plan-hybrid-identity-design-considerations-overview.md) scenario wordt aangeraden twee opties: ===
## <a name="centralize-identity-management"></a>Centraliseer het identiteitsbeheer
In een [hybride identiteit](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?) scenario wordt aangeraden dat u uw on-premises integreren en mappen in de cloud. Met de integratie kunt uw IT-team om accounts te beheren vanaf één locatie, ongeacht waar een account wordt gemaakt. Integratie van helpt ook bij uw gebruikers productiever werken door te geven van een gemeenschappelijke identiteit voor toegang tot zowel cloud en on-premises resources.
>>>>>>> 72fef8deb3b74739d94337401a4a4a0921e88f58


**Beste**: uw on-premises directory's integreren met Azure AD.  
**Details**: Gebruik [Azure AD Connect](../active-directory/connect/active-directory-aadconnect.md) om te synchroniseren van uw on-premises directory met uw clouddirectory.

<<<<<<< HEAD voor meer informatie over Azure AD-synchronisatie, Zie het artikel [uw on-premises identiteiten integreren met Azure Active Directory](../active-directory/hybrid/whatis-hybrid-identity.md).
=======
**Beste**: wachtwoord-hashsynchronisatie inschakelen.  
**Details**: wachtwoord-hashsynchronisatie is een functie die wordt gebruikt om te synchroniseren-hashes van gebruiker wachtwoord-hashes van een on-premises Active Directory-exemplaar naar een cloud-gebaseerde Azure AD-exemplaar.
>>>>>>> 72fef8deb3b74739d94337401a4a4a0921e88f58

Zelfs als u besluit het gebruik van Federatie met Active Directory Federation Services (AD FS) of andere id-providers, kunt u eventueel instellen van wachtwoord-hashsynchronisatie als een back-up in het geval uw on-premises servers mislukt of tijdelijk niet beschikbaar zijn. Hierdoor kunnen gebruikers zich aanmeldt bij de service met behulp van het wachtwoord dat ze zich aanmelden bij hun on-premises Active Directory-exemplaar gebruiken. Daarnaast kunt u Identity Protection voor het detecteren van verdachte referenties door het vergelijken van de wachtwoord-hashes met wachtwoorden die bekend is dat inbreuk worden gepleegd, als een gebruiker op andere services die niet is verbonden met Azure AD hun e-mailadres en wachtwoord heeft gebruikt.

Zie voor meer informatie, [implementeren u wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie](../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).

Organisaties die hun on-premises-identiteit niet met hun cloudidentiteit integreren kunnen meer overhead bij het beheren van accounts hebben. Deze overhead verhoogt de kans op fouten en schendingen van de beveiliging.

<<<<<<< HEAD
> [!NOTE]
> de beslissing om eenmalige aanmelding gebruiken is van invloed op hoe u uw on-premises directory's integreren met uw clouddirectory. Als u wilt dat eenmalige aanmelding, moet u Federatie, gebruiken omdat adreslijstsynchronisatie alleen leveren [dezelfde aanmelding](../active-directory/hybrid/whatis-hybrid-identity.md).
>
>
=======
## <a name="enable-single-sign-on"></a>Eenmalige aanmelding inschakelen

In een wereld mobiel-mobiliteit en de cloud die u wilt inschakelen eenmalige aanmelding (SSO) aan apparaten, apps en services vanaf elke locatie, zodat uw gebruikers productief waar en wanneer zijn kunnen. Wanneer u meerdere id-oplossingen voor het beheren van hebt, wordt dit een probleem met beheerdersrechten niet alleen voor IT, maar ook voor gebruikers die hebben tot meerdere wachtwoorden onthouden.

Met behulp van dezelfde identiteitsoplossing voor voor al uw apps en resources, kunt u eenmalige aanmelding kunt bereiken. En uw gebruikers kunnen dezelfde set referenties gebruiken om te melden en toegang tot de bronnen die ze nodig hebben, ongeacht of de resources zich op locatie bevinden of in de cloud.

**Beste**: eenmalige aanmelding inschakelen.  
**Details**: Azure AD [uitbreiding van on-premises Active Directory](../active-directory/connect/active-directory-aadconnect.md) naar de cloud. Gebruikers kunnen hun primaire werk of school-account gebruiken voor hun domein apparaten, resources van het bedrijf en alle van de web- en SaaS-toepassingen die ze nodig hebben om hun werk te doen. Gebruikers hoeven te onthouden meerdere sets met gebruikersnamen en wachtwoorden, en de toegang tot hun toepassingen kan automatisch worden ingericht (of de inrichting ongedaan gemaakt) op basis van hun groepslidmaatschappen van de organisatie en hun status als een werknemer. En u kunt deze toegang beheren voor galerie-apps of voor uw eigen on-premises apps die u hebt ontwikkeld en gepubliceerd via de [Azure AD-toepassingsproxy](../active-directory/active-directory-application-proxy-get-started.md).

Eenmalige aanmelding gebruiken waarmee gebruikers toegang krijgen tot hun [SaaS-toepassingen](../active-directory/active-directory-appssoaccess-whatis.md) op basis van hun account voor werk of school in Azure AD. Dit is van toepassing niet alleen voor Microsoft SaaS-apps, maar ook met andere apps, zoals [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) en [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). U kunt uw toepassing configureren voor gebruik van Azure AD als een [SAML gebaseerde identiteit](../active-directory/fundamentals-identity.md) provider. Als een besturingselement voor beveiliging, Azure AD niet uitgeven van een token waarmee gebruikers zich aanmelden bij de toepassing, tenzij ze de toegang via Azure AD hebt gekregen. U kunt toegang verlenen rechtstreeks of via een groep dat gebruikers lid zijn van zijn.

Organisaties die geen een gemeenschappelijke identiteit maakt voor het maken van eenmalige aanmelding voor hun gebruikers en toepassingen worden meer blootgesteld aan scenario's waarin gebruikers meerdere wachtwoorden hebben. Deze scenario's vergroot de kans dat gebruikers wachtwoorden opnieuw gebruiken of met behulp van zwakke wachtwoorden.
>>>>>>> 72fef8deb3b74739d94337401a4a4a0921e88f58

## <a name="turn-on-conditional-access"></a>Voorwaardelijke toegang inschakelen
Gebruikers kunnen toegang krijgen tot bronnen van uw organisatie met behulp van verschillende apparaten en apps vanaf elke locatie. Als IT-beheerder, die u wilt ervoor zorgen dat deze apparaten aan uw normen voor beveiliging en naleving. Alleen richten op wie toegang heeft tot een resource is niet voldoende meer.

<<<<<<< HEAD u meer informatie over Azure AD-eenmalige aanmelding met het lezen van het artikel [AD FS-beheer en aanpassingen met Azure AD Connect](../active-directory/hybrid/how-to-connect-fed-management.md).
=== Voor beveiliging en productiviteit in balans brengen, moet u echter nadenken over hoe u een resource wordt geopend voordat u kunt een beslissing van access control. Met voorwaardelijke toegang van Azure AD kunt u deze vereiste aanpakken. Met voorwaardelijke toegang, kunt u automatisch toegang tot het toegangsbeheer voor toegang tot uw cloud-apps die zijn gebaseerd op voorwaarden.
>>>>>>> 72fef8deb3b74739d94337401a4a4a0921e88f58

**Beste**: beheren en toegang tot bedrijfsresources beheren.  
**Details**: Azure AD configureren [voorwaardelijke toegang](../active-directory/active-directory-conditional-access-azure-portal.md) op basis van een groep, de locatie en de gevoeligheid van een toepassing voor SaaS-apps en Azure AD verbonden apps.

## <a name="enable-password-management"></a>Wachtwoordbeheer inschakelen
Als u meerdere tenants hebt of als u wilt dat gebruikers kunnen [hun eigen wachtwoorden opnieuw instellen](../active-directory/active-directory-passwords-update-your-own-password.md), is het belangrijk dat u passende beveiligingsbeleidsregels gebruiken om misbruik te voorkomen.

**Beste**: instellen van de self-service voor wachtwoord opnieuw instellen (SSPR) voor uw gebruikers.  
**Details**: Gebruik de Azure AD [Self-service voor wachtwoord opnieuw instellen](../active-directory-b2c/active-directory-b2c-reference-sspr.md) functie.

**Beste**: Monitor hoe of als SSPR daadwerkelijk wordt gebruikt.  
**Details**: bewaak de gebruikers die zijn geregistreerd met behulp van de Azure AD [activiteitenrapport voor wachtwoord opnieuw instellen van inschrijving](../active-directory/active-directory-passwords-get-insights.md). De rapportagefunctie die Azure AD biedt vindt u antwoorden op vragen met behulp van vooraf gedefinieerde rapporten. Als u op de juiste wijze in licentie gegeven bent, kunt u ook aangepaste query's maken.

## <a name="enforce-multi-factor-verification-for-users"></a>Meervoudige verificatie voor gebruikers afdwingen
Het is raadzaam dat u verificatie in twee stappen voor alle gebruikers vereisen. Dit omvat beheerders en anderen in uw organisatie die een aanzienlijke invloed hebben kan als hun account is aangetast (bijvoorbeeld financiële officers).

Er zijn meerdere opties voor het vereisen van verificatie in twee stappen. De beste optie voor u, is afhankelijk van uw doelstellingen, de Azure AD-editie die u uitvoert en uw licentieprogramma. Zie [hoe u verificatie in twee stappen vereist voor een gebruiker](../active-directory/authentication/howto-mfa-userstates.md) om te bepalen welke optie het beste voor u. Zie de [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) en [Azure multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) prijspagina's voor meer informatie over licenties en prijzen.

Hieronder vindt u opties en prestaties voor het inschakelen van verificatie in twee stappen:

**Optie 1**: [multi-factor Authentication inschakelen door het veranderen van gebruikersstatus](../active-directory/authentication/howto-mfa-userstates.md#enable-azure-mfa-by-changing-user-status).   
**Voordeel**: dit is de traditionele methode voor het vereisen van verificatie in twee stappen. Het werkt met beide [Azure multi-factor Authentication in de cloud en Azure multi-factor Authentication-Server](../active-directory/authentication/concept-mfa-whichversion.md). Met deze methode vereist dat gebruikers verificatie uitvoeren telkens wanneer ze zich aanmelden en vervangt beleid voor voorwaardelijke toegang.

**Optie 2**: [multi-factor Authentication inschakelen met beleid voor voorwaardelijke toegang](../active-directory/authentication/howto-mfa-getstarted.md#enable-multi-factor-authentication-with-conditional-access).   
**Voordeel**: deze optie kunt u te vragen om verificatie in twee stappen onder bepaalde omstandigheden, met behulp van [voorwaardelijke toegang](../active-directory/active-directory-conditional-access-azure-portal.md). Specifieke voorwaarden kunnen aanmelden van gebruikers vanuit verschillende locaties, niet-vertrouwde apparaten of toepassingen die u beschouwt als riskant zijn. Voor het definiëren van specifieke voorwaarden waar u verificatie in twee stappen vereist, kunt u voorkomen constante vragen om uw gebruikers, dit kunnen een onaangename gebruikerservaring.

Dit is de meest flexibele manier om in te schakelen van verificatie in twee stappen voor uw gebruikers. Als u een beleid voor voorwaardelijke toegang werkt alleen voor Azure multi-factor Authentication in de cloud en is een premium-functie van Azure AD. U vindt meer informatie over deze methode in [implementeren van cloud-gebaseerde Azure multi-factor Authentication](../active-directory/authentication/howto-mfa-getstarted.md).

**Optie 3**: multi-factor Authentication inschakelen met beleid voor voorwaardelijke toegang door het evalueren van de gebruiker en meld u risico's van [Azure AD Identity Protection](../active-directory/active-directory-identityprotection.md).   
**Voordeel**: deze optie kunt u naar:

- Detecteren van mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie.
- Automatische antwoorden om gedetecteerde verdachte activiteit die betrekking op de identiteiten van uw organisatie hebben te configureren.
- Verdachte incidenten onderzoeken en onderneem gepaste actie op te lossen.

Deze methode maakt gebruik van de risico-evaluatie van de Azure AD Identity Protection om te bepalen of verificatie in twee stappen vereist is op basis van gebruiker en aanmeldingsrisico voor alle cloudtoepassingen. Deze methode is een Azure Active Directory P2-licentie vereist. U vindt meer informatie over deze methode in [Azure Active Directory Identity Protection](../active-directory/identity-protection/overview.md).

> [!Note]
> Optie 1, meervoudige verificatie inschakelen door het veranderen van de status van de gebruiker, overschrijft de beleidsregels voor voorwaardelijke toegang. Aangezien de opties 2 en 3 gebruikt beleidsregels voor voorwaardelijke toegang, kunt u de optie 1 met hen niet gebruiken.

Organisaties die geen extra lagen van identiteitsbeveiliging, zoals verificatie, toevoegen zijn gevoeliger voor referentie diefstal aanval. Een referentie diefstal aanval kan leiden tot inbreuk op gegevens.

## <a name="use-role-based-access-control-rbac"></a>Op rollen gebaseerd toegangsbeheer (RBAC) gebruiken
Toegang beperken op basis van de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes is van cruciaal belang voor organisaties die willen beveiligingsbeleid voor toegang tot gegevens afdwingen. U kunt [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md) machtigingen toewijzen aan gebruikers, groepen en toepassingen met een bepaald bereik. Het bereik van een roltoewijzing kan een abonnement, een resourcegroep of één resource zijn.

U kunt [ingebouwde RBAC](../role-based-access-control/built-in-roles.md) rollen in Azure machtigingen toewijzen aan gebruikers. Organisaties die gegevenstoegangsbeheer niet afdwingen met behulp van mogelijkheden zoals RBAC mogelijk meer bevoegdheden dan nodig is om hun gebruikers geven. Dit kan leiden tot inbreuk op gegevens doordat de gebruikerstoegang tot bepaalde typen gegevens (bijvoorbeeld hoge impact op bedrijf) die ze al dan niet mogen hebben.

## <a name="lower-exposure-of-privileged-accounts"></a>Lagere blootstelling van bevoegde accounts
Het beveiligen van bevoegde is toegang een kritieke eerste stap in de bescherming van uw bedrijfsactiva. Door het aantal gebruikers die toegang hebben tot het beveiligen van gegevens of resources vermindert de kans dat een kwaadwillende gebruiker de toegang of een geautoriseerde gebruiker per ongeluk die betrekking hebben op een gevoelige resource.

Bevoegde accounts zijn accounts waarmee IT-systemen worden beheerd. Cyber aanvallers richt u op deze accounts om toegang tot de gegevens en systemen van een organisatie te krijgen. Als u wilt beveiligen van bevoegde toegang, moet u de accounts en systemen van het risico wordt blootgesteld aan een kwaadwillende gebruiker isoleren.

U wordt aangeraden dat u ontwikkelt en gaat u als een schema volgt voor het beveiligen van bevoegde toegang tegen cyberaanvallen aanvallers. Voor informatie over het maken van een gedetailleerde roadmap voor het beveiligen van identiteiten en toegang die worden beheerd of gemeld in Azure AD, Microsoft Azure, Office 365 en andere cloudservices, Bekijk [beveiligen van bevoegde toegang voor hybride en cloud-implementaties in Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

Het volgende bevat een overzicht van de aanbevolen procedures die zijn gevonden in [beveiligen van bevoegde toegang voor hybride en cloud-implementaties in Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md):

**Beste**: beheren, controleren en bewaken van toegang tot beschermde accounts.   
**Details**: inschakelen [Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/active-directory-securing-privileged-access.md). Nadat u Privileged Identity Management inschakelen, ontvangt u de melding e-mailberichten voor bevoegde toegang rol wijzigingen. Deze meldingen bieden vroegtijdig waarschuwen wanneer andere gebruikers worden toegevoegd aan maximaal bevoorrechte rollen in uw directory.

**Beste**: identificeren en categoriseren accounts in maximaal bevoorrechte rollen.   
**Details**: na het inschakelen van Azure AD Privileged Identity Management, bekijk de gebruikers die zich in de globale beheerder, beheerder met bevoorrechte rol en andere maximaal bevoorrechte rollen. Verwijderen van accounts die niet langer nodig zijn in deze rollen en de resterende accounts die zijn toegewezen aan beheerdersrollen categoriseren:

- Afzonderlijk toegewezen aan gebruikers met beheerdersrechten en kan worden gebruikt voor niet-administratieve doeleinden (bijvoorbeeld persoonlijke e-mailadres)
- Afzonderlijk toegewezen aan gebruikers met beheerdersrechten en aangewezen voor administratieve doeleinden alleen
- Gedeeld door meerdere gebruikers
- Voor scenario's voor toegang in noodgevallen
- Voor geautomatiseerde scripts
- Voor externe gebruikers

**Beste**: implementeren 'just-in-time' toegang verder verlagen de blootstellingstijd van bevoegdheden en vergroot uw inzicht in het gebruik van accounts met bevoegdheden (JIT).   
**Details**: Azure AD Privileged Identity Management kunt u:

- Gebruikers beperken tot alleen nemen over hun JIT-bevoegdheden.
- Rollen toewijzen voor een kortere duur met vertrouwen dat de bevoegdheden automatisch worden ingetrokken.

**Beste**: ten minste twee accounts voor toegang in noodgevallen definiëren.   
**Details**: de accounts voor toegang in noodgevallen waarmee organisaties bevoorrechte toegang in een bestaand Azure Active Directory-omgeving beperken. Deze accounts over uitgebreide beheerdersmogelijkheden en niet zijn toegewezen aan specifieke personen. Accounts voor toegang in noodgevallen zijn beperkt tot scenario's waarin de normale beheerdersaccounts kunnen niet worden gebruikt. Organisaties moeten van het noodgeval account gebruik om alleen de benodigde hoeveelheid tijd te beperken.

Evalueer de accounts die toegewezen of die in aanmerking komen voor de rol globale beheerder zijn. Als er geen accounts alleen in de cloud met behulp van de `*.onmicrosoft.com` domein (die bestemd zijn voor toegang in noodgevallen), maken ze. Zie beheerdersaccounts voor Noodtoegang beheren in Azure AD voor meer informatie.

**Beste**: multi-factor Authentication en Registreer alle andere over uitgebreide beheerdersmogelijkheden één gebruiker niet-gefedereerde-beheerdersaccounts inschakelen.  
**Details**: overeenkomende Azure multi-factor Authentication bij aanmelding voor alle afzonderlijke gebruikers die permanent zijn toegewezen aan een of meer van de Azure AD-beheerdersrollen: globale beheerder, beheerder met bevoorrechte rol, Exchange Online-beheerder en SharePoint Online-beheerder. De handleiding gebruiken om in te schakelen [multi-factor Authentication voor uw beheerdersaccounts](../active-directory/authentication/howto-mfa-userstates.md) en zorg ervoor dat alle gebruikers hebben [geregistreerd](https://aka.ms/mfasetup).

**Beste**: stappen nemen om de meest gebruikte aangevallen technieken.  
**Details**: [identificeren Microsoft-accounts in een beheerderrol die moet worden overgeschakeld naar de werk- of schoolaccount](../active-directory/users-groups-roles/directory-admin-roles-secure.md#identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts)  

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

**Beste**: een methode om te identificeren:

- Probeert aan te melden bij [zonder worden getraceerd](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md).
- [Brute force](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) aanvallen tegen een bepaald account.
- Probeert aan te melden bij vanaf meerdere locaties.
- Aanmeldingen vanaf [geïnfecteerde apparaten](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md).
- Verdachte IP-adressen.

**Details**: gebruik Azure AD Premium [anomaliedetectie rapporten](../active-directory/active-directory-view-access-usage-reports.md). Zorg ervoor dat processen en procedures voor IT-beheerders om uit te voeren van deze rapporten dagelijks of op aanvraag (meestal in een reactie op incidenten scenario).

**Beste**: hebt u een actieve bewakingssysteem dat de hoogte van risico's gebracht en risiconiveau (hoog, Gemiddeld of laag) voor uw zakelijke vereisten kunt aanpassen.   
**Details**: Gebruik [Azure AD Identity Protection](../active-directory/active-directory-identityprotection.md), die de huidige vlaggen risico's op een eigen dashboard en dagelijkse samenvatting meldingen via e-mail worden verzonden. Ter bescherming van identiteiten in uw organisatie, kunt u risico's gebaseerde beleidsregels die automatisch op gedetecteerde problemen reageren na het verstrijken van een opgegeven risiconiveau configureren.

Organisaties die hun identiteitssystemen niet controleren zijn op gebruiker referenties zijn aangetast. Medeweten verdachte activiteiten zijn plaatsvinden via deze referenties kunnen niet organisaties dit type bedreiging beperken.

## <a name="next-step"></a>Volgende stap
Zie [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md) voor meer best practices voor beveiliging moeten worden gebruikt wanneer bent u het ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.
