---
title: 'Zelfstudie: Azure Active Directory-integratie met monday.com | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en monday.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9e8ad807-0664-4e31-91de-731097c768e2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08108a1718ee917a317b4864de81959ae139eaa5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67097015"
---
# <a name="tutorial-azure-active-directory-integration-with-mondaycom"></a>Zelfstudie: Azure Active Directory-integratie met monday.com

In deze zelfstudie leert u hoe u monday.com integreren met Azure Active Directory (Azure AD).

Monday.com integreren met Azure AD biedt u de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot monday.com heeft gebruiken.
* Gebruikers kunnen worden automatisch aangemeld bij monday.com met hun Azure AD-accounts (eenmalige aanmelding).
* U kunt uw accounts vanaf één locatie beheren, de Azure-portal.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD, [eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met monday.com, moet u de volgende items:

* Een Azure AD-abonnement Als u een Azure AD-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Een abonnement monday.com met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureert en Azure AD eenmalige aanmelding testen in een testomgeving en monday.com integreren met Azure AD.

Monday.com ondersteunt de volgende functies:

* **Serviceprovider geïnitieerde eenmalige aanmelding**
* **Door IDP geïnitieerde eenmalige aanmelding**
* **Just In Time inrichten van gebruikers**

## <a name="add-mondaycom-in-the-azure-portal"></a>Monday.com toevoegen in Azure portal

Als u wilt monday.com integreren met Azure AD, moet u monday.com toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer in het menu links **Azure Active Directory**.

    ![De Azure Active Directory-optie](common/select-azuread.png)

1. Selecteer **Bedrijfstoepassingen** > **Alle toepassingen**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing**.

    ![De nieuwe optie voor de App](common/add-new-app.png)

1. Voer in het zoekvak **monday.com**. Selecteer in de lijst met zoekresultaten **monday.com**, en selecteer vervolgens **toevoegen**.

    ![Monday.com in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met monday.com op basis van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een gekoppelde relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in monday.com.

Als u wilt configureren en Azure AD eenmalige aanmelding met monday.com testen, moet u de volgende bouwstenen uitvoeren:

| Taak | Description |
| --- | --- |
| **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** | Hiermee kunnen uw gebruikers deze functie wilt gebruiken. |
| **[Monday.com eenmalige aanmelding configureren](#configure-mondaycom-single-sign-on)** | Hiermee configureert u de instellingen voor eenmalige aanmelding in de toepassing. |
| **[Maak een testgebruiker Azure AD](#create-an-azure-ad-test-user)** | Met de naam Britta Simon tests Azure AD eenmalige aanmelding voor een gebruiker. |
| **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** | Hiermee kunt Britta Simon gebruik van Azure AD eenmalige aanmelding. |
| **[Maak een testgebruiker monday.com](#create-a-mondaycom-test-user)** | Hiermee maakt u een equivalent van Britta Simon in monday.com die is gekoppeld aan de Azure AD-weergave van de gebruiker. |
| **[Eenmalige aanmelding testen](#test-single-sign-on)** | Hiermee wordt gecontroleerd of de configuratie werkt. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie configureert u Azure AD eenmalige aanmelding met monday.com in Azure portal.

1. In de [Azure-portal](https://portal.azure.com/), in de **monday.com** toepassing integratie venster **eenmalige aanmelding**.

    ![Optie voor eenmalige aanmelding configureren](common/select-sso.png)

1. In de **selecteert u een methode voor eenmalige aanmelding** deelvenster Selecteer **SAML** of **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

1. In de **instellen van eenmalige aanmelding met SAML** venster **bewerken** (het potloodpictogram) te openen de **SAML-basisconfiguratie** deelvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de **SAML-basisconfiguratie** deelvenster, als u een metagegevensbestand voor serviceprovider hebt en u wilt configureren *IDP gestart door de modus*, voer de volgende stappen uit:

    1. Selecteer **metagegevensbestand uploaden**.

       ![De optie-Metagegevens uploaden](common/upload-metadata.png)

    1. Selecteer het bestand met metagegevens, selecteer het mappictogram en selecteer vervolgens **uploaden**.

       ![Selecteer het bestand met metagegevens en selecteer vervolgens de knop voor uploaden](common/browse-upload-metadata.png)

    1. Nadat het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden worden automatisch ingevuld de **SAML-basisconfiguratie** deelvenster:

       ![De IDP-waarden in het SAML-basisconfiguratie](common/idp-intiated.png)

       > [!Note]
       > Als de **id** en **antwoord-URL** waarden worden niet automatisch ingevuld, voert u de waarden handmatig.

1. Het configureren van de toepassing in *SP geïnitieerde modus*:

    1. Selecteer **extra URL's instellen**.
    
    1. In de **aanmeldings-URL** voert u een URL met de volgende indeling: https:\//\<uw domein >. monday.com. Neem contact op met de [monday.com client ondersteuningsteam](mailto:support@monday.com) om op te halen van de aanmeldings-URL.

        ![De Set-optie voor extra URL 's](common/metadata-upload-additional-signon.png)

1. De toepassing monday.com wordt verwacht dat de SAML-asserties ondertekend zijn in een specifieke indeling. Configureer de volgende claims voor deze toepassing. Voor het beheren van deze kenmerkwaarden de **instellen van eenmalige aanmelding met SAML** venster **bewerken** openen de **gebruikerskenmerken** deelvenster.

    ![De gebruiker-deelvenster kenmerken](common/edit-attribute.png)

1. Onder **gebruikersclaims**, selecteer **bewerken** bewerken van de claims. Als u wilt een claim toevoegen, selecteert u **toevoegen nieuwe claim**, en configureer vervolgens in het SAML-token kenmerk, zoals wordt weergegeven in de voorgaande afbeelding. Voer de volgende stappen uit: 

    1. Selecteer **toevoegen nieuwe claim**.

        ![Het toevoegen van nieuwe claim optie in het deelvenster met claims van gebruiker](common/new-save-attribute.png)

    1. In de **gebruikersclaims beheren** deelvenster, stel de volgende waarden:
        
       1. In de **naam** voert u de naam van het kenmerk wordt weergegeven voor de rij van de claim gebruiker.

       1. Laat **Namespace** leeg.

       1. Voor **bron**, selecteer **kenmerk**.

       1. In de **bronkenmerk** , selecteert u de waarde van het kenmerk wordt weergegeven voor de rij van de claim gebruiker.

       1. Selecteer **OK**, en selecteer vervolgens **opslaan**.

       ![De claims van de gebruiker beheren](common/new-attribute-details.png)

1. In de **instellen van eenmalige aanmelding met SAML** deelvenster onder **SAML-handtekeningcertificaat**, selecteer **downloaden** naast **certificaat (Base64)** . Selecteer een optie voor downloaden op basis van uw vereisten. Sla het certificaat op uw computer.

    ![De optie voor het downloaden van certificaat (Base64)](common/certificatebase64.png)

1. In de **monday.com instellen** sectie, kopieert u de volgende URL's op basis van uw vereisten:

    * Aanmeldings-URL
    * Azure AD-id
    * Afmeldings-URL

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-mondaycom-single-sign-on"></a>Monday.com eenmalige aanmelding configureren

Voor het configureren van eenmalige aanmelding aan de monday.com, het verzenden van het gedownloade bestand voor certificaat (Base64) en de relevante URL's die u hebt gekopieerd uit de Azure-portal naar de [monday.com ondersteuningsteam](mailto:support@monday.com). Het ondersteuningsteam monday.com gebruikt de informatie die u verzend om ervoor te zorgen dat de SAML eenmalige aanmelding verbinding aan beide zijden correct is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie gaat u een testgebruiker met de naam Britta Simon maken in de Azure-portal.

1. Selecteer in de Azure-portal **Azure Active Directory** > **Gebruikers** > **Alle gebruikers**.

    ![De gebruikers en alle opties voor gebruikers](common/users.png)

1. Selecteer **Nieuwe gebruiker**.

    ![De nieuwe Gebruikersoptie](common/new-user.png)

1. Voer in het deelvenster **Gebruiker** de volgende stappen uit:

    1. Voer in het vak **Naam** **Britta Simon**in.
  
    1. In de **gebruikersnaam** Voer **brittasimon\@\<uw-bedrijfsdomein >.\< extensie >** . Bijvoorbeeld, **brittasimon\@contoso.com**.

    1. Selecteer de **Show wachtwoord** selectievakje. Noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    1. Selecteer **Maken**.

    ![De gebruiker-deelvenster](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt verlenen u Britta Simon toegang tot monday.com, zodat ze Azure eenmalige aanmelding kan gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** > **monday.com**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Selecteer in de lijst met toepassingen, **monday.com**.

    ![Monday.com in de lijst met toepassingen](common/all-applications.png)

1. Selecteer **Gebruikers en groepen** in het menu.

    ![De optie gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen**. Selecteer vervolgens **Gebruikers en groepen** in het deelvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** venster **Britta Simon** in de lijst met gebruikers. Kies **Selecteren**.

1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** deelvenster, selecteer de relevante functieservices voor de gebruiker in de lijst. Kies **Selecteren**.

1. In de **toevoegen toewijzing** venster **toewijzen**.

### <a name="create-a-mondaycom-test-user"></a>Maak een testgebruiker monday.com

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in de toepassing monday.com. Monday.com biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in monday.com bestaat, wordt een nieuw gemaakt nadat verificatie.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de portal mijn Apps.

Na het instellen van eenmalige aanmelding, wanneer u selecteert **monday.com** in de portal mijn Apps u worden automatisch aangemeld bij monday.com. Zie voor meer informatie over de portal mijn Apps [toegang en gebruik apps in de portal mijn Apps](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie:

- [Lijst met zelfstudies voor het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Eenmalige aanmelding bij toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
