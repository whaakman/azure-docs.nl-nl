---
title: 'Zelfstudie: Azure Active Directory-integratie met Vtiger CRM (SAML) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Vtiger CRM (SAML).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f14e34a6-f51f-4cd1-a6ad-f04df551303d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45782d7827a289cc0942eca61d6b30d071a6721c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450742"
---
# <a name="tutorial-integrate-vtiger-crm-saml-with-azure-active-directory"></a>Zelfstudie: Vtiger CRM (SAML) integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Vtiger CRM (SAML) integreren met Azure Active Directory (Azure AD). Wanneer u Vtiger CRM (SAML) met Azure AD integreren, kunt u het volgende doen:

* Beheren in Azure AD toegang Vtiger CRM (SAML) heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Vtiger CRM (SAML) met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement Vtiger CRM (SAML) eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. 

* Biedt ondersteuning voor Vtiger CRM (SAML) **SP** gestart door SSO
* Biedt ondersteuning voor Vtiger CRM (SAML) **Just In Time** inrichten van gebruikers

## <a name="adding-vtiger-crm-saml-from-the-gallery"></a>Vtiger CRM (SAML) uit de galerie toe te voegen

Voor het configureren van de integratie van Vtiger CRM (SAML) in Azure AD, moet u Vtiger CRM (SAML) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Vtiger CRM (SAML)** in het zoekvak in.
1. Selecteer **Vtiger CRM (SAML)** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Vtiger CRM (SAML) met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een relatie tot stand brengt een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Vtiger CRM (SAML).

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Vtiger CRM (SAML), voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van Vtiger CRM (SAML) SSO](#configure-vtiger-crm-saml-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak de testgebruiker Vtiger CRM (SAML)](#create-vtiger-crm-saml-test-user)**  - hebben een equivalent van Britta Simon in Vtiger CRM (SAML) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Vtiger CRM (SAML)** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:

    | | |
    | - |- |
    | `https://<customer_instance>.od1.vtiger.com` |
    | `https://<customer_instance>.od2.vtiger.com` |
    | `https://<customer_instance>.od1.vtiger.ws` |

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<customer_instance>.od1.vtiger.com/sso/saml?acs`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [Vtiger CRM (SAML) Client-ondersteuningsteam](mailto:support@vtiger.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **certificaat (Base64)** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de **van Vtiger CRM (SAML) ingesteld** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-vtiger-crm-saml-sso"></a>Configureren van eenmalige aanmelding Vtiger CRM (SAML)

Het configureren van eenmalige aanmelding op **Vtiger CRM (SAML)** zijde, moet u voor het verzenden van de gedownloade **certificaat (Base64)** en toepassing van de gekopieerde URL's van Azure portal om te [Vtiger CRM (SAML)-ondersteuning team](mailto:support@vtiger.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B.Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B.Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Vtiger CRM (SAML).

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Vtiger CRM (SAML)** .
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-vtiger-crm-saml-test-user"></a>Testgebruiker Vtiger CRM (SAML) maken

In deze sectie wordt een gebruiker met de naam Britta Simon in Vtiger CRM (SAML) gemaakt. Vtiger CRM (SAML) biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker in Vtiger CRM (SAML) nog niet bestaat, wordt een nieuw gemaakt nadat verificatie.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Vtiger CRM (SAML) in het toegangsvenster selecteert, moet u worden automatisch aangemeld op de Vtiger CRM (SAML) waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)