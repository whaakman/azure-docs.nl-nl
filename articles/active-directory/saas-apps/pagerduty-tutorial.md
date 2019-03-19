---
title: 'Zelfstudie: Azure Active Directory-integratie met PagerDuty | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en PagerDuty.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 5470c13f75d010634f97e87dc1a870a100187973
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57835064"
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Zelfstudie: Azure Active Directory-integratie met PagerDuty

In deze zelfstudie leert u hoe u PagerDuty integreren met Azure Active Directory (Azure AD).
PagerDuty integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot PagerDuty heeft.
* U kunt uw gebruikers worden automatisch aangemeld PagerDuty (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het Azure AD-integratie configureren met PagerDuty, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* PagerDuty eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor PagerDuty **SP** gestart door SSO

## <a name="adding-pagerduty-from-the-gallery"></a>PagerDuty uit de galerie toe te voegen

Voor het configureren van de integratie van PagerDuty in Azure AD, moet u PagerDuty uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen PagerDuty uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **PagerDuty**, selecteer **PagerDuty** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![PagerDuty in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met PagerDuty op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in PagerDuty tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met PagerDuty, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding PagerDuty](#configure-pagerduty-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[PagerDuty testgebruiker maken](#create-pagerduty-test-user)**  : als u wilt een equivalent van Britta Simon in PagerDuty die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Azure AD eenmalige aanmelding configureren met PagerDuty, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **PagerDuty** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![PagerDuty-domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<tenant-name>.pagerduty.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<tenant-name>.pagerduty.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Neem contact op met [PagerDuty-Client-ondersteuningsteam](https://www.pagerduty.com/support/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **PagerDuty instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-pagerduty-single-sign-on"></a>PagerDuty voor eenmalige aanmelding configureren

1. Meld u in een ander browservenster in uw bedrijf Pagerduty site als beheerder.

2. Klik in het menu aan de bovenkant op **Accountinstellingen**.

    ![Account Settings](./media/pagerduty-tutorial/ic778535.png "Account Settings")

3. Klik op **Eenmalige aanmelding**.

    ![Single sign-on](./media/pagerduty-tutorial/ic778536.png "Single sign-on")

4. Op de **Schakel eenmalige aanmelding (SSO)** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding inschakelen](./media/pagerduty-tutorial/ic778537.png "eenmalige aanmelding inschakelen")

    a. Open uw base-64 gecodeerde certificaat gedownload vanuit Azure portal in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** tekstvak
  
    b. Plak in het tekstvak **Aanmeldings-URL** de **aanmeldings-URL** die u in de Azure-portal hebt gekopieerd.
  
    c. Plak in het tekstvak **Afmeldings-URL** de **afmeldings-URL** die u in de Azure-portal hebt gekopieerd.

    d. Selecteer **toestaan gebruikersnaam en wachtwoord aanmelden**.

    e. Selecteer **EXACTE vereist verificatie context vergelijking** selectievakje.

    f. Klik op **Wijzigingen opslaan**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan PagerDuty.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **PagerDuty**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **PagerDuty**.

    ![De PagerDuty-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-pagerduty-test-user"></a>PagerDuty testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich aanmelden bij PagerDuty, moeten ze worden ingericht voor PagerDuty.  
In het geval van PagerDuty is inrichten een handmatige taak.

>[!NOTE]
>U kunt elke andere Pagerduty gebruiker account hulpmiddelen voor het maken of API's geleverd door Pagerduty voor het inrichten van Azure Active Directory-gebruikersaccounts.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **Pagerduty** tenant.

2. Klik in het menu aan de bovenkant op **gebruikers**.

3. Klik op **gebruikers toevoegen**.
   
    ![Gebruikers toevoegen](./media/pagerduty-tutorial/ic778539.png "gebruikers toevoegen")

4.  Op de **uitnodigen van uw team** dialoogvenster, voer de volgende stappen uit:
   
    ![Uw team uitnodigen](./media/pagerduty-tutorial/ic778540.png "uitnodigen van uw team")

    a. Type de **eerste en laatste naam** van gebruiker, zoals **Britta Simon**. 
   
    b. Voer **e** -adres van de gebruiker, zoals **brittasimon\@contoso.com**.
   
    c. Klik op **toevoegen**, en klik vervolgens op **uitnodigingen verzenden**.
   
    >[!NOTE]
    >Alle toegevoegde gebruikers ontvangen een uitnodiging om een PagerDuty-account te maken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel PagerDuty in het toegangsvenster, moet u worden automatisch aangemeld bij de PagerDuty waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

