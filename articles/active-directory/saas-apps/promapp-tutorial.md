---
title: 'Zelfstudie: Azure Active Directory-integratie met Promapp | Microsoft Docs'
description: In deze zelfstudie leert u hoe het configureren van eenmalige aanmelding tussen Azure Active Directory en Promapp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: e91351d4571eaa084865c5a179ed05e6c773b952
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240413"
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a>Zelfstudie: Azure Active Directory-integratie met Promapp

In deze zelfstudie leert u hoe u Promapp integreert met Azure Active Directory (Azure AD).
Deze integratie biedt de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot Promapp heeft gebruiken.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij Promapp (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie [Eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Promapp, moet u beschikken over:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u zich aanmelden voor een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Een Promapp-abonnement met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie configureert en Azure AD eenmalige aanmelding testen in een testomgeving.

* Promapp ondersteunt SP geïnitieerde en IdP gestart door SSO.

* Promapp ondersteunt het inrichten van just-in-time-gebruikers.

## <a name="add-promapp-from-the-gallery"></a>Promapp uit de galerie toevoegen

Als u de integratie van Promapp in Azure AD instelt, moet u Promapp uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**:

    ![Selecteer Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen** > **alle toepassingen**:

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing** aan de bovenkant van het venster:

    ![Nieuwe toepassing selecteren](common/add-new-app.png)

4. Voer in het zoekvak **Promapp**. Selecteer **Promapp** in de zoekresultaten en selecteer vervolgens **toevoegen**.

     ![Zoekresultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureren en testen van Azure AD eenmalige aanmelding met Promapp met behulp van een testgebruiker met de naam Britta Simon.
Om in te schakelen eenmalige aanmelding, moet u een relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Promapp vast te stellen.

Als u wilt configureren en Azure AD eenmalige aanmelding met Promapp testen, moet u deze stappen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  om in te schakelen van de functie voor uw gebruikers.
2. **[Configureren van eenmalige aanmelding Promapp](#configure-promapp-single-sign-on)**  aan de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  zodat Azure AD eenmalige aanmelding voor de gebruiker.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie schakelt u Azure AD eenmalige aanmelding in de Azure-portal.

Voor het configureren van Azure AD eenmalige aanmelding met Promapp, de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/), selecteert u op de pagina Promapp toepassingen integratie **eenmalige aanmelding**:

    ![Schakel eenmalige aanmelding](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen:

    ![Selecteer een methode voor eenmalige aanmelding](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** pictogram opent de **SAML-basisconfiguratie** in het dialoogvenster:

    ![Pictogram bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** in het dialoogvenster, als u wilt configureren van de toepassing in de modus voor IdP gestart door de volgende stappen uitvoeren.

    ![In het dialoogvenster van Basic SAML-configuratie](common/idp-intiated.png)

    1. In de **id** vak, een URL opgeven in dit patroon:

       | |
        |--|
        | `https://go.promapp.com/TENANTNAME/`|
        | `https://au.promapp.com/TENANTNAME/`|
        | `https://us.promapp.com/TENANTNAME/`|
        | `https://eu.promapp.com/TENANTNAME/`|
        | `https://ca.promapp.com/TENANTNAME/`|
        |   |

       > [!NOTE]
       > Azure AD-integratie met Promapp is momenteel alleen geconfigureerd voor verificatie van service gestart. (Dat wil zeggen, start te gaan naar een URL Promapp het verificatieproces uit.) Maar de **antwoord-URL** veld is een verplicht veld.

    1. In de **antwoord-URL** vak, een URL opgeven in dit patroon:

       `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate.aspx`

5. Als u configureren van de toepassing in de modus SP geïnitieerde wilt, selecteert u **extra URL's instellen**. In de **aanmeldings-URL** vak, een URL opgeven in dit patroon:

      `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate`

    ![Promapp domein en URL's, eenmalige aanmelding informatie](common/metadata-upload-additional-signon.png)

   

    > [!NOTE]
    > Deze waarden zijn tijdelijke aanduidingen. U moet gebruiken de werkelijke-id, antwoord-URL en aanmeldings-URL. Neem contact op met de [Promapp ondersteuningsteam](https://www.promapp.com/about-us/contact-us/) om de waarden te verkrijgen. U kunt ook verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** in het dialoogvenster in de Azure-portal.

6. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer de **downloaden** koppelen naast **certificaat (Base64)** , overeenkomstig uw vereisten en sla het certificaat op uw computer:

    ![De koppeling om het certificaat te downloaden](common/certificatebase64.png)

7. In de **Promapp instellen** sectie, kopieert u de juiste URL's, op basis van uw vereisten:

    ![De configuratie van URL's kopiëren](common/copy-configuration-urls.png)

    1. **Aanmeldings-URL**.

    1. **Azure AD Identifier**.

    1. **Afmeldings-URL van**.

### <a name="configure-promapp-single-sign-on"></a>Promapp eenmalige aanmelding configureren

1. Aanmelden bij uw bedrijf Promapp site als een beheerder.

2. Selecteer in het menu aan de bovenkant van het venster **Admin**:
   
    ![Selecteer beheerder][12]

3. Selecteer **configureren**:
   
    ![Selecteer configureren][13]

4. In de **Security** dialoogvenster vak, voer de volgende stappen uit.
   
    ![Het dialoogvenster Beveiliging][14]
    
    1. Plak de **aanmeldings-URL** die u hebt gekopieerd vanuit de Azure portal in de **SSO-aanmeldings-URL** vak.
    
    1. In de **SSO - modus voor eenmalige aanmelding** in de lijst met **optioneel**. Selecteer **Opslaan**.

       > [!NOTE]
       > Optionele modus is alleen voor testdoeleinden. Nadat u tevreden met de configuratie bent, selecteert u **vereist** in de **SSO - modus voor eenmalige aanmelding** lijst om af te dwingen van alle gebruikers om te verifiëren met Azure AD.

    1. Open in Kladblok het certificaat dat u in de vorige sectie hebt gedownload. Kopieer de inhoud van het certificaat zonder de eerste regel ( **---BEGIN CERTIFICATE---** ) of de laatste regel ( **---END CERTIFICATE---** ). Plak de inhoud in certificaat de **SSO-x.509-certificaat** vak en selecteer vervolgens **opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker Britta Simon met de naam in Azure portal.

1. Selecteer in de Azure portal, **Azure Active Directory** selecteren in het linkerdeelvenster **gebruikers**, en selecteer vervolgens **alle gebruikers**:

    ![Selecteer alle gebruikers](common/users.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm:

    ![Nieuwe gebruiker selecteren](common/new-user.png)

3. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit.

    ![In het dialoogvenster](common/user-properties.png)

    1. Voer in het vak **Naam** **Britta Simon**in.
  
    1. In de **gebruikersnaam** Voer **BrittaSimon @\<uwbedrijfsdomein >.\< extensie >** . (Bijvoorbeeld BrittaSimon@contoso.com.)

    1. Selecteer **wachtwoord weergeven**, en noteer de waarde in de **wachtwoord** vak.

    1. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door haar toegang verlenen tot Promapp gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **Promapp**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **Promapp**.

    ![Lijst met toepassingen](common/all-applications.png)

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**:

    ![Gebruikers en groepen selecteren](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Gebruiker toevoegen selecteren](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. Als u een waarde voor de rol in het SAML-verklaring verwacht in de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst. Klik op de **Selecteer** knop aan de onderkant van het scherm.

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

### <a name="just-in-time-user-provisioning"></a>De Just-in-time gebruikersinrichting

Promapp ondersteunt het inrichten van just-in-time-gebruikers. Deze functie is standaard ingeschakeld. Als een gebruiker nog niet in Promapp bestaat, wordt een nieuw gemaakt nadat verificatie.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Nu moet u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel Promapp in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Promapp-exemplaar waarvoor u eenmalige aanmelding hebt ingesteld. Zie voor meer informatie over het toegangsvenster, [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

- [Zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[12]: ./media/promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/promapp-tutorial/tutorial_promapp_07.png
