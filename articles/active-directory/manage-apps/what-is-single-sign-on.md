---
title: Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory? | Microsoft Docs
description: Azure Active Directory gebruiken om in te schakelen single sign-on bij alle van de website en SaaS toepassingen die u nodig hebt voor bedrijven.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.workload: identity
ms.topic: article
ms.date: 06/27/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.custom: it-pro
ms.openlocfilehash: 9f9ae52c5dbdf655190caaceeaa03c444526e4f2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044618"
---
# <a name="what-is-application-access-and-single-sign-on-with-azure-active-directory"></a>Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?
Eenmalige aanmelding betekent toegang te hebben tot alle toepassingen en bronnen die u nodig hebt om zaken te doen, aanmeldt slechts eenmaal met behulp van één gebruikersaccount. Nadat u bent aangemeld, u toegang hebt tot alle van de toepassingen die u nodig hebt zonder vereist is om te verifiëren (bijvoorbeeld, typ een wachtwoord) een tweede keer.

Veel organisaties zijn afhankelijk van de software als een service (SaaS)-toepassingen zoals Office 365, Box en Salesforce voor de productiviteit van eindgebruikers. In het verleden, IT-personeel moet afzonderlijk maken en bijwerken van gebruikersaccounts in elke SaaS-toepassing, en gebruikers hoeven te onthouden van een wachtwoord voor elke SaaS-toepassing.

Azure Active Directory uitbreiding van on-premises Active Directory naar de cloud, zodat gebruikers hun primaire organisatie-account niet alleen aanmelden bij hun domein apparaten en bronnen van de bedrijfsportal gebruiken, maar ook alle van de web- en SaaS-toepassingen die nodig zijn voor de taak.

Dus niet alleen gebruikers hebben geen voor het beheren van meerdere sets met gebruikersnamen en wachtwoorden, hun toepassingen toegang kan automatisch worden ingericht of worden niet ingericht op basis van hun organisatie groepsleden en hun status als een werknemer. Azure Active Directory introduceert beveiliging en governance-toegangsbeheer waarmee u kunt centraal beheren van toegang van gebruikers voor SaaS-toepassingen.

Azure AD maakt eenvoudige integratie naar veel populaire SaaS-toepassingen vandaag; het voorziet in identiteits- en toegangsbeheer en kan gebruikers rechtstreeks, eenmalige aanmelding tot toepassingen of detecteren en ze te starten vanuit een portal, zoals Office 365 of de Azure AD-toegangspaneel.

De architectuur van de integratie bestaat uit de volgende vier belangrijkste bouwstenen:

* Eenmalige aanmelding kan gebruikers toegang tot hun SaaS-toepassingen op basis van hun organisatie-account in Azure AD. Eenmalige aanmelding is wat gebruikers worden geverifieerd bij een toepassing met behulp van hun één organisatie-account kunt.
* Inrichten van gebruikers kunt inrichten van gebruikers en ongedaan maken inrichting in target die SaaS op basis van wijzigingen in Windows Server Active Directory en/of Azure AD. Er is een ingerichte account kan een gebruiker wilt machtigen voor het gebruik van een toepassing, nadat ze zijn geverifieerd via eenmalige aanmelding.
* Toegangsbeheer voor gecentraliseerde toepassingen in de toegang tot Azure portal kunnen één punt van SaaS-toepassingen en het beheer, de mogelijkheid om te delegeren besluitvorming voor toegang tot toepassingen en -goedkeuringen voor iedereen in de organisatie
* Geïntegreerde rapportage en controle van gebruikersactiviteit in Azure AD

## <a name="how-does-single-sign-on-with-azure-active-directory-work"></a>How does single sign-on with Azure Active Directory work? (Hoe werkt eenmalige aanmelding met Azure Active Directory?)
Wanneer gebruikers zich bij een toepassing aanmelden, ze via een verificatieproces waar ze zijn vereist om te bewijzen dat ze zijn wie ze beweren te zijn. Zonder eenmalige aanmelding, dit verificatieproces wordt doorgaans uitgevoerd door het opgeven van een wachtwoord dat is opgeslagen op de toepassing en gebruikers moeten Onthoud dit wachtwoord.

