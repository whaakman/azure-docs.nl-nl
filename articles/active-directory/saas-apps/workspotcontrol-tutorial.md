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
ms.openlocfilehash: 520e625ba9689ebf35e985fe95609c62102e2493
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49312720"
---
# <a name="tutorial-azure-active-directory-integration-with-workspot-control"></a>Zelfstudie: Azure Active Directory-integratie met Workspot besturingselement

In deze zelfstudie leert u hoe u Workspot besturingselement integreren met Azure Active Directory (Azure AD).

Workspot besturingselement te integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Workspot besturingselement heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Workspot besturingselement (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Workspot besturingselement, moet u de volgende items:

- Een Azure AD-abonnement
- Een besturingselement Workspot eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
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
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/workspotcontrol-tutorial/a_new_app.png)

4. Typ in het zoekvak **Workspot besturingselement**, selecteer **Workspot besturingselement** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Workspot beheer op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent Workspot controle is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker Workspot controle tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Workspot besturingselement, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Workspot besturingselement](#create-a-workspot-control-test-user)**  : als u wilt een equivalent van Britta Simon in Workspot-besturingselement dat is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Workspot besturingselement.

**Voor het configureren van Azure AD eenmalige aanmelding met Workspot besturingselement, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **Workspot besturingselement** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/workspotcontrol-tutorial/B1_B2_Select_SSO.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/workspotcontrol-tutorial/b1_b2_saml_sso.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/workspotcontrol-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** intiated modus:

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_url.png)

    a. In de **id** tekstvak typt u een URL met behulp van het volgende patroon: `https://<INSTANCENAME>-saml.workspot.com/saml/metadata`

    b. In de **antwoord-URL** tekstvak typt u een URL met behulp van het volgende patroon: `https://<INSTANCENAME>-saml.workspot.com/saml/assertion`

    c. Klik op **extra URL's instellen** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

     ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_url1.png)

    In de **aanmeldings-URL** tekstvak typt u een URL met behulp van het volgende patroon:  `https://<INSTANCENAME>-saml.workspot.com/`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Workspot Beheerclient ondersteuningsteam](mailto:support@workspot.com) om deze waarden te verkrijgen. 

5. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **certificaat (Base64)** en sla deze op uw computer.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_certficate.png) 

6. Op de **Workspot besturingselement instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    Houd er rekening mee dat de URL kan bijvoorbeeld het volgende:

    a. Aanmeldings-URL

    b. Azure Ad-id

    c. URL voor afmelden

    ![image](./media/workspotcontrol-tutorial/d1_samlsonfigure.png) 

7. In een ander browservenster, meld u aan bij Workspot besturingselement als een beveiligingsbeheerder.

8. Klik in de werkbalk boven aan de pagina op **Setup**, navigeert u vervolgens naar **SAML**.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_setup.png)

9. Op de **Security Assertion Markup Language configuratie** pagina, voert u de volgende stappen uit:
 
    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_saml.png)

    a. In de **entiteit-ID** tekstvak, plak de waarde van **Azure Ad-id** die u hebt gekopieerd vanuit Azure portal.   

    b.In de **URL van de Service aanmelden** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. In de **afmeldings-URL van Service** tekstvak, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal. 

    d. Klik op **updatebestand** knop voor het uploaden van de base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal, in het X.509-certificaat.

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/workspotcontrol-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/workspotcontrol-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/workspotcontrol-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-workspot-control-test-user"></a>Maak een testgebruiker Workspot besturingselement

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij Workspot besturingselement, moeten ze worden ingericht in Workspot besturingselement. In Workspot besturingselement is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij Workspot besturingselement als een beveiligingsbeheerder.

2. Klik in de werkbalk boven aan de pagina op **gebruikers**, navigeert u vervolgens naar **gebruiker toevoegen**.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_adduser.png)

3. Op de **een nieuwe gebruiker toevoegen** pagina, voert u de volgende stappen uit:

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addnewuser.png)

    a. In **voornaam** tekst voert u de voornaam van de gebruiker, zoals **Julia**.

    b. In **achternaam** tekst voert u de achternaam van de gebruiker, zoals **simon**.

    c. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals **Brittasimon@contoso.com**.

    d. Selecteer de juiste gebruikersrol van de **rol** vervolgkeuzelijst.

    e. Selecteer de juiste gebruikersgroep uit de **groep** vervolgkeuzelijst.

    f. Klik op **gebruiker toevoegen**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Workspot besturingselement.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/workspotcontrol-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **Workspot besturingselement**.

    ![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_app.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/workspotcontrol-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/workspotcontrol-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Workspot besturingselement in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Workspot besturingselement.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
