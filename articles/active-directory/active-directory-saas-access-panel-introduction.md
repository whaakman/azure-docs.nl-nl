---
title: Wat is het toegangsvenster in Azure Active Directory? | Microsoft Docs
description: Informatie over het gebruik van variaties van het toegangsvenster (webbrowser, Android-app, iPhone en iPad app) voor toegang tot SaaS-apps.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b5c139766af9e166b12e8833c2ced8be08e743a
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="what-is-the-access-panel"></a>Wat is het toegangsvenster?

Het toegangspaneel is een webportal. Dit kan een gebruiker met een werk of schoolaccount in Azure Active Directory om te bekijken en cloud-gebaseerde toepassingen start een Azure AD-beheerder heeft deze toegang verleend tot. U kunt ook groepsbeheer met Self-service en app-mogelijkheden voor beheer via het toegangsvenster gebruiken.

Het toegangspaneel is gescheiden van de Azure-portal en komt niet voor u om een Azure-abonnement.

![Toegangsvenster][1]

Het toegangsvenster kunt u enkele van uw profielinstellingen, inclusief de mogelijkheid om te bewerken:

- Wijzig het wachtwoord die zijn gekoppeld aan een account voor werk of school

- Bewerken van instellingen voor wachtwoord opnieuw instellen

- Bewerken en neem contact op met de prioriteit van instellingen in verband met multi-factor authentication (voor accounts die zijn vereist om deze te gebruiken door een beheerder)

- Account details, zoals gebruikers-ID, alternatieve e-mailadres en mobile en office telefoonnummers en apparaten bekijken

- Weergeven en cloud-gebaseerde toepassingen die de Azure AD-beheerder heeft deze toegang verleend tot starten. Zie voor meer informatie over het toegangsvenster vanuit het perspectief van de gebruikers met behulp van het toegangsvenster. 

- Groepen zelf beheren. Meer specifiek, kan de beheerder maken en beheren van beveiligingsgroepen en aanvragen beveiligingsgroepen in Azure AD. Zie voor meer informatie [groepsbeheer met Self-service voor gebruikers in Azure AD](active-directory-accessmanagement-self-service-group-management.md) en [groepen beheren](active-directory-manage-groups.md).




## <a name="accessing-the-access-panel"></a>Toegang tot het toegangsvenster

U kunt toegang krijgen tot het toegangsvenster via de volgende URL in een webbrowser:`http://myapps.microsoft.com`

Als u aangepaste huisstijl is geconfigureerd voor de aanmeldingspagina hebt, kunt u deze huisstijl door het domein van uw organisatie aan het einde van de URL toe te voegen kunt laden:`http://myapps.microsoft.com/<your domain>.com`

In dit geval kunt u een actieve of een geverifieerde domeinnaam die is geconfigureerd in uw Azure-portal.

![Wingtip Toys domeinnaam][2]  

U moet de URL voor alle gebruikers die zullen zich aanmelden bij toepassingen die zijn geïntegreerd met Azure AD te verdelen.

## <a name="authentication"></a>Verificatie

Bereiken van het toegangsvenster, moet u worden geverifieerd via een account voor werk of school in Azure AD. U kunt naar rechtstreeks Azure AD zijn geverifieerd. U kunt ook als een organisatie heeft federation geconfigureerd met behulp van Active Directory Federation Services (AD FS) of andere technologieën, kan u worden geverifieerd door Windows Server Active Directory.

Als u een abonnement voor Azure of Office 365 hebt en u hebt gebruikt de Azure portal of een Office 365-toepassing, ziet u de lijst met toepassingen zonder aanmelden opnieuw. Als u niet bent geverifieerd u wordt gevraagd om aan te melden met behulp van de gebruikersnaam en het wachtwoord voor uw account in Azure AD. Als uw organisatie heeft federation geconfigureerd, is typt de gebruikersnaam voldoende.

