---
title: Toegang tot de toepassing zelf en gedelegeerd beheer met Azure Active Directory | Microsoft Docs
description: Dit artikel wordt beschreven hoe u toegang tot de toepassing zelf en gedelegeerd beheer met Azure Active Directory.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 448a7fe8-a162-475e-9ba2-2e3ab59302bc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: curtand
ms.reviewer: asmalser
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 39c62461c9659b0cb4422de88686283ba462c53b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="self-service-application-access-and-delegated-management-with-azure-active-directory"></a>Toegang tot de toepassing zelf en gedelegeerd beheer met Azure Active Directory
Het inschakelen van selfservice mogelijkheden voor eindgebruikers is een veelvoorkomend scenario voor bedrijven. Veel gebruikers, veel toepassingen en de persoon die toegang verlenen beslissingen best-informed is mogelijk niet de directory-beheerder. De beste persoon om te bepalen wie toegang heeft tot een toepassing is vaak een team lead of een andere gedelegeerde beheerder. Echter is de gebruiker die de app gebruikt en de gebruiker weet wat ze moeten mogelijk hun taak uit te voeren.

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen. 

Toegang tot de toepassing zelf is een functie van [Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/) P1 en P2 licensing die directory-beheerders toestaan:

* Gebruikers toestaan te vragen toegang tot toepassingen die gebruikmaken van een tegel 'Meer toepassingen ophalen' in de [Azure AD-Toegangsvenster](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)
* Instellen welke gebruikers toepassingen toegang tot aanvragen kunnen
* Stel al dan niet een goedkeuring vereist voor gebruikers is kunnen zichzelf toegang tot een toepassing toewijzen aan
* Set wie moet goedkeuren van de aanvragen en beheren van toegang voor elke toepassing

Vandaag de dag deze mogelijkheid wordt ondersteund voor alle vooraf geïntegreerde en aangepaste apps die ondersteuning bieden voor federatieve of op basis van wachtwoorden single sign-on in de [Azure Active Directory-toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/), met inbegrip van apps zoals Salesforce, Dropbox, Google Apps, en meer.
Dit artikel wordt beschreven hoe:

* Toegang tot toepassingen selfservice voor eindgebruikers, inclusief het configureren van een optionele goedkeuringswerkstroom configureren 
* Delegeren van beheer van toegang voor specifieke toepassingen naar de meest geschikte mensen in uw organisatie en ze toegangsaanvragen goedkeuren, rechtstreeks toegang tot de geselecteerde gebruikers toewijzen of referenties voor (optioneel) instellen met het Azure AD-Toegangsvenster inschakelen toegang tot toepassingen wanneer op basis van wachtwoorden eenmalige aanmelding is geconfigureerd

## <a name="configuring-self-service-application-access"></a>Toegang tot selfservice-toepassingen configureren
Toegang tot de toepassing zelf inschakelen en welke toepassingen kunnen worden toegevoegd of aangevraagd door uw eindgebruikers, volgt u deze instructies geconfigureerd.

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com/).

2.   Onder de **Active Directory** sectie, selecteer uw directory en selecteer vervolgens de **toepassingen** tabblad. 

3. Selecteer de **toevoegen** knop en de galerijoptie gebruiken om te selecteren en een toepassing toevoegen.

4. Nadat uw app is toegevoegd, krijgt u de pagina snel starten. Klik op **configureren Single Sign-On**, selecteer de gewenste eenmalige aanmelding modus en de configuratie op te slaan. 

5. Selecteer vervolgens de **configureren** tabblad. Instellen zodat gebruikers om toegang te vragen naar deze app van het Azure AD-Toegangsvenster **toestaan van toegang tot de toepassing zelf** naar **Ja**.
  
  ![][1]

