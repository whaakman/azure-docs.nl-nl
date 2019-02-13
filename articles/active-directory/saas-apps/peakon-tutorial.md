---
title: 'Zelfstudie: Azure Active Directory-integratie met Peakon | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Peakon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a944c397-ed3f-4d45-b9b2-6d4bcb6b0a09
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e32f0ac6fcc6839bad973c3d014436d08ae43896
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56211305"
---
# <a name="tutorial-azure-active-directory-integration-with-peakon"></a>Zelfstudie: Azure Active Directory-integratie met Peakon

In deze zelfstudie leert u hoe u Peakon integreren met Azure Active Directory (Azure AD).

Peakon integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Peakon heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Peakon (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Peakon, moet u de volgende items:

- Een Azure AD-abonnement
- Een Peakon eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik geen productie-omgeving, als dat nodig is.
- Als u niet beschikt over een evaluatieomgeving in Azure AD, kunt u [een gratis proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Peakon uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-peakon-from-the-gallery"></a>Peakon uit de galerie toe te voegen

Voor het configureren van de integratie van Peakon in Azure AD, moet u Peakon uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Peakon uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Peakon**, selecteer **Peakon** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Peakon in de lijst met resultaten](./media/peakon-tutorial/tutorial_peakon_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Peakon op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Peakon is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Peakon tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Peakon, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Peakon](#creating-a-peakon-test-user)**  : als u wilt een equivalent van Britta Simon in Peakon die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Peakon.

**Voor het configureren van Azure AD eenmalige aanmelding met Peakon, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Peakon** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. Voer in het gedeelte **Standaard SAML-configuratie** de volgende stappen uit als u de toepassing in de door **IDP geïnitieerde** modus wilt configureren:

    ![Peakon domein en URL's, eenmalige aanmelding informatie](./media/peakon-tutorial/tutorial_peakon_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://app.peakon.com/saml/<companyid>/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://app.peakon.com/saml/<companyid>/assert`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Peakon domein en URL's, eenmalige aanmelding informatie](./media/peakon-tutorial/tutorial_peakon_url1.png)

    Typ een URL in het tekstvak **Aanmeldings-URL**: `https://app.peakon.com/login`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke-id en antwoord-URL die is latert uitgelegd in de zelfstudie.

6. In de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Raw)** en sla het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/peakon-tutorial/tutorial_peakon_certificate.png) 

7. Op de **Peakon instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![Peakon configuratie](common/configuresection.png)

8. In een ander browservenster aanmelden bij Peakon als beheerder.

9. In de menubalk aan de linkerkant van de pagina, klikt u op **configuratie**, navigeert u vervolgens naar **integraties**.

    ![De configuratie](./media/peakon-tutorial/tutorial_peakon_config.png)

10. Op **integraties** pagina, klikt u op **Single Sign-On**.

    ![Eén](./media/peakon-tutorial/tutorial_peakon_single.png)

11. Onder **Single Sign-On** sectie, klikt u op **inschakelen**.

    ![De inschakelen](./media/peakon-tutorial/tutorial_peakon_enable.png)

12. Op de **eenmalige aanmelding voor werknemers die gebruikmaken van SAML** sectie, voert u de volgende stappen uit:

    ![De saml](./media/peakon-tutorial/tutorial_peakon_saml.png)

    a. In de **aanmeldings-URL voor eenmalige aanmelding** tekstvak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    b. In de **afmeldings-URL voor eenmalige aanmelding** tekstvak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **bestand kiezen** voor het uploaden van het certificaat dat u hebt gedownload vanuit Azure portal, in het certificaat.

    d. Klik op de **pictogram** om te kopiëren de **entiteit-ID** en plak deze in **id** -tekstvak in **SAML-basisconfiguratie** sectie in Azure portal.

    e. Klik op de **pictogram** kopiëren de **antwoord-URL (ACS)** en plak deze in **antwoord-URL** -tekstvak in **SAML-basisconfiguratie**   sectie in Azure portal.

    f. Klik op **Opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-peakon-test-user"></a>Het maken van een testgebruiker Peakon

Voor het inschakelen van Azure AD-gebruikers zich aanmelden bij Peakon, moeten ze worden ingericht voor Peakon.  
In het geval van Peakon is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan uw bedrijf Peakon site als een beheerder.

2. In de menubalk aan de linkerkant van de pagina, klikt u op **configuratie**, navigeert u vervolgens naar **werknemers**.

    ![De werknemer](./media/peakon-tutorial/tutorial_peakon_employee.png)

3. Op boven aan de rechterkant van de pagina, klikt u op **toevoegen werknemer**.

      ![De werknemer toevoegen](./media/peakon-tutorial/tutorial_peakon_addemployee.png)

3. Op de **nieuwe werknemer** dialoogvenster pagina, voert u de volgende stappen uit:

     ![De nieuwe werknemer](./media/peakon-tutorial/tutorial_peakon_create.png)

    a. In de **naam** tekstvak, type voornaam als **Julia** en de achternaam als **simon**.

    b. In de **e** tekstvak, typ het e-mailadres zoals **Brittasimon@contoso.com**.

    c. Klik op **maken werknemer**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Peakon.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Peakon**.

    ![Eenmalige aanmelding configureren](./media/peakon-tutorial/tutorial_peakon_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. In de **toevoegen toewijzing** dialoogvenster, selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Peakon in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Peakon.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
