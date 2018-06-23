---
title: Kiezen welk toepassingstype moet worden gebruikt wanneer een toepassing toevoegen | Microsoft Docs
description: Inzicht in de ondersteunde typen toepassingen die u met Azure AD integreren kunt en hun bijbehorende configuratie-opties
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 6ee5be99a64ee6e637501534e0a152c6968c380f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36331305"
---
# <a name="how-to-choose-which-application-type-to-use-when-adding-an-application"></a>Kiezen welk toepassingstype moet worden gebruikt wanneer een toepassing toevoegen

In dit artikel kunt u inzicht in de belangrijkste vier typen toepassingen die u met Azure AD integreren kunt:

* Wat wordt ondersteund door elk van deze
* Waarom u kunt ervoor kiezen welke toepassing
* Het configureren van deze toepassing core eigenschappen, zoals hoe gebruikers worden **ingericht**, of wat **eenmalige aanmelding** technologie gebruiken.

## <a name="supported-application-types-in-azure-ad"></a>De soorten ondersteunde toepassingen in Azure AD

Azure AD ondersteunt vier hoofdtoepassing typen die u kunt toevoegen met behulp van de **toevoegen** functie gevonden onder **bedrijfstoepassingen**. Deze omvatten:

-   **Azure AD-galerie toepassingen** : een toepassing die vooraf geïntegreerde voor eenmalige aanmelding met Azure AD is.

-   **Application Proxy toepassingen** : een toepassing die wordt uitgevoerd in uw on-premises omgeving die u wilt extern beveiligde eenmalige aanmelding op te geven.

-   **Aangepaste ontwikkelde toepassingen** : een toepassing die uw organisatie wil ontwikkelen op het Azure AD-toepassing ontwikkelplatform, maar die mogelijk niet bestaat.

-   **Niet-galerie toepassingen** : Breng uw eigen toepassingen! Een webkoppeling die u wilt dat alle toepassingen die een veld de gebruikersnaam en wachtwoord worden SAML of OpenID Connect-protocollen ondersteunt of ondersteunt SCIM die u wilt integreren voor eenmalige aanmelding met Azure AD.

## <a name="features-and-capabilities-supported-by-all-the-preceding-application-types"></a>Functies en mogelijkheden die worden ondersteund door de voorgaande toepassingstypen

De volgende functies worden ondersteund door een van de voorgaande vier toepassingstypen in Azure AD:

-   **Snel starten** – snel aan de slag met een toepassing door [eenvoudige implementatiestappen](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)

-   **Algemene eigenschappen management** : ophalen van een [direct deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) in een toepassing, [de huisstijl aanpassen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) van een toepassing of [uitschakelen van de toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) voor alle gebruikers.

-   **Gebruiker-en groepsbeheer** – [toewijzen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) of [verwijderen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) gebruikers en groepen in een toepassing, en kunt u de specifieke toepassingsrollen toewijzen deze gebruikers en groepen toegang hebben tot

