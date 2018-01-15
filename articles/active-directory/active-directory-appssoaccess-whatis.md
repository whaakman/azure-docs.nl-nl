---
title: Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory? | Microsoft Docs
description: Gebruik Azure Active Directory voor eenmalige aanmelding tot alle van de website en SaaS toepassingen die u nodig hebt voor bedrijven inschakelen.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.assetid: 75d1a3fd-b3c5-4495-a5c8-c4c24145ff00
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: curtand
ms.reviewer: asmalser
ms.custom: it-pro
ms.openlocfilehash: 42a24654eb059894a855474c922a4dd2da185149
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="what-is-application-access-and-single-sign-on-with-azure-active-directory"></a>Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?
Eenmalige aanmelding betekent wordt toegang tot alle toepassingen en bronnen die u nodig hebt om zaken te doen, wanneer u zich aanmeldt slechts één keer met behulp van één gebruikersaccount. Wanneer u bent aangemeld, kunt u alle toepassingen zonder verificatie openen (bijvoorbeeld: Typ een wachtwoord) een tweede keer.

Veel organisaties zijn afhankelijk van de software als een dienst (SaaS)-toepassingen zoals Office 365, het selectievakje en Salesforce voor de productiviteit van eindgebruikers. IT-personeel moet afzonderlijk maken en bijwerken van gebruikersaccounts in elke SaaS-toepassing in het verleden hebben en gebruikers moeten een wachtwoord voor elke SaaS-toepassing.

Azure Active Directory breidt op de lokale Active Directory in de cloud, zodat gebruikers kunnen hun primaire organisatie-account niet alleen op hun apparaten domein aanmelden en bedrijfsresources gebruiken, maar ook alle web- en SaaS-toepassingen die nodig zijn voor de taak.

Zo niet alleen gebruikers hoeven niet te beheren van meerdere sets met gebruikersnamen en wachtwoorden, hun toepassingen toegang kan automatisch worden ingericht of buiten gebruik gesteld op basis van hun organisatie groepsleden en hun status als een werknemer. Azure Active Directory maakt u kennis met de beveiliging en toegang governance opties waarmee u toegang van gebruikers centraal te beheren via de SaaS-toepassingen.

Azure AD kunt eenvoudige integratie tot een groot aantal populaire SaaS-toepassingen van vandaag; het biedt identiteit en toegang beheren en gebruikers kan rechtstreeks eenmalige aanmelding tot toepassingen of detecteren en ze te starten vanuit een portal zoals Office 365 of het Azure AD-Toegangsvenster.

De architectuur van de integratie bestaat uit de volgende vier belangrijkste bouwstenen:

* Eenmalige aanmelding kan gebruikers toegang krijgen tot hun SaaS-toepassingen op basis van hun organisatieaccount in Azure AD. Eenmalige aanmelding is wat kunnen gebruikers om een toepassing met hun enkele organisatie-account te verifiëren.
* Gebruikers inrichten, kunt u gebruikers inrichten en de inrichting op doel die SaaS op basis van wijzigingen in Windows Server Active Directory en/of Azure AD. Er is een ingerichte account wat kan een gebruiker worden gemachtigd voor het gebruik van een toepassing nadat ze zijn geverifieerd via eenmalige aanmelding.
* Gecentraliseerde Toepassingsbeheer toegang in de Azure portal geeft één punt van SaaS-toepassing toegang en beheer, de mogelijkheid om te delegeren van de toepassing toegang besluitvorming en goedkeuringen voor iedereen in de organisatie
* Geïntegreerde rapportage en controle van gebruikersactiviteit in Azure AD

## <a name="how-does-single-sign-on-with-azure-active-directory-work"></a>How does single sign-on with Azure Active Directory work? (Hoe werkt eenmalige aanmelding met Azure Active Directory?)
Wanneer een gebruiker 'zich aanmeldt' tot een toepassing, doorlopen ze een verificatieproces waarop ze zijn vereist om te bewijzen dat zij zijn die zij beweren te zijn. Dit gebeurt meestal door te voeren van een wachtwoord dat is opgeslagen in de toepassing zonder eenmalige aanmelding en de gebruiker is vereist om dit wachtwoord te kennen.

