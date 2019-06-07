---
title: 'Zelfstudie: Azure Active Directory-integratie met Druva | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Druva.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdf359f38bd7032b4de48a17b91ba1c10b165c91
ms.sourcegitcommit: f9448a4d87226362a02b14d88290ad6b1aea9d82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66807786"
---
# <a name="tutorial-integrate-druva-with-azure-active-directory"></a>Zelfstudie: Druva integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Druva integreert met Azure Active Directory (Azure AD). Wanneer u Druva met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot druva nodig heeft.
* Kunnen uw gebruikers worden automatisch aangemeld druva nodig met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement Druva eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Druva ondersteunt door **SP** en **IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-druva-from-the-gallery"></a>Druva uit de galerie toevoegen

Om de integratie van Druva in Azure Active Directory te configureren, moet u Druva uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Druva** in het zoekvak in.
1. Selecteer **Druva** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Druva met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Druva vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met druva nodig, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van SSO Druva](#configure-druva-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Druva maken](#create-druva-test-user)** : als u een tegenhanger van Britta Simon in Druva wilt hebben die is gekoppeld aan de Azure Active Directory-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Druva** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus wordt de gebruiker heeft geen een stap uitvoeren omdat de app al vooraf geïntegreerd in Azure is.

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://login.druva.com/login`

1. De Druva-toepassing verwacht dat SAML-asserties een specifieke indeling hebben. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

1. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:

    | Name | Bronkenmerk|
    | ------------------- | -------------------- |
    | insync\_auth\_token |Voer de door het token gegenereerde waarde in |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **Druva instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-druva-sso"></a>Druva eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf Druva site als beheerder.

1. Ga naar **Beheren \> Instellingen**.

    ![Settings](./media/druva-tutorial/ic795091.png "Settings")

1. Voer in het dialoogvenster Instellingen voor eenmalige aanmelding de volgende stappen uit:

    ![Instellingen voor eenmalige aanmelding](./media/druva-tutorial/ic795092.png "Instellingen voor eenmalige aanmelding")

    a. Plak in het tekstvak **Aanmeldings-URL van ID-provier** de waarde van de **Aanmeldings-URL** in die u hebt gekopieerd uit de Azure Portal.

    b. Plak in het tekstvak **Afmeldings-URL van ID-provier** de waarde van de **Afmeldings-URL** in die u hebt gekopieerd uit het Azure Portal

    c. Open uw base-64 gecodeerde certificaat in Kladblok, kopieer de inhoud ervan naar het klembord en plak deze naar het tekstvak **Certificaat ID-provider**

    d. Om de pagina **Instellingen** te openen, klikt u op **Opslaan**.

1. Op de pagina **Instellingen** klikt u op **SSO-token genereren**.

    ![Settings](./media/druva-tutorial/ic795093.png "Settings")

1. Voer in het dialoogvenster **Verificatietoken voor eenmalige aanmelding** de volgende stappen uit:

    ![SSO-token](./media/druva-tutorial/ic795094.png "SSO-token")

    a. Klik op **Kopiëren**, plak de gekopieerde waarde in het **Waarde**-tekstvak in de sectie **Kenmerk toevoegen** in de Azure Portal.

    b. Klik op **Sluiten**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `Britta Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `BrittaSimon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan druva nodig.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen **Druva**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-druva-test-user"></a>Druva-testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij druva nodig, moeten ze worden ingericht voor druva nodig. In het geval van Druva is de inrichting een handmatige taak.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Aanmelden bij uw **Druva** bedrijf site als administrator.

1. Ga naar **Beheren \> Gebruikers**.

    ![Gebruikers beheren](./media/druva-tutorial/ic795097.png "Gebruikers beheren")

1. Klik op **Nieuwe maken**.

    ![Gebruikers beheren](./media/druva-tutorial/ic795098.png "Gebruikers beheren")

1. In het dialoogvenster Nieuwe gebruiker maken voert u de volgende stappen uit:

    ![Nieuwe gebruiker maken](./media/druva-tutorial/ic795099.png "Nieuwe gebruiker maken")

    a. In de **e-mailadres** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon\@contoso.com**.

    b. Voer in het tekstvak **Volledige naam** de volledige naam van de gebruiker in, bijvoorbeeld **Britta Simon**.

    c. Klik op **Gebruiker maken**.

> [!NOTE]
> U kunt alle andere door Druva geleverde hulpprogramma's of API's voor Druva-gebruikersaccounts gebruiken voor het inrichten van gebruikersaccounts van Azure Active Directory.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Druva in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de druva nodig waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
