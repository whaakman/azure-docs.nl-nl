---
title: 'Zelfstudie: Azure Active Directory-integratie met 10.000 ft plannen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en 10.000 ft-abonnementen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b60c955e-8fa3-4872-a897-c4e81fd7beac
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 441c9345f92974c387429baac768dbccb03fc995
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107611"
---
# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a>Zelfstudie: Azure Active Directory-integratie met 10.000 ft plannen

In deze zelfstudie leert u over het plannen van 10.000 ft integreren met Azure Active Directory (Azure AD).
10\.000 ft abonnementen integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot 10.000 ft-abonnementen heeft.
* U kunt uw gebruikers worden automatisch aangemeld 10.000 ft plannen (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met 10.000 ft plannen, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* 10\.000 ft plannen voor eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* ondersteuning voor het plannen van 10.000 ft **SP** gestart door SSO
* ondersteuning voor het plannen van 10.000 ft **Just In Time** inrichten van gebruikers

## <a name="adding-10000ft-plans-from-the-gallery"></a>Toevoegen van 10.000 ft plannen uit de galerie

Voor het configureren van de integratie van 10.000 ft-abonnementen in Azure AD, moet u 10.000 ft plannen uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen 10.000 ft plannen uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** klikt u in het navigatievenster aan de linkerkant op het pictogram **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, klikt u op de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **10.000 ft plannen**, selecteer **10.000 ft plannen** vanuit het deelvenster resultaat klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![10\.000 ft plannen in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met 10.000 ft plannen op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in 10.000 ft plannen tot stand is gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met 10.000 ft plannen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van 10000ft plannen Single Sign-On](#configure-10000ft-plans-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak 10000ft plannen testgebruiker](#create-10000ft-plans-test-user)**  : als u wilt een equivalent van Britta Simon in 10.000 ft plannen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met 10.000 ft plannen, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **10.000 ft plannen** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![10\.000 ft-abonnementen domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u de URL: `https://app.10000ft.com`

    b. Typ in het tekstvak **Id (Entiteits-id)** de volgende URL: `https://app.10000ft.com/saml/metadata`

    > [!NOTE]
    > De waarde voor **id** is anders hebt u een aangepast domein. Neem contact op met [10.000 ft Client plannen ondersteuningsteam](https://www.10000ft.com/plans/support) deze waarde op te halen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Raw)** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificateraw.png)

6. Op de **10.000 ft-abonnementen instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-10000ft-plans-single-sign-on"></a>10000ft plannen voor eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op **10.000 ft plannen** zijde, moet u voor het verzenden van de gedownloade **certificaat (Raw)** en toepassing van de gekopieerde URL's van Azure portal om te [ondersteuningsteam 10.000 ft-abonnementen ](https://www.10000ft.com/plans/support). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** veld, typt u `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot 10.000 ft plannen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **10.000 ft plannen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **10.000 ft plannen**.

    ![De koppeling voor het plannen van 10.000 ft in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst Gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-10000ft-plans-test-user"></a>Testgebruiker 10000ft-abonnementen maken

In deze sectie wordt een gebruiker met de naam Britta Simon in 10.000 ft-abonnementen gemaakt. 10\.000 ft plannen biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in 10.000 ft-abonnementen bestaat, wordt een nieuw gemaakt nadat verificatie.

> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [10.000 ft Client plannen ondersteuningsteam](https://www.10000ft.com/plans/support).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel voor het plannen van 10.000 ft in het toegangsvenster, moet u worden automatisch aangemeld bij de 10.000 ft-abonnementen waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