Azure AD ondersteunt drie verschillende manieren aanmelden bij toepassingen:

* **Federatieve eenmalige aanmelding** kunnen toepassingen om te leiden naar Azure AD voor verificatie van de gebruiker in plaats van dat u wordt gevraagd om een eigen wachtwoord. Federatieve eenmalige aanmelding wordt voor toepassingen die ondersteuning voor, zoals SAML 2.0, WS-Federation en OpenID Connect protocollen, en de uitgebreidste modus van eenmalige aanmelding is ondersteund.
* **Wachtwoord gebaseerde eenmalige aanmelding** kunnen beveiligde toepassing wachtwoorden worden opgeslagen en herhalen met behulp van een uitbreiding van web browser of mobiele app. Wachtwoord gebaseerde eenmalige aanmelding gebruikt de bestaande 
*  proces geleverd door de toepassing, maar kan een beheerder de wachtwoorden beheren en vereist niet de gebruiker het wachtwoord kennen.
* **Bestaande eenmalige aanmelding** kan gebruikmaken van een bestaande eenmalige aanmelding die is ingesteld voor de toepassing, maar deze toepassingen worden gekoppeld aan de Office 365 of Azure AD access panel portals kan Azure AD en kunt u ook extra rapportage in Azure AD wanneer de toepassingen er worden gestart.

Wanneer een gebruiker is geverifieerd met een toepassing, moeten ze ook een accountrecord die is ingericht op de toepassing die wordt gemeld dat de toepassing waarbij de machtigingen en het niveau van toegang in de toepassing zijn hebben. Het inrichten van een record voor dit account ofwel automatisch kan worden uitgevoerd kan, of wanneer deze handmatig door een beheerder voordat de gebruiker één aanmelding toegang ontvangt.

 Meer informatie over deze modi voor eenmalige aanmelding en de inrichting hieronder.

### <a name="federated-single-sign-on"></a>Federatieve eenmalige aanmelding
Federatieve eenmalige aanmelding kan de gebruikers in uw organisatie automatisch worden aangemeld bij een SaaS-toepassing van derden door Azure AD met behulp van de gebruikersaccountgegevens uit Azure AD.

Als u al hebt geregistreerd bij Azure AD en u wilt toegang krijgen tot bronnen die worden beheerd door een externe SaaS-toepassing elimineert federation in dit scenario, de noodzaak voor een gebruiker opnieuw worden geverifieerd.

Azure AD kan ondersteuning voor federatieve eenmalige aanmelding met toepassingen die ondersteuning bieden voor de SAML 2.0, WS-Federation, of OpenID connect-protocollen.

Zie ook: [beheren van certificaten voor federatieve eenmalige aanmelding](manage-certificates-for-federated-single-sign-on.md)

### <a name="password-based-single-sign-on"></a>Eenmalige aanmelding op basis van wachtwoord
Configureren van wachtwoord gebaseerde eenmalige aanmelding kan de gebruikers in uw organisatie automatisch worden aangemeld bij een SaaS-toepassing van derden door Azure AD met behulp van de gebruikersaccountgegevens uit de SaaS-toepassing van derden. Wanneer u deze functie inschakelt, wordt Azure AD worden verzameld en veilig opgeslagen door de gebruikersaccountgegevens en het bijbehorende wachtwoord.

Azure AD ondersteunt wachtwoord gebaseerde eenmalige aanmelding voor alle cloud-app die een op HTML gebaseerde aanmeldingspagina is. Met behulp van een aangepaste browserinvoegtoepassing AAD automatiseert de aanmelding via veilig bij het ophalen van referenties voor toepassingen, zoals de gebruikersnaam en het wachtwoord uit de map en voert deze referenties in de pagina voor aanmelding bij toepassingen namens de gebruiker. Er zijn twee gebruiksscenario's:

