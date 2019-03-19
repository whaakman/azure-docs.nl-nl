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
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5ce628716e9e8d0094f678c3d67dcfad9e24ada
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57860530"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Zelfstudie: Azure Active Directory-integratie met Proxyclick

In deze zelfstudie leert u hoe u Proxyclick integreren met Azure Active Directory (Azure AD).

Proxyclick integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Proxyclick heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Proxyclick (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Proxyclick, moet u de volgende items:

- Een Azure AD-abonnement
- Een Proxyclick eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Proxyclick uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-proxyclick-from-the-gallery"></a>Proxyclick uit de galerie toe te voegen
Voor het configureren van de integratie van Proxyclick in Azure AD, moet u Proxyclick uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Proxyclick uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Proxyclick**, selecteer **Proxyclick** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Proxyclick in de lijst met resultaten](./media/proxyclick-tutorial/tutorial_proxyclick_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Proxyclick op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Proxyclick is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Proxyclick tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Proxyclick, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Proxyclick](#create-a-proxyclick-test-user)**  : als u wilt een equivalent van Britta Simon in Proxyclick die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Proxyclick.

**Voor het configureren van Azure AD eenmalige aanmelding met Proxyclick, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Proxyclick** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/proxyclick-tutorial/tutorial_proxyclick_samlbase.png)

1. Op de **Proxyclick domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Proxyclick domein en URL's, eenmalige aanmelding informatie](./media/proxyclick-tutorial/tutorial_proxyclick_url2.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://saml.proxyclick.com/init/<companyId>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://saml.proxyclick.com/consume/<companyId>`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Proxyclick domein en URL's, eenmalige aanmelding informatie](./media/proxyclick-tutorial/tutorial_proxyclick_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://saml.proxyclick.com/init/<companyId>`

    > [!NOTE]
    > Dit zijn geen echte waarden. U kunt deze waarden wordt bijgewerkt met de werkelijke-id, de antwoord-URL en aanmeldings-URL, die later in de zelfstudie wordt uitgelegd.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/proxyclick-tutorial/tutorial_proxyclick_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/proxyclick-tutorial/tutorial_general_400.png)

1. Op de **Proxyclick configuratie** sectie, klikt u op **configureren Proxyclick** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Proxyclick configuratie](./media/proxyclick-tutorial/tutorial_proxyclick_configure.png)

1. In een ander browservenster aanmelden bij uw bedrijf Proxyclick site als beheerder.

1. Selecteer **& Accountinstellingen**.

    ![Proxyclick configuratie](./media/proxyclick-tutorial/configure1.png)

1. Schuif omlaag naar de **INTEGRATIES** en selecteer **SAML**.

    ![Proxyclick configuratie](./media/proxyclick-tutorial/configure2.png)

1. In de **SAML** sectie, voert u de volgende stappen uit:

    ![Proxyclick configuratie](./media/proxyclick-tutorial/configure3.png)

    a. Kopie **URL voor SAML-consument** waarde en plak deze in **antwoord-URL** -tekstvak in **Proxyclick domein en URL's** sectie in Azure portal.

    b. Kopie **Omleidings-URL voor SAML SSO-** waarde en plak deze in **aanmeldings-URL** en **id** tekstvakken in **Proxyclick domein en URL's** sectie in Azure portal.

    c. Selecteer **SAML-aanvraagmethode** als **HTTP-omleiding**.

    d. In de **verlener** tekstvak, plak de waarde van **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    e. In de **eindpunt-URL voor SAML 2.0** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** gekopieerd vanuit Azure portal.

    f. Open het gedownloade certificaat-bestand vanuit Azure portal in Kladblok en plak deze in de **certificaat** tekstvak.

    g. Klik op **Wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/proxyclick-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/proxyclick-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/proxyclick-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/proxyclick-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-proxyclick-test-user"></a>Maak een testgebruiker Proxyclick

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Proxyclick, moeten ze worden ingericht voor Proxyclick. In het geval van Proxyclick is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw bedrijf Proxyclick site aan als beheerder.

1. Klik op **collega's** in de bovenste navigatiebalk.

    ![Werknemer toevoegen](./media/proxyclick-tutorial/user1.png)

1. Klik op **collega toevoegen**

    ![Werknemer toevoegen](./media/proxyclick-tutorial/user2.png)

1. In de **toevoegen van een collega** sectie, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/proxyclick-tutorial/user3.png)

    a. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals **brittasimon\@contoso.com**.

    b. Typ in het tekstvak **Voornaam** de voornaam van de gebruiker, zoals Britta.

    c. Typ in het tekstvak **Achternaam** de achternaam van de gebruiker, zoals Simon.

    d. Klik op **Add User**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Proxyclick.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan Proxyclick toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Proxyclick**.

    ![De koppeling Proxyclick in de lijst met toepassingen](./media/proxyclick-tutorial/tutorial_proxyclick_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Proxyclick in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Proxyclick.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



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

