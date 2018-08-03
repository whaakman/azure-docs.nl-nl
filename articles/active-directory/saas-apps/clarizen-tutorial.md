---
title: 'Zelfstudie: Azure Active Directory-integratie met Clarizen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Clarizen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: jeedes
ms.openlocfilehash: 510bf383848725f3864c40af02c2b309370237f0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438083"
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Zelfstudie: Azure Active Directory-integratie met Clarizen

In deze zelfstudie leert u hoe u Azure Active Directory (Azure AD) integreren met Clarizen. Deze integratie kunt u de volgende voordelen:

- U kunt beheren, in Azure AD, wie toegang tot Clarizen heeft.
- U kunt uw gebruikers kunnen automatisch worden aangemeld bij Clarizen (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Het scenario in deze zelfstudie bestaat uit twee belangrijke taken:

1. Clarizen uit de galerie toevoegen.
1. Configureer en test Azure AD eenmalige aanmelding.

Als u meer informatie over software als een service (SaaS)-app-integratie met Azure AD wilt, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten
Voor het configureren van Azure AD-integratie met Clarizen, moet u de volgende items:

- Een Azure AD-abonnement
- Een Clarizen-abonnement dat ingeschakeld voor eenmalige aanmelding

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Azure AD eenmalige aanmelding testen in een testomgeving. Gebruik niet uw productieomgeving, tenzij dit noodzakelijk is.
- Als u een Azure AD-testomgeving geen hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-the-gallery"></a>Clarizen uit de galerie toevoegen
Voor het configureren van de integratie van Clarizen in Azure AD, Clarizen uit de galerie te toevoegt aan uw lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), in het linkerdeelvenster klikt u op de **Azure Active Directory** pictogram.

    ![Azure Active Directory-pictogram][1]

1. Klik op **bedrijfstoepassingen**. Klik vervolgens op **alle toepassingen**.

    ![Te klikken op 'Enterprise Application' en 'Alle Application'][2]

1. Klik op de **toevoegen** knop aan de bovenkant van het dialoogvenster.

    ![De knop 'Toevoegen'][3]

1. Typ in het zoekvak **Clarizen**.

    !['Clarizen' in het zoekvak te typen](./media/clarizen-tutorial/tutorial_clarizen_000.png)

1. Selecteer in het deelvenster met resultaten **Clarizen**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Clarizen selecteren in het deelvenster met resultaten](./media/clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen
In de volgende secties die u kunt configureren en Azure AD eenmalige aanmelding testen met Clarizen op basis van de testgebruiker Britta Simon.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Clarizen is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Clarizen tot stand worden gebracht. U deze relatie koppeling tot stand brengen door toe te wijzen de waarde van **gebruikersnaam** in Azure AD als de waarde van **gebruikersnaam** in Clarizen.

Als u wilt configureren en Azure AD eenmalige aanmelding met Clarizen testen, voert u de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  zodat uw gebruikers deze functie wilt gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Clarizen](#create-a-clarizen-test-user)**  een equivalent van Britta Simon in Clarizen die is gekoppeld aan de Azure AD-weergave van haar hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren
Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Clarizen.

1. In de Azure-portal op de **Clarizen** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Te klikken op "Single sign-on"][4]

1. In de **eenmalige aanmelding** in het dialoogvenster voor **modus**, selecteer **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    !['SAML gebaseerde aanmelding' selecteren](./media/clarizen-tutorial/tutorial_clarizen_01.png)

