---
title: 'Zelfstudie: Azure Active Directory-integratie met iProva | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en iProva configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 1eaeef9b-4479-4a9f-b1b2-bc13b857c75c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c65f8e023e087c3ca49e0d53f5760fbcd9a5f4d9
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56869204"
---
# <a name="tutorial-azure-active-directory-integration-with-iprova"></a>Zelfstudie: Azure Active Directory-integratie met iProva

In deze zelfstudie leert u hoe u iProva integreert met Azure Active Directory (Azure AD).
De integratie van iProva met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie er toegang heeft tot iProva.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij iProva (eenmalige aanmelding).
* U kunt uw accounts vanaf één locatie beheren, de Azure-portal.

Meer informatie over de integratie van SaaS-apps (Software as a Service) met Azure AD vindt u in [Eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie te configureren met iProva:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u op de website van [Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand krijgen.
* Een iProva-abonnement waarvoor eenmalige aanmelding is ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* iProva ondersteunt door SP geïnitieerde eenmalige aanmelding.

## <a name="add-iprova-from-the-gallery"></a>iProva toevoegen vanuit de galerie

Om de integratie van iProva met Azure AD te configureren, moet u iProva vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

Ga als volgt te werk om iProva vanuit de galerie toe te voegen:

1. In de [Azure-portal](https://portal.azure.com), selecteert u in het linkerdeelvenster **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u **Nieuwe toepassing** bovenaan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **iProva** in het zoekvak. Selecteer **iProva** in het deelvenster met zoekresultaten en selecteer vervolgens **Toevoegen** om de toepassing toe te voegen.

     ![iProva in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte gaat u eenmalige aanmelding van Azure AD met iProva configureren en testen op basis van een testgebruiker met de naam Britta Simon.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in iProva tot stand is gebracht.

Als u eenmalige aanmelding van Azure AD wilt configureren en testen met iProva, moet u de volgende procedures uitvoeren:

- [Configuratiegegevens ophalen uit iProva](#retrieve-configuration-information-from-iprova) ter voorbereiding op de volgende stappen.
- [Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie kunnen gebruiken.
- [Eenmalige aanmelding configureren voor iProva ](#configure-iprova-single-sign-on) om de instellingen voor eenmalige aanmelding aan de clientzijde te configureren.
- [Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user) voor het testen van eenmalige aanmelding van Azure AD met Britta Simon.
- [De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user) zodat Britta Simon gebruik kan maken van eenmalige aanmelding van Azure AD.
- [Een iProva-testgebruiker maken](#create-an-iprova-test-user) om een tegenhanger voor Britta Simon te maken in iProva die wordt gekoppeld aan de Azure AD-voorstelling van de gebruiker.
- [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="retrieve-configuration-information-from-iprova"></a>Configuratiegegevens ophalen uit iProva

In dit gedeelte gaat u gegevens ophalen uit iProva om eenmalige aanmelding van Azure AD te configureren.

1. Open een webbrowser en ga naar de **SAML2-infopagina** van iProva, met behulp van het volgende URL-patroon:

    | | |
    |-|-|
    | `https://SUBDOMAIN.iprova.nl/saml2info`|
    | `https://SUBDOMAIN.iprova.be/saml2info`|
    | | |

     ![De iProva SAML2-infopagina weergeven](media/iprova-tutorial/iprova-saml2-info.png)

2. Laat het tabblad geopend terwijl u de volgende stappen uitvoert op een nieuw browsertabblad.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer deze stappen uit om eenmalige aanmelding van Azure AD bij iProva te configureren:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina met de integratie van de toepassing **iProva** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. Selecteer in het dialoogvenster **Selecteer een methode voor eenmalige aanmelding** de modus **SAML/WS-Fed** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Selecteer op de pagina **Eenmalige aanmelding met SAML instellen** het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Pictogram Bewerken in Standaard SAML-configuratie](common/edit-urls.png)

4. Voer deze stappen uit in het gedeelte **Standaard SAML-configuratie**:

    a. Vul in het vak **Id** de waarde in die wordt weergegeven achter het label **EntityID** op de **SAML2-infopagina van iProva**. Deze pagina is nog steeds geopend op het andere tabblad van de browser.

    b. Vul in het vak **Antwoord-URL** de waarde in die wordt weergegeven achter het label **Reply URL** op de **SAML2-infopagina van iProva**. Deze pagina is nog steeds geopend op het andere tabblad van de browser.

    c. Vul in het vak **Aanmeldings-URL** de waarde in die wordt weergegeven achter het label **Sign-on URL** op de **SAML2-infopagina van iProva**. Deze pagina is nog steeds geopend op het andere tabblad van de browser.

    ![Gegevens voor domein en URL's voor eenmalige aanmelding bij iProva](common/sp-identifier-reply.png)

5. De iProva-toepassing verwacht dat SAML-asserties een specifieke indeling hebben. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Selecteer op de pagina **Eenmalige aanmelding met SAML instellen** het pictogram **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![Het dialoogvenster Gebruikerskenmerken](common/edit-attribute.png)

6. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding. Volg deze stappen.

    | Naam | Bronkenmerk| Naamruimte |
    | ---------------| -------- | -----|
    | `samaccountname` | `user.onpremisessamaccountname`| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    | | |

    a. Selecteer **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![Gebruikersclaims](common/new-save-attribute.png)

    ![Het dialoogvenster Gebruikersclaims beheren](common/new-attribute-details.png)

    b. Typ in het tekstvak **Naam** de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Typ in de lijst **Naamruimte** de naamruimtewaarde voor die rij.

    d. Stel **Bron** in op **Kenmerk**.

    e. Typ in de lijst **Bronkenmerk** de kenmerkwaarde voor die rij.

    f. Selecteer **OK**.

    g. Selecteer **Opslaan**.

7. Klik op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** op het pictogram **Kopiëren** om de **URL voor federatieve metagegevens van de app** te kopiëren en sla deze vervolgens op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-iprova-single-sign-on"></a>Eenmalige aanmelding voor iProva configureren

1. Meld u aan bij iProva met uw **beheerdersaccount**.

2. Open het menu **Go to**.

3. Selecteer **Application management**.

4. Selecteer **General** in het deelvenster **System settings**.

5. Selecteer **Bewerken**.

6. Scrol omlaag naar **Access control**.

    ![Instellingen voor toegangsbeheer van iProva](media/iprova-tutorial/iprova-accesscontrol.png)

7. Zoek de instelling **Users are automatically logged on with their network accounts** en wijzig deze in **Yes, authentication via SAML**. Er worden nu aanvullende opties weergegeven.

8. Selecteer **Set up**.

9. Selecteer **Volgende**.

10. Er wordt nu gevraagd of u federatiegegevens wilt downloaden via een URL of wilt uploaden uit een bestand. Selecteer de optie **From URL**.

    ![Azure AD-metagegevens downloaden](media/iprova-tutorial/iprova-download-metadata.png)

11. Plak de metagegevens-URL die u hebt opgeslagen in de laatste stap van de procedure Eenmalige aanmelding van Azure AD configureren.

12. Selecteer de pijlknop om de metagegevens te downloaden uit Azure AD.

13. Wanneer het downloaden is voltooid **,** wordt er een bevestigingsberichtweergegeven.

14. Selecteer **Volgende**.

15. Sla de optie **Test login** maar even over en klik op de knop **Next**.

16. Selecteer **windowsaccountname** in de vervolgkeuzelijst **Claim to use**.

17. Selecteer **Voltooien**.

18. U keert nu terug naar het scherm **Edit general settings**. Scrol omlaag naar de onderkant van de pagina en selecteer **OK** om de configuratie op te slaan.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In dit gedeelte gaat u een testgebruiker met de naam Britta Simon maken in de Azure-portal.

1. Selecteer in de Azure-portal aan de linkerkant **Azure Active Directory** > **Gebruikers** > **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![De knop Nieuwe gebruiker](common/new-user.png)

3. Volg deze stappen in het dialoogvenster **Gebruiker**.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het vak **Naam** een naam in, zoals **BrittaSimon**.
  
    b. Voer in het vak **Gebruikersnaam** *yourname@yourcompanydomain.extension* in. 
    Een voorbeeld is BrittaSimon@contoso.com.

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.

    d. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot iProva.

1. Selecteer in de Azure-portal **Bedrijfstoepassingen** > **Alle toepassingen** > **iProva**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **iProva** in de lijst met toepassingen.

    ![De koppeling iProva in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het dialoogvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst **Gebruikers** en kies vervolgens **Selecteren** onderaan het scherm.

6. Als u een rolwaarde verwacht in de SAML-assertie, selecteert u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst. Kies **Selecteren** onderaan het scherm.

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

### <a name="create-an-iprova-test-user"></a>Testgebruiker voor iProva maken

1. Meld u aan bij iProva met uw **beheerdersaccount**.

2. Open het menu **Go to**.

3. Selecteer **Application management**.

4. Selecteer **Users** in het deelvenster **Users and user groups**.

5. Selecteer **Toevoegen**.

6. Voer in het vak **Username** *brittasimon@yourcompanydomain.extension* in. 
    Een voorbeeld is BrittaSimon@contoso.com.

7. Voer in het vak **Full name** de volledige naam in, zoals **BrittaSimon**.

8. Selecteer de optie **No password (use single sign-on)**.

9. Typ *yourname@yourcompanydomain.extension* in het vak **E-mail address**. 
   Een voorbeeld is BrittaSimon@contoso.com.

10. Scrol omlaag naar het einde van de pagina en selecteer **Finish**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In dit gedeelte test u de configuratie voor eenmalige aanmelding van Azure AD met behulp van het toegangsvenster.

Wanneer u de tegel iProva selecteert in het toegangsvenster, zou u automatisch moeten worden aangemeld bij het exemplaar van iProva toegangsvenster waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [iProva - Configuration of single sign-on with SAML2](https://webshare.iprova.nl/0wqwm45yn09f5poh/Document.aspx)
