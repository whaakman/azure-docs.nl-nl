---
title: Vier stappen om een sterke identiteit basis met Azure Active Directory
description: Dit onderwerp beschrijft de vier stappen hybride identiteit klanten uitvoeren kunnen om het bouwen van een sterke identiteit foundation.
services: active-directory
author: martincoetzer
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2019
ms.subservice: hybrid
ms.author: martincoetzer
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96b5e8ab63c1784ff073c7ba38cd4a6319db43c5
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67452733"
---
# <a name="four-steps-to-a-strong-identity-foundation-with-azure-active-directory"></a>Vier stappen om een sterke identiteit basis met Azure Active Directory

Beheer van toegang tot apps en gegevens kan niet meer zijn afhankelijk van het traditionele netwerk grens beveiligingsstrategieën zoals perimeternetwerken en firewalls vanwege de snelle verplaatsing van apps naar de cloud. Organisaties moeten nu een oplossing voor hun identiteit te bepalen wie en wat toegang heeft tot de apps en gegevens van de organisatie vertrouwen. Meer organisaties toestaat dat werknemers hun eigen apparaten meebrengen naar het werk en hun apparaten gebruiken vanaf elke locatie die ze verbinding met Internet maken kunnen. Ervoor te zorgen dat die apparaten compatibel en beveiligd zijn, is een belangrijk aandachtspunt in de oplossing voor identiteit die van een organisatie kiest voor het implementeren van geworden. In de hedendaagse digitale werkplek, [identiteit is het primaire besturingselement vlak](https://www.microsoft.com/security/technology/identity-access-management?rtc=1) van elke organisatie verplaatsen naar de cloud.

Bij het overstappen op een Azure Active Directory (Azure AD)-oplossing voor hybride identiteit, krijgen organisaties toegang tot Premiumfuncties die de productiviteit door middel van automatisering, overdracht, selfservice en eenmalige aanmelding-mogelijkheden ontgrendelen. Hiermee kunnen uw werknemers toegang krijgen tot bedrijfsbronnen vanaf elke locatie waar ze nodig hebben voor hun werk en tegelijkertijd uw IT-team om te bepalen die toegang door ervoor te zorgen dat de juiste personen de juiste toegang tot de juiste resources hebben tot stand brengen van beveiligde productiviteit te bieden.

Op basis van onze geleerde lessen, deze controlelijst van aanbevolen procedures kunt u snel implementeren aanbevolen acties voor het samenstellen een *sterke* identity foundation in uw organisatie:

* Eenvoudig verbinding maken met apps
* Het automatisch maken van één identiteit voor elke gebruiker
* Zorg ervoor dat uw gebruikers veilig
* Uw inzichten operationeel maken

## <a name="step-1---connect-to-apps-easily"></a>Stap 1: verbinding maken met apps eenvoudig

Door het verbinden van uw apps met Azure AD, kunt u de productiviteit van eindgebruikers en beveiliging te verbeteren door het inschakelen van eenmalige aanmelding (SSO) en doen inrichten van gebruikers. Door het beheer van uw apps op één plek, Azure AD, kunt u administratieve overhead te minimaliseren en een single point of besturingselement bereiken voor uw beleid voor beveiliging en naleving.

In deze sectie bevat informatie over uw opties voor het beheren van de gebruikerstoegang tot veilige externe toegang tot interne apps en de voordelen van de migratie van uw apps naar Azure AD-apps.

### <a name="make-apps-available-to-your-users-seamlessly"></a>Apps beschikbaar te maken voor uw gebruikers naadloos

Azure AD kan beheerders [toepassingen toevoegen](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal) naar de galerie met Enterprise-toepassingen in de [Azure-portal](https://portal.azure.com/). Toepassingen toevoegen aan de galerie met Enterprise-toepassingen, maakt het eenvoudiger voor u om toepassingen configureren voor het gebruik van Azure AD als id-provider. Ook kunt u toegang tot de toepassing met beleid voor voorwaardelijke toegang voor gebruikers beheren en eenmalige aanmelding (SSO) configureren voor toepassingen, zodat gebruikers niet hoeven in te voeren hun wachtwoorden herhaaldelijk en worden automatisch wordt aangemeld bij zowel on-premises en cloud-gebaseerde toepassingen.

Zodra toepassingen zijn toegevoegd aan de Azure AD-galerie, kunnen gebruikers apps die aan hen zijn toegewezen en zoeken en andere apps aanvragen indien nodig kunnen zien. Azure AD biedt [verschillende methoden](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences) gebruikers toegang hebben tot hun apps:

* Toegangsvenster / mijn Apps
* Startprogramma voor Office 365-Apps
* Directe aanmelding bij federatieve apps
* Directe aanmelding koppelingen

Zie voor meer informatie over de gebruikerstoegang tot apps, **stap 3: uw gebruikers meer mogelijkheden bieden** in dit artikel.

### <a name="migrate-apps-from-active-directory-federation-services-to-azure-ad"></a>Apps uit Active Directory Federatieservices migreren naar Azure AD

Migratie enkele aanmeldings-configuratie van Active Directory Federation Services (ADFS) naar Azure AD kunt aanvullende mogelijkheden voor beveiliging, een consistente beheer en samenwerking. Voor optimale resultaten, is het raadzaam dat u uw apps van AD FS naar Azure AD migreren. Voortzetten van uw toepassing verificatie en autorisatie te maken in Azure AD biedt u de volgende voordelen:

* Kosten beheren
* Risico's beheren
* De productiviteit verhogen
* Naleving en governance-adressering

Zie voor meer informatie, de [uw toepassingen migreren naar Azure Active Directory](https://aka.ms/migrateapps/whitepaper) technisch document.

### <a name="enable-secure-remote-access-to-apps"></a>Veilige externe toegang tot apps inschakelen

[Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-application-proxy) biedt een eenvoudige oplossing voor organisaties on-premises toepassingen naar de cloud voor externe gebruikers die toegang nodig tot interne apps op een veilige manier publiceren. Na een single sign-on bij Azure AD, kunnen gebruikers toegang tot van zowel cloud en on-premises toepassingen via externe URL's of een interne applicatie-portal.

Azure AD Application Proxy biedt de volgende voordelen:

* Uitbreiding van Azure AD met on-premises bronnen
  * Schaalbare beveiliging en bescherming
  * Functies zoals voorwaardelijke toegang en multi-Factor Authentication die gemakkelijk is om in te schakelen
* Er zijn geen onderdelen in het perimeternetwerk, zoals VPN- en traditionele reverse proxy-oplossingen
* Er is geen binnenkomende verbindingen die vereist zijn
* Eenmalige aanmelding (SSO) alle apparaten, middelen en apps in de cloud en on-premises
* Biedt uitgebreide mogelijkheden met eindgebruikers om productief te zijn op elk gewenst moment en overal

### <a name="discover-shadow-it-with-microsoft-cloud-app-security"></a>Schaduw-IT detecteren met Microsoft Cloud App Security

In moderne ondernemingen zijn IT-afdelingen vaak niet op de hoogte van alle cloudtoepassingen die worden gebruikt door de gebruikers hun werk te doen. Wanneer IT-beheerders gevraagd hoeveel cloud apps ze denkt dat hun werknemers gebruiken, op gemiddelde ze bijvoorbeeld 30 of 40. In werkelijkheid is het gemiddelde meer dan 1000 afzonderlijke apps wordt gebruikt door werknemers in uw organisatie. 80% van de werknemers niet-goedgekeurde apps dat niemand heeft bekeken en mogelijk niet compatibel zijn met het beleid voor beveiliging en naleving gebruiken.

[Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) (MCAS) kunt u nuttige apps die populair bij de gebruikers zijn identificeren dat deze kan goedkeuren en toevoegen aan de galerie met Enterprise-toepassingen, zodat gebruikers van de mogelijkheden, zoals eenmalige aanmelding en voorwaardelijke toegang profiteren.

*"** Cloud App Security** kunnen we ervoor te zorgen dat onze mensen zijn correct gebruik van onze cloud en SaaS-toepassingen, op een manier die ondersteuning bieden voor de fundamentele beveiligingsbeleid die helpen beschermen Accenture." *--- [John Blasi, Director, informatiebeveiliging, Accenture beheren](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security)

Naast het detecteren van schaduw-IT, MCAS kunt ook bepalen het risiconiveau van apps, niet-geautoriseerde toegang tot bedrijfsgegevens lekken van gegevens mogelijk en andere security intrinsieke risico's van de toepassingen te voorkomen.

## <a name="step-2---establish-one-identity-for-every-user-automatically"></a>Stap 2: het automatisch maken van één identiteit voor elke gebruiker

Bijeenbrengt on-premises en cloudgebaseerde directory's in een hybride identiteitsoplossing voor Azure AD kunt u uw bestaande on-premises Active Directory-investeringen gebruiken door in te richten van uw bestaande identiteiten in de cloud. De oplossing synchroniseert on-premises identiteiten met Azure AD, terwijl IT houdt de on-premises Active Directory uitgevoerd met een bestaande governance-oplossingen als de primaire bron van de waarheid voor identiteiten. Van Microsoft Azure AD hybride identiteitsoplossing omvat on-premises en cloud-gebaseerde mogelijkheden, het maken van een gemeenschappelijke gebruikersidentiteit in voor verificatie en autorisatie voor alle resources, ongeacht hun locatie.

Uw on-premises directory's integreren met Azure AD zijn uw gebruikers productiever en voorkomt dat gebruikers via meerdere accounts voor apps en services door te geven van een gemeenschappelijke identiteit voor toegang tot zowel cloud en on-premises resources. Met behulp van meerdere accounts is een knelpunt voor eindgebruikers en IT hetzelfde. Vanuit het perspectief van een eindgebruiker, die meerdere accounts betekent dat meerdere wachtwoorden onthouden. Veel gebruikers gebruiken om dit te voorkomen, hetzelfde wachtwoord voor elk account, beschadigd vanuit het beveiligingsoogpunt is. Vanuit het perspectief van een IT leiden opnieuw gebruiken vaak tot meer wachtwoord opnieuw instellen van wachtwoorden en kosten bij de helpdesk samen met de eindgebruiker klachten.

Azure AD Connect is het hulpprogramma dat wordt gebruikt om te synchroniseren van uw on-premises identiteiten met Azure AD, die vervolgens kunnen worden gebruikt voor toegang tot cloudtoepassingen. Zodra de identiteiten in Azure AD, kunnen ze inrichten voor SaaS-toepassingen, zoals Salesforce of Concur.

In deze sectie wij in de lijst aanbevelingen voor hoge beschikbaarheid, moderne verificatie voor de cloud, en uw on-premises-voetafdruk verminderen.

> [!NOTE]
> Als u meer informatie over Azure AD Connect wilt, Zie [wat is Azure AD Connect Sync?](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis)

### <a name="set-up-a-staging-server-for-azure-ad-connect-and-keep-it-up-to-date"></a>Een staging-server instellen voor Azure AD Connect en deze up-to-date te houden

Azure AD Connect speelt een belangrijke rol bij het inrichtingsproces. Als de synchronisatieserver voor een bepaalde reden offline gaat, worden de wijzigingen naar on-premises wordt niet bijgewerkt in de cloud en toegangsproblemen veroorzaken voor gebruikers. Het is belangrijk voor het definiëren van een failoverstrategie kunnen beheerders snel synchronisatie hervatten nadat de synchronisatieserver offline gaat.

Voor maximale beschikbaarheid in het geval uw primaire Azure AD Connect-server offline is, wordt het aanbevolen dat u een afzonderlijke implementeert [testserver](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-staging-server) voor Azure AD Connect. Implementatie van een server kan de beheerder ' ' de tijdelijke server promoveert naar productie door een eenvoudige configuratie-switch. Met een stand-by-server die is geconfigureerd in de faseringsmodus kunt u om te testen en implementeren van nieuwe wijzigingen in de configuratie en introduceren een nieuwe server als buiten bedrijf stellen van de oude heeft.

> [!TIP]
> Azure AD Connect wordt regelmatig bijgewerkt. Het wordt daarom ten zeerste aanbevolen dat u de server met tijdelijke bestanden actueel houden om te profiteren van de verbeteringen in queryprestaties, oplossingen voor problemen en nieuwe mogelijkheden waarmee elke nieuwe versie.

### <a name="enable-cloud-authentication"></a>Cloud-verificatie inschakelen

Organisaties met on-premises Active Directory moeten de directory uitbreiden naar Azure AD met Azure AD Connect en de juiste verificatiemethode configureren. [De juiste verificatiemethode kiezen](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) voor uw organisatie is de eerste stap in uw reis apps verplaatsen naar de cloud. Het is een essentieel onderdeel, aangezien deze beheert de toegang tot alle cloudgegevens en resources.

De eenvoudigste en aanbevolen methode voor het inschakelen van cloud-verificatie voor on-premises directory-objecten in Azure AD is om in te schakelen [wachtwoord-Hashsynchronisatie](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) (WHS). U kunt ook sommige organisaties overwegen inschakelen [Pass through-verificatie](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) (PTA).

Of u WHS of PTA kiest, vergeet niet om in te schakelen [naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso) waarmee gebruikers toegang hebben tot cloud-apps voortdurend zonder te hoeven hun gebruikersnaam en wachtwoord in de app bij het gebruik van Windows 7 en 8-apparaten in uw bedrijf netwerk. Zonder eenmalige aanmelding, moeten gebruikers toepassingsspecifieke wachtwoorden en meld u onthouden bij elke toepassing. Evenzo, IT-personeel moet maken en bijwerken van gebruikersaccounts voor elke toepassing, zoals Office 365, Box en Salesforce. Gebruikers moeten hun wachtwoord onthouden, plus de tijd aan te melden bij elke toepassing. Een gestandaardiseerde eenmalige aanmelding mechanisme voor de hele onderneming is het essentieel is voor de beste gebruikerservaring, beperking van risico's, de mogelijkheid om te rapporteren en governance.

Voor organisaties die al met AD FS of een andere on-premises verificatieprovider, verplaatsen naar Azure AD als id-provider vermindert de complexiteit en de beschikbaarheid te verbeteren. Tenzij u specifieke gebruiksvoorbeelden voor Federatie hebt, wordt aangeraden de migratie van federatieve verificatie naar de PHS en naadloze eenmalige aanmelding of PTA en naadloze eenmalige aanmelding om te profiteren van de voordelen van een lagere on-premises-footprint en de flexibiliteit van de cloud met verbeterde gebruikerservaring. Zie voor meer informatie, [migreren van Federatie naar wachtwoord-hashsynchronisatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/plan-migrate-adfs-password-hash-sync).

### <a name="enable-automatic-deprovisioning-of-accounts"></a>Automatische inrichting van accounts inschakelen

Inschakelen van geautomatiseerde inrichting en het opheffen van inrichting voor uw toepassingen is de beste strategie voor van bestuur van de levenscyclus van identiteiten in verschillende systemen. Azure AD ondersteunt [geautomatiseerde, op beleid gebaseerde inrichting en ongedaan maken van inrichting](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-automatic-user-provisioning-portal) van gebruikersaccounts die tot een verscheidenheid aan populaire SaaS-toepassingen zoals ServiceNow en Salesforce en anderen die implementeren de [SCIM 2.0 protocol](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups). In tegenstelling tot traditionele inrichting oplossingen waarvoor aangepaste code of handmatig uploaden van CSV-bestanden, de provisioning-service wordt gehost in de cloud en functies vooraf geïntegreerde connectors die kunnen worden ingesteld en worden beheerd met behulp van de Azure portal. Een belangrijk voordeel van automatische inrichting is het helpt uw organisatie beveiligen door het direct identiteit van gebruikers verwijderen uit sleutel SaaS-apps wanneer ze de organisatie verlaten.

Zie voor meer informatie over het inrichten van automatische gebruikersaccounts en hoe het werkt, [automatiseren van Gebruikersinrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

## <a name="step-3---empower-your-users-securely"></a>Stap 3: Zorg ervoor dat uw gebruikers veilig

In digitale vandaag de dag is het belangrijk om te verdelen over beveiliging met productiviteit. Echter, eindgebruikers vaak push terug op beveiligingsmaatregelen die vertragingen in hun productiviteit en de toegang tot cloud-apps. Om te helpen dit, biedt Azure AD selfservice mogelijkheden waarmee gebruikers productief blijven terwijl de administratieve overhead worden geminimaliseerd.

Deze sectie vindt u aanbevelingen voor het verwijderen van problemen van uw organisatie door uw gebruikers en tegelijkertijd de kosten waakzaam.

### <a name="enable-self-service-password-reset-for-all-users"></a>Selfservice voor wachtwoordherstel inschakelen voor alle gebruikers

Azure [Self-service voor wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) (SSPR) biedt een eenvoudige manier voor IT-beheerders kunnen gebruikers opnieuw instellen en optimaal te profiteren van hun wachtwoord of account zonder tussenkomst van de beheerder. Het systeem biedt gedetailleerde rapporten zodat u kunt volgen wanneer gebruikers het systeem openen. U ontvangt ook meldingen om u te waarschuwen over misbruik.

Azure AD ontgrendelt standaard accounts bij het uitvoeren van een wachtwoord opnieuw instellen. Echter, wanneer u Azure AD Connect inschakelen [integratie van on-premises](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#on-premises-integration), hebt u ook de mogelijkheid voor het scheiden van deze twee bewerkingen, die gebruikers kunnen hun account ontgrendelen zonder dat het wachtwoord opnieuw in te schakelen.

### <a name="ensure-all-users-are-registered-for-mfa-and-sspr"></a>Zorg ervoor dat alle gebruikers zijn geregistreerd voor MFA en Self-service voor Wachtwoordherstel

Azure biedt rapporten die door u en uw organisatie kunnen worden gebruikt om te controleren of gebruikers zijn geregistreerd voor MFA en Self-service voor Wachtwoordherstel. Gebruikers die nog niet hebt geregistreerd, moet mogelijk worden op het proces te gaan.

De MFA [aanmeldingenrapport](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-reporting) bevat informatie over het gebruik van MFA en geeft u inzicht in hoe MFA in uw organisatie werkt. Toegang tot aanmelden activiteit (en controles en risicogebeurtenissen) voor Azure AD is van cruciaal belang voor het oplossen van problemen, gebruiksanalyse en forensische onderzoeken.

Evenzo, de [Self-service wachtwoordbeheer rapport](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-reporting) kan worden gebruikt om te bepalen wie heeft (of nog niet) geregistreerd voor self-service voor Wachtwoordherstel.

### <a name="self-service-app-management"></a>Self-service app-beheer

Voordat uw gebruikers zelf toepassingen in hun Toegangsvenster detecteren kunnen, moet u om in te schakelen [toegang tot Self-servicetoepassingen](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-manage-self-service-access) op alle toepassingen die u wilt toestaan dat gebruikers zelf detecteren en aanvragen toegang tot. Toegang tot Self-servicetoepassingen is een uitstekende manier om toestaan dat gebruikers zelf toepassingen te detecteren en de bedrijfsgroep om goed te keuren toegang tot deze toepassingen toestaan. U kunt de bedrijfsgroep voor het beheren van de referenties die zijn toegewezen aan gebruikers voor [wachtwoord eenmalige aanmelding op toepassingen](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery#configure-the-application-for-password-single-sign-on) rechtstreeks vanuit hun toegangsvensters.

### <a name="self-service-group-management"></a>Groepsbeheer via selfservice

Gebruikers toewijzen aan toepassingen beste toegewezen bij het gebruik van groepen, omdat ze hoge mate van flexibiliteit en de mogelijkheid om te beheren op schaal toestaan:

* Met dynamisch groepslidmaatschap op basis van kenmerk
* Overdracht naar de app-eigenaren

Azure AD biedt de mogelijkheid om toegang tot bronnen met behulp van beveiligingsgroepen en Office 365-groepen te beheren. Deze groepen kunnen worden beheerd door de Groepseigenaar van een die u kunt goedkeuren of weigeren van aanvragen voor groepslidmaatschap en het beheer van groepslidmaatschap overdragen. Ook wel [selfservicegroepsbeheer](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management), deze functie, bespaart u tijd doordat eigenaren van groepen die een beheerdersrol maken en beheren van groepen zonder afhankelijk zijn van beheerders voor het afhandelen van hun aanvragen zijn niet toegewezen.

## <a name="step-4---operationalize-your-insights"></a>Stap 4: uw inzichten operationeel maken

Controle en logboekregistratie van beveiligingsgerelateerde gebeurtenissen en gerelateerde waarschuwingen zijn essentiële onderdelen van een doeltreffende strategie om ervoor te zorgen dat gebruikers productief blijven en uw organisatie beveiligd is. Beveiligingslogboeken en rapporten kunt vraag beantwoorden zoals:

* Gebruikt u wat u betalen?
* Is er iets verdachte of schadelijke er in mijn tenant?
* Die werd beïnvloed tijdens een beveiligingsincident?

Beveiligingslogboeken en rapporten bieden u een elektronische record van verdachte activiteiten en kunt u patronen die op pogingen tot of geslaagde externe indringingstests van het netwerk en interne aanvallen duiden kunnen detecteren. U kunt controle gebruiken om te controleren van gebruikersactiviteiten, document naleving van regelgeving, forensische analyse en meer doen. Waarschuwingen geven meldingen van beveiligingsgebeurtenissen.

### <a name="assign-least-privileged-admin-roles-for-operations"></a>Minste bevoegde beheerdersrollen voor bewerkingen toewijzen

Als u denkt over de benadering van bewerkingen dat, zijn er enkele niveaus van beheer te houden. Het eerste niveau plaatst de last van beheer op uw globale beheerder (s). Altijd met behulp van de rol globale beheerder, mogelijk geschikt voor kleinere bedrijven. Maar voor grote organisaties met helpdeskmedewerkers en beheerders die verantwoordelijk zijn voor specifieke taken toewijzen van de rol van globale beheerder kan een veiligheidsrisico inhouden omdat deze personen de mogelijkheid biedt voor het beheren van taken die hierboven en buiten wat ze moeten geschikt zijn voor het uitvoeren van.

U kunt in dit geval moet rekening houden met het volgende niveau van beheer. Met behulp van Azure AD, kunt u opgeven dat eindgebruikers als 'beperkte administrators' die taken in minder bevoegdheden functies kunnen beheren. Bijvoorbeeld, u toewijzen uw help helpdesk-medewerkers de [beveiligingslezer](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#security-reader) rol zodat ze beschikken over de mogelijkheid voor het beheren van beveiligingsfuncties met alleen-lezen toegang. Of misschien is het zinvol om toe te wijzen de [authentication-beheerder](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#authentication-administrator) personen om ze te bieden de mogelijkheid de referenties niet-wachtwoord opnieuw instellen of lezen of Azure Service Health-rol.

Zie voor meer informatie, [rol beheerdersmachtigingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

### <a name="monitor-hybrid-components-azure-ad-connect-sync-ad-fs-using-azure-ad-connect-health"></a>Hybride-onderdelen (Azure AD Connect-synchronisatie, AD FS) bewaken met behulp van Azure AD Connect Health

Azure AD Connect en AD FS zijn belangrijke onderdelen die kunnen mogelijk gaan wellicht levenscyclusbeheer en -verificatie en uiteindelijk tot onderbrekingen leiden. Daarom moet u Azure AD Connect Health implementeren voor bewaking en rapportage van deze onderdelen.

Ga voor meer informatie, Lees [Monitor AD FS met Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs).

### <a name="use-azure-monitor-to-collect-data-logs-for-analytics"></a>Azure Monitor gebruiken voor het verzamelen van Logboeken van de gegevens voor analyse

[Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) is een geïntegreerde bewaking portal voor alle Azure AD-Logboeken, waarmee u veel inzicht, geavanceerde analyses en slimme machine learning. U kunt metrische gegevens en logboeken van de portal en via API's om meer inzicht in de status en prestaties van uw resources verbruiken met Azure Monitor. Hiermee kunt één venster inzicht glas ervaring binnen de portal tijdens het inschakelen van een breed scala aan productintegraties via API's en gegevens exportopties die ondersteuning bieden voor traditionele SIEM-systemen van derden. Azure Monitor biedt ook de mogelijkheid om te configureren van regels voor waarschuwingen om te worden geïnformeerd of geautomatiseerde acties uitvoeren op problemen die invloed hebben op uw resources.

![Azure Monitor](./media/four-steps/image1.png)

### <a name="create-custom-dashboards-for-your-leadership-and-your-day-to-day"></a>Maak aangepaste dashboards voor uw leiderschap en uw dag tot dag

Organisaties die niet beschikken over een SIEM-oplossing kunnen downloaden de [Power BI Content Pack](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-power-bi-content-pack) voor Azure AD. De Power BI content pack bevat vooraf gemaakte rapporten om u te helpen begrijpen hoe uw gebruikers vast en gebruikt u Azure AD-functies, waarmee u inzicht in alle activiteiten in uw directory. U kunt ook uw eigen maken [aangepast dashboard](https://docs.microsoft.com/power-bi/service-dashboards) en delen met uw team leiderschap op het gebied voor het rapporteren van dagelijkse activiteiten. Dashboards zijn een uitstekende manier om te controleren van uw bedrijf en alle van de belangrijkste metrische gegevens in een oogopslag zien. De visualisaties op een dashboard kunnen afkomstig zijn van één onderliggende gegevensset of veel, en één onderliggende rapporten. Een dashboard combineert on-premises en cloudgegevens, biedt een geconsolideerde weergave, ongeacht waar de gegevens zich bevinden.

![Aangepaste Power BI-dashboard](./media/four-steps/image2.png)

### <a name="understand-your-support-call-drivers"></a>Inzicht in uw aanroep stuurprogramma's voor ondersteuning

Wanneer u een hybride identiteitsoplossing zoals wordt beschreven in dit artikel implementeert, moet u uiteindelijk u ziet een vermindering in uw ondersteuningsaanvragen. Veelvoorkomende problemen zoals vergeten wachtwoorden en accountvergrendeling af worden verholpen door het implementeren van Azure Self-service voor wachtwoord opnieuw instellen, tijdens het inschakelen van toegang tot Self-servicetoepassingen kunnen gebruikers zelf detecteren en om toegang tot toepassingen zonder te vragen op de IT-afdeling.

Als u een vermindering van ondersteuningsaanvragen niet ziet, wordt aangeraden dat u de aanroep van stuurprogramma's voor ondersteuning in een poging om te bevestigen analyseren of SSPR of toegang tot Self-servicetoepassingen correct is geconfigureerd of als er andere nieuwe problemen die systematisch worden kunnen zijn Wij hebben oog.

*"In onze reis digitale transformatie en we nodig hebben een betrouwbaar identiteits- en access management-provider te vergemakkelijken naadloze toch beveiligde integratie tussen de VS, partners en providers van cloudservices, voor een effectieve ecosysteem; Azure AD is de beste optie biedt ons de benodigde mogelijkheden en zichtbaarheid die we om te detecteren en reageren op risico's."* --- [Yazan Almasri, Global Information Security Director, Aramex](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en)

### <a name="monitor-your-usage-of-apps-to-drive-insights"></a>Uw gebruik van apps tot inzichten controleren

Naast het detecteren van schaduw-IT, bewaking van app-gebruik in uw organisatie met [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) kan helpen uw organisatie terwijl u zich verplaatst om te profiteren van de belofte van cloud-toepassingen. Het kan helpen zorgen ervoor dat u in beheer van uw activa met verbeterde zichtbaarheid in de activiteit en verhoogt de beveiliging van essentiële gegevens in cloudtoepassingen. Het bewaken van appgebruik in uw organisatie met behulp van MCAS, kunt u de volgende vragen beantwoorden:

* Welke niet-goedgekeurde apps zijn werknemers gebruiken voor het opslaan van gegevens in?
* Waar en wanneer gevoelige gegevens worden opgeslagen in de cloud?
* Wie is toegang tot gevoelige gegevens in de cloud?

*"Met Cloud App Security, we kunnen snel afwijkingen te vinden en actie ondernemen."* --- [Eric LePenske, Senior Manager, Information Security, Accenture](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security)

## <a name="summary"></a>Samenvatting

Er zijn veel aspecten voor het implementeren van een hybride identiteitsoplossing, maar deze controlelijst vier stappen ziet u een snel uitvoeren van een infrastructuur voor identiteiten die kan gebruikers meer productiviteit en veiligheid.

* Eenvoudig verbinding maken met apps
* Het automatisch maken van één identiteit voor elke gebruiker
* Zorg ervoor dat uw gebruikers veilig
* Uw inzichten operationeel maken

We hopen dat dit document is een handig overzicht voor het tot stand brengen van een sterke identiteit basis voor uw organisatie.

## <a name="identity-checklist"></a>Controlelijst voor identiteit

Het is raadzaam dat u de volgende controlelijst voor naslaginformatie afdrukken als u uw overstap naar een meer solide basis van identiteit in uw organisatie begint.

### <a name="today"></a>Vandaag

|Klaar?|Item|
|:-|:-|
||Test Self - Service wachtwoord opnieuw instellen (SSPR) voor een groep|
||Hybride-onderdelen met behulp van Azure AD Connect Health bewaken|
||Minste bevoegde beheerdersrollen voor opnieuw toewijzen|
||Schaduw-IT detecteren met Microsoft Cloud App Security|
||Azure Monitor gebruiken voor het verzamelen van Logboeken van de gegevens voor analyse|

### <a name="next-two-weeks"></a>Volgende twee weken

|Klaar?|Item|
|:-|:-|
||Een app beschikbaar maken voor uw gebruikers|
||Azure AD wordt ingericht voor de keuze van een SaaS-app testen|
||Een staging-server instellen voor Azure AD Connect en deze up-to-date te houden|
||Beginnen met het migreren van apps van AD FS met Azure AD|
||Maak aangepaste dashboards voor uw leiderschap en uw dag tot dag|

### <a name="next-month"></a>Volgende maand

|Klaar?|Item|
|:-|:-|
||Uw gebruik van apps tot inzichten controleren|
||Test veilige externe toegang tot apps|
||Zorg ervoor dat alle gebruikers zijn geregistreerd voor MFA en Self-service voor Wachtwoordherstel|
||Cloud-verificatie inschakelen|

### <a name="next-three-months"></a>Komende drie maanden

|Klaar?|Item|
|:-|:-|
||Self-service app-beheer inschakelen|
||Schakel het beheer van selfservicegroepen|
||Uw gebruik van apps tot inzichten controleren|
||Inzicht in uw aanroep stuurprogramma's voor ondersteuning|

## <a name="next-steps"></a>Volgende stappen

Leer hoe u uw beveiligingsstrategie met behulp van de mogelijkheden kunt verhogen van de Azure Active Directory en deze vijf stappen controlelijst - [vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten](https://aka.ms/securitysteps).

Leer hoe de identiteitsfuncties voor in Azure AD kunt u versnel uw overgang naar de cloud onder beheer door op te geven van de oplossingen en mogelijkheden waarmee organisaties snel vaststellen en meer van hun identiteitsbeheer van traditionele verplaatsen on-premises systemen naar Azure AD - [hoe Azure AD biedt gelden voor Cloudbeheer voor On-Premises Workloads](https://aka.ms/cloudgoverned).
