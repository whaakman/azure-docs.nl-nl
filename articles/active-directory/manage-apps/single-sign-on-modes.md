---
title: Bepalen welke eenmalige aanmelding te gebruiken methode | Microsoft Docs
description: Informatie over de modi voor eenmalige aanmelding ondersteund door Azure AD en hoe u kunt kiezen welke computer te kiezen voor de toepassing waarin u geïnteresseerd bent.
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
ms.topic: conceptual
ms.date: 09/09/2018
ms.author: barbkess
ms.openlocfilehash: a9afb3f611a5762d329788b0aa7e1bc52e41fab3
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356493"
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a>Bepalen welke eenmalige aanmelding voor de methode te gebruiken

In dit artikel kunt u inzicht krijgt in de modus voor eenmalige aanmelding ondersteund door Azure AD en hoe u kunt kiezen welke computer te kiezen voor de toepassing waarin u geïnteresseerd bent.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Eenmalige aanmelding en modi die worden ondersteund door specifieke toepassingstypen inrichten

De volgende tabel beschrijft de verschillende single sign-on en inrichting modi ondersteund door elk van de voorgaande toepassingstypen. U kunt deze tabel om u te begrijpen welke toepassing die u toevoegen wilt ter ondersteuning van een bepaald doel te gebruiken.

  ![Tabel met typen App](./media/single-sign-on-modes/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Het kiezen van een modus voor eenmalige aanmelding

Hieronder vindt u de ondersteunde **eenmalige aanmelding** modi voor Azure AD-toepassingen.

-   **Azure AD eenmalige aanmelding uitgeschakeld** – Kies Azure AD eenmalige aanmelding uitgeschakeld **modus voor één aanmelding** als u nog niet gereed voor het integreren van deze toepassing met eenmalige aanmelding met Azure AD, of gewoon test het uit

-   **Gekoppelde aanmelding** : Kies de [gekoppelde Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** hebt u een toepassing die al is verbonden met een bestaande eenmalige aanmelding, of als u alleen wilt publiceren van een eenvoudige koppeling voor uw gebruikers in hun [Toegangsvenster](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) of [startprogramma voor Office 365-toepassingen](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Op basis van wachtwoorden Sign-on** : Kies de [op basis van wachtwoorden Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modus voor één aanmelding** als uw toepassing een HTML-gebruikersnaam en wachtwoord voor het veld wordt weergegeven en u wilt dat deze gebruikersnaam opslaan en wachtwoord voor het veilig naar de toepassing later opnieuw worden afgespeeld

-   **SAML gebaseerde aanmelding** : Kies de [SAML gebaseerde aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single sign-modus als uw toepassing de SAML- of OpenID Connect-protocollen ondersteunt, of u wilt kunnen gebruikers toewijzen aan de rollen van de specifieke toepassing is op basis van regels u definieert in de claims van SAML *(** Opmerking:** deze optie is niet beschikbaar wanneer de application proxy is geconfigureerd voor een toepassing) *

-   **Headers gebaseerde aanmelding** : Kies deze optie [headers gebaseerde aanmelding](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) aanmeldings-modus voor één hebt u een toepassing met PingAccess die ondersteuning biedt voor verificatie op basis van HTTP-header die u wilt uitvoeren van eenmalige aanmelding bij *(** Opmerking:** deze optie is alleen beschikbaar wanneer de toepassingsproxy en PingAccess zijn geconfigureerd voor een toepassing) *

-   **Geïntegreerde Windows-authenticatie** : Kies de [geïntegreerde Windows-verificatie](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) eenmalige aanmelding bij het blootstellen van een on-premises WIA-toepassing die u wilt uitvoeren van eenmalige aanmelding bij *(** Opmerking:** deze optie is alleen beschikbaar wanneer de application proxy is geconfigureerd voor een toepassing) *

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Modi voor eenmalige aanmelding voor aangepaste toepassingen

Toepassingen hebt aangepast ontwikkeld op basis van de [aangepaste toepassing](#_Custom-Developed_Applications) ervaring bieden ook ondersteuning voor extra eenmalige aanmelding niet eerder weergegeven modi, waaronder:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) op basis van aanmelding

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) op basis van aanmelding

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) op basis van aanmelding

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

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)

