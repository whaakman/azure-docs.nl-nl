---
title: Aanbevolen procedures voor het beveiligen van beheerderstoegang in Azure AD | Microsoft Docs
description: Zorg ervoor dat uw organisatie toegang en beheer beheerdersaccounts beveiligd zijn. Voor systeemarchitecten en IT-professionals die Azure AD configureren Azure en Microsoft Online Services.
services: active-directory
keywords: ''
author: curtand
ms.author: curtand
ms.date: 03/09/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.custom: it-pro
ms.reviewer: martincoetzer, MarkMorow
ms.openlocfilehash: 9897a3578ad35376bea81b4821dbc2d84526a736
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="securing-privileged-access-for-hybrid-and-cloud-deployments-in-azure-ad"></a>Bevoegde toegang voor hybride en cloud-implementaties beveiligen in Azure AD

De beveiliging van de meeste of alle zakelijke activa in het moderne organisatie is afhankelijk van de integriteit van de bevoegde accounts dat beheren en het beheer van IT-systemen. Schadelijke actoren cyberbeveiliging aanvallers vaak inclusief doel beheerdersaccounts en andere elementen van bevoorrechte toegang proberen te snel toegang te krijgen tot gevoelige gegevens en systemen credential theft aanvallen. Zijn de gemeenschappelijke verantwoordelijkheden van de cloudserviceprovider en de klant voor cloud services, voorkomen en de reactie. Zie voor meer informatie over de nieuwste dreigingen eindpunten en de cloud de [Microsoft Intelligence beveiligingsrapport](https://www.microsoft.com/security/sir/default.aspx). In dit artikel kan helpen bij het ontwikkelen van een routekaart voor het sluiten van de tussenruimte tussen uw huidige plannen en de informatie die hier wordt beschreven.

> [!NOTE] 
> Microsoft hecht veel belang aan het hoogste niveau van de vertrouwensrelatie, transparantie conformiteit van standaarden en regelgeving. Meer informatie over hoe het team van Microsoft globale respons op incidenten vermindert de gevolgen van aanvallen op basis van cloudservices en hoe de beveiliging is ingebouwd in zakelijke Microsoft-producten en cloudservices op [Microsoft Trust Center - beveiliging](https://www.microsoft.com/en-us/trustcenter/security)en Microsoft naleving doelen op [Microsoft Trust Center - naleving](https://www.microsoft.com/en-us/trustcenter/compliance).

<!--## Risk management, incident response, and recovery preparation

A cyber-attack, if successful, can shut down operations not just for a few hours, but in some cases for days or even weeks. The collateral damage, such as legal ramifications, information leaks, and media coverage, could potentially continue for years. To ensure effective company-wide risk containment, cybersecurity and IT pros must align their response and recovery processes. To reduce the risk of business disruption due to a cyber-attack, industry experts recommend you do the following:

* As part of your risk management operations, establish a crisis management team for your organization that is responsible for managing all types of business disruptions.

* Compare your current risk mitigations, incident response, and recovery plan with industry best practices for managing a business disruption before, during, and after a cyber-attack.

* Develop and implement a roadmap for closing the gaps between your current plans and the best practices described in this document.


## Securing privileged access for hybrid and cloud deployments

does the article really start here?-->
Voor de meeste organisaties afhankelijk de beveiliging van bedrijfsmiddelen van de integriteit van de bevoegde accounts dat beheren en het beheer van IT-systemen. Cyberbeveiliging aanvallers zich richten op de uitgebreide toegang tot infrastructuursystemen (zoals Active Directory en Azure Active Directory) toegang te krijgen tot gevoelige gegevens van een organisatie. 

Traditionele methoden die zich richten op het beveiligen van de ingang naar en uitgang uit punten van een netwerk als de primaire beveiliging perimeter zijn minder effectief vanwege de toename van het gebruik van de SaaS-apps en persoonlijke apparaten op Internet. De natuurlijke vervanging voor de netwerkverbinding van de beveiliging in een complexe moderne ondernemingen is de besturingselementen voor verificatie en autorisatie in een organisatie identiteitlaag. 

Bevoorrechte beheerdersaccounts worden effectief toegang hebt tot deze nieuwe 'beveiliging perimeter." Het is essentieel voor het beveiligen van bevoorrechte toegang, ongeacht of de omgeving on-premises, cloud of hybride on-premises is en in de cloud gehoste services. Beheerderstoegang tegen bepaald tegenstanders beveiligen, moet u voor een volledige en doordacht manier te isoleren van uw organisatie systemen tegen de risico's. 

Wijzigingen van vereist bevoegde toegang beveiligen
* Processen, administratieve procedures en kennisbeheer
* Technische onderdelen zoals host beveiliging en bescherming van de account identiteitsbeheer

Dit document richt zich voornamelijk op maken van een routekaart voor het beveiligen van identiteiten en toegang tot die worden beheerd of gerapporteerd in Azure AD, Microsoft Azure, Office 365 en andere cloudservices. Voor organisaties met on-premises beheerdersaccounts, Zie de richtlijnen voor on-premises en hybride bevoegde toegang beheerd vanuit Active Directory op [bevoegde toegang beveiligen](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access). 

> [!NOTE] 
> De instructies in dit artikel verwijst voornamelijk naar functies van Azure Active Directory die zijn opgenomen in het plan van Azure Active Directory Premium P1 en P2. Azure Active Directory Premium-P2 is opgenomen in het EMS E5-suite en Microsoft 365 E5-suite. In deze richtlijnen wordt ervan uitgegaan dat uw organisatie al een Azure AD Premium-P2 licenties voor uw gebruikers. Als er geen licenties, kunnen sommige van de richtlijnen niet van toepassing op uw organisatie. In dit artikel wordt is de term algemeen beheerder (of de globale beheerder) ook synoniem met 'bedrijf administrator' of 'tenantbeheerder'.

## <a name="develop-a-roadmap"></a>Een roadmap ontwikkelen 

Microsoft raadt aan dat u ontwikkelen en volgen van een Wegwijzer voor het beveiligen van bevoorrechte toegang tegen kwaadwillende personen cyberbeveiliging. U kunt uw roadmap om uw bestaande mogelijkheden en de specifieke vereisten binnen uw organisatie altijd aanpassen. Elke fase van het overzicht moet afkomstig van de kosten en moeite voor tegenstanders voor aanvallen van bevoorrechte toegang voor uw on-premises, cloud en hybride activa. Microsoft raadt u aan de volgende vier roadmap fasen: dit aanbevolen roadmap planningen de meest doeltreffende en de snelste implementaties eerst op basis van ervaringen met cyberbeveiliging aanval incident- en implementatie van Microsoft. Voor deze roadmap zijn bij benadering.

![Fasen van het overzicht met regels die tijd](./media/admin-roles-best-practices/roadmap-timeline.png)

* Fase 1 (24-48 uur): essentiële items die wij u adviseren meteen doen

* Fase 2 (2-4 weken): de meest gebruikte aanval technieken beperken

* Fase 3 (1-3 maanden): bouwen zichtbaarheid en volledig beheer van admin-activiteit maken

* Stap 4 (zes maanden en hoger): gaan met het samenstellen van beveiliging voor uw platform beveiliging verder beperken

Dit framework roadmap is ontworpen om optimaal gebruik van Microsoft-technologieën die u al hebt geïmplementeerd. U kunt ook te profiteren van belangrijke huidige en toekomstige beveiligingstechnologieën en integratie beveiligingsfuncties van andere leveranciers die u al hebt geïmplementeerd, of van plan implementeren te bent. 

## <a name="stage-1-critical-items-that-we-recommend-you-do-right-away"></a>Stap 1: Essentiële items die wij u adviseren doen meteen

![Fase 1](./media/admin-roles-best-practices/stage-one.png)

Fase 1 van het overzicht is gericht op essentiële taken die zijn snel en eenvoudig te implementeren. Het is raadzaam dat u deze enkele items meteen binnen de eerste 24-48 uur om ervoor te zorgen basisniveau van beveiligde bevoorrechte toegang uitvoeren. Deze fase van het beveiligde bevoorrechte toegang-overzicht bevat de volgende acties:

### <a name="general-preparation"></a>Algemene voorbereiding

#### <a name="turn-on-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management inschakelen

Als u Azure AD Privileged Identity Management (PIM) al niet hebt ingeschakeld, doen in uw productie-tenant. Nadat u Privileged Identity Management inschakelt, ontvangt u melding e-mailberichten voor bevoorrechte toegang wijzigingen van de rol. Deze meldingen een vroegtijdige waarschuwing wanneer extra gebruikers worden toegevoegd aan maximaal bevoegdheden rollen in uw directory.

Azure AD Privileged Identity Management is in Azure AD Premium P2- of EMS E5 opgenomen. Deze oplossingen kunnen u toegang tot toepassingen en bronnen beveiligen via de on-premises omgeving en in de cloud. Als u nog niet Azure AD Premium P2- of EMS E5 hebt en wilt evalueren meer van de mogelijkheden waarnaar wordt verwezen in het volgende overzicht, zich aanmelden voor de [Enterprise Mobility + Security gratis proefabonnement van 90 dagen](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial). Deze licentie proefversies gebruiken om Azure AD Privileged Identity Management en Azure AD Identity Protection als activiteiten die gebruikmaken van Azure AD met geavanceerde beveiliging rapportage, controle en waarschuwingen wilt controleren.

Nadat u hebt ingeschakeld op Azure AD Privileged Identity Management:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met een account dat een globale beheerder van uw productie-tenant.

2. Schakel de tenant waar u Privileged Identity Management gebruiken, selecteert u uw gebruikersnaam in de rechterbovenhoek van de Azure portal.

3. Selecteer **alle services** en filter de lijst voor **Azure AD Privileged Identity Management**.

4. Open Privileged Identity Management van de **alle services** vermeld en vastmaken aan uw dashboard.

De eerste persoon die het gebruik van Azure AD Privileged Identity Management in uw tenant krijgt automatisch de **beveiligingsbeheerder** en **beheerder met bevoorrechte rol** rollen in de tenant. Alleen bevoorrechte rol beheerders kunnen de Azure AD-directory roltoewijzingen van gebruikers beheren. Na het toevoegen van Azure AD Privileged Identity Management u ook de beveiligingswizard die u bij de initiële detectie en toewijzing ervaring helpt weergegeven. Zonder aanvullende wijzigingen op dit moment kunt u de wizard afsluiten. 

#### <a name="identify-and-categorize-accounts-that-are-in-highly-privileged-roles"></a>Identificeren en categoriseren accounts die binnen het maximaal bevoorrechte rollen 

Na het inschakelen van Azure AD Privileged Identity Management, de gebruikers bekijken die in de globale beheerder voor directory-functies, beheerder met bevoorrechte rol, beheerder van Exchange Online en SharePoint Online-beheerder. Als u geen Azure AD PIM in uw tenant, kunt u de [PowerShell API](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0). Start met de rol globale beheerder zoals deze rol algemeen is: een gebruiker die deze beheerdersrol is toegewezen, heeft dezelfde machtigingen in voor alle cloudservices waarvoor uw organisatie abonnement een, ongeacht of ze deze rol in de Office 365-portal hebt toegewezen , de Azure portal of met behulp van de Azure AD-module voor Microsoft PowerShell. 

Verwijderen van accounts die niet langer nodig zijn in deze rollen en de resterende accounts die zijn toegewezen aan beheerderrollen categoriseren:

* Afzonderlijk toegewezen aan gebruikers met beheerdersrechten en kan ook worden gebruikt voor niet-administratieve doeleinden (bijvoorbeeld persoonlijke e-mailbericht)
* Afzonderlijk toegewezen aan gebruikers met beheerdersrechten en aangewezen voor administratieve doeleinden alleen
* Gedeeld door meerdere gebruikers
* Voor einde glas Noodtoegang scenario 's
* Voor geautomatiseerde scripts
* Voor externe gebruikers

#### <a name="define-at-least-two-emergency-access-accounts"></a>Ten minste twee accounts voor Noodtoegang definiëren 

Zorg ervoor dat er in een situatie waarin deze kunnen worden per ongeluk vergrendeld buiten het beheer van uw Azure AD-tenant vanwege onvermogen aanmelden of activeren van een bestaande afzonderlijke gebruiker account als beheerder. Bijvoorbeeld, als de organisatie is gefedereerd naar een lokale id-provider, die id-provider is mogelijk niet beschikbaar zodat gebruikers zich in de lokale kunnen niet aanmelden. U kunt de impact van onbedoeld gebrek aan beheerderstoegang verhelpen door twee of meer accounts voor Noodtoegang zijn opgeslagen in uw tenant.

Accounts voor Noodtoegang waarmee organisaties bevoorrechte toegang in een bestaande Azure Active Directory-omgeving te beperken. Deze accounts over uitgebreide beheerdersmogelijkheden en zijn niet toegewezen aan specifieke personen. Accounts voor Noodtoegang zijn beperkt tot noodgevallen voor 'break glas'-scenario's waarin de normale beheerdersaccounts kunnen niet worden gebruikt. Organisaties moet het doel van het beheren en informatie over het gebruik van de noodgevallen account beperken tot alleen die tijd die nodig is. 

Denk na over de accounts die toegewezen of komt in aanmerking voor de rol globale beheerder zijn. Als u hebt niet de cloudconfiguratie gebruikersaccounts via de *. onmicrosoft.com-domein (bedoeld voor Noodtoegang 'nood'), maken ze. Zie voor meer informatie [Noodtoegang beheerdersaccounts beheren in Azure AD](active-directory-admin-manage-emergency-access-accounts.md).

#### <a name="turn-on-multi-factor-authentication-and-register-all-other-highly-privileged-single-user-non-federated-admin-accounts"></a>Meervoudige verificatie inschakelen en alle andere maximaal bevoegdheden één gebruiker niet-gefedereerde beheerdersaccounts registreren 

Azure multi-factor Authentication (MFA) vereist bij het aanmelden voor alle afzonderlijke gebruikers die permanent zijn toegewezen aan een of meer van de Azure AD-beheerdersrollen: hoofdbeheerder, beheerder met bevoorrechte rol beheerder van Exchange Online en SharePoint Online-beheerder. De gids gebruiken om in te schakelen [multi-factor Authentication (MFA) voor uw beheerdersaccounts](authentication/howto-mfa-userstates.md) en zorg ervoor dat alle gebruikers hebt geregistreerd bij [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup). Meer informatie vindt u in stap 2 en 3 van de handleiding [beveiligen van toegang tot gegevens en services in Office 365](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e). 

## <a name="stage-2-mitigate-the-most-frequently-used-attack-techniques"></a>Stap 2: De meest gebruikte aanval technieken beperken

![Fase 2](./media/admin-roles-best-practices/stage-two.png)

Fase 2 van het overzicht is gericht op de meest gebruikte aanval technieken van diefstal van referenties en misbruik beperkende en is ontworpen om te worden geïmplementeerd in ongeveer 2-4 weken. Deze fase van de bevoorrechte toegang beveiligd roadmap omvat de volgende acties.

### <a name="general-preparation"></a>Algemene voorbereiding

#### <a name="conduct-a-inventory-of-services-owners-and-admins"></a>Uitvoeren van een inventaris van services, eigenaars en beheerders

Met de toename in bring-your-own-device (BYOD) en werk van startpagina beleid en de groei van de draadloze verbinding in bedrijven is het van essentieel die verbinding met uw netwerk maakt te bewaken. Een effectieve beveiligingsmethode audit vaak blijkt dat apparaten, toepassingen en programma's op uw netwerk op dat niet wordt ondersteund door IT, en daarom mogelijk niet worden beveiligd. Zie voor meer informatie [Azure beveiligingsbeheer en -bewaking overzicht](../security/security-management-and-monitoring-overview.md). Zorg ervoor dat u de volgende taken in de procedure inventarisatie opneemt. 

* Identificeer de gebruikers die hebben beheerdersrollen en services waar ze kunnen beheren.
* Azure AD PIM gebruiken om erachter te komen welke gebruikers in uw organisatie beheerderstoegang naar Azure AD hebben, met inbegrip van aanvullende functies afgezien van wat is vermeld in stap 1.
* Afgezien van de rollen gedefinieerd in Azure AD, beschikt Office 365 over een set beheerdersrollen die u aan gebruikers in uw organisatie toewijzen kunt. Elke beheerdersrol toegewezen aan algemene bedrijfsfuncties en personen in uw Organisatiemachtigingen voor het uitvoeren van bepaalde taken in het Office 365-beheercentrum biedt. Gebruik het Office-beheercentrum om erachter te komen welke gebruikers in uw organisatie beheerderstoegang op Office 365 hebben, met inbegrip van via rollen niet worden beheerd in Azure AD. Zie voor meer informatie [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) en [aanbevolen beveiligingsprocedures voor Office 365](https://support.office.com/article/Security-best-practices-for-Office-365-9295e396-e53d-49b9-ae9b-0b5828cdedc3).
* De inventarisatie uitvoeren in andere services die uw organisatie wordt gebruikt, zoals Azure, Intune of Dynamics 365.
* Zorg ervoor dat uw beheerdersaccounts (accounts die worden gebruikt voor beheerdoeleinden, niet alleen dagelijkse gebruikersaccounts) hebt gekoppeld aan deze e-mailadressen werken en voor Azure MFA hebt geregistreerd, of gebruik MFA lokale.
* Gebruikers vragen om hun zakelijke rechtvaardiging voor beheerderstoegang.
* Verwijder beheerderstoegang voor deze personen en services die deze niet nodig is.

#### <a name="identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts"></a>Microsoft-accounts in beheerdersrollen die moeten worden omgeschakeld werk of school accounts identificeren 

De eerste globale beheerders voor een organisatie hergebruiken soms hun bestaande Microsoft-accountreferenties aanvang ze gebruikmaken van Azure AD. Deze Microsoft-accounts moeten worden vervangen door afzonderlijke accounts voor cloud-gebaseerde of gesynchroniseerd. 

#### <a name="ensure-separate-user-accounts-and-mail-forwarding-for-global-administrator-accounts"></a>Controleer afzonderlijke gebruikersaccounts en e-mail doorsturen voor globale beheerdersaccounts 

Globale beheerdersaccounts mag geen persoonlijke e-mailadressen als persoonlijke e-mailaccounts regelmatig phished cyberbeveiliging aanvallers worden. Om te scheiden van internet risico's (phishingaanvallen, onbedoelde websurfen) maken vanuit beheerdersbevoegdheden, specifieke accounts voor elke gebruiker met beheerdersbevoegdheden. 

Als u dit nog niet hebt gedaan, maakt u afzonderlijke accounts voor gebruikers voor globale beheerder taken, om te controleren of ze niet per ongeluk openen e-mailberichten of programma's die zijn gekoppeld aan hun beheerdersaccounts uitvoeren. Zorg ervoor dat deze accounts hebben hun e-mail worden doorgestuurd naar een werkende postvak.  

#### <a name="ensure-the-passwords-of-administrative-accounts-have-recently-changed"></a>Zorg ervoor dat de wachtwoorden van beheerdersaccounts onlangs hebt gewijzigd

Zorg dat alle gebruikers zijn aangemeld bij hun beheerdersaccounts en hun wachtwoord is gewijzigd ten minste eenmaal in de afgelopen 90 dagen. Controleer ook die gedeelde accounts in welke meerdere gebruikers het wachtwoord weten hun wachtwoord onlangs gewijzigd heeft gehad.

#### <a name="turn-on-password-hash-synchronization"></a>Synchronisatie van wachtwoordhash inschakelen

Synchronisatie van wachtwoordhash is een functie die wordt gebruikt om te synchroniseren-hashes van gebruiker wachtwoord-hashes van een lokale Active Directory-exemplaar naar een cloud-gebaseerde Azure AD-exemplaar. Zelfs dat als u besluit Federatie met Active Directory Federation Services (AD FS) of een andere id-providers gebruiken, kunt u eventueel instellen van synchronisatie van wachtwoordhash als een back-up in geval uw on-premises infrastructuur zoals AD of AD FS-servers mislukt of niet tijdelijk niet beschikbaar. Hierdoor kunnen gebruikers zich aanmelden bij de service met behulp van het wachtwoord dat ze gebruiken om aan te melden bij hun on-premises AD-exemplaar. Bovendien kan de beveiliging van de identiteit voor het detecteren van gestolen referenties door de wachtwoord-hashes te vergelijken met mogelijk onveilig bekend als een gebruiker is gebruikt, hun hetzelfde e-mailadres en wachtwoord van andere services die niet is verbonden met Azure AD-wachtwoorden.  Zie voor meer informatie [hash Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie implementeren](./connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).

#### <a name="require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users"></a>Multi-factor authentication (MFA) voor gebruikers in alle bevoorrechte rollen, evenals blootgestelde gebruikers vereisen

Azure AD raadt aan dat u multi-factor authentication (MFA) voor alle gebruikers, beheerders en andere gebruikers die een aanzienlijke gevolgen hebben zou als hun account is geknoeid (bijvoorbeeld financieel medewerkers) vereist. Dit vermindert het risico van een aanval als gevolg van een wachtwoord waarmee is geknoeid.

Inschakelen:

* [MFA voor hoge blootstelling accounts](authentication/multi-factor-authentication-security-best-practices.md) zoals accounts van managers in een organisatie 
* [MFA voor elke beheeraccount die is gekoppeld aan een afzonderlijke gebruiker](authentication/howto-mfa-userstates.md) voor andere SaaS-apps verbonden 
* MFA voor alle beheerders voor Microsoft SaaS-apps, inclusief beheerders in rollen beheerd Exchange Online en de Office-portal

Als u Windows Hello voor bedrijven, kan de MFA-vereiste worden voldaan met behulp van het Windows Hello teken in de gebruikerservaring. Zie voor meer informatie [Windows Hello](https://docs.microsoft.com/windows/uwp/security/microsoft-passport). 

#### <a name="configure-identity-protection"></a>Identity Protection configureren 

Azure AD Identity Protection is een algoritme gebaseerde controle en rapportage van hulpprogramma dat u gebruiken kunt voor het detecteren van mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie. U kunt configureren van automatische antwoorden op deze gedetecteerde verdachte activiteiten en onderneem gepaste actie deze op te lossen. Zie voor meer informatie [Azure Active Directory: Identity Protection](active-directory-identityprotection.md).

#### <a name="obtain-your-office-365-secure-score-if-using-office-365"></a>Verkrijgen van uw Office 365 Secure Score (als u Office 365)

Beveiligde Score afbeeldingen welke Office 365-services u gebruikt (zoals OneDrive, SharePoint en Exchange) en vervolgens uw instellingen en activiteiten wordt bekeken en vergelijkt ze met een basislijn voor het tot stand gebracht door Microsoft. U krijgt een score op basis van hoe uitgelijnde u zich met best practices voor beveiliging. Iedereen met beheerdersmachtigingen (globale beheerder of een aangepaste beheerdersrol) voor een Office 365 Business Premium of Enterprise-abonnement toegang tot de Score beveiligen op [ https://securescore.office.com ](https://securescore.office.com/).

#### <a name="review-the-office-365-security-and-compliance-guidance-if-using-office-365"></a>Bekijk de Office 365-richtlijnen voor beveiliging en naleving (als u Office 365)

De [plannen voor beveiliging en naleving](https://support.office.com/article/Plan-for-security-and-compliance-in-Office-365-dc4f704c-6fcc-4cab-9a02-95a824e4fb57) geeft een overzicht van de aanpak voor hoe een Office 365-klant moet configureren van Office 365 en gebruikmaken van andere mogelijkheden van EMS. Vervolgens, revisie 3-6 van hoe u stappen [beveiligen van toegang tot gegevens en services in Office 365](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e) en de handleiding voor het [beveiliging en naleving in Office 365 controleren](https://support.office.com/article/Monitor-security-and-compliance-in-Office-365-b62f1722-fd39-44eb-8361-da61d21509b6).


#### <a name="configure-office-365-activity-monitoring-if-using-office-365"></a>Bewaking van Office 365 activiteit te configureren (als u Office 365)

U kunt controleren hoe mensen in uw organisatie zijn met Office 365-services, zodat u kunt gebruikers die een Administrator-account hebben en die mogelijk niet nodig Office 365 gebruiken vanwege niet aanmelden bij deze portals identificeren. Zie voor meer informatie [activiteit rapporten in het Office 365-beheercentrum](https://support.office.com/article/Activity-Reports-in-the-Office-365-admin-center-0d6dfb17-8582-4172-a9a9-aed798150263).

#### <a name="establish-incidentemergency-response-plan-owners"></a>Incident/emergency response plan eigenaars tot stand brengen

Uitvoeren van respons op incidenten effectief is een complexe onderneming. Daarom vereist tot stand brengen van een geslaagde respons op incidenten mogelijkheid een aanzienlijke planning en resources. Het is essentieel dat u voortdurend voor cyberbeveiliging aanvallen controleren en procedures voor de prioriteit van de verwerking van incidenten vast te stellen. Effectieve methoden voor het verzamelen, analyseren en rapporteren van gegevens zijn essentieel voor het bouwen van relaties en voor het communiceren met andere interne groepen en eigenaren van plan bent. Zie voor meer informatie [Microsoft Security Response Center](https://technet.microsoft.com/security/dn440717). 

#### <a name="secure-on-premises-privileged-administrative-accounts-if-not-already-done"></a>Veilige on-premises administratieve accounts met bevoegdheden als u niet hebt gedaan

Volg de instructies in als uw Azure Active Directory-tenant is gesynchroniseerd met de lokale Active Directory, [beveiliging bevoorrechte toegang Roadmap](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): stap 1. Dit omvat het maken van afzonderlijke beheerdersaccounts voor gebruikers die nodig zijn voor het uitvoeren van beheertaken voor lokale Privileged Access Workstations implementeren voor Active Directory-beheerders en voor het maken van unieke lokale Administrators wachtwoorden voor werkstations en -servers.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Aanvullende stappen voor het beheren van toegang tot Azure organisaties

#### <a name="complete-an-inventory-of-subscriptions"></a>Een inventaris van abonnementen voltooien

De Enterprise portal en de Azure portal gebruiken om te identificeren van de abonnementen in uw organisatie die productietoepassingen hosten. 

#### <a name="remove-microsoft-accounts-from-admin-roles"></a>Microsoft-accounts uit beheerdersrollen verwijderen

Microsoft-accounts van andere programma's, zoals Xbox Live en Outlook mag niet worden gebruikt als administrator-accounts voor organisatie-abonnementen. Verwijder admin status van alle Microsoft-accounts en vervang met Active Directory (bijvoorbeeld chris@contoso.com) werk of school accounts.

#### <a name="monitor-azure-activity"></a>Azure-activiteit controleren

De Azure Activity Log biedt een geschiedenis van gebeurtenissen in de Azure-abonnement. Biedt informatie over wie gemaakt, bijgewerkt en welke resources hebt verwijderd en wanneer deze gebeurtenissen opgetreden. Zie voor meer informatie [Audit en meldingen ontvangen over belangrijke acties in uw Azure-abonnement](../monitoring-and-diagnostics/monitor-quick-audit-notify-action-in-subscription.md).


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Aanvullende stappen voor het beheren van toegang tot andere cloud-apps via Azure AD organisaties 

#### <a name="configure-conditional-access-policies"></a>Beleid voor voorwaardelijke toegang configureren

Bereid de beleidsregels voor voorwaardelijke toegang voor on-premises en cloud-gebaseerde toepassingen. Meer informatie zoeken over als u toegevoegd aan de werkplek apparaten van gebruikers hebt, [instellen van on-premises voorwaardelijke toegang door met behulp van Azure Active Directory-apparaatregistratie](active-directory-device-registration-on-premises-setup.md).


## <a name="stage-3-build-visibility-and-take-full-control-of-admin-activity"></a>Stap 3: Bouwen zichtbaarheid en volledige controle krijgen over admin-activiteit

![Fase 3](./media/admin-roles-best-practices/stage-three.png)

Fase 3 is gebaseerd op de beperkingen van fase 2 en is ontworpen om te worden geïmplementeerd in ongeveer 1-3 maanden. Deze fase van de bevoorrechte toegang beveiligd roadmap omvat de volgende onderdelen.

### <a name="general-preparation"></a>Algemene voorbereiding

#### <a name="complete-an-access-review-of-users-in-administrator-roles"></a>Een beoordeling van de toegang van gebruikers in beheerdersrollen voltooien

Meer zakelijke gebruikers hun bevoorrechte toegang via cloud-services, wat tot een toenemende onbeheerde platform leiden kan. Dit omvat gebruikers om globale beheerders voor Office 365, Azure-abonnementbeheerders en gebruikers met beheerderstoegang voor virtuele machines of via de SaaS-apps. Organisaties moeten in plaats daarvan alle werknemers vooral beheerders, dagelijkse zakelijke transacties als niet-gemachtigd gebruikers verwerken en beheerdersrechten alleen ondernemen zo nodig hebben. Omdat het aantal gebruikers in beheerdersrollen mogelijk gegroeid sinds de eerste toepassing, controleert de volledige toegang om te identificeren en elke gebruiker die in aanmerking komt voor het activeren van beheerdersbevoegdheden bevestigen. 

Ga als volgt te werk:

* Bepalen welke gebruikers Azure AD-beheerders, inschakelen op aanvraag just in time beheerderstoegang en besturingselementen voor beveiliging op basis van rollen.
* Gebruikers die geen duidelijke reden voor admin bevoegde toegang tot een andere rol hebben converteren (als er geen in aanmerking komende rol, verwijdert u deze).

#### <a name="continue-rollout-of-stronger-authentication-for-all-users"></a>Implementatie van een sterker verificatiemechanisme wordt gebruikt voor alle gebruikers gaan 

Vereisen C-suite leidinggevenden, op hoog niveau managers, kritieke IT en beveiligingspersoneel en andere gebruikers maximaal blootgesteld moderne, sterke verificatie, zoals Azure MFA of Windows Hello hebben. 

#### <a name="use-dedicated-workstations-for-administration-for-azure-ad"></a>Speciale werkstations gebruikt voor beheer voor Azure AD

Aanvallers kunnen proberen toe te passen bevoegde accounts toegang krijgen tot gegevens en systemen van een organisatie zodat ze de integriteit en de echtheid van gegevens via schadelijke code die de programmalogica wordt gewijzigd of admin invoeren van een referentie snoops kunnen verstoren. Privileged Access Workstations (poten) bieden een toegewijde besturingssysteem voor gevoelige taken die is beveiligd tegen aanvallen via Internet en bedreigingen met zich mee. Deze gevoelige taken en accounts scheiden van het dagelijks gebruik werkstations en apparaten zeer sterke bescherming bieden tegen phishingaanvallen, toepassing en OS beveiligingsproblemen, verschillende imitatie aanvallen en credential theft aanvallen, zoals toetsaanslag logboekregistratie, Pass-the-Hash en Pass-The-Ticket. Privileged access workstations implementeert, kunt u het risico dat beheerders op een bureaubladomgeving die heeft is gehard beheerdersreferenties behalve invoert reduceren. Zie voor meer informatie [Privileged Access Workstations](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

#### <a name="review-national-institute-of-standards-and-technology-recommendations-for-handling-incidents"></a>Bekijk National Institute of Standards en technologie aanbevelingen voor het afhandelen van incidenten 

Het National Institute of Standards and van Technology (NIST) bevat richtlijnen voor het afhandelen van incidenten, met name voor incident-gerelateerde gegevens te analyseren en de juiste reactie op elk incident te bepalen. Zie voor meer informatie [(NIST) de Computer Security Incident Handling Guide (SP 800 61, revisie 2)](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf).

#### <a name="implement-privileged-identity-management-pim-for-jit-to-additional-administrative-roles"></a>Privileged Identity Management (PIM) implementeren voor JIT tot extra beheerderrollen

Gebruik voor Azure Active Directory, [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) mogelijkheid. Tijdelijke activering van bevoorrechte rollen werkt doordat u:

* Beheerdersbevoegdheden voor het uitvoeren van een specifieke taak te activeren
* Afdwingen van MFA tijdens de activering
* Waarschuwingen gebruiken beheerders informeren over out-of-band-wijzigingen
* Gebruikers kunnen bepaalde bevoegdheden, behouden voor een vooraf geconfigureerde tijdsduur inschakelen
* Dat beveiliging beheerders alle bevoegde identiteiten detecteren, audit rapporten weergeven en onderzoeken om te identificeren van elke gebruiker die in aanmerking komt voor het activeren van beheerdersbevoegdheden toegang maken

Als u al Azure AD Privileged Identity Management gebruikt, pas deze tijdsperioden voor tijdsgebonden bevoegdheden zo nodig (bijvoorbeeld onderhoudsvensters).

#### <a name="determine-exposure-to-password-based-sign-in-protocols-if-using-exchange-online"></a>Bepalen van blootstelling aan op basis van wachtwoorden aanmelden protocollen (als u Exchange Online)

In het verleden uitgegaan protocollen dat de combinaties van gebruikersnaam en wachtwoord zijn ingesloten in apparaten, e-mail-accounts en -telefoons. Maar nu met het risico voor cyberbeveiliging-aanvallen in de cloud, wordt aangeraden identificeren van elke mogelijke gebruiker die, als hun referenties zijn aangetast, kan onherstelbare naar de organisatie, en kunnen uitsluiten aanmelden met hun e-mail via gebruikersnaam / wachtwoord door het implementeren van de vereisten van de sterke verificatie en voorwaardelijke toegang. 

#### <a name="complete-a-roles-review-assessment-for-office-365-roles-if-using-office-365"></a>Voltooien van een beoordeling van de rollen revisie voor Office 365-functies (als u Office 365)

Controleren of alle beheerders gebruikers in de juiste rollen zijn (verwijderen en opnieuw toewijzen volgens deze beoordeling).

#### <a name="review-the-security-incident-management-approach-used-in-office-365-and-compare-with-your-own-organization"></a>Bekijk de beveiliging incidentbeheer benadering gebruikt in Office 365 en vergelijken met uw eigen organisatie

U kunt dit rapport uit downloaden [Security Incident Management in Microsoft Office 365](https://www.microsoft.com/download/details.aspx?id=54302).

#### <a name="continue-to-secure-on-premises-privileged-administrative-accounts"></a>Doorgaan bij de beveiliging van lokaal bevoorrecht beheerdersaccounts

Als uw Azure Active Directory is verbonden met de lokale Active Directory, volgt u de instructies in de [beveiliging bevoorrechte toegang Roadmap](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): fase 2. Dit omvat Privileged Access Workstations implementeren voor alle beheerders, MFA verplicht stellen, met behulp van net genoeg Admin voor DC-onderhoud, de kwetsbaarheid voor aanvallen van domeinen te verlagen, ATA-implementatie voor de aanvalsdetectie.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Aanvullende stappen voor het beheren van toegang tot Azure organisaties

#### <a name="establish-integrated-monitoring"></a>Geïntegreerde controle instellen

De [Azure Security Center](../security-center/security-center-intro.md) biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bij het detecteren van bedreigingen die mogelijk anders opgemerkt, en werkt met een uitgebreid ecosysteem van beveiliging oplossingen.

#### <a name="inventory-your-privileged-accounts-within-hosted-virtual-machines"></a>Inventariseer uw beschermde accounts binnen de gehoste virtuele Machines

In de meeste gevallen moet u geen gebruikers onbeperkte machtigingen geven voor alle Azure-abonnementen of resources. Azure AD-beheerdersrollen kunt u taken scheiden binnen uw organisatie en de hoeveelheid toegang verlenen aan gebruikers die toegang moeten specifieke taken uitvoeren. Bijvoorbeeld, Azure AD-beheerdersrollen gebruiken zodat een beheerder alleen virtuele machines in een abonnement beheren terwijl een andere SQL-databases binnen hetzelfde abonnement kunt beheren. Zie voor meer informatie [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../role-based-access-control/overview.md).

#### <a name="implement-pim-for-azure-ad-administrator-roles"></a>PIM voor Azure AD-beheerdersrollen implementeren

Privileged identity Management gebruiken met Azure AD-beheerdersrollen, beheren en controleren van toegang tot Azure-resources. Met behulp van PIM beschermt bevoegde accounts tegen cyberbeveiliging aanvallen door de blootstellingstijd van bevoegdheden te verlagen en vergroot de zichtbaarheid van het gebruik door middel van rapporten en waarschuwingen. Zie voor meer informatie [RBAC beheren van toegang tot Azure-resources met Privileged Identity Management](../role-based-access-control/pim-azure-resource.md).

#### <a name="use-azure-log-integrations-to-send-relevant-azure-logs-to-your-siem-systems"></a>Azure-logboekanalyse integraties gebruiken om te verzenden relevante Azure logboeken naar uw SIEM-systemen 

Azure-logboekanalyse-integratie kunt u voor het integreren van onbewerkte logboeken van uw Azure-resources van uw organisatie bestaande Security Information en Event Management (SIEM)-systemen. [Integratie van Azure-logboekanalyse](../security/security-azure-log-integration-overview.md) Windows-gebeurtenissen verzamelt van Windows-Logboeken en Azure-resources van Azure activiteitenlogboeken, Azure Security Center-waarschuwingen en Azure diagnostische logboeken. 


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Aanvullende stappen voor het beheren van toegang tot andere cloud-apps via Azure AD organisaties

#### <a name="implement-user-provisioning-for-connected-apps"></a>Gebruikersaanvragen voor verbonden apps implementeren

Azure AD kunt u het maken, onderhoud en verwijderen van de gebruikers-id's, zoals Dropbox, Salesforce, ServiceNow in cloudtoepassingen (SaaS) automatiseren, enzovoort. Zie voor meer informatie [gebruikers inrichten en opheffen van inrichting tot SaaS-toepassingen met Azure AD automatiseren](active-directory-saas-app-provisioning.md).

#### <a name="integrate-information-protection"></a>Gegevensbeveiliging integreren

MCAS kunt u onderzoeken van bestanden en beleidsregels op basis van de classificatielabels voor Azure Information Protection, meer zichtbaarheid en controle van uw gegevens in de cloud inschakelen. Scannen en classificeren van bestanden in de cloud en Azure information protection labels toepassen. Zie voor meer informatie [integratie van Azure Information Protection](https://docs.microsoft.com/cloud-app-security/azip-integration).

#### <a name="configure-conditional-access"></a>Voorwaardelijke toegang configureren

Configureren van voorwaardelijke toegang op basis van een groep, de locatie en de toepassing gevoeligheid voor [SaaS-apps](https://azure.microsoft.com/overview/what-is-saas/) en Azure AD verbonden apps. 

#### <a name="monitor-activity-in-connected-cloud-apps"></a>Activiteit in de gekoppelde cloud-apps bewaken

Toegang van gebruikers in verbonden toepassingen ook is beveiligd, zodat het is raadzaam dat u gebruikmaken van [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). Dit beveiligt enterprise toegang tot de cloud-apps, naast het beveiligen van uw beheerdersaccounts doordat u:

* Uitbreiden zichtbaarheid en beheer van cloud-apps
* Beleidsregels voor toegang, activiteiten en delen van gegevens maken
* Automatisch riskant activiteiten, abnormaal gedrag en bedreigingen identificeren
* Voorkomen dat gegevens worden gelekt
* Minimaliseren risico en geautomatiseerde dreiging te voorkomen en afdwingen van beleid

De Cloud App Security SIEM-agent Cloud App Security is geïntegreerd met uw SIEM-server gecentraliseerde bewaking van Office 365-waarschuwingen en activiteiten wilt inschakelen. Deze wordt uitgevoerd op uw server en waarschuwingen en activiteiten ophaalt uit de Cloud App Security en ze streamt in de SIEM-server. Zie voor meer informatie [SIEM integratie](https://docs.microsoft.com/cloud-app-security/siem).

## <a name="stage-4-continue-building-defenses-to-a-more-proactive-security-posture"></a>Stap 4: Gaan met het samenstellen van beveiliging voor een meer proactieve beveiligingspostuur


![Stap 4](./media/admin-roles-best-practices/stage-four.png)

Stap 4 van de roadmap bouwt voort op de zichtbaarheid van stap 3 en is ontworpen om te worden geïmplementeerd in zes maanden en hoger. Voltooien van een roadmap helpt bij het ontwikkelen van strong bevoegde toegang bescherming tegen potentiële aanvallen die tegenwoordig momenteel bekende en beschikbaar zijn. Helaas beveiligingsrisico's voortdurend evolueren en verschuiven, zodat het is raadzaam dat u de beveiliging als een continue proces gericht op het verhogen van de kosten en verminderen het slagingspercentage van tegenstanders die gericht is op uw omgeving weergeven.

Beveiligen van bevoegde toegang is een belangrijke eerste stap bij het tot stand brengen van garantie van beveiliging voor zakelijke activa in een moderne organisatie, maar is niet het enige deel van een volledige beveiligingsprogramma dat elementen, zoals Groepsbeleid, bewerkingen, informatie bevatten zou beveiliging, servers, toepassingen, pc's, apparaten, cloud-infrastructuurresources en andere onderdelen bieden continue beveiliging garantie. 

Naast het beheren van uw accounts bevoorrechte toegang, raden we dat u de volgende voortdurend bekijken:

* Zorg ervoor dat beheerders hun dagelijkse bedrijven als onbevoegde gebruikers doen.
* Alleen bevoorrechte toegang wanneer deze nodig is verleend en verwijdert u naderhand (just-in-time).
* Behouden en audit controleactiviteit met betrekking tot beschermde accounts.

Zie voor meer informatie over het bouwen van een volledige beveiliging roadmap [Microsoft IT-architectuur cloudresources](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources). Neem contact op met uw Microsoft-vertegenwoordiger of Zie voor meer informatie over het Microsoft-services om te helpen bij deze onderwerpen bezighouden [bouwen kritieke cyberbeveiliging verdediging ter bescherming van uw onderneming](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

Deze laatste lopende fase van de bevoorrechte toegang beveiligd roadmap omvat de volgende onderdelen.

### <a name="general-preparation"></a>Algemene voorbereiding

#### <a name="review-admin-roles-in-azure-active-directory"></a>Beheerdersrollen in Azure Active Directory controleren 

Bepalen of de huidige ingebouwde Azure AD-beheerdersrollen nog steeds up-to-date zijn en zorg ervoor dat gebruikers zich alleen in de functies en delegeringen die ze nodig hebben voor de bijbehorende machtigingen. U kunt afzonderlijke beheerders verschillende functies vervullen aanwijzen met Azure AD. Zie voor meer informatie [beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).

#### <a name="review-users-who-have-administration-of-azure-ad-joined-devices"></a>Controleer gebruikers die voor beheer van Azure AD hebben die lid zijn van apparaten

Zie voor meer informatie [hoe het configureren van hybride Azure Active Directory die lid zijn van apparaten](device-management-hybrid-azuread-joined-devices-setup.md).

#### <a name="review-members-of-built-in-office-365-admin-roleshttpssupportofficecomarticleabout-office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d"></a>Bekijk de leden van [ingebouwde beheerdersrollen voor Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)
Als u werkt met Office 365.
‎
#### <a name="validate-incident-response-plan"></a>Plan voor respons op incidenten valideren

Als u wilt een betere beveiliging dan uw plan, raadt Microsoft dat u regelmatig valideren dat uw plan werkt zoals verwacht:

* Ga via uw bestaande routekaart om te zien wat is overgeslagen
* Op basis van de postmortem analyse, herzien bestaande of nieuwe aanbevolen procedures definiëren
* Zorg ervoor dat uw bijgewerkte beveiligingsplan en best practices binnen uw organisatie worden gedistribueerd


### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Aanvullende stappen voor het beheren van toegang tot Azure organisaties 

Als u wilt bepalen [eigendom van een Azure-abonnement overdragen aan een ander account](../billing/billing-subscription-transfer.md).
‎

## <a name="break-glass-what-to-do-in-an-emergency"></a>'Nood': wat te doen in noodgevallen

![Spoed](./media/admin-roles-best-practices/emergency.jpeg)

1. Belangrijkste managers en -afdelingen met relevante informatie met betrekking tot het incident melden.

2. Controleer uw playbook aanval. 

3. Toegang tot uw combinatie 'nood' account gebruikersnaam en wachtwoord aanmelden bij Azure AD. 

4. Hulp van Microsoft door [openen van een aanvraag voor de ondersteuning van Azure](../azure-supportability/how-to-create-azure-support-request.md).

5. Bekijk de [Azure AD-in rapporten](active-directory-reporting-azure-portal.md). Er is mogelijk een vertraging tussen een gebeurtenis optreedt en het is opgenomen in het rapport.

6. Voor hybride omgevingen, als federatieve en uw AD FS-server niet beschikbaar is, moet u mogelijk tijdelijk overschakelen van federatieve verificatie hash Wachtwoordsynchronisatie te gebruiken. Hierdoor wordt de federatieve domein naar beheerde verificatie tot de AD FS-server weer beschikbaar.

7. E-mailadres voor bevoegde accounts bewaken.

8. Zorg ervoor dat u back-ups van de relevante logboeken voor potentiële forensische en juridisch onderzoek naar opslaan.

Zie voor meer informatie over hoe Microsoft Office 365 omgaat met beveiligingsincidenten [Security Incident Management in Microsoft Office 365](http://aka.ms/Office365SIM).

## <a name="faq-common-questions-we-receive-regarding-securing-privileged-access"></a>Veelgestelde vragen over: Veelgestelde vragen we ontvangen met betrekking tot de bevoegde toegang beveiligen  


**V:** wat moet ik doen als ik alle onderdelen van beveiligde toegang nog niet hebt geïmplementeerd?

**Antwoord:** account ten minste twee break glas definiëren, MFA toewijzen aan bevoorrechte beheerdersaccounts, en afzonderlijke accounts van globale beheerdersaccounts.


**V:** na een inbreuk, wat is er een probleem met de hoogste moet eerst worden aangepakt?

**Antwoord:** Zorg ervoor dat u bent de sterkste waarvoor verificatie is vereist voor personen maximaal beschikbaar.


**V:** wat gebeurt er als onze bevoorrechte beheerders zijn gedeactiveerd?

**Antwoord:** maken van een globale beheerdersaccount die altijd up-to-date wordt gehouden.


**V:** wat er gebeurt als er slechts één globale beheerder links en kan niet worden bereikt? 

**Antwoord:** gebruik een van uw accounts break glas onmiddellijke bevoegde toegang te krijgen.


**V:** hoe kan ik beveiligen beheerders in mijn organisatie?

**Antwoord:** hebben beheerders hun dagelijkse zaken als 'niet-gemachtigd' standaardgebruikers altijd doet.
 

**V:** wat zijn de aanbevolen procedures voor het maken van beheerdersaccounts in Azure AD?

**Antwoord:** Reserve bevoegde toegang voor specifieke beheertaken.


**V:** welke hulpprogramma's bestaan voor het beperken van permanente beheerderstoegang?

**Antwoord:** Privileged Identity Management (PIM) en Azure AD-beheerdersrollen.


**V:** wat is de positie van Microsoft voor het synchroniseren van beheerdersaccounts naar Azure AD?

**Antwoord:** niveau van Tier 0 (inclusief accounts, groepen en andere activa die directe of indirecte beheer van de AD-forest, domeinen of domeincontrollers hebt en alle activa) beheerdersaccounts worden benut alleen voor on-premises AD-accounts en zijn normaliter niet gesynchroniseerd voor Azure AD voor de cloud. 


**V:** hoe voorkom ik dat beheerders willekeurige beheerderstoegang in de portal worden toegewezen?

**Antwoord:** niet-gemachtigde accounts gebruiken voor alle gebruikers en de meeste beheerders. Start een voetafdruk van de organisatie om te bepalen welke enkele beheerdersaccounts moeten bevoorrecht ontwikkelen. En voor gebruikers met beheerdersrechten gemaakte bewaken.


## <a name="next-steps"></a>Volgende stappen

* [Microsoft Trust Center voor de beveiliging van het Product](https://www.microsoft.com/en-us/trustcenter/security) – beveiligingsfuncties van Microsoft cloud-producten en services

* [Microsoft Trust Center - naleving](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings) – uitgebreide reeks naleving aanbiedingen voor cloudservices van Microsoft

* [Richtlijnen voor het uitvoeren van een risicoanalyse](https://www.microsoft.com/en-us/trustcenter/guidance/risk-assessment) -beveiliging en naleving vereisten voor Microsoft-cloudservices beheren

### <a name="other-ms-online-services"></a>Andere onlineservices MS 

* [Microsoft Intune Security](https://www.microsoft.com/en-us/trustcenter/security/intune-security) – Intune mobile device management, beheer van mobiele toepassingen en PC-beheermogelijkheden vanuit de cloud biedt.

* [Beveiliging van Microsoft Dynamics 365](https://www.microsoft.com/en-us/trustcenter/security/dynamics365-security) – Dynamics 365 is de Microsoft cloud-gebaseerde oplossing die bundelt customer relationship management (CRM) en ERP-mogelijkheden (ERP).

 
