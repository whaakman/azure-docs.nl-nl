---
title: 'Zelfstudie: Integratie van Azure Active Directory met Zscaler Beta | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en Zscaler Beta.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07341c1ad30f1242bdff430826fdc82c45e09dac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086070"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Zelfstudie: Integratie van Azure Active Directory met Zscaler Beta

In deze zelfstudie leert u hoe u Zscaler Beta kunt integreren met Azure Active Directory (Azure AD).
Wanneer u Zscaler Bèta met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Zscaler Beta heeft.
* Toestaan dat uw gebruikers automatisch worden aangemeld bij Zscaler Bèta met hun Azure AD-accounts. Dit toegangsbeheer wordt eenmalige aanmelding (SSO) genoemd.
* Uw accounts in één centrale locatie beheren met behulp van de Azure-portal.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u integratie tussen Azure AD met Zscaler Beta wilt configureren, hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Een Zscaler bèta-abonnement dat gebruikmaakt van eenmalige aanmelding.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Zscaler Beta biedt ondersteuning voor Serviceprovider geïnitieerde eenmalige aanmelding.
* Zscaler Beta biedt ondersteuning voor de just-in-time gebruikersinrichting.

## <a name="add-zscaler-beta-from-the-azure-marketplace"></a>Zscaler Bèta toevoegen in Azure Marketplace

Toevoegen voor het configureren van de integratie van Zscaler Beta in Azure AD, Zscaler bèta van de Azure Marketplace aan uw lijst met beheerde SaaS-apps.

