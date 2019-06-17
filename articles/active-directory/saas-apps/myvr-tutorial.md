---
title: 'Zelfstudie: Azure Active Directory-integratie met MyVR | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en MyVR.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b1b4bbc7-cdac-447c-83a3-bcb869159255
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10473c820cd0dbb7c7db3f33de203ca4d636414b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67096492"
---
# <a name="tutorial-integrate-myvr-with-azure-active-directory"></a>Zelfstudie: MyVR integreren met Azure Active Directory

In deze zelfstudie leert u hoe u MyVR integreert met Azure Active Directory (Azure AD). Wanneer u MyVR met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot MyVR heeft.
* Kunnen uw gebruikers worden automatisch aangemeld MyVR met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement MyVR eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. 

* Biedt ondersteuning voor MyVR **SP en IDP** gestart door SSO.
* Biedt ondersteuning voor MyVR **Just In Time** inrichten van gebruikers.

## <a name="adding-myvr-from-the-gallery"></a>MyVR uit de galerie toe te voegen

Voor het configureren van de integratie van MyVR in Azure AD, moet u MyVR uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **MyVR** in het zoekvak in.
1. Selecteer **MyVR** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met MyVR met behulp van een testgebruiker met de naam **B. Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in MyVR vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met MyVR, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Configureren van SSO MyVR](#configure-myvr-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met B. Simon.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - B. Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Maken van de testgebruiker MyVR](#create-myvr-test-user)**  : als u wilt een equivalent van B. Simon in MyVR die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **MyVR** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** als gebruiker wilt configureren van de toepassing in sectie **IDP** gestart modus wordt de gebruiker heeft geen een stap uitvoeren omdat de app al vooraf geïntegreerd in Azure is.

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

   In het tekstvak **Aanmeldings-URL** typt u de URL: `https://ess.virtualroster.net/ess/login.aspx`

1. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:

   | Name | Bronkenmerk|
   | ---------------| --------------- |
   | givenName | user.givenname |
   | surname | user.surname |
   | emailaddress | user.mail |
   | werknemer-id | user.employeeid |

   1. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.
   1. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.
   1. Laat **Naamruimte** leeg.
   1. Selecteer Bron bij **Kenmerk**.
   1. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.
   1. Klik op **OK**.
   1. Klik op **Opslaan**.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **MyVR instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-myvr-sso"></a>Configure MyVR SSO

Het configureren van eenmalige aanmelding op **MyVR** zijde, moet u voor het verzenden van de gedownloade **certificaat (Base64)** en toepassing van de gekopieerde URL's van Azure portal om te [MyVR ondersteuningsteam](mailto:arno.vandenberg@Kronos.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B. Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B. Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon Azure eenmalige aanmelding door toegang te verlenen aan MyVR gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **MyVR**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

   ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B. Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-myvr-test-user"></a>MyVR testgebruiker maken

In deze sectie wordt een gebruiker met de naam van B. Simon gemaakt in MyVR. MyVR biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in MyVR bestaat, wordt een nieuw gemaakt nadat verificatie.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel MyVR in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de MyVR waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
