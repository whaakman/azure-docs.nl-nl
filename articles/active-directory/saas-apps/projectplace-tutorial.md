---
title: 'Zelfstudie: Azure Active Directory-integratie met Projectplace | Microsoft Docs'
description: In deze zelfstudie leert u hoe het configureren van eenmalige aanmelding tussen Azure Active Directory en Projectplace.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 17fbc6bc4f022a15c34c5ca7b9465be392cdc639
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560634"
---
# <a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Zelfstudie: Azure Active Directory-integratie met Projectplace

In deze zelfstudie leert u hoe u Projectplace integreert met Azure Active Directory (Azure AD).

Deze integratie biedt de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot Projectplace heeft gebruiken.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij Projectplace (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie [Eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) voor meer informatie over de integratie van SaaS-apps met Azure AD.

Als u een Azure-abonnement geen [Maak een gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Projectplace, hebt u het volgende nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u zich aanmelden voor een [proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/) abonnement.
* Een Projectplace-abonnement met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie configureert en Azure AD eenmalige aanmelding testen in een testomgeving.

* Projectplace biedt ondersteuning voor Serviceprovider geïnitieerde eenmalige aanmelding.

## <a name="add-projectplace-from-the-gallery"></a>Projectplace uit de galerie toevoegen

Als u de integratie van Projectplace in Azure AD instelt, moet u Projectplace uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**:

    ![Selecteer Azure Active Directory](common/select-azuread.png)

2. Ga naar **bedrijfstoepassingen** > **alle toepassingen**:

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing** aan de bovenkant van het venster:

    ![Nieuwe toepassing selecteren](common/add-new-app.png)

4. Voer in het zoekvak **Projectplace**. Selecteer **Projectplace** in de zoekresultaten en selecteer vervolgens **toevoegen**.

     ![Zoekresultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureren en testen van Azure AD eenmalige aanmelding met Projectplace met behulp van een testgebruiker met de naam Britta Simon.
Om in te schakelen eenmalige aanmelding, moet u een relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Projectplace vast te stellen.

Als u wilt configureren en Azure AD eenmalige aanmelding met Projectplace testen, moet u deze stappen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  om in te schakelen van de functie voor uw gebruikers.
2. **[Configureren van eenmalige aanmelding Projectplace](#configure-projectplace-single-sign-on)**  aan de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  zodat Azure AD eenmalige aanmelding voor de gebruiker.
5. **[Maak een testgebruiker Projectplace](#create-a-projectplace-test-user)**  dat gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie schakelt u Azure AD eenmalige aanmelding in de Azure-portal.

Voor het configureren van Azure AD eenmalige aanmelding met Projectplace, de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Projectplace** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**:

    ![Schakel eenmalige aanmelding](common/select-sso.png)

2. In de **selecteert u een methode voor eenmalige aanmelding** in het dialoogvenster, selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen:

    ![Selecteer een methode voor eenmalige aanmelding](common/select-saml-option.png)

3. Op de **instellen van eenmalige aanmelding met SAML** weergeeft, schakelt de **bewerken** pictogram opent de **SAML-basisconfiguratie** in het dialoogvenster:

    ![Pictogram bewerken](common/edit-urls.png)

4. In de **SAML-basisconfiguratie** in het dialoogvenster de **aanmeldings-URL** vak, een URL opgeven in dit patroon:

    `https://<company>.projectplace.com`

   ![In het dialoogvenster van Basic SAML-configuratie](common/sp-signonurl.png)
    > [!NOTE]
    > Deze waarde is een tijdelijke aanduiding. U moet de werkelijke aanmeldings-URL gebruiken. Neem contact op met de [Projectplace-ondersteuningsteam](https://success.planview.com/Projectplace/Support) om de waarde. U kunt ook verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** in het dialoogvenster in de Azure-portal.

5. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, selecteer de **downloaden** koppelen naast **federatieve metagegevens-XML** , overeenkomstig uw vereisten en sla het certificaat op uw computer:

    ![De koppeling om het certificaat te downloaden](common/metadataxml.png)

6. In de **instellen van Projectplace** sectie, kopieert u de juiste URL's, op basis van uw vereisten.

    ![De configuratie van URL's kopiëren](common/copy-configuration-urls.png)

    1. **Aanmeldings-URL**.

    1. **Azure AD Identifier**.

    1. **Afmeldings-URL van**.

### <a name="configure-projectplace-single-sign-on"></a>Projectplace eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op de **Projectplace** zijde, moet u voor het verzenden van de gedownloade **federatieve metagegevens-XML** certificaat en de URL's die u hebt gekopieerd uit de Azure-portal naar de [ Projectplace-ondersteuningsteam](https://success.planview.com/Projectplace/Support). Dit team zorgt ervoor dat de SAML SSO-verbinding aan beide zijden juist is ingesteld.

>[!NOTE]
>De configuratie voor eenmalige aanmelding moet worden uitgevoerd door de [Projectplace-ondersteuningsteam](https://success.planview.com/Projectplace/Support). U ontvangt een melding zodra de configuratie voltooid is.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker Britta Simon met de naam in Azure portal.

1. Selecteer in de Azure portal, **Azure Active Directory** selecteren in het linkerdeelvenster **gebruikers**, en selecteer vervolgens **alle gebruikers**:

    ![Selecteer alle gebruikers](common/users.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm:

    ![Nieuwe gebruiker selecteren](common/new-user.png)

3. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit.

    ![In het dialoogvenster](common/user-properties.png)

    1. Voer in het vak **Naam** **Britta Simon**in.
  
    1. In de **gebruikersnaam** Voer **BrittaSimon @\<uwbedrijfsdomein >.\< extensie >**. (Bijvoorbeeld BrittaSimon@contoso.com.)

    1. Selecteer **wachtwoord weergeven**, en noteer de waarde in de **wachtwoord** vak.

    1. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruik van Azure AD eenmalige aanmelding door haar toegang verlenen tot Projectplace.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**, en selecteer vervolgens **Projectplace**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen **Projectplace**.

    ![Lijst met toepassingen](common/all-applications.png)

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**:

    ![Gebruikers en groepen selecteren](common/users-groups-blade.png)

4. Selecteer **Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Gebruiker toevoegen selecteren](common/add-assign-user.png)

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. Als u een waarde voor de rol in het SAML-verklaring verwacht in de **rol selecteren** dialoogvenster Selecteer de juiste rol voor de gebruiker in de lijst. Klik op de **Selecteer** knop aan de onderkant van het scherm.

7. Selecteer **Toewijzen** in het dialoogvenster **Toewijzing toevoegen**.

### <a name="create-a-projectplace-test-user"></a>Maak een testgebruiker Projectplace

Om Azure AD-gebruikers zich aanmelden bij Projectplace, moet u ze toevoegen aan Projectplace. U moet deze handmatig toevoegen.

Voor het maken van een gebruikersaccount, de volgende stappen uitvoeren:

1. Aanmelden bij uw **Projectplace** bedrijf site als een beheerder.

2. Ga naar **mensen**, en selecteer vervolgens **leden**:
   
    ![Ga naar personen, en selecteer vervolgens leden](./media/projectplace-tutorial/ic790228.png "personen")

3. Selecteer **lid toevoegen**:
   
    ![Selecteer een lid toevoegen](./media/projectplace-tutorial/ic790232.png "leden toevoegen")

4. In de **Add Member** sectie, de volgende stappen uitvoeren.
   
    ![Toevoegen van lid sectie](./media/projectplace-tutorial/ic790233.png "nieuwe leden")
   
    1. In de **nieuwe leden** voert u het e-mailadres van een geldige Azure AD-account dat u wilt toevoegen.
   
    1. Selecteer **Verzenden**.

   Een e-mailbericht met een koppeling om te bevestigen dat het account voordat deze actief is verzonden naar de houder van Azure AD-account.

>[!NOTE]
>U kunt ook een ander gebruikersaccount maken-hulpprogramma gebruiken of API wordt geleverd door Projectplace Azure AD-gebruikersaccounts wilt toevoegen.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Nu moet u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de Projectplace-tegel in het toegangsvenster selecteert, moet u worden automatisch aangemeld met het Projectplace-exemplaar waarvoor u eenmalige aanmelding hebt ingesteld. Zie voor meer informatie, [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende bronnen

- [Zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
