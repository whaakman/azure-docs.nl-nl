---
title: 'Zelfstudie: Integratie van Azure Active Directory met TargetProcess | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en TargetProcess kunt configureren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/7/2018
ms.author: jeedes
ms.openlocfilehash: 7fb3c1c3e8dbff54802915a69033001cc9378670
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53262160"
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a>Zelfstudie: Integratie van Azure Active Directory met TargetProcess

In deze zelfstudie leert u hoe u TargetProcess kunt integreren met Azure Active Directory (Azure AD).
Integratie van TargetProcess met Azure AD biedt u de volgende voordelen:

* U kunt in Azure AD regelen wie toegang heeft tot TargetProcess.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij TargetProcess (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om de integratie van Azure AD met TargetProcess te configureren:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op TargetProcess waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* TargetProcess biedt ondersteuning voor eenmalige aanmelding die is gestart vanuit **SP**
* TargetProcess biedt ondersteuning voor het **Just In Time** inrichten van gebruikers

## <a name="adding-targetprocess-from-the-gallery"></a>TargetProcess toevoegen vanuit de galerie

U moet TargetProcess vanuit de galerie aan uw lijst met beheerde SaaS-apps toevoegen om de integratie van TargetProcess in Azure AD te configureren.

**Voer de volgende stappen uit om TargetProcess vanuit de galerie toe te voegen:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **TargetProcess**, selecteer **TargetProcess** in het deelvenster met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![TargetProcess in de resultatenlijst](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding via Azure AD met TargetProcess op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in TargetProcess tot stand is gebracht.

U hebt de volgende bouwstenen nodig om eenmalige aanmelding van Azure AD met TargetProcess te configureren en testen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[TargetProcess-testgebruiker maken](#create-targetprocess-test-user)**: om een equivalent van Britta Simon te hebben in TargetProcess die is gekoppeld aan de bijbehorende gebruiker in Azure AD.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding in Azure AD te configureren voor TargetProcess:

1. Selecteer op de integratiepagina voor **TargetProcess** in [Azure Portal](https://portal.azure.com/) de optie **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over TargetProcess-domein en URL’s voor eenmalige aanmelding](common/sp-identifier.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.tpondemand.com/`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<subdomain>.tpondemand.com/`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id.  Neem contact op met het [klantenserviceteam van TargetProcess](mailto:support@targetprocess.com) om deze waarden op te halen.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Kopieer onder **TargetProcess instellen** de URL('s) die u nodig hebt.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

7. Als u de configuratie met **TargetProcess** wilt automatiseren, moet u **My Apps-browserextensie voor veilig aanmelden** installeren door op **De extensie installeren** te klikken.

    ![image](./media/target-process-tutorial/install_extension.png)

8. Als u op **TargetProcess instellen** klikt nadat u de extensie aan de browser hebt toegevoegd, wordt u doorgestuurd naar de TargetProcess-toepassing. Geef hier de referenties voor de beheerder op om u aan te melden bij TargetProcess. In de browserextensie wordt de toepassing automatisch voor u geconfigureerd en worden stappen 9-13 geautomatiseerd.

    **Voer de volgende stappen uit als u de toepassing handmatig wilt configureren:**

9. Meld u bij uw TargetProcess-toepassing aan als beheerder.

10. Klik in het menu bovenaan op **Instellen**.

    ![Instellen](./media/target-process-tutorial/tutorial_target_process_05.png)

11. Klik op **Instellingen**.

    ![Instellingen](./media/target-process-tutorial/tutorial_target_process_06.png)

12. Klik op **Eenmalige aanmelding**.

    ![klik op Eenmalige aanmelding](./media/target-process-tutorial/tutorial_target_process_07.png)

13. Voer in het dialoogvenster Instellingen voor eenmalige aanmelding de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/target-process-tutorial/tutorial_target_process_08.png)

    a. Klik op **Eenmalige aanmelding inschakelen**.

    b. Plak in het tekstvak **Aanmeldings-URL** de waarde van **SAML-service-URL voor eenmalige aanmelding** die u in Azure Portal hebt gekopieerd.

    c. Open het gedownloade certificaat in Kladblok, kopieer de inhoud en plak deze in het tekstvak **Certificaat**.

    d. Klik op **JIT-inrichting inschakelen**.

    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

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

In deze sectie stelt u Britta Simon in staat gebruik te maken van eenmalige aanmelding via Azure door haar toegang te geven tot TargetProcess.

1. Selecteer in Azure Portal **Bedrijfstoepassingen**, **Alle toepassingen** en selecteer vervolgens **TargetProcess**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **TargetProcess** in de lijst met toepassingen.

    ![De koppeling naar TargetProcess in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-targetprocess-test-user"></a>TargetProcess-testgebruiker maken

Dit gedeelte is bedoeld om een gebruiker met de naam Britta Simon te maken in TargetProcess. TargetProcess ondersteunt Just-In-Time-inrichting; deze optie is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Er wordt een nieuwe gebruiker gemaakt als TargetProcess wordt geopend en er nog geen gebruiker bestaat.

> [!Note]
> Neem contact op met het  [ondersteuningsteam van TargetProcess](mailto:support@targetprocess.com) als u handmatig een gebruiker moet maken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Als u op de tegel TargetProcess in het toegangsvenster klikt, wordt u automatisch aangemeld bij de instantie van TargetProcess waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)