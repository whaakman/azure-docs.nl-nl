---
title: Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten in Azure Active Directory
description: Dit document bevat een lijst met belangrijke acties beheerders implementeren moeten om hen te helpen beveiligen hun organisatie met behulp van Azure AD-mogelijkheden
services: active-directory
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 06/18/2018
ms.author: martincoetzer
ms.openlocfilehash: 2a6c94d902c639a2529e501347876d63a3f56d7e
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505606"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten

Als u dit document leest, bent u op de hoogte van de betekenis van beveiliging. U waarschijnlijk uitvoeren de verantwoordelijkheid voor het beveiligen van uw organisatie al. Als u nodig hebt om ervan te overtuigen anderen van het belang van beveiliging, stuurt u hun lezen van de meest recente [Microsoft Security Intelligence report](https://www.microsoft.com/security/intelligence-report).

Dit document helpt u een beter beveiligde verbeteren met behulp van de mogelijkheden van Azure Active Directory met behulp van een controlelijst vijf stappen naar inenten van uw organisatie tegen cyberaanvallen.

Deze controlelijst helpt u snel implementeren kritieke aanbevolen acties ter bescherming van uw organisatie direct door waarin wordt uitgelegd hoe u:

* Optimaliseer gebruik van uw referenties.
* Verminder de oppervlakte van de aanval.
* Automatiseer bedreigingen te kunnen reageren.
* Vergroot uw kennis van controleren en bewaken.
* Meer voorspelbare en volledig door eindgebruikers beveiliging inschakelen met zelfhulpinformatie.

> [!NOTE]
> Veel van de aanbevelingen in dit document gelden alleen voor toepassingen die zijn geconfigureerd voor het gebruik van Azure Active Directory als de id-provider. Apps configureren voor eenmalige aanmelding bent u verzekerd van de voordelen van de referentie-beleid, detectie van bedreigingen, toevoegen controle, logboekregistratie en andere functies aan deze toepassingen. [Eenmalige aanmelding via Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) vormt de basis - waarin alle deze aanbevelingen zijn gebaseerd.

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>Voordat u begint: beveiligen van bevoegde accounts met MFA

Voordat u deze controlelijst, zorg er dan voor dat u geen verkeren terwijl u deze controlelijst leest. U moet eerst uw beschermde accounts beschermen.

Aanvallers die beheer van bevoegde accounts krijgen kunnen grote schade doen, dus het is essentieel dat u eerst deze accounts beveiligen. Inschakelen en vereisen [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) (MFA) voor alle beheerders in uw organisatie met [basisbeveiliging](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-baseline-protection). Als u MFA nog niet hebt geïmplementeerd, moet u dit nu doet. Het is dat belangrijk.

Alle ingesteld? Aan de slag in de controlelijst.

## <a name="step-1---strengthen-your-credentials"></a>Stap 1: het Optimaliseer het gebruik van uw referenties 

De meeste enterprise beveiligingsinbreuken afkomstig zijn met een account met een van een aantal methoden, zoals wachtwoord spray, inbreuk opnieuw afspelen of phishing aangetast. Meer informatie over deze aanvallen in deze video:
> [!VIDEO https://channel9.msdn.com/events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3016/player]

Als gebruikers in uw identiteitssysteem zwakke wachtwoorden en ze niet met multi-factor authentication te versterken, het een paar niet als of wanneer u verkeren – alleen "hoe vaak."

### <a name="make-sure-your-organization-use-strong-authentication"></a>Zorg ervoor dat uw organisatie gebruik sterke verificatie

Gezien de frequentie van de wachtwoorden worden geraden, phished, met schadelijke software wordt gestolen of opnieuw worden gebruikt, is het essentieel is voor back-het wachtwoord met een vorm van sterke referentie: meer informatie over [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication).

### <a name="turn-off-traditional-complexity-expiration-rules-and-start-banning-commonly-attacked-passwords-instead"></a>Traditionele complexiteit, regels, vervaldatum, uitschakelen en start vaak aangevallen wachtwoorden in plaats daarvan verbieden

Veel organisaties gebruiken de traditionele complexiteit (bijvoorbeeld speciale tekens) en de regels voor het verlopen van wachtwoorden. Van Microsoft research heeft laten zien voor dat dit beleid is een schadelijk te zijn, waardoor gebruikers wachtwoorden moeten kiezen die gemakkelijker zijn te achterhalen.

Aanbevelingen van Microsoft, consistent zijn met [richtlijnen van het NIST](https://pages.nist.gov/800-63-3/sp800-63b.html), zijn voor het implementeren van de volgende drie:

1. Vereisen dat wachtwoorden hebben ten minste 8 tekens. Meer is niet noodzakelijkerwijs beter, omdat ze ervoor zorgen gebruikers dat kunnen kiezen een voorspelbare wachtwoord, wachtwoorden opslaan in bestanden of schrijf ze op.
2. Vervaldatum regels, die gebruikers eenvoudig gegiste wachtwoorden zoals vergroten uitschakelen **Summer2018!**.
3. Schakel teken samenstelling vereisten en te voorkomen dat gebruikers vaak aangevallen wachtwoorden kiezen als ze ervoor zorgen gebruikers een dat voor voorspelbare teken vervangingen in wachtwoorden.

U kunt [PowerShell om te voorkomen dat wachtwoorden verlopen](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy) op gebruikers als u een identiteit in Azure AD rechtstreeks maken. Organisaties met on-premises AD met Azure AD Connect sync-identiteiten met Azure AD (ook bekend als een hybride implementatie), moet de on-premises implementeren [intelligente wachtwoordbeleid](https://aka.ms/passwordguidance) met behulp van [Groepsbeleid voor domein instellingen voor](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10)) of [Windows PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy).

Azure Active Directory [dynamische uitgesloten wachtwoorden](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords) functie gedrag van het huidige aanvaller gebruikt om te voorkomen dat gebruikers van het instellen van wachtwoorden die eenvoudig kunnen worden geraden. Deze mogelijkheid is altijd ingeschakeld en organisaties met een hybride implementatie kunnen profiteren van deze functie door in te schakelen [wachtwoord terugschrijven](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-writeback) of ze kunnen implementeren [beveiliging van Azure AD-wachtwoord voor Windows Server Active Directory](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises). Azure AD-wachtwoordbeveiliging voorkomen dat gebruikers in het algemeen gebruikte wachtwoorden kiezen en aangepaste wachtwoorden die u kunt configureren.

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>Beschermt tegen gestolen referenties en tolerantie tegen storingen toevoegen

Als uw organisatie gebruikmaakt van een oplossing voor hybride identiteit, moet u wachtwoord-hashsynchronisatie inschakelen voor de volgende twee redenen:

* De [gebruikers met de referenties zijn gelekt](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) rapport in de Azure AD waarschuwt u van de gebruikersnaam en wachtwoord-paren, die zijn blootgesteld op het 'dark web'. Een ongelooflijke volume van wachtwoorden is gelekt via phishing, malware en het wachtwoord gebruiken voor de sites van derden die later worden geschonden. Microsoft vindt u veel van deze gelekte referenties en vertelt u, in dit rapport, als ze overeenkomen met de referenties in uw organisatie, maar alleen als u [synchronisatie van wachtwoordhashes inschakelen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)!
* Na een storing van de on-premises (bijvoorbeeld in een ransomware-aanval) u moet mogelijk zijn overgestapt naar [cloud-verificatie met behulp van wachtwoord-hashsynchronisatie](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). Deze back-verificatiemethode kunt u blijven toegang houden tot apps die zijn geconfigureerd voor verificatie met Azure Active Directory, met inbegrip van Office 365.

Meer informatie over hoe u [wachtwoord-hashsynchronisatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization) werkt.

### <a name="implement-ad-fs-extranet-lockout"></a>Implementeer AD FS-extranetvergrendeling

Organisaties die toepassingen configureren voor het verifiëren rechtstreeks met Azure AD kunnen gebruikmaken van [slimme vergrendeling van het Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords). AD FS te implementeren als u AD FS in Windows Server 2012R2, [beveiligingsvergrendeling](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection). Als u AD FS op Windows Server 2016 gebruiken, implementeren [van het extranet slimme](https://support.microsoft.com/en-us/help/4096478/extranet-smart-lockout-feature-in-windows-server-2016). AD FS slimme Extranet lockout beschermt tegen brute force-aanvallen, welke doel AD FS bij het voorkomen dat gebruikers ze worden geblokkeerd in Active Directory.

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>Profiteer van intrinsiek veilige, eenvoudiger te gebruiken referenties

Met behulp van [Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification), kunt u wachtwoorden vervangen door sterke tweeledige verificatie op pc's en mobiele apparaten. Deze verificatie bestaat uit een nieuw type referenties van een gebruiker die is gekoppeld aan een apparaat en maakt gebruik van een biometrische of PINCODE.

## <a name="step-2---reduce-your-attack-surface"></a>Stap 2: uw kwetsbaarheid voor aanvallen verminderen

Gezien de alomtegenwoordigheid van wachtwoord inbreuk, is het minimaliseren van de kwetsbaarheid van uw organisatie essentieel. Gebruik van protocollen voor oudere, minder veilig, beperken toegangsvermelding elimineren verwijst en te oefenen met belangrijker controle over beheerderstoegang tot bronnen te helpen de aanval surface area beperken.

### <a name="block-legacy-authentication"></a>Verouderde verificatie blokkeren

Apps met behulp van hun eigen verouderde methoden voor verificatie met Azure AD en toegang hebben tot bedrijfsgegevens, vormen een ander risico voor organisaties. Voorbeelden van apps met verouderde verificatie zijn POP3-, IMAP4 of SMTP-clients. Verouderde apps verifiëren namens de gebruiker en voorkomen dat dit geavanceerde beveiliging evaluaties Azure AD. De alternatieve, moderne verificatie, beperkt u uw beveiligingsrisico, omdat het ondersteuning biedt voor meervoudige verificatie en voorwaardelijke toegang. U wordt aangeraden de volgende drie acties:

1. Blok [verouderde verificatie als u AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12).
2. Setup [SharePoint Online en Exchange Online gebruik van moderne verificatie](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
3. Gebruik [beleid voor voorwaardelijke toegang moet worden geblokkeerd dat verouderde](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication).

### <a name="block-invalid-authentication-entry-points"></a>Ongeldige verificatie-toegangspunten blokkeren

Met behulp van de commerciëlere uitgaan van inbreuk mentaliteit, moet u de impact van de referenties waarmee is geknoeid gebruiker beperken wanneer ze zich voordoen. Voor elke app in uw omgeving kunt u overwegen de geldige use cases: welke groepen, welke netwerken, welke apparaten en andere elementen zijn toegestaan: en blokkeren van de rest. Let erop dat het beperken van het gebruik van [maximaal beschermde of service-accounts](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices). Met [voorwaardelijke toegang voor Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal), kunt u bepalen hoe gemachtigde gebruikerstoegang tot hun apps en resources op basis van specifieke voorwaarden die u definieert.

Let vooral op service-accounts (gebruikt voor het uitvoeren van taken op automatische wijze-accounts). Door middel van voorwaardelijke toegang, kunt u ervoor zorgen deze accounts alleen voor de service, kunnen uitgevoerd vanaf het IP-adres, en op het moment van de dag, die geschikt is.

### <a name="implement-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management implementeren

Een andere gevolgen van het "uitgaan van inbreuk" is het nodig om de kans dat die een verdacht account met een bevoorrechte rol werken kan. 

[Azure AD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) helpt u accountmachtigingen geminimaliseerd door u te helpen:

* Identificeren en te beheren van gebruikers die zijn toegewezen aan beheerdersrollen.
* Krijg inzicht in niet-gebruikte of overmatige bevoegdheden rollen die te verwijderen.
* Stel regels om te maken dat bevoorrechte rollen zijn beveiligd door multi-factor authentication.
* Stel regels om te controleren of bevoorrechte rollen alleen lang genoeg zijn verleend aan het uitvoeren van de bevoegde taak.

Azure AD PIM inschakelen en vervolgens de gebruikers weergeven die zijn toegewezen beheerdersrollen en verwijderen van onnodige accounts in deze rollen. Voor de resterende bevoegde gebruikers, verplaatst u ze van permanente in aanmerking komende. Ten slotte passend beleid om ervoor te zorgen te maken wanneer ze toegang kunnen krijgen tot de bevoorrechte rollen nodig hebben, ze kunnen doen veilig.

Als onderdeel van de implementatie van uw proces bevoegd account, gaat u als volgt de [best practice om ten minste twee noodgevallen accounts te maken](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices) om ervoor te zorgen dat u hebt toegang tot Azure AD als u ontzeg uzelf.

## <a name="step-3---automate-threat-response"></a>Stap 3: bedreigingen te kunnen reageren automatiseren

Azure Active Directory heeft veel mogelijkheden die automatisch worden onderschept aanvallen, als u wilt verwijderen van de latentie tussen detectie en reactie. Vermindert u de kosten en risico's, wanneer u de tijd criminelen verminderen met zichzelf insluiten in uw omgeving. Hier volgen de concrete stappen die u kunt uitvoeren.

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>Beveiligingsbeleid voor gebruikersrisico's met behulp van Azure AD Identity Protection implementeren

Gebruikersrisico geeft aan dat de kans dat de identiteit van een gebruiker is geïnfecteerd en wordt berekend op basis van de [gebruiker risicogebeurtenissen](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) die zijn gekoppeld aan de identiteit van een gebruiker. Een beleid voor gebruikersrisico's is een beleid voor voorwaardelijke toegang die wordt geëvalueerd als het risiconiveau dat aan een specifieke gebruiker of groep. Op basis van laag, worden gemiddeld, hoog risiconiveau, een beleid geconfigureerd voor het blokkeren van toegang of een veilig wachtwoord wijzigen met multi-factor authentication vereisen. De aanbeveling van Microsoft is om te vereisen een beveiligde wachtwoorden wijzigen voor gebruikers op een hoog risico.

![Gebruikers voor wie wordt aangegeven dat ze risico lopen](media/azure-ad/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>Beleid voor aanmeldingsrisico implementeren met behulp van Azure AD Identity Protection

Aanmeldingsrisico is de kans dat iemand anders dan de eigenaar van het account probeert aan te melden met behulp van de identiteit. Een [beleid voor aanmeldingsrisico](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) is een beleid voor voorwaardelijke toegang die wordt geëvalueerd als het risiconiveau dat aan een specifieke gebruiker of groep. Op basis van het risiconiveau (gemiddeld-hoog/laag), worden een beleid geconfigureerd voor de toegang blokkeren of multi-factor authentication afdwingen. Zorg ervoor dat u meervoudige verificatie afdwingen op Medium of een hoger risico-aanmeldingen.

![Zich aanmelden vanaf anonieme IP-adressen](media/azure-ad/azure-ad-sec-steps2.png)

## <a name="step-4---increase-your-awareness"></a>Stap 4: uw kennis vergroten

Controle en logboekregistratie van beveiligingsgerelateerde gebeurtenissen en gerelateerde waarschuwingen zijn essentiële onderdelen van een doeltreffende beveiliging-strategie. Beveiligingslogboeken en rapporten bieden u een elektronische record van verdachte activiteiten en kunt u patronen die op pogingen tot of geslaagde externe indringingstests van het netwerk en interne aanvallen duiden kunnen detecteren. U kunt controle gebruiken om te controleren van gebruikersactiviteiten, document naleving van regelgeving, forensische analyse en meer uitvoeren. Waarschuwingen geven meldingen van beveiligingsgebeurtenissen.

### <a name="monitor-azure-ad"></a>Monitor voor Azure AD

Microsoft Azure-services en functies bieden u configureerbare beveiliging, controle en logboekregistratie opties waarmee u kunt hiaten in uw beveiligingsbeleid en -mechanismen identificeren en oplossen van deze hiaten om te voorkomen dat schendingen. U kunt [logboekregistratie van Azure en controle](https://docs.microsoft.com/azure/security/azure-log-audit) en gebruik [Controleactiviteitenrapporten in de Azure Active Directory-portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs).

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>Azure AD Connect Health in hybride omgevingen bewaken

[AD FS met Azure AD Connect Health Monitoring](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs) biedt u meer inzicht krijgen in de potentiële problemen en zichtbaarheid van aanvallen op uw AD FS-infrastructuur. Azure AD Connect Health biedt waarschuwingen met meer informatie, stappen voor het oplossen en koppelingen naar gerelateerde documentatie; gebruiksanalyse voor verschillende metrische gegevens met betrekking tot verificatieverkeer; bewaking van toepassingsprestaties en rapporten.

![Azure AD Connect Health](media/azure-ad/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>Azure AD Identity Protection-gebeurtenissen controleren

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) is een melding, controle en rapportage waarmee u kunt voor het detecteren van mogelijke beveiligingsproblemen die betrekking hebben op de identiteiten van uw organisatie. Gebeurtenissen van de risico's, zoals de referenties zijn gelekt, onmogelijk traject gedetecteerd en aanmeldingen vanaf geïnfecteerde apparaten, anonieme IP-adressen, IP-adressen die zijn gekoppeld aan de verdachte activiteit en onbekende locaties. Waarschuwingen voor het ontvangen van e-mailadres van gebruikers die risico lopen en/of een e-mail met de wekelijkse samenvatting melding inschakelen.

![Gebruikers voor wie wordt aangegeven dat ze risico lopen](media/azure-ad/azure-ad-sec-steps3.png)

## <a name="step-5---enable-end-user-self-help"></a>Stap 5: inschakelen door eindgebruikers zelfhulp

Zo veel mogelijk moet u beveiliging met productiviteit in balans brengen. Langs de dezelfde regels van het benaderen van uw ontwikkelproces met de denkt dat u een basis voor beveiliging op lange termijn wilt instellen, kunt u problemen van uw organisatie door uw gebruikers en tegelijkertijd de kosten waakzaam verwijderen. 

### <a name="implement-self-service-password-reset"></a>Selfservice voor wachtwoordherstel implementeren

Azure [selfservice voor wachtwoordherstel (SSPR)](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started) biedt een eenvoudige manier voor IT-beheerders kunnen gebruikers hun wachtwoord of account zonder tussenkomst van de beheerder ontgrendelen of opnieuw instellen. Het systeem biedt gedetailleerde rapporten zodat u kunt volgen wanneer gebruikers het systeem openen. U ontvangt ook meldingen om u te waarschuwen over misbruik. 

### <a name="implement-self-service-group-management"></a>Self-service groepsbeheer implementeren

Azure AD biedt de mogelijkheid om toegang tot bronnen met behulp van beveiligingsgroepen en Office 365-groepen te beheren. Deze groepen kunnen worden beheerd door eigenaren van groepen in plaats van IT-beheerders. Ook wel [selfservicegroepsbeheer](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), deze functie kan groepseigenaren wie een beheerdersrol maken en beheren van groepen zonder afhankelijk te zijn beheerders om hun aanvragen te verwerken niet is toegewezen.

### <a name="implement-azure-ad-access-reviews"></a>Implementeer Azure AD-toegangsbeoordelingen

Met [Azure AD-toegangsbeoordelingen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), kunt u lidmaatschap van groepen, toegang tot bedrijfstoepassingen, beheren en toewijzingen van bevoorrechte rollen om ervoor te zorgen dat u een norm voor informatiebeveiliging die geeft gebruikers geen toegang voor onderhouden perioden met een uitgebreid tijd wanneer ze deze niet nodig.

## <a name="summary"></a>Overzicht

Er zijn veel aspecten van een beveiligde infrastructuur voor identiteiten, maar deze controlelijst vijf stap helpt u snel een veiligere en veilige identiteitsinfrastructuur uitvoeren:

* Optimaliseer gebruik van uw referenties.
* Verminder de oppervlakte van de aanval.
* Automatiseer bedreigingen te kunnen reageren.
* Vergroot uw kennis van controleren en bewaken.
* Meer voorspelbare en volledig door eindgebruikers beveiliging inschakelen met zelfhulpinformatie.

We waarderen hoe ernstig u Identiteitbeveiliging en hopen dat dit document is een handig overzicht voor een beter te beveiligen voor uw organisatie.

## <a name="next-steps"></a>Volgende stappen
Als u hulp nodig bij het plannen en implementeren van de aanbevelingen, raadpleegt u de [projectplannen implementatie van Azure AD](http://aka.ms/deploymentplans) voor hulp.
