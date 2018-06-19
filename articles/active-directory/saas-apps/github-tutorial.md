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
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: c799e59dca6d84ce526e29f7b5714a2a0d9da788
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35907331"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Zelfstudie: Azure Active Directory-integratie met GitHub

In deze zelfstudie leert u hoe GitHub integreren met Azure Active Directory (Azure AD).

GitHub integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot GitHub heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij GitHub (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met GitHub, moet u de volgende items:

- Een Azure AD-abonnement
- Een GitHub eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. GitHub uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-github-from-the-gallery"></a>GitHub uit de galerie toevoegen
Voor het configureren van de integratie van GitHub in Azure AD, moet u GitHub uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen GitHub uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **GitHub**, selecteer **GitHub** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![GitHub in de lijst met resultaten](./media/github-tutorial/tutorial_github_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met GitHub op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in GitHub is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in GitHub tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met GitHub, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een GitHub-testgebruiker](#create-a-github-test-user)**  - hebben een equivalent van Britta Simon in GitHub die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw GitHub-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met GitHub, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **GitHub** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/github-tutorial/tutorial_github_samlbase.png)

3. Op de **GitHub-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![GitHub-domein en de URL's van eenmalige aanmelding informatie](./media/github-tutorial/tutorial_github_url.png)

    a. In de **aanmelden URL** textbox, typ een URL met het volgende patroon volgen: `https://github.com/orgs/<entity-id>/sso`

    b. In de **id (entiteit-ID)** textbox, typ een URL met het volgende patroon volgen: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Houd er rekening mee dat deze niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met de werkelijke aanmelding URL en de id. Hier raden we u voor het gebruik van de unieke waarde van een tekenreeks in de id. Ga naar de sectie GitHub Admin naar deze waarden ophaalt.

4. Op de **gebruikerskenmerken** sectie **gebruikers-id** als user.mail.

    ![Eenmalige aanmelding configureren](./media/github-tutorial/tutorial_github_attribute_new01.png)

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/github-tutorial/tutorial_github_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/github-tutorial/tutorial_general_400.png)

7. Op de **GitHub configuratie** sectie, klikt u op **configureren GitHub** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![GitHub-configuratie](./media/github-tutorial/tutorial_github_configure.png) 

8. Meld u in een ander browservenster in uw organisatie GitHub site als beheerder.

9. Navigeer naar **instellingen** en klik op **beveiliging**

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_03.png)

10. Controleer de **inschakelen SAML-verificatie** vak weer te geven de configuratievelden eenmalige aanmelding. Vervolgens gebruikt u de eenmalige aanmelding URL-waarde voor het bijwerken van de URL met eenmalige aanmelding op de configuratie van Azure AD.

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_13.png)

11. Configureer de volgende velden:

    a. In de **aanmelden URL** textbox plakken **SAML aanmelding Service-URL met eenmalige** waarde die u hebt gekopieerd uit de Azure portal.

    b. In de **verlener** textbox plakken **SAML entiteit-ID** waarde die u hebt gekopieerd uit de Azure portal.

    c. Open het gedownloade certificaat vanuit Azure-portal in Kladblok, plak de inhoud in de **openbaar certificaat** textbox.

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_051.png)

12. Klik op **Test SAML-configuratie** om te controleren of er geen validatiefouten of fouten tijdens eenmalige aanmelding.

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_06.png)

13. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/github-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/github-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/github-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/github-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-github-test-user"></a>Maak een GitHub-testgebruiker

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in GitHub. GitHub biedt ondersteuning voor automatisch gebruikers inrichten, dit is standaard ingeschakeld. U vindt meer informatie [hier](github-provisioning-tutorial.md) over het configureren van automatische gebruikers inrichten.

**Als u moet de gebruiker handmatig maken, voert u de volgende stappen uit:**

1. Meld u aan bij uw bedrijf GitHub site als beheerder.

2. Klik op **mensen**.

    ![Mensen](./media/github-tutorial/tutorial_github_config_github_08.png "personen")

3. Klik op **uitnodiging lid**.

    ![Gebruikers uitnodigen](./media/github-tutorial/tutorial_github_config_github_09.png "gebruikers uitnodigen")

4. Op de **uitnodiging lid** dialoogvenster pagina, voert u de volgende stappen uit:

    a. In de **e** textbox, typ de e-mailadres van Britta Simon account.

    ![Personen uitnodigen](./media/github-tutorial/tutorial_github_config_github_10.png "personen uitnodigen")

    b. Klik op **uitnodiging**.

    ![Personen uitnodigen](./media/github-tutorial/tutorial_github_config_github_11.png "personen uitnodigen")

    > [!NOTE]
    > De accounthouder Azure Active Directory wordt een e-mailbericht ontvangen en Ga als volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang met GitHub.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon met GitHub, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **GitHub**.

    ![De GitHub-koppeling in de lijst met toepassingen](./media/github-tutorial/tutorial_github_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de GitHub-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw GitHub-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/github-tutorial/tutorial_general_01.png
[2]: ./media/github-tutorial/tutorial_general_02.png
[3]: ./media/github-tutorial/tutorial_general_03.png
[4]: ./media/github-tutorial/tutorial_general_04.png

[100]: ./media/github-tutorial/tutorial_general_100.png

[200]: ./media/github-tutorial/tutorial_general_200.png
[201]: ./media/github-tutorial/tutorial_general_201.png
[202]: ./media/github-tutorial/tutorial_general_202.png
[203]: ./media/github-tutorial/tutorial_general_203.png

