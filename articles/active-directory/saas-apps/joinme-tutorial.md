---
title: 'Zelfstudie: Azure Active Directory-integratie met join.me | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en join.me.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cda5ea0d-3270-4ba5-ad81-4df108eaad12
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: dc393b8701bd72ea7f9de543de33116ea3cb5653
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48871526"
---
# <a name="tutorial-azure-active-directory-integration-with-joinme"></a>Zelfstudie: Azure Active Directory-integratie met join.me

In deze zelfstudie leert u hoe u join.me integreren met Azure Active Directory (Azure AD).

Join.me integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot join.me heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij join.me (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met join.me, moet u de volgende items:

- Een Azure AD-abonnement
- Een join.me eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Join.me uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-joinme-from-the-gallery"></a>Join.me uit de galerie toe te voegen
Voor het configureren van de integratie van join.me in Azure AD, moet u join.me uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen join.me uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/joinme-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/joinme-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/joinme-tutorial/a_new_app.png)

4. Typ in het zoekvak **join.me**, selecteer **join.me** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/joinme-tutorial/tutorial_joinme_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met join.me op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in join.me is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in join.me tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met join.me, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker join.me](#create-a-joinme-test-user)**  : als u wilt een equivalent van Britta Simon in join.me die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing join.me.

**Voor het configureren van Azure AD eenmalige aanmelding met join.me, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **join.me** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/joinme-tutorial/b1_b2_select_sso.png)

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

      ![image](./media/joinme-tutorial/b1_b2_saml_ssso.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/joinme-tutorial/b1_b2_saml_sso.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/joinme-tutorial/b1-domains_and_urlsedit.png)

5. Op de **SAML-basisconfiguratie** sectie, de gebruiker beschikt niet over een stap uitvoeren omdat de app is al vooraf geïntegreerd met Azure.

     ![image](./media/joinme-tutorial/tutorial_joinme_url.png)
 
6. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en sla deze op uw computer.

    ![image](./media/joinme-tutorial/certificatebase64.png) 

7. Het configureren van eenmalige aanmelding op **join.me** zijde, moet u voor het verzenden van de **App-Url voor federatieve metagegevens** die u hebt gekopieerd vanuit Azure portal om te [join.me ondersteuningsteam](https://help.join.me/s/?language). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/joinme-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/joinme-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/joinme-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
  
### <a name="create-a-joinme-test-user"></a>Maak een testgebruiker join.me

In deze sectie maakt u een gebruiker met de naam van Britta Simon in join.me. Werken met [join.me ondersteuningsteam](https://help.join.me/s/?language) om toe te voegen de gebruikers in het join.me-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot join.me.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/joinme-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **join.me**.

    ![image](./media/joinme-tutorial/tutorial_joinme_app.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/joinme-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/joinme-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel join.me in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing join.me.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

