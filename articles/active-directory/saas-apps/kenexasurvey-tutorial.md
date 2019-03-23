---
title: 'Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en IBM Kenexa enquête Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/08/2019
ms.author: jeedes
ms.openlocfilehash: ed34267aa1f18b4c66fe841164e6a2cde4e27d47
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361044"
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête Enterprise

In deze zelfstudie leert u hoe u IBM Kenexa enquête Enterprise integreren met Azure Active Directory (Azure AD).
IBM Kenexa enquête Enterprise integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot IBM Kenexa enquête Enterprise heeft.
* U kunt uw gebruikers worden automatisch aangemeld met IBM Kenexa enquête Enterprise (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met IBM Kenexa enquête Enterprise, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* IBM Kenexa enquête ondernemingsbrede eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor IBM Kenexa enquête Enterprise **IDP** gestart door SSO

## <a name="adding-ibm-kenexa-survey-enterprise-from-the-gallery"></a>IBM Kenexa enquête Enterprise uit de galerie toe te voegen

Voor het configureren van de integratie van IBM Kenexa enquête Enterprise in Azure AD, moet u IBM Kenexa enquête Enterprise uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen IBM Kenexa enquête Enterprise uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **IBM Kenexa enquête Enterprise**, selecteer **IBM Kenexa enquête Enterprise** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![IBM Kenexa enquête onderneming in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met IBM Kenexa enquête Enterprise op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in IBM Kenexa enquête onderneming tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met IBM Kenexa enquête Enterprise, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van IBM Kenexa enquête Enterprise Single Sign-On](#configure-ibm-kenexa-survey-enterprise-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[IBM Kenexa enquête Enterprise testgebruiker maken](#create-ibm-kenexa-survey-enterprise-test-user)**  : als u wilt een equivalent van Britta Simon in IBM Kenexa enquête onderneming die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met IBM Kenexa enquête Enterprise, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **IBM Kenexa enquête Enterprise** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de pagina **Eenmalige aanmelding instellen met SAML** voert u de volgende stappen uit:

    ![IBM Kenexa enquête Enterprise domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://surveys.kenexa.com/<companycode>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Neem contact op met [IBM Kenexa enquête Enterprise Client-ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. De bedrijfstoepassing IBM Kenexa enquête wordt verwacht dat de asserties Security Asserties Markup Language (SAML) ontvangen in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de kenmerken van SAML-token is vereist. De waarde van de claim van de gebruiker-id in het antwoord moet overeenkomen met de SSO-ID die geconfigureerd in het systeem Kenexa. Als u wilt toewijzen op de juiste gebruikers-id in uw organisatie als SSO Internet Datagram Protocol (IDP), werken met de [IBM Kenexa enquête Enterprise-ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw).

    Standaard wordt de gebruikers-id in Azure AD ingesteld als de waarde van gebruiker UPN (User Principal Name). U kunt deze waarde wijzigen op de **gebruikerskenmerken** tabblad, zoals wordt weergegeven in de volgende schermafbeelding. De integratie werkt pas nadat u de toewijzing hebt voltooid, juist.

    ![image](common/edit-attribute.png)

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

7. Op de **instellen van IBM Kenexa enquête Enterprise** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-ibm-kenexa-survey-enterprise-single-sign-on"></a>IBM Kenexa enquête Enterprise eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op **IBM Kenexa enquête Enterprise** zijde, moet u voor het verzenden van de gedownloade **certificaat (Base64)** en toepassing van de gekopieerde URL's van Azure portal om te [IBM Kenexa Enterprise-ondersteuningsteam enquête](https://www.ibm.com/support/home/?lnk=fcw). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot IBM Kenexa enquête onderneming.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **IBM Kenexa enquête Enterprise**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **IBM Kenexa enquête Enterprise**.

    ![De IBM Kenexa enquête Enterprise-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-ibm-kenexa-survey-enterprise-test-user"></a>IBM Kenexa enquête Enterprise testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in IBM Kenexa enquête onderneming.

Gebruikers in het systeem IBM Kenexa enquête Enterprise maken en de SSO-ID voor hen toewijzen, kunt u werken met de [IBM Kenexa enquête Enterprise-ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw). Deze SSO-ID-waarde moet ook worden toegewezen aan de gebruiker-id-waarde van Azure AD. U kunt deze standaardinstelling wijzigen op de **kenmerk** tabblad.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel IBM Kenexa enquête Enterprise in het toegangsvenster, moet u worden automatisch aangemeld bij de IBM Kenexa enquête onderneming waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

