---
title: Wat is het toegangsvenster in Azure Active Directory? | Microsoft Docs
description: Informatie over het gebruik van variaties op van het toegangsvenster (webbrowser, Android-app, iPhone en iPad-app) voor toegang tot SaaS-apps.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/18
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 31b6741163c9808c32d0f1ae3da8f6f1ffe794eb
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060010"
---
# <a name="what-is-the-access-panel"></a>Wat is het toegangsvenster?

Het toegangsvenster is een portal op Internet. Als u een werk hebt- of in Azure Active Directory (Azure AD schoolaccount), kunt u het toegangsvenster gebruiken om te bekijken en starten van cloud-gebaseerde toepassingen die een Azure AD-beheerder u toegang tot heeft verleend. U kunt ook groepsbeheer met Self-service en mogelijkheden voor app via het toegangsvenster gebruiken.

Het toegangsvenster is gescheiden van de Azure-portal. Dit vereist niet dat u een Azure-abonnement hebt.

![Toegangsvenster][1] met behulp van het toegangsvenster, kunt u enkele van de profielinstellingen van uw te bewerken en doe het volgende:

- Wijzig het wachtwoord dat is gekoppeld aan een account voor werk of school.

- Bewerk de instellingen voor wachtwoord opnieuw instellen.

- Neem contact op met en voorkeur van de instellingen in verband met meervoudige verificatie (voor accounts die zijn vereist om deze te gebruiken door een beheerder) bewerken.

- Account details, zoals gebruikers-ID, alternatief e-mailbericht, mobiele en office telefoonnummers en apparaten bekijken.

- Weergeven en starten van cloud-gebaseerde toepassingen die Azure AD-beheerder u toegang tot heeft verleend. 

- Groepen zelf beheren. Beheerders kunnen maken en beheren van beveiligingsgroepen en aanvragen van beveiligingsgroepen in Azure AD. Zie voor meer informatie, [selfservice groepsbeheer voor gebruikers in Azure AD](../users-groups-roles/groups-self-service-management.md) en [groepen beheren](../fundamentals/active-directory-manage-groups.md).




## <a name="access-the-access-panel"></a>Toegang tot het toegangsvenster

U kunt het toegangsvenster openen door te gaan naar `http://myapps.microsoft.com`.

Als u de aangepaste huisstijl is geconfigureerd voor uw aanmeldingspagina hebt, kunt u de huisstijl van het domein van uw organisatie toe te voegen aan de URL te laden (bijvoorbeeld `http://myapps.microsoft.com/<your domain>.com`).

U kunt een actieve of geverifieerde domeinnaam die is geconfigureerd in uw Azure-portal, zoals hier wordt weergegeven: ![Wingtip Toys domeinnaam][2]  

Distribueer de URL voor alle gebruikers die zich aanmelden bij toepassingen die kunnen worden geïntegreerd met Azure AD.

## <a name="authentication"></a>Verificatie

Als u wilt het toegangspaneel, moet u in Azure AD via een account voor werk- of schoolaccount te worden geverifieerd. U kunt worden geverifieerd bij Azure AD rechtstreeks. U kunt ook als een organisatie heeft federation geconfigureerd met behulp van Active Directory Federation Services (AD FS) of andere technologieën, kunt u worden geverifieerd door Windows Server Active Directory.

Als u een abonnement op Azure of Office 365 hebt en u hebt gebruikt de Azure-portal of een Office 365-toepassing, kunt u de lijst met toepassingen bekijken zonder meldt u zich opnieuw. Als u niet worden geverifieerd, wordt u gevraagd zich aanmeldt met behulp van de gebruikersnaam en het wachtwoord voor uw account in Azure AD. Als uw organisatie heeft Federatie heeft geconfigureerd, is typt de gebruikersnaam voldoende.

Wanneer u bent geverifieerd, kunt u communiceren met de toepassingen die uw beheerder heeft geïntegreerd in de map. Zie voor meer informatie over het integreren van toepassingen met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="web-browser-requirements"></a>Vereisten voor webbrowsers

