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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 79a2bc9d517e3c292268a4a70f08936cb0325fbd
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053084"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Zelfstudie: Azure Active Directory-integratie met GitHub

In deze zelfstudie leert u hoe u GitHub integreren met Azure Active Directory (Azure AD).

GitHub integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot GitHub heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij GitHub (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

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
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. GitHub uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-github-from-the-gallery"></a>GitHub uit de galerie toe te voegen
Voor het configureren van de integratie van GitHub in Azure AD, moet u GitHub uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen GitHub uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **GitHub**, selecteer **GitHub** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![GitHub in de lijst met resultaten](./media/github-tutorial/tutorial_github_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met GitHub op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in GitHub is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in GitHub tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met GitHub, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker GitHub](#create-a-github-test-user)**  : als u wilt een equivalent van Britta Simon in GitHub die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw GitHub-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met GitHub, de volgende stappen uitvoeren:**

1. In de Azure-portal op de **GitHub** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/github-tutorial/tutorial_github_samlbase.png)

3. Op de **GitHub-domein en URL's** sectie, voert u de volgende stappen uit:

    ![GitHub-domein en URL's, eenmalige aanmelding informatie](./media/github-tutorial/tutorial_github_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://github.com/orgs/<entity-id>/sso`

    b. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Houd er rekening mee dat deze niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met de werkelijke aanmelding URL en de id. Hier stellen we voor u voor het gebruik van de unieke waarde van de tekenreeks in de id. Ga naar GitHub Admin om op te halen van deze waarden.

4. Op de **gebruikerskenmerken** sectie, selecteer **gebruikers-id** als user.mail.

    ![Eenmalige aanmelding configureren](./media/github-tutorial/tutorial_github_attribute_new01.png)

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/github-tutorial/tutorial_github_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/github-tutorial/tutorial_general_400.png)

7. Op de **GitHub configuratie** sectie, klikt u op **Configure GitHub** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![GitHub-configuratie](./media/github-tutorial/tutorial_github_configure.png) 

8. Meld u in een ander browservenster naar de site van uw GitHub-organisatie als beheerder.

9. Navigeer naar **instellingen** en klikt u op **beveiliging**

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_03.png)

10. Controleer de **inschakelen SAML-verificatie** vak de velden van de configuratie van eenmalige aanmelding te onthullen. Vervolgens gebruikt u de enkele waarde aanmeldings-URL de URL voor eenmalige aanmelding van Azure AD-configuratie bijwerken.

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_13.png)

11. Configureer de volgende velden:

    a. In de **aanmeldings-URL** tekstvak plakken **SAML-eenmalige aanmelding Service URL** waarde die u hebt gekopieerd vanuit Azure portal.

    b. In de **verlener** tekstvak plakken **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    c. Open het gedownloade certificaat vanuit Azure portal in Kladblok, plak de inhoud in de **openbaar certificaat** tekstvak.

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_051.png)

12. Klik op **Test de SAML-configuratie** om te bevestigen dat er geen validatiefouten of fouten tijdens eenmalige aanmelding.

    ![Instellingen](./media/github-tutorial/tutorial_github_config_github_06.png)

13. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/github-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/github-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/github-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/github-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
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

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar GitHub, de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **GitHub**.

    ![De GitHub-koppeling in de lijst met toepassingen](./media/github-tutorial/tutorial_github_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de GitHub-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw GitHub-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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

