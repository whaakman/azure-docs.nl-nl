---
title: Kiezen welk toepassingstype moet worden gebruikt bij het toevoegen van een toepassing | Microsoft Docs
description: Informatie over de ondersteunde typen toepassingen die u met Azure AD integreren kunt en hun bijbehorende configuratie-opties
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: barbkess
ms.openlocfilehash: 10f2b30addd874d39a6193c810ccbdf4ac1a06e6
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54473313"
---
# <a name="choosing-the-application-type-when-adding-an-application-in-azure-active-directory"></a>Het toepassingstype kiezen bij het toevoegen van een toepassing in Azure Active Directory
Meer informatie over de vier typen toepassingen die u aan Azure Active Directory (Azure AD toevoegen kunt). Wanneer u een toepassing in Azure Active Directory toevoegt, wordt u gevraagd om een van de vier toepassingstype. 

## <a name="what-are-the-types-of-applications"></a>Wat zijn de typen toepassingen?

Azure AD biedt ondersteuning voor vier belangrijkste toepassingstypen die u kunt toevoegen met behulp van de **toevoegen** functie vinden onder **bedrijfstoepassingen**. Deze omvatten:

-   **Azure AD-Galerietoepassingen** : een toepassing die vooraf geïntegreerde voor eenmalige aanmelding met Azure AD is.

-   **Application Proxy toepassingen** : een toepassing die wordt uitgevoerd in uw on-premises-omgeving die u wilt extern veilige eenmalige aanmelding op te geven.

-   **Speciaal ontwikkelde toepassingen** : een toepassing die uw organisatie wil ontwikkelen op Azure AD Application Development Platform, maar die niet nog kan bestaan.

-   **Niet-Galerietoepassingen** : Breng uw eigen toepassingen. Een webkoppeling die u wilt dat elke toepassing die het veld gebruikersnaam en wachtwoord, wordt weergegeven ondersteuning biedt voor SAML- of OpenID Connect-protocol of SCIM die u wilt integreren voor eenmalige aanmelding met Azure AD ondersteunt.

## <a name="features-and-capabilities-supported-by-the-application-types"></a>Functies en mogelijkheden die worden ondersteund door de soorten toepassingen

De volgende functies worden ondersteund door een van de voorgaande vier typen in Azure AD:

-   **Quick start-** – snel aan de slag met een toepassing door [eenvoudige implementatiestappen](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)

-   **Algemene eigenschappen management** – krijgen een [direct deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) aan een toepassing, [de huisstijl aanpassen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) van een toepassing, of [uitschakelen van de toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) voor alle gebruikers.

-   **Beheer van gebruikers en groepen** – [toewijzen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) of [verwijderen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) gebruikers en groepen met een toepassing, en kunt u de specifieke toepassingsrollen toewijzen deze gebruikers en groepen toegang hebben tot

-   **Toegang tot Self-servicetoepassingen** – uw gebruikers te vragen [toegang tot Self-servicetoepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) bij een toepassing uit de Toegangsdeelvensters van hun toepassing ofwel met toevoegen van een toepassing rechtstreeks of [ toevoegen aan een Selfservice ingeschakeld groep](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), eventueel moeten worden goedgekeurd langs de manier waarop bedrijven

-   **Meld u in logboeken** – Zie [alle aanmeldingen bij een toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), of al uw toepassingen

-   **Auditlogboeken** – Zie [gedetailleerde auditlogboeken over wijzigingen in een toepassing](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), of al uw toepassingen

-   **Toegang tot voorwaardelijke en risico's gebaseerde** : Stel krachtige [voorwaarde op basis van toegangsregels](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) die worden afgedwongen wanneer gebruikers proberen aan te melden bij een bepaalde toepassing

-   **Machtigingen weergeven** – bekijken een van de [OAuth2 machtigingen](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) een toepassing heeft toegang tot het in uw directory van één plaats

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Eenmalige aanmelding en modi die worden ondersteund door specifieke toepassingstypen inrichten

