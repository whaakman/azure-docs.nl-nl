---
title: 'Zelfstudie: Azure Active Directory-integratie met regel werkt | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en regel werkt.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.openlocfilehash: 0437bd46537d8b7024618706aba12660abca5512
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414689"
---
# <a name="tutorial-azure-active-directory-integration-with-line-works"></a>Zelfstudie: Azure Active Directory-integratie met regel werkt

In deze zelfstudie leert u hoe u regel werkt integreren met Azure Active Directory (Azure AD).

REGEL werkt integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de regel werkt heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij regel werkt (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met regel werkt, moet u de volgende items:

- Een Azure AD-abonnement
- Een regel werkt eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. REGEL werkt vanuit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-line-works-from-the-gallery"></a>REGEL werkt vanuit de galerie toevoegen
Voor het configureren van de integratie van regel werkt in Azure AD, moet u de regel werkt vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Om toe te voegen regel werkt vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/worksmobile-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/worksmobile-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/worksmobile-tutorial/a_new_app.png)

4. Typ in het zoekvak **regel werkt**, selecteer **regel werkt** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/worksmobile-tutorial/tutorial_lineworks_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met regel werkt op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in regel werkt in Azure AD aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in regel werkt tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met regel werkt, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker regel werkt](#create-a-line-works-test-user)**  : als u wilt een equivalent van Britta Simon in regel werkt die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing regel werkt.

**Voor het configureren van Azure AD eenmalige aanmelding met regel werkt, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **regel werkt** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/worksmobile-tutorial/b1_b2_select_sso.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/worksmobile-tutorial/b1_b2_saml_sso.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/worksmobile-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    a. In de **aanmeldings-URL** tekstvak typt u een URL met behulp van het volgende patroon: `https://auth.worksmobile.com/d/login/<domain>/?userId=<ID@domain>`

    b. In de **id** tekstvak typt u een URL: `worksmobile.com`

    ![image](./media/worksmobile-tutorial/tutorial_lineworks_url.png)

5. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **certificaat (Raw)** en sla deze op uw computer.

    ![image](./media/worksmobile-tutorial/tutorial_lineworks_certficate.png)

6. Op de **instellen van de regel werkt** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![image](./media/worksmobile-tutorial/d1_samlsonfigure.png) 

7. Het configureren van eenmalige aanmelding op **regel werkt** aan clientzijde, lees de [documenten regel werkt SSO](https://developers.worksmobile.com/jp/document/1001080101) en configureer de instelling van een regel werkt.

> [!NOTE]
> U moet het gedownloade bestand voor certificaat .cert converteren naar PEM


### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/worksmobile-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/worksmobile-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/worksmobile-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-line-works-test-user"></a>Maak een testgebruiker regel werkt

In deze sectie maakt u een gebruiker met de naam van Britta Simon in regel werkt. Toegang tot [beheerpagina van de regel werkt](https://admin.worksmobile.com) en voegt u de gebruikers in de regel werkt-platform.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan regel werkt.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/worksmobile-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **regel werkt**.

    ![image](./media/worksmobile-tutorial/tutorial_lineworks_app.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/worksmobile-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/worksmobile-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel regel werkt in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing regel werkt.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