Volg deze stappen om toe te voegen Zscaler bèta van de Azure Marketplace.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het navigatiedeelvenster links **Azure Active Directory**.

    ![Knop Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u een nieuwe toepassing wilt toevoegen, selecteert u **Nieuwe toepassing** bovenaan het dialoogvenster.

    ![Knop Nieuwe toepassing](common/add-new-app.png)

4. Voer in het zoekvak **Zscaler Bèta**. Selecteer **Zscaler Bèta** vanuit het deelvenster met resultaten en selecteer vervolgens **toevoegen**.

     ![Zscaler Beta in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Zscaler Beta op basis van de testgebruiker Britta Simon.
Voor eenmalige aanmelding in om te werken, tot stand brengen van een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker Zscaler bètaversies.

Als u wilt configureren en Azure AD eenmalige aanmelding met Zscaler bètaversie testen, voert u de volgende bouwstenen:

- [Eenmalige aanmelding van Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie kunnen gebruiken.
- [Configureren van eenmalige aanmelding Zscaler Bèta](#configure-zscaler-beta-single-sign-on) de instellingen voor eenmalige aanmelding configureren aan de toepassing.
- [Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user) voor het testen van eenmalige aanmelding van Azure AD met Britta Simon.
- [De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user) zodat Britta Simon gebruik kan maken van eenmalige aanmelding van Azure AD.
- [Maak een testgebruiker Zscaler Bèta](#create-a-zscaler-beta-test-user) hebben een equivalent van Britta Simon Zscaler bètaversies die gekoppeld aan de Azure AD-weergave van de gebruiker.
- [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Volg deze stappen voor het configureren van Azure AD eenmalige aanmelding met Zscaler Bèta.

1. In de [Azure-portal](https://portal.azure.com/) selecteert u **Zscaler Beta** op de integratiepagina van de toepassing **Eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. Selecteer in het dialoogvenster **Selecteer een methode voor eenmalige aanmelding** de modus **SAML/WS-Fed** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt **bewerken** openen de **SAML-basisconfiguratie** in het dialoogvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** sectie, volgt u deze stap:

    ![Zscaler bèta-domein en URL's eenmalige aanmelding informatie](common/sp-intiated.png)

    - In de **aanmeldings-URL** vak, voer de URL die wordt gebruikt door uw gebruikers zich aanmeldt bij uw toepassing Zscaler Beta.

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmelding URL-waarde. Als u de waarde, neem contact op met de [Zscaler bèta-client-ondersteuningsteam](https://www.zscaler.com/company/contact).

5. De toepassing Zscaler Beta wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. U moet aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Selecteer **bewerken** openen de **gebruikerskenmerken** in het dialoogvenster.

    ![In het dialoogvenster kenmerken](common/edit-attribute.png)

6. De toepassing Zscaler Beta wordt verwacht dat een paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de **gebruikersclaims** sectie de **gebruikerskenmerken** in het dialoogvenster, volg deze stappen om toe te voegen van het kenmerk van de SAML-token, zoals wordt weergegeven in de volgende tabel.
    
    | Name | Bronkenmerk | 
    | ---------------| --------------- |
    | memberOf  | user.assignedroles |

    a. Selecteer **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![In het dialoogvenster claims](common/new-save-attribute.png)

    ![Het dialoogvenster Gebruikersclaims beheren](common/new-attribute-details.png)

    b. Typ in het tekstvak **Naam** de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat de **Namespace** vak leeg.

    d. Voor **bron**, selecteer **kenmerk**.

    e. Typ in de lijst **Bronkenmerk** de kenmerkwaarde voor die rij.

    f. Selecteer **OK**.

    g. Selecteer **Opslaan**.

    > [!NOTE]
    > Zie voor informatie over het configureren van rollen in Azure AD, [configureren van de claim rol](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management).

7. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** voor het downloaden van de **certificaat (Base64)** . Sla deze op uw computer.

    ![De koppeling om het certificaat te downloaden](common/certificatebase64.png)

8. In de **Zscaler bèta instellen** sectie, Kopieer de URL's die u nodig hebt voor uw vereisten:

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    - Aanmeldings-URL
    - Azure AD-id
    - Afmeldings-URL

### <a name="configure-zscaler-beta-single-sign-on"></a>Configureren van eenmalige aanmelding Zscaler Beta

1. Installeren voor het automatiseren van de configuratie in Zscaler Bèta, **mijn Apps beveiligde aanmelding browserextensie** hiervoor **de extensie installeren**.

    ![Mijn Apps-extensie](common/install-myappssecure-extension.png)

2. Nadat u de extensie aan de browser toevoegen, selecteren **Zscaler bèta instellen** stuurt u naar de bètaversie van de Zscaler-toepassing. Van daaruit, bieden de referenties van de beheerder zich aanmeldt bij Zscaler Beta. De uitbreiding van de browser configureert u de toepassing voor u automatisch en stappen 3 tot en met 6 automatiseert.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u Zscaler bèta handmatig instelt, open een nieuw browservenster. Aanmelden bij uw site van het bedrijf Zscaler bèta als beheerder en volg deze stappen.

4. Ga naar **beheer** > **verificatie** > **verificatie-instellingen**, en volg deze stappen.
   
    ![Beheer](./media/zscaler-beta-tutorial/ic800206.png "Beheer")

    a. Onder **verificatietype**, selecteer **SAML**.

    b. Selecteer **SAML configureren**.

5. In de **bewerken SAML** venster als volgt te werk: 
            
    ![Manage Users & Authentication](./media/zscaler-beta-tutorial/ic800208.png "Manage Users & Authentication")
    
    a. In de **Portal-URL voor SAML** vak, plak het **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    b. In de **aanmelding naamkenmerk** Voer **NameID**.

    c. In de **openbare SSL-certificaat** Schakel **uploaden** voor het uploaden van het Azure-SAML-handtekeningcertificaat die u hebt gedownload van de Azure-portal.

    d. In-/ uitschakelen **SAML automatische inrichting inschakelen**.

    e. In de **Gebruikersnaamkenmerk weergave** Voer **displayName** als u wilt inschakelen, SAML-autoprovisioning voor displayName kenmerken.

    f. In de **groep naamkenmerk** Voer **memberOf** als u wilt inschakelen, SAML-autoprovisioning voor memberOf kenmerken.

    g. In de **afdeling naamkenmerk** Voer **afdeling** als u wilt inschakelen, SAML-autoprovisioning voor kenmerken van de afdeling.

    h. Selecteer **Opslaan**.

6. Op de **gebruikersverificatie configureren** dialoogvenster pagina, als volgt te werk:

    ![Menu van de activering en de knop activeren](./media/zscaler-beta-tutorial/ic800207.png)

    a. Beweeg de muisaanwijzer over de **activering** menu in de linkerbenedenhoek.

    b. Selecteer **activeren**.

## <a name="configure-proxy-settings"></a>Proxy-instellingen configureren
Volg deze stappen voor het configureren van de proxy-instellingen in Internet Explorer.

1. Start **Internet Explorer**.

2. Selecteer **Internetopties** uit de **extra** menu te openen de **Internetopties** in het dialoogvenster. 
    
     ![In het dialoogvenster Internetopties](./media/zscaler-beta-tutorial/ic769492.png "Internetopties")

3. Selecteer de **verbindingen** tabblad. 
  
     ![Tabblad verbindingen](./media/zscaler-beta-tutorial/ic769493.png "verbindingen")

4. Selecteer **LAN-instellingen** openen de **Local Area Network (LAN)-instellingen** in het dialoogvenster.

5. In de **proxyserver** sectie, als volgt te werk: 
   
    ![Proxy server sectie](./media/zscaler-beta-tutorial/ic769494.png "proxyserver")

    a. Selecteer de **een proxyserver gebruiken voor uw LAN** selectievakje.

    b. In de **adres** Voer **gateway. Zscaler Beta.net**.

    c. In de **poort** Voer **80**.

    d. Selecteer de **proxyserver niet gebruiken voor lokale adressen** selectievakje.

    e. Selecteer **OK** sluiten de **Local Area Network (LAN)-instellingen** in het dialoogvenster.

6. Selecteer **OK** sluiten de **Internetopties** in het dialoogvenster.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Maak een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure-portal aan de linkerkant **Azure Active Directory** > **Gebruikers** > **Alle gebruikers**.

    ![Gebruikers en alle gebruikerskoppelingen](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![De knop Nieuwe gebruiker](common/new-user.png)

3. In de **gebruiker** dialoogvenster vak, als volgt te werk:

    ![In het dialoogvenster](common/user-properties.png)

    a. Voer in het vak **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** Voer `brittasimon@yourcompanydomain.extension`. Een voorbeeld is BrittaSimon@contoso.com.

    c. Selecteer de **Show wachtwoord** selectievakje. Noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

Schakel Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Zscaler Beta.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** > **Zscaler Bèta**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Voer in de lijst met toepassingen, en selecteer **Zscaler Bèta**.

    ![Zscaler bèta-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![Koppeling van gebruikers en groepen](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen**. In de **toevoegen toewijzing** in het dialoogvenster, selecteer **gebruikers en groepen**.

    ![Gebruikersknop toevoegen](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer de gebruiker, zoals **Britta Simon** in de lijst. Kies vervolgens **Selecteer** aan de onderkant van het scherm.

    ![Gebruikers en groepen in het dialoogvenster](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_users.png)

6. In de **rol selecteren** dialoogvenster vak, selecteert u de desbetreffende gebruikersrol in de lijst. Kies vervolgens **Selecteer** aan de onderkant van het scherm.

    ![Selecteer in het dialoogvenster rol](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_roles.png)

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het dialoogvenster toewijzing toevoegen](./media/zscaler-beta-tutorial/tutorial_zscalerbeta_assign.png)

### <a name="create-a-zscaler-beta-test-user"></a>Maak een testgebruiker Zscaler Beta

In deze sectie wordt de gebruiker Britta Simon gemaakt in de bètafase Zscaler. Zscaler Beta biedt ondersteuning voor **Just-In-Time-inrichting van gebruikers**. Deze functie is standaard ingeschakeld. Er is niets te doen in deze sectie. Als er nog geen gebruiker in Zscaler Beta bestaat, wordt er een nieuwe gemaakt nadat deze is geverifieerd.

>[!Note]
>Voor het handmatig maken van een gebruiker, neem contact op met de [Zscaler bèta-ondersteuningsteam](https://www.zscaler.com/company/contact).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

Test uw Azure AD eenmalige aanmelding-configuratie met behulp van het toegangsvenster.

Wanneer u de Zscaler bèta-tegel in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Zscaler-bètaversie waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

