---
title: 'Zelfstudie: Azure Active Directory-integratie met Workspot Control | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Workspot besturingselement.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3ea8e4e9-f61f-4f45-b635-b0e306eda3d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97f375c6f48d3dc497eb59e76f19fc64cf906b56
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57886498"
---
# <a name="tutorial-azure-active-directory-integration-with-workspot-control"></a>Zelfstudie: Azure Active Directory-integratie met Workspot besturingselement

In deze zelfstudie leert u hoe u Workspot besturingselement integreren met Azure Active Directory (Azure AD).

Workspot besturingselement te integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Workspot besturingselement heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Workspot besturingselement (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Workspot besturingselement, moet u de volgende items:

- Een Azure AD-abonnement
- Een besturingselement Workspot eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Workspot-besturingselement toe te voegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-workspot-control-from-the-gallery"></a>Workspot-besturingselement toe te voegen uit de galerie
Voor het configureren van de integratie van Workspot besturingselement in Azure AD, moet u Workspot besturingselement uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Workspot besturingselement uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/workspotcontrol-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/workspotcontrol-tutorial/a_select_app.png)
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![image](./media/workspotcontrol-tutorial/a_new_app.png)

4. Typ in het zoekvak **Workspot besturingselement**, selecteer **Workspot besturingselement** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Workspot beheer op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent Workspot controle is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker Workspot controle tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Workspot besturingselement, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Workspot besturingselement](#create-a-workspot-control-test-user)**  : als u wilt een equivalent van Britta Simon in Workspot-besturingselement dat is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Workspot besturingselement.

**Voor het configureren van Azure AD eenmalige aanmelding met Workspot besturingselement, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **Workspot besturingselement** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/workspotcontrol-tutorial/B1_B2_Select_SSO.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/workspotcontrol-tutorial/b1_b2_saml_sso.png)

3. Klik op de pagina **Eenmalige aanmelding met SAML instellen** u de knop **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![image](./media/workspotcontrol-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** intiated modus:

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_url.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<INSTANCENAME>-saml.workspot.com/saml/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<INSTANCENAME>-saml.workspot.com/saml/assertion`

    c. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

     ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_url1.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<INSTANCENAME>-saml.workspot.com/`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [Workspot Beheerclient ondersteuningsteam](mailto:support@workspot.com) om deze waarden te verkrijgen. 

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **certificaat (Base64)** te downloaden, en slaat u dit op uw computer op.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_certficate.png) 

6. Op de **Workspot besturingselement instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    Houd er rekening mee dat de URL kan bijvoorbeeld het volgende:

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![image](./media/workspotcontrol-tutorial/d1_samlsonfigure.png) 

7. In een ander browservenster, meld u aan bij Workspot besturingselement als een beveiligingsbeheerder.

8. Klik in de werkbalk boven aan de pagina op **Setup**, navigeert u vervolgens naar **SAML**.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_setup.png)

9. Op de **Security Assertion Markup Language configuratie** pagina, voert u de volgende stappen uit:
 
    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_saml.png)

    a. In de **entiteit-ID** tekstvak, plak de waarde van **Azure Ad-id** die u hebt gekopieerd vanuit Azure portal.   

    b.In de **URL van de Service aanmelden** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. In de **afmeldings-URL van Service** tekstvak, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal. 

    d. Klik op **updatebestand** knop voor het uploaden van de base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal, in het X.509-certificaat.

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![image](./media/workspotcontrol-tutorial/d_users_and_groups.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![image](./media/workspotcontrol-tutorial/d_adduser.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![image](./media/workspotcontrol-tutorial/d_userproperties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-workspot-control-test-user"></a>Maak een testgebruiker Workspot besturingselement

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij Workspot besturingselement, moeten ze worden ingericht in Workspot besturingselement. In Workspot besturingselement is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij Workspot besturingselement als een beveiligingsbeheerder.

2. Klik in de werkbalk boven aan de pagina op **gebruikers**, navigeert u vervolgens naar **gebruiker toevoegen**.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_adduser.png)

3. Op de **een nieuwe gebruiker toevoegen** pagina, voert u de volgende stappen uit:

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addnewuser.png)

    a. Voer in het tekstvak **Voornaam** de voornaam van de gebruiker in, zoals **Britta**.

    b. Voer in het tekstvak **Achternaam** de achternaam van de gebruiker in, zoals **Simon**.

    c. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals **Brittasimon\@contoso.com**.

    d. Selecteer de juiste gebruikersrol van de **rol** vervolgkeuzelijst.

    e. Selecteer de juiste gebruikersgroep uit de **groep** vervolgkeuzelijst.

    f. Klik op **Add User**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Workspot besturingselement.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/workspotcontrol-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **Workspot besturingselement**.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_app.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![image](./media/workspotcontrol-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/workspotcontrol-tutorial/d_assign_user.png)

4. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

5. Selecteer in het dialoogvenster **Toewijzing toevoegen** de knop **Toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Workspot besturingselement in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Workspot besturingselement.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
