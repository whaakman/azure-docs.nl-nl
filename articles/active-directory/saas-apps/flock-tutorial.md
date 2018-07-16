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
ms.openlocfilehash: f0bb2b336927b4279403bf87d44a9a5ef1539607
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053101"
---
# <a name="tutorial-azure-active-directory-integration-with-flock"></a>Zelfstudie: Azure Active Directory-integratie met koppel

In deze zelfstudie leert u hoe koppel integreren met Azure Active Directory (Azure AD).

Integratie Koppel met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot koppel heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij koppel (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met koppel, moet u de volgende items:

- Een Azure AD-abonnement
- Een koppel eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen koppel uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-flock-from-the-gallery"></a>Toe te voegen koppel uit de galerie
Voor het configureren van de integratie van Koppel in Azure AD, moet u koppel uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen koppel uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **koppel**, selecteer **koppel** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Koppel in de lijst met resultaten](./media/flock-tutorial/tutorial_flock_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Koppel op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent van koppel is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in koppel tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met koppel, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker koppel](#create-a-flock-test-user)**  : als u wilt een equivalent van Britta Simon in koppel die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing koppel.

**Voor het configureren van Azure AD eenmalige aanmelding met koppel, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **koppel** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/flock-tutorial/tutorial_flock_samlbase.png)

3. Op de **koppel domein en URL's** sectie, voert u de volgende stappen uit:

    ![Koppel domein en URL's, eenmalige aanmelding informatie](./media/flock-tutorial/tutorial_flock_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.flock.com/`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.flock.com/`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [koppel Client ondersteuningsteam](mailto:support@flock.com) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/flock-tutorial/tutorial_flock_certificate.png)

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/flock-tutorial/tutorial_general_400.png)

6. Op de **koppel configuratie** sectie, klikt u op **koppel configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Koppel-configuratie](./media/flock-tutorial/tutorial_flock_configure.png) 

7. In een ander browservenster aanmelden bij uw bedrijf koppel site als beheerder.

8. Selecteer **verificatie** tabblad in het linkernavigatievenster en selecteer vervolgens **SAML-verificatie**.

    ![Koppel-configuratie](./media/flock-tutorial/configure1.png)

9. In de **SAML-verificatie** sectie, voert u de volgende stappen uit:

    ![Koppel-configuratie](./media/flock-tutorial/configure2.png)

    a. In de **SAML 2.0 Endpoint(HTTP)** tekstvak plakken **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure portal.

    b. In de **Identity Provider Issuer** tekstvak plakken **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    c. Open het gedownloade **Certificate(Base64)** vanuit Azure portal in Kladblok, plak de inhoud in de **openbaar certificaat** tekstvak.

    d. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/flock-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/flock-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/flock-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/flock-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-flock-test-user"></a>Maak een testgebruiker koppel

Als u wilt dat gebruikers zich aanmelden bij koppel Azure AD, moeten ze worden ingericht voor koppel. In het geval van koppel is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf koppel site aan als beheerder.

2. Klik op **Team beheren** in het linkernavigatievenster.

    ![Werknemer toevoegen](./media/flock-tutorial/user1.png)

3. Klik op **Add Member** tabblad en selecteer vervolgens **teamleden**.

    ![Werknemer toevoegen](./media/flock-tutorial/user2.png)

4. Voer het e-mailadres van de gebruiker, zoals **Brittasimon@contoso.com** en selecteer vervolgens **gebruikers toevoegen**.

    ![Werknemer toevoegen](./media/flock-tutorial/user3.png)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen koppel.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon koppel, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **koppel**.

    ![De koppeling Koppel in de lijst met toepassingen](./media/flock-tutorial/tutorial_flock_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Koppel in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing koppel.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

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
