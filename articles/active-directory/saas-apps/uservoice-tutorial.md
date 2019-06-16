---
title: 'Zelfstudie: Azure Active Directory-integratie met UserVoice | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en UserVoice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: c0c259d3d05232aa70016771e2a2bce7622730a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67087625"
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Zelfstudie: Azure Active Directory-integratie met UserVoice

In deze zelfstudie leert u hoe u UserVoice integreren met Azure Active Directory (Azure AD).
UserVoice integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot UserVoice heeft.
* U kunt uw gebruikers worden automatisch aangemeld bij UserVoice (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met UserVoice, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* UserVoice eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor UserVoice **SP** gestart door SSO

## <a name="adding-uservoice-from-the-gallery"></a>UserVoice uit de galerie toe te voegen

Voor het configureren van de integratie van UserVoice in Azure AD, moet u UserVoice uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen UserVoice uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **UserVoice**, selecteer **UserVoice** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![UserVoice in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met UserVoice op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in UserVoice tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met UserVoice, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding UserVoice](#configure-uservoice-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[UserVoice-testgebruiker maken](#create-uservoice-test-user)**  : als u wilt een equivalent van Britta Simon in UserVoice die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met UserVoice, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **UserVoice** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![UserVoice-domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<tenantname>.UserVoice.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<tenantname>.UserVoice.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [UserVoice-Client-ondersteuningsteam](https://www.uservoice.com/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Klik in de sectie **SAML-handtekeningcertificaat** op de knop **Bewerken** om het dialoogvenster **SAML-handtekeningcertificaat** te openen.

    ![SAML-handtekeningcertificaat bewerken](common/edit-certificate.png)

6. Kopieer in de sectie **SAML-handtekeningcertificaat** de waarde voor **VINGERAFDRUK** en sla deze op de computer op.

    ![Waarde van vingerafdruk kopiëren](common/copy-thumbprint.png)

7. Op de **instellen van UserVoice** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-uservoice-single-sign-on"></a>UserVoice voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw UserVoice-site bedrijf als beheerder.

2. Klik in de werkbalk bovenaan op **instellingen**, en selecteer vervolgens **webportal** in het menu.
   
    ![De sectie instellingen-App aan](./media/uservoice-tutorial/ic777519.png "instellingen")

3. Op de **webportal** tabblad, in de **gebruikersverificatie** sectie, klikt u op **bewerken** openen de **gebruikersverificatie bewerken** dialoogvenster pagina.
   
    ![Webportal tabblad](./media/uservoice-tutorial/ic777520.png "webportal")

4. Op de **gebruikersverificatie bewerken** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Verificatie van de gebruiker bewerken](./media/uservoice-tutorial/ic777521.png "verificatie van de gebruiker bewerken")
   
    a. Klik op **Single Sign-On (SSO)** .
 
    b. Plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit de Azure portal in de **extern aanmelden SSO** tekstvak.

    c. Plakken de **afmeldings-URL van** waarde die u hebt gekopieerd vanuit de Azure portal in de **SSO externe afmelding tekstvak**.
 
    d. Plak de **vingerafdruk** waarde die u hebt gekopieerd vanuit Azure portal in de **huidige certificaat SHA1-vingerafdruk** tekstvak.
    
    e. Klik op **verificatie-instellingen opslaan**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan UserVoice.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **UserVoice**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **UserVoice**.

    ![De UserVoice-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-uservoice-test-user"></a>UserVoice-testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden met UserVoice, moeten ze worden ingericht in UserVoice. In het geval van UserVoice is inrichten een handmatige taak.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Voer de volgende stappen uit als u een gebruikersaccount wilt inrichten:

1. Aanmelden bij uw **UserVoice** tenant.

2. Ga naar **instellingen**.
   
    ![Settings](./media/uservoice-tutorial/ic777811.png "Settings")

3. Klik op **algemene**.

4. Klik op **Agents en machtigingen**.
   
    ![Agents en machtigingen](./media/uservoice-tutorial/ic777812.png "Agents en machtigingen")

5. Klik op **beheerders toevoegen**.
   
    ![Beheerders toevoegen](./media/uservoice-tutorial/ic777813.png "beheerders toevoegen")

6. Op de **uitnodigen beheerders** dialoogvenster, voer de volgende stappen uit:
   
    ![Beheerders uitnodigen](./media/uservoice-tutorial/ic777814.png "beheerders uitnodigen")
   
    a. Typ het e-mailadres van het account dat u wilt inrichten, en klik vervolgens op in het tekstvak e-mailberichten **toevoegen**.
   
    b. Klik op **Uitnodigen**.

> [!NOTE]
> U kunt alle andere UserVoice gebruiker-account maken van hulpprogramma's of API's geleverd door UserVoice aan inrichten AAD-gebruikersaccounts.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de UserVoice-tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de UserVoice waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