Azure AD ondersteunt drie verschillende manieren aan te melden bij toepassingen:

* **Federatieve eenmalige aanmelding** kunnen toepassingen wordt omgeleid naar Azure AD voor verificatie van de gebruiker in plaats van naar een eigen wachtwoord wordt gevraagd. Dit wordt ondersteund voor toepassingen die ondersteuning voor protocollen zoals SAML 2.0, WS-Federation of OpenID Connect, en de allerbeste modus van eenmalige aanmelding.
* **Op basis van wachtwoorden eenmalige aanmelding** veilige opslag voor servertoepassingen wachtwoord en opnieuw afspelen met een browser webextensie of mobiele app. Dit maakt gebruik van het proces voor bestaande aanmelden geleverd door de toepassing, maar Hiermee kan een beheerder de wachtwoorden beheren en vereist niet dat de gebruiker het wachtwoord kennen.
* **Bestaande eenmalige aanmelding** kan gebruikmaken van een bestaande eenmalige aanmelding die is ingesteld voor de toepassing, maar kan deze toepassingen worden gekoppeld aan de portals Office 365 of Azure AD access deelvenster Azure AD en kunt u ook aanvullende rapportage in Azure AD wanneer de toepassingen er worden gestart.

Wanneer een gebruiker is geverifieerd met een toepassing, ze moet bovendien beschikken over een record voor een account ingericht op de toepassing waarin wordt gemeld de toepassing dat waarin er machtigingen en toegangsniveau die zich binnen de toepassing. Het inrichten van een record voor dit account ofwel automatisch kan worden uitgevoerd of kan optreden handmatig door een beheerder voordat de gebruiker één aanmelding toegang ontvangt.

 Meer informatie over deze modi voor eenmalige aanmelding en inrichting hieronder.

### <a name="federated-single-sign-on"></a>Federatieve eenmalige aanmelding
Federatieve eenmalige aanmelding kan de gebruikers in uw organisatie automatisch worden aangemeld bij een SaaS-toepassing van derden door Azure AD met behulp van de gegevens van de gebruikersaccount van Azure AD.

In dit scenario wanneer u al hebt geregistreerd in Azure AD en u wilt toegang krijgen tot bronnen die worden beheerd door een derde partij SaaS-toepassing hoeven federation voor een gebruiker te worden geverifieerd.

Azure AD kan ondersteuning voor federatieve eenmalige aanmelding met toepassingen die ondersteuning bieden voor het SAML 2.0, WS-Federation, of OpenID connect protocollen.

Zie ook: [beheren van certificaten voor federatieve eenmalige aanmelding](active-directory-sso-certs.md)

### <a name="password-based-single-sign-on"></a>Eenmalige aanmelding op basis van wachtwoord
Configureren op basis van wachtwoorden eenmalige aanmelding kan de gebruikers in uw organisatie automatisch worden aangemeld bij een SaaS-toepassing van derden door Azure AD met behulp van de gegevens van de gebruikersaccount van de SaaS-toepassing van derden. Wanneer u deze functie inschakelt, wordt Azure AD verzameld en veilig opgeslagen gegevens van de gebruikersaccount en het bijbehorende wachtwoord.

Azure AD ondersteunt op basis van wachtwoorden eenmalige aanmelding voor een cloud-app die een op basis van een HTML-aanmeldingspagina is. Met behulp van een aangepaste browser-invoegtoepassing AAD automatiseert van de gebruiker aanmelden via veilig bij het ophalen van referenties van de toepassing zoals de gebruikersnaam en het wachtwoord van de map en voert u deze referenties in van de toepassing aanmeldingspagina namens de de gebruiker. Er zijn twee gebruiksvoorbeelden:

