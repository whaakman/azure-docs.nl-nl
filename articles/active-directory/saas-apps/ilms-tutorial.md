---
title: 'Zelfstudie: Azure Active Directory-integratie met iLMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en iLMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ef88a2860603a9450db27b744413ac0a886e81e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57863267"
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Zelfstudie: Azure Active Directory-integratie met iLMS

In deze zelfstudie leert u hoe u iLMS integreren met Azure Active Directory (Azure AD).
ILMS integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot iLMS heeft.
* U kunt uw gebruikers worden automatisch aangemeld iLMS (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iLMS, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* iLMS eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* biedt ondersteuning voor iLMS **SP en IDP** gestart door SSO

## <a name="adding-ilms-from-the-gallery"></a>ILMS uit de galerie toe te voegen

Voor het configureren van de integratie van iLMS in Azure AD, moet u iLMS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iLMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **iLMS**, selecteer **iLMS** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![iLMS in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met iLMS op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in iLMS tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met iLMS, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van iLMS Single Sign-On](#configure-ilms-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker iLMS](#create-ilms-test-user)**  : als u wilt een equivalent van Britta Simon in iLMS die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met iLMS, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **iLMS** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![iLMS domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    a. In de **id** in het tekstvak, plak de **id** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in de beheerportal iLMS.

    b. In de **antwoord-URL** in het tekstvak, plak de **eindpunt (URL)** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in het volgende patroon van iLMS-beheerportal `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![iLMS domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

    In de **aanmeldings-URL** in het tekstvak, plak de **eindpunt (URL)** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in de beheerportal iLMS als `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

6. Om in te schakelen JIT wordt ingericht, wordt uw toepassing iLMS de SAML-asserties ondertekend verwacht in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram **Bewerken** om het dialoogvenster Gebruikerskenmerken te openen.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > U moet inschakelen **Un-recognized-gebruikersaccount maken** in iLMS om toe te wijzen deze kenmerken. Volg de instructies [hier](http://support.inspiredelearning.com/customer/portal/articles/2204526) voor een beter beeld van de configuratie van de kenmerken.

7. Bovendien hierboven verwacht iLMS toepassing paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:

    | Name | Bronkenmerk|
    | --------|------------- |
    | deling | user.department |
    | regio | user.state |
    | department | user.jobtitle |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

9. Op de **iLMS instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-ilms-single-sign-on"></a>ILMS eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw **iLMS-beheerportal** als beheerder.

2. Klik op **SSO:SAML** onder **instellingen** tabblad SAML-instellingen openen en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/1.png)

3. Vouw de **serviceprovider** sectie en kopieert u de **id** en **eindpunt (URL)** waarde.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/2.png) 

4. Onder **id-Provider** sectie, klikt u op **metagegevens importeren**.

5. Selecteer de **Federatiemetagegevens** bestand gedownload vanuit Azure Portal uit **SAML-handtekeningcertificaat** sectie.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_ssoconfig1.png)

6. Als u inschakelen wilt voor het maken van accounts iLMS voor ongedaan maken inrichting JIT-gebruikers herkennen, volgt u onderstaande stappen te volgen:

    a. Controleer **Account maken voor niet-herkende gebruiker**.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/tutorial_ilms_ssoconfig2.png)

    b. De kenmerken in Azure AD met de kenmerken in iLMS toewijzen. Geef de naam van de kenmerken of de standaardwaarde is opgegeven in de kolom kenmerk.

    c. Ga naar **bedrijfsregels** tabblad en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/5.png)

    d. Controleer **Un-recognized regio's maken, afdelingen en afdelingen** om regio's, afdelingen en afdelingen die nog niet bestaan op het moment van eenmalige aanmelding te maken.

    e. Controleer **Update gebruiker profiel tijdens aanmelding** om op te geven of profiel van de gebruiker is bijgewerkt met elke eenmalige aanmelding.

    f. Als de **lege waarden van de Update voor niet verplichte velden in het gebruikersprofiel** optie is ingeschakeld, optioneel profiel velden die leeg bij het aanmelden wordt zijn ook voor zorgen dat iLMS profiel van de gebruiker bevatten lege waarden voor deze velden.

    g. Controleer **fout meldingse-mail verzenden** en voer het e-mailadres van de gebruiker waar u de fout e-mailmelding ontvangen.

7. Klik op **Save** om de instellingen op te slaan.

    ![Eenmalige aanmelding configureren](./media/ilms-tutorial/save.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot iLMS.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **iLMS**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **iLMS**.

    ![De koppeling iLMS in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-ilms-test-user"></a>ILMS testgebruiker maken

Toepassing ondersteunt Just in time gebruikersinrichting en na-verificatiegebruikers automatisch in de toepassing gemaakt worden. JIT werkt als u hebt geklikt op de **Un-recognized-gebruikersaccount maken** selectievakje tijdens de SAML-configuratie-instelling op iLMS-beheerportal.

Als u een gebruiker handmatig hebt gemaakt wilt, volgt u onderstaande stappen te volgen:

1. Meld u aan bij uw bedrijf iLMS site aan als beheerder.

2. Klik op **gebruiker registreren** onder **gebruikers** tabblad openen **gebruiker registreren** pagina.

   ![Werknemer toevoegen](./media/ilms-tutorial/3.png)

3. Op de **gebruiker registreren** pagina, de volgende stappen uitvoeren.

    ![Werknemer toevoegen](./media/ilms-tutorial/create_testuser_add.png)

    a. In de **voornaam** tekstvak, de eerste naam zoals Julia.

    b. In de **achternaam** tekstvak typt u de achternaam, zoals Simon.

    c. In de **E-mail-ID** tekstvak, typ het e-mailadres van de gebruiker, zoals BrittaSimon@contoso.com.

    d. In de **regio** vervolgkeuzelijst, selecteer de waarde voor de regio.

    e. In de **deling** vervolgkeuzelijst, selecteer de waarde voor afdeling.

    f. In de **afdeling** vervolgkeuzelijst, selecteer de waarde voor afdeling.

    g. Klik op **Opslaan**.

    > [!NOTE]
    > U kunt registratie-e-mail naar gebruiker verzenden door te selecteren **registratie E-mail** selectievakje.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel iLMS in het toegangsvenster, moet u worden automatisch aangemeld bij de iLMS waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)