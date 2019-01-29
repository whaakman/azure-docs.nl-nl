---
title: 'Zelfstudie: Azure Active Directory-integratie met Riskware | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Riskware.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jeedes
ms.openlocfilehash: 3b4c979bf03b23280c9389a043375f088624efe6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163242"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Zelfstudie: Azure Active Directory-integratie met Riskware

In deze zelfstudie leert u hoe u Riskware integreren met Azure Active Directory (Azure AD).

Riskware integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Riskware heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Riskware (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Riskware, moet u de volgende items:

- Een Azure AD-abonnement
- Een Riskware eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Riskware uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-riskware-from-the-gallery"></a>Riskware uit de galerie toe te voegen
Voor het configureren van de integratie van Riskware in Azure AD, moet u Riskware uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Riskware uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Riskware**, selecteer **Riskware** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Riskware in de lijst met resultaten](./media/riskware-tutorial/tutorial_riskware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Riskware op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Riskware is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Riskware tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Riskware, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Riskware](#create-a-riskware-test-user)**  : als u wilt een equivalent van Britta Simon in Riskware die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Riskware.

**Voor het configureren van Azure AD eenmalige aanmelding met Riskware, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Riskware** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/riskware-tutorial/tutorial_riskware_samlbase.png)

1. Op de **Riskware domein en URL's** sectie, voert u de volgende stappen uit:

    ![Riskware domein en URL's, eenmalige aanmelding informatie](./media/riskware-tutorial/tutorial_riskware_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon:
    | Omgeving| URL-patroon|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | PROD| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | DEMO| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` |
    |||

    b. In de **id (entiteits-ID)** tekstvak, een URL typen:
    | Omgeving| URL-patroon|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | PROD| `https://riskcloud.net/prod` |
    | DEMO| `https://riskcloud.net/demo` |
    |||

    > [!NOTE]
    > De waarde van de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Riskware Client ondersteuningsteam](mailto:support@pansoftware.com.au) om de waarde.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/riskware-tutorial/tutorial_riskware_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/riskware-tutorial/tutorial_general_400.png)

1. Op de **Riskware configuratie** sectie, klikt u op **configureren Riskware** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Riskware configuratie](./media/riskware-tutorial/tutorial_riskware_configure.png)

1. In een ander browservenster aanmelden bij uw bedrijf Riskware site als beheerder.

1. Klik in de rechterbovenhoek op **onderhoud** om de onderhoudspagina te openen.

    ![Riskware configuraties beheren](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Klik in de onderhoudspagina op **verificatie**.

    ![Configuratie van Riskware authen](./media/riskware-tutorial/tutorial_riskware_authen.png)

1. In **verificatieconfiguratie** pagina, voert u de volgende stappen uit:

    ![Configuratie van Riskware authenconfig](./media/riskware-tutorial/tutorial_riskware_config.png)

    a. Selecteer **Type** als **SAML** voor verificatie.

    b. In de **Code** tekstvak typt u uw code zoals AZURE_UAT.

    c. In de **beschrijving** tekstvak typt u de beschrijving in, zoals AZURE-configuratie voor eenmalige aanmelding.

    d. In **één teken op pagina** tekstvak, plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

    e. In **afmelden pagina** tekstvak, plak de **afmelding URL** waarde die u hebt gekopieerd vanuit Azure portal.

    f. In de **Post formulierveld** tekstvak typt u de veldnaam aanwezig zijn in de Post-antwoord met SAML, zoals SAMLResponse

    g. In de **XML-tagnaam identiteit** tekstvak kenmerk van het type, waarin de unieke id in het SAML-antwoord, zoals NameID.

    h. Open het gedownloade **Metadata Xml** kopieert het certificaat uit het bestand met metagegevens van Azure-portal in Kladblok en plak deze in de **certificaat** tekstvak

    i. In **consument URL** tekstvak, plak de waarde van **antwoord-URL**, die u ontvangt van het ondersteuningsteam.

    j. In **verlener** tekstvak, plak de waarde van **id**, die u ontvangt van het ondersteuningsteam.

    > [!Note]
    > Neem contact op met [Riskware Client ondersteuningsteam](mailto:support@pansoftware.com.au) om deze waarden te verkrijgen

    k. Selecteer **gebruik POST** selectievakje.

    l. Selecteer **gebruik SAML-aanvraag** selectievakje.

    m. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/riskware-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/riskware-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/riskware-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/riskware-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-riskware-test-user"></a>Maak een testgebruiker Riskware

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Riskware, moeten ze worden ingericht voor Riskware. In Riskware is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij Riskware als een beveiligingsbeheerder.

1. Klik in de rechterbovenhoek op **onderhoud** om de onderhoudspagina te openen. 

    ![Configuratie van Riskware onderhoudt](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. Klik in de onderhoudspagina op **mensen**.

    ![Configuratie van Riskware personen](./media/riskware-tutorial/tutorial_riskware_people.png)

1. Selecteer **Details** tabblad en voer de volgende stappen uit:

    ![Informatie over de configuratie van Riskware](./media/riskware-tutorial/tutorial_riskware_details.png)

    a. Selecteer **Type persoon** zoals werknemer.

    b. In **voornaam** tekstvak, geef de voornaam van de gebruiker, zoals **Julia**.

    c. In **achternaam** tekstvak, geef de achternaam van de gebruiker, zoals **Simon**.

1. Op de **Security** tabblad, voert u de volgende stappen uit:

    ![Configuratie van Riskware beveiliging](./media/riskware-tutorial/tutorial_riskware_security.png)

    a. Onder **verificatie** sectie, selecteer de **verificatie** modus, die u hebt instellen zoals AZURE-configuratie voor eenmalige aanmelding.

    b. Onder **inloggegevens** sectie in de **gebruikers-ID** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    c. In de **wachtwoord** tekstvak, voer het wachtwoord van de gebruiker.

1. Op de **organisatie** tabblad, voert u de volgende stappen uit:

    ![Configuratie van Riskware org](./media/riskware-tutorial/tutorial_riskware_org.png)

    a. Selecteer de optie als **Level1** organisatie.

    b. Onder **persoon primaire werkplek** sectie in de **locatie** tekstvak typt u uw locatie.

    c. Onder **werknemer** sectie, selecteer **Werknemerstatus** zoals Casual.

1. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Riskware.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan Riskware toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Riskware**.

    ![De koppeling Riskware in de lijst met toepassingen](./media/riskware-tutorial/tutorial_riskware_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Riskware in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Riskware.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/riskware-tutorial/tutorial_general_01.png
[2]: ./media/riskware-tutorial/tutorial_general_02.png
[3]: ./media/riskware-tutorial/tutorial_general_03.png
[4]: ./media/riskware-tutorial/tutorial_general_04.png

[100]: ./media/riskware-tutorial/tutorial_general_100.png

[200]: ./media/riskware-tutorial/tutorial_general_200.png
[201]: ./media/riskware-tutorial/tutorial_general_201.png
[202]: ./media/riskware-tutorial/tutorial_general_202.png
[203]: ./media/riskware-tutorial/tutorial_general_203.png

