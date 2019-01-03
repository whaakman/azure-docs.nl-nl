---
title: 'Zelfstudie: Azure Active Directory-integratie met GitHub | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en GitHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeedes
ms.openlocfilehash: 41e8fb40f07a88cb6fa2108a38db7f973fccd2f9
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53607795"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Zelfstudie: Azure Active Directory-integratie met GitHub

In deze zelfstudie leert u hoe u GitHub integreren met Azure Active Directory (Azure AD).

GitHub integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot GitHub heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij GitHub (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met GitHub, moet u de volgende items:

- Een Azure AD-abonnement
- Een GitHub eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. GitHub uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-github-from-the-gallery"></a>GitHub uit de galerie toe te voegen
Voor het configureren van de integratie van GitHub in Azure AD, moet u GitHub uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen GitHub uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![image](./media/github-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/github-tutorial/a_select_app.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![image](./media/github-tutorial/a_new_app.png)

4. Typ in het zoekvak **GitHub**, selecteer **GitHub** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![image](./media/github-tutorial/tutorial_github_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met GitHub op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in GitHub is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in GitHub tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met GitHub, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker GitHub](#create-a-github-test-user)**  : als u wilt een equivalent van Britta Simon in GitHub die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw GitHub-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met GitHub, de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **GitHub** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/github-tutorial/b1_b2_select_sso.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/github-tutorial/b1_b2_saml_sso.png)

3. Klik op de pagina **Eenmalige aanmelding met SAML instellen** u de knop **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![image](./media/github-tutorial/b1-domains_and_urlsedit.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![image](./media/github-tutorial/tutorial_github_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://github.com/orgs/<entity-id>/sso`

    b. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Houd er rekening mee dat deze niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met de werkelijke aanmelding URL en de id. Hier stellen we voor u voor het gebruik van de unieke waarde van de tekenreeks in de id. Ga naar GitHub Admin om op te halen van deze waarden.

5. GitHub-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Klik op **bewerken** te openen **gebruikerskenmerken** dialoogvenster.

    ![image](./media/github-tutorial/i3-attribute.png)

6. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    a. Klik op **bewerken** te openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./media/github-tutorial/i2-attribute.png)

    ![image](./media/github-tutorial/i4-attribute.png)

    b. Uit de **bronkenmerk** , selecteert u de waarde van het kenmerk.

    c. Klik op **Opslaan**.

7. In de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **certificaat (Base64)** en sla deze op uw computer.

    ![image](./media/github-tutorial/tutorial_github_certficate.png)

8. Op de **Stel GitHub in** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![image](./media/github-tutorial/d1_samlsonfigure.png)

9. Meld u in een ander browservenster naar de site van uw GitHub-organisatie als beheerder.

10. Navigeer naar **instellingen** en klikt u op **beveiliging**

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_03.png)

11. Controleer de **inschakelen SAML-verificatie** vak de velden van de configuratie van eenmalige aanmelding te onthullen. Vervolgens gebruikt u de enkele waarde aanmeldings-URL de URL voor eenmalige aanmelding van Azure AD-configuratie bijwerken.

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_13.png)

12. Configureer de volgende velden:

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_051.png)

    a. In de **aanmeldings-URL** tekstvak plakken **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    b. In de **verlener** tekstvak plakken **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure portal.

    c. Open het gedownloade certificaat vanuit Azure portal in Kladblok, plak de inhoud in de **openbaar certificaat** tekstvak.

    d. Klik op **bewerken** bewerkingspictogram van de **handtekeningmethode** en **Digest-methode** van **RSA-SHA1** en **SHA1**naar **RSA-SHA256** en **SHA256** zoals hieronder wordt weergegeven.

    ![image](./media/github-tutorial/tutorial_github_sha.png)

13. Klik op **Test de SAML-configuratie** om te bevestigen dat er geen validatiefouten of fouten tijdens eenmalige aanmelding.

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_06.png)

14. Klik op **Opslaan**.

> [!NOTE]
> Eenmalige aanmelding in GitHub wordt geverifieerd op een specifieke organisatie in GitHub en is geen vervanging voor de verificatie van GitHub zelf. Dus als de gebruikerssessie github.com is verlopen, kan u worden gevraagd om te verifiëren met de GitHub-ID en wachtwoord tijdens het proces voor eenmalige aanmelding.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![image](./media/github-tutorial/d_users_and_groups.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![image](./media/github-tutorial/d_adduser.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![image](./media/github-tutorial/d_userproperties.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="create-a-github-test-user"></a>Maak een testgebruiker GitHub

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in GitHub. GitHub ondersteunt automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](github-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

**Als u moet de gebruiker handmatig hebt gemaakt, voert u de volgende stappen uit:**

1. Meld u aan bij uw bedrijf GitHub site aan als beheerder.

2. Klik op **mensen**.

    ![Mensen](./media/github-tutorial/tutorial_github_config_github_08.png "personen")

3. Klik op **uitnodiging lid**.

    ![Gebruikers uitnodigen](./media/github-tutorial/tutorial_github_config_github_09.png "gebruikers uitnodigen")

4. Op de **uitnodiging lid** dialoogvenster pagina, voert u de volgende stappen uit:

    a. In de **e** tekstvak typt u het e-mailadres van Britta Simon-account.

    ![Anderen uitnodigen](./media/github-tutorial/tutorial_github_config_github_10.png "anderen uitnodigen")

    b. Klik op **uitnodiging verzenden**.

    ![Anderen uitnodigen](./media/github-tutorial/tutorial_github_config_github_11.png "anderen uitnodigen")

    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en gaat u als volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan GitHub.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/github-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **GitHub**.

    ![image](./media/github-tutorial/tutorial_github_app.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![image](./media/github-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/github-tutorial/d_assign_user.png)

4. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de GitHub-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw GitHub-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


