---
title: Bepalen welke eenmalige aanmelding te gebruiken methode | Microsoft Docs
description: "Inzicht in de modus voor één aanmelding ondersteund door Azure AD en hoe u kunt kiezen welke optie te kiezen voor de toepassing dat u geïnteresseerd bent in."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 80f4a965920fec9cb578c1bee235c7857f37431e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a>Bepalen welke eenmalige aanmelding te gebruiken methode

In dit artikel helpt u bij het begrijpen van de modi voor eenmalige aanmelding ondersteund door Azure AD en hoe u kunt kiezen welke optie te kiezen voor de toepassing dat u geïnteresseerd bent in.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Eenmalige aanmelding en modi die worden ondersteund door specifieke toepassingstypen inrichten

De volgende tabel beschrijft de verschillende eenmalige aanmelding en inrichting modi die worden ondersteund door elk van de bovenstaande toepassingstypen. U kunt deze tabel gebruiken om u te helpen te begrijpen welke toepassing die u toevoegen wilt ter ondersteuning van een specifiek doel.

  ![Azië typen tabel](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Het kiezen van een modus voor eenmalige aanmelding

De ondersteunde **eenmalige aanmelding** modi voor Azure AD-toepassingen worden hieronder weergegeven.

-   **Azure AD eenmalige aanmelding uitgeschakeld** – Kies Azure AD eenmalige aanmelding uitgeschakeld **modus voor één aanmelding** als u nog niet klaar voor de integratie van deze toepassing met eenmalige aanmelding met Azure AD of gewoon test het uit

-   **Gekoppelde aanmelding** – Kies de [gekoppelde aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** als u een toepassing die al is gekoppeld aan een bestaande eenmalige aanmelding oplossing hebt of als u wilt publiceren van een eenvoudige koppeling voor uw gebruikers in hun [toepassing Toegangspaneel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) of [startprogramma voor toepassingen van Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Op basis van wachtwoorden aanmelding** – Kies de [op basis van wachtwoorden aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** als een HTML-gebruikersnaam en wachtwoord veld Hiermee maakt u uw toepassing en u wilt opslaan die gebruikersnaam en wachtwoord veilig naar de toepassing later worden cookies

-   **Op basis van SAML aanmelding** – Kies de [op basis van SAML aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) eenmalige aanmelding modus als uw toepassing de SAML- of OpenID Connect-protocollen ondersteunt of u wilt kunnen gebruikers toewijzen aan de rollen van de specifieke toepassing is op basis van regels die u in het SAML-claims definieert *(**Opmerking:** deze optie is niet beschikbaar wanneer de toepassingsproxy is geconfigureerd voor een toepassing) *

-   **Op basis van een koptekst aanmelding** : Kies deze optie [headers gebaseerde aanmelding](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) aanmelding modus voor één hebt u een toepassing met PingAccess die ondersteuning biedt voor HTTP-header gebaseerde verificatie die u wilt uitvoeren van eenmalige aanmelding bij *(**Opmerking:** deze optie is alleen beschikbaar wanneer de toepassingsproxy en PingAccess geconfigureerd is voor een toepassing) *

-   **Geïntegreerde Windows-verificatie** – Kies de [geïntegreerde Windows-verificatie](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) eenmalige aanmelding bij het blootstellen van een on-premises WIA-toepassing die u wilt uitvoeren van eenmalige aanmelding bij *(**Opmerking:** deze optie is alleen beschikbaar wanneer de toepassingsproxy is geconfigureerd voor een toepassing) *

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Modi voor eenmalige aanmelding voor aangepaste ontwikkelde toepassingen

Toepassingen hebt u aangepaste ontwikkeld via de [ontwikkelde aangepaste toepassing](#_Custom-Developed_Applications) ervaring bieden ook ondersteuning voor extra eenmalige aanmelding modi niet hierboven worden vermeld. Deze omvatten:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) op basis van eenmalige aanmelding

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) op basis van eenmalige aanmelding

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) op basis van eenmalige aanmelding

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) op basis van eenmalige aanmelding

Lees de [ontwikkelaarshandleiding Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) voor meer informatie over het maken van een toepassing met aangepaste ontwikkeld die ondersteuning biedt voor deze modi voor eenmalige aanmelding.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Het instellen van een toepassing aanmelding modus voor één

Instellen van een toepassing **eenmalige aanmelding** modus, volgt u onderstaande instructies:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)