1. **Beheerder beheert referenties** – beheerders kunnen maken en beheren van referenties van de toepassing en deze referenties toewijzen aan gebruikers of groepen die toegang nodig tot de toepassing. In dergelijke gevallen de eindgebruiker hoeft niet te weten de referenties, maar nog steeds één aanmelding toegang krijgt tot de toepassing gewoon door erop te klikken in het deelvenster toegang of via een opgegeven koppeling. Hierdoor kunnen beide, beheer van de levenscyclus van de referenties door de beheerder, evenals de gemak voor eindgebruikers waarbij ze niet hoeft te onthouden of app-specifiek wachtwoorden beheren. De referenties zijn van de eindgebruiker verscholen tijdens de automatische aanmelding; ze zijn echter technisch kunnen worden gedetecteerd door de gebruiker met de foutopsporing voor web hulpprogramma's en gebruikers en beheerders moeten het dezelfde beveiligingsbeleid volgen als de referenties zijn rechtstreeks door de gebruiker weergegeven. Beheerder geleverde referenties zijn nuttig wanneer ze toegang verlenen account die wordt gedeeld door veel gebruikers, zoals sociale media of toepassingen delen van documenten.
2. **Gebruiker beheert referenties** – beheerders kunnen toepassingen toewijzen aan gebruikers of groepen en eindgebruikers hun eigen referenties rechtstreeks bij de toegang tot de toepassing voor het eerst in het deelvenster toegang in te voeren. Hiermee maakt u gemakkelijker te maken voor eindgebruikers waarbij ze niet hoeft voortdurend de wachtwoorden van de app-specifiek invoeren telkens wanneer die ze toegang krijgen de toepassing tot. Deze gebruiksvoorbeeld kan ook worden gebruikt als een stap voor stap steen voor administratief beheer van de referenties op, waarbij de beheerder nieuwe referenties voor de toepassing op een later tijdstip instellen kunt zonder de toegang van apps van de eindgebruiker.

In beide gevallen moeten referenties worden opgeslagen in een versleutelde status in de map en alleen via HTTPS wordt doorgegeven tijdens het proces voor automatische aanmelding. Gebruik op basis van wachtwoorden eenmalige aanmelding, biedt Azure AD een oplossing voor identiteitsbeheer voor gemakkelijke toegang voor apps die niet kunnen ondersteunen federation-protocollen.

Eenmalige aanmelding op basis van wachtwoorden is afhankelijk van een Browseruitbreiding veilig ophalen van de toepassing en gebruikersspecifieke gegevens van Azure AD en toepassen op de service. Deze functie wordt ondersteund door de meeste van derden SaaS-toepassingen die worden ondersteund door Azure AD.

Voor eenmalige aanmelding op basis van wachtwoorden kunnen van de eindgebruiker browsers zijn:
* Internet Explorer 8, 9, 10, 11--op Windows 7 of hoger
* Rand verjaardagseditie van Windows 10 of hoger 
* Chrome--Op Windows 7 of hoger, en op Mac OS X of hoger
* Firefox 26,0 of later--op Windows XP SP2 of hoger, en op Mac OS X 10,6 of hoger

### <a name="existing-single-sign-on"></a>Bestaande eenmalige aanmelding
Bij het configureren van eenmalige aanmelding voor een toepassing, biedt de Azure portal een derde optie van ' bestaande Single Sign-On '. Deze optie kunt gewoon de beheerder een koppeling naar een toepassing maken en plaats het op het toegangsvenster voor geselecteerde gebruikers.

Bijvoorbeeld als er een toepassing die is geconfigureerd voor verificatie van gebruikers met behulp van Active Directory Federation Services 2.0, kan een beheerder de optie ' bestaande Single Sign-On ' gebruiken voor het maken van een koppeling in het deelvenster toegang. Wanneer gebruikers toegang krijgen de koppeling tot, zijn ze geverifieerd met behulp van Active Directory Federation Services 2.0 of de bestaande één aanmelding oplossing wordt geleverd door de toepassing.

### <a name="user-provisioning"></a>Gebruikers inrichten
Voor bepaalde toepassingen kunt u Azure AD geautomatiseerde gebruikersinrichting en ongedaan inrichten van accounts in de derde partij SaaS-toepassingen uit binnen de Azure-beheerportal, met behulp van de gegevens van uw Windows Server Active Directory of Azure AD-identiteit. Wanneer een gebruiker is gemachtigd in Azure AD voor een van deze toepassingen, kan een account automatisch worden gemaakt (ingericht) in de doel-SaaS-toepassing.

