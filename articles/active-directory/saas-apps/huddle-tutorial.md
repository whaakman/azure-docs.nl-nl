---
title: 'Zelfstudie: Azure Active Directory-integratie met kruipen dicht | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en kruipen dicht.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: 050a5a210ab10a557b149513a0416b188f4618ba
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885900"
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Zelfstudie: Azure Active Directory-integratie met kruipen dicht

In deze zelfstudie leert u hoe u kruipen dicht integreert met Azure Active Directory (Azure AD).

Kruipen dicht integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot kruipen dicht heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij kruipen dicht (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met kruipen dicht, moet u de volgende items:

- Een Azure AD-abonnement
- Een kruipen dicht eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen kruipen dicht in de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-huddle-from-the-gallery"></a>Toe te voegen kruipen dicht in de galerie

Voor het configureren van de integratie van kruipen dicht bij Azure AD, moet u kruipen dicht in de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen kruipen dicht in de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **kruipen dicht**. Selecteer **kruipen dicht** vanuit het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/huddle-tutorial/tutorial_huddle_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met kruipen dicht op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in kruipen dicht is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in kruipen dicht tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met kruipen dicht, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker kruipen dicht](#creating-a-huddle-test-user)**  : als u wilt een equivalent van Britta Simon in kruipen dicht die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing kruipen dicht.

**Voor het configureren van Azure AD eenmalige aanmelding met kruipen dicht, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **kruipen dicht** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

    ![Eenmalige aanmelding configureren](./media/huddle-tutorial/tutorial_general_300.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/huddle-tutorial/tutorial_general_301.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/huddle-tutorial/tutorial_general_302.png)

5. Op de **SAML-basisconfiguratie** sectie de volgende stappen uitvoeren als u wilt configureren van de toepassing in **IDP** intiated modus:

    > [!NOTE]
    > Uw exemplaar kruipen dicht worden automatisch gedetecteerd in het domein dat u hieronder.

    ![Kruipen dicht domein en URL's één aanmeldings-informatie](./media/huddle-tutorial/tutorial_huddle_url.png)

    a. In de **id** tekstvak, een URL typen:

    | | |
    |--|--|
    | `https://login.huddle.net`|
    | `https://login.huddle.com`|
    | |

    b. In de **antwoord-URL** tekstvak, een URL typen:

    | | |
    |--|--|
    | `https://login.huddle.net/saml/browser-sso`|
    | `https://login.huddle.com/saml/browser-sso`|
    | `https://login.huddle.com/saml/idp-initiated-sso`|
    | |

    c. Klik op **extra URL's instellen** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Kruipen dicht domein en URL's één aanmeldings-informatie](./media/huddle-tutorial/tutorial_huddle_url1.png)

    In de **aanmeldings-URL** tekstvak typt u een van de URL met behulp van het volgende patroon:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.com`|
    | `https://us.huddle.com`|
    | |

    > [!NOTE]
    > De aanmeldings-URL-waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [kruipen dicht Client ondersteuningsteam](https://huddle.zendesk.com) deze waarde op te halen.

6. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van het juiste certificaat volgens uw vereiste en sla deze op uw computer.

    ![Eenmalige aanmelding configureren](./media/huddle-tutorial/tutorial_huddle_certificate.png)

7. Op de **instellen kruipen dicht** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Eenmalige aanmelding configureren](./media/huddle-tutorial/tutorial_huddle_configure.png)

8. Het configureren van eenmalige aanmelding op **kruipen dicht** zijde, moet u voor het verzenden van het certificaat dat u hebt gedownload en de URL's die u hebt gekopieerd uit **instellen** **kruipen dicht** sectie vanuit Azure portal om te [kruipen dicht Client ondersteuningsteam](https://huddle.zendesk.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

    >[!NOTE]
    > Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam kruipen dicht. U ontvangt een melding wanneer de configuratie is voltooid.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](./media/huddle-tutorial/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](./media/huddle-tutorial/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-huddle-test-user"></a>Het maken van een testgebruiker kruipen dicht

Als u wilt dat Azure AD-gebruikers zich aanmelden bij kruipen dicht, moeten ze worden ingericht voor het kruipen dicht. In het geval van kruipen dicht is inrichten een handmatige taak.

**Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **kruipen dicht** bedrijf site als administrator.

2. Klik op **werkruimte**.

3. Klik op **mensen \> anderen uitnodigen**.

    ![Mensen](./media/huddle-tutorial/IC787838.png "personen")

4. In de **maken van een uitnodiging voor een nieuwe** sectie, voert u de volgende stappen uit:
  
    ![Nieuwe uitnodiging](./media/huddle-tutorial/IC787839.png "nieuwe uitnodiging")
  
    a. In de **kiest u een team dat anderen uitnodigen** in de lijst met **team**.

    b. Type de **e-mailadres** van een geldige Azure AD-account die u inrichten wilt **Enter e-mailadres voor mensen die u wilt uitnodigen** tekstvak.

    c. Klik op **uitnodigen**.

    >[!NOTE]
    > De houder van Azure AD-account ontvangt een e-mailbericht ook een koppeling voor het account te bevestigen voordat deze geactiveerd wordt.

>[!NOTE]
>U kunt een andere kruipen dicht gebruiker-account maken-hulpprogramma's of API's die worden geleverd door kruipen dicht gebruiken voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan kruipen dicht.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **kruipen dicht**.

    ![Eenmalige aanmelding configureren](./media/huddle-tutorial/tutorial_huddle_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel kruipen dicht in het toegangsvenster, moet u de aanmeldingspagina van kruipen dicht toepassing automatisch beschikt.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/huddle-tutorial/tutorial_general_01.png
[2]: ./media/huddle-tutorial/tutorial_general_02.png
[3]: ./media/huddle-tutorial/tutorial_general_03.png
[4]: ./media/huddle-tutorial/tutorial_general_04.png
[100]: ./media/huddle-tutorial/tutorial_general_100.png
[200]: ./media/huddle-tutorial/tutorial_general_200.png
[201]: ./media/huddle-tutorial/tutorial_general_201.png
[202]: ./media/huddle-tutorial/tutorial_general_202.png
[203]: ./media/huddle-tutorial/tutorial_general_203.png
