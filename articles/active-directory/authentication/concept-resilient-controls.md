---
title: Maken van een strategie voor flexibele toegang beheren met Azure Active Directory
description: Dit document bevat informatie over strategieën een organisatie moet nemen om te bieden van flexibiliteit om het risico van de vergrendeling van het tijdens onvoorziene onderbrekingen
services: active-directory
author: martincoetzer
manager: mtillman
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 12/19/2018
ms.author: martincoetzer
ms.openlocfilehash: 34d60d82ff70ecf683b955b8b796b5d3269df53c
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54401908"
---
# <a name="create-a-resilient-access-control-management-strategy-with-azure-active-directory"></a>Maken van een strategie voor flexibele toegang beheren met Azure Active Directory

>[!NOTE]
> De informatie in dit document geeft de huidige weergave van Microsoft Corporation op de problemen beschreven op de datum van publicatie. Omdat Microsoft reageren moet op veranderingen in de markt, moeten niet worden geïnterpreteerd als een toezegging van Microsoft en Microsoft de nauwkeurigheid van de informatie die na de datum van publicatie kan niet garanderen.

Organisaties die afhankelijk van een enkele toegangsbeheer, zoals multi-factor authentication (MFA) of een enkel netwerklocatie zijn voor het beveiligen van hun IT-systemen zijn vatbaar voor fouten bij de toegang tot hun apps en resources als die één toegangsbeheer niet beschikbaar of niet juist geconfigureerd. Bijvoorbeeld, kan een natuurlijke ramp resulteren in het ontbreken van grote segmenten van telecommunicatie-infrastructuur of bedrijfsnetwerken. Een dergelijke onderbreking kan verhinderen dat eindgebruikers en beheerders kunnen aanmelden.

Dit document bevat informatie over strategieën een organisatie moet nemen om te bieden van flexibiliteit om het risico van de vergrendeling van het tijdens onvoorziene onderbrekingen met de volgende scenario's:

 1. Organisaties hun flexibiliteit om het risico van vergrendeling kunnen verhogen **voordat een onderbreking** door het implementeren van risicobeperking strategieën of Noodscenario's.
 2. Organisaties toegang houden tot apps en resources die ze ervoor kiezen **tijdens een onderbreking** door risicobeperking strategieën en Noodscenario's in plaats.
 3. Organisaties moeten ervoor zorgen ze de gegevens, zoals Logboeken, behouden **na een onderbreking** en voordat ze een voorwaardelijke ze geïmplementeerd terugdraaien.
 4. Organisaties die nog niet hebt geïmplementeerd ter preventie strategieën of alternatieve plannen kunnen mogelijk zijn voor het implementeren van **noodgevallen opties** te bekommeren om de onderbreking.

## <a name="key-guidance"></a>Belangrijke richtlijnen

Er zijn vier belangrijke takeaways in dit document:

* Beheerder accountvergrendeling voorkomen met behulp van accounts voor toegang in noodgevallen.
* Implementeren met behulp van voorwaardelijke toegang (CA) in plaats van MFA per gebruiker MFA.
* Vergrendeling van het gebruiker beperken met behulp van meerdere besturingselementen voor voorwaardelijke toegang (CA).
* Beperk gebruiker vergrendeling van het door het inrichten van meerdere verificatiemethoden of -equivalenten voor elke gebruiker.

## <a name="before-a-disruption"></a>Voordat u een onderbreking

Een onderbreking van de werkelijke beperkende, moet de primaire focus van een organisatie in omgaan met toegang tot besturingselement oplossen die zich kunnen voordoen. Beperkende omvat planning voor een gebeurtenis plus uitvoeringsstrategieën zodat u zeker dat besturingselementen voor toegang en bewerkingen worden niet beïnvloed tijdens onderbrekingen.