1. **Referenties worden beheerd door de beheerder** : beheerders kunnen maken en beheren van referenties voor toepassingen en deze referenties toewijzen aan gebruikers of groepen die toegang nodig tot de toepassing. In dergelijke gevallen de eindgebruiker hoeft niet te weten de referenties, maar nog steeds één aanmelding toegang tot de toepassing krijgt gewoon door erop te klikken in het toegangsvenster of via een opgegeven koppeling. Dit proces kunt beide, beheer van de levenscyclus van de referenties door de beheerder, evenals de gemak voor eindgebruikers waarbij ze niet hoeven te onthouden of app-specifieke wachtwoorden beheren. De referenties zijn van de eindgebruiker verscholen tijdens de automatische aanmelding; ze zijn echter in technisch opzicht kunnen worden gedetecteerd door de gebruiker met behulp van hulpprogramma's voor foutopsporing voor web en gebruikers en beheerders moeten de dezelfde beleidsregels voor veiligheid volgen als de referenties zijn rechtstreeks door de gebruiker weergegeven. Referenties voor beheerder geleverde zijn nuttig bij het opgeven van toegang tot die wordt gedeeld door veel gebruikers, zoals sociale media of toepassingen delen van documenten.
2. **Referenties worden beheerd in gebruiker** : beheerders kunnen toepassingen toewijzen aan gebruikers of groepen, en dat de eindgebruikers hun eigen referenties rechtstreeks bij toegang tot de toepassing voor de eerste keer in hun Toegangsvenster in te voeren. Hiermee maakt u een eenvoudige voor eindgebruikers waarbij ze niet hoeven voortdurend de app-specifieke wachtwoorden invoeren telkens wanneer die ze toegang krijgen de toepassing tot. Gebruikers kunnen blijven voor het beheren van hun wachtwoord door bij te werken of deze zo nodig worden verwijderd. Deze use case kan ook worden gebruikt als een basismethode voor administratief beheer van de referenties op, waarbij de beheerder van de nieuwe referenties voor de toepassing op een later tijdstip instellen kunt zonder te hoeven wijzigen van de ervaring van de app-toegang van de eindgebruiker.

In beide gevallen referenties worden opgeslagen in een versleutelde status in de map, en alleen via HTTPS worden doorgegeven tijdens het proces voor automatische aanmelding. Met behulp van wachtwoord gebaseerde eenmalige aanmelding, biedt Azure AD een oplossing voor identiteitsbeheer voor handige toegang voor apps die zijn niet geschikt voor federation-protocollen ondersteunen.

Eenmalige aanmelding op basis van wachtwoorden is afhankelijk van een browserextensie veilig in de toepassing en de gebruikersspecifieke informatie ophalen uit Azure AD en pas deze toe op de service. Deze functie wordt ondersteund door de meeste externe SaaS-toepassingen die worden ondersteund door Azure AD.

Voor eenmalige aanmelding op basis van wachtwoorden kunnen van de eindgebruiker browsers zijn:
* Internet Explorer 11--op Windows 7 of hoger
* Edge op Windows 10 Anniversary Edition of hoger 
* Chrome--Op Windows 7 of hoger, en op Mac OS X of hoger
* Firefox 26,0 of later--op Windows XP SP2 of hoger, en Mac OS X 10,6 of hoger

### <a name="existing-single-sign-on"></a>Bestaande eenmalige aanmelding
Bij het configureren van eenmalige aanmelding voor een toepassing, biedt de Azure-portal een derde optie van ' bestaande Single Sign-On '. Deze optie kan alleen de beheerder een koppeling naar een toepassing maken en plak deze in het toegangsvenster voor de geselecteerde gebruikers.

Bijvoorbeeld, als er een toepassing die is geconfigureerd voor het verifiëren van gebruikers met behulp van Active Directory Federation Services 2.0, kunt een beheerder de optie ' bestaande Single Sign-On ' gebruiken om te maken van een koppeling naar het in het toegangsvenster. Wanneer gebruikers toegang krijgen de koppeling tot, zijn ze geverifieerd met behulp van Active Directory Federation Services 2.0 of welke bestaande eenmalige aanmelding-oplossing wordt geleverd door de toepassing.

### <a name="user-provisioning"></a>Inrichten van gebruikers
Voor bepaalde toepassingen, Azure AD maakt het mogelijk om geautomatiseerde gebruikersinrichting en ongedaan maken inrichting van accounts in externe SaaS-toepassingen uit in Azure portal, de gegevens van uw Windows Server Active Directory of Azure AD-identiteit. Wanneer een gebruiker wordt gegeven van machtigingen in Azure AD voor een van deze toepassingen, kan automatisch een account worden gemaakt (ingericht) in de doel-SaaS-toepassing.

