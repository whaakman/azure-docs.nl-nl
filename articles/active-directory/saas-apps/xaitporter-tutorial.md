---
title: 'Zelfstudie: Azure Active Directory-integratie met XaitPorter | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en XaitPorter.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d33c7cb7-0550-425b-882a-619a713a71b7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2019
ms.author: jeedes
ms.openlocfilehash: ada93903113cedd68e23ec66a36dceef99ec1233
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65896164"
---
# <a name="tutorial-azure-active-directory-integration-with-xaitporter"></a>Zelfstudie: Azure Active Directory-integratie met XaitPorter

In deze zelfstudie leert u hoe u XaitPorter integreren met Azure Active Directory (Azure AD).
XaitPorter integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot XaitPorter heeft.
* U kunt uw gebruikers worden automatisch aangemeld XaitPorter (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met XaitPorter, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Eenmalige aanmelding XaitPorter ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor XaitPorter **SP** gestart door SSO

## <a name="adding-xaitporter-from-the-gallery"></a>XaitPorter uit de galerie toe te voegen

Voor het configureren van de integratie van XaitPorter in Azure AD, moet u XaitPorter uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen XaitPorter uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **XaitPorter**, selecteer **XaitPorter** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![XaitPorter in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met XaitPorter op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in XaitPorter tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met XaitPorter, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding XaitPorter](#configure-xaitporter-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker XaitPorter](#create-xaitporter-test-user)**  : als u wilt een equivalent van Britta Simon in XaitPorter die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met XaitPorter, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **XaitPorter** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![XaitPorter domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<subdomain>.xaitporter.com/saml/login`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<subdomain>.xaitporter.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [XaitPorter Client ondersteuningsteam](https://www.xait.com/support/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

6. Geef de **IP-adres** of de **App-Url voor federatieve metagegevens** naar de [SmartRecruiters ondersteuningsteam](https://www.smartrecruiters.com/about-us/contact-us/), zodat XaitPorter kunt controleren of het IP-adres dat bereikbaar is vanaf is uw Het exemplaar van de XaitPorter whitelist bij hun kant configureren. 

### <a name="configure-xaitporter-single-sign-on"></a>XaitPorter voor eenmalige aanmelding configureren

1. Voor het automatiseren van de configuratie in XaitPorter, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup XaitPorter** wordt u doorgeleid naar de toepassing XaitPorter. Geef de referenties van de beheerder zich aanmelden bij XaitPorter daar. De browserextensie wordt automatisch configureren van de toepassing voor u en stap 3-6 automatiseren.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u XaitPorter handmatig instellen wilt, opent u een nieuw browservenster en meld u in uw site van het bedrijf XaitPorter als beheerder en voer de volgende stappen uit:

4. Klik op **Admin**.

    ![Eenmalige aanmelding configureren](./media/xaitporter-tutorial/user1.png)

5. Selecteer **beheren Single Sign-On** uit de **Setup van System** vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/xaitporter-tutorial/user2.png)

6. In de **beheren van EENMALIGE aanmelding** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/xaitporter-tutorial/user3.png)

    a. Selecteer **verificatie met eenmalige aanmelding inschakelen**.

    b. In **instellingen van de identiteit** tekstvak plakken **App-Url voor federatieve metagegevens** die u hebt gekopieerd uit de Azure-portal en op **ophalen**.

    c. Selecteer **Autocreation van gebruikers inschakelen**.

    d. Klik op **OK**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan XaitPorter.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **XaitPorter**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **XaitPorter**.

    ![De koppeling XaitPorter in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-xaitporter-test-user"></a>XaitPorter testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in XaitPorter. Werken met [XaitPorter Client ondersteuningsteam](https://www.xait.com/support/) om toe te voegen de gebruikers in het XaitPorter-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel XaitPorter in het toegangsvenster, moet u worden automatisch aangemeld bij de XaitPorter waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

