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
ms.openlocfilehash: 367310527619d4bdb5f84a80c567a9d83698846e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433767"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Zelfstudie: Azure Active Directory-integratie met Fluxx Labs

In deze zelfstudie leert u hoe u Fluxx Labs te integreren met Azure Active Directory (Azure AD).

Fluxx Labs te integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Fluxx Labs heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Fluxx Labs (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Fluxx Labs, moet u de volgende items:

- Een Azure AD-abonnement
- Een Fluxx Labs eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Fluxx Labs uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-fluxx-labs-from-the-gallery"></a>Fluxx Labs uit de galerie toe te voegen
Voor het configureren van de integratie van Fluxx Labs in Azure AD, moet u Fluxx Labs uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Fluxx Labs uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Fluxx Labs**, selecteer **Fluxx Labs** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Fluxx Labs in de lijst met resultaten](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Fluxx Labs op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Fluxx Labs is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Fluxx Labs tot stand worden gebracht.

Wijs in Fluxx Labs, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Fluxx Labs, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Fluxx Labs](#create-a-fluxx-labs-test-user)**  : als u wilt een equivalent van Britta Simon in Fluxx Labs die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Fluxx Labs.

**Voor het configureren van Azure AD eenmalige aanmelding met Fluxx Labs, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Fluxx Labs** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_samlbase.png)

1. Op de **Fluxx Labs domein en URL's** sectie, voert u de volgende stappen uit:

    ![Fluxx Labs domein en URL's, eenmalige aanmelding informatie](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon:

    | Omgeving | URL-patroon|
    |-------------|------------|
    | Productie | `https://<subdomain>.fluxx.io` |
    | Testfase vóór productie | `https://<subdomain>.preprod.fluxxlabs.com`|
        
    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:

    | Omgeving | URL-patroon|
    |-------------|------------|
    | Productie | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Testfase vóór productie | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke id en de antwoord-URL. Neem contact op met [Fluxx Labs ondersteuningsteam](mailto:travis@fluxxlabs.com) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/fluxxlabs-tutorial/tutorial_general_400.png)

1. Op de **Fluxx Labs configuratie** sectie, klikt u op **configureren Fluxx Labs** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Configuratie van Fluxx Labs](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_configure.png)

1. In een ander browservenster, meld u aan bij uw bedrijf Fluxx Labs site als administrator.

1. Selecteer **Admin** hieronder de **instellingen** sectie.

    ![Configuratie van Fluxx Labs](./media/fluxxlabs-tutorial/config1.png)

1. Selecteer in het deelvenster Beheer **Plug-ins** > **integraties** en selecteer vervolgens **SAML SSO-(Disabled)**

    ![Configuratie van Fluxx Labs](./media/fluxxlabs-tutorial/config2.png)

1. Voer de volgende stappen uit in de sectie kenmerk:

    ![Configuratie van Fluxx Labs](./media/fluxxlabs-tutorial/config3.png)

    a. Selecteer de **SAML SSO** selectievakje.

    b. In de **pad aanvraag** tekstvak, type **/auth/saml**.

    c. In de **Callback pad** tekstvak, type **/auth/saml/callback**.

    d. In de **Assertion Consumer Service Url(Single Sign-On URL)** tekstvak, voer de **antwoord-URL** waarde die u hebt opgegeven in de Azure-portal.

    e. In de **doelgroep (SP entiteits-ID)** tekstvak, voer de **id** waarde die u hebt opgegeven in de Azure-portal.

    f. In de **doel-URL van SSO-identiteitsprovider** tekstvak, plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

    g. Het base-64 gecodeerde certificaat openen in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **Provider identiteitscertificaat** tekstvak.

    h. In **naam-id indeling** tekstvak, voert u de waarde `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    i. Klik op **Opslaan**.

    > [!NOTE]
    > Zodra de opgeslagen inhoud het veld wordt weergegeven leeg voor beveiliging, maar de waarde in de configuratie is opgeslagen.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/fluxxlabs-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/fluxxlabs-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/fluxxlabs-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/fluxxlabs-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-fluxx-labs-test-user"></a>Maak een testgebruiker Fluxx Labs

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Fluxx Labs, moeten ze worden ingericht in Fluxx Labs. In het geval van Fluxx Labs is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Fluxx Labs site aan als beheerder.

1. Klik op de hieronder weergegeven **pictogram**.

    ![Configuratie van Fluxx Labs](./media/fluxxlabs-tutorial/config6.png)

1. Klik op het dashboard op de hieronder weergegeven pictogram opent de **nieuwe personen** kaart.

    ![Configuratie van Fluxx Labs](./media/fluxxlabs-tutorial/config4.png)

1. Op de **nieuwe personen** sectie, voert u de volgende stappen uit:

    ![Configuratie van Fluxx Labs](./media/fluxxlabs-tutorial/config5.png)

    a. E-mail als de unieke id Fluxx Labs gebruiken voor SSO-aanmeldingen. Vul de **SSO UID** veld met de e-mailadres van de gebruiker, die overeenkomt met het e-mailadres, die ze als voor aanmelding bij met eenmalige aanmelding gebruiken.

    b. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Fluxx Labs.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon tot Fluxx Labs, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Fluxx Labs**.

    ![De koppeling Fluxx Labs in de lijst met toepassingen](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Fluxx Labs in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Fluxx Labs.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

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
