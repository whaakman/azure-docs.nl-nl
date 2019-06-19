---
title: 'Zelfstudie: Azure Active Directory-integratie met Mitel Connect | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Mitel verbinding maken.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 204f540b-09f1-452b-a52f-78143710ef76
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2019
ms.author: jeedes
ms.openlocfilehash: aa3548a71e403728cbec4b8df2b0dce1cf6abde7
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164479"
---
# <a name="tutorial-azure-active-directory-integration-with-mitel-micloud-connect"></a>Zelfstudie: Azure Active Directory-integratie met Mitel MiCloud Connect

In deze zelfstudie leert u hoe u integreert Mitel MiCloud verbinding maken met Azure Active Directory (Azure AD). Integratie van MiCloud verbinding maken met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang heeft tot MiCloud verbinding maken met apps met behulp van de enterprise-referenties.
* U kunt gebruikers inschakelen op uw account om te worden automatisch aangemeld MiCloud (Single Sign-On) verbinding met hun Azure AD-accounts.


Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met MiCloud verbinding maken, moet u de volgende items:

* Een Azure AD-abonnement

  Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Een account Mitel MiCloud verbinding maken

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD eenmalige aanmelding (SSO).

* Biedt ondersteuning voor Mitel verbinding **SP** gestart door SSO

## <a name="adding-mitel-connect-from-the-gallery"></a>Toe te voegen Mitel verbinding maken vanuit de galerie

Voor het configureren van de integratie van Mitel verbinding maken met Azure AD, moet u Mitel verbinding toevoegen uit de galerie aan de lijst met beheerde SaaS-apps in Azure portal.

**Om toe te voegen Mitel verbinding maken vanuit de galerie, voer de volgende stappen uit:**

