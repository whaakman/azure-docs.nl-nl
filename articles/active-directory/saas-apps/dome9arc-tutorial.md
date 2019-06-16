---
title: 'Zelfstudie: Azure Active Directory-integratie met punt CloudGuard Dome9 boog controleren | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en controleer punt CloudGuard Dome9 boog.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdaaab8257d3a79130902e1ba0466f9cf15484f4
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147148"
---
# <a name="tutorial-integrate-check-point-cloudguard-dome9-arc-with-azure-active-directory"></a>Zelfstudie: Check Point CloudGuard Dome9 boog integreren met Azure Active Directory

In deze zelfstudie leert u hoe om te controleren op punt CloudGuard Dome9 boog integreren met Azure Active Directory (Azure AD). Wanneer u Controleer punt CloudGuard Dome9 boog met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot het punt CloudGuard Dome9 boog controleren heeft.
* Kunnen uw gebruikers worden automatisch aangemeld om te controleren op punt CloudGuard Dome9 boog met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Check Point CloudGuard Dome9 boog eenmalige aanmelding (SSO) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Controleer punt CloudGuard Dome9 boog ondersteunt **SP en IDP** gestart door SSO.

## <a name="adding-check-point-cloudguard-dome9-arc-from-the-gallery"></a>Controleer punt CloudGuard Dome9 boog uit de galerie toe te voegen

Voor het configureren van de integratie van punt CloudGuard Dome9 boog controleren in Azure AD, moet u controleren punt CloudGuard Dome9 boog uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **controleren punt CloudGuard Dome9 boog** in het zoekvak in.
1. Selecteer **controleren punt CloudGuard Dome9 boog** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met controleren punt CloudGuard Dome9 boog met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in punt CloudGuard Dome9 boog controleren.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met een punt CloudGuard Dome9 boog controleren, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Controleer punt CloudGuard Dome9 boog configureren](#configure-check-point-cloudguard-dome9-arc)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B.Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Controleer punt CloudGuard Dome9 boog testgebruiker maken](#create-check-point-cloudguard-dome9-arc-test-user)**  hebben een equivalent van B.Simon controleren punt CloudGuard Dome9 boog die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **controleren punt CloudGuard Dome9 boog** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding** .
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://secure.dome9.com/`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > U selecteert de waarde voor uw bedrijfsnaam in de beheerportal van Dome9, zoals later in de zelfstudie wordt uitgelegd.

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://secure.dome9.com/sso/saml/<yourcompanyname>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke antwoord-URL en aanmeldings-URL. Neem contact op met [controleren punt CloudGuard Dome9 boog Client ondersteuningsteam](mailto:Dome9@checkpoint.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Check Point CloudGuard Dome9 boog toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram **Bewerken** om het dialoogvenster Gebruikerskenmerken te openen.

    ![image](common/edit-attribute.png)

7. Bovendien hierboven verwacht controleren punt CloudGuard Dome9 boog toepassing paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel: 

    | Name |  Bronkenmerk|
    | ---------------| --------------- |
    | memberof | user.assignedroles |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **instellen controleren punt CloudGuard Dome9 boog** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-check-point-cloudguard-dome9-arc"></a>Check Point CloudGuard Dome9 boog configureren

1. In een ander browservenster, meld u aan bij uw bedrijf controleren punt CloudGuard Dome9 boog site als beheerder.

2. Klik op **Profile Settings** in de rechterbovenhoek en vervolgens op **Account Settings**. 

    ![Controleer de configuratie van punt CloudGuard Dome9 boog](./media/dome9arc-tutorial/configure1.png)

3. Navigeer naar **SSO** en klik vervolgens op **ENABLE**.

    ![Controleer de configuratie van punt CloudGuard Dome9 boog](./media/dome9arc-tutorial/configure2.png)

4. Voer in de sectie SSO Configuration de volgende stappen uit:

    ![Controleer de configuratie van punt CloudGuard Dome9 boog](./media/dome9arc-tutorial/configure3.png)

    a. Voer in het tekstvak **Account ID** de bedrijfsnaam in. Deze waarde is moet worden gebruikt in de antwoord-URL die worden vermeld in de Azure-portal **SAML-basisconfiguratie** sectie.

    b. In de **verlener** tekstvak, plak de waarde van **Azure AD-id**, die u in Azure portal hebt gekopieerd.

    c. Plak in het tekstvak **Idp endpoint url** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    d. Open in Kladblok het met Base64 gecodeerde certificaat dat u hebt gedownload, kopieer de inhoud ervan naar het Klembord en plak deze vervolgens in het tekstvak **X.509 certificate**.

    e. Klik op **Opslaan**.

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

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te controleren op punt CloudGuard Dome9 boog.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **controleren punt CloudGuard Dome9 boog**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-check-point-cloudguard-dome9-arc-test-user"></a>Controleer punt CloudGuard Dome9 boog testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij punt CloudGuard Dome9 boog controleren, moeten deze zijn ingericht in toepassing. Controleer punt CloudGuard Dome9 boog biedt ondersteuning voor just-in-time inrichting maar voor die goed te laten werken, gebruiker moet selecteren bepaalde **rol** en dezelfde toe te wijzen aan de gebruiker.

   >[!Note]
   >Voor **rol** maken en andere details Neem contact op met [controleren punt CloudGuard Dome9 boog Client ondersteuningsteam](mailto:Dome9@checkpoint.com).

**Voor het handmatig inrichten van een gebruikersaccount moet u de volgende stappen uitvoeren:**

1. Meld u aan uw bedrijf controleren punt CloudGuard Dome9 boog site als een beheerder.

2. Klik op **Users & Roles** en vervolgens op **Users**.

    ![Werknemer toevoegen](./media/dome9arc-tutorial/user1.png)

3. Klik op **ADD USER**.

    ![Werknemer toevoegen](./media/dome9arc-tutorial/user2.png)

4. Voer in de sectie **Create User** de volgende stappen uit:

    ![Werknemer toevoegen](./media/dome9arc-tutorial/user3.png)

    a. Typ in het tekstvak **Email** het e-mailadres van de gebruiker, bijvoorbeeld B.Simon@contoso.com.

    b. In de **voornaam** tekstvak, type de voornaam van de gebruiker, zoals B.

    c. Typ in het tekstvak **Last Name** de achternaam van de gebruiker, bijvoorbeeld Simon.

    d. Zet **SSO User** op **On**.

    e. Klik op **MAKEN**.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel controleren punt CloudGuard Dome9 boog in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de controleren punt CloudGuard Dome9 boog waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)