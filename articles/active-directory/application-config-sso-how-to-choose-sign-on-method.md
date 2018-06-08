---
title: Bepalen welke eenmalige aanmelding te gebruiken methode | Microsoft Docs
description: Inzicht in de modus voor één aanmelding ondersteund door Azure AD en hoe u kunt kiezen welke optie te kiezen voor de toepassing dat u geïnteresseerd bent in.
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 52f4045ebb30bf32989e9f7b946fd03d468a9de2
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "34070665"
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a>Bepalen welke eenmalige aanmelding te gebruiken methode

In dit artikel helpt u bij het begrijpen van de modi voor eenmalige aanmelding ondersteund door Azure AD en hoe u kunt kiezen welke optie te kiezen voor de toepassing dat u geïnteresseerd bent in.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Eenmalige aanmelding en modi die worden ondersteund door specifieke toepassingstypen inrichten

De volgende tabel beschrijft de verschillende eenmalige aanmelding en inrichting modi ondersteund door elk van de voorgaande toepassingstypen. U kunt deze tabel gebruiken om u te helpen te begrijpen welke toepassing die u toevoegen wilt ter ondersteuning van een specifiek doel.

  ![Azië typen tabel](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Het kiezen van een modus voor eenmalige aanmelding

Hieronder vindt u de ondersteunde **eenmalige aanmelding** modi voor Azure AD-toepassingen.

-   **Azure AD eenmalige aanmelding uitgeschakeld** – Kies Azure AD eenmalige aanmelding uitgeschakeld **modus voor één aanmelding** als u nog niet klaar voor de integratie van deze toepassing met eenmalige aanmelding met Azure AD of gewoon test het uit

-   **Eenmalige aanmelding gekoppeld** – Kies de [gekoppelde aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** als u een toepassing die al is gekoppeld aan een bestaande eenmalige aanmelding oplossing hebt of als u wilt publiceren van een eenvoudige koppeling voor uw gebruikers in hun [toepassing Toegangspaneel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) of [startprogramma voor toepassingen van Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Op basis van wachtwoorden aanmelding** – Kies de [op basis van wachtwoorden aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** als een HTML-gebruikersnaam en wachtwoord veld Hiermee maakt u uw toepassing en u wilt dat gebruikersnaam opslaan en wachtwoord voor het veilig naar de toepassing later worden cookies

-   **Op basis van SAML aanmelding** – Kies de [op basis van SAML aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) eenmalige aanmelding modus als uw toepassing de SAML- of OpenID Connect-protocollen ondersteunt of u wilt kunnen gebruikers toewijzen aan de rollen van de specifieke toepassing is op basis van regels u definieert in het SAML-claims *(** Opmerking:** deze optie is niet beschikbaar wanneer de toepassingsproxy is geconfigureerd voor een toepassing) *

-   **Op basis van een koptekst aanmelding** : Kies deze optie [headers gebaseerde aanmelding](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) aanmelding modus voor één hebt u een toepassing met PingAccess die ondersteuning biedt voor verificatie op basis van HTTP-header die u wilt uitvoeren van eenmalige aanmelding bij *(** Opmerking:** deze optie is alleen beschikbaar wanneer de toepassingsproxy en PingAccess zijn geconfigureerd voor een toepassing) *

-   **Geïntegreerde Windows-verificatie** – Kies de [geïntegreerde Windows-verificatie](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) eenmalige aanmelding bij het blootstellen van een on-premises WIA-toepassing die u wilt uitvoeren van eenmalige aanmelding bij *(** Opmerking:** deze optie is alleen beschikbaar wanneer de toepassingsproxy is geconfigureerd voor een toepassing) *

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

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op **eenmalige aanmelding** in de toepassing linkermenubalk menu.

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)

