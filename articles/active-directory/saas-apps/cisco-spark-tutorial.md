---
title: 'Zelfstudie: Azure Active Directory-integratie met Cisco Webex | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cisco Webex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1dc537a631cd083da0f902fb4fcd44d47756eeba
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67471783"
---
# <a name="tutorial-integrate-cisco-webex-with-azure-active-directory"></a>Zelfstudie: Cisco Webex integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Cisco Webex integreert met Azure Active Directory (Azure AD). Wanneer u Cisco Webex met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Cisco Webex heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Cisco Webex met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Cisco Webex eenmalige aanmelding (SSO) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Cisco Webex **SP en IDP** geïnitieerde eenmalige aanmelding en ondersteunt **automatisch** inrichten van gebruikers.

## <a name="adding-cisco-webex-from-the-gallery"></a>Cisco Webex toevoegen vanuit de galerie

Voor het configureren van de integratie van Cisco Webex in Azure AD moet u Cisco Webex vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Cisco Webex** in het zoekvak in.
1. Selecteer **Cisco Webex** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Cisco Webex met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Cisco Webex vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Cisco Webex, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van Cisco Webex](#configure-cisco-webex)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B.Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Cisco Webex testgebruiker maken](#create-cisco-webex-test-user)**  hebben een equivalent van B.Simon in Cisco Webex die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Cisco Webex** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** uploadt u het bestand met metagegevens dat u hebt gedownload van de **serviceprovider** en configureert u de toepassing door de volgende stappen uit te voeren:

    >[!Note]
    >U krijgt het metagegevensbestand van de Service-Provider van de **Cisco Webex** Portal. 

    a. Klik op **metagegevensbestand uploaden**.

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    c. Als het uploaden van het metagegevensbestand van de serviceprovider is geslaagd, worden de waarden voor de **id** en **Antwoord-URL** automatisch ingevuld in de sectie **Standaard SAML-configuratie**:

    In de **aanmeldings-URL** tekstvak, plak de waarde van **antwoord-URL**, die wordt autofilled door SP uploaden van de metagegevens van het bestand.

5. In de Cisco Webex-toepassing worden de SAML-asserties in een specifieke indeling verwacht. Hiervoor moet u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van uw SAML-tokenkenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram **Bewerken** om de kenmerken toe te voegen.

    ![image](common/edit-attribute.png)

6. Bovendien verwacht de Cisco Webex-toepassing nog enkele kenmerken die als SAML-antwoord moeten worden doorgestuurd. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:
    
    | Name |  Bronkenmerk|
    | ---------------|--------- |
    | uid | user.userprincipalname |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **federatieve metagegevens-XML** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/metadataxml.png)

1. Op de **instellen van Cisco Webex** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-cisco-webex"></a>Cisco Webex configureren

1. Meld u aan bij [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) met uw volledige beheerdersreferenties.

2. Selecteer **Settings** en klik in de sectie **Authentication** op **Modify**.

    ![Eenmalige aanmelding configureren](./media/cisco-spark-tutorial/tutorial_cisco_spark_10.png)
  
3. Selecteer **Integrate a 3rd-party identity provider. (Advanced)** en ga naar het volgende scherm.

4. Sleep het bestand met de Azure AD-metagegevens naar de pagina **Import Idp Metadata** of gebruik de optie 'file browser' om het bestand te zoeken en te uploaden. Selecteer vervolgens **Require certificate signed by a certificate authority in Metadata (more secure)** en klik op **Next**.

    ![Eenmalige aanmelding configureren](./media/cisco-spark-tutorial/tutorial_cisco_spark_11.png)

5. Selecteer **Test SSO Connection**. Wanneer er een nieuw browsertabblad wordt geopend, moet u zich verifiëren bij Azure AD door u aan te melden.

6. Ga terug naar het browsertabblad **Cisco Cloud Collaboration Management**. Als de test is geslaagd, selecteert u **This test was successful.** Schakel de optie voor eenmalige aanmelding in en klik op **Next**.

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

In deze sectie schakelt u B.Simon Azure eenmalige aanmelding door toegang te verlenen aan Cisco Webex gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Typ en selecteer **Cisco Webex** in de lijst met toepassingen.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-cisco-webex-test-user"></a>Testgebruiker voor Cisco Webex maken

In deze sectie gaat u een gebruiker met de naam Britta Simon maken in Cisco Webex. In deze sectie gaat u een gebruiker met de naam Britta Simon maken in Cisco Webex.

1. Meld u aan bij [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) met uw volledige beheerdersreferenties.

2. Klik op **Users** en vervolgens op **Manage Users**.
   
    ![Eenmalige aanmelding configureren](./media/cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. Selecteer **Manually Add or Modify Users** in het venster **Manage User** en klik vervolgens op **Next**.

4. Selecteer **Names and Email address**. Vul vervolgens de tekstvakken als volgt in:

    ![Eenmalige aanmelding configureren](./media/cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    a. In de **voornaam** tekstvak, type de voornaam van de gebruiker, zoals **B**.

    b. Typ in het tekstvak **Last Name** de achternaam van de gebruiker, bijvoorbeeld **Simon**.

    c. In de **e-mailadres** tekstvak type e-mailadres van gebruiker, zoals b.simon@contoso.com.

5. Klik op het plusteken om Britta Simon toe te voegen. Klik op **Volgende**.

6. Klik in het venster **Add Services for Users** op **Save** en daarna op **Finish**.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de Cisco Webex-tegel in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Cisco Webex waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)