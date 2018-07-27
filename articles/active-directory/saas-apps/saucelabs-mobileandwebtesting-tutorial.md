---
title: 'Zelfstudie: Azure Active Directory-integratie met paginafilters Labs - mobiele en Web testen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en paginafilters Labs - mobiele en Web-test.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3142d947-70e5-4345-8a30-b92d8715fac9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: jeedes
ms.openlocfilehash: 55d84256f408e80600308ede22dbaa903b070d90
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265694"
---
# <a name="tutorial-azure-active-directory-integration-with-sauce-labs---mobile-and-web-testing"></a>Zelfstudie: Azure Active Directory-integratie met paginafilters Labs - mobiele en Web testen

In deze zelfstudie leert u over het integreren van saus Labs - mobiele en Web testen met Azure Active Directory (Azure AD).

Integratie van saus Labs - mobiele en Web testen met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot saus Labs - mobiele en Web-test heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij saus Labs - mobiele en Web testen (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met paginafilters Labs - mobiele en Web test, moet u de volgende items:

- Een Azure AD-abonnement
- Een saus Labs - mobiele en Web test eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Saus Labs - mobiele en Web testen vanuit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sauce-labs---mobile-and-web-testing-from-the-gallery"></a>Saus Labs - mobiele en Web testen vanuit de galerie toevoegen
Als u wilt configureren van de integratie van saus Labs - mobiele en Web testen in Azure AD, moet u saus Labs - mobiele en Web testen vanuit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen, paginafilters Labs - mobiele en Web testen vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **saus Labs - mobiele en Web testen**, selecteer **saus Labs - mobiele en Web testen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Saus Labs - mobiele en Web tests in de lijst met resultaten](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureren en testen van Azure AD eenmalige aanmelding met paginafilters Labs - mobiele en Web testen op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in saus Labs - mobiele en Web-test is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in saus Labs - mobiele en Web tests tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met paginafilters Labs - mobiele en Web test, moet u om uit te voeren van de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een saus Labs - mobiele en Web testen testgebruiker](#create-a-sauce-labs---mobile-and-web-testing-test-user)**  : als u wilt een equivalent van Britta Simon in saus Labs - mobiele en Web tests die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw saus Labs - mobiele en Web-toepassing te testen.

**Voor het configureren van Azure AD eenmalige aanmelding met paginafilters Labs - mobiele en Web test, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **saus Labs - mobiele en Web testen** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_samlbase.png)

3. Op de **saus Labs - mobiele en Web testen domein en URL's** sectie, de gebruiker beschikt niet over de stappen uitvoeren omdat de app is al vooraf geïntegreerd met Azure.

    ![Saus Labs - mobiele en Web-domein testen en één URL's aanmeldings-informatie](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_url.png)

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_certificate.png)

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_400.png)

6. Meld u aan uw saus Labs - mobiele en Web site bedrijf als beheerder testen in een ander browservenster.

7. Klik op de **Gebruikerspictogram** en selecteer **Management Team** tabblad.

    ![Eenmalige aanmelding configureren](./media/saucelabs-mobileandwebtesting-tutorial/configure1.png)

8. Voer uw **domeinnaam** in het tekstvak.

    ![Eenmalige aanmelding configureren](./media/saucelabs-mobileandwebtesting-tutorial/configure2.png)

9. Klik op **configureren** tabblad.

    ![Eenmalige aanmelding configureren](./media/saucelabs-mobileandwebtesting-tutorial/configure3.png)

10. In de **configureren van eenmalige aanmelding** sectie, voert u de volgende stappen uit.

    ![Eenmalige aanmelding configureren](./media/saucelabs-mobileandwebtesting-tutorial/configure4.png)

    a. Klik op **Bladeren** en upload het metagegevensbestand gedownload van de Azure AD.

    b. Selecteer de **JIT-inrichting toestaan** selectievakje.

    c. Clcik **opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/saucelabs-mobileandwebtesting-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-sauce-labs---mobile-and-web-testing-test-user"></a>Maken van een saus Labs - mobiele en Web testen gebruiker testen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in saus Labs - mobiele en Web-test. Saus Labs - mobiele en Web testen biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot saus Labs - mobiele en Web testen als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [saus Labs - mobiele en Web testen ondersteuningsteam](mailto:support@saucelabs.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot saus Labs - mobiele en Web testen.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon tot saus Labs - mobiele en Web test, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **saus Labs - mobiele en Web testen**.

    ![De saus Labs - mobiele en Web testen in de lijst met toepassingen koppelen](./media/saucelabs-mobileandwebtesting-tutorial/tutorial_saucelabs_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de saus Labs - mobiele en Web-test-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw saus Labs - mobiele en Web-toepassing te testen.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_01.png
[2]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_02.png
[3]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_03.png
[4]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_04.png

[100]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_100.png

[200]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_200.png
[201]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_201.png
[202]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_202.png
[203]: ./media/saucelabs-mobileandwebtesting-tutorial/tutorial_general_203.png