Ten minste het toegangsvenster vereist een browser die ondersteuning biedt voor JavaScript en CSS is ingeschakeld. Om te worden aangemeld bij toepassingen via wachtwoord gebaseerde eenmalige aanmelding (SSO), moet u de uitbreiding voor toegang tot Configuratiescherm geïnstalleerd in uw browser hebben. De extensie wordt automatisch gedownload wanneer u een toepassing selecteert die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden.

Het installatieprogramma is specifiek voor architectuur. Als u op de koppeling klikt, krijgt u alleen het installatieprogramma voor de OS-architectuur die u momenteel worden uitgevoerd op. Als u een beheerder van de implementatie van toepassing bent, ervoor zorgen dat u gaat u naar de koppeling van een 64-bits en 32-bits-apparaat om op te halen van beide installatieprogramma's.


De extensie van het Configuratiescherm toegang is momenteel beschikbaar voor:
- **Edge**: Windows 10 Anniversary Edition of hoger. 
- **Chrome**: op Windows 7 of hoger, en op Mac OS X of hoger.
- **Firefox 26,0 of hoger**: in Windows XP SP2 of hoger, en op Mac OS X 10.6 of hoger.
- **Internet Explorer 11**: op Windows 7 of hoger (beperkte ondersteuning).

## <a name="my-apps-secure-sign-in-extension"></a>Extensie Veilige aanmelding bij mijn apps
Als u wilt aanmelden bij op basis van wachtwoorden eenmalige aanmelding, moet u de extensie. Nadat de extensie is geïnstalleerd, u kunt aanmelden bij deze aanvullende functies inschakelen door het selecteren van **Meld u aan de slag**. 

- U kunt aanmelden bij een app rechtstreeks met behulp van de app **aanmeldings-URL**. Wanneer u de URL van de app gebruikt, wordt de extensie detecteert de actie en biedt de mogelijkheid van het aanmelden van de extensie.
- U kunt uw Apps vanuit het toegangsvenster starten met behulp van de *snelle zoekactie* functie van de extensie. 
- De extensie ziet u de laatste drie toepassingen die u in de markt gebracht **onlangs gebruikte** sectie.
- U kunt URL's voor interne bedrijf terwijl deze extern via [Application Proxy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

> [!NOTE]
> Aanvullende functies zijn alleen beschikbaar voor Microsoft Edge, Chrome en Firefox.
>
U kunt de extensie downloaden rechtstreeks vanuit de volgende sites:
- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)

Als u een URL voor mijn Apps dan `https://myapps.microsoft.com`, de standaard-URL configureren door het volgende te doen:
1. Terwijl u bent *niet* aangemeld bij de extensie, met de rechtermuisknop op het pictogram van de extensie.
2. Selecteer in het menu **URL voor mijn Apps**.
3. Selecteer de standaard-URL.
4. Selecteer het pictogram van de extensie.
5. Selecteer **Meld u aan de slag**.

