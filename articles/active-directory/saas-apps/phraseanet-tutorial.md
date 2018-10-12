---
title: 'Zelfstudie: Azure Active Directory-integratie met Phraseanet | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Phraseanet.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 078d84ce-e054-4ae1-a52e-46a94a6959a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2018
ms.author: jeedes
ms.openlocfilehash: cfeadbf1d46d9e36f8619cafe29d9dd69aad6eec
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49118538"
---
# <a name="tutorial-azure-active-directory-integration-with-phraseanet"></a>Zelfstudie: Azure Active Directory-integratie met Phraseanet

In deze zelfstudie leert u hoe u Phraseanet integreren met Azure Active Directory (Azure AD).

Phraseanet integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Phraseanet heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Phraseanet (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Phraseanet, moet u de volgende items:

- Een Azure AD-abonnement
- Een Phraseanet eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Phraseanet uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-phraseanet-from-the-gallery"></a>Phraseanet uit de galerie toe te voegen
Voor het configureren van de integratie van Phraseanet in Azure AD, moet u Phraseanet uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Phraseanet uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/phraseanet-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/phraseanet-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/phraseanet-tutorial/a_new_app.png)

4. Typ in het zoekvak **Phraseanet**, selecteer **Phraseanet** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/phraseanet-tutorial/tutorial_phraseanet_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Phraseanet op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Phraseanet is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Phraseanet tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Phraseanet, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Phraseanet](#create-a-phraseanet-test-user)**  : als u wilt een equivalent van Britta Simon in Phraseanet die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Phraseanet.

**Voor het configureren van Azure AD eenmalige aanmelding met Phraseanet, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **Phraseanet** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/phraseanet-tutorial/b1_b2_select_sso.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/phraseanet-tutorial/b1_b2_saml_sso.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/phraseanet-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    In de **aanmeldings-URL** in het tekstvak, een URL als:  `https://<SUBDOMAIN>.alchemyasp.com`

    ![image](./media/phraseanet-tutorial/tutorial_phraseanet_url.png)

    > [!NOTE] 
    > De aanmeldings-URL-waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Phraseanet ondersteuningsteam](mailto:support@alchemy.fr) om de waarde.
 
5. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **federatieve metagegevens-XML**  en sla deze op uw computer.

    ![image](./media/phraseanet-tutorial/tutorial_phraseanet_certificate.png) 

6. Het configureren van eenmalige aanmelding op **Phraseanet** zijde, moet u voor het verzenden van de gedownloade **federatieve metagegevens-XML** naar [Phraseanet ondersteuningsteam](mailto:support@alchemy.fr). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/phraseanet-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/phraseanet-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/phraseanet-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-phraseanet-test-user"></a>Maak een testgebruiker Phraseanet

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Phraseanet. Werken met [Phraseanet ondersteuningsteam](mailto:support@alchemy.fr) om toe te voegen de gebruikers in het Phraseanet-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Phraseanet.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/phraseanet-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **Phraseanet**.

    ![image](./media/phraseanet-tutorial/tutorial_phraseanet_app.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/phraseanet-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/phraseanet-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Phraseanet in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Phraseanet.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


