---
title: 'Zelfstudie: Azure Active Directory-integratie met Appraisd | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Appraisd.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.openlocfilehash: dae9a59b89f03a50b1adaed55cd8f97c06906526
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49118541"
---
# <a name="tutorial-azure-active-directory-integration-with-appraisd"></a>Zelfstudie: Azure Active Directory-integratie met Appraisd

In deze zelfstudie leert u hoe u Appraisd integreren met Azure Active Directory (Azure AD).

Appraisd integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Appraisd heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Appraisd (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Appraisd, moet u de volgende items:

- Een Azure AD-abonnement
- Een Appraisd eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Appraisd uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-appraisd-from-the-gallery"></a>Appraisd uit de galerie toe te voegen
Voor het configureren van de integratie van Appraisd in Azure AD, moet u Appraisd uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Appraisd uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/appraisd-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/appraisd-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/appraisd-tutorial/a_new_app.png)

4. Typ in het zoekvak **Appraisd**, selecteer **Appraisd** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/appraisd-tutorial/tutorial_appraisd_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Appraisd op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Appraisd is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Appraisd tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Appraisd, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Appraisd](#create-an-appraisd-test-user)**  : als u wilt een equivalent van Britta Simon in Appraisd die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Appraisd.

**Voor het configureren van Azure AD eenmalige aanmelding met Appraisd, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **Appraisd** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/appraisd-tutorial/B1_B2_Select_SSO.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/appraisd-tutorial/b1_b2_saml_sso.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/appraisd-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_url.png)

    a. Klik op **extra URL's instellen**. 

    b. In de **Relaystatus** tekstvak typt u een URL: `<TENANTCODE>`

    c. Als u wilt configureren van de toepassing in **SP** modus gestart de **aanmeldings-URL** tekstvak typt u een URL met behulp van het volgende patroon: `https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > U krijgt de werkelijke waarde van de aanmeldings-URL en de Relay-status op de pagina voor Appraisd SSO-configuratie die later in de zelfstudie wordt uitgelegd.
    
5. Appraisd toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. Klik op **bewerken** te openen **gebruikerskenmerken** dialoogvenster.

    ![image](./media/appraisd-tutorial/i3-attribute.png)

6. In de **gebruikersclaims** sectie op de **gebruikerskenmerken** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    a. Klik op **bewerken** te openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./media/appraisd-tutorial/i2-attribute.png)

    ![image](./media/appraisd-tutorial/i4-attribute.png)

    b. Uit de **bronkenmerk** , selecteert u de waarde van het kenmerk.

    c. Klik op **Opslaan**. 

7. In de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **certificaat (Base64)** en sla deze op uw computer.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_certficate.png) 

8. Op de **Appraisd instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure Ad-id

    c. URL voor afmelden

    ![image](./media/appraisd-tutorial/d1_samlsonfigure.png)

9. In een ander browservenster aanmelden bij Appraisd als een beveiligingsbeheerder.

10. In de rechterbovenhoek van de pagina, klikt u op **instellingen** pictogram en navigeer vervolgens naar **configuratie**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

11. Aan de linkerkant van het menu, klikt u op **eenmalige aanmelding SAML**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_single.png)

12. Op de **SAML 2.0 Single Sign-On configuratie** pagina, voert u de volgende stappen uit:
    
    ![image](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. Kopiëren de **Relay standaardstatus** waarde en plak deze in **Relaystatus** -tekstvak in **SAML-basisconfiguratie** in Azure portal.

    b. Kopiëren de **Service geïnitieerde aanmeldings-URL** waarde en plak deze in **aanmeldings-URL** -tekstvak in **SAML-basisconfiguratie** in Azure portal.

13. Schuif omlaag in de dezelfde pagina onder **gebruikers identificeren**, voer de volgende stappen uit:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. In de **Identity Provider aanmeldings-URL voor eenmalige** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd uit de Azure-portal en op **opslaan**.

    b. In de **Identity Provider Issuer URL** tekstvak, plak de waarde van **Azure Ad-id**, die u hebt gekopieerd uit de Azure-portal en op **opslaan**.

    c. In Kladblok, opent u het base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal, Kopieer de inhoud en plak deze in de **X.509-certificaat** vak en klikt u op **opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/appraisd-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/appraisd-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/appraisd-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-an-appraisd-test-user"></a>Maak een testgebruiker Appraisd

Om in te schakelen van Azure AD gebruikers zich aanmelden bij Appraisd, ze moeten worden ingericht in Appraisd. In Appraisd is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij Appraisd als een beveiligingsbeheerder.

2. In de rechterbovenhoek van de pagina, klikt u op **instellingen** pictogram en navigeer vervolgens naar **beheer centrum**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. Klik in de werkbalk boven aan de pagina op **mensen**, navigeert u vervolgens naar **een nieuwe gebruiker toevoegen**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. Op de **een nieuwe gebruiker toevoegen** pagina, voert u de volgende stappen uit:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)
    
    a. In **voornaam** tekst voert u de voornaam van de gebruiker, zoals **Julia**.

    b. In **achternaam** tekst voert u de achternaam van de gebruiker, zoals **simon**.

    c. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals **Brittasimon@contoso.com**.

    d. Klik op **Gebruiker toevoegen**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Appraisd.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/appraisd-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **Appraisd**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_app.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/appraisd-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/appraisd-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster, selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Appraisd in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Appraisd.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
