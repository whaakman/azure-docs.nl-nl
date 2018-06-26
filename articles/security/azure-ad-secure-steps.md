---
title: Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten in Azure Active Directory
description: Dit document bevat een lijst met belangrijke acties beheerders implementeren moeten, zodat ze veilig hun organisatie met behulp van Azure AD-mogelijkheden
services: active-directory
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 06/18/2018
ms.author: martincoetzer
ms.openlocfilehash: b15fff6e868bfac973f9d2a7277f0fac1e29d845
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938425"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten

Als u dit document leest, bent u rekening houden met het belang van beveiliging. U waarschijnlijk uitvoeren de verantwoordelijkheid voor het beveiligen van uw organisatie al. Als u nodig hebt om u te verleiden anderen het belang van beveiliging, verzenden om te lezen van de meest recente [Microsoft Security Intelligence rapport](https://www.microsoft.com/security/intelligence-report).

Dit document helpt u een veiliger houding met behulp van de mogelijkheden van Azure Active Directory met behulp van een controlelijst vijf stap voor uw organisatie tegen cyberbeveiliging aanvallen Inoculeer.

Deze controlelijst helpt u snel implementeren kritieke aanbevolen acties ter bescherming van uw organisatie direct door waarin wordt uitgelegd hoe:

* Versterking van uw referenties.
* Uw oppervlak aanval verminderen.
* Threat antwoord automatiseren.
* Vergroot uw kennis van controleren en bewaken.
* Meer voorspelbare en volledig door eindgebruikers beveiliging inschakelen met fouten.

> [!NOTE]
> Veel van de aanbevelingen in dit document gelden alleen voor toepassingen die zijn geconfigureerd voor gebruik van Azure Active Directory als de id-provider. Apps configureren voor eenmalige aanmelding kan de voordelen van de referentie-beleid, detectie van dreigingen toevoegen controle, registreren en andere functies aan deze toepassingen. [Eenmalige aanmelding via Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) vormt de basis - waarin alle deze aanbevelingen zijn gebaseerd.

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>Voordat u begint: beveiligen van bevoegde accounts met MFA

Voordat u deze controlelijst, zorg er dan voor dat u geen verkeren terwijl u bij het lezen van deze controlelijst. U moet eerst uw beschermde accounts beveiligen.

Aanvallers die controle van bevoorrechte accounts kunnen enorm veel schade doen, dus is het essentieel dat u eerst deze accounts beveiligen. Inschakelen en vereisen [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) (MFA) voor alle beheerders in uw organisatie met [basislijn beveiliging](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-baseline-protection). Als u MFA niet hebt geïmplementeerd, doet u het nu! Het is dat belangrijk.

Alle instellen? Aan de slag in de controlelijst.

## <a name="step-1---strengthen-your-credentials"></a>Stap 1: versterking van uw referenties 

De meeste enterprise beveiligingslekken afkomstig zijn met een account met een van een aantal methoden zoals wachtwoord spuitnevel, inbreuk opnieuw afspelen of phishing aangetast. Meer informatie over deze aanvallen in deze video:
> [!VIDEO https://channel9.msdn.com/events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3016/player]

Als gebruikers in uw identiteitssysteem zwakke wachtwoorden en ze niet met multi-factor authentication versterking, een kwestie van als of wanneer u verkeren – alleen 'hoe vaak.' is niet

### <a name="make-sure-your-organization-use-strong-authentication"></a>Controleer of uw organisatie gebruik sterke verificatie

Gezien de frequentie van wachtwoorden worden geraden, phished, gestolen met schadelijke software of opnieuw worden gebruikt, is het essentieel dat u het wachtwoord met een vorm van sterke referentie back: meer informatie over [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication).

### <a name="turn-off-traditional-complexity-expiration-rules-and-start-banning-commonly-attacked-passwords-instead"></a>Traditionele complexiteit, verlopen regels, uitschakelen en vaak aangevallen wachtwoorden in plaats daarvan verboden starten

Veel organisaties gebruiken de traditionele complexiteit (bijvoorbeeld speciale tekens) en de regels voor het verlopen van wachtwoorden. Microsoft onderzoek heeft aangetoond dat deze beleidsregels schadelijk, waardoor gebruikers wachtwoorden moeten kiezen die gemakkelijker zijn te raden zijn.

De aanbevelingen van Microsoft, consistent zijn met [richtlijnen van het NIST](https://pages.nist.gov/800-63-3/sp800-63b.html), zijn de volgende drie implementeren:

1. Vereisen wachtwoorden hebben ten minste 8 tekens. Meer is niet noodzakelijkerwijs beter als ze voorkomen gebruikers kiezen voorspelbare wachtwoorden dat, wachtwoorden opslaan in bestanden of schrijf ze op.
2. Verlopen van regels, die gebruikers eenvoudig verzonnen wachtwoorden zoals station uitschakelen **Summer2018!**.
3. Schakel de teken samenstelling vereisten en te voorkomen dat gebruikers kiezen vaak aangevallen wachtwoorden, omdat deze gebruikers kunnen kiezen voorspelbare teken vervangingen in wachtwoorden.

U kunt [PowerShell om te voorkomen dat wachtwoorden verlopen](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy) van gebruikers als u een identiteit in Azure AD rechtstreeks maken. Organisaties met on-premises AD met Azure AD Connect sync-identiteiten met Azure AD (ook wel bekend als een hybride implementatie), moet lokale implementeren [intelligent wachtwoordbeleid](https://aka.ms/passwordguidance) met [domeingroepsbeleid instellingen](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10)) of [Windows PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy).

Azure Active Directory [dynamische verboden wachtwoord](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords) functie huidige aanvaller gedrag gebruikt om te voorkomen dat gebruikers van het instellen van wachtwoorden die gemakkelijk kunnen worden geraden. Deze mogelijkheid is altijd ingeschakeld en organisaties met een hybride implementatie kunnen profiteren van deze functie doordat [wachtwoord terugschrijven](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-writeback) of ze kunnen implementeren [wachtwoordbeveiliging Azure AD voor Windows Server Active Directory](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises). Azure AD-wachtwoordbeveiliging voorkomen dat gebruikers in het algemeen gebruikte wachtwoorden kiezen en aangepaste wachtwoorden die u kunt configureren.

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>Beschermt tegen gestolen referenties en herstelmogelijkheden tegen storingen toevoegen

Als uw organisatie gebruikmaakt van een hybride identiteitsoplossing, moet u synchronisatie van wachtwoordhash inschakelen om de volgende twee redenen:

* De [gebruikers met gelekte referenties](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) rapport in de Azure AD waarschuwt u van gebruikersnaam en wachtwoord waardeparen die zijn blootgesteld op de 'donkere web'. Er is een enorme hoeveelheid wachtwoorden gelekt via phishing, malware en het wachtwoord opnieuw kunnen worden gebruikt op sites van derden die later worden geschonden. Microsoft vindt u veel van deze gelekte referenties en vertelt u, in dit rapport als deze overeenkomen met de referenties in uw organisatie, maar alleen als u [hash wachtwoordsynchronisatie inschakelen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)!
* In het geval van een storing van de lokale (bijvoorbeeld in een aanval ransomware) u kunt dan wel overschakelen op [cloud-verificatie met behulp van synchronisatie van wachtwoordhash](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). Deze back-verificatiemethode kunt u doorgaan bij toegang tot apps die zijn geconfigureerd voor verificatie met Azure Active Directory, met inbegrip van Office 365.

Meer informatie over het [synchronisatie van wachtwoordhash](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization) werkt.

### <a name="implement-ad-fs-extranet-lockout"></a>Implementeer AD FS-extranetvergrendeling

Organisaties die toepassingen om te verifiëren rechtstreeks naar Azure AD configureren profiteren van [smart Azure AD-vergrendeling](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords). AD FS te implementeren als u AD FS gebruikt, [extranetvergrendeling](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection). Extranet lockout beschermd tegen gewelddadige aanvallen, welke doel AD FS bij het voorkomen dat gebruikers wordt vergrendeld in de Active Directory.

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>Profiteren van intrinsiek veilige, eenvoudiger om referenties te gebruiken

Met behulp van [Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification), kunt u wachtwoorden vervangen door sterke verificatie met twee factoren op pc's en mobiele apparaten. Deze verificatie bestaat uit een nieuw type gebruikersreferentie die is gekoppeld aan een apparaat en gebruikt een biometrische of PINCODE.

## <a name="step-2---reduce-your-attack-surface"></a>Stap 2: uw kwetsbaarheid te verminderen

Gezien de pervasiveness wachtwoord inbreuk, is voor het minimaliseren van de kwetsbaarheid voor aanvallen in uw organisatie essentieel. Gebruik van protocollen voor oudere, minder veilig, beperken toegangsvermelding elimineren toegangspunten en meer controle over beheerderstoegang tot bronnen uitoefenen vermindert het oppervlak aanval.

### <a name="block-legacy-authentication"></a>Verouderde verificatie blok

Apps met behulp van hun eigen verouderde methoden voor verificatie met Azure AD en toegang hebben tot bedrijfsgegevens, vormen een ander risico voor organisaties. Voorbeelden van apps met verouderde verificatie zijn POP3-, IMAP4 of SMTP-clients. Verouderde verificatie apps namens de gebruiker verifiëren en te voorkomen dat Azure AD doen geavanceerde beveiliging evaluaties. De alternatieve, moderne verificatie, beperkt u het beveiligingsrisico omdat deze biedt ondersteuning voor meervoudige verificatie en voorwaardelijke toegang. U wordt aangeraden de volgende drie acties:

1. Blok [verouderde verificatie als u AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12).
2. Setup [SharePoint Online en Exchange Online moderne verificatie gebruiken](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
3. Gebruik [beleidsregels voor voorwaardelijke toegang blokkeren verouderde verificatie](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication).

### <a name="block-invalid-authentication-entry-points"></a>Blok ongeldige verificatie-toegangspunten

Met behulp van de aannemen dat inbreuk commerciëlere mentaliteit, moet u reduceert de gevolgen van waarmee is geknoeid gebruikersreferenties wanneer ze optreden. Voor elke app in uw omgeving kunt u de geldige gebruiksvoorbeelden: welke groepen, welke netwerken welke apparaten en andere elementen zijn gemachtigd – en vervolgens de rest blokkeren. Wees voorzichtig te beperken van het gebruik van [maximaal bevoorrechte of service-accounts](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices). Met [voorwaardelijke toegang van Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal), kunt u bepalen hoe geautoriseerde gebruikerstoegang tot hun apps en resources op basis van specifieke voorwaarden die u definieert.

Wat aandachtiger naar service-accounts (gebruikt voor het uitvoeren van taken op automatische wijze-accounts). Voorwaardelijke toegang gebruikt, kunt u ervoor zorgen dergelijke accounts kunnen alleen worden uitgevoerd op de service van de IP- en op het moment van de dag waarvoor geschikt is.

### <a name="implement-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management implementeren

Een andere gevolgen van het 'wordt ervan uitgegaan dat inbreuk' is het nodig om de kans dat die een verdacht account met een bevoorrechte rol werken kan. 

[Azure AD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) helpt u accountmachtigingen geminimaliseerd door u te helpen:

* Identificeren en te beheren van gebruikers met beheerdersrechten rol.
* Inzicht in niet-gebruikte of overmatige bevoegdheid rollen die moet worden verwijderd.
* Tot stand brengen regels voor Zorg ervoor dat bevoorrechte rollen zijn beveiligd door multi-factor authentication.
* Tot stand brengen regels om te controleren of bevoorrechte rollen alleen lang genoeg krijgen de bevoorrechte taak uit te voeren.

Azure AD PIM inschakelen en vervolgens de gebruikers bekijken die zijn toegewezen beheerdersrollen en verwijder overbodige accounts in deze rollen. Voor de resterende bevoegde gebruikers, verplaatsen van permanente naar in aanmerking komt. Ten slotte passend beleid om ervoor te zorgen te maken wanneer ze toegang te krijgen tot de bevoorrechte rollen, ze kunnen doen veilig.

Als onderdeel van de implementatie van uw proces bevoegd account, volgt u de [aanbevolen om ten minste twee noodgevallen accounts maken](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices) om ervoor te zorgen dat u hebt toegang tot Azure AD als u zelf worden vergrendeld.

## <a name="step-3---automate-threat-response"></a>Stap 3: threat antwoord automatiseren

Azure Active Directory heeft veel mogelijkheden die automatisch voor het verwijderen van de latentie tussen detectie en antwoord-aanvallen te onderscheppen. U kunt de kosten te verlagen en risico's, wanneer u de tijd criminelen verminderen met zichzelf insluiten in uw omgeving. Hier volgen de concrete stappen die u kunt uitvoeren.

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>Implementeren met behulp van Azure AD Identity Protection beveiligingsbeleid van risico's van gebruiker

Gebruiker risico geeft de kans op de identiteit van een gebruiker er inbreuk is gemaakt en wordt berekend op basis van de [risicogebeurtenissen gebruiker](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) die zijn gekoppeld aan de identiteit van een gebruiker. Gebruikersbeleid risico is een beleid voor voorwaardelijke toegang die wordt geëvalueerd als het risiconiveau aan een specifieke gebruiker of groep. Op basis van de laag, worden gemiddeld, hoog risiconiveau, een beleid geconfigureerd voor toegang wordt geblokkeerd of een beveiligde wachtwoordwijziging met multi-factor authentication vereisen. Microsoft aanbeveling is een wijziging beveiligd wachtwoord vereisen voor gebruikers op een hoog risico.

![Gebruikers voor wie wordt aangegeven dat ze risico lopen](media/azure-ad/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>Beleid voor aanmelden risico implementeren met behulp van Azure AD Identity Protection

Aanmelden risico is de kans dat iemand anders dan de eigenaar van het account probeert aan te melden met behulp van de identiteit. Een [aanmelden risico beleid](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) is een beleid voor voorwaardelijke toegang die wordt geëvalueerd als het risiconiveau aan een specifieke gebruiker of groep. Op basis van het risiconiveau (gemiddeld-hoog/laag), worden een beleid geconfigureerd voor toegang wordt geblokkeerd of afdwingen van multi-factor authentication-server. Zorg ervoor dat u MFA afdwingen op Gemiddeld of hoger risico aanmeldingen.

![Aanmelden van anonieme IP-adressen](media/azure-ad/azure-ad-sec-steps2.png)

## <a name="step-4---increase-your-awareness"></a>Stap 4: uw awareness verhogen

Controle en logboekregistratie van gebeurtenissen die betrekking hebben op beveiliging en gerelateerde waarschuwingen zijn essentiële onderdelen van een strategie doeltreffende beveiliging. Beveiligingslogboeken en rapporten bieden u een elektronisch record van verdachte activiteiten en helpen u de patronen die geprobeerde of geslaagde externe binnendringen van het netwerk en interne aanvallen aangeven mogelijk detecteren. U kunt controle gebruikersactiviteit regelgeving document controleren, voert u forensische analyse en meer. Waarschuwingen bevatten meldingen beveiligingsgebeurtenissen.

### <a name="monitor-azure-ad"></a>Monitor voor Azure AD

Microsoft Azure-services en functies bieden u configureerbare beveiliging voor controle en logboekregistratie opties waarmee u hiaten in uw beveiligingsbeleid en de mechanismen en los deze lacunes ter voorkoming van inbreuk. U kunt [Azure logboekregistratie en controle](https://docs.microsoft.com/azure/security/azure-log-audit) en gebruiken [activiteitsrapporten in de Azure Active Directory-portal Audit](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs).

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>Azure AD Connect Health in hybride omgevingen bewaken

[AD FS met Azure AD Connect Health Monitoring](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs) biedt beter inzicht in mogelijke problemen en zichtbaarheid van aanvallen op uw AD FS-infrastructuur. Azure AD Connect Health biedt waarschuwingen met meer informatie, stappen voor het oplossen en koppelingen naar gerelateerde documentatie; gebruiksanalyse voor verschillende metrische gegevens die zijn gerelateerd aan verificatieverkeer; bewaking van toepassingsprestaties en rapporten.

![Azure AD Connect Health](media/azure-ad/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>Azure AD Identity Protection gebeurtenissen controleren

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) is een melding, bewaking en rapportage van hulpprogramma voor het detecteren van mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie. Het risicogebeurtenissen, zoals gelekte referenties, onmogelijke reis detecteert en aanmeldingen vanaf geïnfecteerde apparaten, anonieme IP-adressen, IP-adressen die zijn gekoppeld aan de verdachte activiteit en onbekende locaties. Melding van waarschuwingen voor het ontvangen van e-mailadres van gebruikers risico en/of een wekelijks overzicht via e-mail inschakelen.

![Gebruikers voor wie wordt aangegeven dat ze risico lopen](media/azure-ad/azure-ad-sec-steps3.png)

## <a name="step-5---enable-end-user-self-help"></a>Stap 5 - inschakelen door eindgebruikers zelfhulp

Zo veel mogelijk moet u beveiliging met productiviteit worden verdeeld. Op dezelfde manier van bijna uw reis met de denkt dat u een basis voor beveiliging op lange termijn wilt instellen, kunt u wrijving verwijderen uit uw organisatie door uw gebruikers tijdens de vigilant. 

### <a name="implement-self-service-password-reset"></a>Selfservice voor wachtwoordherstel implementeren

Azure [selfservice voor wachtwoordherstel (SSPR)](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started) biedt een eenvoudige methode voor IT-beheerders kunnen gebruikers opnieuw instellen of hun wachtwoorden of accounts zonder tussenkomst van de beheerder te ontgrendelen. Het systeem biedt gedetailleerde rapporten zodat u kunt volgen wanneer gebruikers het systeem openen. U ontvangt ook meldingen om u te waarschuwen over misbruik. 

### <a name="implement-self-service-group-management"></a>Self-service groepsbeheer implementeren

Azure AD biedt de mogelijkheid om toegang tot bronnen met behulp van beveiligingsgroepen en Office 365-groepen te beheren. Deze groepen kunnen worden beheerd door de eigenaars Groepsbeleid in plaats van de IT-beheerders. Ook wel [selfservicegroepsbeheer](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), met deze functie kunt eigenaars Groepsbeleid die een beheerderrol maken en beheren van groepen zonder afhankelijk te zijn van de beheerders om hun aanvragen te verwerken niet zijn toegewezen.

### <a name="implement-azure-ad-access-reviews"></a>Implementeer Azure AD access beoordelingen

Met [beoordeelt de toegang van Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), kunt u lidmaatschap van groepen, toegang tot bedrijfstoepassingen, beheren en bevoorrechte roltoewijzingen aan Zorg dat er een standaard geeft geen gebruikers toegang voor uitgebreide perioden de tijd is niet nodig hebben.

## <a name="summary"></a>Samenvatting

Er zijn veel aspecten van een beveiligde infrastructuur voor identiteiten, maar deze controlelijst vijf stap helpt u snel een veiliger en veilige identity-infrastructuur uitvoeren:

* Versterking van uw referenties.
* Uw oppervlak aanval verminderen.
* Threat antwoord automatiseren.
* Vergroot uw kennis van controleren en bewaken.
* Meer voorspelbare en volledig door eindgebruikers beveiliging inschakelen met fouten.

Bedankt hoe ernstig identiteit beveiliging en hopen dat u dat dit document is een handig overzicht van een veiliger houding voor uw organisatie.

## <a name="next-steps"></a>Volgende stappen
Als u hulp nodig bij het plannen en implementeren van de aanbevelingen, raadpleegt u de [Azure AD-implementatieplannen project](http://aka.ms/deploymentplans) voor hulp.