### <a name="why-do-you-need-resilient-access-control"></a>Waarom moet u flexibele toegangsbeheer?

 Identiteit is de controlelaag van gebruikers met toegang tot apps en resources. Uw identiteitssysteem bepaalt welke gebruikers en onder welke omstandigheden, zoals besturingselementen voor toegang of verificatievereisten, gebruikers toegang krijgen tot de toepassingen. Wanneer een of meer verificatie en vereisten voor toegangsbeheer zijn niet beschikbaar voor gebruikers worden geverifieerd vanwege onvoorziene omstandigheden, kunnen organisaties een of beide van de volgende problemen optreden:

* **Beheerder vergrendeling:** Beheerders kunnen niet de tenant of services beheren.
* **Vergrendeling van de gebruiker:** Gebruikers geen toegang tot apps of resources.

### <a name="administrator-lockout-contingency"></a>Beheerder accountvergrendeling noodplan voor

Als u wilt ontgrendelen beheerderstoegang tot uw tenant, moet u de accounts voor toegang in noodgevallen maken. Deze accounts voor toegang in noodgevallen, ook wel bekend als *nood* -accounts, kunt u toegang tot de configuratie van Azure AD beheren als normale bevoegd account toegang procedures zijn niet beschikbaar. Ten minste twee accounts voor toegang in noodgevallen moeten worden gemaakt na de [aanbevelingen voor toegang in noodgevallen]( https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access).

### <a name="mitigating-user-lockout"></a>Beperkende gebruiker accountvergrendeling

 Gebruiken om het risico van gebruiker vergrendeling van het beleid voor voorwaardelijke toegang met meerdere besturingselementen, zodat gebruikers een keuze uit hoe ze toegang apps en resources tot krijgen. Door middel van een gebruiker de keuze tussen, aanmelden, bijvoorbeeld met MFA **of** aanmelden vanaf een beheerd apparaat **of** heeft van de gebruiker aanmelden vanaf het bedrijfsnetwerk bevinden, als een van de besturingselementen voor toegang niet beschikbaar is andere opties om door te gaan werken.

#### <a name="microsoft-recommendations"></a>Aanbevelingen van Microsoft

De volgende besturingselementen voor toegang in uw bestaande beleidsregels voor voorwaardelijke toegang voor organisatie opnemen:

