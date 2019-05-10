---
title: 'Zelfstudie: Azure Active Directory-integratie met webMethods integratiesuite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en webMethods integratiesuite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 97261535-7a2d-4d73-94c8-38116b8a776e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.openlocfilehash: 8dead16c60a26e2fc53953ed65337195c3b2aa67
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65470792"
---
# <a name="tutorial-azure-active-directory-integration-with-webmethods-integration-suite"></a>Zelfstudie: Azure Active Directory-integratie met webMethods integratiesuite

In deze zelfstudie leert u hoe u webMethods integratiesuite integreren met Azure Active Directory (Azure AD).
WebMethods integratiesuite integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot webMethods integratiesuite heeft.
* U kunt uw gebruikers worden automatisch aangemeld webMethods integratiesuite (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts in één centrale locatie - Azure portal beheren.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met webMethods integratiesuite, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* webMethods integratiesuite eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* webMethods integratiesuite ondersteunt **SP** en **IDP** gestart door SSO

* webMethods integratiesuite ondersteunt **just-in-time** inrichten van gebruikers

## <a name="adding-webmethods-integration-suite-from-the-gallery"></a>WebMethods integratiesuite uit de galerie toe te voegen

Voor het configureren van de integratie van webMethods integratiesuite in Azure AD, moet u webMethods integratiesuite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen webMethods integratiesuite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **webMethods integratiesuite**, selecteer **webMethods integratiesuite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![webMethods integratiesuite in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met webMethods integratiesuite op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in webMethods integratiesuite tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met webMethods integratiesuite, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van webMethods integratie Suite Single Sign-On](#configure-webmethods-integration-suite-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[WebMethods integratiesuite testgebruiker maken](#create-webmethods-integration-suite-test-user)**  : als u wilt een equivalent van Britta Simon in webMethods integratiesuite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met webMethods integratiesuite, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **webMethods integratiesuite** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Het configureren van de **webMethods integratie van Cloud**op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart uitvoeren de volgende stappen uit:

    ![informatie webMethods integratie Suite domein en URL's eenmalige aanmelding](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon:

    | |
    |--|
    | `<SUBDOMAIN>.webmethodscloud.com` |
    | `<SUBDOMAIN>.webmethodscloud.eu` |
    | `<SUBDOMAIN>.webmethodscloud.de` |

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:

    | |
    |--|
    | `https://<SUBDOMAIN>.webmethodscloud.com/integration/live/saml/ssoResponse` |
    | `https://<SUBDOMAIN>.webmethodscloud.eu/integration/live/saml/ssoResponse` |
    | `https://<SUBDOMAIN>.webmethodscloud.de/integration/live/saml/ssoResponse` |

    c. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![informatie webMethods integratie Suite domein en URL's eenmalige aanmelding](common/metadata-upload-additional-signon.png)

    d. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon:

    | |
    |--|
    | `https://<SUBDOMAIN>.webmethodscloud.com/integration/live/saml/ssoRequest` |
    | `https://<SUBDOMAIN>.webmethodscloud.eu/integration/live/saml/ssoRequest` |
    | `https://<SUBDOMAIN>.webmethodscloud.de/integration/live/saml/ssoRequest` |

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [webMethods Integration Suite Client ondersteuningsteam](https://empower.softwareag.com/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Het configureren van de **webMethods API Cloud**op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart uitvoeren de volgende stappen uit:

    ![informatie webMethods integratie Suite domein en URL's eenmalige aanmelding](common/idp-intiated.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon:

    | |
    |--|
    | `<SUBDOMAIN>.webmethodscloud.com` |
    | `<SUBDOMAIN>.webmethodscloud.eu` |
    | `<SUBDOMAIN>.webmethodscloud.de` |

    b. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:

    | |
    |--|
    | `https://<SUBDOMAIN>.webmethodscloud.com/umc/rest/saml/initsso` |
    | `https://<SUBDOMAIN>.webmethodscloud.eu/umc/rest/saml/initsso` |
    | `https://<SUBDOMAIN>.webmethodscloud.de/umc/rest/saml/initsso` |

    c. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![informatie webMethods integratie Suite domein en URL's eenmalige aanmelding](common/metadata-upload-additional-signon.png)

    d. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon:

    | |
    |--|
    | `https://api.webmethodscloud.com/umc/rest/saml/initsso/?tenant=<TENANTID>` |
    | `https://api.webmethodscloud.eu/umc/rest/saml/initsso/?tenant=<TENANTID>` |
    | `https://api.webmethodscloud.de/umc/rest/saml/initsso/?tenant=<TENANTID>` |

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [webMethods Integration Suite Client ondersteuningsteam](https://empower.softwareag.com/) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De downloadkoppeling certificaat](common/metadataxml.png)

7. Op de **webMethods integratiesuite instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

### <a name="configure-webmethods-integration-suite-single-sign-on"></a>WebMethods integratie Suite eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op **webMethods integratiesuite** zijde, moet u voor het verzenden van de gedownloade **federatieve metagegevens-XML** en toepassing van de gekopieerde URL's van Azure portal om te [webMethods Integratie-Suite-ondersteuningsteam](https://empower.softwareag.com/). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon** in.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot webMethods integratiesuite.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **webMethods integratiesuite**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **webMethods integratiesuite**.

    ![De webMethods integratiesuite koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst Gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-webmethods-integration-suite-test-user"></a>WebMethods integratiesuite testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in webMethods integratiesuite. webMethods integratiesuite biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in webMethods integratiesuite bestaat, wordt een nieuw gemaakt nadat verificatie.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de webMethods integratiesuite tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de webMethods integratiesuite waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

