---
title: 'Zelfstudie: Azure Active Directory-integratie met Cisco Webex vergaderingen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cisco Webex-vergaderingen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 308f745489fba2e2b539a2f2615b65228565dcf9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57850006"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex-meetings"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Webex vergaderingen

In deze zelfstudie leert u hoe u Cisco Webex vergaderingen integreren met Azure Active Directory (Azure AD).
Cisco Webex vergaderingen integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Cisco Webex-vergaderingen heeft.
* U kunt uw gebruikers worden automatisch aangemeld Cisco Webex vergaderingen (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Cisco Webex vergaderingen, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Cisco Webex vergaderingen eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Cisco Webex vergaderingen **SP** gestart door SSO

* Biedt ondersteuning voor Cisco Webex vergaderingen **Just In Time** inrichten van gebruikers

## <a name="adding-cisco-webex-meetings-from-the-gallery"></a>Cisco Webex vergaderingen uit de galerie toe te voegen

Voor het configureren van de integratie van Cisco Webex vergaderingen in Azure AD, moet u Cisco Webex vergaderingen uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cisco Webex vergaderingen uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Cisco Webex vergaderingen**, selecteer **Cisco Webex vergaderingen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Cisco Webex vergaderingen in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Cisco Webex-vergaderingen op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker Cisco Webex vergaderingen tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Cisco Webex vergaderingen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van Cisco Webex vergaderingen Single Sign-On](#configure-cisco-webex-meetings-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Cisco Webex vergaderingen testgebruiker maken](#create-cisco-webex-meetings-test-user)**  : als u wilt een equivalent van Britta Simon in Cisco Webex vergaderingen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Cisco Webex vergaderingen, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Cisco Webex vergaderingen** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. In de Azure-portal, op de pagina **Eenmalige aanmelding met SAML instellen**, klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** uploadt u het bestand met metagegevens dat u hebt gedownload van de **serviceprovider** en configureert u de toepassing door de volgende stappen uit te voeren:

    >[!Note]
    >U krijgt het bestand met metagegevens voor Service-Provider, die later in wordt uitgelegd de **configureren Cisco Webex vergaderingen Single Sign-On** gedeelte van de zelfstudie. 

    a. Klik op **Metagegevensbestand uploaden**.

    ![Metagegevensbestand uploaden](common/upload-metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Metagegevensbestand kiezen](common/browse-upload-metadata.png)

    c. Als het uploaden van het metagegevensbestand van de serviceprovider is geslaagd, worden de waarden voor de **id** en **Antwoord-URL** automatisch ingevuld in de sectie **Standaard SAML-configuratie**:

    ![Cisco Webex vergaderingen domein en URL's, eenmalige aanmelding informatie](common/sp-identifier-reply.png)

    In de **aanmeldings-URL** tekstvak, plak de waarde van **antwoord-URL** die wordt automatisch gevuld op SP metagegevens van bestand is geüpload.

5. Cisco vergaderingen Webex-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram  **Bewerken**  om de kenmerken toe te voegen.

    ![image](common/edit-attribute.png)

6. Verwijderen van de standaardkenmerken van **gebruikersclaims** back-sectie en Cisco Webex-vergaderingen toepassing aantal meer kenmerken verwacht moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:
    
    | Name | Bronkenmerk|
    | ---------------|  --------- |
    |   firstname    | user.givenname |
    |   lastname    | user.surname |
    |   e-mail       | user.mail |
    |   uid    | user.mail |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](./media/cisco-webex-tutorial/tutorial-cisco-webex-addnewclaim.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

8. Op de **instellen van Cisco Webex vergaderingen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-cisco-webex-meetings-single-sign-on"></a>Cisco Webex vergaderingen eenmalige aanmelding configureren

1. Ga naar [Cisco samenwerking Cloudbeheer](https://www.webex.com/go/connectadmin) met de referenties van uw beheer.

2. Ga naar **beveiligingsinstellingen** en navigeer naar **federatief Web SSO-configuratie**.
 
    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial-cisco-webex-10.png)

3. Op de **federatief Web SSO-configuratie** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial-cisco-webex-11.png)

    a. Typ de naam van het protocol in het tekstvak Federation-Protocol.

    b. Klik op **SAML-metagegevens importeren** koppeling voor het uploaden van het bestand met metagegevens, die u hebt gedownload vanuit Azure portal.

    c. Klik op **exporteren** knop om te downloaden van de metagegevens van de Service Provider-bestand en upload het in de **SAML-basisconfiguratie** sectie in Azure portal.

    d. In de **AuthContextClassRef** tekstvak, type `urn:oasis:names:tc:SAML:2.0:ac:classes:unspecified` en als u wilt dat het inschakelen van het gebruik van Azure AD MFA typt u de twee waarden, zoals `urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport;urn:oasis:names:tc:SAML:2.0:ac:classes:X509`

    e. Selecteer **automatisch van het maken van accounts**.

    >[!NOTE]
    >Voor het inschakelen van **just-in-time** gebruikersinrichting die u nodig hebt om te controleren of de **-Account maken automatisch**. Daarnaast SAML-token kenmerken moeten worden doorgegeven in het SAML-antwoord.

    f. Klik op **Opslaan**. 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen voor Cisco Webex vergaderingen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Cisco Webex vergaderingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Cisco Webex vergaderingen**.

    ![De koppeling Cisco Webex vergaderingen in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-cisco-webex-meetings-test-user"></a>Cisco Webex vergaderingen testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon Cisco Webex-vergaderingen. Biedt ondersteuning voor Cisco Webex vergaderingen **just-in-time** inrichten, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in de Webex-vergaderingen Cisco bestaat, wordt een nieuw wordt gemaakt wanneer u probeert te krijgen tot Cisco Webex-vergaderingen.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Cisco Webex-vergaderingstegel in het toegangsvenster, moet u worden automatisch aangemeld bij de Cisco Webex-vergaderingen waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

