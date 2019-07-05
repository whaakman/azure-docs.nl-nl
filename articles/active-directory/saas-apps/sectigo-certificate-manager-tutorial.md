---
title: 'Zelfstudie: Azure Active Directory-integratie met Sectigo Certificate Manager | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Sectigo Certificate Manager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 62cd6987-3373-4b58-b1ff-589f4a3d70a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0447a8dd464363ae7e076dde2520565005d7c0a5
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67588239"
---
# <a name="tutorial-azure-active-directory-integration-with-sectigo-certificate-manager"></a>Zelfstudie: Azure Active Directory-integratie met Sectigo Certificate Manager

In deze zelfstudie leert u hoe u Sectigo Certificate Manager integreert met Azure Active Directory (Azure AD).

Sectigo Certificate Manager integreren met Azure AD biedt u de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot Sectigo Certificate Manager heeft kunt gebruiken.
* Gebruikers kunnen worden automatisch aangemeld op Sectigo Certificate Manager met hun Azure AD-accounts (eenmalige aanmelding).
* U kunt uw accounts vanaf één locatie beheren, de Azure-portal.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD, [eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Sectigo Certificate Manager, moet u de volgende items:

* Een Azure AD-abonnement Als u een Azure AD-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Sectigo Certificate Manager-abonnement met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureert en Azure AD eenmalige aanmelding testen in een testomgeving en Sectigo Certificate Manager integreren met Azure AD.

Sectigo Certificate Manager ondersteunt de volgende functies:

* **Serviceprovider geïnitieerde eenmalige aanmelding**
* **Door IDP geïnitieerde eenmalige aanmelding**

## <a name="add-sectigo-certificate-manager-in-the-azure-portal"></a>Sectigo Certificate Manager toevoegen in Azure portal

Als u wilt Sectigo Certificate Manager integreren met Azure AD, moet u Sectigo Certificate Manager toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer in het menu links **Azure Active Directory**.

    ![De Azure Active Directory-optie](common/select-azuread.png)

1. Selecteer **Bedrijfstoepassingen** > **Alle toepassingen**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing**.

    ![De nieuwe optie voor de App](common/add-new-app.png)

1. Voer in het zoekvak **Sectigo Certificate Manager**. Selecteer in de lijst met zoekresultaten **Sectigo Certificate Manager**, en selecteer vervolgens **toevoegen**.

    ![Sectigo Certificate Manager in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Sectigo Certificate Manager op basis van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een gekoppelde relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in Sectigo Certificate Manager.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Sectigo Certificate Manager, moet u de volgende bouwstenen uitvoeren:

| Taak | Description |
| --- | --- |
| **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** | Hiermee kunnen uw gebruikers deze functie wilt gebruiken. |
| **[Configureren van eenmalige aanmelding Sectigo Certificate Manager](#configure-sectigo-certificate-manager-single-sign-on)** | Hiermee configureert u de instellingen voor eenmalige aanmelding in de toepassing. |
| **[Maak een testgebruiker Azure AD](#create-an-azure-ad-test-user)** | Met de naam Britta Simon tests Azure AD eenmalige aanmelding voor een gebruiker. |
| **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** | Hiermee kunt Britta Simon gebruik van Azure AD eenmalige aanmelding. |
| **[Maak een testgebruiker Sectigo Certificate Manager](#create-a-sectigo-certificate-manager-test-user)** | Hiermee maakt u een equivalent van Britta Simon in Sectigo Certificate Manager die is gekoppeld aan de Azure AD-weergave van de gebruiker. |
| **[Eenmalige aanmelding testen](#test-single-sign-on)** | Hiermee wordt gecontroleerd of de configuratie werkt. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie configureert u Azure AD eenmalige aanmelding met Sectigo Certificate Manager in Azure portal.

1. In de [Azure-portal](https://portal.azure.com/), in de **Sectigo Certificate Manager** toepassing integratie venster **eenmalige aanmelding**.

    ![Optie voor eenmalige aanmelding configureren](common/select-sso.png)

1. In de **selecteert u een methode voor eenmalige aanmelding** deelvenster Selecteer **SAML** of **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

1. In de **instellen van eenmalige aanmelding met SAML** venster **bewerken** (het potloodpictogram) te openen de **SAML-basisconfiguratie** deelvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de **SAML-basisconfiguratie** deelvenster, het configureren van *IDP gestart door de modus*, voer de volgende stappen uit:

    1. In de **id** voert u een van deze URL's:
       * https:\//cert-manager.com/shibboleth
       * https:\//hard.cert-manager.com/shibboleth

    1. In de **antwoord-URL** voert u een van deze URL's:
        * https:\//cert-manager.com/Shibboleth.sso/SAML2/POST
        * https:\//hard.cert-manager.com/Shibboleth.sso/SAML2/POST

    1. Selecteer **extra URL's instellen**.

    1. In de **Relaystatus** voert u een van deze URL's:
       * https:\//cert-manager.com/customer/SSLSupport/idp
       * https:\//hard.cert-manager.com/customer/SSLSupport/idp

    ![Sectigo Certificate Manager-domein en URL's, eenmalige aanmelding informatie](common/idp-relay.png)

1.  Het configureren van de toepassing in *SP geïnitieerde modus*, voer de volgende stappen uit:

    * In de **aanmeldings-URL** voert u een van deze URL's:
      * https:\//cert-manager.com/Shibboleth.sso/Login
      * https:\//hard.cert-manager.com/Shibboleth.sso/Login

      ![Sectigo Certificate Manager-domein en URL's, eenmalige aanmelding informatie](common/both-signonurl.png)

1. In de **instellen van eenmalige aanmelding met SAML** deelvenster in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** naast **certificaat (Base64)** . Selecteer een optie voor downloaden op basis van uw vereisten. Sla het certificaat op uw computer.

    ![De optie voor het downloaden van certificaat (Base64)](common/certificatebase64.png)

1. In de **Stel Sectigo Certificate Manager** sectie, kopieert u de volgende URL's op basis van uw vereisten:

    * Aanmeldings-URL
    * Azure AD-id
    * Afmeldings-URL

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-sectigo-certificate-manager-single-sign-on"></a>Configureren van eenmalige aanmelding Sectigo Certificate Manager

Voor het configureren van eenmalige aanmelding aan de Sectigo Certificate Manager, het verzenden van het gedownloade bestand voor certificaat (Base64) en de relevante URL's die u hebt gekopieerd uit de Azure-portal naar de [Sectigo Certificate Manager ondersteuningsteam](https://sectigo.com/support). Het ondersteuningsteam Sectigo Certificate Manager gebruikt de informatie die u verzend om ervoor te zorgen dat de SAML eenmalige aanmelding verbinding aan beide zijden correct is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

In deze sectie gaat u een testgebruiker met de naam Britta Simon maken in de Azure-portal.

1. Selecteer in de Azure-portal **Azure Active Directory** > **Gebruikers** > **Alle gebruikers**.

    ![De gebruikers en alle opties voor gebruikers](common/users.png)

1. Selecteer **Nieuwe gebruiker**.

    ![De nieuwe Gebruikersoptie](common/new-user.png)

1. Voer in het deelvenster **Gebruiker** de volgende stappen uit:

    1. Voer in het vak **Naam** **Britta Simon**in.
  
    1. In de **gebruikersnaam** Voer **brittasimon\@\<uw-bedrijfsdomein >.\< extensie\>** . Bijvoorbeeld, **brittasimon\@contoso.com**.

    1. Selecteer de **Show wachtwoord** selectievakje. Noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    1. Selecteer **Maken**.

    ![De gebruiker-deelvenster](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt u Britta Simon toegang verlenen tot Sectigo Certificate Manager zodat ze Azure eenmalige aanmelding kunt gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** > **Sectigo Certificate Manager**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Selecteer in de lijst met toepassingen, **Sectigo Certificate Manager**.

    ![Sectigo Certificate Manager in de lijst met toepassingen](common/all-applications.png)

1. Selecteer **Gebruikers en groepen** in het menu.

    ![De optie gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen**. Selecteer vervolgens **Gebruikers en groepen** in het deelvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** venster **Britta Simon** in de lijst met gebruikers. Kies **Selecteren**.

1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** deelvenster, selecteer de relevante functieservices voor de gebruiker in de lijst. Kies **Selecteren**.

1. In de **toevoegen toewijzing** venster **toewijzen**.

### <a name="create-a-sectigo-certificate-manager-test-user"></a>Maak een testgebruiker Sectigo Certificate Manager

In deze sectie maakt u een gebruiker met de naam Britta Simon in Sectigo Certificate Manager. Werken met de [Sectigo Certificate Manager ondersteuningsteam](https://sectigo.com/support) om toe te voegen van de gebruiker in het platform Sectigo Certificate Manager. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de portal mijn Apps.

Na het instellen van eenmalige aanmelding, wanneer u selecteert **Sectigo Certificate Manager** in de portal mijn Apps wordt u automatisch afgemeld Sectigo Certificate Manager. Zie voor meer informatie over de portal mijn Apps [toegang en gebruik apps in de portal mijn Apps](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie:

- [Lijst met zelfstudies voor het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Eenmalige aanmelding bij toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


