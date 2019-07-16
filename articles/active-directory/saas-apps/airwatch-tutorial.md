---
title: 'Zelfstudie: Azure Active Directory-integratie met AirWatch | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding kunt configureren tussen Azure Active Directory en AirWatch.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f0a684c6a38ba3a95438941f668b36b23d278df
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68227578"
---
# <a name="tutorial-integrate-airwatch-with-azure-active-directory"></a>Zelfstudie: AirWatch integreren met Azure Active Directory

In deze zelfstudie leert u hoe u AirWatch integreert met Azure Active Directory (Azure AD). Wanneer u AirWatch met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot AirWatch heeft.
* Kunnen uw gebruikers worden automatisch aangemeld AirWatch met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement AirWatch eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor AirWatch **SP** gestart door SSO.

## <a name="adding-airwatch-from-the-gallery"></a>AirWatch toevoegen vanuit de galerie

Voor het configureren van de integratie van AirWatch in Azure AD moet u AirWatch vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **AirWatch** in het zoekvak in.
1. Selecteer **AirWatch** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

Configureren en testen van Azure AD-eenmalige aanmelding met AirWatch met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in AirWatch vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met AirWatch, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van SSO AirWatch](#configure-airwatch-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Een testgebruiker maken in AirWatch](#create-airwatch-test-user)** : om in AirWatch een tegenhanger van Britta Simon te hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
5. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **AirWatch** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    1. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    1. Typ in het vak **Id (Entiteits-id)** de waarde als: `AirWatch`

    > [!NOTE]
    > Dit is niet de echte waarde. Werk deze waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [het ondersteuningsteam van AirWatch Client](https://www.air-watch.com/company/contact-us/) om deze waarde te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. De AirWatch-toepassing verwacht de SAML-asserties in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

1. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:

    | Name |  Bronkenmerk|
    |---------------|----------------|
    | UID | user.userprincipalname |
    | | |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **federatieve metagegevens-XML** en selecteer **downloaden** de Metadata XML en sla deze op uw computer.

   ![De downloadkoppeling certificaat](common/metadataxml.png)

1. Op de **AirWatch instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-airwatch-sso"></a>AirWatch eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf AirWatch site als beheerder.

1. Op de instellingenpagina. Selecteer **instellingen > bedrijfsintegratie > directoryservices**.

   ![Settings](./media/airwatch-tutorial/ic791921.png "Settings")

1. Klik op het tabblad **Gebruiker**. Typ uw domeinnaam in het vak **Basis-DN** en klik op **Opslaan**.

   ![Gebruiker](./media/airwatch-tutorial/ic791922.png "gebruiker")

1. Klik op het tabblad **Server**.

   ![Server](./media/airwatch-tutorial/ic791923.png "Server")

1. Voer de volgende stappen uit op de **LDAP** sectie:

    ![Uploaden](./media/airwatch-tutorial/ic791924.png "LDAP")   

    a. Selecteer **Geen** als **Type adressenlijst**.

    b. Selecteer **SAML gebruiken voor verificatie**.

1. Op de **SAML 2.0** sectie om de gedownloade certificaat te uploaden, klikt u op **uploaden**.

    ![Uploaden](./media/airwatch-tutorial/ic791932.png "Uploaden")

1. Voer in het gedeelte **Aanvraag** de volgende stappen uit:

    ![Aanvraag](./media/airwatch-tutorial/ic791925.png "Aanvraag")  

    a. Selecteer **POST** als **Aanvraagbindingstype**.

    b. In de Azure-portal op de **configureren van eenmalige aanmelding bij AirWatch** dialoogvenster pagina-, Kopieer de **aanmeldings-URL** waarde en plak deze in de **eenmalige aanmelding op URL identiteitsprovider** tekstvak.

    c. Als **NameID-indeling** selecteert u **E-mailadres**.

    d. Als **aanvragen verificatiebeveiliging**, selecteer **geen**.

    e. Klik op **Opslaan**.

1. Klik nogmaals op het tabblad **Gebruiker**.

    ![Gebruiker](./media/airwatch-tutorial/ic791926.png "Gebruiker")

1. Voer in het gedeelte **Kenmerk** de volgende stappen uit:

    ![Kenmerk](./media/airwatch-tutorial/ic791927.png "Kenmerk")

    a. Typ `http://schemas.microsoft.com/identity/claims/objectidentifier` in het tekstvak **Object-ID**.

    b. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` in het vak **Gebruikersnaam**.

    c. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` in het vak **Weergavenaam**.

    d. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` in het vak **Voornaam**.

    e. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` in het vak **Achternaam**.

    f. Typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` in het tekstvak **E-mailadres**.

    g. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt u een testgebruiker in Azure portal B.Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan AirWatch.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer **AirWatch** in de lijst met toepassingen.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-airwatch-test-user"></a>Een testgebruiker maken in AirWatch

Om Azure AD-gebruikers zich aanmelden bij AirWatch, moeten ze worden ingericht voor AirWatch. In het geval van AirWatch is inrichten een handmatige taak.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Aanmelden bij uw **AirWatch** bedrijf site als administrator.

2. Klik in het linkernavigatievenster op **Accounts** en vervolgens op **Gebruikers**.
  
   ![Gebruikers](./media/airwatch-tutorial/ic791929.png "Gebruikers")

3. In de **gebruikers** menu, klikt u op **lijstweergave**, en klik vervolgens op **toevoegen > gebruiker toevoegen**.
  
   ![Gebruiker toevoegen](./media/airwatch-tutorial/ic791930.png "Gebruiker toevoegen")

4. Voer in het dialoogvenster **Gebruiker toevoegen/bewerken** de volgende stappen uit:

   ![Gebruiker toevoegen](./media/airwatch-tutorial/ic791931.png "Gebruiker toevoegen")

   a. Typ de **Gebruikersnaam**, het **Wachtwoord**, **Wachtwoord bevestigen**, **Voornaam**, **Achternaam**, **E-mailadres** van een geldig Azure Active Directory-account dat u wilt inrichten in de desbetreffende tekstvakken.

   b. Klik op **Opslaan**.

> [!NOTE]
> U kunt ook alle andere hulpprogramma's voor het creëren van AirWatch-gebruikersaccounts of API's van AirWatch gebruiken om AAD-gebruikersaccounts in te richten.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel AirWatch in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de AirWatch waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
