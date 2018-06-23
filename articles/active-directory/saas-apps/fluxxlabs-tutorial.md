---
title: 'Zelfstudie: Azure Active Directory-integratie met Fluxx Labs | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Fluxx Labs.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 0b5fb84f8d8fbed7e1d4112e96b00af3e460661a
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317099"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Zelfstudie: Azure Active Directory-integratie met Fluxx Labs

In deze zelfstudie leert u hoe Fluxx Labs integreren met Azure Active Directory (Azure AD).

Fluxx Labs integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Fluxx Labs heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Fluxx Labs (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Fluxx Labs, moet u de volgende items:

- Een Azure AD-abonnement
- Een Fluxx Labs eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Fluxx Labs uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-fluxx-labs-from-the-gallery"></a>Fluxx Labs uit de galerie toevoegen
Voor het configureren van de integratie van Fluxx Labs in Azure AD, moet u Fluxx Labs uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Fluxx Labs uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Fluxx Labs**, selecteer **Fluxx Labs** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Fluxx Labs in de lijst met resultaten](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Fluxx Labs op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Fluxx Labs in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Fluxx Labs tot stand worden gebracht.

Wijs in Fluxx Labs, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Fluxx Labs, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Fluxx Labs](#create-a-fluxx-labs-test-user)**  - Fluxx Labs die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Fluxx Labs.

**Voor het configureren van Azure AD eenmalige aanmelding met Fluxx Labs, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Fluxx Labs** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_samlbase.png)

3. Op de **Fluxx Labs domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Fluxx Labs domein eenmalige aanmelding informatie](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:

    | Omgeving | URL-patroon|
    |-------------|------------|
    | Productie | `https://<subdomain>.fluxx.io` |
    | Testfase vóór productie | `https://<subdomain>.preprod.fluxxlabs.com`|
        
    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:

    | Omgeving | URL-patroon|
    |-------------|------------|
    | Productie | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Testfase vóór productie | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL. Neem contact op met [Fluxx Labs ondersteuningsteam](mailto:travis@fluxxlabs.com) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/fluxxlabs-tutorial/tutorial_general_400.png)

6. Op de **Fluxx Labs configuratie** sectie, klikt u op **configureren Fluxx Labs** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Fluxx Labs configuratie](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_configure.png)

7. Een ander browservenster geopend, meld u aan bij uw bedrijf Fluxx Labs site als administrator.

8. Selecteer **Admin** onder de **instellingen** sectie.

    ![Fluxx Labs configuratie](./media/fluxxlabs-tutorial/config1.png)

9. Selecteer in het deelvenster Beheer **invoegtoepassingen** > **integraties** en selecteer vervolgens **SAML SSO-(Disabled)**

    ![Fluxx Labs configuratie](./media/fluxxlabs-tutorial/config2.png)

10. Voer de volgende stappen uit in de sectie kenmerk:

    ![Fluxx Labs configuratie](./media/fluxxlabs-tutorial/config3.png)

    a. Selecteer de **SAML SSO** selectievakje.

    b. In de **pad aanvragen** textbox type **/auth/saml**.

    c. In de **retouraanroep pad** textbox type **/auth/saml/callback**.

    d. In de **Assertion Consumer Service Url(Single Sign-On URL)** textbox, voer de **antwoord-URL** waarde, die u hebt ingevoerd in de Azure-portal.

    e. In de **doelgroep (SP entiteit-ID)** textbox, voer de **id** waarde, die u hebt ingevoerd in de Azure-portal.

    f. In de **identiteit Provider SSO doel-URL** textbox, plak de **SAML Single Sign-On Service-URL** waarde, die u hebt gekopieerd uit de Azure portal.

    g. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **Provider identiteitscertificaat** textbox.

    h. In **naam-id indeling** textbox, voert u de waarde `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    i. Klik op **Opslaan**.

    > [!NOTE]
    > Wanneer de inhoud die is opgeslagen, wordt het veld wordt weergegeven leeg laten voor beveiliging, maar de waarde in de configuratie is opgeslagen.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/fluxxlabs-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/fluxxlabs-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/fluxxlabs-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/fluxxlabs-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-fluxx-labs-test-user"></a>Een testgebruiker Fluxx Labs maken

Om Azure AD-gebruikers zich aanmelden bij Fluxx Labs, moeten ze worden ingericht in Fluxx Labs. In het geval van Fluxx Labs is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Fluxx Labs site als beheerder.

2. Klik op de hieronder weergegeven **pictogram**.

    ![Fluxx Labs configuratie](./media/fluxxlabs-tutorial/config6.png)

3. Klik op het dashboard op de hieronder weergegeven pictogram openen de **nieuwe mensen** kaart.

    ![Fluxx Labs configuratie](./media/fluxxlabs-tutorial/config4.png)

4. Op de **nieuwe mensen** sectie, voert u de volgende stappen uit:

    ![Fluxx Labs configuratie](./media/fluxxlabs-tutorial/config5.png)

    a. Fluxx Labs e-mail gebruiken als de unieke id voor eenmalige aanmelding aanmeldingen. Vul de **SSO UID** veld met de e-mailadres van de gebruiker, die overeenkomt met het e-mailadres ze als aanmelding met eenmalige aanmelding gebruiken.

    b. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot Labs Fluxx.

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen Fluxx Labs, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Fluxx Labs**.

    ![De koppeling Fluxx Labs in de lijst met toepassingen](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Fluxx Labs in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Fluxx Labs.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/fluxxlabs-tutorial/tutorial_general_01.png
[2]: ./media/fluxxlabs-tutorial/tutorial_general_02.png
[3]: ./media/fluxxlabs-tutorial/tutorial_general_03.png
[4]: ./media/fluxxlabs-tutorial/tutorial_general_04.png

[100]: ./media/fluxxlabs-tutorial/tutorial_general_100.png

[200]: ./media/fluxxlabs-tutorial/tutorial_general_200.png
[201]: ./media/fluxxlabs-tutorial/tutorial_general_201.png
[202]: ./media/fluxxlabs-tutorial/tutorial_general_202.png
[203]: ./media/fluxxlabs-tutorial/tutorial_general_203.png
