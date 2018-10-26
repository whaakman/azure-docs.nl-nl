---
title: 'Zelfstudie: Azure Active Directory-integratie met BlueJeans | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: jeedes
ms.openlocfilehash: cff1512c56dba9907adbf1bb4452f11d47d0787d
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095225"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Zelfstudie: Azure Active Directory-integratie met BlueJeans

In deze zelfstudie leert u hoe u BlueJeans integreren met Azure Active Directory (Azure AD).

BlueJeans integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BlueJeans heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij BlueJeans (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BlueJeans, moet u de volgende items:

- Een Azure AD-abonnement
- Een BlueJeans eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BlueJeans uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bluejeans-from-the-gallery"></a>BlueJeans uit de galerie toe te voegen

Voor het configureren van de integratie van BlueJeans in Azure AD, moet u BlueJeans uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen BlueJeans uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **BlueJeans**, selecteer **BlueJeans** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![BlueJeans in de lijst met resultaten](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met BlueJeans op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in BlueJeans is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in BlueJeans tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met BlueJeans, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker BlueJeans](#creating-a-bluejeans-test-user)**  : als u wilt een equivalent van Britta Simon in BlueJeans die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing BlueJeans.

**Voor het configureren van Azure AD eenmalige aanmelding met BlueJeans, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **BlueJeans** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/bluejeans-tutorial/tutorial_general_301.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/bluejeans-tutorial/tutorial_bluejeans_editurl.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![BlueJeans domein en URL's, eenmalige aanmelding informatie](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > De waarde van de aanmelding is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [BlueJeans Client ondersteuningsteam](https://support.bluejeans.com/contact) om de waarde.

6. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/bluejeans-tutorial/tutorial_bluejeans_certficate.png) 

7. Op de **BlueJeans instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![BlueJeans configuratie](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

8. In een ander browservenster aanmelden bij uw **BlueJeans** bedrijf site als beheerder.

9. Ga naar **ADMIN \> GROEPSINSTELLINGEN \> SECURITY**.

    ![Beheerder](./media/bluejeans-tutorial/IC785868.png "Admin")

10. In de **SECURITY** sectie, voert u de volgende stappen uit:

    ![SAML-eenmalige aanmelding](./media/bluejeans-tutorial/IC785869.png "SAML-eenmalige aanmelding")

    a. Selecteer **SAML-eenmalige aanmelding**.

    b. Selecteer **automatische inrichting inschakelen**.

11. Verdergaan met de volgende stappen uit:

    ![Pad van het certificaat](./media/bluejeans-tutorial/IC785870.png "pad van het certificaat")

    a. Klik op **bestand kiezen**, het uploaden van het base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal.

    b. In de **aanmeldings-URL** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. In de **URL voor wachtwoord wijzigen** tekstvak, plak de waarde van **URL van wijzigen wachtwoord** die u hebt gekopieerd vanuit Azure portal.

    d. In de **afmeldings-URL van** tekstvak, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal.

12. Verdergaan met de volgende stappen uit:

    ![Wijzigingen opslaan](./media/bluejeans-tutorial/IC785874.png "wijzigingen opslaan")

    a. In de **gebruikers-id** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. In de **e** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Klik op **wijzigingen opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](./media/bluejeans-tutorial/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](./media/bluejeans-tutorial/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-bluejeans-test-user"></a>Het maken van een testgebruiker BlueJeans

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in BlueJeans. BlueJeans ondersteunt automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](bluejeans-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

**Als u moet de gebruiker handmatig hebt gemaakt, voert u de volgende stappen uit:**

1. Meld u aan bij uw **BlueJeans** bedrijf site als beheerder.

2. Ga naar **ADMIN \> gebruikers beheren \> gebruiker toevoegen**.

    ![Beheerder](./media/bluejeans-tutorial/IC785877.png "Admin")

    >[!IMPORTANT]
    >De **gebruiker toevoegen** tabblad is alleen beschikbaar als in de **SECUTIRY tabblad**, **automatische inrichting inschakelen** is uitgeschakeld. 

3. In de **gebruiker toevoegen** sectie, voert u de volgende stappen uit:

    ![Gebruiker toevoegen](./media/bluejeans-tutorial/IC785886.png "gebruiker toevoegen")

    a. In **voornaam** tekst voert u de voornaam van de gebruiker, zoals **Julia**.

    b. In **achternaam** tekst voert u de achternaam van de gebruiker, zoals **simon**.

    c. In **kiezen van een gebruikersnaam BlueJeans** tekst voert u de gebruikersnaam van gebruiker, zoals **Brittasimon**

    d. In **maken van een wachtwoord** tekst vak, Voer uw wachtwoord.

    e. In **bedrijf** tekst voert u uw bedrijf.

    f. In **e-mailadres** tekst vak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    g. In **maken van een BlueJeans vergadering i. d** tekst voert u uw vergadering-ID.

    h. In **kiezen van een wachtwoordcode Moderator** tekst vak, Voer uw wachtwoordcode.

    i. Klik op **doorgaan**.

    ![Gebruiker Addd](./media/bluejeans-tutorial/IC785887.png "Addd gebruiker")

    "J". Klik op **gebruiker toevoegen**.

>[!NOTE]
>U kunt alle andere BlueJeans gebruiker-account maken van hulpprogramma's of API's geleverd door BlueJeans voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan BlueJeans.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **BlueJeans**.

    ![Eenmalige aanmelding configureren](./media/bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel BlueJeans in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing BlueJeans.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png
