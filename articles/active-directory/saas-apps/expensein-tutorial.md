---
title: 'Zelfstudie: Azure Active Directory-integratie met ExpenseIn | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ExpenseIn.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 6ac8053b-a216-45d8-bf5e-ecd37d808e57
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c09542013dff3a18965d1070216a938c26a144e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102845"
---
# <a name="tutorial-integrate-expensein-with-azure-active-directory"></a>Zelfstudie: ExpenseIn integreren met Azure Active Directory

In deze zelfstudie leert u hoe u ExpenseIn integreert met Azure Active Directory (Azure AD). Wanneer u ExpenseIn met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot ExpenseIn heeft.
* Kunnen uw gebruikers worden automatisch aangemeld ExpenseIn met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement ExpenseIn eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor ExpenseIn **SP en IDP** gestart door SSO.

## <a name="adding-expensein-from-the-gallery"></a>ExpenseIn uit de galerie toe te voegen

Voor het configureren van de integratie van ExpenseIn in Azure AD, moet u ExpenseIn uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **ExpenseIn** in het zoekvak in.
1. Selecteer **ExpenseIn** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met ExpenseIn met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ExpenseIn vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met ExpenseIn, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van ExpenseIn](#configure-expensein)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B.Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker ExpenseIn](#create-expensein-test-user)**  hebben een equivalent van B.Simon in ExpenseIn die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **ExpenseIn** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus, de volgende stap uitvoeren:

    In de **antwoord-URL** tekst typt u een van de URL:

    | |
    |--|
    | `https://app.expensein.com/samlcallback` |
    | `https://mobileapi.expensein.com/identity/samlcallback` |

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://app.expensein.com/saml`

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en klikt u op **Downloaden** voor het downloaden van de **certificaat (Base64)** en sla deze op uw computer.

   ![De link om het certificaat te downloaden](./media/expensein-tutorial/copy-metdataurl-certificate.png)

1. Op de **ExpenseIn instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-expensein"></a>ExpenseIn configureren

1. Voor het automatiseren van de configuratie in ExpenseIn, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup ExpenseIn** wordt u doorgeleid naar de toepassing ExpenseIn. Geef de referenties van de beheerder zich aanmelden bij ExpenseIn daar. De browserextensie wordt automatisch de toepassing voor u configureren en stappen 3 tot 5 automatiseren.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u ExpenseIn handmatig instellen wilt, opent u een nieuw browservenster en meld u in uw site van het bedrijf ExpenseIn als beheerder en voer de volgende stappen uit:

4. Klik op **Admin** boven aan de pagina en navigeer vervolgens naar **Single Sign-On** en klikt u op **toevoegen provider**.

     ![ExpenseIn configuratie](./media/expenseIn-tutorial/config01.png)

5. Op de **nieuwe id-Provider** pop, voer de volgende stappen uit:

    ![ExpenseIn configuratie](./media/expenseIn-tutorial/config02.png)

    a. In de **providernaam** tekst typt u de naam, zoals bijvoorbeeld: Azure.

    b. Selecteer **Ja** als **toestaan aanmelding Provider Intitated**.

    c. In de **doel-Url** tekst vak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    d. In de **verlener** tekst vak, plak de waarde van **Azure AD-id**, die u hebt gekopieerd vanuit Azure portal.

    e. Het certificaat (Base64) openen in Kladblok, Kopieer de inhoud en plak deze in de **certificaat** in het tekstvak.

    f. Klik op **Create**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B.Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ExpenseIn.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **ExpenseIn**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-expensein-test-user"></a>ExpenseIn testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij ExpenseIn, moeten ze worden ingericht voor ExpenseIn. In ExpenseIn is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij ExpenseIn als beheerder.

2. Klik op **Admin** boven aan de pagina en navigeer vervolgens naar **gebruikers** en klikt u op **nieuwe gebruiker**.

     ![ExpenseIn configuratie](./media/expenseIn-tutorial/config03.png)

3. Op de **Details** pop, voer de volgende stappen uit:

    ![ExpenseIn configuratie](./media/expenseIn-tutorial/config04.png)

    a. In **voornaam** tekst voert u de voornaam van de gebruiker, zoals **B**.

    b. Typ in het tekstvak **Achternaam** de achternaam van de gebruiker, zoals **Simon**.

    c. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals `B.Simon@contoso.com`.

    d. Klik op **Create**.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel ExpenseIn in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de ExpenseIn waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
