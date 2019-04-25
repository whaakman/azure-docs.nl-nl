---
title: 'Zelfstudie: Azure Active Directory-integratie met oneindige Campus | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en oneindige Campus.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3995b544-e751-4e0f-ab8b-c9a3862da6ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 91f1d7151debb1f1f3a562337d6c37e4f63a7ee7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60273439"
---
# <a name="tutorial-azure-active-directory-integration-with-infinite-campus"></a>Zelfstudie: Azure Active Directory-integratie met oneindige Campus

In deze zelfstudie leert u hoe u oneindige Campus integreren met Azure Active Directory (Azure AD).
Oneindige Campus integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot oneindige Campus heeft.
* U kunt uw gebruikers worden automatisch aangemeld oneindige Campus (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met oneindige Campus, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Oneindige Campus eenmalige aanmelding ingeschakeld abonnement
* Ten minste moet u een Azure Active Directory-beheerder, en over de beveiligingsrol van een Campus-Product van "Student Information System (SIS)" om de configuratie te voltooien.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor oneindige Campus **SP** gestart door SSO

## <a name="adding-infinite-campus-from-the-gallery"></a>Oneindige Campus uit de galerie toe te voegen

Voor het configureren van de integratie van oneindige Campus in Azure AD, moet u oneindige Campus uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen oneindige Campus uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, klikt u op de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **oneindige Campus**, selecteer **oneindige Campus** vanuit het deelvenster resultaat klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Oneindige Campus in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met oneindige Campus op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in een oneindige Campus tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met oneindige Campus, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van oneindige Campus Single Sign-On](#configure-infinite-campus-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Oneindige Campus testgebruiker maken](#create-infinite-campus-test-user)**  : als u wilt een equivalent van Britta Simon in oneindige Campus die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met oneindige Campus, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **oneindige Campus** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. De volgende stappen uitvoeren in de sectie SAML-basisconfiguratie (Houd er rekening mee dat het domein met het Model die als host fungeert varieert, maar de **volledig gekwalificeerde domeinnaam** waarde moet overeenkomen met de installatie van oneindige Campus):

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<DOMAIN>.infinitecampus.com/campus/<DISTRICTNAME>`

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>`

    ![Oneindige Campus-domein en URL's, eenmalige aanmelding informatie](common/sp-identifier-reply.png)

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-infinite-campus-single-sign-on"></a>Oneindige Campus Single Sign-On configureren

1. In een ander browservenster aanmelden bij onbeperkte Campus als een beveiligingsbeheerder.

2. Aan de linkerkant van het menu, klikt u op **Systeembeheer**.

    ![De beheerder](./media/infinitecampus-tutorial/tutorial_infinitecampus_admin.png)

3. Navigeer naar **gebruikersbeveiliging** > **SAML Management** > **SSO-configuratie van serviceprovider**.

    ![De saml](./media/infinitecampus-tutorial/tutorial_infinitecampus_saml.png)

4. Op de **SSO-configuratie van serviceprovider** pagina, voert u de volgende stappen uit:

    ![De eenmalige aanmelding](./media/infinitecampus-tutorial/tutorial_infinitecampus_sso.png)

    a. Selecteer **inschakelen SAML-eenmalige aanmelding**.

    b. Bewerken de **optionele kenmerknaam** bevat **naam**

    c. Op de **een optie voor het ophalen van gegevens van de id-Provider (IDP)-server** sectie, selecteer **metagegevens-URL**, plak de **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd vanuit Azure portal in het vak en klik vervolgens op **synchronisatie**.

    d. Nadat u hebt geklikt **synchronisatie** de waarden ophalen automatisch ingevuld in **SSO-configuratie van serviceprovider** pagina. Deze waarden kunnen worden geverifieerd aan de waarden die in stap 4 hierboven.

    e. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld BrittaSimon@contoso.com.

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

> [!NOTE]
> Als u wilt dat al uw Azure-gebruikers eenmalige aanmelding hebben toegang tot onbeperkte Campus en afhankelijk zijn van oneindige Campus machtigingen voor interne systeem om toegang te beheren, kunt u instellen de **gebruiker toewijzing vereist** eigenschap van de toepassing op Nee en de volgende stappen overslaan.

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan oneindige Campus.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **oneindige Campus**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **oneindige Campus**.

    ![De oneindige Campus-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-infinite-campus-test-user"></a>Oneindige Campus testgebruiker maken

Oneindige Campus heeft een architectuur met demografische gegevens die zijn gericht. Neem contact op met [oneindige Campus-ondersteuningsteam](mailto:sales@infinitecampus.com) om toe te voegen de gebruikers in het oneindige Campus-platform.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel oneindige Campus in het toegangsvenster, moet u worden automatisch aangemeld bij de oneindige Campus waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
