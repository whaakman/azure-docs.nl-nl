---
title: 'Zelfstudie: Azure Active Directory-integratie met Mozy Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Mozy Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.openlocfilehash: 55c83c91f4c183e7863c6a9fa129b5469e52afdd
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57834996"
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met Mozy Enterprise

In deze zelfstudie leert u hoe u Mozy Enterprise integreren met Azure Active Directory (Azure AD).
Mozy Enterprise integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Mozy Enterprise heeft.
* U kunt uw gebruikers worden automatisch aangemeld Mozy Enterprise (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Mozy Enterprise, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Mozy ondernemingsbrede eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Mozy Enterprise **SP** gestart door SSO

## <a name="adding-mozy-enterprise-from-the-gallery"></a>Mozy Enterprise uit de galerie toe te voegen

Voor het configureren van de integratie van Mozy Enterprise in Azure AD, moet u Mozy Enterprise uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Mozy Enterprise uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Mozy Enterprise**, selecteer **Mozy Enterprise** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Mozy onderneming in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Mozy Enterprise op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de onderneming Mozy tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Mozy Enterprise, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Mozy Enterprise Single Sign-On](#configure-mozy-enterprise-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak Mozy Enterprise testgebruiker](#create-mozy-enterprise-test-user)**  : als u wilt een equivalent van Britta Simon in Mozy-onderneming die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Mozy Enterprise, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Mozy Enterprise** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Mozy Enterprise domein en URL's, eenmalige aanmelding informatie](common/sp-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<tenantname>.Mozyenterprise.com`

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Mozy Enterprise Client-ondersteuningsteam](http://support.mozy.com/) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **Mozy Enterprise instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-mozy-enterprise-single-sign-on"></a>Configureer Mozy Enterprise Single Sign-On

1. Meld u in een ander browservenster in uw bedrijf Mozy Enterprise site als beheerder.

2. In de **configuratie** sectie, klikt u op **verificatiebeleid**.
   
    ![Verificatiebeleid](./media/mozy-enterprise-tutorial/ic777314.png "verificatiebeleid")

3. Op de **verificatiebeleid** sectie, voert u de volgende stappen uit:
   
    ![Verificatiebeleid](./media/mozy-enterprise-tutorial/ic777315.png "verificatiebeleid")
   
    a. Selecteer **adreslijstservice** als **Provider**.
   
    b. Selecteer **Gebruik LDAP-Push**.
   
    c. Klik op het tabblad **SAML-verificatie**.
   
    d. Plakken **aanmeldings-URL**, die u hebt gekopieerd vanuit de Azure portal in de **-URL webverificatie** tekstvak.
   
    e. Plakken **Azure AD-id**, die u hebt gekopieerd vanuit de Azure portal in de **SAML-eindpunt** tekstvak.
   
    f. Open uw gedownloade base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u het gehele certificaat in **SAML-certificaat** tekstvak.
   
    g. Selecteer **eenmalige aanmelding inschakelen voor beheerders zich kunnen aanmelden met hun netwerkreferenties**.
   
    h. Klik op **Wijzigingen opslaan**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Mozy onderneming.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Mozy Enterprise**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Mozy Enterprise**.

    ![De koppeling Mozy Enterprise in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-mozy-enterprise-test-user"></a>Mozy Enterprise testgebruiker maken

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij Mozy Enterprise, moeten ze worden ingericht voor Mozy Enterprise. In het geval van Mozy Enterprise is inrichten een handmatige taak.

>[!NOTE]
>U kunt alle andere Mozy Enterprise gebruiker-account maken van hulpprogramma's of API's geleverd door Mozy Enterprise aan inrichten AAD-gebruikersaccounts.

**Voer de volgende stappen uit om een gebruikersaccount in te richten:**

1. Meld u aan bij uw **Mozy Enterprise** tenant.

2. Klik op **gebruikers**, en klik vervolgens op **Add New User**.
   
    ![Gebruikers](./media/mozy-enterprise-tutorial/ic777317.png "Gebruikers")
   
    >[!NOTE]
    >De **Add New User** optie wordt alleen weergegeven als **Mozy** is geselecteerd als de provider onder **verificatiebeleid**. Als SAML-verificatie is geconfigureerd, worden klikt u vervolgens de gebruikers automatisch toegevoegd aan hun eerste keer aanmelden via eenmalige aanmelding op.
    
3. In het gebruikersdialoogvenster nieuwe de volgende stappen uitvoeren:
   
    ![Gebruikers toevoegen](./media/mozy-enterprise-tutorial/ic777318.png "gebruikers toevoegen")
   
    a. Uit de **kiest u een groep** , selecteert u een groep.
   
    b. Uit de **welk type gebruiker** , selecteert u een type.
   
    c. In de **gebruikersnaam** tekstvak, typ de naam van de Azure AD-gebruiker.
   
    d. In de **e** tekstvak typt u het e-mailadres van de Azure AD-gebruiker.
   
    e. Selecteer **verzenden van e-mailadres van gebruiker instructie**.
   
    f. Klik op **toevoegen van gebruiker (s)**.

     >[!NOTE]
     > Nadat de gebruiker is gemaakt, wordt een e-mailbericht worden verzonden naar de Azure AD-gebruiker met een koppeling om te bevestigen dat het account voordat deze geactiveerd wordt.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Mozy Enterprise in het toegangsvenster, moet u worden automatisch aangemeld bij de Mozy onderneming waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

