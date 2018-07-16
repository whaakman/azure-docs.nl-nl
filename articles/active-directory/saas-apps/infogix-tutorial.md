---
title: 'Zelfstudie: Azure Active Directory-integratie met Infogix Data3Sixty beheren | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Infogix Data3Sixty beheren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: aa3109b8-bdbe-45ae-933a-2eb4dc03855c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: ef45bbb20ccb5db8c383717eb81bfe8fa18720c7
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39042840"
---
# <a name="tutorial-azure-active-directory-integration-with-infogix-data3sixty-govern"></a>Zelfstudie: Azure Active Directory-integratie met Infogix Data3Sixty beheren

In deze zelfstudie leert u over het integreren van Infogix Data3Sixty beheren met Azure Active Directory (Azure AD).

Infogix Data3Sixty regelen integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Infogix Data3Sixty beheren heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Infogix Data3Sixty beheren (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Infogix Data3Sixty beheren, moet u de volgende items:

- Een Azure AD-abonnement
- Een Infogix Data3Sixty regelen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen Infogix Data3Sixty beheren vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-infogix-data3sixty-govern-from-the-gallery"></a>Toe te voegen Infogix Data3Sixty beheren vanuit de galerie
Voor het configureren van de integratie van Infogix Data3Sixty beheren in Azure AD, moet u Infogix Data3Sixty beheren vanuit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen Infogix Data3Sixty beheren vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Infogix Data3Sixty bepalen**, selecteer **Infogix Data3Sixty regelen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Infogix Data3Sixty beheren in de lijst met resultaten](./media/infogix-tutorial/tutorial_infogix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Infogix Data3Sixty bepalen op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Infogix Data3Sixty beheren in Azure AD aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Infogix Data3Sixty bepalen tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Infogix Data3Sixty beheren, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Infogix Data3Sixty regelen](#create-an-infogix-data3sixty-govern-test-user)**  : als u wilt een equivalent van Britta Simon in Infogix Data3Sixty beheren die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Infogix Data3Sixty beheren.

**Voor het configureren van Azure AD eenmalige aanmelding met Infogix Data3Sixty beheren, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Infogix Data3Sixty regelen** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/infogix-tutorial/tutorial_infogix_samlbase.png)

3. Op de **Infogix Data3Sixty regelen domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Infogix Data3Sixty regelen domein en URL's, eenmalige aanmelding informatie](./media/infogix-tutorial/tutorial_infogix_url.png)

    a. In de **id** tekstvak, een URL typen: `https://data3sixty.com/ui`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.data3sixty.com/sso/acs`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Infogix Data3Sixty regelen domein en URL's, eenmalige aanmelding informatie](./media/infogix-tutorial/tutorial_infogix_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.data3sixty.com`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [Infogix Data3Sixty regelen Client ondersteuningsteam](mailto:data3sixtysupport@infogix.com) om deze waarden te verkrijgen.

5. Toepassing Infogix Data3Sixty regelen wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Single Sign-On attb configureren](./media/infogix-tutorial/tutorial_infogix_attribute.png)
    
6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | Voornaam           | User.givenName |
    | Achternaam        | User.surname |
    | gebruikersnaam       | User.mail    |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Configureren van eenmalige aanmelding toevoegen](./media/infogix-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On Addattb configureren](./media/infogix-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Raw)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/infogix-tutorial/tutorial_infogix_certificate.png)

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/infogix-tutorial/tutorial_general_400.png)
    
9. Op de **Infogix Data3Sixty beheren configuratie** sectie, klikt u op **configureren Infogix Data3Sixty regelen** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Infogix Data3Sixty beheren configuratie](./media/infogix-tutorial/tutorial_infogix_configure.png) 

10. Het configureren van eenmalige aanmelding op **Infogix Data3Sixty regelen** zijde, moet u voor het verzenden van de gedownloade **certificaat (Raw), URL van de afmelding, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [ Ondersteuningsteam Infogix Data3Sixty regelen](mailto:data3sixtysupport@infogix.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/infogix-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/infogix-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/infogix-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/infogix-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-infogix-data3sixty-govern-test-user"></a>Maak een testgebruiker Infogix Data3Sixty beheren


Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Infogix Data3Sixty beheren. Infogix Data3Sixty regelen biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Infogix Data3Sixty beheren als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [ondersteuningsteam Infogix Data3Sixty regelen](mailto:data3sixtysupport@infogix.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te bepalen van Infogix Data3Sixty.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon om te bepalen van Infogix Data3Sixty, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Infogix Data3Sixty regelen**.

    ![De koppeling Infogix Data3Sixty beheren in de lijst met toepassingen](./media/infogix-tutorial/tutorial_infogix_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Infogix Data3Sixty beheren in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Infogix Data3Sixty beheren.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/infogix-tutorial/tutorial_general_01.png
[2]: ./media/infogix-tutorial/tutorial_general_02.png
[3]: ./media/infogix-tutorial/tutorial_general_03.png
[4]: ./media/infogix-tutorial/tutorial_general_04.png

[100]: ./media/infogix-tutorial/tutorial_general_100.png

[200]: ./media/infogix-tutorial/tutorial_general_200.png
[201]: ./media/infogix-tutorial/tutorial_general_201.png
[202]: ./media/infogix-tutorial/tutorial_general_202.png
[203]: ./media/infogix-tutorial/tutorial_general_203.png

