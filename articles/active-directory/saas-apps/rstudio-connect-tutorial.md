---
title: 'Zelfstudie: Azure Active Directory-integratie met RStudio Connect | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RStudio verbinding maken.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9bc78022-6d38-4476-8f03-e3ca2551e72e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/04/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32aa7a531de2e236e3941bbe8afd84d845f80f99
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64726029"
---
# <a name="tutorial-azure-active-directory-integration-with-rstudio-connect"></a>Zelfstudie: Azure Active Directory-integratie met RStudio Connect

In deze zelfstudie leert u over het integreren van RStudio verbinding maken met Azure Active Directory (Azure AD).
Integratie van RStudio verbinding maken met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot de RStudio-verbinding heeft.
* U kunt uw gebruikers worden automatisch aangemeld RStudio verbinding maken (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met RStudio verbinding maken, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* RStudio verbinding maken. Er is een [45 dagen gratis evaluatie.](https://www.rstudio.com/products/connect/)

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor RStudio verbinding **SP en IDP** gestart door SSO

* Biedt ondersteuning voor RStudio verbinding **Just In Time** inrichten van gebruikers

## <a name="adding-rstudio-connect-from-the-gallery"></a>Toevoegen van RStudio verbinding maken vanuit de galerie

Voor het configureren van de integratie van RStudio verbinding maken met Azure AD, moet u RStudio verbinding maken vanuit de galerie aan uw lijst met beheerde SaaS-apps toevoegen.

**Als u wilt RStudio verbinding maken vanuit de galerie toevoegt, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **RStudio verbinding maken met**, selecteer **RStudio verbinding** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![RStudio verbinding maken in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met RStudio verbinding maken op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de RStudio-verbinding tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met RStudio verbinding maken, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van RStudio verbinding maken met Single Sign-On](#configure-rstudio-connect-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak testgebruiker RStudio verbinding](#create-rstudio-connect-test-user)**  : als u wilt een equivalent van Britta Simon in die is gekoppeld aan de Azure AD-weergave van de gebruiker verbinding maken met RStudio hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met RStudio verbinding maken, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **RStudio verbinding** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart Voer de volgende stappen uit, vervangen `<example.com>` met uw RStudio Server verbinding maken Adres en poort:

    ![RStudio verbinding maken met domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<example.com>/__login__/saml`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<example.com>/__login__/saml/acs`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![RStudio verbinding maken met domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<example.com>/`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Ze worden bepaald uit het serveradres van RStudio verbinding maken (`https://example.com` in de bovenstaande voorbeelden). Neem contact op met de [ondersteuningsteam RStudio verbinding](mailto:support@rstudio.com) als u problemen ondervindt. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Uw toepassing RStudio verbinding wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding ziet u de lijst met standaardkenmerken, waarbij **nameidentifier** is toegewezen aan **user.userprincipalname**. RStudio verbinding toepassing verwacht **nameidentifier** worden toegewezen met **user.mail**, dus u de kenmerktoewijzing van het bewerken moet door te klikken op **bewerken** pictogram en wijzig de kenmerk wordt toegewezen.

    ![image](common/edit-attribute.png)

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-rstudio-connect-single-sign-on"></a>Configureren van RStudio verbinding maken met Single Sign-On

Configureren van eenmalige aanmelding in voor **RStudio verbinding**, moet u de **App-Url voor federatieve metagegevens** en **serveradres** hierboven hebt gebruikt. Dit doet u in het configuratiebestand RStudio verbinding maken met `/etc/rstudio-connect.rstudio-connect.gcfg`.

Dit is een voorbeeld-configuratiebestand:

```
[Server]
SenderEmail =

; Important! The user-facing URL of your RStudio Connect server.
Address = 

[Http]
Listen = :3939

[Authentication]
Provider = saml

[SAML]
Logging = true

; Important! The URL where your IdP hosts the SAML metadata or the path to a local copy of it placed in the RStudio Connect server.
IdPMetaData = 

IdPAttributeProfile = azure
SSOInitiated = IdPAndSP
```

Store uw **serveradres** in de `Server.Address` waarde, en de **App-Url voor federatieve metagegevens** in de `SAML.IdPMetaData` waarde.

Als u problemen met configuratie hebt, kunt u lezen de [beheerdershandleiding voor verbinding maken met RStudio](https://docs.rstudio.com/connect/admin/authentication.html#authentication-saml) of stuur een e-mail de [RStudio ondersteuningsteam](mailto:support@rstudio.com) voor hulp.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen RStudio verbinding maken.

1. Selecteer in de Azure portal **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **RStudio verbinding**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **RStudio verbinding**.

    ![De RStudio-Connect-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-rstudio-connect-test-user"></a>Testgebruiker RStudio verbinding maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in RStudio verbinding maken. RStudio verbinding biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in de RStudio-verbinding bestaat, wordt een nieuw wordt gemaakt wanneer u probeert te krijgen tot RStudio verbinding maken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel RStudio verbinding maken in het toegangsvenster, moet u worden automatisch aangemeld bij de RStudio verbinding waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

