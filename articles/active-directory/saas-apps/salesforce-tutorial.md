---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.openlocfilehash: 5be86017926e09b5db5fd08f1c33d316b9031bc4
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54264288"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce

In deze zelfstudie leert u hoe u Salesforce integreert met Azure Active Directory (Azure AD).
Het integreren van Salesforce met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot Salesforce.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Salesforce (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Salesforce hebt u de volgende zaken nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Salesforce waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Salesforce biedt ondersteuning voor met **SP** geïnitieerde eenmalige aanmelding

* Salesforce biedt ondersteuning voor het **Just-In-Time** inrichten van gebruikers

* Salesforce biedt ondersteuning voor het [**automatisch** inrichten van gebruikers](salesforce-provisioning-tutorial.md)

## <a name="adding-salesforce-from-the-gallery"></a>Salesforce toevoegen vanuit de galerie

Voor het configureren van de integratie van Salesforce in Azure AD moet u Salesforce vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u Salesforce vanuit de galerie wilt toevoegen, voert u de volgende stappen uit:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Salesforce** in het zoekvak, selecteer **Salesforce** in het deelvenster met resultaten en klik op **Toevoegen** om de toepassing toe te voegen.

     ![Salesforce in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u eenmalige aanmelding van Azure AD configureren en testen met Salesforce op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Salesforce tot stand is gebracht.

Als u eenmalige aanmelding van Azure AD wilt configureren en testen met Salesforce, moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Salesforce-eenmalige aanmelding configureren](#configure-salesforce-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de toepassingszijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[Een Salesforce-testgebruiker maken](#create-salesforce-test-user)**: als u een equivalent van Britta Simon in Salesforce wilt hebben dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
5. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD-eenmalige aanmelding met Salesforce voert u de volgende stappen uit:

1. In [Azure Portal](https://portal.azure.com/) selecteert u op de integratiepagina van de **Salesforce**-toepassing de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over eenmalige aanmelding voor het Salesforce-domein en -URL's](common/sp-identifier.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met de volgende indeling:

    Bedrijfsaccount: `https://<subdomain>.my.salesforce.com`

    Ontwikkelaarsaccount: `https://<subdomain>-dev-ed.my.salesforce.com`

    b. Typ in het tekstvak **Id** een waarde met de volgende indeling:

    Bedrijfsaccount: `https://<subdomain>.my.salesforce.com`

    Ontwikkelaarsaccount: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en -id. Neem contact op met het [Salesforce-klantondersteuningsteam](https://help.salesforce.com/support) om deze waarden te verkrijgen.

5. Ga op de pagina **Eenmalige aanmelding met SAML instellen** naar de sectie **SAML-handtekeningcertificaat** en klik op **Downloaden** om het **XML-bestand met federatieve metagegevens** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en op te slaan op uw computer.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. In de sectie **Salesforce instellen** kopieert u de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-salesforce-single-sign-on"></a>Eenmalige aanmelding configureren voor Salesforce

1. Open een nieuw tabblad in uw browser en meld u aan bij uw Salesforce-beheerdersaccount.

2. Klik op **Instellen** onder het **instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/configure1.png)

3. Schuif in het navigatiedeelvenster omlaag naar **INSTELLINGEN** en klik op **Identiteit** om het bijbehorende gedeelte uit te vouwen. Klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-admin-sso.png)

4. Op de pagina **Instellingen voor eenmalige aanmelding** klikt u op de knop **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Als u eenmalige aanmelding niet kunt inschakelen voor uw Salesforce-account, moet u mogelijk contact opnemen met het [Salesforce-klantondersteuningsteam](https://help.salesforce.com/support).

5. Selecteer **SAML ingeschakeld** en klik vervolgens op **Opslaan**.

      ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-enable-saml.png)

6. Als u uw SAML-instellingen voor eenmalige aanmelding wilt configureren, klikt u op **Nieuw op basis van het bestand met metagegevens**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-admin-sso-new.png)

7. Klik op **Bestand kiezen** om het XML-bestand met metagegevens te uploaden dat u hebt gedownload uit Azure Portal. Klik dan op **Maken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/xmlchoose.png)

8. Op de pagina **SAML-instellingen voor eenmalige aanmelding** worden de velden automatisch ingevuld. Klik op Opslaan.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/salesforcexml.png)

9. Klik in het navigatiedeelvenster links in Salesforce op **Bedrijfsinstellingen** om het bijbehorende gedeelte uit te vouwen. Klik dan op **Mijn domein**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-my-domain.png)

10. Schuif omlaag naar het gedeelte **Verificatieconfiguratie** en klik op de knop **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-edit-auth-config.png)

11. In het gedeelte **Verificatieconfiguratie** schakelt u **AzureSSO** in als **verificatieservice** voor de SAML-configuratie voor eenmalige aanmelding. Klik dan op **Opslaan** .

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Als er meer dan één verificatieservice wordt geselecteerd, wordt gebruikers gevraagd met welke verificatieservice ze zich graag willen aanmelden om gebruik te maken van eenmalige aanmelding in uw Salesforce-omgeving. Als u niet wilt dat dit gebeurt, moet u **andere verificatieservices uitgeschakeld laten**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Salesforce.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **Salesforce**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **Salesforce** in de lijst met toepassingen.

    ![De Salesforce-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-salesforce-test-user"></a>Een Salesforce-testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Salesforce. Salesforce biedt ondersteuning voor just-in-time inrichten, wat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet bestaat in Salesforce, wordt er een nieuwe gemaakt wanneer u Salesforce opent. Salesforce ondersteunt ook automatische inrichting van gebruikers. Meer details over het configureren van automatische inrichting van gebruikers vindt u [hier](salesforce-provisioning-tutorial.md).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de Salesforce-tegel in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de instantie van Salesforce waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

* [Inrichten van gebruikers configureren](salesforce-provisioning-tutorial.md)