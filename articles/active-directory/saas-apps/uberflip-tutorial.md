---
title: 'Zelfstudie: Azure Active Directory-integratie met Uberflip | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Uberflip.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 754b1f5b-6694-4fd6-9e1e-9fad769c64db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 69e86e486a9cdb058b972bda5176c14e15f4630a
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682724"
---
# <a name="tutorial-azure-active-directory-integration-with-uberflip"></a>Zelfstudie: Azure Active Directory-integratie met Uberflip

In deze zelfstudie leert u hoe u Uberflip integreren met Azure Active Directory (Azure AD).

Uberflip integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Uberflip heeft.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij Uberflip (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Uberflip, moet u de volgende items:

* Een Azure AD-abonnement Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Een abonnement Uberflip met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

Uberflip ondersteunt de volgende functies:

* SP geïnitieerde en IDP geïnitieerde eenmalige aanmelding (SSO).
* De Just-in-time gebruikersinrichting.

## <a name="add-uberflip-from-the-azure-marketplace"></a>Uberflip toevoegen in Azure Marketplace

Voor het configureren van de integratie van Uberflip in Azure AD, moet u Uberflip vanuit Azure Marketplace toevoegen aan uw lijst met beheerde SaaS-apps:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer de knop **Azure Active Directory** in het linkerdeelvenster.

   ![De Azure Active Directory-optie](common/select-azuread.png)

1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.

   ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Als u wilt een nieuwe toepassing toevoegen, selecteert u **+ nieuwe toepassing** aan de bovenkant van het deelvenster.

   ![De nieuwe optie voor de App](common/add-new-app.png)

1. Voer in het zoekvak **Uberflip**. Selecteer in de lijst met zoekresultaten **Uberflip**, en selecteer vervolgens **toevoegen** om toe te voegen van de toepassing.

   ![Uberflip in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Uberflip op basis van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Uberflip vast te stellen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Uberflip, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  zodat uw gebruikers deze functie wilt gebruiken.
1. **[Configureren van eenmalige aanmelding Uberflip](#configure-uberflip-single-sign-on)**  de instellingen voor eenmalige aanmelding configureren aan de toepassing.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Maak een testgebruiker Uberflip](#create-an-uberflip-test-user)**  zodat er een gebruiker met de naam Britta Simon in Uberflip die gekoppeld aan de Azure AD-gebruiker met de naam Britta Simon.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Uberflip, voert u de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com/)op de **Uberflip** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Optie voor eenmalige aanmelding configureren](common/select-sso.png)

1. In de **selecteert u een methode voor eenmalige aanmelding** deelvenster Selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

1. Op de **instellen van eenmalige aanmelding met SAML** venster **bewerken** (het potloodpictogram) te openen de **SAML-basisconfiguratie** deelvenster.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** deelvenster, voert u een van de volgende stappen uit, afhankelijk van welke SSO-modus die u wilt configureren:

   * Configureren van de toepassing in de IDP gestart door SSO-modus in de **antwoord-URL (URL van de Bevestigingsconsumerservice)** voert u een URL met behulp van het volgende patroon:

     `https://app.uberflip.com/sso/saml2/<IDPID>/<ACCOUNTID>`

     ![Uberflip domein en URL's, eenmalige aanmelding informatie](common/both-replyurl.png)

     > [!NOTE]
     > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke antwoord-URL. Als u de werkelijke waarde, neem contact op met de [Uberflip ondersteuningsteam](mailto:support@uberflip.com). U kunt ook verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** deelvenster in de Azure-portal.

   * Selecteer voor het configureren van de toepassing in de modus van de Serviceprovider geïnitieerde eenmalige aanmelding, **extra URL's instellen**, en klik in de **aanmeldings-URL** voert u deze URL:

     `https://app.uberflip.com/users/login`

     ![Uberflip domein en URL's, eenmalige aanmelding informatie](common/both-signonurl.png)

1. Op de **instellen van eenmalige aanmelding met SAML** deelvenster in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** voor het downloaden van de **federatieve metagegevens-XML**  uit de opgegeven opties en bewaar deze op uw computer.

   ![De optie voor het downloaden van federatieve metagegevens-XML](common/metadataxml.png)

1. In de **Uberflip instellen** deelvenster, Kopieer de URL of URL's die u nodig hebt:

   * **Aanmeldings-URL**
   * **Azure AD Identifier**
   * **Afmeldings-URL**

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-uberflip-single-sign-on"></a>Uberflip eenmalige aanmelding configureren

Voor het configureren van eenmalige aanmelding aan de Uberflip, die u nodig hebt voor het verzenden van het gedownloade federatieve metagegevens van XML-bestand en de juiste URL's gekopieerd vanuit Azure portal naar de [Uberflip ondersteuningsteam](mailto:support@uberflip.com). Het team Uberflip zorgt u ervoor dat de SAML SSO-verbinding aan beide zijden juist is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie gaat u een testgebruiker met de naam Britta Simon maken in de Azure-portal.

1. Selecteer in de Azure-portal aan de linkerkant **Azure Active Directory** > **Gebruikers** > **Alle gebruikers**.

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

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot Uberflip.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** > **Uberflip**.

    ![Deelvenster voor Enterprise applications](common/enterprise-applications.png)

1. Selecteer in de lijst met toepassingen, **Uberflip**.

    ![Uberflip in de lijst met toepassingen](common/all-applications.png)

1. In het linkerdeelvenster onder **beheren**, selecteer **gebruikers en groepen**.

    ![De optie 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **+ gebruiker toevoegen**, en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** deelvenster.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** venster **Britta Simon** in de **gebruikers** lijst en kies vervolgens **Selecteer** aan de onderkant van het deelvenster.

1. Als u verwacht een waarde voor de rol in het SAML-verklaring, klikt u vervolgens de **rol selecteren** deelvenster, selecteert u de juiste rol voor de gebruiker in de lijst. Kies aan de onderkant van het deelvenster **Selecteer**.

1. In de **toevoegen toewijzing** venster **toewijzen**.

### <a name="create-an-uberflip-test-user"></a>Maak een testgebruiker Uberflip

Een gebruiker met de naam Britta Simon is nu gemaakt in Uberflip. U hoeft te ondernemen om deze gebruiker te maken. Uberflip biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Als een gebruiker met de naam Britta Simon nog niet in Uberflip bestaat, wordt een nieuw gemaakt nadat verificatie.

> [!NOTE]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [Uberflip ondersteuningsteam](mailto:support@uberflip.com).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de portal mijn Apps.

Wanneer u selecteert **Uberflip** in de portal mijn Apps u moet worden automatisch aangemeld bij de Uberflip-abonnement waarvoor u van eenmalige aanmelding instellen. Zie voor meer informatie over de portal mijn Apps [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

* [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)