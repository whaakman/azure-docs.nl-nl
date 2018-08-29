---
title: 'Zelfstudie: Azure Active Directory-integratie met Nuclino | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Nuclino.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 74bbab82-5581-4dcf-8806-78f77c746968
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: jeedes
ms.openlocfilehash: 1a5346b98de48b1a2f8928c3c2bf30730588e9c1
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43146129"
---
# <a name="tutorial-azure-active-directory-integration-with-nuclino"></a>Zelfstudie: Azure Active Directory-integratie met Nuclino

In deze zelfstudie leert u hoe u Nuclino integreren met Azure Active Directory (Azure AD).

Nuclino integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Nuclino heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Nuclino (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Nuclino, moet u de volgende items:

- Een Azure AD-abonnement
- Een Nuclino eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Nuclino uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-nuclino-from-the-gallery"></a>Nuclino uit de galerie toe te voegen

Voor het configureren van de integratie van Nuclino in Azure AD, moet u Nuclino uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Nuclino uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Nuclino**, selecteer **Nuclino** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Nuclino in de lijst met resultaten](./media/nuclino-tutorial/tutorial_nuclino_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Nuclino op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Nuclino is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Nuclino tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Nuclino, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Nuclino](#create-a-nuclino-test-user)**  : als u wilt een equivalent van Britta Simon in Nuclino die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Nuclino.

**Voor het configureren van Azure AD eenmalige aanmelding met Nuclino, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Nuclino** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/nuclino-tutorial/tutorial_nuclino_samlbase.png)

3. Op de **Nuclino domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Nuclino domein en URL's, eenmalige aanmelding informatie](./media/nuclino-tutorial/tutorial_nuclino_url1.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://api.nuclino.com/api/sso/<UNIQUE-ID>/metadata`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://api.nuclino.com/api/sso/<UNIQUE-ID>/acs`

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL van de **verificatie** sectie, die verderop in deze zelfstudie wordt uitgelegd.

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Nuclino domein en URL's, eenmalige aanmelding informatie](./media/nuclino-tutorial/tutorial_nuclino_url2.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://app.nuclino.com/<UNIQUE-ID>/login`

    > [!NOTE]
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Nuclino Client ondersteuningsteam](mailto:contact@nuclino.com) deze waarde op te halen.

5. Nuclino toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/Nuclino-tutorial/tutorial_attribute.png)

6. Klik op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie om uit te breiden de kenmerken. De volgende stappen uitvoeren op elk van de kenmerken weergegeven:

    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |
    | first_name | User.givenName |
    | last_name | User.surname |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/nuclino-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/nuclino-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak, type de **kenmerknaam** wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/nuclino-tutorial/tutorial_nuclino_certificate.png)

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/nuclino-tutorial/tutorial_general_400.png)

9. Op de **Nuclino configuratie** sectie, klikt u op **configureren Nuclino** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Nuclino configuratie](./media/nuclino-tutorial/tutorial_nuclino_configure.png)

10. In een ander browservenster aanmelden bij uw bedrijf Nuclino site als beheerder.

11. Klik op de **PICTOGRAM**.

    ![Nuclino configuratie](./media/nuclino-tutorial/configure1.png)

12. Klik op de **Azure AD-eenmalige aanmelding** en selecteer **Team instellingen** in de vervolgkeuzelijst.

    ![Nuclino configuratie](./media/nuclino-tutorial/configure2.png)

13. Selecteer **verificatie** in het navigatiedeelvenster links.

    ![Nuclino configuratie](./media/nuclino-tutorial/configure3.png)

14. In de **verificatie** sectie, voert u de volgende stappen uit:

    ![Nuclino configuratie](./media/nuclino-tutorial/configure4.png)

    a. Selecteer **SAML gebaseerde eenmalige aanmelding (SSO)**.

    b. Kopie **ACS-URL (u moet kopieert en plakt deze met uw provider voor eenmalige aanmelding)** waarde en plak deze in de **antwoord-URL** tekstvak van de **Nuclino domein en URL's** sectie in Azure Portal.

    c. Kopie **entiteit-ID (u moet kopieert en plakt deze met uw provider voor eenmalige aanmelding)** waarde en plak deze in de **id** tekstvak van de **Nuclino domein en URL's** sectie in Azure Portal.

    d. In de **SSO URL** tekstvak, plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

    e. In de **entiteit-ID** tekstvak, plak de **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    f. Open uw gedownloade **Certificate(Base64)** bestand in Kladblok. Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **openbaar certificaat** in het tekstvak.

    g. Klik op **wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/nuclino-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/nuclino-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/nuclino-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/nuclino-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-nuclino-test-user"></a>Maak een testgebruiker Nuclino

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Nuclino. Nuclino biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Nuclino als deze nog niet bestaat.

> [!Note]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Nuclino ondersteuningsteam](mailto:contact@nuclino.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Nuclino.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan Nuclino toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Nuclino**.

    ![De koppeling Nuclino in de lijst met toepassingen](./media/nuclino-tutorial/tutorial_nuclino_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Nuclino in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Nuclino.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/nuclino-tutorial/tutorial_general_01.png
[2]: ./media/nuclino-tutorial/tutorial_general_02.png
[3]: ./media/nuclino-tutorial/tutorial_general_03.png
[4]: ./media/nuclino-tutorial/tutorial_general_04.png

[100]: ./media/nuclino-tutorial/tutorial_general_100.png

[200]: ./media/nuclino-tutorial/tutorial_general_200.png
[201]: ./media/nuclino-tutorial/tutorial_general_201.png
[202]: ./media/nuclino-tutorial/tutorial_general_202.png
[203]: ./media/nuclino-tutorial/tutorial_general_203.png