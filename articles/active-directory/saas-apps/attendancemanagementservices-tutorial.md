---
title: 'Zelfstudie: Azure Active Directory-integratie met aanwezigheid | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en aanwezigheid Management Services.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1f56e612-728b-4203-a545-a81dc5efda00
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7143d0afce7a3644286703a9eba0da1ee45305f2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106544"
---
# <a name="tutorial-azure-active-directory-integration-with-attendance-management-services"></a>Zelfstudie: Azure Active Directory-integratie met aanwezigheid

In deze zelfstudie leert u hoe u aanwezigheid Management Services integreren met Azure Active Directory (Azure AD).
Aanwezigheid Management Services integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot aanwezigheid Management Services heeft.
* U kunt uw gebruikers worden automatisch aangemeld aanwezigheid Management Services (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met aanwezigheid Management Services, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Beheerservices aanwezigheid eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor beheerservices aanwezigheid **SP** gestart door SSO

## <a name="adding-attendance-management-services-from-the-gallery"></a>Aanwezigheid Management-Services uit de galerie toevoegen

Voor het configureren van de integratie van aanwezigheid beheerservices in Azure AD, moet u aanwezigheid Management-Services uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen aanwezigheid Management-Services uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **aanwezigheid beheerservices**, selecteer **aanwezigheid Management Services** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Aanwezigheid Management-Services in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met aanwezigheid Management Services op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in aanwezigheid Management-Services tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met aanwezigheid Management Services, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer aanwezigheid Management Services Single Sign-On](#configure-attendance-management-services-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak aanwezigheid beheerservices testgebruiker](#create-attendance-management-services-test-user)**  : als u wilt een equivalent van Britta Simon in aanwezigheid-beheerservices die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met aanwezigheid Management Services, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **aanwezigheid beheerservices** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Aanwezigheid Management Services-domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://id.obc.jp/<tenant information >/`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://id.obc.jp/<tenant information >/`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [aanwezigheid Management Services Client ondersteuningsteam](https://www.obcnet.jp/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **ingesteld met beheerservices aanwezigheid** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-attendance-management-services-single-sign-on"></a>Aanwezigheid Management Services eenmalige aanmelding configureren

1. In een ander browservenster aanmelden voor uw bedrijf aanwezigheid Management Services site als administrator.

1. Klik op **SAML-verificatie** onder de **Security beheersectie**.

    ![Aanwezigheid Management Services-configuratie](./media/attendancemanagementservices-tutorial/user1.png)

1. Voer de volgende stappen uit:

    ![Aanwezigheid Management Services-configuratie](./media/attendancemanagementservices-tutorial/user2.png)

    a. Selecteer **gebruik SAML-verificatie**.

    b. In de **id** tekstvak, plak de waarde van **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure portal.

    c. In de **eindpunt-URL webverificatie** tekstvak, plak de waarde van **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **selecteert u een bestand** voor het uploaden van het certificaat dat u hebt gedownload van Azure AD.

    e. Selecteer **uitschakelen wachtwoordverificatie**.

    f. Klik op **registratie**

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen voor aanwezigheid Management Services.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **aanwezigheid Management Services**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **aanwezigheid beheerservices**.

    ![De aanwezigheid Management Services-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-attendance-management-services-test-user"></a>Beheerservices aanwezigheid testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij aanwezigheid Management Services, moeten ze worden ingericht in aanwezigheid Management Services. In het geval van aanwezigheid Management Services, met de inrichting is een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan uw bedrijf aanwezigheid Management Services site als een beheerder.

1. Klik op **Gebruikersbeheer** onder de **Security beheersectie**.

    ![Werknemer toevoegen](./media/attendancemanagementservices-tutorial/user5.png)

1. Klik op **nieuwe regels aanmelding**.

    ![Werknemer toevoegen](./media/attendancemanagementservices-tutorial/user3.png)

1. In de **OBCiD informatie** sectie, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/attendancemanagementservices-tutorial/user4.png)

    a. In de **OBCiD** tekstvak, typ het e-mailadres van gebruiker, zoals `BrittaSimon\@contoso.com`.

    b. In het tekstvak **Wachtwoord** typt u het wachtwoord van de gebruiker.

    c. Klik op **registratie**

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de aanwezigheid Management Services-tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de aanwezigheid Management-Services waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)