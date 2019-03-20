---
title: 'Zelfstudie: Azure Active Directory-integratie met Igloo Software | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Igloo Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/06/2019
ms.author: jeedes
ms.openlocfilehash: d5c814fbf180ddd1cd3b447533a89cc577151d6c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58093783"
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Zelfstudie: Azure Active Directory-integratie met Igloo Software

In deze zelfstudie leert u hoe u Igloo om Software te integreren met Azure Active Directory (Azure AD).
Igloo Software integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Igloo Software heeft.
* U kunt uw gebruikers worden automatisch aangemeld Igloo Software (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Igloo Software, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Igloo Software eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor igloo Software **SP** gestart door SSO
* Biedt ondersteuning voor igloo Software **Just In Time** inrichten van gebruikers

## <a name="adding-igloo-software-from-the-gallery"></a>Igloo Software uit de galerie toe te voegen

Voor het configureren van de integratie van Igloo Software in Azure AD, moet u Igloo Software uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Igloo Software uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Igloo Software**, selecteer **Igloo Software** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Igloo Software in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Igloo-Software op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Software Igloo tot stand worden gebracht.

Als u wilt configureren en Azure AD eenmalige aanmelding met Igloo Software testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Igloo Software Single Sign-On](#configure-igloo-software-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak Igloo Software testgebruiker](#create-igloo-software-test-user)**  : als u wilt een equivalent van Britta Simon in Igloo-Software die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Igloo Software, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Igloo Software** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Igloo Software domein en URL's, eenmalige aanmelding informatie](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<company name>.igloocommmunities.com`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<company name>.igloocommmunities.com/saml.digest`

    c. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [Igloo softwareclient ondersteuningsteam](https://www.igloosoftware.com/services/support) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **Igloo Software instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-igloo-software-single-sign-on"></a>Igloo Software eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf Igloo Software site als beheerder.

2. Ga naar de **Configuratiescherm**.

     ![Configuratiescherm](./media/igloo-software-tutorial/ic799949.png "Configuratiescherm")

3. In de **lidmaatschap** tabblad **Sign In instellingen**.

    ![Meld u aan instellingen](./media/igloo-software-tutorial/ic783968.png "Aanmeldinstellingen")

4. Klik in de sectie SAML-configuratie **SAML-verificatie configureren**.

    ![SAML-configuratie](./media/igloo-software-tutorial/ic783969.png "SAML-configuratie")

5. In de **algemene configuratie** sectie, voert u de volgende stappen uit:

    ![Algemene configuratie](./media/igloo-software-tutorial/ic783970.png "algemene configuratie")

    a. In de **verbindingsnaam** tekstvak typt u een aangepaste naam voor uw configuratie.

    b. In de **IdP aanmeldings-URL** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. In de **afmeldings-URL van id-provider** tekstvak, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal.

    d. Selecteer **afmelden antwoord en Type van de HTTP-aanvraag** als **POST**.

    e. Open uw **base-64** gecodeerd certificaat in Kladblok gedownload vanuit Azure portal, de inhoud van het kopiëren naar het Klembord en plakt u deze naar de **openbaar certificaat** tekstvak.

6. In de **antwoord en verificatieconfiguratie**, voer de volgende stappen uit:

    ![Antwoord en verificatieconfiguratie](./media/igloo-software-tutorial/IC783971.png "antwoord en Authentication-configuratie")
  
    a. Als **id-Provider**, selecteer **Microsoft ADFS**.

    b. Als **ID-Type**, selecteer **e-mailadres**. 

    c. In de **e kenmerk** tekstvak, type **emailaddress**.

    d. In de **voornaam kenmerk** tekstvak, type **givenname**.

    e. In de **laatste naamkenmerk** tekstvak, type **achternaam**.

7. Voer de volgende stappen uit om de configuratie te voltooien:

    ![Het maken van de gebruiker op aanmelden](./media/igloo-software-tutorial/IC783972.png "maken door gebruiker op aanmelden") 

    a. Als **maken door gebruiker op aanmelden**, selecteer **een nieuwe gebruiker in uw site maken wanneer ze zich aanmelden**.

    b. Als **Aanmeldinstellingen**, selecteer **gebruik SAML-knop op het scherm 'Aanmelden'**.

    c. Klik op **Opslaan**.

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

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Igloo Software.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Igloo Software**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Igloo Software**.

    ![De koppeling Igloo Software in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-igloo-software-test-user"></a>Testgebruiker Igloo Software maken

Er is geen actie-item voor u het inrichten van gebruikers naar Software Igloo configureren.  

Wanneer een toegewezen gebruiker wil zich aanmelden bij Igloo Software via het toegangsvenster, controleert Igloo Software of de gebruiker bestaat.  Als er nog geen gebruikersaccount beschikbaar is, wordt deze automatisch gemaakt door Igloo Software.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Igloo Software in het toegangsvenster, moet u worden automatisch aangemeld bij de Igloo Software waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)