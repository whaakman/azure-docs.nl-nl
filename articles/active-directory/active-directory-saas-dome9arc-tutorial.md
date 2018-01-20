---
title: 'Zelfstudie: Azure Active Directory-integratie met Dome9 boog | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Dome9 boog.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: 2ce4bb1be8b0124c69991765e18ce9922bd2f4a4
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-dome9-arc"></a>Zelfstudie: Azure Active Directory-integratie met Dome9 boog

In deze zelfstudie leert u hoe Dome9 boog integreren met Azure Active Directory (Azure AD).

Dome9 boog integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Dome9 boog heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Dome9 boog (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Dome9 boog, moet u de volgende items:

- Een Azure AD-abonnement
- Een Dome9 boog eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Dome9 boog uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-dome9-arc-from-the-gallery"></a>Dome9 boog uit de galerie toevoegen
Voor het configureren van de integratie van Dome9 boog in Azure AD, moet u Dome9 boog uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Dome9 boog uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Dome9 boog**, selecteer **Dome9 boog** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Dome9 boog in de lijst met resultaten](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Dome9 boog op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Dome9 boog is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker Dome9 boog tot stand worden gebracht.

Dome9 boog, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Dome9 boog, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Dome9 boog](#create-a-dome9-arc-test-user)**  - Dome9 boog die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Dome9 boog configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Dome9 boog, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Dome9 boog** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_samlbase.png)

3. Op de **Dome9 boog domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Dome9 boog domein eenmalige aanmelding informatie](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_url.png)

    a. In de **id** textbox, typ de URL:`https://secure.dome9.com/`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > De naamwaarde voor uw bedrijf selecteert u in het beheerportal dome9, die verderop in de zelfstudie wordt beschreven.

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Dome9 boog domein eenmalige aanmelding informatie](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://secure.dome9.com/sso/saml/<yourcompanyname>`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [Dome9 boog Client ondersteuningsteam](https://dome9.com/about/contact-us/) ophalen van deze waarden. 

5. De SAML-asserties verwacht de softwaretoepassing Dome9 boog in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Single Sign-On attb configureren](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam kenmerk  | Waarde kenmerk | 
    | --------------- | --------------- | 
    | memberof | user.assignedroles | 
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren attb toevoegen](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9_04.png)

    ![Single Sign-On bewerken attb configureren](./media/active-directory-saas-dome9arc-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_certificate.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-dome9arc-tutorial/tutorial_general_400.png)
    
9. Op de **Dome9 boog configuratie** sectie, klikt u op **configureren Dome9 boog** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Dome9 boog configuratie](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_configure.png) 

10. In een ander browservenster, meld u bij uw bedrijf Dome9 boog site als beheerder.

11. Klik op de **profielinstellingen** in de rechterbovenhoek en klik op **Accountinstellingen**. 

    ![Dome9 boog configuratie](./media/active-directory-saas-dome9arc-tutorial/configure1.png)

12. Navigeer naar **SSO** en klik vervolgens op **inschakelen**.

    ![Dome9 boog configuratie](./media/active-directory-saas-dome9arc-tutorial/configure2.png)

13. Voer de volgende stappen uit in de configuratiesectie van eenmalige aanmelding:

    ![Dome9 boog configuratie](./media/active-directory-saas-dome9arc-tutorial/configure3.png)

    a. Voer de naam van het bedrijf in de **Account-ID** textbox. Deze waarde is in het antwoord op een dat URL vermeld in de Azure portal sectie van de URL moet worden gebruikt.

    b. In de **verlener** textbox, plakt u de waarde van **SAML entiteit-ID**, waardoor u de Azure portal-formulier hebt gekopieerd.

    c. In de **Idp eindpunt-url** textbox, plakt u de waarde van **SAML Single Sign-On Service-URL**, waardoor u de Azure portal-formulier hebt gekopieerd.

    d. Open uw gedownloade Base64-gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **X.509-certificaat** textbox.

    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-dome9-arc-test-user"></a>Maak een testgebruiker Dome9 boog

Om Azure AD-gebruikers zich aanmelden bij Dome9 boog, moeten ze worden ingericht in toepassing. Dome9 boog ondersteuning biedt voor inrichting van just-in-time, maar voor die goed te laten werken, gebruiker heeft, selecteert u bepaalde **rol** en dezelfde toewijzen aan de gebruiker.

   >[!Note] 
   >Voor **rol** maken en andere details contact [Dome9 boog Client ondersteuningsteam](https://dome9.com/about/contact-us/).

**Als u wilt inrichten handmatig een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Dome9 boog site als beheerder.

2. Klik op de **gebruikers en rollen** en klik vervolgens op **gebruikers**.

    ![Werknemer toevoegen](./media/active-directory-saas-dome9arc-tutorial/user1.png)

3. Klik op **gebruiker toevoegen**.

    ![Werknemer toevoegen](./media/active-directory-saas-dome9arc-tutorial/user2.png)

4. In de **gebruiker maken** sectie, voert u de volgende stappen uit:
    
    ![Werknemer toevoegen](./media/active-directory-saas-dome9arc-tutorial/user3.png)

    a. In de **e** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    b. In de **voornaam** textbox type voornaam van de gebruiker zoals Britta.

    c. In de **achternaam** textbox achternaam van de gebruiker zoals Simon type.

    d. Controleer **SSO gebruiker** als **op**.

    e. Klik op **maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Dome9 boog.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon Dome9 boog, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Dome9 boog**.

    ![De koppeling Dome9 boog in de lijst met toepassingen](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Dome9 boog in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Dome9 boog.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_203.png

