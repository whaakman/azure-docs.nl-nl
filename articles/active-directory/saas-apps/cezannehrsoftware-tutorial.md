---
title: 'Zelfstudie: Azure Active Directory-integratie met HR-Software Cezanne | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cezanne HR-Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 62b42e15-c282-492d-823a-a7c1c539f2cc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 1d5edd1335d99c369fa3ef560470edab41276a7a
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819727"
---
# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Zelfstudie: Azure Active Directory-integratie met Cezanne HR-Software

In deze zelfstudie leert u hoe u Cezanne HR om Software te integreren met Azure Active Directory (Azure AD).

Cezanne HR Software integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Cezanne HR-Software heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Cezanne HR-Software (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Cezanne HR-Software, moet u de volgende items:

- Een Azure AD-abonnement
- Een Cezanne HR Software eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cezanne HR-Software uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Cezanne HR-Software uit de galerie toe te voegen
Voor het configureren van de integratie van Cezanne HR-Software in Azure AD, moet u Cezanne HR-Software uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cezanne HR-Software uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Cezanne HR Software**, selecteer **Cezanne HR Software** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Cezanne HR-Software in de lijst met resultaten](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Cezanne HR-Software op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Cezanne HR-Software is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Cezanne HR Software tot stand worden gebracht.

Wijs in Cezanne HR-Software, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Cezanne HR-Software, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Cezanne HR Software](#create-a-cezannehrsoftware-test-user)**  : als u wilt een equivalent van Britta Simon in Cezanne HR-Software die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Cezanne HR-Software.

**Voor het configureren van Azure AD eenmalige aanmelding met Cezanne HR-Software, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Cezanne HR Software** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

1. Op de **Cezanne HR Software domein en URL's** sectie, voert u de volgende stappen uit:

    ![Cezanne HR Software domein en URL's, eenmalige aanmelding informatie](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u de URL: `https://w3.cezanneondemand.com/CezanneOnDemand/-/<tenantidentifier>`

    b. In de **id** tekstvak typt u de URL: `https://w3.cezanneondemand.com/CezanneOnDemand/`

    c. In de **antwoord-URL** tekstvak typt u de URL: `https://w3.cezanneondemand.com:443/cezanneondemand/-/<tenantidentifier>/Saml/samlp`
    
    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en antwoord-URL. Neem contact op met [Cezanne HR-softwareclient ondersteuningsteam](https://cezannehr.com/services/support/) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/cezannehrsoftware-tutorial/tutorial_general_400.png)

1. Op de **Cezanne HR-softwareconfiguratie** sectie, klikt u op **Cezanne HR-Software configureren** openen **aanmelding configureren** venster.

    ![De softwareconfiguratie voor Cezanne HR](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png)

1. Schuif omlaag naar de **Naslaggids** sectie. Kopieer de **SAML Single Sign-On Service-URL en SAML entiteit-ID** van de **Naslaggids sectie.**

    ![De softwareconfiguratie voor Cezanne HR](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure1.png)

1. In een ander browservenster aanmelden voor uw tenant Cezanne HR Software als een beheerder.

1. Klik in het linkernavigatiedeelvenster op **Setup van System**. Ga naar **beveiligingsinstellingen**. Navigeer vervolgens naar **configuratie voor eenmalige aanmelding**.

    ![Configureren van eenmalige aanmelding op App-zijde](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

1. In de **toestaan dat gebruikers zich aanmelden met behulp van de volgende eenmalige aanmelding (SSO) Service** Configuratiescherm, Controleer de **SAML 2.0** vak en selecteer de **Advanced Configuration** optie.

    ![Configureren van eenmalige aanmelding op App-zijde](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

1. Klik op **nieuwe toevoegen** knop.

    ![Configureren van eenmalige aanmelding op App-zijde](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

1. Voer de volgende stappen uit op **SAML 2.0-id-PROVIDERS** sectie.

    ![Configureren van eenmalige aanmelding op App-zijde](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. Voer de naam van uw id-Provider als de **weergavenaam**.

    b. In de **entiteits-id** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal. 

    c. Wijzig de **SAML-Binding** op 'POST'.

    d. In de **Security Token Service-eindpunt** tekstvak, plak de waarde van **SAML-eenmalige aanmelding Service URL** die u hebt gekopieerd vanuit Azure portal.

    e. Voer in het tekstvak gebruikersnaam van het ID-kenmerk `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. Klik op **uploaden** pictogram om de gedownloade certificaat vanuit Azure-portal te uploaden.
    
    g. Klik op de knop **OK**. 

1. Klik op de knop **Save**.

    ![Configureren van eenmalige aanmelding op App-zijde](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/cezannehrsoftware-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/cezannehrsoftware-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/cezannehrsoftware-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/cezannehrsoftware-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Maak een testgebruiker Cezanne HR-Software

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij Cezanne HR-Software, moeten ze worden ingericht in Cezanne HR-Software. In het geval van Cezanne HR-Software is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1.  Meld u in uw bedrijf Cezanne HR Software site als beheerder.

1.  Klik in het linkernavigatiedeelvenster op **Setup van System**. Ga naar **gebruikers beheren**. Navigeer vervolgens naar **Add New User**.

    ![New User](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "New User")

1.  Op **persoon Details** sectie, voert u onderstaande stappen te volgen:

    ![New User](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "New User")
    
    a. Stel **interne gebruiker** als uitgeschakeld.
    
    b. In de **voornaam** tekstvak, type de voornaam van de gebruiker, zoals **Julia**.  
 
    c. In de **achternaam** tekstvak, type de achternaam van de gebruiker, zoals **Simon**.
    
    d. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

1.  Op **accountgegevens** sectie, voert u onderstaande stappen te volgen:

    ![New User](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "New User")
    
    a. In het tekstvak **Gebruikersnaam** typt u het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.
    
    b. In het tekstvak **Wachtwoord** typt u het wachtwoord van de gebruiker.
    
    c. Selecteer **HR Professional** als **beveiligingsrol**.
    
    d. Klik op **OK**.

1. Navigeer naar **Single Sign-On** tabblad en selecteer **nieuwe toevoegen** in de **SAML 2.0-id's** gebied.

    ![Gebruiker](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Gebruiker")

1. Kies uw id-Provider voor de **id-Provider** en in het tekstvak van **gebruikers-id**, voer het e-mailadres van Britta Simon-account.

    ![Gebruiker](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Gebruiker")
    
1. Klik op de knop **Save**.

    ![Gebruiker](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Gebruiker")

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Cezanne HR Software.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Cezanne HR-software, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Cezanne HR Software**.

    ![De koppeling Cezanne HR-Software in de lijst met toepassingen](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Cezanne HR-Software in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Cezanne HR-Software.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/cezannehrsoftware-tutorial/tutorial_general_203.png

