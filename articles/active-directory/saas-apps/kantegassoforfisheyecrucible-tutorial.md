---
title: 'Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor FishEye/filterkroes | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kantega SSO voor FishEye/filterkroes.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9fe951fd-1530-4d33-a1a4-390385b99ce9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 16-04-2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e363b6284b29b28b8bfc3081221e23ea1842a322
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67099012"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-fisheyecrucible"></a>Zelfstudie: Azure Active Directory-integratie met Kantega SSO voor FishEye/filterkroes

In deze zelfstudie leert u hoe u Kantega SSO voor FishEye/filterkroes integreren met Azure Active Directory (Azure AD).
Kantega SSO voor FishEye/filterkroes integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Kantega SSO voor FishEye/filterkroes heeft.
* U kunt uw gebruikers worden automatisch aangemeld Kantega SSO voor FishEye/filterkroes (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Kantega SSO voor FishEye/filterkroes, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Kantega SSO voor eenmalige aanmelding FishEye/filterkroes ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Kantega SSO voor FishEye/filterkroes **SP en IDP** gestart door SSO

## <a name="adding-kantega-sso-for-fisheyecrucible-from-the-gallery"></a>Kantega SSO voor FishEye/filterkroes uit de galerie toe te voegen

Voor het configureren van de integratie van Kantega SSO voor FishEye/filterkroes in Azure AD, moet u Kantega SSO voor FishEye/filterkroes uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Kantega SSO voor FishEye/filterkroes uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** , klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Kantega SSO voor FishEye/filterkroes**, selecteer **Kantega SSO voor FishEye/filterkroes** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Kantega SSO voor FishEye/filterkroes in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Kantega SSO voor FishEye/filterkroes op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Kantega SSO voor FishEye/filterkroes tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Kantega SSO voor FishEye/filterkroes, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van SSO voor FishEye/filterkroes Single Sign-On Kantega](#configure-kantega-sso-for-fisheyecrucible-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Kantega SSO maken voor de testgebruiker FishEye/filterkroes](#create-kantega-sso-for-fisheyecrucible-test-user)**  : als u wilt een equivalent van Britta Simon in Kantega SSO voor FishEye/filterkroes die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Kantega SSO voor FishEye/filterkroes, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Kantega SSO voor FishEye/filterkroes** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In het gedeelte **Standaard SAML-configuratie** voert u de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    ![Kantega SSO voor FishEye/filterkroes domein en URL's één aanmeldings-informatie](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Kantega SSO voor FishEye/filterkroes domein en URL's één aanmeldings-informatie](common/metadata-upload-additional-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Deze waarden zijn ontvangen tijdens de configuratie van FishEye/filterkroes-invoegtoepassing die later in de zelfstudie wordt uitgelegd.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

7. Op de **Kantega SSO voor FishEye/filterkroes instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-kantega-sso-for-fisheyecrucible-single-sign-on"></a>SSO voor vissenoog/filterkroes Single Sign-On Kantega configureren

1. In een ander browservenster aanmelden bij uw FishEye/filterkroes on-premises server als beheerder.

1. Wijs het tandwiel aan met de muisaanwijzer en klik op **Add-ons**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon1.png)

1. Systeeminstellingen sectie, klikt u op **nieuwe invoegtoepassingen zoeken**. 

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/add-on2.png)

1. Search **Kantega SSO voor filterkroes** en klikt u op **installeren** knop voor het installeren van de nieuwe SAML-invoegtoepassing.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon2.png)

1. De installatie van de invoegtoepassing wordt gestart. 

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon33.png)

1. Nadat de installatie voltooid is. Klik op **Sluiten**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon34.png)

1.  Klik op **Beheren**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon35.png)

1. Klik op **Configure** om de nieuwe invoegtoepassing te configureren. 

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon3.png)

1. In de **SAML** sectie. Selecteer **Azure Active Directory (Azure AD)** uit de **id-provider toevoegen** vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon4.png)

1. Selecteer het abonnementsniveau van als **Basic**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon5.png)

1. Op de **eigenschappen van de App** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon6.png)

    a. Kopiëren de **App ID URI** waarde en deze gebruiken als **-id, de antwoord-URL en de aanmeldings-URL** op de **SAML-basisconfiguratie** sectie in Azure portal.

    b. Klik op **volgende**.

1. Op de **metagegevens importeren** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon7.png)

    a. Selecteer **metagegevensbestand op mijn computer**, en het bestand met metagegevens uploaden, die u hebt gedownload vanuit Azure portal.

    b. Klik op **volgende**.

1. Op de **naam en de SSO-locatie** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon8.png)

    a. Naam van de id-Provider in **identiteit providernaam** tekstvak (bijvoorbeeld Azure AD).

    b. Klik op **volgende**.

1. Controleer of het certificaat voor ondertekening en klikt u op **volgende**.   

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon9.png)

1. Op de **vissenoog gebruikersaccounts** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon10.png)

    a. Selecteer **gebruikers in de FishEye interne Directory maken, indien nodig** en voer de juiste naam van de groep voor gebruikers (kan meerdere Nee. van groepen met door komma's gescheiden).

    b. Klik op **volgende**.

1. Klik op **Voltooien**.

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon11.png)

1. Op de **bekend domeinen voor Azure AD** sectie, voert u de volgende stappen uit:  

    ![Eenmalige aanmelding configureren](./media/kantegassoforfisheyecrucible-tutorial/addon12.png)

    a. Selecteer **bekend domeinen** in het linkerdeelvenster van de pagina.

    b. Voer de domeinnaam van het in de **bekend domeinen** tekstvak.

    c. Klik op **Opslaan**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Kantega SSO voor FishEye/filterkroes.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Kantega SSO voor FishEye/filterkroes**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Kantega SSO voor FishEye/filterkroes**.

    ![De SSO Kantega voor FishEye/filterkroes koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-kantega-sso-for-fisheyecrucible-test-user"></a>Kantega SSO voor FishEye/filterkroes testgebruiker maken

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij FishEye/filterkroes, moeten ze worden ingericht voor FishEye/filterkroes. In Kantega SSO voor FishEye/filterkroes is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u met uw on-premises filterkroes server aan als beheerder.

1. Beweeg de muisaanwijzer op het tandwiel en klikt u op de **gebruikers**.

    ![Werknemer toevoegen](./media/kantegassoforfisheyecrucible-tutorial/user1.png)

1. Onder **gebruikers** tabblad sectie, klikt u op **gebruiker toevoegen**.

    ![Werknemer toevoegen](./media/kantegassoforfisheyecrucible-tutorial/user2.png)

1. Op de **Add New User** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/kantegassoforfisheyecrucible-tutorial/user3.png)

    a. In het tekstvak **Gebruikersnaam** typt u het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.

    b. In de **weergavenaam** tekstvak weergavenaam van de gebruiker, zoals Britta Simon.

    c. Typ in het tekstvak **Email address** het e-mailadres van de gebruiker, bijvoorbeeld Brittasimon@contoso.com.

    d. In het tekstvak **Wachtwoord** typt u het wachtwoord van de gebruiker.

    e. In de **wachtwoord bevestigen** tekstvak, het wachtwoord van gebruiker opnieuw invoeren.

    f. Klik op **Toevoegen**.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de SSO Kantega FishEye/filterkroes tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de Kantega SSO voor FishEye/filterkroes waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)