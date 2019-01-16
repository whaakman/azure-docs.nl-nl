---
title: 'Zelfstudie: Azure Active Directory-integratie met Palo Alto Networks - Captive Portal | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Palo Alto Networks - Captive Portal configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 67a0b476-2305-4157-8658-2ec3625850d5
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/25/2018
ms.author: jeedes
ms.openlocfilehash: eff08cc17f475e2b6ad6406e463de27371bbe5b1
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064726"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---captive-portal"></a>Zelfstudie: Azure Active Directory-integratie met Palo Alto Networks - Captive Portal

In deze zelfstudie leert u hoe u Palo Alto Networks - Captive Portal integreert met Azure Active Directory (Azure AD).
De integratie van Palo Alto Networks - Captive Portal met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot Palo Alto Networks - Captive Portal.
* U kunt instellen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij Palo Alto Networks - Captive Portal (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om Azure AD-integratie te configureren met Palo Alto Networks - Captive Portal:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op Palo Alto Networks - Captive Portal waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Palo Alto Networks - Captive Portal biedt ondersteuning voor door **IDP** geïnitieerde eenmalige aanmelding

* Palo Alto Networks - Captive Portal ondersteunt **Just In Time**-gebruikersinrichting

## <a name="adding-palo-alto-networks---captive-portal-from-the-gallery"></a>Palo Alto Networks - Captive Portal toevoegen vanuit de galerie

Om de integratie van Palo Alto Networks - Captive Portal te configureren in Azure AD, moet u Palo Alto Networks - Captive Portal vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Palo Alto Networks - Captive Portal vanuit de galerie toe te voegen:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Palo Alto Networks - Captive Portal** in het zoekvak, selecteer **Palo Alto Networks - Captive Portal** in het deelvenster met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![Palo Alto Networks - Captive Portal toevoegen vanuit de galerie](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met Palo Alto Networks - Captive Portal op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in Palo Alto Networks - Captive Portal tot stand is gebracht.

Om eenmalige aanmelding van Azure AD met Palo Alto Networks - Captive Portal te configureren en testen, moet u de volgende procedures voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor Palo Alto Networks - Captive Portal configureren](#configure-palo-alto-networks---captive-portal-single-sign-on)**: de instellingen voor eenmalige aanmelding aan de toepassingszijde configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker voor Palo Alto Networks - Captive Portal maken](#create-palo-alto-networks---captive-portal-test-user)**: als u een tegenhanger van Britta Simon in Palo Alto Networks - Captive Portal wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding van Azure AD te configureren met Palo Alto Networks - Captive Portal:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina van de integratie van **Palo Alto Networks - Captive Portal** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Voer in het dialoogvenster **Standaard SAML-configuratie** de volgende stappen uit:

    ![Gegevens van domein en URL's voor eenmalige aanmelding van Palo Alto Networks - Captive Portal](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<Customer Firewall Hostname>/SAML20/SP`

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<Customer Firewall Hostname>/SAML20/SP/ACS`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Neem contact op met het [ondersteuningsteam van Palo Alto Networks - Captive Portal](https://support.paloaltonetworks.com/support) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="configure-palo-alto-networks---captive-portal-single-sign-on"></a>Eenmalige aanmelding configureren voor Palo Alto Networks - Captive Portal

1. Open de site van Palo Alto als beheerder in een ander browservenster.

2. Klik op **Apparaat**.

    ![Eenmalige aanmelding voor Palo Alto configureren](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Selecteer **SAML-id-provider** in de linkernavigatiebalk en klik op Importeren om het metagegevensbestand te importeren.

    ![Eenmalige aanmelding voor Palo Alto configureren](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin2.png)

4. Voer de volgende acties uit in het venster Importeren

    ![Eenmalige aanmelding voor Palo Alto configureren](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. Geef in het tekstvak **Profile Name** een naam op, bijvoorbeeld Azure AD Admin UI.
    
    b. Klik in **Metagegevens van id-provider** op **Bladeren** en selecteer het bestand metadata.xml dat u vanuit Azure Portal hebt gedownload
    
    c. Klik op **OK**

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

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot Palo Alto Networks - Captive Portal.

1. Selecteer in de Azure-portal achtereenvolgens **Bedrijfstoepassingen**, **Alle toepassingen** en **Palo Alto Networks - Captive Portal**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **Palo Alto Networks - Captive Portal** in de lijst met toepassingen.

    ![De koppeling naar Palo Alto Networks - Captive Portal in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-palo-alto-networks---captive-portal-test-user"></a>Een testgebruiker maken voor Palo Alto Networks - Captive Portal

In dit gedeelte maakt u een gebruiker met de naam Britta Simon in Palo Alto Networks - Captive Portal. Palo Alto Networks - Captive Portal ondersteunt **Just In Time-gebruikersinrichting**, wat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als er nog geen gebruiker in Palo Alto Networks - Captive Portal bestaat, wordt er na verificatie een nieuwe gemaakt.

> [!NOTE]
> Als u handmatig een gebruiker moet maken, neemt u contact op met het [klantondersteuningsteam van Palo Alto Networks - Captive Portal](https://support.paloaltonetworks.com/support)

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

Captive Portal is geconfigureerd achter de firewall op de virtuele Windows-machine. Om eenmalige aanmelding te testen bij Captive Portal, meldt u zich met behulp van RDP aan bij de virtuele Windows-machine. Open in de RDP-sessie een willekeurige website in uw browser. U ziet nu automatisch de URL voor eenmalige aanmelding en de prompt voor verificatie. Als de verificatie is voltooid, kunt u naar de gewenste websites navigeren.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

