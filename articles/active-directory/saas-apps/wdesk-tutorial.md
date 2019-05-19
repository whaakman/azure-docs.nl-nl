---
title: 'Zelfstudie: Azure Active Directory-integratie met Wdesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Wdesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 71feb455457fdf75fb19121bac1927b42fe38b67
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "65865346"
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a>Zelfstudie: Azure Active Directory-integratie met Wdesk

In deze zelfstudie leert u hoe u Wdesk integreren met Azure Active Directory (Azure AD).
Wdesk integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Wdesk heeft.
* U kunt uw gebruikers worden automatisch aangemeld Wdesk (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Wdesk, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Wdesk ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Wdesk **SP** en **IDP** gestart door SSO

## <a name="adding-wdesk-from-the-gallery"></a>Wdesk uit de galerie toe te voegen

Voor het configureren van de integratie van Wdesk in Azure AD, moet u Wdesk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Wdesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Wdesk**, selecteer **Wdesk** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Wdesk in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Wdesk op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Wdesk tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Wdesk, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Wdesk](#configure-wdesk-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Wdesk](#create-wdesk-test-user)**  : als u wilt een equivalent van Britta Simon in Wdesk die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Wdesk, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Wdesk** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Wdesk domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Wdesk domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. U krijgt deze waarden van WDesk portal bij het configureren van de eenmalige aanmelding.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **Wdesk instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-wdesk-single-sign-on"></a>Wdesk voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij Wdesk als een beveiligingsbeheerder.

2. In de onderaan, links, klikt u op **Admin** en kies **accountbeheerder**:
 
     ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. Navigeer in Wdesk Admin, naar **Security**, klikt u vervolgens **SAML** > **SAML-instellingen**:

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

4. Onder **algemene instellingen**, Controleer de **SAML eenmalige aanmelding inschakelen**:

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

5. Onder **Details van Service Provider**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Kopieer de **aanmeldings-URL** en plak deze in **aanmeldings-Url** tekstvak in Azure portal.
   
      b. Kopieer de **metagegevens-Url** en plak deze in **id** tekstvak in Azure portal.
       
      c. Kopieer de **consument url** en plak deze in **antwoord-Url** tekstvak in Azure portal.
   
      d. Klik op **opslaan** in Azure portal de wijzigingen op te slaan.      

6. Klik op **IdP-instellingen configureren** openen **IdP-instellingen bewerken** dialoogvenster. Klik op **bestand kiezen** vinden de **Metadata.xml** bestand dat u hebt opgeslagen in Azure portal, klikt u vervolgens uploaden.
    
    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
7. Klik op **wijzigingen opslaan**.

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Wdesk.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Wdesk**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Wdesk**.

    ![De koppeling Wdesk in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-wdesk-test-user"></a>Wdesk testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Wdesk, moeten ze worden ingericht voor Wdesk. In Wdesk is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij Wdesk als een beveiligingsbeheerder.

2. Navigeer naar **Admin** > **Admin Account**.

     ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. Klik op **leden** onder **mensen**.

4. Klik nu op **Add Member** openen **Add Member** in het dialoogvenster. 
   
    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/createuser1.png)  

5. In **gebruiker** tekst voert u de gebruikersnaam van gebruiker, zoals brittasimon@contoso.com en klikt u op **doorgaan** knop.

    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/createuser3.png)

6.  Geef de details, zoals hieronder weergegeven:
  
    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/createuser4.png)
 
    a. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals brittasimon@contoso.com.

    b. Voer in het tekstvak **Voornaam** de voornaam van de gebruiker in, zoals **Britta**.

    c. Typ in het tekstvak **Achternaam** de achternaam van de gebruiker, zoals **Simon**.

7. Klik op **lid opslaan** knop.  

    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/createuser5.png)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Wdesk in het toegangsvenster, moet u worden automatisch aangemeld bij de Wdesk waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

