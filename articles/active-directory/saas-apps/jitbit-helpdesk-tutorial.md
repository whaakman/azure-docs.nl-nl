---
title: 'Zelfstudie: Azure Active Directory-integratie met de Jitbit Helpdesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jitbit Helpdesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 71b7cfb92b4caded75e0723564a09fae9f10858c
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58359770"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Zelfstudie: Azure Active Directory-integratie met Jitbit Helpdesk

In deze zelfstudie leert u hoe u Jitbit Helpdesk integreren met Azure Active Directory (Azure AD).
Jitbit Helpdesk integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Jitbit Helpdesk heeft.
* U kunt uw gebruikers worden automatisch aangemeld Jitbit Helpdesk (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de Jitbit Helpdesk, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Eenmalige aanmelding Jitbit Helpdesk ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor de Jitbit Helpdesk **SP** gestart door SSO

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Jitbit Helpdesk uit de galerie toe te voegen

Voor het configureren van de integratie van Jitbit Helpdesk in Azure AD, moet u Jitbit Helpdesk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Jitbit Helpdesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Jitbit Helpdesk**, selecteer **Jitbit Helpdesk** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Jitbit Helpdesk in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met de Jitbit Helpdesk op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Jitbit Helpdesk tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met de Jitbit Helpdesk, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Jitbit Helpdesk Single Sign-On](#configure-jitbit-helpdesk-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak Jitbit Helpdesk testgebruiker](#create-jitbit-helpdesk-test-user)**  : als u wilt een equivalent van Britta Simon in Jitbit Helpdesk die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met de Jitbit Helpdesk, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Jitbit Helpdesk** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Jitbit Helpdesk domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    | |
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Deze waarde is niet echt. Werk deze waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Jitbit Helpdesk Client ondersteuningsteam](https://www.jitbit.com/support/) deze waarde op te halen.

    b. In de **id (entiteits-ID)** tekstvak typt u een URL als volgt: `https://www.jitbit.com/web-helpdesk/`

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. In de **Jitbit Helpdesk instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-jitbit-helpdesk-single-sign-on"></a>Configureer Jitbit Helpdesk Single Sign-On

1. In een ander browservenster aanmelden bij uw bedrijf Jitbit Helpdesk site als beheerder.

1. Klik in de werkbalk bovenaan op **beheer**.

    ![Beheer](./media/jitbit-helpdesk-tutorial/ic777681.png "Beheer")

1. Klik op **algemene instellingen**.

    ![Gebruikers, bedrijven en machtigingen](./media/jitbit-helpdesk-tutorial/ic777680.png "gebruikers, bedrijven en machtigingen")

1. In de **verificatie-instellingen** configuratie sectie, voert u de volgende stappen uit:

    ![Verificatie-instellingen](./media/jitbit-helpdesk-tutorial/ic777683.png "verificatie-instellingen")

    a. Selecteer **inschakelen-SAML één 2.0 Meld u aan bij**, aan te melden bij het gebruik van eenmalige aanmelding (SSO), met **OneLogin**.

    b. In de **eindpunt-URL** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. Open uw **base-64** gecodeerd certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** tekstvak

    d. Klik op **Save changes** (Wijzigingen opslaan).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot de Jitbit Helpdesk.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Jitbit Helpdesk**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Jitbit Helpdesk**.

    ![De Jitbit Helpdesk-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-jitbit-helpdesk-test-user"></a>Jitbit Helpdesk testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij de Jitbit Helpdesk, moeten ze worden ingericht voor Jitbit Helpdesk. In het geval van Jitbit Helpdesk is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij uw **Jitbit Helpdesk** tenant.

1. Klik in het menu aan de bovenkant op **beheer**.

    ![Beheer](./media/jitbit-helpdesk-tutorial/ic777681.png "Beheer")

1. Klik op **gebruikers, bedrijven en machtigingen**.

    ![Gebruikers, bedrijven en machtigingen](./media/jitbit-helpdesk-tutorial/ic777682.png "gebruikers, bedrijven en machtigingen")

1. Klik op **Gebruiker toevoegen**.

    ![Add user](./media/jitbit-helpdesk-tutorial/ic777685.png "Add user")

1. In de sectie maken, typt u de gegevens van de Azure AD-account dat u wilt inrichten gaat als volgt:

    ![Maak](./media/jitbit-helpdesk-tutorial/ic777686.png "maken")

   a. In de **gebruikersnaam** tekstvak, typ de gebruikersnaam van de gebruiker, zoals **BrittaSimon**.

   b. In de **e** tekstvak type e-mailadres van de gebruiker, zoals **BrittaSimon@contoso.com**.

   c. Typ in het tekstvak **First Name** de voornaam van de gebruiker, bijvoorbeeld **Britta**.

   d. Typ in het tekstvak **Last Name** de achternaam van de gebruiker, bijvoorbeeld **Simon**.

   e. Klik op **Create**.

> [!NOTE]
> U kunt een andere Jitbit Helpdesk gebruiker-account maken-hulpprogramma's of API's die worden geleverd door de Jitbit Helpdesk gebruiken voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Jitbit Helpdesk in het toegangsvenster, moet u worden automatisch aangemeld bij de Jitbit Helpdesk waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
