---
title: 'Zelfstudie: Azure Active Directory-integratie met InsideView | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en InsideView.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: jeedes
ms.openlocfilehash: 76db7381985853a81843df812dffcd8ab2bd8f9a
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65407468"
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Zelfstudie: Azure Active Directory-integratie met InsideView

In deze zelfstudie leert u hoe u InsideView integreren met Azure Active Directory (Azure AD).
InsideView integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot InsideView heeft.
* U kunt uw gebruikers worden automatisch aangemeld InsideView (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met InsideView, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding InsideView ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor InsideView **IDP** gestart door SSO

## <a name="adding-insideview-from-the-gallery"></a>InsideView uit de galerie toe te voegen

Voor het configureren van de integratie van InsideView in Azure AD, moet u InsideView uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen InsideView uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **InsideView**, selecteer **InsideView** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![InsideView in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met InsideView op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in InsideView tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met InsideView, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van eenmalige aanmelding InsideView](#configure-insideview-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker InsideView](#create-insideview-test-user)**  : als u wilt een equivalent van Britta Simon in InsideView die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met InsideView, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **InsideView** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![InsideView domein en URL's, eenmalige aanmelding informatie](common/idp-reply.png)

    In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://my.insideview.com/iv/<STS Name>/login.iv`

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke antwoord-URL. Neem contact op met [InsideView Client ondersteuningsteam](mailto:support@insideview.com) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Raw)** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De downloadkoppeling certificaat](common/certificateraw.png)

6. Op de **InsideView instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-insideview-single-sign-on"></a>InsideView voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf InsideView site als beheerder.

1. Klik in de werkbalk bovenaan op **Admin**, **SingleSignOn instellingen**, en klik vervolgens op **toevoegen SAML**.
   
   ![Eenmalige aanmelding instellingen SAML](./media/insideview-tutorial/ic794135.png "eenmalige aanmelding van de SAML-instellingen")

1. In de **toevoegen van een nieuwe SAML** sectie, voert u de volgende stappen uit:

    ![Toevoegen van een nieuwe SAML](./media/insideview-tutorial/ic794136.png "toevoegen van een nieuwe SAML")

    a. In de **STS Name** tekstvak, typ een naam voor uw configuratie.

    b. In **SamlP/WS-Federation ongevraagde eindpunt** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    c. Open uw base-64 gecodeerde certificaat, dat u hebt gedownload vanuit Azure portal, de inhoud van het kopiëren naar het Klembord en plakt u deze naar de **STS Certificate** tekstvak.

    d. In de **Crm-Id Gebruikerstoewijzing** tekstvak, type `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. In de **Crm-e-mailbericht toewijzen** tekstvak, type `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. In de **Crm voornaam toewijzing** tekstvak, type `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    g. In de **Crm lastName toewijzing** tekstvak, type `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.  

    h. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld BrittaSimon@contoso.com.

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan InsideView.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **InsideView**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **InsideView**.

    ![De koppeling InsideView in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-insideview-test-user"></a>InsideView testgebruiker maken

Om Azure AD-gebruikers zich aanmelden bij InsideView, moeten ze worden ingericht voor InsideView. In het geval van InsideView is inrichten een handmatige taak.

Als u gebruikers of contactpersonen gemaakt in InsideView, neem contact op met [InsideView ondersteuningsteam](mailto:support@insideview.com).

> [!NOTE]
> U kunt alle andere InsideView gebruiker-account maken van hulpprogramma's of API's geleverd door InsideView voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel InsideView in het toegangsvenster, moet u worden automatisch aangemeld bij de InsideView waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
