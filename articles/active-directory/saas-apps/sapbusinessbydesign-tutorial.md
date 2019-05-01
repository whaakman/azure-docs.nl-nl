---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Business ByDesign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23669671c9aec2ebad8e03e06a0ea1b139214cad
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64683858"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign

In deze zelfstudie leert u hoe u SAP Business ByDesign integreren met Azure Active Directory (Azure AD).
SAP Business ByDesign integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot SAP Business ByDesign heeft.
* U kunt uw gebruikers worden automatisch aangemeld SAP Business ByDesign (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Business ByDesign, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* SAP Business ByDesign eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor SAP Business ByDesign **SP** gestart door SSO

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP Business ByDesign uit de galerie toe te voegen

Voor het configureren van de integratie van SAP Business ByDesign in Azure AD, moet u SAP Business ByDesign uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP Business ByDesign uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **SAP Business ByDesign**, selecteer **SAP Business ByDesign** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SAP Business ByDesign in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SAP Business ByDesign op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SAP Business ByDesign tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP Business ByDesign, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van SAP Business ByDesign Single Sign-On](#configure-sap-business-bydesign-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[SAP Business ByDesign testgebruiker maken](#create-sap-business-bydesign-test-user)**  : als u wilt een equivalent van Britta Simon hebben in SAP Business ByDesign die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met SAP Business ByDesign, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **SAP Business ByDesign** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![SAP Business ByDesign domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<servername>.sapbydesign.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<servername>.sapbydesign.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Neem contact op met [SAP Business ByDesign Client ondersteuningsteam](https://www.sap.com/products/cloud-analytics.support.html) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. SAP Business ByDesign toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen.

    ![image](common/edit-attribute.png)

6. Klik op de **bewerken** bewerkingspictogram van de **naam id-waarde**.

    ![image](media/sapbusinessbydesign-tutorial/mail-prefix1.png)

7. Op de **beheren gebruikersclaims** sectie, voert u de volgende stappen uit: ![installatiekopie](media/sapbusinessbydesign-tutorial/mail-prefix2.png)

    a. Selecteer **transformatie** als een **bron**.

    b. In de **transformatie** vervolgkeuzelijst **ExtractMailPrefix()**.

    c. In de **Parameter 1** vervolgkeuzelijst, selecteert u het gebruikerskenmerk die u wilt gebruiken voor uw implementatie. Als u bijvoorbeeld de werknemer-id wilt gebruiken als unieke gebruikers-id en u de waarde van het kenmerk in de ExtensionAttribute2 hebt opgeslagen, selecteert u vervolgens user.extensionattribute2.

    d. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

9. Op de **instellen van SAP Business ByDesign** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-sap-business-bydesign-single-sign-on"></a>SAP Business ByDesign voor eenmalige aanmelding configureren

1. Meld u aan bij de portal van uw SAP Business ByDesign met administrator-rechten.

2. Navigeer naar **toepassings- en algemene Gebruikersbeheertaak** en klikt u op de **id-Provider** tabblad.

3. Klik op **nieuwe id-Provider** en selecteert u de metagegevens-XML-bestand dat u hebt gedownload vanuit Azure portal. Door het importeren van de metagegevens worden automatisch het vereiste handtekeningcertificaat en versleutelingscertificaat geüpload.

    ![Eenmalige aanmelding configureren](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

4. Om op te nemen de **URL van de Bevestigingsconsumerservice** selecteren in de SAML-aanvraag **opnemen URL van de Bevestigingsconsumerservice**.

5. Klik op **Eenmalige aanmelding activeren**.

6. Sla uw wijzigingen op.

7. Klik op het tabblad **Mijn systeem**.

    ![Eenmalige aanmelding configureren](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

8. In de **Azure AD aanmeldings-URL** tekstvak plakken **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    ![Eenmalige aanmelding configureren](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

9. Opgeven of de werknemer handmatig kiezen kunt tussen het aanmelden met gebruikersnaam en wachtwoord of eenmalige aanmelding door te selecteren **handmatige Identity Provider selectie**.

10. In de **SSO URL** sectie, geef de URL die moet worden gebruikt door de werknemer op aanmelden op het systeem.
    In de verzonden naar de werknemer vervolgkeuzelijst URL, kunt u kiezen uit de volgende opties:

    **Niet-SSO-URL**

    Alleen de normale systeem-URL wordt naar de werknemer verstuurd. De werknemer moet kan niet aanmelden met behulp van eenmalige aanmelding, en gebruik wachtwoord of certificaat in plaats daarvan.

    **SSO-URL**

    Alleen de SSO-URL wordt naar de werknemer verzonden. De werknemer kunt aanmelden met behulp van eenmalige aanmelding. Een verificatieaanvraag wordt omgeleid via de IdP.

    **Automatische selectie**

    Als eenmalige aanmelding niet actief is, wordt alleen de normale systeem-URL naar de werknemer verstuurd. Als eenmalige aanmelding actief is, controleert het systeem of de werknemer een wachtwoord heeft. Als er een wachtwoord beschikbaar is, worden zowel de SSO-URL als de niet-SSO-URL naar de werknemer verzonden. Als de werknemer geen wachtwoord heeft, wordt echter alleen de URL voor eenmalige aanmelding naar de werknemer verzonden.

11. Sla uw wijzigingen op.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot SAP Business ByDesign.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **SAP Business ByDesign**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **SAP Business ByDesign**.

    ![De SAP Business ByDesign-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-sap-business-bydesign-test-user"></a>SAP Business ByDesign testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SAP Business ByDesign. Neem contact op met [SAP Business ByDesign Client ondersteuningsteam](https://www.sap.com/products/cloud-analytics.support.html) om toe te voegen de gebruikers in de SAP Business ByDesign-platform. 

> [!NOTE]
> Zorg ervoor dat NameID-waarde met het gebruikersnaamveld in de SAP Business ByDesign-platform overeenkomen moet.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel SAP Business ByDesign in het toegangsvenster, moet u worden automatisch aangemeld bij de SAP Business ByDesign waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
