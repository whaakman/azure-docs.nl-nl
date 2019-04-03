---
title: 'Zelfstudie: Azure Active Directory-integratie met herkennen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en herkennen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: bd772a10cd64b4198e994fdefa671444447c8a53
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58849557"
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Zelfstudie: Azure Active Directory-integratie met herkennen

In deze zelfstudie leert u hoe u kunt herkennen integreren met Azure Active Directory (Azure AD).
Herkennen integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot herkennen heeft.
* U kunt uw gebruikers worden automatisch aangemeld te herkennen (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met herkennen, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Eenmalige aanmelding ingeschakeld abonnement herkennen

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Ondersteunt herkennen **SP** gestart door SSO

## <a name="adding-recognize-from-the-gallery"></a>Herkennen in de galerie toevoegen

Voor het configureren van de integratie van herkennen in Azure AD, moet u herkennen in de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voor het herkennen van de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **herkennen**, selecteer **herkennen** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Herkennen in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met herkennen op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in herkennen tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met herkennen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer herkennen Single Sign-On](#configure-recognize-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak de testgebruiker herkennen](#create-recognize-test-user)**  : als u wilt een equivalent van Britta Simon in herkennen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met herkennen, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **herkennen** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, hebt u **Service Provider-bestand met metagegevens**, voer de volgende stappen uit:

    >[!NOTE]
    >Krijgt u de **Service Provider-bestand met metagegevens** uit de **configureren herkennen Single Sign-On** gedeelte van de zelfstudie.

    a. Klik op **Metagegevensbestand uploaden**.

    ![Metagegevensbestand uploaden](common/upload-metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Metagegevensbestand kiezen](common/browse-upload-metadata.png)

    c. Nadat het bestand met metagegevens is geüpload, de **id** waarde krijgen automatisch ingevuld in de sectie SAML-basisconfiguratie.

    ![Domein en URL's één aanmelding informatie herkennen](common/sp-identifier.png)

     In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://recognizeapp.com/<your-domain>/saml/sso`

    > [!Note]
    > Als de **id** waarde krijg niet automatisch ingevuld, krijgt u de id-waarde door het openen van de metagegevens van de URL van de Provider uit de sectie SSO-instellingen die wordt later in de **één voor het herkennen van configureren Aanmeldings-** gedeelte van de zelfstudie. De waarde voor de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Client herkent ondersteuningsteam](mailto:support@recognizeapp.com) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **herkennen instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-recognize-single-sign-on"></a>Configureren van eenmalige aanmelding herkennen

1. Meld u aan uw tenant herkennen als een beheerder in een ander browservenster.

2. Klik in de rechterbovenhoek op **Menu**. Ga naar **beheerder van de bedrijfsportal**.
   
    ![Eenmalige aanmelding in de app configureren](./media/recognize-tutorial/tutorial_recognize_000.png)

3. Klik in het linkernavigatiedeelvenster op **Instellingen**.
   
    ![Eenmalige aanmelding in de app configureren](./media/recognize-tutorial/tutorial_recognize_001.png)

4. Voer de volgende stappen uit op **SSO-instellingen** sectie.
   
    ![Eenmalige aanmelding in de app configureren](./media/recognize-tutorial/tutorial_recognize_002.png)
    
    a. Als **SSO inschakelen**, selecteer **ON**.

    b. In de **IDP entiteit-ID** tekstvak, plak de waarde van **Azure AD-id** die u hebt gekopieerd vanuit Azure portal.
    
    c. In de **doel-url voor eenmalige aanmelding** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.
    
    d. In de **Slo-doel-url** tekstvak, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal. 
    
    e. Open uw gedownloade **certificaat (Base64)** -bestand in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **certificaat** tekstvak.
    
    f. Klik op de **instellingen opslaan** knop. 

5. Naast de **SSO-instellingen** sectie, Kopieer de URL onder **Service Provider metagegevens-url**.
   
    ![Eenmalige aanmelding in de app configureren](./media/recognize-tutorial/tutorial_recognize_003.png)

6. Open de **metagegevens-URL-koppeling** onder een lege browser voor het downloaden van het document met metagegevens. Vervolgens kopieert u de value(entityID) EntityDescriptor uit het bestand en plak deze in **id** -tekstvak in **SAML-basisconfiguratie** in Azure portal.
    
    ![Eenmalige aanmelding in de app configureren](./media/recognize-tutorial/tutorial_recognize_004.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te herkennen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **herkennen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **herkennen**.

    ![De koppeling herkennen in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-recognize-test-user"></a>Herkennen testgebruiker maken

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij herkennen, moeten ze worden ingericht voor herkennen. In het geval van herkennen is inrichten een handmatige taak.

Deze app biedt geen ondersteuning voor SCIM wordt ingericht, maar heeft een andere gebruiker synchronisatie die gebruikers inricht. 

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw bedrijf herkennen site als beheerder.

2. Klik in de rechterbovenhoek op **Menu**. Ga naar **beheerder van de bedrijfsportal**.

3. Klik in het linkernavigatiedeelvenster op **Instellingen**.

4. Voer de volgende stappen uit op **gebruiker synchronisatie** sectie.
   
    ![New User](./media/recognize-tutorial/tutorial_recognize_005.png "New User")
   
    a. Als **synchronisatie ingeschakeld**, selecteer **ON**.
   
    b. Als **kiezen synchronisatie provider**, selecteer **Microsoft / Office 365**.
   
    c. Klik op **gebruiker synchronisatie uitvoeren**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel herkennen in het toegangsvenster, moet u worden automatisch aangemeld bij het herkennen waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