-   **Toegang tot de toepassing zelf** – zodat uw gebruikers om aan te vragen [toegang tot de toepassing zelf](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tot een toepassing van de toepassing toegang panelen ofwel door een toepassing toevoegen rechtstreeks of [ toevoegen aan een Selfservice ingeschakeld groep](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management)desgewenst langs de manier waarop bedrijven als goedkeuring wordt vereist

-   **Aanmelden logboeken** – Zie [alle de aanmeldingen tot een toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), of al uw toepassingen

-   **Controlelogboeken** – Zie [controlelogboeken over wijzigingen in een toepassing gedetailleerde](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), of naar al uw toepassingen

-   **Voorwaardelijke en risico gebaseerde toegang** – Stel krachtige [regels voor toegang op basis van een voorwaarde](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) die worden afgedwongen wanneer gebruikers proberen aan te melden bij een bepaalde toepassing

-   **Machtigingen weergave** – weergeven van de [OAuth2 machtigingen](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) een toepassing toegang heeft tot plaats in de directory van één

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Eenmalige aanmelding en modi die worden ondersteund door specifieke toepassingstypen inrichten

De volgende tabel beschrijft de verschillende eenmalige aanmelding en inrichting modi ondersteund door elk van de voorgaande toepassingstypen. U kunt deze tabel gebruiken om u te helpen te begrijpen welke toepassing die u toevoegen wilt ter ondersteuning van een specifiek doel.

  ![Tabel van App-typen](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Het kiezen van een modus voor eenmalige aanmelding

Hieronder vindt u de ondersteunde **eenmalige aanmelding** modi voor Azure AD-toepassingen.

-   **Azure AD eenmalige aanmelding uitgeschakeld** – Kies Azure AD eenmalige aanmelding uitgeschakeld **modus voor één aanmelding** als u nog niet klaar voor de integratie van deze toepassing met eenmalige aanmelding met Azure AD of gewoon test het uit

-   **Eenmalige aanmelding gekoppeld** – Kies de [gekoppelde aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** als u een toepassing die al is gekoppeld aan een bestaande eenmalige aanmelding oplossing hebt of als u wilt publiceren van een eenvoudige koppeling voor uw gebruikers in hun [toepassing Toegangspaneel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) of [startprogramma voor toepassingen van Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Op basis van wachtwoorden aanmelding** – Kies de [op basis van wachtwoorden aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** als een HTML-gebruikersnaam en wachtwoord veld Hiermee maakt u uw toepassing en u wilt dat gebruikersnaam opslaan en wachtwoord voor het veilig naar de toepassing later worden cookies

-   **Op basis van SAML aanmelding** – Kies de [op basis van SAML aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) eenmalige aanmelding modus als uw toepassing de SAML- of OpenID Connect-protocollen ondersteunt of u wilt kunnen gebruikers toewijzen aan de rollen van de specifieke toepassing is op basis van regels u definieert in het SAML-claims *

   >[!NOTE]
   >Deze optie is niet beschikbaar wanneer de toepassingsproxy is geconfigureerd voor een toepassing.
   >
   >

-   **Op basis van een koptekst aanmelding** : Kies deze optie [headers gebaseerde aanmelding](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) aanmelding modus voor één hebt u een toepassing met PingAccess die ondersteuning biedt voor verificatie op basis van HTTP-header die u wilt uitvoeren van eenmalige aanmelding bij 

   >[!NOTE]
   >Deze optie is alleen beschikbaar wanneer de toepassingsproxy en PingAccess is geconfigureerd voor een toepassing.
   >
   >

-   **Geïntegreerde Windows-verificatie** – Kies de [geïntegreerde Windows-verificatie](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) eenmalige aanmelding bij het blootstellen van een on-premises WIA-toepassing die u wilt uitvoeren van eenmalige aanmelding bij 

   >[!NOTE]
   >Deze optie is alleen beschikbaar wanneer de toepassingsproxy is geconfigureerd voor een toepassing.
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Modi voor eenmalige aanmelding voor aangepaste ontwikkelde toepassingen

Toepassingen hebt u aangepaste ontwikkeld via de [ontwikkelde aangepaste toepassing](#_Custom-Developed_Applications) ervaring bieden ook ondersteuning voor extra eenmalige aanmelding modi niet eerder is vermeld, waaronder:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) op basis van eenmalige aanmelding

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) op basis van eenmalige aanmelding

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) op basis van eenmalige aanmelding

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) op basis van eenmalige aanmelding

Lees de [ontwikkelaarshandleiding Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) voor meer informatie over het maken van een toepassing met aangepaste ontwikkeld die ondersteuning biedt voor deze modi voor eenmalige aanmelding.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Het instellen van een toepassing aanmelding modus voor één

Instellen van een toepassing **eenmalige aanmelding** modus, volgt u deze instructies:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing waarvoor u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op **eenmalige aanmelding** in de toepassing linkermenubalk menu.

## <a name="how-to-choose-a-provisioning-mode"></a>Een inrichtingsmethode kiezen

-   **Handmatige inrichting** – Kies de [handmatige](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) Inrichtingsmethode als u bestaande accounts hebt of wilt voor deze toepassing buiten Azure AD-accounts beheren.

-   **Automatische inrichting** – Kies de [automatische](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **inrichtingsmodus** als u wilt automatische inrichting op basis van een API en/of de inrichting van gebruikersaccounts aan deze toepassing inschakelen 

   >[!NOTE]
   >Deze optie is alleen beschikbaar voor toepassingen binnen de **aanbevolen** categorie van de [Azure AD-Toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).
   >
   >

-   **Automatische inrichting op basis van SCIM** – gebruiken [automatische inrichting op basis van SCIM](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) als uw toepassing ondersteuning biedt voor het protocol SCIM voor het detecteren van wijzigingen in gebruikers en groepen die automatisch worden verzonden voor wijzigingen in elke toepassing die is geïntegreerd met Azure AD 

   >[!NOTE]
   >Deze optie wordt niet vermeld als een bepaalde Inrichtingsmethode, maar is standaard ingeschakeld voor alle toepassingen die zijn geïntegreerd met Azure AD.
   >
   >

## <a name="how-to-set-an-applications-provisioning-mode"></a>Het instellen van een toepassing de Inrichtingsmethode

Instellen van een toepassing **inrichting** modus, volgt u deze instructies:

Instellen van een toepassing **eenmalige aanmelding** modus, volgt u deze instructies:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van de belangrijkste links navigatiemenu.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van de linkernavigatiebalk Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing waarvoor u wilt inrichten.

7.  Nadat de toepassing wordt geladen, klikt u op **inrichten** in de toepassing linkermenubalk menu.

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](manage-apps/what-is-application-management.md)
