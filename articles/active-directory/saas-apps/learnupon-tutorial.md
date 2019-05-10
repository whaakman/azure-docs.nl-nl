---
title: 'Zelfstudie: Azure Active Directory-integratie met LearnUpon | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LearnUpon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 574c21dc2713f10513ac296e7db538e20a94c9d6
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406532"
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Zelfstudie: Azure Active Directory-integratie met LearnUpon

In deze zelfstudie leert u hoe u LearnUpon integreren met Azure Active Directory (Azure AD).
LearnUpon integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot LearnUpon heeft.
* U kunt uw gebruikers worden automatisch aangemeld LearnUpon (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LearnUpon, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding LearnUpon ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.


* Biedt ondersteuning voor LearnUpon **IDP** gestart door SSO

* Biedt ondersteuning voor LearnUpon **Just In Time** inrichten van gebruikers


## <a name="adding-learnupon-from-the-gallery"></a>LearnUpon uit de galerie toe te voegen

Voor het configureren van de integratie van LearnUpon in Azure AD, moet u LearnUpon uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LearnUpon uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **LearnUpon**, selecteer **LearnUpon** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![LearnUpon in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met LearnUpon op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LearnUpon tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met LearnUpon, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van eenmalige aanmelding LearnUpon](#configure-learnupon-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker LearnUpon](#create-learnupon-test-user)**  : als u wilt een equivalent van Britta Simon in LearnUpon die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met LearnUpon, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **LearnUpon** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![LearnUpon domein en URL's, eenmalige aanmelding informatie](common/idp-reply.png)

    In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke antwoord-URL. Neem contact op met [LearnUpon Client ondersteuningsteam](https://www.learnupon.com/features/support/) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de **instellen van eenmalige aanmelding met SAML** pagina, Ga naar de **VINGERAFDRUK** -dit wordt toegevoegd aan uw LearnUpon SAML-instellingen.

    ![De downloadkoppeling certificaat](common/certificateraw.png)

6. Op de **LearnUpon instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-learnupon-single-sign-on"></a>LearnUpon voor eenmalige aanmelding configureren

1. Open een ander browserexemplaar en zich aanmeldt bij LearnUpon met een administrator-account.

1. Klik op de **instellingen** tabblad.

    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_06.png)

1. Klik op **Single Sign On - SAML**, en klik vervolgens op **algemene instellingen** om SAML-instellingen te configureren.
   
    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_07.png) 

1. In de **algemene instellingen** sectie, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. Selecteer **ingeschakeld**.

    b. Selecteer **versie** als **2.0**.

    c. Selecteer **overslaan voorwaarden** als **Nee**.

    d. In de **parameternaam SAML-Token plaatsen** tekstvak, type de naam van de aanvraagparameter-bericht naar de URL voor SAML-consument hierboven aangegeven dat de SAML-verklaring worden geverifieerd en geverifieerd - bijvoorbeeld bevat **SAMLResponse** .

    e. In de **indeling van de id** tekstvak, type de waarde die waar in de SAML-verklaring (e-mailadres) van de gebruikers-id aangeeft bevindt zich - bijvoorbeeld `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.
  
    f. In de **Providerlocatie identificeren** tekstvak typt u de waarde die waar de gebruikers om te worden verzonden aangeeft als ze op het pictogram van uw geüploade vanuit het aanmeldingsscherm van de Azure portal.
  
    g. In de **afmeldings-URL** tekstvak, plak de **afmeldings-URL van** waarde die u hebt gekopieerd vanuit Azure portal.

    h. Klik op **vinger afdrukken bestellen beheren**, en vervolgens de vingerafdruk van het gedownloade certificaat te uploaden.

1. Klik op **gebruikersinstellingen**, en voer de volgende stappen uit:

     ![Eenmalige aanmelding configureren](./media/learnupon-tutorial/tutorial_learnupon_11.png)  

    a. In de **voornaam id indeling** tekstvak, type de waarde die kan worden achterhaald terwijl in het SAML-verklaring de voornaam van de gebruikers zich - bijvoorbeeld bevindt: `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
  
    b. In de **indeling van de laatste id** tekstvak, type de waarde die kan worden achterhaald terwijl in het SAML-verklaring de achternaam van de gebruikers zich - bijvoorbeeld bevindt: `https://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan LearnUpon.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **LearnUpon**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **LearnUpon**.

    ![De koppeling LearnUpon in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-learnupon-test-user"></a>LearnUpon testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in LearnUpon. LearnUpon biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in LearnUpon bestaat, wordt een nieuw gemaakt nadat verificatie. Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact opnemen met [LearnUpon ondersteuningsteam](https://www.learnupon.com/features/support/).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel LearnUpon in het toegangsvenster, moet u worden automatisch aangemeld bij de LearnUpon waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)