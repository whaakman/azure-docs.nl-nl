---
title: 'Zelfstudie: Azure Active Directory-integratie met TigerText Secure Messenger | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TigerText Secure Messenger.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: 840b1fe55556cfd853e0928164891d6b21b17cc2
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956864"
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Zelfstudie: Azure Active Directory-integratie met TigerText Secure Messenger

In deze zelfstudie leert u hoe u TigerText beveiligen Messenger integreren met Azure Active Directory (Azure AD).

TigerText Secure Messenger integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot TigerText Secure Messenger heeft.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij TigerText Secure Messenger (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TigerText Secure Messenger, moet u de volgende items:

* Een Azure AD-abonnement Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Een abonnement TigerText Secure Messenger met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureert en Azure AD eenmalige aanmelding testen in een testomgeving en TigerText Secure Messenger integreren met Azure AD.

TigerText Secure Messenger ondersteunt Serviceprovider geïnitieerde eenmalige aanmelding (SSO).

## <a name="add-tigertext-secure-messenger-from-the-azure-marketplace"></a>Beveiligde Messenger TigerText toevoegen in Azure Marketplace

Voor het configureren van de integratie van TigerText Secure Messenger in Azure AD, moet u TigerText beveiligen Messenger vanuit Azure Marketplace toevoegen aan uw lijst met beheerde SaaS-apps:

1. Meld u aan bij [Azure Portal](https://portal.azure.com?azure-portal=true).
1. Selecteer de knop **Azure Active Directory** in het linkerdeelvenster.

    ![De Azure Active Directory-optie](common/select-azuread.png)

1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Als u wilt een nieuwe toepassing toevoegen, selecteert u **+ nieuwe toepassing** aan de bovenkant van het deelvenster.

    ![De nieuwe optie voor de App](common/add-new-app.png)

1. Voer in het zoekvak **TigerText Secure Messenger**. Selecteer in de lijst met zoekresultaten **TigerText Secure Messenger**, en selecteer vervolgens **toevoegen** om toe te voegen van de toepassing.

    ![TigerText Secure Messenger in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met TigerText Secure Messenger op basis van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in TigerText Secure Messenger.

Om te configureren en testen van Azure AD eenmalige aanmelding met TigerText Secure Messenger, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  zodat uw gebruikers deze functie wilt gebruiken.
1. **[Configureren van eenmalige aanmelding TigerText Secure Messenger](#configure-tigertext-secure-messenger-single-sign-on)**  de instellingen voor eenmalige aanmelding configureren aan de toepassing.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Maak een testgebruiker TigerText Secure Messenger](#create-a-tigertext-secure-messenger-test-user)**  zodat er een gebruiker met de naam Britta Simon TigerText Secure Messenger die gekoppeld aan de Azure AD-gebruiker met de naam Britta Simon.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met TigerText Secure Messenger, voert u de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com/)op de **TigerText Secure Messenger** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Optie voor eenmalige aanmelding configureren](common/select-sso.png)

1. Op de **selecteert u een methode voor eenmalige aanmelding** deelvenster Selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

1. Op de **instellen van eenmalige aanmelding met SAML** venster **bewerken** (het potloodpictogram) te openen de **SAML-basisconfiguratie** deelvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de **SAML-basisconfiguratie** in het deelvenster de volgende stappen uit:

    ![TigerText Secure Messenger-domein en URL's eenmalige aanmelding informatie](common/sp-identifier.png)

    1. In de **aanmeldings-URL** voert u een URL:

       `https://home.tigertext.com`

    1. In de **id (entiteits-ID)** vak, typ een URL met behulp van het volgende patroon:

       `https://saml-lb.tigertext.me/v1/organization/<instance ID>`

    > [!NOTE]
    > De **id (entiteits-ID)** waarde is niet echt. Deze waarde bijwerken met de werkelijke-id. Als u de waarde, neem contact op met de [TigerText Secure Messenger-ondersteuningsteam](mailto:prosupport@tigertext.com). U kunt ook verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** deelvenster in de Azure-portal.

1. Op de **instellen van eenmalige aanmelding met SAML** deelvenster in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** voor het downloaden van de **federatieve metagegevens-XML**  uit de opgegeven opties en bewaar deze op uw computer.

    ![De optie voor het downloaden van federatieve metagegevens-XML](common/metadataxml.png)

1. In de **TigerText Secure Messenger instellen** sectie, Kopieer de URL of URL's die u nodig hebt:

   * **Aanmeldings-URL**
   * **Azure AD Identifier**
   * **Afmeldings-URL**

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-tigertext-secure-messenger-single-sign-on"></a>Configureren van eenmalige aanmelding TigerText Secure Messenger

Voor het configureren van eenmalige aanmelding aan de TigerText Secure Messenger, die u nodig hebt voor het verzenden van het gedownloade federatieve metagegevens van XML-bestand en de juiste URL's gekopieerd vanuit Azure portal naar de [TigerText Secure Messenger-ondersteuningsteam](mailto:prosupport@tigertext.com). Het team TigerText Secure Messenger zorgt u ervoor dat de SAML SSO-verbinding aan beide zijden juist is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie gaat u een testgebruiker met de naam Britta Simon maken in de Azure-portal.

1. Selecteer in de Azure-portal aan de linkerkant **Azure Active Directory**   > **Gebruikers** > **Alle gebruikers**.

    ![De gebruikers- en 'Alle gebruikers' opties](common/users.png)

1. Aan de bovenkant van het scherm, selecteer **+ nieuwe gebruiker**.

    ![Nieuwe Gebruikersoptie](common/new-user.png)

1. In de **gebruiker** in het deelvenster de volgende stappen uit:

    ![De gebruiker-deelvenster](common/user-properties.png)

    1. Voer in het vak **Naam** **Britta Simon**in.
  
    1. In de **gebruikersnaam** Voer **BrittaSimon\@\<uwbedrijfsdomein >.\< extensie >**. Bijvoorbeeld, **BrittaSimon\@contoso.com**.

    1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.

    1. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken ze door toegang te verlenen tot TigerText Secure Messenger.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** > **TigerText Secure Messenger**.

    ![Deelvenster voor Enterprise applications](common/enterprise-applications.png)

1. Selecteer in de lijst met toepassingen, **TigerText Secure Messenger**.

    ![TigerText Secure Messenger in de lijst met toepassingen](common/all-applications.png)

1. In het linkerdeelvenster onder **beheren**, selecteer **gebruikers en groepen**.

    ![De optie 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **+ gebruiker toevoegen**, en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** deelvenster.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** venster **Britta Simon** in de **gebruikers** lijst en kies vervolgens **Selecteer** aan de onderkant van het deelvenster.

1. Als u verwacht een waarde voor de rol in het SAML-verklaring, klikt u vervolgens de **rol selecteren** deelvenster, selecteert u de juiste rol voor de gebruiker in de lijst. Kies aan de onderkant van het deelvenster **Selecteer**.

1. In de **toevoegen toewijzing** venster **toewijzen**.

### <a name="create-a-tigertext-secure-messenger-test-user"></a>Maak een testgebruiker TigerText Secure Messenger

In deze sectie maakt u een gebruiker met de naam van Britta Simon TigerText Secure Messenger. Werken met de [TigerText Secure Messenger-ondersteuningsteam](mailto:prosupport@tigertext.com) Britta Simon toevoegen als een gebruiker in TigerText Secure Messenger. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de portal mijn Apps.

Wanneer u selecteert **TigerText Secure Messenger** in de portal mijn Apps u moet worden automatisch aangemeld bij de TigerText Secure Messenger-abonnement waarvoor u van eenmalige aanmelding instellen. Zie voor meer informatie over de portal mijn Apps [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies voor het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

* [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
