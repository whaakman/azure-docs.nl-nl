---
title: 'Zelfstudie: Azure Active Directory-integratie met Displayr | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Displayr.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b739b4e3-1a37-4e3c-be89-c3945487f4c1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdb4cc26a435ba161532b324ae5a04fed8eb9437
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158404"
---
# <a name="tutorial-azure-active-directory-integration-with-displayr"></a>Zelfstudie: Azure Active Directory-integratie met Displayr

In deze zelfstudie leert u hoe u Displayr integreren met Azure Active Directory (Azure AD).
Displayr integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Displayr heeft.
* U kunt uw gebruikers worden automatisch aangemeld Displayr (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Displayr, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Displayr ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Displayr **SP** gestart door SSO

## <a name="adding-displayr-from-the-gallery"></a>Displayr uit de galerie toe te voegen

Voor het configureren van de integratie van Displayr in Azure AD, moet u Displayr uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Displayr uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Displayr**, selecteer **Displayr** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Displayr in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Displayr op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Displayr tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Displayr, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Displayr](#configure-displayr-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Displayr](#create-displayr-test-user)**  : als u wilt een equivalent van Britta Simon in Displayr die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Displayr, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Displayr** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stap uit:

    ![Displayr domein en URL's, eenmalige aanmelding informatie](common/sp-intiated.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL: `https://app.displayr.com/Login`

5. In de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **certificaat (Raw)** uit de opgegeven opties aan de hand van uw behoeften en bewaar deze op uw computer.

    ![De link om het certificaat te downloaden](common/certificateraw.png)

6. Displayr toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram **Bewerken** om het dialoogvenster Gebruikerskenmerken te openen.

    ![image](common/edit-attribute.png)

7. Bovendien hierboven verwacht Displayr toepassing paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de **gebruikerskenmerken en Claims** sectie op de **groepclaims (Preview)** dialoogvenster, voer de volgende stappen uit:

    a. Klik op de **pen** naast **groepen die worden geretourneerd in de claim**.

    ![image](./media/displayr-tutorial/config04.png)

    ![image](./media/displayr-tutorial/config05.png)

    b. Selecteer **alle groepen** uit de lijst met keuzerondjes.

    c. Selecteer **bronkenmerk** van **groeps-ID**.

    d. Controleer **aanpassen van de naam van de groepclaim**.

    e. Controleer **groepen als rolclaims verzenden**.

    f. Klik op **Opslaan**.

8. Op de **Set-up Displayr** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-displayr-single-sign-on"></a>Displayr voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij Displayr als beheerder.

2. Klik op **instellingen** navigeer vervolgens naar **Account**.

    ![Configuratie](./media/displayr-tutorial/config01.png)

3. Schakel over naar **instellingen** in het bovenste menu en schuif omlaag in de pagina voor het klikken op **configureren van eenmalige aanmelding op (SAML)**.

    ![Configuratie](./media/displayr-tutorial/config02.png)

4. Op de **eenmalige aanmelding op (SAML)** pagina, voert u de volgende stappen uit:

    ![Configuratie](./media/displayr-tutorial/config03.png)

    a. Controleer de **inschakelen eenmalige aanmelding op (SAML)** vak.

    b. In de **verlener** tekst vak, plak de waarde van **Azure AD-id**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **aanmeldings-URL** tekst vak, plak de waarde van **aanmeldings-URL**, die u hebt gekopieerd vanuit Azure portal.

    d. In de **afmeldings-URL van** tekst vak, plak de waarde van **afmeldings-URL van**, die u hebt gekopieerd vanuit Azure portal.

    e. Het certificaat (Raw) openen in Kladblok, Kopieer de inhoud en plak deze in de **certificaat** in het tekstvak.

    f. **Groep toewijzingen** zijn optioneel.

    g. Klik op **Opslaan**.  

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Displayr.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Displayr**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Displayr**.

    ![De koppeling Displayr in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst Gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-displayr-test-user"></a>Displayr testgebruiker maken

Als u wilt dat Azure AD-gebruikers, meld u aan in op Displayr, ze moeten worden ingericht voor Displayr. In Displayr is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij Displayr als beheerder.

2. Klik op **instellingen** navigeer vervolgens naar **Account**.

    ![Displayr configuratie](./media/displayr-tutorial/config01.png)

3. Schakel over naar **instellingen** in het bovenste menu en schuif omlaag in de pagina totdat **gebruikers** sectie en vervolgens klikt op **nieuwe gebruiker**.

    ![Displayr configuratie](./media/displayr-tutorial/config07.png)

4. Op de **nieuwe gebruiker** pagina, voert u de volgende stappen uit:

    ![Displayr configuratie](./media/displayr-tutorial/config06.png)

    a. In **naam** tekst voert u de naam van gebruiker, zoals **Brittasimon**.

    b. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals `Brittasimon@contoso.com`.

    c. Selecteer de juiste **groepslidmaatschap**.

    d. Klik op **Opslaan**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Displayr in het toegangsvenster, moet u worden automatisch aangemeld bij de Displayr waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

