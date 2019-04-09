---
title: 'Zelfstudie: Azure Active Directory-integratie met Work.com | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Work.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9a978c8e32acb504ac97e3ca039deb8906e1543
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274430"
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Zelfstudie: Azure Active Directory-integratie met Work.com

In deze zelfstudie leert u hoe u Work.com integreren met Azure Active Directory (Azure AD).
Work.com integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Work.com heeft.
* U kunt uw gebruikers worden automatisch aangemeld Work.com (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Work.com, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding Work.com ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Work.com **SP** gestart door SSO

## <a name="adding-workcom-from-the-gallery"></a>Work.com uit de galerie toe te voegen

Voor het configureren van de integratie van Work.com in Azure AD, moet u Work.com uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Work.com uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Work.com**, selecteer **Work.com** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Work.com in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Work.com op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Work.com tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Work.com, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Work.com](#configure-workcom-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Work.com](#create-workcom-test-user)**  : als u wilt een equivalent van Britta Simon in Work.com die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

>[!NOTE]
>Voor het configureren van eenmalige aanmelding, moet u een aangepaste domeinnaam van Work.com nog instellen. U moet ten minste een domeinnaam te definiëren, de domeinnaam van uw testen en deze implementeren in uw hele organisatie.

Voor het configureren van Azure AD eenmalige aanmelding met Work.com, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Work.com** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Work.com domein en URL's, eenmalige aanmelding informatie](common/sp-signonurl.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `http://<companyname>.my.salesforce.com`

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Work.com Client ondersteuningsteam](https://help.salesforce.com/articleView?id=000159855&type=3) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **Work.com instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-workcom-single-sign-on"></a>Work.com voor eenmalige aanmelding configureren

1. Aanmelden bij uw tenant Work.com als administrator.

2. Ga naar **Setup**.
   
    ![Setup](./media/work-com-tutorial/ic794108.png "Setup")

3. In het navigatiedeelvenster links in de **beheren** sectie, klikt u op **domeinbeheer** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein** openen de **Mijn domein** pagina. 
   
    ![My Domain](./media/work-com-tutorial/ic767825.png "My Domain")

4. Om te bevestigen dat uw domein correct is ingesteld, zorg ervoor dat deze zich op "**stap 4 geïmplementeerd naar gebruikers**' en bekijk uw"**mijn domeininstellingen**'.
   
    ![Domein geïmplementeerd voor gebruiker](./media/work-com-tutorial/ic784377.png "domein geïmplementeerd voor gebruiker")

5. Aanmelden bij uw tenant Work.com.

6. Ga naar **Setup**.
    
    ![Setup](./media/work-com-tutorial/ic794108.png "Setup")

7. Vouw de **beveiligingsmechanismen** menu en klik vervolgens op **instellingen voor eenmalige aanmelding**.
    
    ![Instellingen voor eenmalige aanmelding](./media/work-com-tutorial/ic794113.png "Instellingen voor eenmalige aanmelding")

8. Op de **instellingen voor eenmalige aanmelding** dialoogvenster pagina, voert u de volgende stappen uit:
    
    ![SAML ingeschakeld](./media/work-com-tutorial/ic781026.png "SAML ingeschakeld")
    
    a. Selecteer **SAML Enabled**.
    
    b. Klik op **Nieuw**.

9. In de **eenmalige SAML-aanmelding instellingen** sectie, voert u de volgende stappen uit:
    
    ![Eenmalige SAML-aanmelding instelling](./media/work-com-tutorial/ic794114.png "eenmalige SAML-aanmelding instellen")
    
    a. Typ in het tekstvak **Name** een naam voor de configuratie.  
       
    > [!NOTE]
    > Een waarde voor **naam** automatisch ingevuld de **API-naam** tekstvak.
    
    b. In **verlener** tekstvak, plak de waarde van **Azure AD-id** die u hebt gekopieerd vanuit Azure portal.
    
    c. Als u wilt uploaden van het gedownloade certificaat vanuit Azure portal, klikt u op **Bladeren**.
    
    d. In de **entiteit-Id** tekstvak, type `https://salesforce-work.com`.
    
    e. Als **SAML identiteitstype**, selecteer **verklaring bevat de Federation-ID van het gebruikersobject**.
    
    f. Als **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentfier van het onderwerp overzicht**.
    
    g. In **aanmeldings-URL van id-Provider** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    h. In **afmeldings-URL van id-Provider** tekstvak, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal.
    
    i. Als **Service Provider gestart aanvragen Binding**, selecteer **HTTP Post**.
    
    j. Klik op **Opslaan**.

10. Klik in de klassieke portal van Work.com in het linkernavigatiedeelvenster op **domeinbeheer** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein** openen de **mijn domein** de pagina. 
    
    ![My Domain](./media/work-com-tutorial/ic794115.png "My Domain")

11. Op de **mijn domein** pagina, in de **aanmelden pagina huisstijl** sectie, klikt u op **bewerken**.
    
    ![Login Page Branding](./media/work-com-tutorial/ic767826.png "Login Page Branding")

12. Op de **aanmelden pagina huisstijl** pagina, in de **verificatieservice** sectie, de naam van uw **SAML SSO-instellingen** wordt weergegeven. Selecteer deze en klik vervolgens op **opslaan**.
    
    ![Login Page Branding](./media/work-com-tutorial/ic784366.png "Login Page Branding")

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Work.com.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Work.com**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Work.com**.

    ![De koppeling Work.com in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-workcom-test-user"></a>Work.com testgebruiker maken

Voor Azure Active Directory-gebruikers kunnen zich aanmelden, moeten ze worden ingericht op Work.com. In het geval van Work.com is inrichten een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voer de volgende stappen uit om de inrichting van gebruikers te configureren:

1. Meld u aan bij uw bedrijf Work.com site als beheerder.

2. Ga naar **Setup**.
   
    ![Setup](./media/work-com-tutorial/IC794108.png "Setup")

3. Ga naar **gebruikers beheren \> gebruikers**.
   
    ![Gebruikers beheren](./media/work-com-tutorial/IC784369.png "Gebruikers beheren")

4. Klik op **New User**.
   
    ![Alle gebruikers](./media/work-com-tutorial/IC794117.png "alle gebruikers")

5. Uitvoeren in de sectie gebruikers bewerken in de volgende stappen in de kenmerken van een geldige Azure AD-account die u inrichten in de bijbehorende tekstvakken wilt:
   
    ![Gebruiker bewerken](./media/work-com-tutorial/ic794118.png "gebruiker bewerken")
   
    a. In de **voornaam** tekstvak, type de **voornaam** van de gebruiker **Julia**.
    
    b. In de **achternaam** tekstvak, type de **achternaam** van de gebruiker **Simon**.
    
    c. In de **Alias** tekstvak, type de **naam** van de gebruiker **BrittaS**.
    
    d. In de **e** tekstvak, type de **e-mailadres** van gebruiker Brittasimon@contoso.com.
    
    e. In de **gebruikersnaam** tekstvak, typ een naam van gebruiker, zoals Brittasimon@contoso.com.
    
    f. In de **bijnaam** tekstvak, typ een **bijnaam** van gebruiker **Simon**.
    
    g. Selecteer **rol**, **gebruikerslicentie**, en **profiel**.
    
    h. Klik op **Opslaan**.  
      
    > [!NOTE]
    > De houder van Azure AD-account ontvangt een e-mailbericht ook een koppeling voor het account te bevestigen voordat deze geactiveerd wordt.
    > 

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Work.com in het toegangsvenster, moet u worden automatisch aangemeld bij de Work.com waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

