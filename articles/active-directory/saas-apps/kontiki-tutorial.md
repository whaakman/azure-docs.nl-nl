---
title: 'Zelfstudie: Azure Active Directory-integratie met Kontiki | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kontiki.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8d5e5413-da4c-40d8-b1d0-f03ecfef030b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfb154bce34b2ceda99b82c7ca3534b8a8ee0a1f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098492"
---
# <a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Zelfstudie: Azure Active Directory-integratie met Kontiki

In deze zelfstudie leert u hoe u Kontiki integreren met Azure Active Directory (Azure AD).

Kontiki integreren met Azure AD biedt u de volgende voordelen:

* U kunt Azure AD om te bepalen wie toegang tot Kontiki heeft gebruiken.
* Gebruikers kunnen worden automatisch aangemeld bij Kontiki met hun Azure AD-accounts (eenmalige aanmelding).
* U kunt uw accounts vanaf één locatie beheren, de Azure-portal.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD, [eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Kontiki, moet u de volgende items:

* Een Azure AD-abonnement Als u een Azure AD-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Een abonnement Kontiki met eenmalige aanmelding ingeschakeld.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureert en Azure AD eenmalige aanmelding testen in een testomgeving en Kontiki integreren met Azure AD.

Kontiki ondersteunt de volgende functies:

* **Serviceprovider geïnitieerde eenmalige aanmelding**
* **Just In Time inrichten van gebruikers**

## <a name="add-kontiki-in-the-azure-portal"></a>Kontiki toevoegen in Azure portal

Als u wilt Kontiki integreren met Azure AD, moet u Kontiki toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer in het menu links **Azure Active Directory**.

    ![De Azure Active Directory-optie](common/select-azuread.png)

1. Selecteer **Bedrijfstoepassingen** > **Alle toepassingen**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing**.

    ![De nieuwe optie voor de App](common/add-new-app.png)

1. Voer in het zoekvak **Kontiki**. Selecteer in de lijst met zoekresultaten **Kontiki**, en selecteer vervolgens **toevoegen**.

    ![Kontiki in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Kontiki op basis van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een gekoppelde relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in Kontiki.

Als u wilt configureren en Azure AD eenmalige aanmelding met Kontiki testen, moet u de volgende bouwstenen uitvoeren:

| Taak | Description |
| --- | --- |
| **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** | Hiermee kunnen uw gebruikers deze functie wilt gebruiken. |
| **[Kontiki eenmalige aanmelding configureren](#configure-kontiki-single-sign-on)** | Hiermee configureert u de instellingen voor eenmalige aanmelding in de toepassing. |
| **[Maak een testgebruiker Azure AD](#create-an-azure-ad-test-user)** | Met de naam Britta Simon tests Azure AD eenmalige aanmelding voor een gebruiker. |
| **[De Azure AD-testgebruiker toewijzen](#assign-the-azure-ad-test-user)** | Hiermee kunt Britta Simon gebruik van Azure AD eenmalige aanmelding. |
| **[Maak een testgebruiker Kontiki](#create-a-kontiki-test-user)** | Hiermee maakt u een equivalent van Britta Simon in Kontiki die is gekoppeld aan de Azure AD-weergave van de gebruiker. |
| **[Eenmalige aanmelding testen](#test-single-sign-on)** | Hiermee wordt gecontroleerd of de configuratie werkt. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie configureert u Azure AD eenmalige aanmelding met Kontiki in Azure portal.

1. In de [Azure-portal](https://portal.azure.com/), in de **Kontiki** toepassing integratie venster **eenmalige aanmelding**.

    ![Optie voor eenmalige aanmelding configureren](common/select-sso.png)

1. In de **selecteert u een methode voor eenmalige aanmelding** deelvenster Selecteer **SAML** of **SAML/WS-Federation** modus voor eenmalige aanmelding inschakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

1. In de **instellen van eenmalige aanmelding met SAML** venster **bewerken** (het potloodpictogram) te openen de **SAML-basisconfiguratie** deelvenster.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de **SAML-basisconfiguratie** deelvenster in de **aanmeldings-URL** tekst voert u een URL met de volgende indeling: `https://<companyname>.mc.eval.kontiki.com`

    ![Kontiki domein en URL's, eenmalige aanmelding informatie](common/sp-signonurl.png)

    > [!NOTE]
    > Neem contact op met de [Kontiki Client ondersteuningsteam](https://customersupport.kontiki.com/enterprise/contactsupport.html) om de juiste waarde te gebruiken. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. In de **instellen van eenmalige aanmelding met SAML** deelvenster in de **SAML-handtekeningcertificaat** sectie, selecteer **downloaden** naast **voorfederatievemetagegevens-XML**. Selecteer een optie voor downloaden op basis van uw vereisten. Sla het certificaat op uw computer.

    ![De optie voor federatieve metagegevens-XML-certificaat downloaden](common/metadataxml.png)

1. In de **Kontiki instellen** sectie, kopieert u de volgende URL's op basis van uw vereisten:

    * Aanmeldings-URL
    * Azure AD-id
    * Afmeldings-URL

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-kontiki-single-sign-on"></a>Kontiki eenmalige aanmelding configureren

Voor het configureren van eenmalige aanmelding aan de Kontiki, het verzenden van het gedownloade federatieve metagegevens-XML-bestand en de relevante URL's die u hebt gekopieerd uit de Azure-portal naar de [Kontiki ondersteuningsteam](https://customersupport.kontiki.com/enterprise/contactsupport.html). Het ondersteuningsteam Kontiki gebruikt de informatie die u verzend om ervoor te zorgen dat de SAML eenmalige aanmelding verbinding aan beide zijden correct is ingesteld.

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

In deze sectie maakt u Britta Simon toegang verlenen aan Kontiki, zodat ze Azure eenmalige aanmelding kan gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen** > **alle toepassingen** > **Kontiki**.

    ![Het deelvenster Bedrijfstoepassingen](common/enterprise-applications.png)

1. Selecteer in de lijst met toepassingen, **Kontiki**.

    ![Kontiki in de lijst met toepassingen](common/all-applications.png)

1. Selecteer **Gebruikers en groepen** in het menu.

    ![De optie gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **Gebruiker toevoegen**. Selecteer vervolgens **Gebruikers en groepen** in het deelvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** venster **Britta Simon** in de lijst met gebruikers. Kies **Selecteren**.

1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** deelvenster, selecteer de relevante functieservices voor de gebruiker in de lijst. Kies **Selecteren**.

1. In de **toevoegen toewijzing** venster **toewijzen**.

### <a name="create-a-kontiki-test-user"></a>Maak een testgebruiker Kontiki

Er is geen actie-item voor u configureren in Kontiki inrichten van gebruikers. Wanneer een toegewezen gebruiker wil zich aanmelden bij Kontiki met behulp van de portal mijn Apps, Kontiki wordt gecontroleerd of de gebruiker bestaat. Als er geen gebruikersaccount wordt gevonden, wordt het gebruikersaccount automatisch gemaakt door Kontiki.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van de portal mijn Apps.

Na het instellen van eenmalige aanmelding, wanneer u selecteert **Kontiki** in de portal mijn Apps u worden automatisch aangemeld bij Kontiki. Zie voor meer informatie over de portal mijn Apps [toegang en gebruik apps in de portal mijn Apps](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie:

- [Lijst met zelfstudies voor het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Eenmalige aanmelding bij toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
