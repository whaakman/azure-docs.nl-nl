---
title: 'Zelfstudie: Azure Active Directory-integratie met Displayr | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Displayr.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: b739b4e3-1a37-4e3c-be89-c3945487f4c1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 93a1ad1f9fbc01cd06b3aaffc8a718634e8454d6
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357035"
---
# <a name="tutorial-integrate-displayr-with-azure-active-directory"></a>Zelfstudie: Displayr integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Displayr integreert met Azure Active Directory (Azure AD). Wanneer u Displayr met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Displayr heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Displayr met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement Displayr eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Displayr **SP** gestart door SSO.

## <a name="adding-displayr-from-the-gallery"></a>Displayr uit de galerie toe te voegen

Voor het configureren van de integratie van Displayr in Azure AD, moet u Displayr uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Displayr** in het zoekvak in.
1. Selecteer **Displayr** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Displayr met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Displayr vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Displayr, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van Displayr](#configure-displayr)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker Displayr](#create-displayr-test-user)**  hebben een equivalent van Britta Simon in Displayr die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Displayr** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **Set-up Single Sign-On met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de sectie **Standaard SAML-configuratie** voert u de volgende stap uit:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<YOURDOMAIN>.displayr.com`

    b. In de **id (entiteits-ID)** tekstvak typt u een URL met behulp van het volgende patroon:`<YOURDOMAIN>.displayr.com`

    >[!NOTE]
    >Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [Displayr Client ondersteuningsteam](mailto:support@displayr.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar de patronen die wordt weergegeven in de sectie SAML-basisconfiguratie in Azure portal.

1. Op de **Set-up Single Sign-On met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Displayr toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram **Bewerken** om het dialoogvenster gebruikerskenmerken te openen.

    ![image](common/edit-attribute.png)

1. Bovendien hierboven verwacht Displayr toepassing paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de **gebruikerskenmerken en Claims** sectie op de **groepclaims (Preview)** dialoogvenster, voer de volgende stappen uit:

    a. Klik op de **pen** naast **groepen die worden geretourneerd in de claim**.

    ![image](./media/displayr-tutorial/config04.png)

    ![image](./media/displayr-tutorial/config05.png)

    b. Selecteer **alle groepen** uit de lijst met keuzerondjes.

    c. Selecteer **bronkenmerk** van **groeps-ID**.

    d. Controleer **aanpassen van de naam van de groepclaim**.

    e. Controleer **groepen als rolclaims verzenden**.

    f. Klik op **Opslaan**.

1. Op de **Set-up Displayr** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-displayr"></a>Displayr configureren

1. Voor het automatiseren van de configuratie in Displayr, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup Displayr** wordt u doorgeleid naar de toepassing Displayr. Geef de referenties van de beheerder zich aanmelden bij Displayr daar. De browserextensie wordt automatisch configureren van de toepassing voor u en stap 3-6 automatiseren.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u Displayr handmatig instellen wilt, opent u een nieuw browservenster en meld u in uw site van het bedrijf Displayr als beheerder en voer de volgende stappen uit:

4. Klik op **instellingen** navigeer vervolgens naar **Account**.

    ![Configuratie](./media/displayr-tutorial/config01.png)

5. Schakel over naar **instellingen** in het bovenste menu en schuif omlaag in de pagina voor het klikken op **configureren van eenmalige aanmelding op (SAML)** .

    ![Configuratie](./media/displayr-tutorial/config02.png)

6. Op de **eenmalige aanmelding op (SAML)** pagina, voert u de volgende stappen uit:

    ![Configuratie](./media/displayr-tutorial/config03.png)

    a. Controleer de **inschakelen eenmalige aanmelding op (SAML)** vak.

    b. Kopieert de werkelijke **id** waarde uit de **SAML-basisconfiguratie** sectie van Azure AD en plak deze in de **verlener** in het tekstvak.

    c. In de **aanmeldings-URL** tekst vak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    d. In de **afmeldings-URL van** tekst vak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    e. Het certificaat (Base64) openen in Kladblok, Kopieer de inhoud en plak deze in de **certificaat** in het tekstvak.

    f. **Groep toewijzingen** zijn optioneel.

    g. Klik op **Opslaan**.  

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

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door toegang te verlenen aan Displayr gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Displayr**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-displayr-test-user"></a>Displayr testgebruiker maken

Als u wilt dat Azure AD-gebruikers, meld u aan in op Displayr, ze moeten worden ingericht voor Displayr. In Displayr is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij Displayr als beheerder.

2. Klik op **instellingen** navigeer vervolgens naar **Account**.

    ![Displayr configuratie](./media/displayr-tutorial/config01.png)

3. Schakel over naar **instellingen** in het bovenste menu en schuif omlaag in de pagina totdat **gebruikers** sectie en vervolgens klikt op **nieuwe gebruiker**.

    ![Displayr configuratie](./media/displayr-tutorial/config07.png)

4. Op de **nieuwe gebruiker** pagina, voert u de volgende stappen uit:

    ![Displayr configuratie](./media/displayr-tutorial/config06.png)

    a. In **naam** tekst voert u de naam van gebruiker, zoals **Brittasimon**.

    b. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals `Brittasimon@contoso.com`.

    c. Selecteer de juiste **groepslidmaatschap**.

    d. Klik op **Opslaan**.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Displayr in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Displayr waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
