---
title: 'Zelfstudie: Azure Active Directory-integratie met Proxyclick | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Proxyclick.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5c58a859-71c2-4542-ae92-e5f16a8e7f18
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: jeedes
ms.openlocfilehash: 6e0d8c5c178afc63c9c177d31d0a14104b127941
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36213335"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Zelfstudie: Azure Active Directory-integratie met Proxyclick

In deze zelfstudie leert u hoe Proxyclick integreren met Azure Active Directory (Azure AD).

Proxyclick integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Proxyclick heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Proxyclick (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Proxyclick, moet u de volgende items:

- Een Azure AD-abonnement
- Een Proxyclick eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Proxyclick uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-proxyclick-from-the-gallery"></a>Proxyclick uit de galerie toevoegen
Voor het configureren van de integratie van Proxyclick in Azure AD, moet u Proxyclick uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Proxyclick uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Proxyclick**, selecteer **Proxyclick** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Proxyclick in de lijst met resultaten](./media/proxyclick-tutorial/tutorial_proxyclick_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Proxyclick op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Proxyclick is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Proxyclick tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Proxyclick, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Proxyclick](#create-a-proxyclick-test-user)**  - Proxyclick die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Proxyclick configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Proxyclick, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Proxyclick** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/proxyclick-tutorial/tutorial_proxyclick_samlbase.png)

3. Op de **Proxyclick domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Proxyclick domein eenmalige aanmelding informatie](./media/proxyclick-tutorial/tutorial_proxyclick_url2.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://saml.proxyclick.com/init/<companyId>`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://saml.proxyclick.com/consume/<companyId>`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Proxyclick domein eenmalige aanmelding informatie](./media/proxyclick-tutorial/tutorial_proxyclick_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://saml.proxyclick.com/init/<companyId>`

    > [!NOTE]
    > Deze waarden zijn niet echt. U kunt deze waarden wordt bijgewerkt met de werkelijke id, antwoord-URL en aanmeldings-URL, die verderop in de zelfstudie wordt beschreven.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/proxyclick-tutorial/tutorial_proxyclick_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/proxyclick-tutorial/tutorial_general_400.png)

7. Op de **Proxyclick configuratie** sectie, klikt u op **configureren Proxyclick** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Proxyclick configuratie](./media/proxyclick-tutorial/tutorial_proxyclick_configure.png)

8. In een ander browservenster, meld u aan bij uw bedrijf Proxyclick site als beheerder.

9. Selecteer **& Accountinstellingen**.

    ![Proxyclick configuratie](./media/proxyclick-tutorial/configure1.png)

10. Schuif omlaag naar de **INTEGRATIES** en selecteer **SAML**.

    ![Proxyclick configuratie](./media/proxyclick-tutorial/configure2.png)

11. In de **SAML** sectie, voert u de volgende stappen uit:

    ![Proxyclick configuratie](./media/proxyclick-tutorial/configure3.png)

    a. Kopiëren **SAML Consumer URL** waarde en plak deze in **antwoord-URL** textbox in **Proxyclick domein en de URL's** sectie op Azure-portal.

    b. Kopiëren **Omleidings-URL van SAML SSO** waarde en plak deze in **aanmelden URL** en **id** tekstvakken in **Proxyclick domein en de URL's** sectie in Azure portal.

    c. Selecteer **SAML-aanvraagmethode** als **HTTP-omleiding**.

    d. In de **verlener** textbox, plak de waarde van **SAML entiteit-ID** waarde, die u hebt gekopieerd vanuit Azure-portal.

    e. In de **eindpunt-URL van SAML 2.0** textbox, plak de waarde van **SAML Single Sign-On Service-URL** gekopieerd vanuit Azure-portal.

    f. Het gedownloade certificaatbestand openen vanuit Azure-portal in Kladblok en plak deze in de **certificaat** textbox.

    g. Klik op **wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/proxyclick-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/proxyclick-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/proxyclick-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/proxyclick-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-proxyclick-test-user"></a>Een testgebruiker Proxyclick maken

Om Azure AD-gebruikers zich aanmelden bij Proxyclick, moeten ze worden ingericht in Proxyclick. In het geval van Proxyclick is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Proxyclick site als beheerder.

2. Klik op **collega's** van de bovenste navigatiebalk.

    ![Werknemer toevoegen](./media/proxyclick-tutorial/user1.png)

3. Klik op **collega toevoegen**

    ![Werknemer toevoegen](./media/proxyclick-tutorial/user2.png)

4. In de **toevoegen van een collega** sectie, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/proxyclick-tutorial/user3.png)

    a. In de **e** textbox, typ het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    b. In de **voornaam** textbox, de eerste naam van gebruiker zoals Britta.

    c. In de **achternaam** textbox, typt u de achternaam van de gebruiker zoals Simon.

    d. Klik op **gebruiker toevoegen**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Proxyclick.

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen Proxyclick, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Proxyclick**.

    ![De koppeling Proxyclick in de lijst met toepassingen](./media/proxyclick-tutorial/tutorial_proxyclick_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Proxyclick in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Proxyclick.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/proxyclick-tutorial/tutorial_general_01.png
[2]: ./media/proxyclick-tutorial/tutorial_general_02.png
[3]: ./media/proxyclick-tutorial/tutorial_general_03.png
[4]: ./media/proxyclick-tutorial/tutorial_general_04.png

[100]: ./media/proxyclick-tutorial/tutorial_general_100.png

[200]: ./media/proxyclick-tutorial/tutorial_general_200.png
[201]: ./media/proxyclick-tutorial/tutorial_general_201.png
[202]: ./media/proxyclick-tutorial/tutorial_general_202.png
[203]: ./media/proxyclick-tutorial/tutorial_general_203.png

