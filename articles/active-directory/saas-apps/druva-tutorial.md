---
title: 'Zelfstudie: Azure Active Directory-integratie met Druva | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Druva.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/04/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5aac1b32f420f4a028777d1a9b5dc6b31cab23a1
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56872553"
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Zelfstudie: Azure Active Directory-integratie met Druva

In deze zelfstudie leert u hoe u Druva kunt integreren met Azure Active Directory (Azure AD).
Druva integreren met Azure Active Directory biedt u de volgende voordelen:

* U kunt in Azure Active Directory beheren wie toegang tot Druva nodig heeft.
* U kunt uw gebruikers zich automatisch laten aanmelden bij Druva (eenmalige aanmelding) met hun Azure Active Directory-account.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure Active Directory-integratie met Druva heeft u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Druva waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Druva ondersteunt door **SP** en **IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-druva-from-the-gallery"></a>Druva uit de galerie toevoegen

Om de integratie van Druva in Azure Active Directory te configureren, moet u Druva uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u Druva uit de galerie wilt toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Druva**, selecteer **Druva** in het resultaatvenster en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Druva in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding bij Azure Active Directory met Druva op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure Active Directory-gebruiker en de daaraan gerelateerde gebruiker in Druva tot stand is gebracht.

Om eenmalige aanmelding bij Azure Active Directory met Druva te configureren en testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding bij Druva configureren](#configure-druva-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor Druva maken](#create-druva-test-user)**: als u een tegenhanger van Britta Simon in Druva wilt hebben die is gekoppeld aan de Azure Active Directory-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van eenmalige aanmelding bij Azure Active Directory met Druva, moet u de volgende stappen uitvoeren:

1. Selecteer in de [Azure Portal](https://portal.azure.com/) **Eenmalige aanmelding** op de integratiepagina van de toepassing **Druva**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Domein- en URL-gegevens voor eenmalige aanmelding met Druva](common/idp-identifier.png)

    In het tekstvak **Id** typt u een tekenreekswaarde: `druva-cloud`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![image](common/both-preintegrated-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://cloud.druva.com/home`

6. De Druva-toepassing verwacht dat SAML-asserties een specifieke indeling hebben. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

7. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit: 

    | Naam | Bronkenmerk|
    | ------------------- | -------------------- |
    | insync\_auth\_token |Voer de door het token gegenereerde waarde in |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

9. In de sectie **Druva instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-druva-single-sign-on"></a>Druva voor eenmalige aanmelding configureren

1. Meld u in een ander browservenster aan bij uw bedrijfssite van Druva als een beheerder.

2. Ga naar **Beheren \> Instellingen**.

    ![Settings](./media/druva-tutorial/ic795091.png "Settings")

3. Voer in het dialoogvenster Instellingen voor eenmalige aanmelding de volgende stappen uit:

    ![Instellingen voor eenmalige aanmelding](./media/druva-tutorial/ic795092.png "Instellingen voor eenmalige aanmelding")
    
    a. Plak in het tekstvak **Aanmeldings-URL van ID-provier** de waarde van de **Aanmeldings-URL** in die u hebt gekopieerd uit de Azure Portal.
        
    b. Plak in het tekstvak **Afmeldings-URL van ID-provier** de waarde van de **Afmeldings-URL** in die u hebt gekopieerd uit het Azure Portal
        
    c. Open uw base-64 gecodeerde certificaat in Kladblok, kopieer de inhoud ervan naar het klembord en plak deze naar het tekstvak **Certificaat ID-provider**
     
    d. Om de pagina **Instellingen** te openen, klikt u op **Opslaan**.

4. Op de pagina **Instellingen** klikt u op **SSO-token genereren**.

    ![Settings](./media/druva-tutorial/ic795093.png "Settings")

5. Voer in het dialoogvenster **Verificatietoken voor eenmalige aanmelding** de volgende stappen uit:

    ![SSO-token](./media/druva-tutorial/ic795094.png "SSO-token")
    
    a. Klik op **Kopiëren**, plak de gekopieerde waarde in het **Waarde**-tekstvak in de sectie **Kenmerk toevoegen** in de Azure Portal.
    
    b. Klik op **Sluiten**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Druva.

1. Selecteer **Bedrijfstoepassingen** in Azure Portal, selecteer **Alle toepassingen** en selecteer vervolgens **Druva**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **Druva**.

    ![De Druva-link in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-druva-test-user"></a>Druva-testgebruiker maken

Als u wilt dat Azure Active Directory-gebruikers zich kunnen aanmelden bij Druva, moeten ze worden ingericht voor Druva. In het geval van Druva is de inrichting een handmatige taak.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u als administrator aan bij uw **Druva**-bedrijfssite.

2. Ga naar **Beheren \> Gebruikers**.
   
    ![Gebruikers beheren](./media/druva-tutorial/ic795097.png "Gebruikers beheren")

3. Klik op **Nieuwe maken**.
   
    ![Gebruikers beheren](./media/druva-tutorial/ic795098.png "Gebruikers beheren")

4. In het dialoogvenster Nieuwe gebruiker maken voert u de volgende stappen uit:
   
    ![Nieuwe gebruiker maken](./media/druva-tutorial/ic795099.png "Nieuwe gebruiker maken")
   
    a. Typ in het tekstvak **Email-adres** het e-mailadres van de gebruiker, bijvoorbeeld **brittasimon@contoso.com**.
   
    b. Voer in het tekstvak **Volledige naam** de volledige naam van de gebruiker in, bijvoorbeeld **Britta Simon**.
   
    c. Klik op **Gebruiker maken**.

>[!NOTE]
>U kunt alle andere door Druva geleverde hulpprogramma's of API's voor Druva-gebruikersaccounts gebruiken voor het inrichten van gebruikersaccounts van Azure Active Directory.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Druva in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van Druva waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