6. Als u wilt configureren (optioneel) een goedkeuringswerkstroom voor toegangsaanvragen, stel **moeten worden goedgekeurd voordat u toegang verleent** naar **Ja**. Een of meer goedkeurders kunnen worden geselecteerd met de **goedkeurders** knop.

  Een goedkeurder kan elke gebruiker in de organisatie met een Azure AD-account, en kan zijn verantwoordelijk voor het account inrichten, licenties, of andere bedrijfsproces uw organisatie vereist voordat het verlenen van toegang tot een app. De goedkeurder kan zelfs de eigenaar van de groep van een of meer groepen gedeeld en de gebruikers toewijzen aan een van deze groepen om hem toegang via een gedeeld account. 

  Als geen goedkeuring vereist is, klikt u vervolgens krijgen onmiddellijk gebruikers de toepassing die is toegevoegd aan hun Azure AD-Toegangsvenster. Als de toepassing is ingesteld voor [automatisch gebruikers inrichten](active-directory-saas-app-provisioning.md), of is ingesteld ['gebruikers wordt beheerd' wachtwoord SSO modus](active-directory-appssoaccess-whatis.md#password-based-single-sign-on), de gebruiker moet een gebruiker account en het wachtwoord weten al hebben.

7. Als de toepassing is geconfigureerd voor gebruik op basis van wachtwoorden eenmalige aanmelding en vervolgens een optie voor het toestaan van de goedkeurder de SSO-referenties voor elke gebruiker in te stellen is ook beschikbaar. Zie voor meer informatie het gedeelte over [gedelegeerd beheer van toegang](#delegated-application-access-management).

8. Ten slotte de **groep voor Self-Assigned gebruikers** ziet u de naam van de groep die wordt gebruikt voor het opslaan van de gebruikers die zijn verleend of toegewezen toegang tot de toepassing. De goedkeurder toegang wordt de eigenaar van deze groep. Als de weergegeven naam van de groep niet bestaat, wordt deze automatisch gemaakt. Naam van de groep kan desgewenst worden ingesteld op de naam van een bestaande groep.

9. Om de configuratie op te slaan, klikt u op **opslaan** aan de onderkant van het scherm. Gebruikers kunnen nu op aanvraag toegang tot deze app vanuit het deelvenster toegang.

10. Om te proberen de gebruikerservaring, je aanmelden bij van uw organisatie Azure AD-Toegangsvenster op https://myapps.microsoft.com, bij voorkeur met een ander account die niet een goedkeurder app. 

11. Onder de **toepassingen** en klik op de **meer toepassingen ophalen** tegel. Deze tegel worden weergegeven voor tal van alle toepassingen die zijn ingeschakeld voor toegang tot selfservice toepassingen in de map en de mogelijkheid om te zoeken en filteren op categorie app aan de linkerkant. 

12. Het serversysteemstatus van het proces voor aanvragen te klikken op een app. Als er geen goedkeuringsproces vereist is, wordt de toepassing wordt onmiddellijk toegevoegd onder de **toepassingen** tabblad na een korte bevestiging. Goedkeuring is vereist, vervolgens ziet u een dialoogvenster waarmee dit wordt aangegeven als een e-mailbericht wordt verzonden naar goedkeurders. U moet worden aangemeld bij het toegangsvenster als een niet-goedkeurder om te zien van dit proces voor aanvragen.

13. Het e-mailbericht stuurt de goedkeurder te melden bij de Azure AD-Toegangsvenster en de aanvraag goedkeuren. Zodra de aanvraag is goedgekeurd (en eventuele speciale processen die u definieert zijn uitgevoerd door de fiatteur), ziet de gebruiker de toepassing worden weergegeven onder hun **toepassingen** tabblad waar ze kunnen zich in de App.

## <a name="delegated-application-access-management"></a>Gedelegeerde toegang voor toepassingsbeheer
Een toepassing toegang goedkeurder kan elke gebruiker in uw organisatie die het meest geschikte persoon goedkeuren of weigeren van toegang tot de betreffende toepassing zijn. Deze gebruiker kan verantwoordelijk voor het account inrichten, licenties, of andere bedrijfsproces uw organisatie nodig heeft voor het verlenen van toegang tot een app.

Bij het configureren van selfservice toepassing toegang die hierboven worden beschreven, een toegewezen toepassing goedkeurders Zie een extra **toepassingen beheren** tegel in het deelvenster van de toegang tot Azure AD, waarin u ziet u welke toepassingen die ze de de beheerder van de toegang voor. Te klikken op een app, wordt een scherm met verschillende opties weergegeven.

![][2]

### <a name="approve-requests"></a>Goedkeuren van aanvragen
De **aanvragen goedkeuren** tegel kunt goedkeurders om weer te geven in afwachting van goedkeuring die specifiek zijn voor die app en wordt omgeleid naar het tabblad goedkeuringen waarbij de aanvragen kunnen worden bevestigd of geweigerd. De goedkeurder ontvangt ook automatische e-mailberichten wanneer een aanvraag wordt gemaakt zodat deze wat te doen.

### <a name="add-users"></a>Gebruikers toevoegen
De **gebruikers toevoegen** tegel kunt goedkeurders rechtstreeks geselecteerde gebruikers toegang verlenen tot de toepassing. Wanneer u op deze tegel klikt, ziet de goedkeurder dat een dialoogvenster ze kan bekijken en zoeken naar gebruikers in de directory. Toevoegen van de resultaten van een gebruiker in de toepassing wordt weergegeven in deze gebruiker Azure AD access panelen of Office 365. Als u een handmatige gebruiker inrichtingsproces is vereist bij de app voordat de gebruiker zich kan aanmelden, kunnen de goedkeurder moet dit proces uitvoeren alvorens toegang toe te wijzen.  

### <a name="manage-users"></a>Gebruikers beheren
De **gebruikers beheren** tegel kunt goedkeurders direct bijwerken of verwijderen van welke gebruikers toegang hebben tot de toepassing. 

### <a name="configure-password-sso-credentials-if-applicable"></a>Wachtwoord SSO-referenties configureren (indien van toepassing)
De **configureren** tegel wordt alleen weergegeven als de toepassing is geconfigureerd door de IT-beheerder om te gebruiken op basis van wachtwoorden eenmalige aanmelding en de beheerder de goedkeurder de mogelijkheid krijgen om SSO wachtwoordreferenties zoals wordt beschreven eerder. Wanneer u selecteert, krijgt de goedkeurder hoe de referenties zijn doorgegeven aan toegewezen gebruikers op verschillende manieren:

![][3]

* **Gebruikers zich aanmelden met hun eigen wachtwoorden** : In deze modus wordt het toegewezen gebruikers weten wat de gebruikersnamen en wachtwoorden voor de toepassing zijn en vraag ze bij hun eerste aanmelden bij de toepassing invoeren. Het scenario komt overeen met het wachtwoord SSO geval waarin de [gebruikers referenties beheren](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Gebruikers automatisch afgemeld bij het gebruik van afzonderlijke accounts die ik beheer** : In deze modus kan de toegewezen gebruikers hoeven niet in te voeren of hun referenties app-specifiek weten wanneer aanmeldt bij de toepassing. In plaats daarvan de goedkeurder stelt de referenties voor elke gebruiker na het toewijzen van toegang met behulp van de **gebruiker toevoegen** tegel. Wanneer de gebruiker op de toepassing in het deelvenster toegang of Office 365 klikt, worden ze automatisch afgemeld bij het gebruik van de referenties die zijn ingesteld door de goedkeurder. Het scenario komt overeen met het wachtwoord SSO geval waarin de [beheerders referenties beheren](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Gebruikers worden automatisch aangemeld met een enkele account die ik beheer** -een speciaal geval deze aanvraag is geschikt voor gebruik wanneer alle toegewezen gebruikers moeten met een enkele account voor gedeelde toegang te krijgen. De meest voorkomende gebruik voor deze functie wordt met sociale mediatoepassingen, waarbij een organisatie heeft een enkele ' ' bedrijfsaccount en meerdere gebruikers nodig hebben om updates te dat account. Het scenario is ook komt overeen met het wachtwoord SSO geval waarin de [beheerders referenties beheren](active-directory-appssoaccess-whatis.md#password-based-single-sign-on). Echter, nadat u deze optie selecteert, de goedkeurder wordt gevraagd naar de gebruikersnaam en wachtwoord invoeren voor het één gedeelde account. Als voltooid, worden alle toegewezen gebruikers zijn ondertekend in dit account wordt gebruikt wanneer op de toepassing in hun Azure AD access panelen of Office 365 te klikken.

## <a name="additional-resources"></a>Aanvullende resources
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)

<!--Image references-->
[1]: ./media/active-directory-self-service-application-access/ssaa_admin.PNG
[2]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app.PNG
[3]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app_config.PNG
