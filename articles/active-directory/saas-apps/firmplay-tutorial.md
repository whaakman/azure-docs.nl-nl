---
title: 'Zelfstudie: Azure Active Directory-integratie met FirmPlay - werknemer Advocacy voor werving | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FirmPlay - ondersteuning voor werving werknemer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: ee638e34a2db2c397b7f536dfe80c6ea08d7738c
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543633"
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Zelfstudie: Azure Active Directory-integratie met FirmPlay - ondersteuning voor werving werknemer

In deze zelfstudie leert u hoe u integreert FirmPlay - werknemer Advocacy voor werving met Azure Active Directory (Azure AD).
Integratie van FirmPlay - werknemer Advocacy voor werving met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot FirmPlay - ondersteuning voor werving werknemer heeft.
* U kunt uw gebruikers worden automatisch aangemeld FirmPlay - ondersteuning voor werving (Single Sign-On) met hun Azure AD-accounts werknemer kunt inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FirmPlay - werknemer Advocacy voor werving, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* FirmPlay - ondersteuning voor eenmalige aanmelding werving werknemer ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* FirmPlay - werknemer Advocacy voor werving ondersteunt **SP** gestart door SSO

## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a>FirmPlay - werknemer Advocacy voor werving uit de galerie toevoegen

Voor het configureren van de integratie van FirmPlay - werknemer Advocacy voor werving in Azure AD, moet u FirmPlay - werknemer Advocacy voor werving uit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen, FirmPlay - werknemer Advocacy voor werving uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **FirmPlay - werknemer Advocacy voor werving**, selecteer **FirmPlay - werknemer Advocacy voor werving** van resultaat deelvenster klik vervolgens op **toevoegen** knop de toepassing toevoegen.

     ![FirmPlay - werknemer Advocacy voor werving in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met FirmPlay - ondersteuning voor werving werknemer op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding in om te werken, een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in FirmPlay - moet werknemer Advocacy voor werving tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met FirmPlay - werknemer Advocacy voor werving, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer FirmPlay - werknemer Advocacy voor werving Single Sign-On](#configure-firmplay---employee-advocacy-for-recruiting-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak FirmPlay - ondersteuning voor de testgebruiker werving werknemer](#create-firmplay---employee-advocacy-for-recruiting-test-user)**  - dat een equivalent van Britta Simon in FirmPlay - werknemer ondersteuning voor het werven van die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met FirmPlay - werknemer Advocacy voor werving, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **FirmPlay - werknemer Advocacy voor werving** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![FirmPlay - werknemer Advocacy voor werven van domein en URL's één aanmeldings-informatie](common/sp-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<your-subdomain>.firmplay.com/`

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [FirmPlay - werknemer Advocacy voor werven van Client-ondersteuningsteam](mailto:engineering@firmplay.com) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **FirmPlay - werknemer Advocacy voor werving instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-firmplay---employee-advocacy-for-recruiting-single-sign-on"></a>FirmPlay - werknemer ondersteuning voor het werven van eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op **FirmPlay - werknemer Advocacy voor werving** zijde, moet u voor het verzenden van de gedownloade **certificaat (Base64)** en de juiste URL's opgehaald uit Azure portal om te [ FirmPlay - ondersteuning voor het ondersteuningsteam werving werknemer](mailto:engineering@firmplay.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan FirmPlay - werknemer Advocacy voor werving.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **FirmPlay - werknemer Advocacy voor werving**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **FirmPlay - werknemer Advocacy voor werving**.

    ![De FirmPlay - werknemer Advocacy voor werving koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-firmplay---employee-advocacy-for-recruiting-test-user"></a>FirmPlay - werknemer Advocacy voor werving testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in FirmPlay - ondersteuning voor werving werknemer. Werken met [FirmPlay - ondersteuning voor het ondersteuningsteam werving werknemer](mailto:engineering@firmplay.com) om toe te voegen de gebruikers in de FirmPlay - werknemer Advocacy voor werving platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de FirmPlay - werknemer Advocacy werving tegel in het toegangsvenster, u moet worden automatisch aangemeld bij de FirmPlay - werknemer Advocacy voor werving voor dat u een SSO instellen. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

