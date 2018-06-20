---
title: 'Zelfstudie: Azure Active Directory-integratie met koppel | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en koppel.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7b2c3ac5-17f1-49a0-8961-c541b258d4b1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: 633828cc7e244dca1e43c45852910211aeda68d9
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229492"
---
# <a name="tutorial-azure-active-directory-integration-with-flock"></a>Zelfstudie: Azure Active Directory-integratie met koppel

In deze zelfstudie leert u hoe koppel integreren met Azure Active Directory (Azure AD).

Integreren Koppel met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot koppel heeft.
- U kunt uw gebruikers automatisch ophalen aangemelde koppel (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met koppel, moet u de volgende items:

- Een Azure AD-abonnement
- Een koppel eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen koppel uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-flock-from-the-gallery"></a>Toe te voegen koppel uit de galerie
Voor het configureren van de integratie van Koppel met Azure AD, moet u koppel uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen koppel uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **koppel**, selecteer **koppel** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Koppel in de lijst met resultaten](./media/flock-tutorial/tutorial_flock_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Koppel op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in koppel is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in koppel tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met koppel, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker koppel](#create-a-flock-test-user)**  - koppel die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing koppel.

**Voor het configureren van Azure AD eenmalige aanmelding met koppel, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **koppel** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/flock-tutorial/tutorial_flock_samlbase.png)

3. Op de **koppel domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Koppel domein en de URL's van eenmalige aanmelding informatie](./media/flock-tutorial/tutorial_flock_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.flock.com/`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.flock.com/`

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [koppel Client ondersteuningsteam](mailto:support@flock.com) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/flock-tutorial/tutorial_flock_certificate.png)

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/flock-tutorial/tutorial_general_400.png)

6. Op de **koppel configuratie** sectie, klikt u op **koppel configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Koppel-configuratie](./media/flock-tutorial/tutorial_flock_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf koppel site als beheerder.

8. Selecteer **verificatie** tabblad van het navigatievenster van de linkerkant en selecteer vervolgens **SAML-verificatie**.

    ![Koppel-configuratie](./media/flock-tutorial/configure1.png)

9. In de **SAML-verificatie** sectie, voert u de volgende stappen uit:

    ![Koppel-configuratie](./media/flock-tutorial/configure2.png)

    a. In de **SAML 2.0 Endpoint(HTTP)** textbox plakken **SAML Single Sign-On Service-URL** waarde die u hebt gekopieerd uit de Azure portal.

    b. In de **identiteit Provider verlener** textbox plakken **SAML entiteit-ID** waarde die u hebt gekopieerd uit de Azure portal.

    c. Open de gedownloade **Certificate(Base64)** vanuit Azure-portal in Kladblok, plak de inhoud in de **openbaar certificaat** textbox.

    d. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/flock-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/flock-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/flock-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/flock-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-flock-test-user"></a>Een testgebruiker koppel maken

Om Azure AD-gebruikers zich aanmelden bij koppel, moeten ze worden ingericht in koppel. Inrichting is een handmatige taak in het geval van koppel toe.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf koppel site als beheerder.

2. Klik op **Team beheren** in het linkernavigatievenster.

    ![Werknemer toevoegen](./media/flock-tutorial/user1.png)

3. Klik op **lid toevoegen** tabblad en selecteer vervolgens **teamleden**.

    ![Werknemer toevoegen](./media/flock-tutorial/user2.png)

4. Voer het e-mailadres van de gebruiker zoals **Brittasimon@contoso.com** en selecteer vervolgens **gebruikers toevoegen**.

    ![Werknemer toevoegen](./media/flock-tutorial/user3.png)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verleent koppel.

![Toewijzen van de gebruikersrol][200]

**Als u wilt toewijzen Britta Simon koppel, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **koppel**.

    ![De koppeling Koppel in de lijst met toepassingen](./media/flock-tutorial/tutorial_flock_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Koppel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing koppel.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/flock-tutorial/tutorial_general_01.png
[2]: ./media/flock-tutorial/tutorial_general_02.png
[3]: ./media/flock-tutorial/tutorial_general_03.png
[4]: ./media/flock-tutorial/tutorial_general_04.png

[100]: ./media/flock-tutorial/tutorial_general_100.png

[200]: ./media/flock-tutorial/tutorial_general_200.png
[201]: ./media/flock-tutorial/tutorial_general_201.png
[202]: ./media/flock-tutorial/tutorial_general_202.png
[203]: ./media/flock-tutorial/tutorial_general_203.png