1. Inrichten van meerdere verificatiemethoden voor elke gebruiker die afhankelijk van verschillende communicatiekanalen, bijvoorbeeld de Microsoft Authenticator-app (internet-gebaseerd), een OATH-token (gegenereerde op apparaat) en een SMS (toestellen voor telefonie zijn).
2. Implementeer Windows Hello voor bedrijven op Windows 10-apparaten om te voldoen aan vereisten voor MFA rechtstreeks vanuit aanmelden van apparaten.
3. Gebruik vertrouwde apparaten via [Hybrid van Azure AD Join](https://docs.microsoft.com/azure/active-directory/devices/overview) of [door Microsoft Intune beheerde apparaten](https://docs.microsoft.com/intune/planning-guide). Vertrouwde apparaten wordt de gebruikerservaring verbeteren, omdat het vertrouwde apparaat zelf kan voldoen aan de vereisten voor sterke verificatie van beleid zonder een MFA-controle voor de gebruiker. MFA wordt vervolgens zijn vereist wanneer een nieuw apparaat registreren en bij het openen van apps of bronnen van niet-vertrouwde apparaten.
4. Gebruik Azure AD identity protection risico's gebaseerd beleid die voorkomen toegang dat wanneer de gebruiker of de aanmelding lopen in plaats van vaste MFA-beleid is.

>[!NOTE]
> Risico's gebaseerd beleid vereist dat [Azure AD Premium P2](https://azure.microsoft.com/pricing/details/active-directory/) licenties.

In het volgende voorbeeld wordt de beleidsregels die u moet maken voor een robuuste toegangsbeheer voor de gebruiker toegang tot hun apps en resources beschreven. In dit voorbeeld moet u een beveiligingsgroep **AppUsers** met de doelgebruikers u toegang wilt verlenen aan een groep met de naam **CoreAdmins** met de core-beheerders en een groep met de naam  **EmergencyAccess** met de accounts voor toegang in noodgevallen.
In dit voorbeeld beleidsset verleent geselecteerde gebruikers in **AppUsers**, toegang tot geselecteerde apps als ze verbinding vanaf een vertrouwd apparaat maakt of het bieden van sterke verificatie, bijvoorbeeld MFA. Deze uitsluit noodgevallen accounts en core beheerders.

**CA-beleid voor risicobeperking instellen:**

* Beleid 1: Toegang blokkeren tot personen buiten de doelgroepen
  * Gebruikers en groepen: Alle gebruikers bevat. AppAccess, CoreAdmins en EmergencyAccess uitsluiten
  * Cloud-Apps: Alle apps opnemen
  * Voorwaarden: (Geen)
  * Controle van verlenen: Blok
* Beleid 2: Toegang verlenen tot AppUsers MFA of vertrouwd apparaat vereisen.
  * Gebruikers en groepen: AppUsers bevatten. CoreAdmins en EmergencyAccess uitsluiten
  * Cloud-Apps: Alle apps opnemen
  * Voorwaarden: (Geen)
  * Controle van verlenen: Toegang verlenen, meervoudige verificatie vereisen, vereisen dat het apparaat compatibel is. Voor meerdere besturingselementen: Een van de geselecteerde besturingselementen vereisen.

### <a name="contingencies-for-user-lockout"></a>Gebeurtenissen voor gebruiker accountvergrendeling

U kunt ook kunt uw organisatie ook beleid voor onvoorziene gebeurtenissen maken. Onvoorziene gebeurtenissen om beleid te maken, moet u criteria een afweging tussen de bedrijfscontinuïteit, operationele kosten, financiële kosten en beveiligingsrisico's definiëren. U kunt bijvoorbeeld een beleid voor onvoorziene gebeurtenissen activeren alleen voor een subset van gebruikers voor een subset van apps, voor een subset van clients, of van een subset van locaties. Beleid voor onvoorziene gebeurtenissen, beheerders en eindgebruikers toegang krijgt tot apps en resources, tijdens een onderbreking wanneer er geen methode risicobeperking is geïmplementeerd.
Inzicht krijgen in de blootstelling tijdens een onderbreking vermindert het risico en is een belangrijk onderdeel van uw planningsproces. Voor het maken van uw plan voor onvoorziene gebeurtenissen, moet u eerst de volgende zakelijke vereisten van uw organisatie bepalen:

1. Bepaal uw essentiële apps vooraf: Wat zijn de apps die u toegang tot, zelfs met een lagere risico's / beveiligingspostuur moet geven? Een lijst van deze apps bouwen en zorg ervoor dat uw andere belanghebbenden (business, beveiliging, juridische, leiderschap) alle gaat ermee akkoord dat als alle toegangsbeheer is verdwenen, deze apps nog steeds blijven moeten om uit te voeren. U waarschijnlijk gaat einde van categorieën van:
  * **Categorie 1 essentiële apps** die niet beschikbaar kan niet meer dan een paar minuten, bijvoorbeeld de Apps die rechtstreeks van invloed op de omzet van de organisatie.
  * **Categorie 2 belangrijke apps** dat het bedrijf moet toegankelijk zijn binnen een paar uur.
  * **Categorie 3 met lage prioriteit apps** dat bestand is tegen een onderbreking van een paar dagen.
2. Voor apps in de categorie 1 en 2 raadt Microsoft dat u vooraf van plan bent welk type niveau van toegang dat u wilt toestaan:
  * Wilt u volledige toegang of beperkte sessie, zoals het beperken van downloads toestaan?
  * Wilt u toegang tot deel uit van de app, maar niet de hele app?
  * Wilt u de toegang tot de worker van informatie toestaan en blokkeren van toegang als beheerder, totdat het toegangsbeheer is hersteld?
3. Voor deze apps wordt ook aangeraden dat u van plan bent welke mogelijkheden van toegang die u opzettelijk wordt geopend en welke u wordt afgesloten:
  * Wilt u de browser toestaan alleen toegang en blok rich clients die offline gegevens kunnen opslaan?
  * Wilt u toegang toestaan alleen voor gebruikers binnen het bedrijfsnetwerk en houden buiten gebruikers geblokkeerd?
  * Wilt u om toegang te verlenen in bepaalde landen of regio's alleen tijdens de onderbreking?
  * Wilt u beleid voor het beleid voor onvoorziene gebeurtenissen, met name voor essentiële-apps, mislukt of slaagt als een alternatieve toegangsbeheer niet beschikbaar is?

#### <a name="microsoft-recommendations"></a>Aanbevelingen van Microsoft

Beleid voor onvoorziene gebeurtenissen voorwaardelijke toegang is een **uitgeschakeld beleid** die Azure MFA, externe MFA, risico's gebaseerde of het apparaat gebaseerde besturingselementen worden weggelaten. Vervolgens, wanneer uw organisatie besluit om het activeren van uw plan voor onvoorziene gebeurtenissen, beheerders kunnen het beleid inschakelen en uitschakelen van het normale beleid op basis van een besturingselement.

>[!IMPORTANT]
> Uitschakelen van beleidsregels die van beveiliging voor uw gebruikers afdwingen, zelfs tijdelijk beperkt u uw beveiligingspostuur zolang het plan voor onvoorziene gebeurtenissen van kracht.

* Als een onderbreking van een set van fallback-beleid te configureren op in één referentietype of één access control-mechanisme gevolgen toegang tot uw apps. Configureer een beleid uitgeschakelde status waarvoor lid worden van domein als een besturingselement, zoals een back-up voor een actief beleid waarvoor een MFA-provider van derden.
* Vermindert het risico van ongewenste actoren wachtwoorden te raden als MFA niet vereist is, door de procedures in de [wachtwoord richtlijnen](https://aka.ms/passwordguidance) technisch document.
* Implementeer [Azure AD selfservice wachtwoord opnieuw instellen (SSPR)](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) en [Protection voor Azure AD-wachtwoord](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-deploy) om ervoor te zorgen dat gebruikers niet gebruiken algemene wachtwoord en de voorwaarden die u wilt blokkeren.
* Gebruik van beleid dat de toegang in de apps beperken als een bepaalde mate van verificatie in plaats van gewoon terug te vallen voor volledige toegang niet wordt bereikt. Bijvoorbeeld:
  * Configureer een back-upbeleid waarmee de claim sessie met beperkte toegang tot Exchange en SharePoint worden verzonden.
  * Als uw organisatie Microsoft Cloud App Security gebruikt, kunt u overwegen terug te vallen op een beleid dat alleen mogelijk maakt tussen MCAS en vervolgens MCAS geeft alleen-lezen toegang, maar niet worden geüpload.
* Naam van uw beleid om ervoor te zorgen dat het is eenvoudig te vinden tijdens een onderbreking. De volgende elementen in de naam van het beleid zijn:
  * Een *label nummer* voor het beleid.
  * Tekst om weer te geven, dit beleid is alleen urgente gevallen. Bijvoorbeeld: **SCHAKEL IN NOODGEVALLEN**
  * De *onderbreking* is van toepassing op. Bijvoorbeeld: **During MFA Disruption**
  * Een *volgnummer* om de volgorde weer te geven u het beleid moet activeren.
  * De *apps* is van toepassing op.
  * De *besturingselementen* wordt toegepast.
  * De *voorwaarden* vereist is.
  
Deze naamgevingsnorm voor het beleid voor onvoorziene gebeurtenissen zijn als volgt: 

`
EMnnn - ENABLE IN EMERGENCY: [Disruption][i/n] - [Apps] - [Controls] [Conditions]
`

Het volgende voorbeeld: **Voorbeeld van de A - Contingency CA-beleid toegang herstellen naar bedrijfskritische Apps voor samenwerking**, is een typische zakelijke onvoorziene gebeurtenissen. In dit scenario, de organisatie doorgaans MFA is vereist voor alle toegang tot Exchange Online en SharePoint Online en de onderbreking van de in dit geval is dat de MFA-provider voor de klant heeft een onderbreking (of Azure MFA, on-premises MFA-provider, of externe MFA). Dit beleid beperkt deze onderbreking door gebruikers in staat specifieke gerichte toegang tot deze apps uit de vertrouwde Windows-apparaten alleen wanneer ze de app vanuit het vertrouwde bedrijfsnetwerk openen. Dit wordt ook noodgevallen accounts en core beheerders uitsluiten van deze beperkingen. De beoogde gebruikers wordt vervolgens toegang krijgen tot Exchange Online en SharePoint Online, terwijl andere gebruikers nog steeds geen toegang tot de apps die vanwege de onderbreking. In dit voorbeeld moet een benoemde netwerklocatie **CorpNetwork** en een beveiligingsgroep **ContingencyAccess** met de doelgebruikers, een groep met de naam **CoreAdmins** met de Core-beheerders en een groep met de naam **EmergencyAccess** met de accounts voor toegang in noodgevallen. De gebeurtenis moet vier beleidsregels om de gewenste toegang te bieden. 

**Voorbeeld van de A - Contingency CA-beleid toegang herstellen naar essentiële samenwerkings-Apps:**

* Beleid 1: Vereisen dat apparaten aan een domein voor Exchange en SharePoint
  * Naam: EM001 - INSCHAKELEN IN NOODGEVALLEN: Onderbreking van de MFA [1/4] - Exchange, SharePoint - vereisen Hybrid Azure AD Join
  * Gebruikers en groepen: ContingencyAccess bevatten. CoreAdmins en EmergencyAccess uitsluiten
  * Cloud-Apps: Exchange Online en SharePoint Online
  * Voorwaarden: Alle
  * Controle van verlenen: Vereisen van een domein
  * Status: Uitgeschakeld
* Beleid 2: Blok-platforms dan Windows
  * Naam: EM002 - INSCHAKELEN IN NOODGEVALLEN: MFA wordt onderbroken toegang van de [2/4] - Exchange SharePoint - blokkeren met uitzondering van Windows
  * Gebruikers en groepen: Alle gebruikers bevat. CoreAdmins en EmergencyAccess uitsluiten
  * Cloud-Apps: Exchange Online en SharePoint Online
  * Voorwaarden: Platform omvatten alle platformen voor apparaten, uitsluiten Windows
  * Controle van verlenen: Blok
  * Status: Uitgeschakeld
* 3-beleid: Netwerken dan CorpNetwork blokkeren
  * Naam: EM003 - INSCHAKELEN IN NOODGEVALLEN: MFA wordt onderbroken toegang van de [3/4] - Exchange SharePoint - blokkeren met uitzondering van het netwerk van bedrijf
  * Gebruikers en groepen: Alle gebruikers bevat. CoreAdmins en EmergencyAccess uitsluiten
  * Cloud-Apps: Exchange Online en SharePoint Online
  * Voorwaarden: Locaties zijn onder andere een willekeurige locatie, CorpNetwork uitsluiten
  * Controle van verlenen: Blok
  * Status: Uitgeschakeld
* 4-beleid: EAS expliciet blokkeren
  * Naam: EM004 - INSCHAKELEN IN NOODGEVALLEN: Onderbreking van de MFA [4/4] - Exchange - blok EAS voor alle gebruikers
  * Gebruikers en groepen: Alle gebruikers bevat
  * Cloud-Apps: Exchange Online opnemen
  * Voorwaarden: Client-apps: Exchange Active Sync
  * Controle van verlenen: Blok
  * Status: Uitgeschakeld

De volgorde van de activering:

1. ContingencyAccess, CoreAdmins en EmergencyAccess uitsluiten van het bestaande MFA-beleid. Controleer of dat een gebruiker in ContingencyAccess hebben toegang tot SharePoint Online en Exchange Online.
2. Beleid 1 inschakelen: Controleer of gebruikers op een domein apparaten die niet in de groepen uitsluiten toegang hebben tot Exchange Online en SharePoint Online zijn. Controleer of gebruikers in de groep uitsluitingen hebben toegang tot SharePoint Online en Exchange vanaf elk apparaat.
3. Beleid 2 inschakelen: Controleer of gebruikers die zich niet in de groep uitsluiten geen toegang tot SharePoint Online en Exchange Online vanaf hun mobiele apparaten. Controleer of gebruikers in de groep uitsluitingen hebben toegang tot SharePoint en Exchange vanaf elk apparaat (Windows/iOS/Android).
4. Beleid 3 inschakelen: Controleer of gebruikers die zich niet in de groepen uitsluiten heeft geen toegang tot SharePoint en Exchange uit het bedrijfsnetwerk bevinden, zelfs met een domein computer lid is. Controleer of gebruikers in de groep uitsluitingen hebben toegang tot SharePoint en Exchange met een netwerk.
5. Beleid 4 inschakelen: Controleer of dat alle gebruikers Exchange Online kunnen niet ophalen uit de systeemeigen e-mailtoepassingen op mobiele apparaten.
6. Het bestaande MFA-beleid voor SharePoint Online en Exchange Online uitschakelen.

In dit voorbeeld volgende **voorbeeld B - noodplan voor CA-beleid voor mobiele toegang tot Salesforce**, een business-app-toegang wordt hersteld. In dit scenario wordt vereist de klant doorgaans hun verkoop werknemers toegang met Salesforce (geconfigureerd voor eenmalige aanmelding op met Azure AD) van mobiele apparaten, zodat alleen toegestaan vanaf compatibele apparaten. De onderbreking van de in dit geval is dat er een probleem met het evalueren van het nalevingsbeleid voor apparaten en de onderbreking gevoelige tegelijk gebeurt er wanneer het verkoopteam behoeften toegang tot Salesforce deals sluiten. Deze beleidsregels voor onvoorziene gebeurtenissen wordt kritieke gebruikers toegang verlenen aan Salesforce vanaf een mobiel apparaat zodat ze blijven kunnen deals sluiten en het bedrijf niet worden onderbroken. In dit voorbeeld **SalesforceContingency** bevat alle Sales employees die willen toegang behouden en **SalesAdmins** bevat die nodig zijn beheerders van Salesforce.

**Voorbeeld van de B - Contingency CA-beleid:**

* Beleid 1: Iedereen in het team SalesContingency niet blokkeren
  * Naam: EM001 - INSCHAKELEN IN NOODGEVALLEN: Apparaat naleving onderbreking [1/2] - Salesforce - blok alle gebruikers behalve SalesforceContingency
  * Gebruikers en groepen: Alle gebruikers bevat. SalesAdmins en SalesforceContingency uitsluiten
  * Cloud-Apps: SalesForce.
  * Voorwaarden: Geen
  * Controle van verlenen: Blok
  * Status: Uitgeschakeld
* Beleid 2: Het verkoopteam vanaf elk platform dan mobile (voor surface area van aanvallen verkleinen) blokkeren
  * Naam: EM002 - INSCHAKELEN IN NOODGEVALLEN: Apparaat naleving onderbreking [2/2] - Salesforce - blok alle platforms met uitzondering van iOS en Android
  * Gebruikers en groepen: SalesforceContingency bevatten. SalesAdmins uitsluiten
  * Cloud-Apps: SalesForce
  * Voorwaarden: Platform omvatten alle Apparaatplatformen, uitsluiten iOS en Android
  * Controle van verlenen: Blok
  * Status: Uitgeschakeld

De volgorde van de activering:

1. SalesAdmins en SalesforceContingency uitsluiten van het bestaande apparaatnalevingsbeleid voor Salesforce. Controleer of dat een gebruiker in de groep SalesforceContingency toegang tot Salesforce.
2. Beleid 1 inschakelen: Controleer of gebruikers buiten SalesContingency heeft geen toegang tot Salesforce. Controleer of gebruikers in de SalesAdmins en SalesforceContingency hebben toegang tot Salesforce.
3. Beleid 2 inschakelen: Controleer of gebruikers in de groep SalesContigency geen toegang tot Salesforce vanaf hun laptops Windows/Mac, maar kunnen nog steeds toegang tot vanaf hun mobiele apparaten. Controleer of dat salesadmin kunt nog steeds toegang tot Salesforce vanaf elk apparaat.
4. Het bestaande apparaatnalevingsbeleid uitschakelen voor Salesforce.

### <a name="deploy-password-hash-sync-even-if-you-are-federated-or-use-pass-through-authentication"></a>Synchronisatie van wachtwoordhashes implementeren, zelfs als u een federatief zijn of Pass through-verificatie gebruiken

Vergrendeling van de gebruiker kan ook optreden als de volgende voorwaarden voldaan wordt:

- Uw organisatie gebruikmaakt van een hybride identiteitsoplossing met Pass through-verificatie of Federatie.
- Uw on-premises identity-systemen (zoals Active Directory, AD FS of een afhankelijk onderdeel) zijn niet beschikbaar. 
 
Als u meer flexibele, uw organisatie moet [synchronisatie van wachtwoordhashes inschakelen](https://docs.microsoft.com/azure/security/azure-ad-choose-authn), omdat hiermee u kunt [overschakelen op het gebruik van de synchronisatie van wachtwoordhashes](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-user-signin) als uw on-premises identity-systemen niet beschikbaar zijn.

#### <a name="microsoft-recommendations"></a>Aanbevelingen van Microsoft
 Synchronisatie van wachtwoordhashes met de Azure AD Connect-wizard, ongeacht of uw organisatie gebruikmaakt van Federatie of Pass through-verificatie inschakelen.

>[!IMPORTANT]
> Niet is verplicht om te converteren van gebruikers die zijn gefedereerd met beheerde verificatie hash Wachtwoordsynchronisatie te gebruiken.

## <a name="during-a-disruption"></a>Tijdens een onderbreking

Als u ervoor hebt gekozen voor het implementeren van een plan voor risicobeperking, zich kunt u kunt automatisch een één access control onderbreking te overbruggen. Echter, als u ervoor hebt gekozen voor het maken van een plan voor onvoorziene gebeurtenissen, kunt u zich de beleidsregels voor onvoorziene gebeurtenissen tijdens de controle-verstoring van gebruikerstoegang activeren:

1. Uw onvoorziene gebeurtenissen beleid waarmee Doelgebruikers, toegang tot bepaalde apps van specifieke netwerken verlenen beschikken.
2. Uw reguliere beleidsregels voor beheer op basis van uitschakelen.

### <a name="microsoft-recommendations"></a>Aanbevelingen van Microsoft

Afhankelijk van welke oplossingen of onvoorziene gebeurtenissen worden gebruikt tijdens een onderbreking, kan uw organisatie toegang met alleen wachtwoorden worden verlenen. Er is geen beveiliging is een aanzienlijke beveiligingsrisico's moet zorgvuldig worden afgewogen. Organisaties moeten:

1. Documenteer elke wijziging en de vorige status kunnen terugdraaien van alle gebeurtenissen die u, zodra de besturingselementen voor toegang volledig operationeel zijn geïmplementeerd als onderdeel van uw strategie voor het beheer van wijzigen.
2. Wordt ervan uitgegaan dat kwaadwillende actoren probeert het verzamelen van wachtwoorden via wachtwoord water en phishing-aanvallen, terwijl u MFA uitgeschakeld. Ongeldige actoren mogelijk ook, al wachtwoorden op dat eerder heeft geen toegang verleend aan een resource die kan worden uitgevoerd tijdens deze periode. U kunt dit risico gedeeltelijk door hun wachtwoord opnieuw in te stellen voordat u MFA uitschakelt voor hen beperken voor kritieke gebruikers, zoals leidinggevenden.
3. Archiveren van alle aanmeldingsactiviteiten om te bepalen wie toegang heeft tot wat tijdens de tijd die MFA is uitgeschakeld.
4. [Alle risicogebeurtenissen gerapporteerd sorteren](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) tijdens dit venster.

## <a name="after-a-disruption"></a>Na een onderbreking

Wijzigingen ongedaan maken het u als onderdeel van de geactiveerde plan voor onvoorziene gebeurtenissen zodra de service is hersteld, waardoor de wordt onderbroken. 

1. Het normale beleid inschakelen
2. Uw beleidsregels voor onvoorziene gebeurtenissen uitschakelen. 
3. Eventuele andere wijzigingen die u tijdens de onderbreking van de beschreven en wordt teruggedraaid.
4. Als u een account voor toegang in noodgevallen gebruikt, moet u referenties opnieuw wilt genereren en de details van de nieuwe referenties een fysiek beveiligde als onderdeel van uw account voor toegang in noodgevallen procedures.
5. Blijven [sorteren alle risicogebeurtenissen gerapporteerd](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) na de onderbreking voor verdachte activiteiten.
6. Alle vernieuwingstokens die zijn uitgegeven intrekken [met behulp van PowerShell](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0) doel een groep gebruikers. Alle vernieuwingstokens intrekken is belangrijk voor bevoegde accounts die worden gebruikt tijdens de onderbreking en hierbij wordt hen dwingen zich te verifiëren en te voldoen aan het besturingselement van de herstelde beleidsregels.

## <a name="emergency-options"></a>Opties voor noodgevallen

 In het geval van nood en uw organisatie niet eerder implementeren van een beperking of een plan voor onvoorziene gebeurtenissen en volg de aanbevelingen in de [onvoorziene gebeurtenissen om de gebruiker vergrendeling](#contingencies-for-user-lockout) sectie als u al gebruik van voorwaardelijke toegang beleid voor het afdwingen van MFA.
Als uw organisatie van verouderde MFA-beleid per gebruiker gebruikmaakt, kunt u de volgende opties overwegen:

1. Als u het bedrijfsnetwerk uitgaande IP-adres hebt, kunt u hen toevoegen als vertrouwde IP-adressen verificatie alleen met het bedrijfsnetwerk in te schakelen.
 2. Als u de inventaris van de uitgaande IP-adressen hebt, of u vereist om toegang te krijgen binnen en buiten het bedrijfsnetwerk bevinden, kunt u de volledige IPv4-adresruimte toevoegen als goedgekeurde IP-adressen door 0.0.0.0/1 en 128.0.0.0/1 op te geven.

>[!IMPORTANT]
 > Als u de vertrouwde IP-adressen om de toegang blokkering te vergroten, zal niet risicogebeurtenissen die zijn gekoppeld aan IP-adressen (bijvoorbeeld onmogelijk traject of onbekende locaties) worden gegenereerd.

>[!NOTE]
 > Configureren van [vertrouwde IP-adressen](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-mfasettings) voor Azure MFA is alleen beschikbaar bij [Azure AD Premium-licenties](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-licensing).

## <a name="learn-more"></a>Meer informatie

* [Documentatie over Azure AD-verificatie](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-iis)
* [Beheerdersaccounts voor EMS-toegang beheren in Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)
* [Benoemde locaties configureren in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)
 * [Set-MsolDomainFederationSettings](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0)
* [Het configureren van hybride Azure Active Directory verbonden apparaten](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan)
* [Implementatiehandleiding Windows Hello voor Bedrijven](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide)
 * [Password Guidance - Microsoft Research](https://research.microsoft.com/pubs/265143/microsoft_password_guidance.pdf)
* [Wat zijn de voorwaarden voor de voorwaardelijke toegang van Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions)
* [Wat zijn besturingselementen voor toegang in Azure Active Directory voor voorwaardelijke toegang?](https://docs.microsoft.com/azure/active-directory/conditional-access/controls)
