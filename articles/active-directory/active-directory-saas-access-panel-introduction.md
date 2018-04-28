---
title: Wat is het toegangsvenster in Azure Active Directory? | Microsoft Docs
description: Informatie over het gebruik van variaties van het toegangsvenster (webbrowser, Android-app, iPhone en iPad app) voor toegang tot SaaS-apps.
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
ms.date: 03/06/2018
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c8ea85a0906071d6b30f176de7509e5f346b513
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="what-is-the-access-panel"></a>Wat is het toegangsvenster?

Het toegangspaneel is een webportal. Als u een werk hebt of school-account in Azure Active Directory (Azure AD), kunt u het toegangsvenster weergeven en starten van cloud-gebaseerde toepassingen die Azure AD-beheerder u toegang tot heeft verleend. U kunt ook groepsbeheer met Self-service en app-mogelijkheden voor beheer via het toegangsvenster gebruiken.

Het toegangspaneel is gescheiden van de Azure-portal. Dit vereist niet dat u een Azure-abonnement hebt.

![Toegangspaneel][1] met behulp van het toegangsvenster kunt u enkele van uw profielinstellingen bewerken en doe het volgende:

- Wijzig het wachtwoord die zijn gekoppeld aan een account voor werk of school.

- Bewerk de instellingen voor wachtwoord opnieuw instellen.

- Bewerken en neem contact op met de prioriteit van instellingen in verband met multi-factor authentication (voor accounts die zijn vereist om deze te gebruiken door een beheerder).

- Account details, zoals gebruikers-ID, alternatieve e-mailbericht, mobiele en office telefoonnummers en apparaten bekijken.

- Weergeven en starten van cloud-gebaseerde toepassingen die de Azure AD-beheerder u toegang tot heeft verleend. 

- Groepen zelf beheren. Beheerders kunnen maken en beheren van beveiligingsgroepen en beveiligingsgroepen aanvragen in Azure AD. Zie voor meer informatie [groepsbeheer met Self-service voor gebruikers in Azure AD](active-directory-accessmanagement-self-service-group-management.md) en [groepen beheren](active-directory-manage-groups.md).




## <a name="access-the-access-panel"></a>Toegang tot het toegangsvenster

U toegang hebt tot het toegangsvenster door te gaan naar `http://myapps.microsoft.com`.

Als u aangepaste huisstijl is geconfigureerd voor de aanmeldingspagina hebt, kunt u de huisstijl door het domein van uw organisatie toegevoegd aan de URL te laden (bijvoorbeeld `http://myapps.microsoft.com/<your domain>.com`).

U kunt een actieve of geverifieerde domeinnaam die is geconfigureerd in uw Azure-portal, zoals hier wordt weergegeven: ![Wingtip Toys domeinnaam][2]  

De URL voor alle gebruikers die zich aanmelden bij toepassingen die zijn geïntegreerd met Azure AD distribueren.

## <a name="authentication"></a>Verificatie

Bereiken van het toegangsvenster, moet u worden geverifieerd via een account voor werk of school in Azure AD. U kunt naar rechtstreeks Azure AD zijn geverifieerd. U kunt ook als een organisatie heeft federation geconfigureerd met behulp van Active Directory Federation Services (AD FS) of andere technologieën, kan u worden geverifieerd door Windows Server Active Directory.

Als u een abonnement voor Azure of Office 365 hebt en u hebt gebruikt de Azure portal of een Office 365-toepassing, kunt u de lijst met toepassingen weergeven zonder meldt u zich opnieuw. Als u niet bent geverifieerd, wordt u gevraagd om aan te melden met behulp van de gebruikersnaam en het wachtwoord voor uw account in Azure AD. Als uw organisatie heeft federation geconfigureerd, is typt de gebruikersnaam voldoende.