Wanneer een gebruiker wordt verwijderd of de gegevens worden gewijzigd in Azure AD, worden deze wijzigingen ook weergegeven in de SaaS-toepassing. Dit houdt in dat, is het configureren van geautomatiseerd beheer van identiteitslevenscycli kan beheerders om te bepalen en geautomatiseerde inrichting en ongedaan maken inrichting van SaaS-toepassingen. In Azure AD, wordt deze automatisering van beheer van identiteitslevenscycli ingeschakeld door het inrichten van gebruikers.

Zie voor meer informatie, [geautomatiseerde Gebruikersinrichting en het opheffen van inrichting voor SaaS-toepassingen](../active-directory-saas-app-provisioning.md)

## <a name="get-started-with-the-azure-ad-application-gallery"></a>Aan de slag met de Azure AD-toepassingsgalerie
Klaar om te beginnen? Implementeren van eenmalige aanmelding tussen Azure AD en SaaS-toepassingen die uw organisatie gebruikt, volgt u deze richtlijnen.

### <a name="using-the-azure-ad-application-gallery"></a>Met behulp van de Azure AD-toepassingsgalerie
De [Azure Active Directory-Toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/) biedt een overzicht van toepassingen die bekend zijn bij het ondersteunen van een vorm van eenmalige aanmelding met Azure Active Directory.

![Online Azure-app-galerie](media/what-is-single-sign-on/onlineappgallery.png)

Hier volgen enkele tips voor het vinden van apps door welke mogelijkheden worden ondersteund:

* Azure AD biedt ondersteuning voor automatische inrichting en ongedaan maken inrichting voor alle 'Aanbevolen' apps in de [Azure Active Directory-Toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/).
* Een lijst met federatieve toepassingen die specifiek ondersteuning bieden voor federatieve eenmalige aanmelding met behulp van een protocol zoals SAML, WS-Federation, of OpenID Connect vindt u [hier](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx).

Als u uw toepassing hebt gevonden, kunt u beginnen door het volgen van stapsgewijze instructies in de app-galerie en in de Azure-portal om in te schakelen eenmalige aanmelding.

### <a name="application-not-in-the-gallery"></a>De toepassing niet in de galerie?
Als uw toepassing niet in de Azure AD-toepassingsgalerie gevonden is, hebt u deze opties:

