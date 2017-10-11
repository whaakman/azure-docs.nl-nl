---
title: 'Zelfstudie: Azure Active Directory-integratie met Clarizen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Clarizen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 574c6877bddac8be7d6d541bfabbdc10f6be3101
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Zelfstudie: Azure Active Directory-integratie met Clarizen

In deze zelfstudie leert u het integreren van Azure Active Directory (Azure AD) met Clarizen. Deze integratie biedt u de volgende voordelen:

- U kunt beheren, in Azure AD, die toegang tot Clarizen heeft.
- U kunt uw gebruikers automatisch worden aangemeld bij Clarizen (eenmalige aanmelding) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie, de Azure-portal beheren.

Het scenario in deze zelfstudie bestaat uit twee belangrijke taken:

1. Clarizen uit de galerie toevoegen.
2. Configureren en testen eenmalige aanmelding Azure AD.

Als u meer informatie over de software als een dienst (SaaS)-app-integratie met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten
Voor het configureren van Azure AD-integratie met Clarizen, moet u de volgende items:

- Een Azure AD-abonnement
- Een Clarizen-abonnement dat ingeschakeld voor eenmalige aanmelding

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Eenmalige aanmelding Azure AD testen in een testomgeving. Gebruik niet uw productieomgeving, tenzij dit noodzakelijk is.
- Als u een Azure AD-testomgeving geen hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-the-gallery"></a>Clarizen uit de galerie toevoegen
Voor het configureren van de integratie van Clarizen in Azure AD Clarizen uit de galerie te toevoegt aan de lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), in het linkerdeelvenster klikt u op de **Azure Active Directory** pictogram.

    ![Azure Active Directory-pictogram][1]

2. Klik op **bedrijfstoepassingen**. Klik vervolgens op **alle toepassingen**.

    ![Te klikken op 'Bedrijfstoepassingen' en 'Alle toepassingen'][2]

3. Klik op de **toevoegen** knop aan de bovenkant van het dialoogvenster.

    ![De knop "Toevoegen"][3]

