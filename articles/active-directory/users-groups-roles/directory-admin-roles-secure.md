---
title: Aanbevolen procedures voor beveiliging van beheerderstoegang - Azure Active Directory | Microsoft Docs
description: Zorg ervoor dat de administratieve toegang en beheer accounts van uw organisatie beveiligd zijn. Voor het systeemarchitecten en IT-professionals die Azure AD configureren Azure en Microsoft Online Services.
services: active-directory
keywords: ''
author: curtand
manager: mtillman
ms.author: curtand
ms.date: 03/18/2019
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.custom: it-pro
ms.reviewer: martincoetzer; MarkMorow
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81d09978c3333a5b76c09f8c7dac85998d342f03
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287227"
---
# <a name="securing-privileged-access-for-hybrid-and-cloud-deployments-in-azure-ad"></a>Beveiligen van bevoegde toegang voor cloud- en hybride implementaties in Azure AD

De beveiliging van de meeste of alle bedrijfsassets in de moderne organisatie, is afhankelijk van de integriteit van de bevoegde accounts waarmee IT-systemen worden beheerd. Kwaadwillende actoren vaak met inbegrip van cyberaanvallers richten beheerdersaccounts en andere elementen van bevoegde toegang proberen te snel toegang krijgen tot gevoelige gegevens en systemen met behulp van referentie diefstal aanvallen. Zijn de gezamenlijke verantwoordelijkheden van de cloudserviceprovider en de klant voor cloud services, voorkomen en de reactie. Zie voor meer informatie over de nieuwste bedreigingen van eindpunten en de cloud, de [Microsoft Security Intelligence Report](https://www.microsoft.com/security/operations/security-intelligence-report). In dit artikel kunt u bij ontwikkelen van een schema voor het sluiten van de tussenruimte tussen uw huidige plannen en de richtlijnen die hier worden beschreven.

> [!NOTE]
> Microsoft streeft naar het hoogste niveau van vertrouwen, transparantie, conformiteit met de normen en naleving van regelgeving. Meer informatie over hoe het team van Microsoft wereldwijd reageren op incidenten vermindert de gevolgen van aanvallen op cloudservices en hoe de beveiliging is ingebouwd in zakelijke Microsoft-producten en cloudservices op [Microsoft Trust Center - beveiliging](https://www.microsoft.com/trustcenter/security)en Microsoft naleving doelen op [Microsoft Trust Center - naleving](https://www.microsoft.com/trustcenter/compliance).

<!--## Risk management, incident response, and recovery preparation

A cyber-attack, if successful, can shut down operations not just for a few hours, but in some cases for days or even weeks. The collateral damage, such as legal ramifications, information leaks, and media coverage, could potentially continue for years. To ensure effective company-wide risk containment, cybersecurity and IT pros must align their response and recovery processes. To reduce the risk of business disruption due to a cyber-attack, industry experts recommend you do the following:

* As part of your risk management operations, establish a crisis management team for your organization that is responsible for managing all types of business disruptions.

* Compare your current risk mitigations, incident response, and recovery plan with industry best practices for managing a business disruption before, during, and after a cyber-attack.

* Develop and implement a roadmap for closing the gaps between your current plans and the best practices described in this document.


## Securing privileged access for hybrid and cloud deployments

does the article really start here?-->
Voor de meeste organisaties is de beveiliging van bedrijfsassets afhankelijk van de integriteit van de bevoegde accounts waarmee IT-systemen worden beheerd. Cyberaanvallers richten zich richten op bevoegde toegang tot infrastructuursystemen (zoals Active Directory en Azure Active Directory) toegang te krijgen tot gevoelige gegevens van een organisatie. 

Traditionele methoden die zich richten op het beveiligen van de in- en uitgangen punten van een netwerk bevinden als de primaire beveiligingsperimeter zijn minder effectief vanwege de toename van het gebruik van SaaS-apps en persoonlijke apparaten op Internet. De natuurlijke vervanging voor de netwerkbeveiligingsperimeter in een complexe moderne onderneming is de besturingselementen voor verificatie en autorisatie in de identiteitslaag van een organisatie.

Bevoegde beheerdersaccounts is nagenoeg controle over deze nieuwe 'beveiligingsperimeter." Het is essentieel voor het beveiligen van bevoegde toegang, ongeacht of de omgeving on-premises, cloud of hybride on-premises is en gehoste services in de cloud. Beheerderstoegang tegen vastberaden aanvallers te beveiligen, moet u een volledige en doordachte benadering voor het isoleren van uw organisatie systemen tegen risico's. 

Wijzigingen in vereist het beveiligen van bevoegde toegang

* Processen, beheerpraktijken en kennisbeheer
* Technische onderdelen zoals hostbeveiliging, accountbeveiliging en identiteitsbeheer

Dit document richt zich voornamelijk op het maken van een schema voor het beveiligen van identiteiten en toegang tot die worden beheerd of gerapporteerd in Azure AD, Microsoft Azure, Office 365 en andere cloudservices. Voor organisaties die beschikken over on-premises administratieve accounts, Zie de richtlijnen voor on-premises en hybride bevoegde toegang beheerd via Active Directory op [beveiligen van bevoegde toegang](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access). 

> [!NOTE] 
> De instructies in dit artikel verwijst voornamelijk bedoeld voor de functies van Azure Active Directory die zijn opgenomen in Azure Active Directory Premium P1 en P2-abonnementen. Azure Active Directory Premium P2 is opgenomen in het EMS E5-suite en Microsoft 365 E5-suite. In deze richtlijnen wordt ervan uitgegaan dat uw organisatie al beschikt over Azure AD Premium P2-licenties aangeschaft voor uw gebruikers. Als u deze licenties hebt, kunnen sommige van de richtlijnen niet van toepassing op uw organisatie. In dit artikel wordt is de term globale beheerder (of de globale beheerder) ook synoniem met een 'bedrijfsbeheerder' of 'tenantbeheerder'.

## <a name="develop-a-roadmap"></a>Ontwikkelen van een schema 

Microsoft raadt aan dat u ontwikkelt en gaat u als een schema volgt voor het beveiligen van bevoegde toegang tegen cyberaanvallen aanvallers. U kunt altijd uw roadmap om te voldoen aan uw bestaande mogelijkheden en de specifieke vereisten binnen uw organisatie aanpassen. Elke fase van het schema moet de duurder en moeilijker voor aanvallers voor aanvallen van bevoegde toegang voor uw on-premises, cloud en hybride activa te verhogen. Microsoft raadt aan de volgende vier roadmap fasen: Deze aanbevolen roadmap schema's de meest efficiënte en de snelste implementaties eerst op basis van de ervaringen van Microsoft met bestrijdingsproces incident en antwoord-implementatie. De tijdlijnen voor dit schema zijn bij benadering.

![Fasen van het schema met tijd regels](./media/directory-admin-roles-secure/roadmap-timeline.png)

* Fase 1 (24 tot 48 uur): Essentiële items die wij u adviseren doen meteen

* Fase 2 (2-4 weken): Beperken van de meest gebruikte aanvalstechnieken

* Fase 3 (1-3 maanden): Zichtbaarheid bouwen en bouw volledig beheer van beheerdersactiviteiten bewerkstelligen

* Fase 4 (zes maanden en hoger): Doorgaan met het ontwikkelen van beveiliging voor het verder beperken van uw platform voor beveiliging

Deze roadmap-framework is ontworpen om optimaal gebruik van Microsoft-technologieën die u mogelijk al hebt geïmplementeerd. U kunt ook profiteren van belangrijke huidige en toekomstige beveiligingstechnologieën en integreren beveiligingsprogramma's van andere leveranciers die u al hebt geïmplementeerd of van plan implementeren te bent. 

## <a name="stage-1-critical-items-that-we-recommend-you-do-right-away"></a>Fase 1: Essentiële items die wij u adviseren doen meteen

![Fase 1 kritieke items eerst doen](./media/directory-admin-roles-secure/stage-one.png)

Fase 1 van het schema is gericht op kritieke taken die zijn snel en eenvoudig te implementeren. Het is raadzaam dat u deze enkele items direct in de eerste 24 tot 48 uur doen om te controleren of een basisniveau van secure bevoorrechte toegang. Deze fase van het beveiligingsschema voor bevoegde toegang beveiligd bevat de volgende acties:

### <a name="general-preparation"></a>Algemene voorbereiden

#### <a name="turn-on-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management inschakelen

Als u Azure AD Privileged Identity Management (PIM) nog niet hebt ingeschakeld, doen in uw productietenant. Nadat u Privileged Identity Management inschakelen, ontvangt u de melding e-mailberichten voor bevoegde toegang rol wijzigingen. Deze meldingen bieden vroegtijdig waarschuwen wanneer andere gebruikers worden toegevoegd aan maximaal bevoorrechte rollen in uw directory.

Azure AD Privileged Identity Management is opgenomen in Azure AD Premium P2- of EMS E5. Deze oplossingen kunnen u de toegang tot toepassingen en bronnen beveiligen via de on-premises omgeving en naar de cloud. Als u niet al Azure AD Premium P2- of EMS E5 hebt en wilt evalueren meer van de functies die in dit schema wordt verwezen, zich aanmelden voor de [Enterprise Mobility + Security gratis proefversie van 90 dagen](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial). Gebruik deze proefversies licentie om Azure AD Privileged Identity Management en Azure AD Identity Protection, voor het bewaken van activiteiten met behulp van Azure AD, geavanceerde rapportage van beveiliging, controle en waarschuwingen.

Nadat u hebt ingeschakeld op de Azure AD Privileged Identity Management:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met een account dat een globale beheerder van uw productietenant.

2. Schakel de tenant waar u wilt gebruiken van Privileged Identity Management, selecteer uw gebruikersnaam in de rechterbovenhoek van de Azure-portal.

3. Selecteer **alle services** en filter de lijst voor **Azure AD Privileged Identity Management**.

4. Open Privileged Identity Management van de **alle services** lijst en vastmaken aan uw dashboard.

De eerste persoon die het gebruik van Azure AD Privileged Identity Management in uw tenant wordt toegewezen aan de **beveiligingsbeheerder** en **beheerder met bevoorrechte rol** rollen in de tenant. Alleen beheerders met bevoorrechte rol kunnen de Azure AD directory-roltoewijzingen van gebruikers beheren. Na het toevoegen van Azure AD Privileged Identity Management, worden u ook de beveiligingswizard die u helpt bij de eerste ervaring van detectie en toewijzing weergegeven. Zonder aanvullende wijzigingen op dit moment kunt u de wizard afsluiten. 

#### <a name="identify-and-categorize-accounts-that-are-in-highly-privileged-roles"></a>Identificeren en categoriseren accounts in maximaal bevoorrechte rollen 

Na het inschakelen van Azure AD Privileged Identity Management, bekijk de gebruikers die zich in de directory-rollen globale beheerder, beheerder met bevoorrechte rol, beheerder van Exchange Online en SharePoint Online-beheerder. Als u geen Azure AD PIM in uw tenant, kunt u de [PowerShell API](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0). Starten met de rol globale beheerder, omdat deze rol generiek is: een gebruiker aan wie deze beheerdersrol is toegewezen, heeft dezelfde machtigingen voor alle cloud-services waarvoor uw organisatie is geabonneerd, ongeacht of ze deze rol in de Microsoft 365 bent toegewezen -beheercentrum, de Azure portal of met behulp van de Azure AD-module voor Microsoft PowerShell. 

Verwijder alle accounts die niet meer nodig zijn in deze rollen. Categoriseren vervolgens de resterende accounts die zijn toegewezen aan beheerdersrollen:

* Afzonderlijk toegewezen aan gebruikers met beheerdersrechten en kan ook worden gebruikt voor niet-administratieve doeleinden (bijvoorbeeld persoonlijke e-mailadres)
* Afzonderlijk toegewezen aan gebruikers met beheerdersrechten en aangewezen voor administratieve doeleinden alleen
* Gedeeld door meerdere gebruikers
* Voor break om toegang in noodgevallen scenario 's
* Voor geautomatiseerde scripts
* Voor externe gebruikers

#### <a name="define-at-least-two-emergency-access-accounts"></a>Ten minste twee accounts voor toegang in noodgevallen definiëren 

Zorg ervoor dat u in een situatie waarbij ze kunnen worden per ongeluk vergrendeld buiten het beheer van uw Azure AD-tenant vanwege onvermogen Meld u aan of het activeren van een bestaande afzonderlijke gebruiker-account als een beheerder niet ophalen. Bijvoorbeeld, als de organisatie is gefedereerd met een on-premises id-provider, die de id-provider mogelijk niet beschikbaar, zodat gebruikers zich niet in de on-premises. U kunt de gevolgen van het per ongeluk gebrek aan administratieve toegang beperken door twee of meer accounts voor toegang in noodgevallen zijn opgeslagen in uw tenant.

Accounts voor toegang in noodgevallen kunnen organisaties bevoorrechte toegang beperken in een bestaande Azure Active Directory-omgeving. Deze accounts over uitgebreide beheerdersmogelijkheden en niet zijn toegewezen aan specifieke personen. Accounts voor toegang in noodgevallen zijn beperkt tot noodgevallen voor "verbreken om" scenario's waarbij normale beheerdersaccounts kunnen niet worden gebruikt. Organisaties moeten ervoor zorgen voor het doel van het beheren van en het gebruik van de noodgevallen account beperken tot alleen die tijd die nodig is.

Evalueer de accounts die toegewezen of die in aanmerking komen voor de rol globale beheerder zijn. Als u een niet ziet heeft alleen in de cloud gebruikersaccounts via de *. onmicrosoft.com-domein (die bestemd zijn voor toegang in noodgevallen "verbreken om"), maken ze. Zie voor meer informatie, [beheerdersaccounts voor Noodtoegang beheren in Azure AD](directory-emergency-access.md).

#### <a name="turn-on-multi-factor-authentication-and-register-all-other-highly-privileged-single-user-non-federated-admin-accounts"></a>Meervoudige verificatie inschakelen en de registratie van alle andere veel bevoegdheden één gebruiker niet-gefedereerde administratoraccounts

Azure multi-factor Authentication (MFA) vereisen bij aanmelding voor alle afzonderlijke gebruikers die permanent zijn toegewezen aan een of meer van de Azure AD-beheerdersrollen: Globale beheerder, beheerder met bevoorrechte rol, beheerder van Exchange Online en SharePoint Online-beheerder. De handleiding gebruiken om in te schakelen [multi-factor Authentication (MFA) voor uw beheerdersaccounts](../authentication/howto-mfa-userstates.md) en zorg ervoor dat alle gebruikers hebt geregistreerd bij [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup). Meer informatie vindt u in stap 2 en 3 van de handleiding [toegang tot gegevens en services in Office 365 beveiligen](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e). 

## <a name="stage-2-mitigate-the-most-frequently-used-attack-techniques"></a>Fase 2: Beperken van de meest gebruikte aanvalstechnieken

![Fase 2 Mitigate veelgebruikte aanvallen](./media/directory-admin-roles-secure/stage-two.png)

Fase 2 van de roadmap richt zich op het vaak de meest beperkende aanvalstechnieken waarbij referenties worden gestolen en misbruikt gebruikt en kan worden geïmplementeerd in ongeveer 2 tot 4 weken. Deze fase van het beveiligingsschema voor bevoegde toegang beveiligd bevat de volgende acties.

### <a name="general-preparation"></a>Algemene voorbereiden

#### <a name="conduct-an-inventory-of-services-owners-and-admins"></a>Uitvoeren van een inventaris van services, eigenaars en beheerders

Met de toename in bring-your-own-device (BYOD) en werk van startpagina beleid en de groei van draadloze connectiviteit in bedrijven is het essentieel dat u die verbinding met uw netwerk controleren. Een effectieve beveiligingscontrole blijkt vaak apparaten, toepassingen en programma's die worden uitgevoerd op uw netwerk die niet worden ondersteund door IT, en daarom mogelijk niet beveiligd. Zie voor meer informatie, [Azure beveiligingsbeheer en overzicht van prestatiebewaking](../../security/security-management-and-monitoring-overview.md). Zorg ervoor dat u alle van de volgende taken in uw inventarisproces opneemt. 

* Identificeer de gebruikers die hebben beheerdersrollen en -services waar ze kunnen beheren.
* Azure AD PIM gebruiken om erachter te komen welke gebruikers in uw organisatie beheerderstoegang tot Azure AD hebben, met inbegrip van aanvullende functies dan die in fase 1 worden weergegeven.
* Buiten de rollen gedefinieerd in Azure AD, bevat Office 365 een reeks beheerdersrollen die u aan gebruikers in uw organisatie toewijzen kunt. Elke beheerdersrol toegewezen aan algemene zakelijke functies, en geeft personen in uw Organisatiemachtigingen voor het uitvoeren van bepaalde taken de [Microsoft 365-beheercentrum](https://admin.microsoft.com). Gebruik het Microsoft 365-beheercentrum om erachter te komen welke gebruikers in uw organisatie beheerderstoegang tot Office 365 hebben, onder andere via de rollen niet worden beheerd in Azure AD. Zie voor meer informatie, [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) en [aanbevolen beveiligingsprocedures voor Office 365](https://support.office.com/article/Security-best-practices-for-Office-365-9295e396-e53d-49b9-ae9b-0b5828cdedc3).
* De inventarisatie uitvoeren in andere services die uw organisatie wordt gebruikt, zoals Azure, Intune of Dynamics 365.
* Zorg ervoor dat uw beheerdersaccounts (accounts die worden gebruikt voor beheerdoeleinden, niet alleen dagelijkse gebruikersaccounts) hebt die is gekoppeld aan deze e-mailadressen werken en voor Azure MFA hebt geregistreerd of MFA on-premises gebruiken.
* Gebruikers vragen om hun zakelijke rechtvaardiging voor beheerderstoegang.
* Beheerderstoegang voor deze personen en services die niet nodig verwijderen.

#### <a name="identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts"></a>Microsoft-accounts in een beheerderrol die moet worden overgeschakeld naar de werk- of schoolaccount identificeren

Soms de initiële globale beheerders voor een organisatie hun bestaande referenties van de Microsoft-account gebruiken wanneer ze begonnen met het gebruik van Azure AD. Deze Microsoft-accounts moeten worden vervangen door afzonderlijke accounts voor cloud-gebaseerde of gesynchroniseerd. 

#### <a name="ensure-separate-user-accounts-and-mail-forwarding-for-global-administrator-accounts"></a>Zorg ervoor dat afzonderlijke gebruikersaccounts en doorsturen voor accounts van globale beheerders van e-mail 

Global administrator-accounts moeten geen persoonlijke e-mailadressen, zoals persoonlijke e-mailaccounts regelmatig phished cyber aanvallers worden. Om te scheiden van internetrisico (phishing-aanvallen, onbedoeld surfen) van beheerdersbevoegdheden, maakt u specifieke accounts voor elke gebruiker met beheerdersbevoegdheden. 

Als u hebt nog niet gedaan, maakt u afzonderlijke accounts voor gebruikers om uit te voeren taken van de globale beheerder, om te controleren of ze niet per ongeluk openen van e-mailberichten of programma's die zijn gekoppeld aan hun beheerdersaccounts worden uitgevoerd. Zorg ervoor dat deze accounts hebben hun e-mailbericht doorgestuurd naar een werkende postvak.  

#### <a name="ensure-the-passwords-of-administrative-accounts-have-recently-changed"></a>Zorg ervoor dat de wachtwoorden van beheerdersaccounts onlangs hebt gewijzigd

Zorg ervoor dat alle gebruikers zijn aangemeld bij hun administratoraccounts en hun wachtwoord is gewijzigd ten minste één keer in de afgelopen 90 dagen. Zorg er ook voor dat alle gedeelde accounts waarin meerdere gebruikers het wachtwoord weten had u hun wachtwoord onlangs hebt gewijzigd.

#### <a name="turn-on-password-hash-synchronization"></a>Wachtwoord-hashsynchronisatie inschakelen

Wachtwoord-hashsynchronisatie is een functie die wordt gebruikt om te synchroniseren-hashes van gebruiker wachtwoord-hashes van een on-premises Active Directory-exemplaar naar een cloud-gebaseerde Azure AD-exemplaar. Zelfs dat als u besluit het gebruik van Federatie met Active Directory Federation Services (AD FS) of andere id-providers, kunt u eventueel instellen van wachtwoord-hashsynchronisatie als een back-up in geval uw on-premises infrastructuur, zoals AD of AD FS-servers mislukt of wordt tijdelijk niet beschikbaar. Hierdoor kunnen gebruikers zich aanmeldt bij de service met behulp van het wachtwoord dat ze gebruiken voor het aanmelden bij hun on-premises AD-exemplaar. Bovendien kan Identity Protection voor het detecteren van verdachte referenties door het vergelijken van de wachtwoord-hashes met wachtwoorden die bekend is dat inbreuk worden gepleegd, als een gebruiker leunt op hun e-mailadres en wachtwoord op andere services die niet is verbonden met Azure AD.  Zie voor meer informatie, [implementeren u wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie](../hybrid/how-to-connect-password-hash-synchronization.md).

#### <a name="require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users"></a>Multi-factor authentication (MFA) vereist voor gebruikers in alle bevoorrechte rollen, evenals de beschikbaar gemaakte gebruikers

Azure AD wordt aanbevolen dat u multi-factor authentication (MFA) voor al uw gebruikers, beheerders en alle andere gebruikers die een aanzienlijke gevolgen hebben zou als het account is aangetast (bijvoorbeeld financiële officers) vereist. Dit vermindert het risico van een aanval vanwege een wachtwoord is verdacht.

Schakel in:

* [MFA-beleid voor voorwaardelijke toegang met](../authentication/howto-mfa-getstarted.md) voor alle gebruikers in uw organisatie.

Als u Windows Hello voor bedrijven, kan de MFA-vereiste worden voldaan met behulp van het Windows Hello-teken in ervaring. Zie voor meer informatie, [Windows Hello](https://docs.microsoft.com/windows/uwp/security/microsoft-passport). 

#### <a name="configure-identity-protection"></a>Identity Protection configureren 

Azure AD Identity Protection is een algoritme op basis van controle en rapportage die u gebruiken kunt voor het detecteren van mogelijke beveiligingsproblemen die betrekking hebben op de identiteiten van uw organisatie. U kunt automatische antwoorden om deze gedetecteerde verdachte activiteiten te configureren en onderneem gepaste actie op te lossen. Zie voor meer informatie, [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

#### <a name="obtain-your-office-365-secure-score-if-using-office-365"></a>Uw Office 365 beveiligen Score ophalen (als u Office 365)

Beveilig Score zoekt uit welke Office 365-services u gebruikt (zoals OneDrive, SharePoint en Exchange) en vervolgens kijkt naar de instellingen en de activiteiten en vergelijkt ze met een basislijn voor het tot stand gebracht door Microsoft. U krijgt een score op basis van hoe uitgelijnde u met aanbevolen procedures voor beveiliging bent. Iedereen met beheerdersmachtigingen (globale beheerder of een aangepaste-beheerdersrol) voor een Office 365 Business Premium of Enterprise-abonnement hebben toegang tot de Score beveiligen op [ https://securescore.office.com ](https://securescore.office.com/).

#### <a name="review-the-office-365-security-and-compliance-guidance-if-using-office-365"></a>Bekijk de Office 365-beveiligings- en -richtlijnen (als u Office 365)

De [plan voor beveiliging en naleving](https://support.office.com/article/Plan-for-security-and-compliance-in-Office-365-dc4f704c-6fcc-4cab-9a02-95a824e4fb57) geeft een overzicht van de werkwijze voor hoe Office 365-klant moet configureren van het Office 365 en gebruikmaken van andere mogelijkheden van EMS. Klik, revisie 3-6 van hoe u stappen [toegang tot gegevens en services in Office 365 beveiligen](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e) en de handleiding voor informatie over het [beveiliging en naleving in Office 365 controleren](https://support.office.com/article/Monitor-security-and-compliance-in-Office-365-b62f1722-fd39-44eb-8361-da61d21509b6).

#### <a name="configure-office-365-activity-monitoring-if-using-office-365"></a>Office 365 Activiteitenbewaking te configureren (als u Office 365)

U kunt controleren hoe mensen in uw organisatie zijn Office 365-services, zodat u kunt gebruikers die een Administrator-account hebben en die mogelijk niet Office 365 toegang nodig omdat u niet aanmelden bij de portals identificeren. Zie voor meer informatie, [aanmeldactiviteitenrapporten in de Microsoft 365-beheercentrum](https://support.office.com/article/Activity-Reports-in-the-Office-365-admin-center-0d6dfb17-8582-4172-a9a9-aed798150263).

#### <a name="establish-incidentemergency-response-plan-owners"></a>Eigenaren van incident/emergency response-plan maken

Uitvoeren van reacties op incidenten effectief is een complexe onderneming. Daarom vereist tot stand brengen van een geslaagde respons op incidenten mogelijkheid aanzienlijk planning en resources. Het is essentieel dat u voortdurend voor cyberaanvallen controleren en procedures voor het afhandelen van incidenten prioriteren vast te stellen. Effectieve methoden van het verzamelen, analyseren en rapporteren van gegevens zijn essentieel voor relaties maken en voor communicatie met andere interne groepen en eigenaren van plan bent. Zie voor meer informatie, [Microsoft Security Response Center](https://technet.microsoft.com/security/dn440717). 

#### <a name="secure-on-premises-privileged-administrative-accounts-if-not-already-done"></a>Veilige on-premises bevoegde beheerdersaccounts, als dat nog niet gedaan

Als uw Azure Active Directory-tenant is gesynchroniseerd met on-premises Active Directory, volgt u de instructies in [bevoorrechte toegang beveiligingsschema](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): Fase 1. Dit omvat het maken van afzonderlijke beheerdersaccounts voor gebruikers die nodig zijn voor het uitvoeren van beheertaken voor on-premises implementatie van Privileged Access Workstations voor Active Directory-beheerders en het maken van unieke lokale beheerderswachtwoorden voor werkstations en -servers.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Aanvullende stappen voor organisaties toegang tot Azure beheren

#### <a name="complete-an-inventory-of-subscriptions"></a>Voer een inventarisatie maken van abonnementen

De Enterprise portal en Azure portal gebruiken om te identificeren van de abonnementen in uw organisatie die als host fungeren van productie-Apps.

#### <a name="remove-microsoft-accounts-from-admin-roles"></a>Microsoft-accounts verwijderen van-beheerdersrollen

Microsoft-accounts van andere programma's, zoals Xbox Live en Outlook moeten niet worden gebruikt als administrator-accounts voor organisatie-abonnementen. Beheerstatus van alle Microsoft-accounts verwijderen en vervangen met Azure Active Directory (bijvoorbeeld chris@contoso.com) werk- of schoolaccount.

#### <a name="monitor-azure-activity"></a>Azure-activiteit controleren

De Azure-activiteitenlogboek biedt een geschiedenis van gebeurtenissen op abonnementsniveau in Azure. Het biedt informatie over wie die zijn gemaakt, bijgewerkt en welke resources hebt verwijderd, en wanneer deze gebeurtenissen opgetreden. Zie voor meer informatie, [controle en meldingen ontvangen over belangrijke acties in uw Azure-abonnement](../../azure-monitor/platform/quick-audit-notify-action-subscription.md).

### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Aanvullende stappen voor organisaties toegang tot andere cloud-apps via Azure AD beheren

#### <a name="configure-conditional-access-policies"></a>Beleid voor voorwaardelijke toegang configureren

Bereid beleid voor voorwaardelijke toegang voor on-premises en cloudtoepassingen. Als u apparaten van gebruikers toegevoegd aan de werkplek hebt, krijgt u meer informatie uit [instellen van on-premises voorwaardelijke toegang met behulp van Azure Active Directory device Registration service](../active-directory-device-registration-on-premises-setup.md).


## <a name="stage-3-build-visibility-and-take-full-control-of-admin-activity"></a>Fase 3: Zichtbaarheid bouwen en uitvoeren van volledige beheer van beheerdersactiviteiten bewerkstelligen

![Fase 3 nemen beheer van beheerdersactiviteiten bewerkstelligen](./media/directory-admin-roles-secure/stage-three.png)

Fase 3 gebaseerd op de oplossingen uit fase 2 en is ontworpen om te worden geïmplementeerd in ongeveer 1-3 maanden. Deze fase van het beveiligingsschema voor bevoegde toegang beveiligd omvat de volgende onderdelen.

### <a name="general-preparation"></a>Algemene voorbereiden

#### <a name="complete-an-access-review-of-users-in-administrator-roles"></a>Een toegangscontrole van gebruikers in beheerdersrollen voltooien

Bevoegde toegang via cloudservices, wat tot een toenemende niet-beheerde platform leiden kan zijn verkrijgen van meer zakelijke gebruikers. Dit omvat gebruikers die globale beheerders voor Office 365, Azure-abonnementbeheerders en gebruikers die beheerderstoegang tot VM's of via de SaaS-apps hebben steeds. Organisaties moeten in plaats daarvan alle werknemers met name beheerders, dagelijkse zakelijke transacties als niet-gemachtigde gebruikers verwerken en beheerdersrechten alleen ondernemen zo nodig hebben. Omdat het aantal gebruikers in beheerdersrollen kan gegroeid sinds de eerste toepassing, controleert de volledige toegang om te identificeren en controleer of elke gebruiker die in aanmerking voor het activeren van beheerdersbevoegdheden. 

Ga als volgt te werk:

* Bepalen welke gebruikers Azure AD-beheerders, inschakelen op aanvraag, just-in-time beheerderstoegang en besturingselementen voor de beveiliging op basis van rollen.
* Converteren van gebruikers die geen duidelijke reden voor uitgebreide beheerderstoegang tot een andere rol hebben (als er geen in aanmerking komende rol verwijderen).

#### <a name="continue-rollout-of-stronger-authentication-for-all-users"></a>Implementatie van betere verificatie voor alle gebruikers gaan 

Vereisen van C-suite leidinggevenden, op hoog niveau managers, kritieke IT en personeel en andere zeer beschikbaar gemaakte gebruikers moderne, sterke verificatie, zoals Azure MFA of Windows Hello. 

#### <a name="use-dedicated-workstations-for-administration-for-azure-ad"></a>Gebruik van speciale werkstations voor het beheer voor Azure AD

Aanvallers kunnen proberen te richten op accounts met bevoegdheden om toegang te krijgen tot de gegevens en systemen van een organisatie, zodat ze de integriteit en de echtheid van gegevens, via schadelijke code die wordt gewijzigd van de programmalogica of de beheerder invoeren van een referentie snoops kunnen verstoren. Privileged Access Workstations (paw's) bieden een speciaal besturingssysteem voor gevoelige taken die is beschermd tegen aanvallen via Internet en dreigingsvectoren. Deze gevoelige taken en accounts te scheiden van de dagelijkse gebruik van werkstations en apparaten biedt zeer goede beveiliging tegen phishing-aanvallen, toepassing en OS beveiligingsproblemen, verschillende imitatie-aanvallen en referentie diefstal aanvallen zoals toetsaanslag logboekregistratie, Pass-the-Hash en Pass-The-Ticket. Privileged access workstations implementeert, kunt u het risico dat beheerders beheerdersreferenties, behalve invoeren in een desktop-omgeving die is is geworden beperken. Zie voor meer informatie, [Privileged Access Workstations](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

#### <a name="review-national-institute-of-standards-and-technology-recommendations-for-handling-incidents"></a>Bekijk National Institute of Standards and Technology aanbevelingen voor het afhandelen van incidenten 

Het National Institute of Standards and van Technology (NIST) bevat richtlijnen voor het afhandelen van incidenten, met name voor het analyseren van gegevens met betrekking tot incident en het bepalen van de juiste reactie naar elk incident. Zie voor meer informatie, [(NIST) de Computer Security Incident Handling Guide (SP 800-61, revisie 2)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf).

#### <a name="implement-privileged-identity-management-pim-for-jit-to-additional-administrative-roles"></a>Privileged Identity Management (PIM) implementeren voor JIT aan aanvullende beheerdersrollen

Voor Azure Active Directory, gebruikt u [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md) mogelijkheid. Gedurende beperkte tijd activering van bevoorrechte rollen werkt doordat u:

* De beheerdersmachtigingen om uit te voeren van een specifieke taak activeren
* Afdwingen van MFA tijdens de activering
* Waarschuwingen gebruiken om te beheerders informeren over out-of-band-wijzigingen
* Gebruikers in staat te bewaren van bepaalde bevoegdheden voor een vooraf ingestelde tijd wordt opgelost
* Beveiligingsbeheerders alle bevoegde identiteiten detecteren, auditrapporten weergeven en maken van toegangsbeoordelingen voor het identificeren van elke gebruiker die in aanmerking voor het activeren van beheerdersbevoegdheden toestaan

Als u al Azure AD Privileged Identity Management, pas deze tijdsbestek voor tijdsgebonden bevoegdheden zo nodig (bijvoorbeeld onderhoudsvensters).

#### <a name="determine-exposure-to-password-based-sign-in-protocols-if-using-exchange-online"></a>De blootstelling aan op basis van wachtwoorden aanmelden protocollen bepalen (als u Exchange Online)

In het verleden uitgegaan protocollen dat de combinaties van gebruikersnaam en wachtwoord zijn ingesloten in apparaten, e-mailaccounts en telefoons. Maar nu met het risico voor cyberaanvallen in de cloud, raden we aan identificeren van elke mogelijke gebruiker door wie, als hun referenties zijn aangetast, kan catastrofale aan de organisatie worden en kunnen uitsluiten aanmelden bij hun e-mail via gebruikersnaam / het wachtwoord door het implementeren van sterke verificatievereisten en voorwaardelijke toegang. U kunt blokkeren [verouderde verificatie door middel van voorwaardelijke toegang](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/block-legacy-authentication). Controleer de details op [het blokkeren van basisverificatie](https://docs.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/disable-basic-authentication-in-exchange-online) via Exchnage online. 

#### <a name="complete-a-roles-review-assessment-for-office-365-roles-if-using-office-365"></a>Voer een evaluatie van de beoordeling rollen voor Office 365-rollen (als u Office 365)

Bepaal of alle beheerders gebruikers zich in de juiste rollen (verwijderen en opnieuw toewijzen op basis van deze evaluatie).

#### <a name="review-the-security-incident-management-approach-used-in-office-365-and-compare-with-your-own-organization"></a>De security incident management-benadering die wordt gebruikt in Office 365 bekijken en vergelijken met uw eigen organisatie

U kunt dit rapport uit downloaden [Security Incident Management in Microsoft Office 365](https://www.microsoft.com/download/details.aspx?id=54302).

#### <a name="continue-to-secure-on-premises-privileged-administrative-accounts"></a>Doorgaan met het beveiligen van on-premises bevoegde beheerdersaccounts

Als uw Azure Active Directory is verbonden met on-premises Active Directory, volgt u de instructies in de [bevoorrechte toegang beveiligingsschema](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): Fase 2. Dit omvat het implementeren van Privileged Access Workstations voor alle beheerders, MFA vereisen, net voldoende beheer gebruiken voor DC-onderhoud, de kwetsbaarheid voor aanvallen van domeinen te verlagen, ATA-implementatie voor de aanvalsdetectie.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Aanvullende stappen voor organisaties toegang tot Azure beheren

#### <a name="establish-integrated-monitoring"></a>Geïntegreerde controle instellen

De [Azure Security Center](../../security-center/security-center-intro.md) biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bij het detecteren van bedreigingen die mogelijk anders onopgemerkt en werkt met een uitgebreid ecosysteem van beveiliging oplossingen.

#### <a name="inventory-your-privileged-accounts-within-hosted-virtual-machines"></a>Inventariseer uw beschermde accounts in de gehoste virtuele Machines

In de meeste gevallen moet u geen gebruikers onbeperkte machtigingen geven voor alle Azure-abonnementen of resources. Azure AD-beheerdersrollen kunt u taken scheiden binnen uw organisatie en alleen de mate van toegang verlenen aan gebruikers die moeten specifieke taken uitvoeren. Azure AD-beheerdersrollen bijvoorbeeld gebruiken om één beheerder alleen de virtuele machines in een abonnement beheren terwijl een andere SQL-databases binnen hetzelfde abonnement kunt beheren. Zie voor meer informatie, [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../../role-based-access-control/overview.md).

#### <a name="implement-pim-for-azure-ad-administrator-roles"></a>PIM voor Azure AD-beheerdersrollen implementeren

Privileged identity Management gebruiken met Azure AD-beheerdersrollen, beheren en controleren van toegang tot Azure-resources. Met behulp van PIM beschermt bevoegde accounts tegen cyberaanvallen door de blootstellingstijd van bevoegdheden te verlagen en vergroot uw inzicht in het gebruik ervan door middel van rapporten en waarschuwingen. Zie voor meer informatie, [RBAC beheren de toegang tot Azure-resources met Privileged Identity Management](../../role-based-access-control/pim-azure-resource.md).

#### <a name="use-azure-log-integrations-to-send-relevant-azure-logs-to-your-siem-systems"></a>Integratie van Azure log gebruiken voor het verzenden van relevante Azure-logboeken naar uw SIEM-systemen 

Azure-Logboekintegratie kunt u onbewerkte logboeken van uw Azure-resources van uw organisatie bestaande Security Information en Event Management (SIEM)-systemen integreren. [Azure-Logboekintegratie](../../security/security-azure-log-integration-overview.md) Windows-gebeurtenissen verzamelt van Windows Event Viewer-logboeken en Azure-resources van Azure-activiteitenlogboeken, waarschuwingen van Azure Security Center en Azure diagnostische logboeken. 


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Aanvullende stappen voor organisaties toegang tot andere cloud-apps via Azure AD beheren

#### <a name="implement-user-provisioning-for-connected-apps"></a>Gebruikers inrichten voor verbonden apps implementeren

Azure AD kunt u het maken, onderhoud en verwijderen van gebruikers-id's in cloudtoepassingen (SaaS), zoals Dropbox, Salesforce, ServiceNow automatiseren, enzovoort. Zie voor meer informatie, [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure AD](../manage-apps/user-provisioning.md).

#### <a name="integrate-information-protection"></a>Information protection integreren

MCAS kunt u bestanden onderzoeken en instellen van beleid op basis van Azure Information Protection-classificatielabels, zorgt voor meer zichtbaarheid en controle over uw gegevens in de cloud. Scannen en classificeren van bestanden in de cloud en Azure information protection-labels van toepassing. Zie voor meer informatie, [integratie van Azure Information Protection](https://docs.microsoft.com/cloud-app-security/azip-integration).

#### <a name="configure-conditional-access"></a>Voorwaardelijke toegang configureren

Beleid voor voorwaardelijke toegang op basis van een groep, de locatie en de gevoeligheid van een toepassing voor [SaaS-apps](https://azure.microsoft.com/overview/what-is-saas/) en Azure AD verbonden apps. 

#### <a name="monitor-activity-in-connected-cloud-apps"></a>Migratieactiviteiten in de gekoppelde cloud-apps bewaken

Om ervoor te zorgen voor toegang van gebruikers wordt beveiligd in ook verbonden toepassingen, het is raadzaam dat u gebruikmaken van [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). Dit beveiligt de toegang tot enterprise cloud-apps, naast het beveiligen van uw beheerdersaccounts, doordat u:

* Zichtbaarheid en beheer van cloud-apps uitbreiden
* Beleidsregels voor toegang, activiteiten en het delen van gegevens maken
* Automatisch riskante activiteiten, abnormaal gedrag en bedreigingen identificeren
* Voorkomen dat gegevens worden gelekt
* Minimaliseren van risico's en geautomatiseerde threat preventie en afdwingen van beleid

De Cloud App Security SIEM-agent kan Cloud App Security worden geïntegreerd met uw SIEM-server om in te schakelen gecentraliseerde bewaking van Office 365-waarschuwingen en activiteiten. Deze wordt uitgevoerd op uw server en haalt waarschuwingen en activiteiten van Cloud App Security en verzendt deze naar de SIEM-server. Zie voor meer informatie, [SIEM-integratie](https://docs.microsoft.com/cloud-app-security/siem).

## <a name="stage-4-continue-building-defenses-to-a-more-proactive-security-posture"></a>Fase 4: Doorgaan met het ontwikkelen van bescherming voor een meer proactieve beveiliging proactievere

![Fase 4 een proactieve beveiligingspostuur vaststellen](./media/directory-admin-roles-secure/stage-four.png)

Fase 4 van het schema borduurt voort op de zichtbaarheid van stap 3 en is ontworpen om te worden geïmplementeerd in zes maanden en daarbuiten. Voltooien van een roadmap helpt u sterke ontwikkelen met bevoegde toegang bescherming tegen potentiële aanvallen die tegenwoordig op dit moment bekend en beschikbaar zijn. Helaas beveiligingsrisico's continu veranderen en wilt, raden wij aan dat u als een continu proces op de kosten verhogen en verlagen van het slagingspercentage van aanvallers die gericht is op uw omgeving bekijken.

Het beveiligen van bevoegde toegang is een kritieke eerste stap voor het tot stand brengen van veiligheidsgaranties voor zakelijke activa in een moderne organisatie, maar het is niet het enige onderdeel van een volledige beveiligingsprogramma elementen zoals beleid, bewerkingen, informatie beveiliging, servers, toepassingen, pc's, apparaten, cloudinfrastructuur en andere onderdelen bieden continue goed beveiligingsprogramma. 

Naast het beheren van uw accounts bevoegde toegang, raden we dat u de volgende regelmatig controleren:

* Zorg ervoor dat beheerders hun dagelijkse werkzaamheden als niet-gemachtigde gebruikers doen.
* Alleen wanneer dat nodig is van bevoegde toegang verlenen en verwijdert u deze later (just-in-time).
* Behouden en controleert u controle-activiteiten met betrekking tot beschermde accounts.

Zie voor meer informatie over het bouwen van een compleet beveiligingsschema [Microsoft IT-architectuur cloudbronnen](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources). Neem contact op met uw Microsoft-vertegenwoordiger of Zie voor meer informatie over het gebruik van Microsoft-services om te helpen met een van deze onderwerpen [kritieke cyber beveiliging ter bescherming van uw onderneming bouwen](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

Deze laatste lopende fase van het beveiligingsschema voor bevoegde toegang beveiligd omvat de volgende onderdelen.

### <a name="general-preparation"></a>Algemene voorbereiden

#### <a name="review-admin-roles-in-azure-active-directory"></a>Beheerdersrollen in Azure Active Directory controleren 

Te bepalen of de huidige ingebouwde Azure AD-beheerdersrollen nog steeds up-to-date zijn en zorg ervoor dat gebruikers zich alleen in de functies en overdrachten die ze nodig hebben voor de bijbehorende machtigingen. Met Azure AD, kunt u afzonderlijke beheerders verschillende functies vervullen aanwijzen. Zie voor meer informatie, [beheerdersrollen toewijzen in Azure Active Directory](directory-assign-admin-roles.md).

#### <a name="review-users-who-have-administration-of-azure-ad-joined-devices"></a>Controleer gebruikers met beheer van Azure AD gekoppelde apparaten

Zie voor meer informatie, [hoe het configureren van hybride Azure Active Directory apparaten gekoppelde](../device-management-hybrid-azuread-joined-devices-setup.md).

#### <a name="review-members-of-built-in-office-365-admin-roleshttpssupportofficecomarticleabout-office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d"></a>Bekijk de leden van [ingebouwde Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)
Als u Office 365.
‎
#### <a name="validate-incident-response-plan"></a>Plan voor respons op incidenten valideren

Ter verbetering van uw abonnement, raadt Microsoft aan dat u regelmatig valideren dat uw plan werkt zoals verwacht:

* Via uw bestaande roadmap om te zien wat is overgeslagen
* Op basis van de postmortem analyse, aanpassen bestaande of nieuwe aanbevolen procedures worden gedefinieerd
* Zorg ervoor dat uw bijgewerkte reactieplan en aanbevolen procedures worden gedistribueerd binnen uw organisatie


### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Aanvullende stappen voor organisaties toegang tot Azure beheren 

Als u wilt bepalen [eigendom van een Azure-abonnement naar een ander account overdragen](../../billing/billing-subscription-transfer.md).
‎

## <a name="break-glass-what-to-do-in-an-emergency"></a>"Verbreken om": wat te doen in geval van nood

![Accounts voor noodgevallen break glas toegang](./media/directory-admin-roles-secure/emergency.jpeg)

1. Informeer de belangrijkste managers en security officers met relevante informatie over het incident.

2. Controleer uw playbook aanval. 

3. Toegang tot uw combinatie "verbreken om" account gebruikersnaam en wachtwoord aanmelden bij Azure AD. 

4. Hulp krijgen van Microsoft door [openen van een Azure-ondersteuningsaanvraag](../../azure-supportability/how-to-create-azure-support-request.md).

5. Bekijk de [Azure AD-aanmeldingsrapporten](../reports-monitoring/overview-reports.md). Er is mogelijk een vertraging tussen een gebeurtenis plaatsvindt en wanneer deze is opgenomen in het rapport.

6. Voor hybride omgevingen, als federatieve en uw AD FS-server niet beschikbaar is, moet u mogelijk tijdelijk overschakelen van federatieve verificatie hash Wachtwoordsynchronisatie te gebruiken. Hierdoor wordt de domein-Federatie naar beheerde verificatie totdat de AD FS-server beschikbaar.

7. E-mailadres voor bevoegde accounts bewaken.

8. Zorg ervoor dat u back-ups van relevante logboekbestanden voor mogelijke juridisch en forensisch onderzoek opslaat.

Zie voor meer informatie over hoe Microsoft Office 365 beveiligingsincidenten verwerkt [Security Incident Management in Microsoft Office 365](https://aka.ms/Office365SIM).

## <a name="faq-common-questions-we-receive-regarding-securing-privileged-access"></a>FAQ: Veelgestelde vragen die wij over het beveiligen van bevoegde toegang ontvangen  

**V:** Wat moet ik doen als ik alle onderdelen van beveiligde toegang nog niet hebt geïmplementeerd?

**Antwoord:** Ten minste twee break-glas account definiëren, MFA toewijzen aan uw bevoegde beheerdersaccounts en afzonderlijke accounts van globale beheerdersaccounts.

**V:** Wat is het belangrijkste probleem dat u moet eerst worden verholpen na een inbreuk?

**Antwoord:** Zorg ervoor dat u bent de sterkste verificatie vereist voor personen maximaal beschikbaar gemaakt.

**V:** Wat gebeurt er als onze bevoorrechte beheerders zijn uitgeschakeld?

**Antwoord:** Maak een globale beheerder-account dat altijd up-to-date wordt gehouden.

**V:** Wat gebeurt er als er slechts één globale beheerder links en ze kunnen niet worden bereikt? 

**Antwoord:** Gebruik een van uw accounts break om onmiddellijke bevoegde toegang te krijgen.

**V:** Hoe kan ik beheerders beveiligen in mijn organisatie?

**Antwoord:** Hebben beheerders hun dagelijkse zaken als "niet-gemachtigd" standaardgebruikers altijd doen.

**V:** Wat zijn de aanbevolen procedures voor het maken van beheerdersaccounts in Azure AD?

**Antwoord:** Reserve bevoegde toegang voor specifieke beheertaken.

**V:** Welke hulpprogramma's bestaan voor het verminderen van permanente beheerderstoegang?

**Antwoord:** Privileged Identity Management (PIM) en Azure AD-beheerdersrollen.

**V:** Wat is de positie van Microsoft voor het synchroniseren van beheerdersaccounts naar Azure AD?

**Antwoord:** Laag 0-beheerdersaccounts (inclusief accounts, groepen en andere activa die directe of indirecte beheer van de AD-forest, domeinen of domeincontrollers hebt en alle activa) worden gebruikt voor on-premises AD-accounts en zijn doorgaans niet gesynchroniseerd voor Azure AD voor de cloud.

**V:** Hoe houd we beheerders willekeurige beheerderstoegang in de portal worden toegewezen?

**Antwoord:** Niet-gemachtigde accounts gebruiken voor alle gebruikers en de meeste beheerders. Gestart door de ontwikkeling van een footprint van de organisatie om te bepalen welke paar beheerdersaccounts moeten bevoegdheden. En controleer het zojuist gemaakte gebruikers met beheerrechten.

## <a name="next-steps"></a>Volgende stappen

* [Microsoft Trust Center voor de beveiliging van het Product](https://www.microsoft.com/trustcenter/security) – beveiligingsfuncties van Microsoft-producten en services in de cloud

* [Microsoft Trust Center - naleving](https://www.microsoft.com/trustcenter/compliance/complianceofferings) – uitgebreide set compliance-aanbiedingen voor cloudservices van Microsoft

* [Instructies over het uitvoeren van een risicoanalyse](https://www.microsoft.com/trustcenter/guidance/risk-assessment) -vereisten voor beveiliging en naleving voor Microsoft-cloudservices beheren

### <a name="other-microsoft-online-services"></a>Andere Microsoft Online Services

* [Microsoft Intune Security](https://www.microsoft.com/trustcenter/security/intune-security) – Intune biedt beheer van mobiele apparaten, beheer van mobiele toepassingen en beheermogelijkheden voor pc's vanuit de cloud.

* [Microsoft Dynamics 365 security](https://www.microsoft.com/trustcenter/security/dynamics365-security) : Dynamics 365 is de Microsoft cloud-gebaseerde oplossing die verenigt klantrelatiebeheer (CRM) en voor enterprise resource planning (ERP-)-mogelijkheden.