Wanneer een gebruiker wordt verwijderd of hun gegevens worden gewijzigd in Azure AD, worden deze wijzigingen ook weergegeven in de SaaS-toepassing. Dit houdt in dat, is het configureren van de levenscyclus van geautomatiseerde identiteitsbeheer kan beheerders beheren en zodat geautomatiseerde inrichting en ongedaan inrichting vanaf SaaS-toepassingen. In Azure AD, wordt deze automatisering van de levenscyclus van identiteitsbeheer ingeschakeld door gebruikers inrichten.

Zie voor meer informatie, [geautomatiseerde Gebruikersinrichting en Deprovisioning tot SaaS-toepassingen](active-directory-saas-app-provisioning.md)

## <a name="get-started-with-the-azure-ad-application-gallery"></a>Aan de slag met Azure AD-toepassingsgalerie
Klaar om aan de slag te gaan? Voor het implementeren van eenmalige aanmelding tussen Azure AD en SaaS-toepassingen die uw organisatie gebruikt, volgt u deze richtlijnen.

### <a name="using-the-azure-ad-application-gallery"></a>Met behulp van de Azure AD-toepassingsgalerie
De [Azure Active Directory-Toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/) biedt een overzicht van toepassingen die bekend zijn bij het ondersteunen van een vorm van eenmalige aanmelding bij Azure Active Directory.

![Azure online app-galerie](media/active-directory-appssoaccess-whatis/onlineappgallery.png)

Hier volgen enkele tips voor het vinden van apps door welke mogelijkheden die ze ondersteunen:

* Azure AD biedt ondersteuning voor automatische inrichting en ongedaan inrichten voor alle 'Aanbevolen' apps in de [Azure Active Directory-Toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/).
* Een lijst met federatieve toepassingen die specifiek ondersteuning bieden voor federatieve eenmalige aanmelding via een protocol zoals SAML, WS-Federation, of OpenID Connect vindt [hier](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx).

Als u uw toepassing hebt gevonden, u kunt aan de slag door Volg de stapsgewijze instructies die zijn gepresenteerd in de app-galerie en in de Azure-beheerportal voor eenmalige aanmelding inschakelen.

### <a name="application-not-in-the-gallery"></a>De toepassing niet in de galerie?
Als uw toepassing niet in de galerie van Azure AD-toepassing gevonden is, hebt u deze opties:

