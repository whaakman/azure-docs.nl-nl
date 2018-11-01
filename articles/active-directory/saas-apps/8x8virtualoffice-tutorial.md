---
title: 'Zelfstudie: Azure Active Directory-integratie met 8 x 8 virtuele Office | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en 8 x 8 virtuele Office.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.openlocfilehash: 53db637bf7ad47896747b491fcbe31123fdb104e
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741807"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Zelfstudie: Azure Active Directory-integratie met 8 x 8 virtuele Office

In deze zelfstudie leert u hoe u 8 x 8 virtuele Office integreren met Azure Active Directory (Azure AD).

Integratie van 8 x 8 biedt virtuele Office met Azure AD u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot 8 x 8 virtuele Office heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij 8 x 8 virtuele Office (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met 8 x 8 virtuele Office, moet u de volgende items:

- Een Azure AD-abonnement
- Een 8 x 8 virtuele Office eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. 8 x 8 virtuele Office uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 virtuele Office uit de galerie toe te voegen

Voor het configureren van de integratie van 8 x 8 virtuele Office in Azure AD, moet u 8 x 8 virtuele Office uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen 8 x 8 virtuele Office uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **8 x 8 virtuele Office**, selecteer **8 x 8 virtuele Office** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![8 x 8 virtuele kantoor in de lijst met resultaten](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met 8 x 8 die virtuele Office op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente-gebruiker in 8 x 8 virtuele Office aan een gebruiker in Azure AD is. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in 8 x 8 virtuele Office moet tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met 8 x 8 virtuele Office, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker van 8 x 8 virtuele Office](#creating-a-8x8-virtual-office-test-user)**  : als u wilt een equivalent van Britta Simon in 8 x 8 virtuele Office die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw 8 x 8 virtuele Office-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met 8 x 8 virtuele Office, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **8 x 8 virtuele Office** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![8 x 8 virtuele Office-domein en URL's één aanmeldings-informatie](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. In de **id** tekstvak, een URL typen: `https://sso.8x8.com/saml2`

    b. In de **antwoord-URL** tekstvak, een URL typen: `https://sso.8x8.com/saml2`

5. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Raw)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

6. Op de **instellen van 8 x 8 virtuele Office** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![8 x 8 virtuele Office-configuratie](common/configuresection.png)

7. Aanmelding bij uw tenant 8 x 8 virtuele Office als beheerder.

8. Selecteer **virtuele Office Account Mgr** toepassing in het venster.

    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Selecteer **Business** account die u wilt beheren en op **aanmelden** knop.

    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Klik op **ACCOUNTS** tabblad in de menulijst met.

    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Klik op **Single Sign On** in de lijst van Accounts.
  
    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Selecteer **Single Sign On** onder verificatiemethoden en klik op **SAML**.

    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. In de **SAML-eenmalige aanmelding** sectie, voert u de volgende stappen uit:

    ![Aan de App configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    a. In de **Sign In URL** tekstvak plakken **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    b. In de **afmeldings-URL** tekstvak plakken **afmeldings-URL van** waarde die u hebt gekopieerd vanuit Azure portal.

    c. In de **URL-verlener** tekstvak plakken **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **Bladeren** knop voor het uploaden van het certificaat dat u hebt gedownload van Azure portal.

    e. Klik op de knop **Opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
  
### <a name="creating-a-8x8-virtual-office-test-user"></a>Het maken van een gebruiker 8 x 8 virtuele Office testen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in 8 x 8 virtuele Office. 8 x 8 virtuele Office biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot 8 x 8 virtuele Office als deze nog niet bestaat.

> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [8 x 8 virtuele Office-ondersteuningsteam](https://www.8x8.com/about-us/contact-us).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot 8 x 8 virtuele Office.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **8 x 8 virtuele Office**.

    ![Eenmalige aanmelding configureren](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel 8 x 8 virtuele Office in het toegangsvenster, u moet u automatisch aangemeld bij uw 8 x 8 virtuele Office-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
