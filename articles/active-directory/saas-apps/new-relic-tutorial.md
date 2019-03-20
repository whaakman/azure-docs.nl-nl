---
title: 'Zelfstudie: Azure Active Directory-integratie met New Relic | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en New Relic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: 4a9141c0840ff5d962adceb0d0481102cff868d4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57897347"
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Zelfstudie: Azure Active Directory-integratie met New Relic

In deze zelfstudie leert u hoe u New Relic integreren met Azure Active Directory (Azure AD).
New Relic integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot de New Relic heeft.
* U kunt uw gebruikers worden automatisch aangemeld New Relic (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met New Relic, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Nieuwe Relic eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* New Relic biedt ondersteuning voor **SP** gestart door SSO

## <a name="adding-new-relic-from-the-gallery"></a>New Relic uit de galerie toe te voegen

Voor het configureren van de integratie van New Relic in Azure AD, moet u New Relic uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de New Relic uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **New Relic**, selecteer **New Relic** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![New Relic in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met New Relic op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in New Relic tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met New Relic, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van nieuwe Relic Single Sign-On](#configure-new-relic-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[New Relic testgebruiker maken](#create-new-relic-test-user)**  : als u wilt een equivalent van Britta Simon in New Relic die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met New Relic, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **New Relic** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Nieuwe Relic domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In de **aanmeldings-URL** tekstvak typt u een URL met behulp van het volgende patroon: `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login` -Zorg ervoor dat u vervangen door uw eigen New Relic-Account-ID.

    b. Typ een URL in het vak **Id (Entiteits-id)**: `rpm.newrelic.com`

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **instellen van New Relic** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-new-relic-single-sign-on"></a>Nieuwe Relic eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw **New Relic** bedrijf site als administrator.

2. Klik in het menu aan de bovenkant op **Accountinstellingen**.
   
    ![Account Settings](./media/new-relic-tutorial/ic797036.png "Account Settings")

3. Klik op de **beveiligings- en authenticatieservices** tabblad en klik vervolgens op de **eenmalige aanmelding** tabblad.
   
    ![Single Sign-On](./media/new-relic-tutorial/ic797037.png "Single Sign-On")

4. Voer de volgende stappen uit op de pagina van het dialoogvenster SAML:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
    a. Klik op **bestand kiezen** om uw gedownloade Azure Active Directory-certificaat te uploaden.

    b. In de **externe aanmeldings-URL** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.
   
    c. In de **afmelden aanvoer URL** tekstvak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **mijn wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door toegang te verlenen aan de New Relic gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **New Relic**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **New Relic**.

    ![De New Relic-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-new-relic-test-user"></a>New Relic testgebruiker maken

Als u wilt dat Azure Active Directory-gebruikers zich aanmelden bij de New Relic, moeten ze worden ingericht in New Relic. In het geval van New Relic is inrichten een handmatige taak.

**Voor het inrichten van een New Relic-gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **New Relic** bedrijf site als administrator.

2. Klik in het menu aan de bovenkant op **Accountinstellingen**.
   
    ![Account Settings](./media/new-relic-tutorial/ic797040.png "Account Settings")

3. In de **Account** deelvenster aan de linkerkant te klikken op **samenvatting**, en klik vervolgens op **gebruiker toevoegen**.
   
    ![Account Settings](./media/new-relic-tutorial/ic797041.png "Account Settings")

4. Op de **actieve gebruikers** dialoogvenster, voer de volgende stappen uit:
   
    ![Actieve gebruikers](./media/new-relic-tutorial/ic797042.png "actieve gebruikers")
   
    a. In de **e** tekstvak typt u het e-mailadres van een geldige Azure Active Directory-gebruiker die u inrichten wilt.

    b. Als **rol** Selecteer **gebruiker**.

    c. Klik op **deze gebruiker toevoegen**.

>[!NOTE]
>U kunt alle andere New Relic gebruiker-account maken van hulpprogramma's of API's geleverd door New Relic aan inrichten AAD-gebruikersaccounts.
> 

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de New Relic-tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de New Relic waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

