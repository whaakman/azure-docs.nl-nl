---
title: 'Zelfstudie: Azure Active Directory-integratie met iProva | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en iProva configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1eaeef9b-4479-4a9f-b1b2-bc13b857c75c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: eb64fd7a7270c3bae03a94385fc77581637ac675
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53981236"
---
# <a name="tutorial-azure-active-directory-integration-with-iprova"></a>Zelfstudie: Azure Active Directory-integratie met iProva

In deze zelfstudie leert u hoe u iProva integreert met Azure Active Directory (Azure AD).
De integratie van iProva met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD beheren wie er toegang heeft tot iProva.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij iProva (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie te configureren met iProva:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Abonnement op iProva met eenmalige aanmelding (SSO) ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* iProva ondersteunt door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-iprova-from-the-gallery"></a>iProva toevoegen vanuit de galerie

Om de integratie van iProva in Azure AD te configureren, moet u iProva vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Ga als volgt te werk om iProva vanuit de galerie toe te voegen:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **iProva** in het zoekvak, selecteer **iProva** in de lijst met resultaten en klik vervolgens op de knop **Toevoegen**  om de toepassing toe te voegen.

     ![iProva in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte gaat u eenmalige aanmelding van Azure AD met iProva configureren en testen op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in iProva tot stand is gebracht.

Als u eenmalige aanmelding van Azure AD wilt configureren en testen met iProva, moet u de volgende procedures uitvoeren:

1. **[Configuratiegegevens ophalen uit iProva](#retrieve-configuration-information-from-iprova)**: ter voorbereiding op de volgende stappen.
2. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
3. **[Eenmalige aanmelding voor iProva configureren](#configure-iprova-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
4. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
5. **[Testgebruiker voor iProva maken](#create-iprova-test-user)**: een tegenhanger voor Britta Simon maken in iProva die wordt gekoppeld aan de Azure AD-voorstelling van de gebruiker.
6. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
7. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="retrieve-configuration-information-from-iprova"></a>Configuratiegegevens ophalen uit iProva

In dit gedeelte gaat u enkele essentiële gegevens ophalen uit iProva.
U hebt deze gegevens nodig om eenmalige aanmelding van Azure AD te configureren.

1. Open een webbrowser en ga naar de **SAML2-infopagina** in iProva, met behulp van het volgende URL-patroon:

    | | |
    |-|-|
    | `https://SUBDOMAIN.iprova.nl/saml2info`|
    | `https://SUBDOMAIN.iprova.be/saml2info`|
    | | |

     ![De iProva SAML2-infopagina weergeven](media/iprova-tutorial/iprova-saml2-info.png)

2. Laat het tabblad geopend terwijl u de volgende stappen uitvoert op een nieuw browsertabblad.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren met iProva:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina met de integratie van de toepassing **iProva** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    a. Vul in het veld **Id** de waarde in die wordt weergegeven achter het label **EntityID** op de **iProva SAML2-infopagina** (nog steeds geopend in het andere browsertabblad).

    b. Vul in het veld **Antwoord-URL** de waarde in die wordt weergegeven achter het label **Reply URL** op de **iProva SAML2-infopagina** (nog steeds geopend in het andere browsertabblad).

    c. Vul in het veld **Aanmeldings-URL** de waarde in die wordt weergegeven achter het label **Sign-on URL** op de **iProva SAML2-infopagina** (nog steeds geopend in het andere browsertabblad).

    ![Gegevens voor domein en URL's voor eenmalige aanmelding bij iProva](common/sp-identifier-reply.png)

5. De iProva-toepassing verwacht dat SAML-asserties een specifieke indeling hebben. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

6. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** configureert u het kenmerk van het SAML-token zoals wordt weergegeven in de bovenstaande afbeelding en voert u de volgende stappen uit:

    | Naam | Bronkenmerk| Naamruimte |
    | ---------------| -------- | -----|
    | `samaccountname` | `user.onpremisessamaccountname`| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    | | |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Typ de naamruimtewaarde voor die rij in de lijst met **naamruimten**.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

7. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-iprova-single-sign-on"></a>Eenmalige aanmelding voor iProva configureren

1. Meld u aan bij iProva met uw **beheerdersaccount**.

2. Open het menu **Go to**.

3. Klik op **Application management**.

4. Klik op **General** in het deelvenster **System settings**.

5. Klik op **Edit**.

6. Scrol omlaag naar **Access control**.

    ![Instellingen voor toegangsbeheer van iProva](media/iprova-tutorial/iprova-accesscontrol.png)

7. Zoek de instelling **Users are automatically logged on with their network accounts** en wijzig deze in **Yes, authentication via SAML**. Er worden nu aanvullende opties weergegeven.

8. Klik op de knop **Setup**.

9. Klik op de knop **Next**

10. Er wordt nu gevraagd of u federatiegegevens wilt downloaden via een URL of wilt uploaden uit een bestand. Selecteer de optie **from URL**.

    ![Azure AD-metagegevens downloaden](media/iprova-tutorial/iprova-download-metadata.png)

11. Plak nu de metagegevens-URL die u hebt opgeslagen in de laatste stap van de procedure **Azure AD configureren voor eenmalige aanmelding**.

12. Klik op de pijlknop om de metagegevens te downloaden uit Azure AD.

13. Wanneer het **downloaden is voltooid, wordt er een bevestigingsbericht**weergegeven.

14. Klik op de knop **Next**

15. Sla de optie **Test login** maar even over en klik op de knop **Next**.

16. Selecteer **windowsaccountname** in de vervolgkeuzelijst **Claim to use**.

17. Klik op de knop **Finish**.

18. U keert nu terug naar het scherm **Edit general settings**. **Scrol omlaag** naar de onderkant van de pagina en klik op de knop **OK** om de configuratie op te slaan.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** de naam **Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **yourname@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot iProva.

1. Selecteer in de Azure-portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **iProva**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **iProva** in de lijst met toepassingen.

    ![De koppeling iProva in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-iprova-test-user"></a>Testgebruiker voor iProva maken

1. Meld u aan bij iProva met uw **beheerdersaccount**.

2. Open het menu **Go to**.

3. Klik op **Application management**.

4. Klik op **Users** in het deelvenster **Users and user groups**.

5. Klik op de knop **Toevoegen**.

6. Typ **brittasimon@yourcompanydomain.extension** in het veld **Username**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

7. Voer in het veld **Full name** de volledige naam in, zoals **BrittaSimon**.

8. Selecteer de optie **No password (use single sign-on)**.

9. Typ **yourname@yourcompanydomain.extension** in het veld **E-mail address**, bijvoorbeeld BrittaSimon@contoso.com.

10. Scrol omlaag naar het einde van de pagina en klik op de knop **Finish**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel iProva klikt, zou u automatisch moeten worden aangemeld bij het exemplaar van in het toegangsvenster waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [iProva - Configuration of single sign-on with SAML2](https://webshare.iprova.nl/0wqwm45yn09f5poh/Document.aspx)