---
title: 'Zelfstudie: Azure Active Directory-integratie met PageDNA | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en PageDNA.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c8765864-45f4-48c2-9d86-986a4aa431e4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c7f8c8efcad0a07a3d3a56925866b10d94f82ed
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68227465"
---
# <a name="tutorial-azure-active-directory-integration-with-pagedna"></a>Zelfstudie: Azure Active Directory-integratie met PageDNA

In deze zelfstudie leert u hoe u PageDNA integreren met Azure Active Directory (Azure AD).

PageDNA integreren met Azure AD biedt u de volgende voordelen:

* In Azure AD, kunt u bepalen wie toegang heeft tot PageDNA.
* U kunt uw gebruikers kunnen automatisch worden aangemeld bij PageDNA (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met PageDNA, moet u de volgende items:

* Een Azure AD-abonnement Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Een abonnement PageDNA met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureert en Azure AD eenmalige aanmelding testen in een testomgeving en PageDNA integreren met Azure AD.

PageDNA ondersteunt de volgende functies:

* Serviceprovider geïnitieerde eenmalige aanmelding (SSO).

* De Just-in-time gebruikersinrichting.

## <a name="add-pagedna-from-the-azure-marketplace"></a>PageDNA toevoegen in Azure Marketplace

Voor het configureren van de integratie van PageDNA in Azure AD, moet u PageDNA vanuit Azure Marketplace toevoegen aan uw lijst met beheerde SaaS-apps:

1. Meld u aan bij [Azure Portal](https://portal.azure.com?azure-portal=true).
1. Selecteer de knop **Azure Active Directory** in het linkerdeelvenster.

    ![De Azure Active Directory-optie](common/select-azuread.png)

1. Ga naar **Bedrijfstoepassingen** en selecteer vervolgens **Alle toepassingen**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Als u wilt een nieuwe toepassing toevoegen, selecteert u **+ nieuwe toepassing** aan de bovenkant van het deelvenster.

    ![De nieuwe optie voor de App](common/add-new-app.png)

1. Voer in het zoekvak **PageDNA**. Selecteer in de lijst met zoekresultaten **PageDNA**, en selecteer vervolgens **toevoegen** om toe te voegen van de toepassing.

    ![PageDNA in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met PageDNA op basis van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in PageDNA.

Om te configureren en testen van Azure AD eenmalige aanmelding met PageDNA, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  zodat uw gebruikers deze functie wilt gebruiken.
1. **[Configureren van eenmalige aanmelding PageDNA](#configure-pagedna-single-sign-on)**  de instellingen voor eenmalige aanmelding configureren aan de toepassing.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Maak een testgebruiker PageDNA](#create-a-pagedna-test-user)**  zodat er een gebruiker met de naam Britta Simon in PageDNA die gekoppeld aan de Azure AD-gebruiker met de naam Britta Simon.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met PageDNA, voert u de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com/)op de **PageDNA** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Optie voor eenmalige aanmelding configureren](common/select-sso.png)

1. In de **selecteert u een methode voor eenmalige aanmelding** deelvenster Selecteer **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

1. Op de **instellen van eenmalige aanmelding met SAML** venster **bewerken** (het potloodpictogram) te openen de **SAML-basisconfiguratie** deelvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de **SAML-basisconfiguratie** in het deelvenster de volgende stappen uit:

    ![PageDNA domein en URL's, eenmalige aanmelding informatie](common/sp-identifier.png)

    1. In de **aanmeldings-URL** voert u een URL met behulp van een van de volgende patronen:

        ||
        |--|
        | `https://stores.pagedna.com/<your site>` |
        | `https://<your domain>` |
        | `https://<your domain>/<your site>` |
        | `https://www.nationsprint.com/<your site>` |
        | |

    1. In de **id (entiteits-ID)** voert u een URL met behulp van een van de volgende patronen:

        ||
        |--|
        | `https://stores.pagedna.com/<your site>/saml2ep.cgi` |
        | `https://www.nationsprint.com/<your site>/saml2ep.cgi` |
        | |

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Als u deze waarden, neem contact op met de [PageDNA ondersteuningsteam](mailto:success@pagedna.com). U kunt ook verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** deelvenster in de Azure-portal.

1. In de **instellen van eenmalige aanmelding met SAML** deelvenster in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** downloaden **certificaat (Raw)** uit de opgegeven opties en bewaar deze op uw computer.

    ![De optie voor het certificaat (Raw) downloaden](common/certificateraw.png)

1. In de **PageDNA instellen** sectie, Kopieer de URL of URL's die u nodig hebt:

   * **Aanmeldings-URL**
   * **Azure AD Identifier**
   * **Afmeldings-URL**

    ![De configuratie van URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-pagedna-single-sign-on"></a>PageDNA eenmalige aanmelding configureren

Voor het configureren van eenmalige aanmelding aan de PageDNA, door het gedownloade certificaat (Raw) en de juiste gekopieerde URL's te verzenden vanuit de Azure-portal naar de [PageDNA ondersteuningsteam](mailto:success@pagedna.com). Het team PageDNA zorgt u ervoor dat de SAML SSO-verbinding aan beide zijden juist is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In dit gedeelte gaat u een testgebruiker met de naam Britta Simon maken in de Azure-portal.

1. Selecteer in de Azure-portal aan de linkerkant **Azure Active Directory**   > **Gebruikers** > **Alle gebruikers**.

    ![De gebruikers- en 'Alle gebruikers' opties](common/users.png)

1. Aan de bovenkant van het scherm, selecteer **+ nieuwe gebruiker**.

    ![Nieuwe Gebruikersoptie](common/new-user.png)

1. In de **gebruiker** in het deelvenster de volgende stappen uit:

    ![De gebruiker-deelvenster](common/user-properties.png)

    1. Voer in het vak **Naam** **Britta Simon**in.
  
    1. In de **gebruikersnaam** Voer **BrittaSimon\@\<uwbedrijfsdomein >.\< extensie >** . Bijvoorbeeld, **BrittaSimon\@contoso.com**.

    1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.

    1. Selecteer **Maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u de gebruiker Britta Simon Azure eenmalige aanmelding gebruiken door de gebruiker toch toegang verlenen tot PageDNA.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** > **PageDNA**.

    ![Deelvenster voor Enterprise applications](common/enterprise-applications.png)

1. Selecteer in de lijst met toepassingen, **PageDNA**.

    ![PageDNA in de lijst met toepassingen](common/all-applications.png)

1. In het linkerdeelvenster onder **beheren**, selecteer **gebruikers en groepen**.

    ![De optie 'Gebruikers en groepen'](common/users-groups-blade.png)

1. Selecteer **+ gebruiker toevoegen**, en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** deelvenster.

    ![Het deelvenster toewijzing toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** venster **Britta Simon** in de **gebruikers** lijst en kies vervolgens **Selecteer** aan de onderkant van het deelvenster.

1. Als u verwacht een waarde voor de rol in het SAML-verklaring, klikt u vervolgens de **rol selecteren** deelvenster, selecteert u de juiste rol voor de gebruiker in de lijst. Kies aan de onderkant van het deelvenster **Selecteer**.

1. In de **toevoegen toewijzing** venster **toewijzen**.

### <a name="create-a-pagedna-test-user"></a>Maak een testgebruiker PageDNA

Een gebruiker met de naam Britta Simon is nu gemaakt in PageDNA. U hoeft te ondernemen om deze gebruiker te maken. PageDNA biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Als een gebruiker met de naam Britta Simon nog niet in PageDNA bestaat, wordt een nieuw gemaakt nadat verificatie.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de portal mijn Apps.

Wanneer u selecteert **PageDNA** in de portal mijn Apps u moet worden automatisch aangemeld bij de PageDNA-abonnement waarvoor u van eenmalige aanmelding instellen. Zie voor meer informatie over de portal mijn Apps [toegang en gebruik apps op de portal mijn Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies voor het integreren van SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [Eenmalige aanmelding bij toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

