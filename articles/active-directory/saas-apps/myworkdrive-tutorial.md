---
title: 'Zelfstudie: Azure Active Directory-integratie met MyWorkDrive | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en MyWorkDrive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeedes
ms.openlocfilehash: 7310d300c68399c31d9580f070602aa3adbc75e3
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094053"
---
# <a name="tutorial-azure-active-directory-integration-with-myworkdrive"></a>Zelfstudie: Azure Active Directory-integratie met MyWorkDrive

In deze zelfstudie leert u over het integreren van MyWorkDrive met Azure Active Directory (Azure AD).

MyWorkDrive integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot MyWorkDrive heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij MyWorkDrive (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met MyWorkDrive, moet u de volgende items:

- Een Azure AD-abonnement
- Een MyWorkDrive eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. MyWorkDrive uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-myworkdrive-from-the-gallery"></a>MyWorkDrive uit de galerie toe te voegen
Voor het configureren van de integratie van MyWorkDrive met Azure AD, moet u MyWorkDrive uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen MyWorkDrive uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/myworkdrive-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/myworkdrive-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/myworkdrive-tutorial/a_new_app.png)

4. Typ in het zoekvak **MyWorkDrive**, selecteer **MyWorkDrive** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met MyWorkDrive op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in MyWorkDrive is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in MyWorkDrive tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met MyWorkDrive, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker MyWorkDrive](#create-a-myworkdrive-test-user)**  : als u wilt een equivalent van Britta Simon in MyWorkDrive die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing MyWorkDrive.

**Voor het configureren van Azure AD eenmalige aanmelding met MyWorkDrive, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **MyWorkDrive** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/myworkdrive-tutorial/B1_B2_Select_SSO.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/myworkdrive-tutorial/b1_b2_saml_sso.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/myworkdrive-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_url.png)

    In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`

5. Klik op **extra URL's instellen** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<SERVER.DOMAIN.COM>/Account/Login-saml` 

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke antwoord-URL en de aanmeldings-URL.  Voer uw eigen bedrijf MyWorkDrive Server host name:e.g.
    > 
    > Antwoord-URL: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`
    > 
    > Aanmeldings-URL:`https://yourserver.yourdomain.com/Account/Login-saml`
    > 
    > Neem contact op met het ondersteuningsteam MyWorkDrive Client als u twijfelt over het instellen van uw eigen hostnaam en SSL-certificaat voor deze waarden.

6. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op kopiëren **pictogram** kopiëren **App federatieve metagegevens Url** en klikt u op **downloaden** voor het downloaden van de **certificaat (Base64)** Sla deze op uw computer.

    ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_certficate.png) 

7. Op de **instellen van MyWorkDrive** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    Houd er rekening mee dat de URL kan bijvoorbeeld het volgende:

    a. Aanmeldings-URL

    b. Azure Ad-id

    c. URL voor afmelden

    ![image](./media/myworkdrive-tutorial/d1_samlsonfigure.png) 

8. Voor het configureren van eenmalige aanmelding aan MyWorkDrive, downloaden de **certificaat (Base64), URL van de afmelding, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** handmatig configureren op MyWorkDrive server of kopiëren en plakken van de Azure  **App-URL voor federatieve metagegevens** in uw MyWorkDrive Server Admin deelvenster SAML Azure AD-Configuratiescherm. Voor meer informatie contact op met [MyWorkDrive ondersteuningsteam](mailto:support@myworkdrive.com).

    
### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/myworkdrive-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/myworkdrive-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/myworkdrive-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-a-myworkdrive-test-user"></a>Maak een testgebruiker MyWorkDrive

In deze sectie maakt u een gebruiker met de naam van Britta Simon in MyWorkDrive. Werken met [MyWorkDrive ondersteuningsteam](mailto:support@myworkdrive.com) om toe te voegen de gebruikers in het MyWorkDrive-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan MyWorkDrive.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/myworkdrive-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **MyWorkDrive**.

    ![image](./media/myworkdrive-tutorial/tutorial_myworkdrive_app.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/myworkdrive-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/myworkdrive-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel MyWorkDrive in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing MyWorkDrive.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
