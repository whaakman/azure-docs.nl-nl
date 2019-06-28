---
title: 'Zelfstudie: Azure Active Directory-integratie met Ungerboeck Software | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Ungerboeck Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bd861069-8a15-4d29-aa26-0d7be3a0a94a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 69c7a7a8fe0ef346cb26e3b0b4f14d3dae7211b9
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2019
ms.locfileid: "67370304"
---
# <a name="tutorial-integrate-ungerboeck-software-with-azure-active-directory"></a>Zelfstudie: Ungerboeck Software integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Ungerboeck om Software te integreren met Azure Active Directory (Azure AD). Wanneer u Ungerboeck Software met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Ungerboeck Software heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Ungerboeck Software met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement Ungerboeck Software eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Ungerboeck Software **SP** gestart door SSO.

## <a name="adding-ungerboeck-software-from-the-gallery"></a>Ungerboeck Software uit de galerie toe te voegen

Voor het configureren van de integratie van Ungerboeck Software in Azure AD, moet u Ungerboeck Software uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Ungerboeck Software** in het zoekvak in.
1. Selecteer **Ungerboeck Software** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Ungerboeck Software met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Ungerboeck Software vast te stellen.

Als u wilt configureren en Azure AD-eenmalige aanmelding met Ungerboeck Software testen, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Ungerboeck Software SSO configureren](#configure-ungerboeck-software-sso)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B.Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak Ungerboeck Software testgebruiker](#create-ungerboeck-software-test-user)**  hebben een equivalent van B.Simon in Ungerboeck-Software die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Ungerboeck Software** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    1. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SUBDOMAIN>.ungerboeck.com/prod`

    1. In het tekstvak **Id (Entiteits-id)** typt u een URL met het volgende patroon:
    
       **Voor productie-omgeving**:

       | |
       |-|
       | `https://<SUBDOMAIN>.ungerboeck.com/prod` |
       | `https://<SUBDOMAIN>.ungerboeck.net/prod` |
       | `https://<SUBDOMAIN>.ungerboeck.io/prod` |
       | |

        **Voor de testomgeving**:

       | |
       |-|
       | `https://<SUBDOMAIN>.ungerboeck.com/test` |
       | `https://<SUBDOMAIN>.ungerboeck.net/test` |
       | `https://<SUBDOMAIN>.ungerboeck.io/test` |
       | |

   > [!NOTE]
   > Dit zijn geen echte waarden. Deze waarden bijwerken met de werkelijke aanmelding URL en de id die wordt uitgelegd later in de **configureren Ungerboeck Software Single Sign-On** gedeelte van de zelfstudie.

1. Klik in de sectie **SAML-handtekeningcertificaat** op de knop **Bewerken** om het dialoogvenster **SAML-handtekeningcertificaat** te openen.

    ![SAML-handtekeningcertificaat bewerken](common/edit-certificate.png)

1. Kopieer in de sectie **SAML-handtekeningcertificaat** de waarde voor **VINGERAFDRUK** en sla deze op de computer op.

    ![Waarde van vingerafdruk kopiëren](common/copy-thumbprint.png)

1. Op de **Ungerboeck Software instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-ungerboeck-software-sso"></a>Software-Ungerboeck eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op **Ungerboeck Software** zijde, moet u voor het verzenden van de **vingerafdrukwaarde** en toepassing van de gekopieerde URL's van Azure portal om te [Ungerboeck Software ondersteuningsteam](mailto:Rhonda.Jannings@ungerboeck.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

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

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Ungerboeck Software.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Ungerboeck Software**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-ungerboeck-software-test-user"></a>Testgebruiker Ungerboeck Software maken

In deze sectie maakt u een gebruiker met de naam van B.Simon in Ungerboeck Software. Werken met [Ungerboeck Software ondersteuningsteam](mailto:Rhonda.Jannings@ungerboeck.com) om toe te voegen de gebruikers in het platform Ungerboeck Software. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Ungerboeck Software in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Ungerboeck Software waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)