* **Toevoegen van een niet-vermelde app u** -gebruik van de categorie aangepast in de app-galerie binnen de Azure-beheerportal om verbinding maken met een niet-vermelde toepassing die van uw organisatie gebruikmaakt. U kunt elke toepassing die SAML 2.0 als federatieve app ondersteunt of elke toepassing die is een op basis van een HTML-aanmeldingspagina als wachtwoord SSO app toevoegen. Zie voor meer informatie in dit artikel op [uw eigen toepassing toe te voegen](application-config-sso-how-to-configure-federated-sso-non-gallery.md).
* **Toevoegen van uw eigen app die u ontwikkelt** : als u de toepassing hebt ontwikkeld uzelf, volg de richtlijnen in de Azure AD-documentatie voor ontwikkelaars voor het implementeren van federatieve eenmalige aanmelding of inrichten met de Azure AD graph API. Zie de volgende bronnen voor meer informatie:
  
  * [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md) (Verificatiescenario's voor Azure AD)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)
* **Aanvragen van een app-integratie** -vraag ondersteuning voor de toepassing u met behulp van moet de [forum met feedback van Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory/).

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken
U kunt de Active Directory-extensie in de Azure portal gebruiken voor het configureren van de toepassing eenmalige aanmelding. Als eerste stap moet u een map selecteert in de sectie Active Directory in de portal:

![][2]

Voor het beheren van uw SaaS-toepassingen van derden, kunt u overschakelen naar het tabblad toepassingen van de geselecteerde map. Deze weergave biedt beheerders:

* Nieuwe toepassingen uit de galerie van Azure AD, evenals de apps die u ontwikkelt toevoegen
* Geïntegreerde toepassingen verwijderen
* De toepassingen die ze al hebt geïntegreerd beheren

Typische administratieve taken voor een SaaS-toepassing van derden zijn:

* Inschakelen van eenmalige aanmelding met Azure AD, met behulp van wachtwoord SSO of, indien beschikbaar voor het doel SaaS federatieve SSO
* Desgewenst schakelen gebruikers inrichten voor gebruikers inrichten en de inrichting ongedaan (identity lifecycle management)
* Voor toepassingen waarvoor gebruikers inrichten is ingeschakeld, selecteren welke gebruikers toegang hebben tot de toepassing

Voor apps die ondersteuning bieden voor federatieve eenmalige aanmelding galerie moet configuratie normaal gesproken u aanvullende configuratie-instellingen zoals certificaten en metagegevens voor het maken van een federatieve vertrouwensrelatie tussen de app van derden en Azure AD. De configuratiewizard leidt u door de details en biedt u eenvoudige toegang tot de SaaS-toepassing-specifieke gegevens en instructies.

Voor het galerie-apps die ondersteuning bieden voor automatisch gebruikers inrichten, moet u hiervoor Azure AD-machtigingen voor het beheren van uw accounts in de SaaS-toepassing te geven. U moet ten minste bieden Azure AD-referenties moeten worden gebruikt wanneer verificatie via bij de doeltoepassing. Of extra configuratie-instellingen moeten worden opgegeven, is afhankelijk van de vereisten van de toepassing.

## <a name="deploying-azure-ad-integrated-applications-to-users"></a>Implementatie van Azure AD geïntegreerde toepassingen voor gebruikers
Azure AD levert aanpasbare op verschillende manieren voor het implementeren van toepassingen voor eindgebruikers in uw organisatie:

* Azure AD-Toegangsvenster
* Startprogramma voor toepassingen van Office 365
* Directe aanmelding bij federatieve apps
* Dieptekoppelingen naar federatieve apps, op basis van wachtwoorden, of bestaande apps

Welke methode(n) die u wilt implementeren in uw organisatie is uw keuze.

### <a name="azure-ad-access-panel"></a>Azure AD-Toegangsvenster
Het toegangsvenster op https://myapps.microsoft.com is een webgebaseerde portal waarmee gebruikers met een organisatieaccount in Azure Active Directory om weer te geven en starten van cloud-gebaseerde toepassingen waartoe ze toegang hebben gekregen door de beheerder van Azure AD . Als u een eindgebruiker met [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), kunt u ook gebruikmaken van mogelijkheden voor groepsbeheer met Self-service via het toegangsvenster.

![Azure AD-Toegangsvenster](media/active-directory-appssoaccess-whatis/azure-ad-access-panel.png)

Het Toegangspaneel is gescheiden van de Azure-portal en vereist geen gebruikers aan een Azure-abonnement of een Office 365-abonnement hebt.

Zie voor meer informatie over het Azure AD-Toegangsvenster de [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

### <a name="office-365-application-launcher"></a>Startprogramma voor toepassingen van Office 365
Voor organisaties die Office 365 hebt geïmplementeerd, worden toepassingen die zijn toegewezen aan gebruikers via Azure AD wordt ook weergegeven in de Office 365-portal op https://portal.office.com/myapps. Dit maakt het eenvoudig en voor gebruikers in een organisatie hun apps starten zonder gebruik te maken van een tweede portal en de aanbevolen app startende oplossing voor organisaties met behulp van Office 365.

![][4]

Zie voor meer informatie over het startprogramma voor toepassingen van Office 365, [uw App worden weergegeven in de Office 365 app linksboven](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

### <a name="direct-sign-on-to-federated-apps"></a>Directe aanmelding bij federatieve apps
Meest federatieve toepassingen die ondersteuning bieden voor SAML 2.0 en WS-Federation en het OpenID connect ook ondersteuning voor de mogelijkheid voor gebruikers om te beginnen bij de toepassing en vervolgens ophalen aangemeld via Azure AD door automatische omleiding of door te klikken op een koppeling aan te melden. Dit staat bekend als serviceprovider-aanmelding is gestart en de meeste federatieve toepassingen in de galerie van Azure AD-toepassing ondersteunen deze (Zie de documentatie die is gekoppeld van de app configuratie voor één aanmelding wizard in de Azure-beheerportal voor meer informatie).

![][5]

### <a name="direct-sign-on-links-for-federated-password-based-or-existing-apps"></a>Directe aanmelding koppelingen voor federatieve, op basis van wachtwoorden of bestaande apps
Daarnaast ondersteunt Azure AD directe eenmalige aanmelding koppelingen naar afzonderlijke toepassingen die ondersteuning bieden voor op basis van wachtwoorden eenmalige aanmelding, bestaande eenmalige aanmelding en een vorm van federatieve eenmalige aanmelding.

Deze koppelingen zijn speciaal ontworpen URL's die een gebruiker via het Azure AD-teken in het proces voor een bepaalde toepassing verzenden zonder de gebruiker starten vanuit de Azure AD-Toegangsvenster of Office 365. Deze URL voor eenmalige aanmelding vindt u onder het tabblad Dashboard van alle vooraf geïntegreerde toepassingen in de sectie Active Directory van de Azure-beheerportal, zoals wordt weergegeven in de onderstaande schermafbeelding.

![][6]

Deze koppelingen kunnen worden gekopieerd en geplakt elke locatie die u wilt een koppeling aanmelden aan de geselecteerde toepassing. Dit kan zijn in een e-mailbericht, of in een aangepaste web gebaseerde portal die u hebt ingesteld voor toegang tot de toepassingen van gebruiker. Hier volgt een voorbeeld van een Azure AD direct één aanmeldings-URL voor Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Net als bij organisatiespecifieke URL's voor het toegangsvenster, u kunt verder aanpassen deze URL door een van de actieve of geverifieerde domeinen voor uw directory nadat het myapps.microsoft.com domein toe te voegen. Dit zorgt ervoor dat eventuele huisstijl van de organisatie is geladen onmiddellijk op de pagina aanmelden zonder dat de gebruiker hoeven in te voeren hun gebruikers-ID eerst:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Wanneer een geautoriseerde gebruiker op een van deze koppelingen toepassingsspecifieke klikt, ze eerst Zie van hun organisatie-aanmeldingspagina (ervan uitgaande dat ze nog niet bent aangemeld) en na het aanmelden omgeleid naar de app op het toegangsvenster zonder eerst te stoppen. Als de gebruiker vereisten ontbreken er voor toegang tot de toepassing, zoals de Browseruitbreiding van eenmalige aanmelding op basis van wachtwoorden, wordt de gebruiker voor het installeren van de ontbrekende extensie gevraagd op de koppeling. De URL van de koppeling ook constant is gebleven als wijzigingen in de configuratie voor eenmalige aanmelding voor de toepassing.

Deze koppelingen de dezelfde mechanismen toegang gebruiken als het toegangspaneel en Office 365 en alleen die gebruikers of groepen die zijn toegewezen aan de toepassing in de Azure-beheerportal kan worden geverifieerd. Elke gebruiker die niet geautoriseerd is ziet echter een bericht waarin staat dat ze geen toegang hebben gekregen en een koppeling krijgt naar het laden van het toegangsvenster om weer te geven van de beschikbare toepassingen waarvoor ze toegang hebben.

## <a name="related-articles"></a>Verwante artikelen:
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Zoeken naar niet-toegestane cloud-toepassingen met Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md)
* [Inleiding tot het beheren van toegang tot Apps](active-directory-managing-access-to-apps.md)
* [Vergelijking van mogelijkheden voor het beheren van externe identiteiten in Azure AD](active-directory-b2b-compare-external-identities.md)

<!--Image references-->
[1]: ./media/active-directory-appssoaccess-whatis/onlineappgallery.png
[2]: ./media/active-directory-appssoaccess-whatis/azuremgmtportal.png
[3]: ./media/active-directory-appssoaccess-whatis/accesspanel.png
[4]: ./media/active-directory-appssoaccess-whatis/officeapphub.png
[5]: ./media/active-directory-appssoaccess-whatis/workdaymobile.png
[6]: ./media/active-directory-appssoaccess-whatis/deeplink.png
