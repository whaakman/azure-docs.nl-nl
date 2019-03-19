---
title: 'Zelfstudie: Azure Active Directory-integratie met PureCloud by Genesys | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en PureCloud by Genesys.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16a46db-5de2-4681-b7e0-94c670e3e54e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: d1cc7735b8caa952a5ab7695d3e1f35b03fec1bb
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57890675"
---
# <a name="tutorial-azure-active-directory-integration-with-purecloud-by-genesys"></a>Zelfstudie: Azure Active Directory-integratie met PureCloud by Genesys

In deze zelfstudie leert u hoe u PureCloud by Genesys kunt integreren met Azure Active Directory (Azure AD).
De integratie van PureCloud by Genesys met Azure AD heeft de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot PureCloud by Genesys.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij PureCloud by Genesys (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Om Azure AD-integratie te configureren met PureCloud by Genesys hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op PureCloud by Genesys waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* PureCloud by Genesys ondersteunt door **SP en IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-purecloud-by-genesys-from-the-gallery"></a>PureCloud by Genesys toevoegen vanuit de galerie

Om de integratie van PureCloud by Genesys in Azure AD te configureren, moet u PureCloud by Genesys vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om PureCloud by Genesys toe te voegen vanuit de galerie:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **PureCloud by Genesys**, selecteer **PureCloud by Genesys** in het resultaatvenster en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![PureCloud by Genesys in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met PureCloud by Genesys op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in PureCloud by Genesys tot stand is gebracht.

Als u eenmalige aanmelding van Azure AD wilt configureren en testen met PureCloud by Genesys, moet u de volgende procedures uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor PureCloud by Genesys configureren](#configure-purecloud-by-genesys-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de clientzijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Testgebruiker voor PureCloud by Genesys maken](#create-purecloud-by-genesys-test-user)**: als u een tegenhanger van Britta Simon in PureCloud by Genesys wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren met PureCloud by Genesys:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de overzichtspagina van de integratie voor **PureCloud by Genesys** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Gegevens voor domein en URL's voor eenmalige aanmelding bij PureCloud by Genesys](common/idp-intiated.png)

    a. Typ in het tekstvak **Id** een URL overeenkomstig uw regio:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.au/saml` |

    b. Typ in het tekstvak **Antwoord-URL** een URL overeenkomstig uw regio:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.com.au/saml` |

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Gegevens voor domein en URL's voor eenmalige aanmelding bij PureCloud by Genesys](common/metadata-upload-additional-signon.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL overeenkomstig uw regio:
    
    | |
    |--|
    | `https://login.mypurecloud.com` |
    | `https://login.mypurecloud.de` |
    | `https://login.mypurecloud.jp` |
    | `https://login.mypurecloud.ie` |
    | `https://login.mypurecloud.com.au` |

6. In de PureCloud by Genesys-toepassing worden de SAML-asserties in een specifieke indeling verwacht. Hiervoor moet u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van uw SAML-tokenkenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram  **Bewerken**  om het dialoogvenster  **Gebruikerskenmerken**  te openen.

    ![image](common/edit-attribute.png)

7. Bovendien verwacht de PureCloud by Genesys-toepassing nog enkele kenmerken die als SAML-antwoord moeten worden doorgestuurd. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:

    | Name | Bronkenmerk|
    | ---------------| --------------- |
    | Email | user.userprinicipalname |
    | OrganizationName | `Your organization name` |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

9. Kopieer in het gedeelte **PureCloud by Genesys instellen** de juiste URL('s) overeenkomstig wat u nodig hebt.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-purecloud-by-genesys-single-sign-on"></a>Eenmalige aanmelding voor PureCloud by Genesys configureren

1. Meld u in een ander browservenster als beheerder aan bij PureCloud by Genesys.

2. Klik op **Admin** in de rechterbovenhoek en ga vervolgens naar **Single Sign-on** onder **Integrations**.

    ![Eenmalige aanmelding configureren](./media/purecloud-by-genesys-tutorial/configure01.png)

3. Selecteer het tabblad **ADFS/Azure AD(Premium)** en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/purecloud-by-genesys-tutorial/configure02.png)

    a. Klik op **Browse** om het met base-64 gecodeerde certificaat dat u hebt gedownload uit de Azure-portal te uploaden naar het **ADFS-certificaat**.

    b. Plak in het tekstvak **ADFS Issuer URI** de waarde van **Azure AD-id**, die u hebt gekopieerd uit de Azure-portal.

    c. Plak in het tekstvak **Target URI** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    d. Om de waarde voor **Relying Party Identifier** te verkrijgen, gaat u naar de Azure-portal. Klik op de integratiepagina voor de **PureCloud by Genesys**-toepassing op het tabblad **Eigenschappen** en kopieer de waarde van **Toepassings-id**. Plak deze in het tekstvak **Relying Party Identifier**. 

    ![Eenmalige aanmelding configureren](./media/purecloud-by-genesys-tutorial/configure06.png)

    e. Klik op **Opslaan**.   

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

In deze sectie gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot PureCloud by Genesys.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, selecteer **Alle toepassingen** en selecteer vervolgens **PureCloud by Genesys**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer **PureCloud by Genesys** in de lijst met toepassingen.

    ![De koppeling PureCloud by Genesys in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-purecloud-by-genesys-test-user"></a>PureCloud by Genesys-testgebruiker maken

Als u wilt dat Azure AD-gebruikers zich aanmelden bij PureCloud by Genesys, moeten ze worden ingericht in PureCloud by Genesys. In het geval van PureCloud by Genesys is dat een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij PureCloud by Genesys als beheerder.

2. Klik op **Admin** in de rechterbovenhoek en ga vervolgens naar **People** onder **People & Permissions**.

    ![Eenmalige aanmelding configureren](./media/purecloud-by-genesys-tutorial/configure03.png)

3. Klik op de pagina People op **Add Person**.

    ![Eenmalige aanmelding configureren](./media/purecloud-by-genesys-tutorial/configure04.png)

4. Voer de volgende stappen uit in het pop-upvenster **Add People to the Organization**:

    ![Eenmalige aanmelding configureren](./media/purecloud-by-genesys-tutorial/configure05.png)

    a. Voer in het tekstvak **Full Name** de naam van de gebruiker in, bijvoorbeeld **Britta Simon**.

    b. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals **brittasimon\@contoso.com**.
    
    c. Klik op **Create**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel PureCloud by Genesys klikt, wordt u automatisch aangemeld bij de instantie van PureCloud by Genesys waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