De volgende tabel beschrijft de verschillende single sign-on en inrichting modi ondersteund door elk van de voorgaande toepassingstypen. U kunt deze tabel om u te begrijpen welke toepassing die u toevoegen wilt ter ondersteuning van een bepaald doel te gebruiken.

  ![Tabel met typen App](./media/choose-application-type/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Het kiezen van een modus voor eenmalige aanmelding

Hieronder vindt u de ondersteunde **eenmalige aanmelding** modi voor Azure AD-toepassingen.

-   **Azure AD eenmalige aanmelding uitgeschakeld** – Kies Azure AD eenmalige aanmelding uitgeschakeld **modus voor één aanmelding** als u nog niet gereed voor het integreren van deze toepassing met eenmalige aanmelding met Azure AD, of gewoon test het uit

-   **Gekoppelde aanmelding** : Kies de [gekoppelde Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** hebt u een toepassing die al is verbonden met een bestaande eenmalige aanmelding, of als u alleen wilt publiceren van een eenvoudige koppeling voor uw gebruikers in hun [Toegangsvenster](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) of [startprogramma voor Office 365-toepassingen](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Op basis van wachtwoorden Sign-on** : Kies de [op basis van wachtwoorden Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** als uw toepassing een HTML-gebruikersnaam en wachtwoord voor het veld wordt weergegeven en u wilt dat deze gebruikersnaam opslaan en wachtwoord voor het veilig naar de toepassing later opnieuw worden afgespeeld

-   **SAML gebaseerde aanmelding** : Kies de [SAML gebaseerde aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single sign-modus als uw toepassing de SAML- of OpenID Connect-protocollen ondersteunt, of u wilt kunnen gebruikers toewijzen aan de rollen van de specifieke toepassing is op basis van regels u definieert in de claims van SAML *

   >[!NOTE]
   >Deze optie is niet beschikbaar wanneer de application proxy is geconfigureerd voor een toepassing.
   >
   >

-   **Headers gebaseerde aanmelding** : Kies deze optie [headers gebaseerde aanmelding](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) aanmeldings-modus voor één hebt u een toepassing met PingAccess die ondersteuning biedt voor verificatie op basis van HTTP-header die u wilt uitvoeren van eenmalige aanmelding bij 

   >[!NOTE]
   >Deze optie is alleen beschikbaar wanneer de toepassingsproxy en PingAccess is geconfigureerd voor een toepassing.
   >
   >

-   **Geïntegreerde Windows-authenticatie** : Kies de [geïntegreerde Windows-verificatie](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) eenmalige aanmelding bij het blootstellen van een on-premises WIA-toepassing die u wilt uitvoeren van eenmalige aanmelding bij 

   >[!NOTE]
   >Deze optie is alleen beschikbaar wanneer de application proxy is geconfigureerd voor een toepassing.
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Modi voor eenmalige aanmelding voor aangepaste toepassingen

Toepassingen hebt aangepast ontwikkeld op basis van de [aangepaste toepassing](#_Custom-Developed_Applications) ervaring bieden ook ondersteuning voor extra eenmalige aanmelding niet eerder weergegeven modi, waaronder:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) op basis van aanmelding

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) op basis van aanmelding

-   [WS-Federation 1.2](https://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) op basis van aanmelding

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) op basis van aanmelding

Lees de [ontwikkelaarsgids van Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) voor meer informatie over het maken van een aangepaste toepassing die ondersteuning biedt voor deze modi voor eenmalige aanmelding.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Over het instellen van een toepassing één modus aanmelding

Om in te stellen van een toepassing **eenmalige aanmelding** modus, volgt u deze instructies:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing waarvoor u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

## <a name="how-to-choose-a-provisioning-mode"></a>Een inrichtingsmethode kiezen

-   **Handmatige inrichting** : Kies de [handmatig](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) inrichtingsmodus als u bestaande accounts hebt of wilt voor deze toepassing buiten Azure AD-accounts beheren.

-   **Automatische inrichting** : Kies de [automatische](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **inrichtingsmodus** als u wilt inschakelen van automatische inrichting op basis van API en/of ongedaan maken inrichting van gebruikersaccounts aan deze toepassing 

   >[!NOTE]
   >Deze optie is alleen beschikbaar voor toepassingen binnen de **aanbevolen** categorie van de [Azure AD-Toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).
   >
   >

-   **Automatische inrichting op basis van SCIM** – gebruiken [automatische inrichting op basis van SCIM](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) als uw toepassing het SCIM-protocol ondersteunt voor het detecteren van wijzigingen in gebruikers en groepen die automatisch worden uitgezonden wijzigingen elke toepassing die is geïntegreerd met Azure AD 

   >[!NOTE]
   >Deze optie wordt niet vermeld als een bepaalde Inrichtingsmethode, maar is standaard ingeschakeld voor alle toepassingen die kunnen worden geïntegreerd met Azure AD.
   >
   >

## <a name="how-to-set-an-applications-provisioning-mode"></a>Over het instellen van een toepassing de inrichtingsmodus

Om in te stellen van een toepassing **inrichting** modus, volgt u deze instructies:

Om in te stellen van een toepassing **eenmalige aanmelding** modus, volgt u deze instructies:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing waarvoor u wilt inrichten configureren.

7.  Nadat de toepassing wordt geladen, klikt u op **Provisioning** in het navigatiemenu aan de van de toepassing.

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](what-is-application-management.md)