4. Typ in het zoekvak **Clarizen**.

    !['Clarizen' in het zoekvak te typen](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. Selecteer in het deelvenster met resultaten **Clarizen**, en klik vervolgens op **toevoegen** de toepassing toevoegen.

    ![Clarizen selecteren in het deelvenster met resultaten](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In de volgende secties die u kunt configureren en testen eenmalige aanmelding Azure AD met Clarizen op basis van de testgebruiker Britta Simon.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Clarizen is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Clarizen tot stand worden gebracht. U de relatie van deze koppeling tot stand brengen door toe te wijzen de waarde van **gebruikersnaam** in Azure AD als de waarde van **gebruikersnaam** in Clarizen.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Clarizen, voert u de volgende elementen:

1. **[Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on)**  zodat uw gebruikers om deze functie te gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Clarizen](#create-a-clarizen-test-user)**  Clarizen die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren
Schakel Azure AD eenmalige aanmelding in de Azure portal en configureer eenmalige aanmelding in uw toepassing Clarizen.

1. In de Azure-portal op de **Clarizen** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Te klikken op "Single sign-on"][4]

2. In de **eenmalige aanmelding** in het dialoogvenster voor **modus**, selecteer **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    !['Op basis van SAML aanmelding' selecteren](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. In de **Clarizen domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Dialoogvensters voor id en de antwoord-URL](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    a. In de **id** typt u de waarde als: **Clarizen**

    b. In de **antwoord-URL** typt u een URL met behulp van het volgende patroon volgen: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Deze zijn niet de werkelijke waarden. U moet de werkelijke id gebruiken en antwoord-URL. Hier is het raadzaam dat u de unieke waarde van een tekenreeks als de id gebruiken. Als u de werkelijke waarden, neem contact op met de [Clarizen ondersteuningsteam](https://success.clarizen.com/hc/en-us/requests/new).

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **nieuw certificaat maken**.

    ![Te klikken op 'Nieuw certificaat maken'](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. In de **nieuw certificaat maken** dialoogvenster, klikt u op het pictogram Agenda en kiest u een vervaldatum. Klik vervolgens op **Opslaan**.

    ![Selecteren en een vervaldatum opslaan](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. In de **SAML-certificaat voor ondertekening van** sectie **nieuwe certificaat activeren**, en klik vervolgens op **opslaan**.

    ![Als u het selectievakje voor het nieuwe certificaat actief te maken](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. In de **rollovercertificaat** in het dialoogvenster, klikt u op **OK**.

    ![Te klikken op 'OK' om te bevestigen dat u wilt activeren van het certificaat](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. In de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Te klikken op 'Certificate (Base64)"om het downloaden te starten](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. In de **Clarizen configuratie** sectie, klikt u op **configureren Clarizen** openen de **eenmalige aanmelding configureren** venster.

    ![Te klikken op 'Clarizen configureren'](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    !['Configureren aanmelding' venster, inclusief URL's en bestanden](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. In een ander browservenster aanmelden bij uw bedrijf Clarizen site als beheerder.

11. Klik op uw gebruikersnaam en klik vervolgens op **instellingen**.

    ![Te klikken op 'Instellingen' onder uw gebruikersnaam](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "instellingen")

12. Klik op de **globale instellingen** tabblad. Klik naast **Federated Authentication**, klikt u op **bewerken**.

    ![Tabblad 'Globale instellingen'](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globale instellingen")

13. In de **Federated Authentication** dialoogvenster vak, voert u de volgende stappen uit:

    ![In het dialoogvenster 'Federated Authentication'](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication")

    a. Selecteer **inschakelen voor federatieve verificatie**.

    b. Klik op **uploaden** om uw gedownloade certificaat te uploaden.

    c. In de **aanmelden URL** en voer de waarde van **SAML Single Sign-On Service-URL** van het Configuratievenster van Azure AD-toepassing.

    d. In de **Sign-out URL** en voer de waarde van **Sign-Out URL** van het Configuratievenster van Azure AD-toepassing.

    e. Selecteer **POST gebruiken**.

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Maak een testgebruiker Britta Simon aangeroepen in de Azure portal.

![Naam en e-mailadres van de testgebruiker Azure AD][100]

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** pictogram.

    ![Azure Active Directory-pictogram](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. Klik op **gebruikers en groepen**, en klik vervolgens op **alle gebruikers** om de lijst met gebruikers weer te geven.

    ![Te klikken op 'Gebruikers en groepen' en 'Alle gebruikers'](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. Klik boven in het dialoogvenster op **toevoegen** openen de **gebruiker** in het dialoogvenster.

    ![De knop "Toevoegen"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![In het dialoogvenster met de naam, e-mailadres en wachtwoord 'Gebruiker' ingevuld](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van het account Britta Simon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van **wachtwoord**.

    d. Klik op **Create**.

### <a name="create-a-clarizen-test-user"></a>Een testgebruiker Clarizen maken
Om Azure AD-gebruikers aan te melden bij Clarizen, moet u gebruikersaccounts inrichten. In het geval van Clarizen is inrichting een handmatige taak.

1. Meld u aan bij uw bedrijf Clarizen site als beheerder.

2. Klik op **mensen**.

    ![Te klikken op 'Mensen'](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "personen")

3. Klik op **gebruiker uitnodigen**.

    ![Knop 'Gebruiker uitnodigen'](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "gebruikers uitnodigen")

4. In de **personen uitnodigen** dialoogvenster vak, voert u de volgende stappen uit:

    ![Dialoogvenster 'Personen uitnodigen'](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "personen uitnodigen")

    a. In de **e** typt u het e-mailadres van het account Britta Simon.

    b. Klik op **uitnodigen**.

    > [!NOTE]
    > De accounthouder Azure Active Directory wordt een e-mailbericht ontvangen en Ga als volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen
Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan Clarizen inschakelen.

![Toegewezen testgebruiker][200]

1. In de Azure portal, opent u de toepassingen weergeven, blader naar de directoryweergave, klikt u op **bedrijfstoepassingen**, en klik vervolgens op **alle toepassingen**.

    ![Te klikken op 'Bedrijfstoepassingen' en 'Alle toepassingen'][201]

2. Selecteer in de lijst met toepassingen **Clarizen**.

    ![Clarizen selecteren in de lijst](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. Klik in het linkerdeelvenster op **gebruikers en groepen**.

    ![Te klikken op 'Gebruikers en groepen'][202]

4. Klik op de **toevoegen** knop. Klik in de **toevoegen toewijzing** dialoogvenster, **gebruikers en groepen**.

    ![De knop "Toevoegen" en het dialoogvenster 'Toewijzing toevoegen'][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de lijst met gebruikers.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

7. In de **toevoegen toewijzing** in het dialoogvenster, klikt u op de **toewijzen** knop.

### <a name="test-single-sign-on"></a>Test eenmalige aanmelding
Uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Als u op de tegel Clarizen in het deelvenster toegang, moet u worden automatisch aangemeld bij uw toepassing Clarizen.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