* **Toevoegen van een niet-vermelde app u** -gebruik van de aangepaste categorie in de app-galerie in Azure portal om een niet-vermelde toepassing die van uw organisatie gebruikmaakt verbinding te maken. U kunt elke toepassing die ondersteuning biedt voor SAML 2.0 als federatieve app of elke toepassing die is een op HTML gebaseerde aanmeldingspagina opgeven als wachtwoord SSO-app toevoegen. Zie voor meer informatie in dit artikel op [uw eigen toepassing toe te voegen](../application-config-sso-how-to-configure-federated-sso-non-gallery.md).
* **Toevoegen aan uw eigen app die u ontwikkelt** : als u de toepassing hebt ontwikkeld zelf, volg de richtlijnen in de Azure AD-documentatie voor ontwikkelaars voor het implementeren van federatieve eenmalige aanmelding of inrichten met behulp van de Azure AD graph API. Zie de volgende bronnen voor meer informatie:
  
  * [Authentication Scenarios for Azure AD](../active-directory-authentication-scenarios.md) (Verificatiescenario's voor Azure AD)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)
* **Aanvragen van een app-integratie** -ondersteuning aanvragen voor de toepassing die u nodig hebt met de [Azure AD-Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/).

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken
U kunt de Active Directory-extensie gebruiken in Azure portal het configureren van de toepassing eenmalige aanmelding. Als eerste stap moet u selecteert u een map in de sectie Active Directory in de portal:

![](./media/what-is-single-sign-on/azuremgmtportal.png)

Voor het beheren van uw SaaS-toepassingen van derden, kunt u overschakelen naar het tabblad toepassingen van de geselecteerde map. In deze weergave kan beheerders:

* Nieuwe toepassingen uit de galerie van Azure AD, evenals de apps die u ontwikkelt toevoegen
* Geïntegreerde toepassingen verwijderen
* Beheren van de toepassingen die ze al hebt geïntegreerd

Typische administratieve taken voor een SaaS-toepassing van derden zijn:

* Inschakelen van eenmalige aanmelding met Azure AD, met behulp van SSO-wachtwoord of, indien beschikbaar voor het doel SaaS, federatieve SSO
* (Optioneel), waardoor gebruikers inrichten voor gebruiker inrichting en ongedaan maken inrichting (beheer van identiteitslevenscycli)
* Voor toepassingen 
* Wanneer het inrichten van gebruikers is ingeschakeld, te selecteren welke gebruikers toegang hebben tot deze toepassing

Voor galerie-apps die ondersteuning bieden voor federatieve eenmalige aanmelding, moet configuratie normaal gesproken u bieden aanvullende configuratie-instellingen, zoals certificaten en metagegevens voor het maken van een federatieve vertrouwensrelatie tussen de Apps van derden en Azure AD. De configuratiewizard begeleidt u bij de details en biedt u eenvoudige toegang tot de SaaS-toepassing-specifieke gegevens en instructies.

Voor galerie-apps die ondersteuning bieden voor automatisch gebruikers inrichten, moet hiervoor u Azure AD-machtigingen voor het beheren van uw accounts in de SaaS-toepassing te geven. Ten minste moet u referenties Azure AD moet gebruiken bij het verifiëren van boven aan de doeltoepassing opgeven. Of aanvullende configuratie-instellingen moeten worden opgegeven, is afhankelijk van de vereisten van de toepassing.

## <a name="deploying-azure-ad-integrated-applications-to-users"></a>Azure AD geïntegreerde toepassingen voor gebruikers implementeren
Azure AD biedt verschillende aanpasbare methoden voor het implementeren van toepassingen voor eindgebruikers in uw organisatie:

* Azure AD-Toegangsvenster
* Startprogramma voor Office 365-toepassingen
* Directe aanmelding bij federatieve apps
* Dieptekoppelingen naar federatieve apps, op basis van wachtwoorden, of bestaande apps

Welke methode(n) die u wilt implementeren in uw organisatie is's naar eigen inzicht.

### <a name="azure-ad-access-panel"></a>Azure AD-Toegangsvenster
Het deelvenster toegang https://myapps.microsoft.com is een webportal waarmee een eindgebruiker met een organisatie-account in Azure Active Directory om weer te geven en starten cloud-gebaseerde toepassingen waaraan ze zijn toegang verleend door de Azure AD-beheerder. Als u een eindgebruiker met [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), u kunt ook gebruikmaken van mogelijkheden voor groepsbeheer met Self-service via het toegangsvenster.

![Azure AD-Toegangsvenster](media/what-is-single-sign-on/azure-ad-access-panel.png)

Het toegangsvenster is gescheiden van de Azure-portal en hoeft geen gebruikers hebben een Azure-abonnement of een Office 365-abonnement.

Zie voor meer informatie over de Azure AD-toegangspaneel, de [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

### <a name="office-365-application-launcher"></a>Startprogramma voor Office 365-toepassingen
Voor organisaties die Office 365 hebt geïmplementeerd, toepassingen die zijn toegewezen aan gebruikers via Azure AD ook worden weergegeven in de Office 365-portal op https://portal.office.com/myapps. Dit maakt het gemakkelijk en handig voor gebruikers in een organisatie hun apps starten zonder gebruik te maken van een tweede portal en is de aanbevolen app starten oplossing voor organisaties met behulp van Office 365.

![](./media/what-is-single-sign-on/officeapphub.png)

Zie voor meer informatie over het startprogramma voor Office 365-toepassingen, [uw App worden weergegeven in het startprogramma voor Office 365](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

### <a name="direct-sign-on-to-federated-apps"></a>Directe aanmelding bij federatieve apps
Meest federatieve toepassingen die ondersteuning bieden voor SAML 2.0, WS-Federation en OpenID connect ook ondersteuning voor de mogelijkheid voor gebruikers om te beginnen bij de toepassing en vervolgens u aangemeld via Azure AD door automatische omleiding of door te klikken op een koppeling aan te melden bij. Dit staat bekend als serviceprovider-aanmelding wordt gestart en meest federatieve toepassingen in de Azure AD-toepassingsgalerie ondersteuning bieden voor deze (Zie de documentatie gekoppeld aan de app configuratie voor eenmalige aanmelding wizard in de Azure-portal voor meer informatie).

![](./media/what-is-single-sign-on/workdaymobile.png)

### <a name="direct-sign-on-links-for-federated-password-based-or-existing-apps"></a>Directe aanmelding koppelingen voor federatieve, op basis van wachtwoorden of bestaande apps
Azure AD biedt ook ondersteuning voor directe eenmalige aanmelding in koppelingen naar afzonderlijke toepassingen die ondersteuning op basis van wachtwoorden eenmalige aanmelding, bestaande eenmalige aanmelding en een vorm van federatieve eenmalige aanmelding bieden.

Deze koppelingen zijn speciaal ontworpen URL's die een gebruiker door het proces voor aanmelding bij Azure AD voor een bepaalde toepassing verzenden zonder de gebruiker start ze vanuit de Azure AD toegang tot deelvenster of Office 365. Deze één aanmeldings-URL's kan worden gevonden op het tabblad Dashboard van vooraf geïntegreerde toepassingen in de sectie Active Directory van Azure portal, zoals wordt weergegeven in de onderstaande schermafbeelding.

![](./media/what-is-single-sign-on/deeplink.png)

Deze koppelingen kan worden gekopieerd en geplakt waar die u wilt een koppeling aanmelding aan de geselecteerde toepassing wilt opgeven. Dit wordt mogelijk in een e-mailbericht, of in een aangepaste web-portal op basis van die u hebt ingesteld voor toegang tot de toepassing van gebruiker. Hier volgt een voorbeeld van een Azure AD direct één aanmeldings-URL voor Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Net als bij de organisatie-specifieke URL's voor het toegangsvenster, u kunt verder aanpassen deze URL door een van de actieve of geverifieerde domeinen voor uw directory nadat de myapps.microsoft.com domein toe te voegen. Dit zorgt ervoor dat eventuele huisstijl van de organisatie wordt geladen onmiddellijk op de pagina aanmelden zonder dat de gebruiker hoeft in te voeren hun gebruikers-ID eerst:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Wanneer een geautoriseerde gebruiker op een van deze koppelingen toepassingsspecifieke klikt, ze eerst zien van hun organisatie-aanmeldingspagina (ervan uitgaande dat ze nog niet bent aangemeld) en na het aanmelden worden omgeleid naar de app zonder eerst op het toegangsvenster stoppen. Als de gebruiker vereisten ontbreken er voor toegang tot de toepassing, zoals de Browseruitbreiding van eenmalige aanmelding op basis van wachtwoorden, wordt klikt u vervolgens de koppeling de gebruiker gevraagd de ontbrekende extensie installeren. URL van de koppeling blijft ook constante als de configuratie voor eenmalige aanmelding voor de toepassing wordt gewijzigd.

Deze koppelingen de mechanismen voor hetzelfde besturingselement gebruiken als het toegangsvenster en de Office 365, en alleen deze gebruikers of groepen die zijn toegewezen aan de toepassing in Azure portal is mogelijk om te verifiëren. Elke gebruiker die niet gemachtigd is zien echter een bericht waarin wordt uitgelegd dat ze geen toegang is verleend en een koppeling naar het laden van het toegangsvenster als u wilt weergeven van beschikbare toepassingen waarvoor ze toegang hebben, krijgen.

## <a name="related-articles"></a>Verwante artikelen:
* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](../saas-apps/tutorial-list.md)
* [Cloud Discovery instellen](/cloud-app/security/set-up-cloud-discovery)
* [Inleiding tot het beheren van toegang tot Apps](what-is-access-management.md)
* [Vergelijking van mogelijkheden voor het beheer van externe identiteiten in Azure AD](../active-directory-b2b-compare-b2c.md)


