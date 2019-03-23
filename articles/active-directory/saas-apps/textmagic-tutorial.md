---
title: 'Zelfstudie: Azure Active Directory-integratie met TextMagic | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TextMagic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3e5b49d2-7096-46bc-a9ce-90e09177ba28
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: f65ad80425f12304624f242871a605d4417ca533
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361826"
---
# <a name="tutorial-azure-active-directory-integration-with-textmagic"></a>Zelfstudie: Azure Active Directory-integratie met TextMagic

In deze zelfstudie leert u hoe u TextMagic integreren met Azure Active Directory (Azure AD).
TextMagic integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot TextMagic heeft.
* U kunt uw gebruikers worden automatisch aangemeld TextMagic (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TextMagic, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Eenmalige aanmelding TextMagic ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor TextMagic **IDP** gestart door SSO
* Biedt ondersteuning voor TextMagic **Just In Time** inrichten van gebruikers

## <a name="adding-textmagic-from-the-gallery"></a>TextMagic uit de galerie toe te voegen

Voor het configureren van de integratie van TextMagic in Azure AD, moet u TextMagic uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TextMagic uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **TextMagic**, selecteer **TextMagic** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![TextMagic in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met TextMagic op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TextMagic tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met TextMagic, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding TextMagic](#configure-textmagic-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker TextMagic](#create-textmagic-test-user)**  : als u wilt een equivalent van Britta Simon in TextMagic die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met TextMagic, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **TextMagic** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![TextMagic domein en URL's, eenmalige aanmelding informatie](common/idp-identifier.png)

    In het tekstvak **Id** typt u een URL: `https://my.textmagic.com/saml/metadata`

5. Uw toepassing TextMagic wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding ziet u de lijst met standaardkenmerken, waarbij **nameidentifier** is toegewezen aan **user.userprincipalname**. TextMagic toepassing verwacht **nameidentifier** worden toegewezen met **user.mail**, dus u de kenmerktoewijzing van het bewerken moet door te klikken op **bewerken** pictogram en wijzig de kenmerk wordt toegewezen.

    ![image](common/edit-attribute.png)

6. Bovendien hierboven verwacht TextMagic toepassing paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:

    | Name |   Bronkenmerk| Naamruimte  |
    | --------------- | --------------- | --------------- |
    | bedrijf | user.companyname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | voornaam               | user.givenname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | achternaam            | user.surname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | telefoon               | user.telephonenumber |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | | |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Typ de naamruimtewaarde voor die rij in het tekstvak **Naamruimte**.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

8. Op de **TextMagic instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-textmagic-single-sign-on"></a>TextMagic voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf TextMagic site als beheerder.

2. Selecteer **Accountinstellingen** onder de gebruikersnaam.

    ![TextMagic configuratie](./media/textmagic-tutorial/config1.png)

3. Klik op de **eenmalige aanmelding (SSO)** tabblad en vult u de volgende velden:  

    ![TextMagic configuratie](./media/textmagic-tutorial/config2.png)

    a. In **id-provider entiteit-ID:** tekstvak, plak de waarde van **Azure AD-id**, die u hebt gekopieerd vanuit Azure portal.

    b. In **id-provider voor eenmalige aanmelding-URL:** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    c. In **id-provider SLO-URL:** tekstvak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    d. Open uw **base-64 gecodeerde certificaat** in Kladblok gedownload vanuit Azure portal, kopieert u de inhoud ervan naar het Klembord en plakt u deze naar de **openbare x509 certificaat:** tekstvak.

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TextMagic.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **TextMagic**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **TextMagic**.

    ![De koppeling TextMagic in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-textmagic-test-user"></a>TextMagic testgebruiker maken

Toepassing ondersteunt **Just-in-time gebruikersinrichting** en na-verificatiegebruikers, in de toepassing automatisch worden gemaakt. U moet de gegevens eenmaal op de eerste aanmelding bij het activeren van de onderliggende account in het systeem in te vullen.
Er is geen actie-item voor u in deze sectie.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel TextMagic in het toegangsvenster, moet u worden automatisch aangemeld bij de TextMagic waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)