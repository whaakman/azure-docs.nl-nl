---
title: 'Zelfstudie: Azure Active Directory-integratie met grijs Pe | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en grijs Pe.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 073f8641-b64d-4754-b1a6-2b91c865b13d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02df0a5d13aeb90049383f61d743e8a11e93fc79
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188525"
---
# <a name="tutorial-azure-active-directory-integration-with-gra-pe"></a>Zelfstudie: Azure Active Directory-integratie met grijs Pe

In deze zelfstudie leert u hoe u grijs Pe integreren met Azure Active Directory (Azure AD).

Grijs Pe integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot grijs Pe heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij grijs-Pe (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met grijs Pe, moet u de volgende items:

- Een Azure AD-abonnement
- Een grijs Pe eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Grijs Pe vanuit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-gra-pe-from-the-gallery"></a>Grijs Pe vanuit de galerie toevoegen
Voor het configureren van de integratie van grijs Pe in Azure AD, moet u grijs Pe vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt grijs Pe vanuit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/grape-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/grape-tutorial/a_select_app.png)
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![image](./media/grape-tutorial/a_new_app.png)

4. Typ in het zoekvak **grijs Pe**, selecteer **grijs Pe** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/grape-tutorial/tutorial_grape_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met grijs-Pe gebaseerd op een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in grijs Pe is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in grijs Pe tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met grijs Pe, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker grijs Pe](#create-a-gra-pe-test-user)**  : als u wilt een equivalent van Britta Simon in grijs-Pe die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing grijs Pe.

**Voor het configureren van Azure AD eenmalige aanmelding met grijs Pe, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **grijs Pe** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/grape-tutorial/b1_b2_select_sso.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/grape-tutorial/b1_b2_saml_sso.png)

3. Klik op de pagina **Eenmalige aanmelding met SAML instellen** u de knop **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![image](./media/grape-tutorial/b1-domains_and_urlsedit.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    In de **aanmeldings-URL** in het tekstvak, een URL als:  `https://btm.tts.co.jp/portal/apl/SSOLogin.aspx`

    ![image](./media/grape-tutorial/tutorial_grape_url.png)

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **certificaat (Base64)** te downloaden, en slaat u dit op uw computer op.

    ![image](./media/grape-tutorial/tutorial_grape_certficate.png)

6. Op de **grijs Pe instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![image](./media/grape-tutorial/d1_samlsonfigure.png) 

7. Het configureren van eenmalige aanmelding op **grijs Pe** zijde, moet u voor het verzenden van de gedownloade **certificaat (Base64)** en gekopieerde **afmeldings-URL van aanmeldings-URL, Azure AD-id,** naar [ Het ondersteuningsteam grijs Pe](https://www.toppantravel.com/inquiry/). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![image](./media/grape-tutorial/d_users_and_groups.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![image](./media/grape-tutorial/d_adduser.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![image](./media/grape-tutorial/d_userproperties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-gra-pe-test-user"></a>Maak een testgebruiker grijs Pe

In deze sectie maakt u een gebruiker met de naam van Britta Simon in grijs Pe. Werken met [grijs Pe ondersteuningsteam](https://www.toppantravel.com/inquiry/) om toe te voegen de gebruikers in het grijs Pe-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan grijs Pe.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/grape-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **grijs Pe**.

    ![image](./media/grape-tutorial/tutorial_grape_app.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![image](./media/grape-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/grape-tutorial/d_assign_user.png)

4. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

5. Selecteer in het dialoogvenster **Toewijzing toevoegen** de knop **Toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel grijs Pe in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing grijs Pe.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


