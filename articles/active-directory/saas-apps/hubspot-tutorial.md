---
title: 'Zelfstudie: Azure Active Directory-integratie met HubSpot | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en HubSpot.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 57343ccd-53ea-4e62-9e54-dee2a9562ed5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b2c765778fc2bdd8425cc3f375831c0d317e753
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67100896"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Zelfstudie: Azure Active Directory-integratie met HubSpot

In deze zelfstudie leert u hoe u HubSpot integreren met Azure Active Directory (Azure AD).

HubSpot integreren met Azure AD biedt u de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot HubSpot heeft gebruiken.
* Gebruikers kunnen worden automatisch aangemeld bij HubSpot met hun Azure AD-accounts (eenmalige aanmelding).
* U kunt uw accounts vanaf één locatie beheren, de Azure-portal.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD, [eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met HubSpot, moet u de volgende items:

* Een Azure AD-abonnement Als u een Azure AD-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Een abonnement HubSpot met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureert en Azure AD eenmalige aanmelding testen in een testomgeving en HubSpot integreren met Azure AD.

HubSpot ondersteunt de volgende functies:

* **Serviceprovider geïnitieerde eenmalige aanmelding**
* **Door IDP geïnitieerde eenmalige aanmelding**

## <a name="add-hubspot-in-the-azure-portal"></a>HubSpot toevoegen in Azure portal

Als u wilt HubSpot integreren met Azure AD, moet u HubSpot toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer in het menu links **Azure Active Directory**.

    ![De Azure Active Directory-optie](common/select-azuread.png)

1. Selecteer **Bedrijfstoepassingen** > **Alle toepassingen**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing**.

    ![De nieuwe optie voor de App](common/add-new-app.png)

1. Voer in het zoekvak **HubSpot**. Selecteer in de lijst met zoekresultaten **HubSpot**, en selecteer vervolgens **toevoegen**.

    ![HubSpot in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met HubSpot op basis van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een gekoppelde relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in HubSpot.

Als u wilt configureren en Azure AD eenmalige aanmelding met HubSpot testen, moet u de volgende bouwstenen uitvoeren:

| Taak | Description |
| --- | --- |
| **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** | Hiermee kunnen uw gebruikers deze functie wilt gebruiken. |
| **[HubSpot eenmalige aanmelding configureren](#configure-hubspot-single-sign-on)** | Hiermee configureert u de instellingen voor eenmalige aanmelding in de toepassing. |
| **[Maak een testgebruiker Azure AD](#create-an-azure-ad-test-user)** | Met de naam Britta Simon tests Azure AD eenmalige aanmelding voor een gebruiker. |
| **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** | Hiermee kunt Britta Simon gebruik van Azure AD eenmalige aanmelding. |
| **[Maak een testgebruiker HubSpot](#create-a-hubspot-test-user)** | Hiermee maakt u een equivalent van Britta Simon in HubSpot die is gekoppeld aan de Azure AD-weergave van de gebruiker. |
| **[Eenmalige aanmelding testen](#test-single-sign-on)** | Hiermee wordt gecontroleerd of de configuratie werkt. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie configureert u Azure AD eenmalige aanmelding met HubSpot in Azure portal.

1. In de [Azure-portal](https://portal.azure.com/), in de **HubSpot** toepassing integratie venster **eenmalige aanmelding**.

    ![Optie voor eenmalige aanmelding configureren](common/select-sso.png)

1. In de **selecteert u een methode voor eenmalige aanmelding** deelvenster Selecteer **SAML** of **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

1. In de **instellen van eenmalige aanmelding met SAML** venster **bewerken** (het potloodpictogram) te openen de **SAML-basisconfiguratie** deelvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de **SAML-basisconfiguratie** deelvenster, het configureren van *IDP gestart door de modus*, voer de volgende stappen uit:

    1. In de **id** voert u een URL met de volgende indeling: https:\//api.hubspot.com/login-api/v1/saml/login?portalId=\<klant-ID\>.

    1. In de **antwoord-URL** voert u een URL met de volgende indeling: https:\//api.hubspot.com/login-api/v1/saml/acs?portalId=\<klant-ID\>.

    ![HubSpot domein en URL's, eenmalige aanmelding informatie](common/idp-intiated.png)

    > [!NOTE]
    > Als u wilt de URL-indeling, kunt u ook verwijzen naar de patronen die wordt weergegeven in de **SAML-basisconfiguratie** deelvenster in de Azure-portal.

1. Het configureren van de toepassing in *SP geïnitieerde* modus:

    1. Selecteer **extra URL's instellen**.

    1. In de **aanmeldings-URL** Voer **https:\//app.hubspot.com/login**.

    ![De Set-optie voor extra URL 's](common/metadata-upload-additional-signon.png)

1. In de **instellen van eenmalige aanmelding met SAML** deelvenster in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** naast **certificaat (Base64)** . Selecteer een optie voor downloaden op basis van uw vereisten. Sla het certificaat op uw computer.

    ![De optie voor het downloaden van certificaat (Base64)](common/certificatebase64.png)

1. In de **HubSpot instellen** sectie, kopieert u de volgende URL's op basis van uw vereisten:

    * Aanmeldings-URL
    * Azure AD-id
    * Afmeldings-URL

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-hubspot-single-sign-on"></a>HubSpot eenmalige aanmelding configureren

1. Open een nieuw tabblad in uw browser en meld u aan uw beheerdersaccount HubSpot.

1. Selecteer de **instellingen** pictogram in de rechterbovenhoek van de pagina.

    ![Het pictogram instellingen in HubSpot](./media/hubspot-tutorial/config1.png)

1. Selecteer **standaardwaarden Account**.

    ![De optie Account standaard ingesteld op HubSpot](./media/hubspot-tutorial/config2.png)

1. Schuif omlaag naar de **Security** uit en selecteer vervolgens **instellen**.

    ![Het instellen van de optie in HubSpot](./media/hubspot-tutorial/config3.png)

1. In de **instellen van eenmalige aanmelding** sectie, voert u de volgende stappen uit:

    1. In de **doelgroep URl (Service Provider entiteits-ID)** Schakel **kopie** om de waarde te kopiëren. In de Azure-portal in de **SAML-basisconfiguratie** deelvenster, plak de waarde in de **id** vak.

    1. In de **Meld u aan bij de URl, ACS, ontvanger of omleiden** Schakel **kopie** om de waarde te kopiëren. In de Azure-portal in de **SAML-basisconfiguratie** deelvenster, plak de waarde in de **antwoord-URL** vak.

    1. In HubSpot, in de **Identity Provider-id of URL-verlener** vak, plak de waarde voor **Azure AD-id** die u hebt gekopieerd in de Azure-portal.

    1. In HubSpot, in de **Identity Provider aanmeldings-URL voor eenmalige** vak, plak de waarde voor **aanmeldings-URL** die u hebt gekopieerd in de Azure-portal.

    1. In Windows Kladblok en open het Certificate(Base64)-bestand dat u hebt gedownload. Selecteer en kopieer de inhoud van het bestand. Klik, in HubSpot, plak deze in de **X.509-certificaat** vak.

    1. Selecteer **controleren**.

        ![Het instellen van de sectie voor eenmalige aanmelding in HubSpot](./media/hubspot-tutorial/config4.png)

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

In deze sectie maakt u Britta Simon toegang verlenen aan HubSpot, zodat ze Azure eenmalige aanmelding kan gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** > **HubSpot**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Selecteer in de lijst met toepassingen, **HubSpot**.

    ![HubSpot in de lijst met toepassingen](common/all-applications.png)

1. Selecteer **Gebruikers en groepen** in het menu.

    ![De optie gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen**. Selecteer vervolgens **Gebruikers en groepen** in het deelvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** venster **Britta Simon** in de lijst met gebruikers. Kies **Selecteren**.

1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** deelvenster, selecteer de relevante functieservices voor de gebruiker in de lijst. Kies **Selecteren**.

1. In de **toevoegen toewijzing** venster **toewijzen**.

### <a name="create-a-hubspot-test-user"></a>Maak een testgebruiker HubSpot

Een gebruiker zich aanmeldt bij HubSpot, de gebruiker moet worden ingericht in HubSpot, zodat Azure AD. In HubSpot is inrichten een handmatige taak.

Voor het inrichten van een gebruikersaccount in HubSpot:

1. Meld u aan uw bedrijf HubSpot site als beheerder.

1. Selecteer de **instellingen** pictogram in de rechterbovenhoek van de pagina.

    ![Het pictogram instellingen in HubSpot](./media/hubspot-tutorial/config1.png)

1. Selecteer **gebruikers en Teams**.

    ![De optie gebruikers en Teams in HubSpot](./media/hubspot-tutorial/user1.png)

1. Selecteer **gebruiker maken**.

    ![De gebruikersoptie in HubSpot](./media/hubspot-tutorial/user2.png)

1. In de **toevoegen e addess(es)** voert u het e-mailadres van de gebruiker in de indeling brittasimon\@contoso.com en selecteer vervolgens **volgende**.

    ![De e-mailadressen toevoegen in het gedeelte van de gebruikers maken in HubSpot vak](./media/hubspot-tutorial/user3.png)

1. In de **maken gebruikers** sectie, selecteert u elk tabblad. Stel de desbetreffende opties en de machtigingen voor de gebruiker op elk tabblad. Selecteer vervolgens **Volgende**.

    ![Tabbladen in het gedeelte van de gebruikers maken in HubSpot](./media/hubspot-tutorial/user4.png)

1. Selecteer voor het verzenden van de uitnodiging voor de gebruiker, **verzenden**.

    ![De optie verzenden in HubSpot](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > De gebruiker wordt geactiveerd nadat de gebruiker de uitnodiging accepteert.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de portal mijn Apps.

Na het instellen van eenmalige aanmelding, wanneer u selecteert **HubSpot** in de portal mijn Apps u worden automatisch aangemeld bij HubSpot. Zie voor meer informatie over de portal mijn Apps [toegang en gebruik apps in de portal mijn Apps](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie:

- [Lijst met zelfstudies voor het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Eenmalige aanmelding bij toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