Wanneer u bent geverifieerd, kunt u communiceren met de toepassingen die de beheerder heeft geïntegreerd in de map. Zie voor meer informatie over toepassingen integreren met Azure AD, [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Vereisten voor webbrowsers

Ten minste het toegangsvenster vereist een browser die JavaScript ondersteunt en CSS is ingeschakeld. Als u wilt worden aangemeld via op basis van wachtwoorden eenmalige aanmelding (SSO) aan toepassingen, moet u de uitbreiding van toegang tot Configuratiescherm geïnstalleerd in uw browser hebben. De extensie wordt automatisch gedownload wanneer u een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden selecteert.

Het installatieprogramma is specifiek voor architectuur. Als u op de koppeling klikt, krijgt u alleen het installatieprogramma voor de OS-architectuur die momenteel worden uitgevoerd op. Als u een beheerder van de implementatie van toepassing zijn, ervoor zorgen dat u gaat u naar de downloadkoppeling vanaf een apparaat 64-bits en 32-bits installatieprogramma's van zowel ophalen.


De extensie van het Configuratiescherm toegang is momenteel beschikbaar voor:
- **Rand**: verjaardagseditie van Windows 10 of hoger. 
- **Chrome**: Windows 7 of hoger, en op Mac OS X of hoger.
- **Firefox 26,0 of hoger**: Windows XP SP2 of hoger, en op Mac OS X 10.6 of hoger.
- **Internet Explorer 11**: op Windows 7 of hoger (beperkte ondersteuning).

## <a name="my-apps-secure-sign-in-extension"></a>Extensie Veilige aanmelding bij mijn apps
Als u wilt aanmelden bij op basis van wachtwoorden eenmalige aanmelding, moet u de extensie. Nadat de uitbreiding is geïnstalleerd, kunt u aanmelden voor deze aanvullende functies inschakelen door het selecteren van **melden aan de slag**. 

- U kunt aanmelden bij een app via de app **aanmeldings-URL**. Wanneer u de app-URL, wordt de extensie detecteert de actie en biedt de mogelijkheid van aanmelding van de extensie.
- U kunt een van uw apps uit het toegangsvenster starten met behulp van de *snel zoeken* functie van de extensie. 
- De extensie ziet u de laatste drie toepassingen die u in gestart **recent gebruikte** sectie.

> [!NOTE]
> Aanvullende functies zijn alleen beschikbaar voor rand en Chrome, Firefox.
>

Als u een URL voor mijn Apps dan `https://myapps.microsoft.com`, standaard-URL configureren door het volgende te doen:
1. Terwijl u bent *niet* aangemeld bij de extensie, met de rechtermuisknop op het pictogram extensie.
2. Selecteer in het menu **URL voor mijn Apps**.
3. Selecteer uw standaard-URL.
4. Selecteer het pictogram extensie.
5. Selecteer **melden aan de slag**.

## <a name="mobile-app-support"></a>Ondersteuning voor mobiele Apps

Het team van Azure Active Directory publiceert de mobiele app voor mijn Apps. Wanneer u de app installeert, kunt u zich aanmeldt bij de SSO-toepassingen op basis van wachtwoorden op iOS en Android-apparaten.

> [!NOTE]
> U kunt aanmelden bij toepassingen die ondersteuning bieden voor federatie met Azure AD (inclusief Salesforce, Google Apps, Dropbox, Box, Concur, Workday, Office 365 en meer dan 70 anderen) op vrijwel elke webbrowser op elk apparaat zonder een invoegtoepassing of mobiele app. Moet worden gebruikt op een mobiel apparaat elkaar [toegang tot Configuratiescherm ervaringen](https://myapps.microsoft.com/) ook de mobiele app voor mijn Apps niet vereist.
>
>

### <a name="my-apps-for-android"></a>Mijn Apps voor Android

Mijn Apps voor Android wordt ondersteund op een Android-apparaat met Android versie 4.1 of hoger.  

Het is beschikbaar op de [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Mijn Apps voor Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Mijn Apps voor iPhone en iPad

Mijn Apps voor iOS wordt ondersteund op een iPhone of iPad met iOS-versie 7 of hoger.  

Het is beschikbaar op de [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Mijn Apps voor iOS][4]    


## <a name="managed-browser-for-my-apps"></a>Beheerde browser voor mijn Apps

Mijn Apps is ook worden geïntegreerd met de Intune Managed Browser. De Intune Managed Browser voor iOS en Android-apparaten speelt een belangrijke rol bij het ervoor te zorgen dat de gegevens op mobiele apparaten beveiligd blijven. De browser kunt u veilig bekijken en navigeer van webpagina's die mogelijk bedrijfsgegevens bevatten en deze biedt een veilige websurfervaring.  

U krijgt u snel toegang tot mijn Apps op uw startpagina van Managed Browser en in uw bladwijzers zodat minder klikken vereist zijn voor elke toepassing die u toegang wilt bereiken.

Door Intune beheerde Browser is beschikbaar op de [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) en [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Beheerde browser voor mijn Apps][5]    


## <a name="tips-for-testing-the-user-experience"></a>Tips voor het testen van de gebruikerservaring

Als u een Azure-beheerder bent en u bent aangemeld bij de Azure-portal met behulp van een account in de directory, bent u automatisch aangemeld voor het toegangsvenster als uw huidige account gebruiken. Deze weergave bevat alle toepassingen die aan u zijn toegewezen.

Om te testen in een *verschillende* gebruiker account, het volgende doen:

1. Selecteer op de rechterbovenhoek van de Azure-portal of in het deelvenster toegang **Afmelden**. 
2. Ga naar de [toegangspaneel](http://myapps.microsoft.com).
3. Typ de gebruikersnaam en het wachtwoord voor het account in uw directory die u wilt testen op de aanmeldingspagina.


## <a name="starting-applications"></a>Starten van toepassingen

Deze sectie wordt beschreven van verschillende typen toepassingen die kunnen worden weergegeven in het deelvenster toegang.

### <a name="office-365-applications"></a>Office 365-toepassingen

Als uw organisatie van Office 365-toepassingen gebruikmaakt en u een licentie voor Office 365-toepassingen worden weergegeven in het deelvenster toegang.

Wanneer u een tegel toepassing voor een Office 365-toepassing selecteert, wordt u omgeleid naar de toepassing en automatisch aangemeld.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Microsoft en derden toepassingen die zijn geconfigureerd op basis van een federatieve aanmelding bij

Uw beheerder kan toepassingen toevoegen in de sectie Active Directory van de Azure-portal met de SSO-modus ingesteld op **Azure AD Single Sign-On**. U kunt deze toepassingen zien alleen als de beheerder heeft u toegang tot deze expliciet gegeven.

Wanneer u een tegel voor een toepassing selecteert, wordt u omgeleid en automatisch aangemeld bij deze.

### <a name="password-based-sso-without-identity-provisioning"></a>Eenmalige aanmelding op basis van wachtwoorden zonder identiteit inrichten

Uw beheerder kan toepassingen toevoegen in de sectie Active Directory van de Azure-portal met de SSO-modus ingesteld op **op basis van wachtwoorden Single Sign-On**. Alle gebruikers in de map ziet alle toepassingen die zijn geconfigureerd in deze modus.

De eerste keer dat u een tegel toepassing selecteert wordt u gevraagd het wachtwoord SSO-invoegtoepassing voor Internet Explorer of Chrome installeren. De installatie moet u mogelijk opnieuw opstarten van uw webbrowser. Als u terug naar het toegangspaneel en de tegel toepassing opnieuw selecteert, wordt u gevraagd om een gebruikersnaam en wachtwoord voor de toepassing. Wanneer u uw gebruikersnaam en wachtwoord hebt ingevoerd, worden de referenties veilig opgeslagen en gekoppeld aan uw account in Azure AD.

De volgende keer dat u de tegel toepassing selecteert bent u automatisch aangemeld bij de toepassing.  

U hoeft niet te uw referenties opnieuw invoeren en of het wachtwoord SSO-invoegtoepassing installeren.

Als uw referenties in de derde partij doeltoepassing hebt gewijzigd, moet u ook de referenties die zijn opgeslagen in Azure AD bijwerken. 

Voor het bijwerken van uw referenties, het volgende doen:

1. Selecteer het pictogram op de tegel toepassing.
2. Selecteer **referenties bijwerken** gebruikersnaam en wachtwoord voor de toepassing opnieuw in te voeren.


### <a name="password-based-sso-with-identity-provisioning"></a>Eenmalige aanmelding op basis van wachtwoorden met het leveren van identiteit

Uw beheerder kan toepassingen toevoegen in de sectie Active Directory van de Azure-portal met de SSO-modus ingesteld op **op basis van wachtwoorden Single Sign-On**, samen met het inrichten van de identiteit.

De eerste keer dat u een tegel toepassing selecteert wordt u gevraagd het wachtwoord SSO-invoegtoepassing voor Internet Explorer of Chrome installeren. De installatie moet u mogelijk opnieuw opstarten van uw webbrowser.  

Als u terug naar het toegangspaneel en de tegel toepassing opnieuw in en selecteert u automatisch bent aangemeld bij de toepassing.

Sommige toepassingen moet u mogelijk uw wachtwoord bij de eerste aanmelding te wijzigen. Als uw referenties in de derde partij doeltoepassing hebt gewijzigd, moet u ook de referenties die zijn opgeslagen in Azure AD bijwerken. 

Voor het bijwerken van uw referenties, het volgende doen:

1. Selecteer het pictogram op de tegel toepassing.
2. Selecteer **referenties bijwerken** gebruikersnaam en wachtwoord voor de toepassing opnieuw in te voeren.


### <a name="application-with-existing-sso-solutions"></a>Toepassing met bestaande oplossingen voor eenmalige aanmelding

De Azure portal biedt voor meer informatie over het configureren van eenmalige aanmelding voor een toepassing een derde optie voor bestaande eenmalige aanmelding. Deze optie kunt de beheerder om een koppeling maken naar een toepassing op en plaats deze in het deelvenster toegang voor geselecteerde gebruikers.

Bijvoorbeeld, als een toepassing is geconfigureerd om gebruikers te verifiëren met behulp van AD FS 2.0, kunt de beheerder de optie bestaande eenmalige aanmelding gebruiken voor het maken van een koppeling in het deelvenster toegang. Wanneer u de koppeling opent, kunt u bent geverifieerd via AD FS 2.0 of een andere bestaande oplossing voor eenmalige aanmelding biedt voor de toepassing.


## <a name="next-steps"></a>Volgende stappen

- Een lijst met alle onderwerpen met betrekking tot toepassingsbeheer, Zie de [artikel index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md).
 
- Zie voor informatie over het integreren van een SaaS-app met Azure AD, de [lijst met zelfstudies over het integreren van SaaS-apps](active-directory-saas-tutorial-list.md).
 
- Zie voor meer informatie over het beheren van apps met Azure AD, de [Inleiding tot één aanmelding en beheren van app-toegang met Azure Active Directory](active-directory-appssoaccess-whatis.md).
 
- Zie voor meer informatie over gebruikers inrichten, [gebruikers inrichten en opheffen van inrichting tot SaaS-toepassingen automatiseren](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