1. Klik in de **[Azure-portal](https://portal.azure.com)** in het navigatiepaneel aan de linkerkant op **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Klik op **bedrijfstoepassingen** en klik vervolgens op **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Klik op **nieuwe toepassing**.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Type **Mitel verbinding** in het zoekveld, klikt u op **Mitel verbinding** vanuit het deelvenster met resultaten en klik vervolgens op **toevoegen**.

     ![Mitel verbinding maken in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met MiCloud verbinding maken op basis van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in MiCloud verbinding tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met MiCloud verbinding maken, moet u de volgende stappen uit:

1. **[MiCloud Connect te configureren voor eenmalige aanmelding met Azure AD](#configure-micloud-connect-for-sso-with-azure-ad)**  : als u wilt dat uw gebruikers deze functie wilt gebruiken en de SSO-instellingen configureren aan de toepassing.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
4. **[Maak een testgebruiker Mitel MiCloud verbinding](#create-a-mitel-micloud-connect-test-user)**  : als u wilt een equivalent van Britta Simon op uw MiCloud Connect-account dat is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-micloud-connect-for-sso-with-azure-ad"></a>MiCloud configureren voor eenmalige aanmelding met Azure AD Connect

In deze sectie maakt u Azure AD eenmalige aanmelding voor MiCloud verbinding maken in Azure portal inschakelen en configureren van uw account MiCloud verbinding maken om toe te staan van eenmalige aanmelding met Azure AD.

Als u wilt configureren MiCloud verbinding maken met eenmalige aanmelding voor Azure AD, is het eenvoudigste het openen van de Azure-portal en de Mitel accountportal naast elkaar. U moet enkele gegevens vanuit Azure portal kopiëren naar de accountportal van Mitel en enkele van de accountportal van Mitel naar de Azure-portal.


1. Openen van de configuratiepagina in de [Azure-portal](https://portal.azure.com/), doet u het volgende:

    a. Op de **Mitel verbinding** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

    b. In de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **SAML**.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)
    
    De SAML gebaseerde aanmelding op de pagina wordt weergegeven.

2. U opent het configuratiedialoogvenster in de accountportal van Mitel, het volgende doen:

    a. Op de **telefoonsysteem** menu, klikt u op **functies van invoegtoepassingen**.

    b. Aan de rechterkant van **Single Sign-On**, klikt u op **activeren** of **instellingen**.
    
    Verbinding maken met eenmalige aanmelding in het dialoogvenster instellingen wordt weergegeven.
    
3. Selecteer de **inschakelen Single Sign-On** selectievakje.
    ![image](./media/mitel-connect-tutorial/Mitel_Connect_Enable.png)


4. Klik in de Azure-portal op de **bewerken** pictogram in de **SAML-basisconfiguratie** sectie.
    ![image](common/edit-urls.png)

    De basisconfiguratie van SAML-dialoogvenster wordt weergegeven.

5.  Kopieer de URL van de **Mitel-id (entiteits-ID)** veld in de accountportal van Mitel en plak deze in de **id (entiteits-ID)** veld in de Azure-portal.

6. Kopieer de URL van de **antwoord-URL (URL van de Bevestigingsconsumerservice)** veld in de accountportal van Mitel en plak deze in de **antwoord-URL (URL van de Bevestigingsconsumerservice)** veld in de Azure-portal.  
   ![image](./media/mitel-connect-tutorial/Mitel_Azure_BasicConfig.png)

7. In de **aanmeldings-URL** in het tekstvak, typ een van de volgende URL's:

    * **https://portal.shoretelsky.com** -de accountportal van Mitel gebruiken als de standaard Mitel-toepassing
    * **https://teamwork.shoretel.com** -Teamwerk gebruiken als de standaard Mitel-toepassing

    **OPMERKING**: De toepassing van de Mitel standaard is de toepassing die wordt geopend wanneer een gebruiker op de tegel Mitel verbinding maken in het toegangsvenster klikt. Dit is ook de toepassing die wordt gebruikt bij het uitvoeren van een test-installatie van Azure AD.

8. Klik op **opslaan** in de **SAML-basisconfiguratie** in het dialoogvenster in de Azure-portal.

9. In de **SAML-handtekeningcertificaat** sectie op de **SAML gebaseerde aanmelding** pagina in Azure portal, klikt u op **downloaden** naast **certificaat (Base64)** voor het downloaden van de **certificaat voor ondertekening van** en sla deze op uw computer.
    ![image](./media/mitel-connect-tutorial/Azure_SigningCert.png)

10. Open het certificaat voor ondertekening van bestand in een teksteditor, kopieert u alle gegevens in het bestand en plak vervolgens de gegevens in de **certificaat voor ondertekening van** veld in de accountportal van Mitel. 
    ![image](./media/mitel-connect-tutorial/Mitel_Connect_SigningCert.png)

11. In de **Setup Mitel verbinding** sectie op de **SAML gebaseerde aanmelding** pagina van de Azure-portal, doet u het volgende:

    a. Kopieer de URL van de **aanmeldings-URL** veld en plak deze in de **aanmelden URL** veld in de accountportal van Mitel.

    b. Kopieer de URL van de **Azure AD-id** veld en plak deze in de **entiteit-ID** veld in de accountportal van Mitel.
    ![image](./media/mitel-connect-tutorial/Mitel_Azure_SetupConnect.png)

12. Klik op **opslaan** op de **verbinding maken met Single Sign-On instellingen** in het dialoogvenster in de accountportal van Mitel.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

In deze sectie maakt u een testgebruiker Britta Simon met de naam in Azure portal.

1. Klik in de Azure-portal, in het linkerdeelvenster op **Azure Active Directory**, klikt u op **gebruikers**, en klik vervolgens op **alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Klik op **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. Voer de volgende stappen uit in het dialoogvenster Eigenschappen van gebruiker:

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. In de **naam** veld, typt u **BrittaSimon**.
  
    b. In de **gebruikersnaam** veld, typt u brittasimon @\<uwbedrijfsdomein\>.\< extensie\>.  
Bijvoorbeeld BrittaSimon@contoso.com.

    c. Selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door toegang te verlenen om Mitel verbinding te gebruiken.

1. Klik in de Azure-portal op **bedrijfstoepassingen**, en klik vervolgens op **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Klik in de lijst met toepassingen, op **Mitel verbinding**.

    ![De koppeling Mitel verbinding maken in de lijst met toepassingen](common/all-applications.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op **gebruiker toevoegen**, klikt u vervolgens op **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de **gebruikers** lijst en klik vervolgens op **Selecteer** aan de onderkant van het scherm.

6. Als u een waarde voor de rol in het SAML-verklaring verwacht, selecteert u de juiste rol voor de gebruiker in de lijst in de **rol selecteren** dialoogvenster en klik vervolgens op **Selecteer** aan de onderkant van het scherm.

7. In de **toevoegen toewijzing** dialoogvenster, klikt u op **toewijzen**.

### <a name="create-a-mitel-micloud-connect-test-user"></a>Maak een testgebruiker Mitel MiCloud verbinding maken

In deze sectie maakt u een gebruiker met de naam Britta Simon op uw account MiCloud verbinding maken. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding.

Zie voor meer informatie over het toevoegen van gebruikers in de accountportal van Mitel de [gebruiker toe te voegen](https://docs.shoretel.com/connectcloud/Account/Users/AddUser) Mitel Knowledge Base-artikel.

Een gebruiker maken voor uw account MiCloud verbinding maken met de volgende details:

  * **Naam:** Britta Simon

* **Zakelijke e-mailadres:** `brittasimon@<yourcompanydomain>.<extension>`   
(Voorbeeld: [ brittasimon@contoso.com ](mailto:brittasimon@contoso.com))

* **Gebruikersnaam:** `brittasimon@<yourcompanydomain>.<extension>`  
(Voorbeeld: [ brittasimon@contoso.com ](mailto:brittasimon@contoso.com); gebruikersnaam van de gebruiker is meestal hetzelfde als zakelijk e-mailadres van de gebruiker)

**OPMERKING:** Van de gebruiker MiCloud verbinding maken met gebruikersnaam moet identiek zijn aan e-mailadres van de gebruiker in Azure.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u op de tegel Mitel verbinding maken in het toegangsvenster, u automatisch moet worden omgeleid om aan te melden bij de toepassing MiCloud verbinding u hebt geconfigureerd als de standaard in de **aanmeldings-URL** veld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