1. In de **Clarizen domein en URL's** sectie, voert u de volgende stappen uit:

    ![Selectievakjes voor id en antwoord-URL](./media/clarizen-tutorial/tutorial_clarizen_02.png)

    a. In de **id** typt u de waarde: **Clarizen**

    b. In de **antwoord-URL** typt u een URL met behulp van het volgende patroon: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Dit zijn niet de werkelijke waarden. U moet de werkelijke-id en antwoord-URL. Hier is het raadzaam dat u de unieke waarde van een tekenreeks als de id gebruikt. Als u de werkelijke waarden, neem contact op met de [Clarizen ondersteuningsteam](https://success.clarizen.com/hc/en-us/requests/new).

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **nieuw certificaat maken**.

    ![Te klikken op 'Nieuw certificaat maken'](./media/clarizen-tutorial/tutorial_clarizen_03.png)    

1. In de **nieuw certificaat maken** dialoogvenster vak, klikt u op het kalenderpictogram en selecteer een vervaldatum. Klik vervolgens op **Opslaan**.

    ![Selecteren en het opslaan van een vervaldatum](./media/clarizen-tutorial/tutorial_general_300.png)

1. In de **SAML-handtekeningcertificaat** sectie, selecteer **nieuw certificaat activeren**, en klik vervolgens op **opslaan**.

    ![Schakel het selectievakje voor het nieuwe certificaat actief te maken](./media/clarizen-tutorial/tutorial_clarizen_04.png)

1. In de **rollovercertificaat** in het dialoogvenster, klikt u op **OK**.

    ![Te klikken op "OK" om te bevestigen dat u wilt het certificaat te activeren](./media/clarizen-tutorial/tutorial_general_400.png)

1. In de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Te klikken op 'certificaat (Base64)"om het downloaden te starten](./media/clarizen-tutorial/tutorial_clarizen_05.png)

1. In de **Clarizen configuratie** sectie, klikt u op **configureren Clarizen** openen de **aanmelding configureren** venster.

    ![Te klikken op 'Clarizen configureren'](./media/clarizen-tutorial/tutorial_clarizen_06.png)

    !['Aanmelding configureren' venster, met inbegrip van bestanden en URL 's](./media/clarizen-tutorial/tutorial_clarizen_07.png)

1. In een ander browservenster aanmelden bij uw bedrijf Clarizen site als beheerder.

1. Klik op uw gebruikersnaam en klik vervolgens op **instellingen**.

    ![Te klikken op 'Instellingen' onder uw gebruikersnaam](./media/clarizen-tutorial/tutorial_clarizen_001.png "instellingen")

1. Klik op de **globale instellingen** tabblad. Klik naast **Federated Authentication**, klikt u op **bewerken**.

    ![Tabblad 'Algemene instellingen'](./media/clarizen-tutorial/tutorial_clarizen_002.png "globale instellingen")

1. In de **Federated Authentication** dialoogvenster vak, voer de volgende stappen uit:

    ![In het dialoogvenster 'Federatieve verificatie'](./media/clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication")

    a. Selecteer **inschakelen voor federatieve verificatie**.

    b. Klik op **uploaden** om uw gedownloade certificaat te uploaden.

    c. In de **aanmelden URL** voert u de waarde van **Single Sign-On Service URL voor SAML** van het Configuratievenster van Azure AD-toepassing.

    d. In de **afmelding URL** voert u de waarde van **afmelding URL** van het Configuratievenster van Azure AD-toepassing.

    e. Selecteer **gebruiken POST**.

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD
Maken in Azure portal, een testgebruiker Britta Simon genoemd.

![De naam en e-mailadres van de Azure AD-testgebruiker][100]

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** pictogram.

    ![Azure Active Directory-pictogram](./media/clarizen-tutorial/create_aaduser_01.png)

1. Klik op **gebruikers en groepen**, en klik vervolgens op **alle gebruikers** om de lijst met gebruikers weer te geven.

    ![Te klikken op 'Gebruikers en groepen' en 'Alle gebruikers'](./media/clarizen-tutorial/create_aaduser_02.png)

1. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** in het dialoogvenster.

    ![De knop 'Toevoegen'](./media/clarizen-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![In het dialoogvenster 'Gebruiker' met de naam, e-mailadres en wachtwoord wordt ingevuld](./media/clarizen-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van de Britta Simon-account.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van **wachtwoord**.

    d. Klik op **Create**.

### <a name="create-a-clarizen-test-user"></a>Maak een testgebruiker Clarizen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Clarizen. Clarizen ondersteunt automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](clarizen-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

**Als u moet de gebruiker handmatig hebt gemaakt, kunt voert u de volgende stappen:**

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Clarizen, moet u gebruikersaccounts inrichten. In het geval van Clarizen is inrichten een handmatige taak.

1. Meld u aan uw bedrijf Clarizen site als een beheerder.

1. Klik op **mensen**.

    ![Te klikken op 'People'](./media/clarizen-tutorial/create_aaduser_001.png "personen")

1. Klik op **gebruiker uitnodigen**.

    ![Knop 'Gebruiker uitnodigen'](./media/clarizen-tutorial/create_aaduser_002.png "gebruikers uitnodigen")

1. In de **personen uitnodigen** dialoogvenster vak, voer de volgende stappen uit:

    ![In het dialoogvenster 'Anderen uitnodigen'](./media/clarizen-tutorial/create_aaduser_003.png "personen uitnodigen")

    a. In de **e** typt u het e-mailadres van de Britta Simon-account.

    b. Klik op **uitnodigen**.

    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en gaat u als volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen
Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot Clarizen inschakelen.

![Toegewezen testgebruiker][200]

1. In de Azure portal, opent u de toepassingen weergeven, blader naar de directoryweergave, klikt u op **bedrijfstoepassingen**, en klik vervolgens op **alle toepassingen**.

    ![Te klikken op 'Enterprise Application' en 'Alle Application'][201]

1. Selecteer in de lijst met toepassingen, **Clarizen**.

    ![Clarizen selecteren in de lijst](./media/clarizen-tutorial/tutorial_clarizen_50.png)

1. Klik in het linkerdeelvenster op **gebruikers en groepen**.

    ![Te klikken op 'Gebruikers en groepen'][202]

1. Klik op de knop **Toevoegen**. Klik in de **toevoegen toewijzing** in het dialoogvenster, selecteer **gebruikers en groepen**.

    ![De knop 'Toevoegen' en het dialoogvenster 'Toewijzing toevoegen'][203]

1. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de lijst met gebruikers.

1. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

1. In de **toevoegen toewijzing** in het dialoogvenster, klikt u op de **toewijzen** knop.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen
Test uw Azure AD eenmalige aanmelding-configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Clarizen in het toegangsvenster, moet u worden automatisch aangemeld bij uw toepassing Clarizen.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](clarizen-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/clarizen-tutorial/tutorial_general_01.png
[2]: ./media/clarizen-tutorial/tutorial_general_02.png
[3]: ./media/clarizen-tutorial/tutorial_general_03.png
[4]: ./media/clarizen-tutorial/tutorial_general_04.png

[100]: ./media/clarizen-tutorial/tutorial_general_100.png

[200]: ./media/clarizen-tutorial/tutorial_general_200.png
[201]: ./media/clarizen-tutorial/tutorial_general_201.png
[202]: ./media/clarizen-tutorial/tutorial_general_202.png
[203]: ./media/clarizen-tutorial/tutorial_general_203.png