Wanneer u bent geverifieerd, kunt u communiceren met de toepassingen die de beheerder heeft geïntegreerd in de map. Zie voor meer informatie over toepassingen integreren met Azure AD, [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Vereisten voor webbrowsers

Ten minste het toegangsvenster vereist een browser die JavaScript ondersteunt en CSS is ingeschakeld. Voor de gebruiker zijn aangemeld via op basis van wachtwoorden eenmalige aanmelding (SSO) aan toepassingen, moet de extensie van het Configuratiescherm toegang in uw browser worden geïnstalleerd. De extensie wordt automatisch gedownload wanneer u een toepassing die is geconfigureerd voor eenmalige aanmelding op basis van wachtwoorden selecteert.

De extensie van het Configuratiescherm toegang is momenteel beschikbaar voor:
-   Rand verjaardagseditie van Windows 10 of hoger 

-   Chrome--Op Windows 7 of hoger, en op Mac OS X of hoger

-   Firefox 26,0 of later--op Windows XP SP2 of hoger, en op Mac OS X 10,6 of hoger

-   Internet Explorer 8, 9, 10, 11--op Windows 7 of hoger (beperkte ondersteuning)

## <a name="my-apps-secure-sign-in-extension"></a>Extensie Veilige aanmelding bij mijn apps
De extensie is vereist voor gebruikers die zich aanmeldt bij op basis van wachtwoorden eenmalige aanmelding. Zodra de geïnstalleerde gebruikers ook aanvullende functies kunnen inschakelen door in te loggen de extensie door te klikken op **melden aan de slag**. 

- Kunnen gebruikers zich aanmelden in apps via de app **aanmeldings-URL**. Wanneer de gebruiker naar de app aanmeldings-URL navigeert wordt de extensie detecteert DPM dit en wordt de optie voor de gebruiker zich aanmeldt bij deze van de extensie.
- Gebruikers kunnen hun apps van de toegang Configuratiescherm met behulp ook openen de **snel zoeken** functie van de extensie. 
- De uitbreiding wordt ook weergegeven voor gebruikers de laatste drie toepassingen die gestart onder **recent gebruikte** sectie.

> [!NOTE]
> Aanvullende functies zijn alleen beschikbaar voor rand en Chrome, Firefox.
>
>

Als u van een andere mijn Apps URL dan https://myapps.microsoft.com gebruikmaakt vervolgens moet u uw standaard-URL echter de volgende stappen uit:
1. Terwijl niet aangemeld bij de extensie **Klik met de rechtermuisknop** het extensie-pictogram.
2. Klik op **URL voor mijn Apps selecteren** in het menu.
3. **Selecteer** uw standaard-URL.
4. Klik op het pictogram extensie.
5. Aanmelden bij de extensie door te selecteren **melden aan de slag**.

## <a name="mobile-app-support"></a>Ondersteuning voor mobiele Apps

Het team van Azure Active Directory publiceert de mijn mobiele app van apps. Wanneer u de app installeert, kunt u zich aanmeldt bij de SSO-toepassingen op basis van wachtwoorden op iOS en Android-apparaten.

> [!NOTE]
> U kunt aanmelden bij toepassingen die ondersteuning bieden voor federatie met Azure AD (inclusief Salesforce, Google Apps, Dropbox, Box, Concur, Workday, Office 365 en meer dan 70 anderen) op vrijwel elke webbrowser op elk apparaat zonder een invoegtoepassing of mobiele app. Alle andere [toegang tot Configuratiescherm ervaringen](https://myapps.microsoft.com/) ook hoeven niet de mijn mobiele app van apps moet worden gebruikt op een mobiel apparaat.
>
>

### <a name="my-apps-for-android"></a>Mijn apps voor Android

Mijn apps voor Android wordt ondersteund op een Android-apparaat met Android versie 4.1 en hoger.  
Is beschikbaar in de [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Mijn apps voor Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Mijn apps voor iPhone en iPad

Mijn apps voor iOS wordt ondersteund op een iPhone of iPad met versie voor iOS 7 en hoger.  
Is beschikbaar in de [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Mijn apps voor iOS][4]    



## <a name="managed-browser-for-my-apps"></a>Beheerde browser voor mijn apps

Mijn apps is ook geïntegreerd in de Intune Managed Browser. De Intune Managed Browser voor iOS en Android-apparaten speelt een belangrijke rol om ervoor te zorgen dat de gegevens op mobiele apparaten beveiligd blijven. Hiermee kunt u veilig kunt weergeven en webpagina's die mogelijk bedrijfsgegevens bevatten en biedt een veilige websurfervaring.  
U snel toegang tot mijn apps op uw startpagina van Managed Browser en in uw bladwijzers, zodat u minder klikken bereiken van elke toepassing die u wilt openen.

Is beschikbaar in de [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) en [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Mananged browser voor mijn apps][5]    





## <a name="tips-for-testing-the-user-experience"></a>Tips voor het testen van de gebruikerservaring

Als u een Azure-beheerder bent en u bent aangemeld bij de Azure-portal met behulp van een account in de directory, bent u automatisch aangemeld voor het toegangsvenster als uw huidige account gebruiken. In dit geval ziet u alle toepassingen die aan u zijn toegewezen.

**Voor testen als een *verschillende* gebruikersaccount:**

1. Klik op het gebruikersmenu in de rechterbovenhoek van de Azure-portal of in het deelvenster toegang en selecteer vervolgens **Afmelden**. 
2. Ga naar de [toegangspaneel](http://myapps.microsoft.com).
3. Typ de gebruikersnaam en het wachtwoord voor het account in uw directory die u wilt testen op de aanmeldingspagina.


## <a name="starting-applications"></a>Starten van toepassingen

Verschillende soorten toepassingen kunnen worden weergegeven in het deelvenster toegang.

### <a name="office-365-applications"></a>Office 365-toepassingen

Als uw organisatie van Office 365-toepassingen gebruikmaakt en u een licentie voor Office 365-toepassingen worden weergegeven in het deelvenster toegang.

Als u op de tegel van een toepassing voor een Office 365-toepassing, wordt u omgeleid naar de toepassing en automatisch aangemeld.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Microsoft en derden toepassingen die zijn geconfigureerd op basis van een federatieve aanmelding bij

Uw beheerder kan toepassingen toevoegen in de sectie Active Directory van de Azure-portal met de SSO-modus ingesteld op **Azure AD Single Sign-On**. U kunt deze toepassingen alleen zien als de beheerder heeft u toegang tot de toepassingen expliciet verleend.

Als u op een tegel voor een van deze toepassingen, wordt u omgeleid en automatisch aangemeld bij de toepassing.

### <a name="password-based-sso-without-identity-provisioning"></a>Eenmalige aanmelding op basis van wachtwoorden zonder identiteit inrichten

Uw beheerder kan toepassingen toevoegen in de sectie Active Directory van de Azure-portal met de SSO-modus ingesteld op **op basis van wachtwoorden Single Sign-On**. Alle gebruikers in de map ziet alle toepassingen die zijn geconfigureerd in deze modus.

De eerste keer u klikken op een tegel voor een van deze toepassingen u wordt gevraagd het wachtwoord SSO-invoegtoepassing voor Internet Explorer of Chrome installeren. De installatie moet u mogelijk opnieuw opstarten van uw webbrowser. Wanneer u naar het toegangspaneel terugkeert en klikt u nogmaals op de tegel toepassing, wordt u gevraagd om een gebruikersnaam en wachtwoord voor de toepassing. Wanneer u uw gebruikersnaam en wachtwoord hebt ingevoerd, worden deze referenties veilig opgeslagen en gekoppeld aan uw account in Azure AD.

De volgende keer dat u op de tegel toepassing bent u automatisch aangemeld bij de toepassing.  
U hoeft niet te uw referenties opnieuw invoeren en of het wachtwoord SSO-invoegtoepassing installeren.

Als uw referenties in de derde partij doeltoepassing hebt gewijzigd, moet u ook de referenties die zijn opgeslagen in Azure AD bijwerken. 

**Referenties bijwerken:**

1. Selecteer het pictogram op de tegel toepassing.
2. Selecteer **referenties bijwerken** gebruikersnaam en wachtwoord voor de toepassing opnieuw in te voeren.


### <a name="password-based-sso-with-identity-provisioning"></a>Eenmalige aanmelding op basis van wachtwoorden met het leveren van identiteit

Uw beheerder kan toevoegen toepassingen in de **Active Directory** sectie van de Azure-portal met de SSO-modus ingesteld op **op basis van wachtwoorden Single Sign-On**, samen met het inrichten van de identiteit.

De eerste keer gebruikt, u klikt op de tegel van een toepassing voor een van deze toepassingen, wordt u gevraagd voor het installeren van de **wachtwoord SSO-invoegtoepassing voor Internet Explorer of Chrome**. De installatie moet u mogelijk opnieuw opstarten van uw webbrowser.  
Wanneer u naar het toegangspaneel terugkeert en klikt u op de tegel toepassing opnieuw, bent u automatisch aangemeld bij de toepassing.

Sommige toepassingen moet u mogelijk uw wachtwoord op de eerste aanmeldingspagina te wijzigen. Als uw referenties in de derde partij doeltoepassing hebt gewijzigd, moet u ook de referenties die zijn opgeslagen in Azure AD bijwerken. 

**Referenties bijwerken:**

1. Selecteer het pictogram op de tegel toepassing.
2. Selecteer **referenties bijwerken** gebruikersnaam en wachtwoord voor de toepassing opnieuw in te voeren.


### <a name="application-with-existing-sso-solutions"></a>Toepassing met bestaande oplossingen voor eenmalige aanmelding

Voor meer informatie over het configureren van eenmalige aanmelding voor een toepassing de Azure portal biedt een derde optie, genaamd **bestaande Single Sign-On**. Deze optie kunt de beheerder om een koppeling maken naar een toepassing op en plaats deze in het deelvenster toegang voor geselecteerde gebruikers.

Bijvoorbeeld, als een toepassing is geconfigureerd om gebruikers te verifiëren met behulp van AD FS 2.0, uw beheerder kan gebruiken de **bestaande Single Sign-On** optie voor het maken van een koppeling in het deelvenster toegang. Wanneer u de koppeling opent, kunt u bent geverifieerd via AD FS 2.0 of een andere bestaande oplossing voor eenmalige aanmelding biedt voor de toepassing.


## <a name="next-steps"></a>Volgende stappen

- Zie voor een lijst met alle onderwerpen met betrekking tot Toepassingsbeheer de [artikel index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md).
 
- Zie voor informatie over het integreren van een SaaS-app in Azure AD, de [lijst met zelfstudies over het integreren van SaaS-apps](active-directory-saas-tutorial-list.md).
 
- Zie voor meer informatie over het beheren van apps met Azure AD, de [Inleiding tot één aanmelding en beheren van app-toegang met Azure Active Directory](active-directory-appssoaccess-whatis.md).
 
- Zie voor meer informatie over gebruikers inrichten, [gebruikers inrichten en opheffen van inrichting tot SaaS-toepassingen automatiseren](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
