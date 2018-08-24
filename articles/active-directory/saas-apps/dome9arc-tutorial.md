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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2018
ms.author: jeedes
ms.openlocfilehash: 934520764749b5abce9aefe22b8eb9a5d8e490f2
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746488"
---
# <a name="tutorial-azure-active-directory-integration-with-dome9-arc"></a>Zelfstudie: Azure Active Directory-integratie met Dome9 boog

In deze zelfstudie leert u hoe u Dome9 boog integreren met Azure Active Directory (Azure AD).

Dome9 boog integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Dome9 boog heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de boog Dome9 (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Dome9 boog, moet u de volgende items:

- Een Azure AD-abonnement
- Een Dome9 boog eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Dome9 boog uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-dome9-arc-from-the-gallery"></a>Dome9 boog uit de galerie toe te voegen

Voor het configureren van de integratie van Dome9 boog in Azure AD, moet u Dome9 boog uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Dome9 boog uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Dome9 boog**, selecteer **Dome9 boog** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Dome9 boog in de lijst met resultaten](./media/dome9arc-tutorial/tutorial_dome9arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Dome9 boog op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent Dome9 boog is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker Dome9 boog tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Dome9 boog, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Dome9 boog](#create-a-dome9-arc-test-user)**  : als u wilt een equivalent van Britta Simon Dome9 boog die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Dome9 boog.

**Voor het configureren van Azure AD eenmalige aanmelding met Dome9 boog, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Dome9 boog** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/dome9arc-tutorial/tutorial_dome9arc_samlbase.png)

3. Op de **Dome9 boog domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Dome9 boog domein en URL's, eenmalige aanmelding informatie](./media/dome9arc-tutorial/tutorial_dome9arc_url.png)

    a. In de **id** tekstvak typt u de URL: `https://secure.dome9.com/`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > De waarde van uw bedrijf naam selecteert u in de beheerportal dome9, die later in de zelfstudie wordt uitgelegd.

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Dome9 boog domein en URL's, eenmalige aanmelding informatie](./media/dome9arc-tutorial/tutorial_dome9arc_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://secure.dome9.com/sso/saml/<yourcompanyname>`
 
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [Dome9 boog Client ondersteuningsteam](https://dome9.com/about/contact-us/) om deze waarden te verkrijgen. 

5. De toepassing Dome9 boog Software wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Single Sign-On attb configureren](./media/dome9arc-tutorial/tutorial_dome9arc_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk  | Waarde kenmerk | 
    | --------------- | --------------- | 
    | memberOf | User.assignedroles |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Configureren van eenmalige aanmelding attb toevoegen](./media/dome9arc-tutorial/tutorial_dome9_04.png)

    ![Single Sign-On bewerken attb configureren](./media/dome9arc-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.
    
    > [!NOTE]
    > Raadpleeg dit [koppeling](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-enterprise-app-role-management) over het configureren en instellen van de functies voor de toepassing.

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/dome9arc-tutorial/tutorial_dome9arc_certificate.png) 

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/dome9arc-tutorial/tutorial_general_400.png)

9. Op de **Dome9 boog configuratie** sectie, klikt u op **configureren Dome9 boog** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Configuratie van Dome9 boog](./media/dome9arc-tutorial/tutorial_dome9arc_configure.png) 

10. Meld u in een ander browservenster in uw bedrijf Dome9 boog site als beheerder.

11. Klik op de **profielinstellingen** in de rechterbovenhoek en vervolgens op **Accountinstellingen**. 

    ![Configuratie van Dome9 boog](./media/dome9arc-tutorial/configure1.png)

12. Navigeer naar **SSO** en klik vervolgens op **inschakelen**.

    ![Configuratie van Dome9 boog](./media/dome9arc-tutorial/configure2.png)

13. In de sectie configuratie van eenmalige aanmelding kunt u de volgende stappen uitvoeren:

    ![Configuratie van Dome9 boog](./media/dome9arc-tutorial/configure3.png)

    a. Voer de bedrijfsnaam in de **Account-ID** tekstvak. Deze waarde is moet worden gebruikt in het antwoord op een dat URL vermeld in de Azure portal-URL-sectie.

    b. In de **verlener** tekstvak, plak de waarde van **SAML entiteit-ID**, die u in Azure portal hebt gekopieerd.

    c. In de **Idp eindpunt-url** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u in Azure portal hebt gekopieerd.

    d. Uw gedownloade Base64-gecodeerd certificaat openen in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** tekstvak.

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/dome9arc-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/dome9arc-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/dome9arc-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/dome9arc-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-dome9-arc-test-user"></a>Maak een testgebruiker Dome9 boog

Om Azure AD-gebruikers zich aanmelden bij Dome9 boog, moeten ze worden ingericht in de toepassing. Dome9 boog biedt ondersteuning voor just-in-time inrichting maar voor die goed te laten werken, gebruiker moet selecteren bepaalde **rol** en dezelfde toe te wijzen aan de gebruiker.

   >[!Note]
   >Voor **rol** maken en andere details Neem contact op met [Dome9 boog Client ondersteuningsteam](https://dome9.com/about/contact-us/).

**Voor het inrichten van handmatig een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Dome9 boog site aan als beheerder.

2. Klik op de **gebruikers en rollen** en klik vervolgens op **gebruikers**.

    ![Werknemer toevoegen](./media/dome9arc-tutorial/user1.png)

3. Klik op **gebruiker toevoegen**.

    ![Werknemer toevoegen](./media/dome9arc-tutorial/user2.png)

4. In de **Create User** sectie, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/dome9arc-tutorial/user3.png)

    a. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    b. In de **voornaam** tekstvak, type de voornaam van de gebruiker, zoals Julia.

    c. In de **achternaam** tekstvak Achternaam van de gebruiker, zoals Simon type.

    d. Controleer **SSO gebruiker** als **op**.

    e. Klik op **maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Dome9 boog.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Dome9 boog, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Dome9 boog**.

    ![De koppeling Dome9 boog in de lijst met toepassingen](./media/dome9arc-tutorial/tutorial_dome9arc_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Dome9 boog in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Dome9 boog.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/dome9arc-tutorial/tutorial_general_01.png
[2]: ./media/dome9arc-tutorial/tutorial_general_02.png
[3]: ./media/dome9arc-tutorial/tutorial_general_03.png
[4]: ./media/dome9arc-tutorial/tutorial_general_04.png

[100]: ./media/dome9arc-tutorial/tutorial_general_100.png

[200]: ./media/dome9arc-tutorial/tutorial_general_200.png
[201]: ./media/dome9arc-tutorial/tutorial_general_201.png
[202]: ./media/dome9arc-tutorial/tutorial_general_202.png
[203]: ./media/dome9arc-tutorial/tutorial_general_203.png