Als u wilt gebruiken intern bedrijf URL's tijdens het externe bestand met de extensie, het volgende doen:
1. [Application Proxy configureren](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-enable) op uw tenant.
2. [De toepassing publiceren](https://docs.microsoft.com/en-us/azure/active-directory/application-proxy-publish-azure-portal) en -URL via de toepassingsproxy.
3. Installeer de extensie en ME niet aanmelden door te selecteren, meld u aan de slag.
4. Nu kunt u bladeren naar de URL van de interne bedrijf zelfs terwijl deze extern is.

> [!NOTE]
> U kan automatisch worden omgeleid naar bedrijf URL's uitschakelen door het tandrad van instellingen in het hoofdmenu te selecteren en klikken op **uit** voor de optie bedrijf interne URL-omleiding.


## <a name="mobile-app-support"></a>Ondersteuning voor mobiele Apps

Het team van Azure Active Directory publiceert de mobiele app voor mijn Apps. Wanneer u de app installeert, kunt u zich aanmelden op wachtwoord gebaseerde SSO-toepassingen op iOS en Android-apparaten.

> [!NOTE]
> U kunt aanmelden bij toepassingen die ondersteuning bieden voor federatie met Azure AD (inclusief Salesforce, Google Apps, Dropbox, Box, Concur, Workday, Office 365 en meer dan 70 anderen) op vrijwel elke webbrowser, op elk apparaat, zonder een invoegtoepassing of mobiele app. Moet worden gebruikt op een mobiel apparaat, de andere [toegang tot deelvenster ervaringen](https://myapps.microsoft.com/) ook de Apps in mijn mobiele app niet vereist.
>
>

### <a name="my-apps-for-android"></a>Mijn Apps voor Android

Mijn Apps voor Android wordt ondersteund op een Android-apparaat dat Android-versie 4.1 of hoger wordt uitgevoerd.  

Het is beschikbaar op de [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Mijn Apps voor Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Mijn Apps voor iPhone en iPad

Mijn Apps voor iOS wordt ondersteund op een iPhone of iPad met iOS-versie 7 of hoger.  

Het is beschikbaar op de [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Mijn Apps voor iOS][4]    


## <a name="managed-browser-for-my-apps"></a>Beheerde browser voor mijn Apps

Mijn Apps is ook geïntegreerd met de Intune Managed Browser. De Intune Managed Browser voor iOS en Android-apparaten speelt een belangrijke rol bij het ervoor te zorgen dat gegevens op mobiele apparaten optimaal beveiligd blijft. De browser kunt u veilig weergeven en navigeer van webpagina's die mogelijk bedrijfsgegevens bevatten en het biedt een veilige websurfervaring.  

Krijgt u snel toegang tot mijn Apps op uw startpagina van Managed Browser en in uw bladwijzers zodat minder klikken zijn vereist voor het bereiken van elke toepassing die u wilt openen.

Intune Managed Browser is beschikbaar op de [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) en [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Beheerde browser voor mijn Apps][5]    


## <a name="tips-for-testing-the-user-experience"></a>Tips voor het testen van de gebruikerservaring

Als u een Azure-beheerder bent en u bent aangemeld bij Azure portal met behulp van een account in de map, bent u automatisch aangemeld bij het toegangsvenster als uw huidige account. Deze weergave bevat alle toepassingen die aan u zijn toegewezen.

Om te testen een *verschillende* gebruiker account, het volgende doen:

1. Selecteer in de rechterbovenhoek van de Azure portal of in het toegangsvenster, **Afmelden**. 
2. Ga naar de [Toegangsvenster](http://myapps.microsoft.com).
3. Typ de gebruikersnaam en het wachtwoord voor het account in uw directory die u wilt testen op de aanmeldingspagina.


## <a name="starting-applications"></a>Het starten van toepassingen

Deze sectie wordt beschreven van verschillende typen toepassingen die kunnen worden weergegeven in het toegangsvenster.

### <a name="office-365-applications"></a>Office 365-toepassingen

Als uw organisatie van Office 365-toepassingen gebruikmaakt en u een licentie voor de Office 365-toepassingen worden weergegeven in het toegangsvenster.

Als u de tegel van een toepassing voor een Office 365-toepassing selecteert, wordt u omgeleid naar de toepassing en automatisch aangemeld.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Microsoft en derden door toepassingen die zijn geconfigureerd met op basis van een federatieve eenmalige aanmelding

De beheerder kan toepassingen toevoegen in de sectie Active Directory van Azure portal met de SSO-modus ingesteld op **Azure AD eenmalige aanmelding**. U kunt deze toepassingen zien alleen als uw beheerder heeft u toegang tot deze expliciet verleend.

Wanneer u een tegel voor een toepassing selecteert, kunt u bent omgeleid en automatisch aangemeld bij deze.

### <a name="password-based-sso-without-identity-provisioning"></a>Wachtwoord gebaseerde SSO zonder in te richten van identiteit

De beheerder kan toepassingen toevoegen in de sectie Active Directory van Azure portal met de SSO-modus ingesteld op **wachtwoord gebaseerde Single Sign-On**. Alle toepassingen die zijn geconfigureerd in deze modus kunnen zien door alle gebruikers in de map.

De eerste keer dat u de tegel van een toepassing, selecteert wordt u gevraagd het wachtwoord SSO-invoegtoepassing voor Internet Explorer of Chrome installeren. De installatie moet u mogelijk opnieuw opstarten van uw webbrowser. Wanneer u gaat u terug naar het toegangsvenster en de tegel van de toepassing opnieuw selecteren, wordt u gevraagd een gebruikersnaam en wachtwoord voor de toepassing. Wanneer u uw gebruikersnaam en wachtwoord hebt ingevoerd, worden de referenties veilig opgeslagen en gekoppeld aan uw account in Azure AD.

De volgende keer dat u de tegel van de toepassing, selecteert worden u automatisch aangemeld bij de toepassing.  

U hebt geen uw referenties opnieuw invoeren en of het wachtwoord SSO-invoegtoepassing installeren.

Als uw referenties in de doeltoepassing van derden zijn gewijzigd, moet u uw referenties die zijn opgeslagen in Azure AD ook bijwerken. 

Voor het bijwerken van uw referenties, het volgende doen:

1. Selecteer het pictogram op de tegel van de toepassing.
2. Selecteer **referenties bijwerken** de gebruikersnaam en het wachtwoord voor de toepassing opnieuw in te voeren.


### <a name="password-based-sso-with-identity-provisioning"></a>Eenmalige aanmelding op basis van wachtwoorden met het inrichten van identiteit

De beheerder kan toepassingen toevoegen in de sectie Active Directory van Azure portal met de SSO-modus ingesteld op **wachtwoord gebaseerde Single Sign-On**, samen met identiteit wordt ingericht.

De eerste keer dat u de tegel van een toepassing, selecteert wordt u gevraagd het wachtwoord SSO-invoegtoepassing voor Internet Explorer of Chrome installeren. De installatie moet u mogelijk opnieuw opstarten van uw webbrowser.  

Als u terug naar het toegangsvenster en de tegel van de toepassing opnieuw, selecteert u automatisch worden aangemeld bij de toepassing.

Sommige toepassingen moet u mogelijk uw wachtwoord bij de eerste aanmelding te wijzigen. Als uw referenties in de doeltoepassing van derden zijn gewijzigd, moet u de referenties die zijn opgeslagen in Azure AD ook bijwerken. 

Voor het bijwerken van uw referenties, het volgende doen:

1. Selecteer het pictogram op de tegel van de toepassing.
2. Selecteer **referenties bijwerken** de gebruikersnaam en het wachtwoord voor de toepassing opnieuw in te voeren.


### <a name="application-with-existing-sso-solutions"></a>Toepassing met bestaande oplossingen voor eenmalige aanmelding

De Azure portal biedt voor het configureren van eenmalige aanmelding voor een toepassing, een derde optie bestaande eenmalige aanmelding. Deze optie kunt de beheerder om een koppeling naar een toepassing maken en plaats deze in het toegangsvenster voor de geselecteerde gebruikers.

Bijvoorbeeld, als een toepassing is geconfigureerd om gebruikers te verifiëren met behulp van AD FS 2.0, kunt uw beheerder de optie bestaande eenmalige aanmelding gebruiken om te maken van een koppeling naar het in het toegangsvenster. Wanneer u de koppeling opent, wordt u geverifieerd via AD FS 2.0 of een andere bestaande oplossing voor eenmalige aanmelding de toepassing biedt.


## <a name="next-steps"></a>Volgende stappen

- Een lijst met alle onderwerpen met betrekking tot toepassingsbeheer, Zie de [artikelenindex voor Toepassingsbeheer in Azure Active Directory](../active-directory-apps-index.md).
 
- Zie voor meer informatie over het integreren van een SaaS-app met Azure AD, de [lijst met zelfstudies over het integreren van SaaS-apps](../saas-apps/tutorial-list.md).
 
- Zie voor meer informatie over het beheren van apps met Azure AD, de [single sign-on en beheren van app-toegang met Azure Active Directory-inleiding](../manage-apps/what-is-single-sign-on.md).
 
- Zie voor meer informatie over het inrichten van gebruikers, [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen](../active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
