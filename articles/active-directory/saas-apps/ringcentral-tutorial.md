---
title: 'Zelfstudie: Azure Active Directory-integratie met RingCentral | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RingCentral.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: 7cd20397c122834a23586bf51af6467f58531094
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997237"
---
# <a name="tutorial-azure-active-directory-integration-with-ringcentral"></a>Zelfstudie: Azure Active Directory-integratie met RingCentral

In deze zelfstudie leert u hoe u RingCentral integreren met Azure Active Directory (Azure AD).
RingCentral integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot RingCentral heeft.
* U kunt uw gebruikers worden automatisch aangemeld RingCentral (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met RingCentral, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding RingCentral ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor RingCentral **SP** gestart door SSO

## <a name="adding-ringcentral-from-the-gallery"></a>RingCentral uit de galerie toe te voegen

Voor het configureren van de integratie van RingCentral in Azure AD, moet u RingCentral uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen RingCentral uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **RingCentral**, selecteer **RingCentral** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![RingCentral in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met RingCentral op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in RingCentral tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met RingCentral, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding RingCentral](#configure-ringcentral-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak RingCentral testgebruiker](#create-ringcentral-test-user)**  : als u wilt een equivalent van Britta Simon in RingCentral die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met RingCentral, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **RingCentral** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Voer in de sectie **Standaard SAML-configuratie** de volgende stappen uit als u beschikt over een **bestand met metagegevens van de serviceprovider**:

    a. Klik op **Metagegevensbestand uploaden**.

    ![Metagegevensbestand uploaden](common/upload-metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Metagegevensbestand kiezen](common/browse-upload-metadata.png)

    c. Nadat het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld **SAML-basisconfiguratie** sectie.

    ![RingCentral domein en URL's, eenmalige aanmelding informatie](common/sp-identifier-reply.png)

    In de **aanmeldings-URL** tekstvak typt u een URL:
    
    | |
    |--|
    | `https://service.ringcentral.com`|
    | `https://service.ringcentral.com.au`|
    | `https://service.ringcentral.co.uk`|
    | `https://service.ringcentral.eu`|

    > [!Note]
    > U krijgt de **Service Provider-bestand met metagegevens** op de pagina voor RingCentral SSO-configuratie die later in de zelfstudie wordt uitgelegd.

5. Als u geen **Service Provider-bestand met metagegevens**, voer de volgende stappen uit:

    a. In de **aanmeldings-URL** tekstvak, een URL typen:

    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    b. In de **id** tekstvak, een URL typen:

    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    c. Typ een URL in het tekstvak **Antwoord-URL**:

    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

    ![RingCentral domein en URL's, eenmalige aanmelding informatie](common/sp-identifier-reply.png)

6. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-ringcentral-single-sign-on"></a>RingCentral voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij RingCentral als een beveiligingsbeheerder.

1. Klik op de bovenkant op **extra**.

    ![image](./media/ringcentral-tutorial/ringcentral1.png)

1. Navigeer naar **Single Sign-on**.

    ![image](./media/ringcentral-tutorial/ringcentral2.png)

1. Op de **Single Sign-on** pagina onder **SSO-configuratie** sectie van **stap 1** klikt u op **bewerken** en voer de volgende stappen uit:

    ![image](./media/ringcentral-tutorial/ringcentral3.png)

1. Op de **instellen Single Sign-on** pagina, voert u de volgende stappen uit:

    ![image](./media/ringcentral-tutorial/ringcentral4.png)

    a. Klik op **Bladeren** voor het uploaden van het bestand met metagegevens die u hebt gedownload vanuit Azure portal.

    b. Na het uploaden van de metagegevens van de waarden ophalen automatisch ingevuld in **SSO algemene informatie** sectie.

    c. Onder **kenmerk toewijzing** sectie, selecteer **kaart e-kenmerk voor het** als `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. Klik op **Opslaan**.

    e. Van **stap 2** klikt u op **downloaden** voor het downloaden van de **Service Provider-bestand met metagegevens** en upload dit in **SAML-basisconfiguratie** sectie voor het automatisch vullen de **id** en **antwoord-URL** waarden in Azure portal.

    ![image](./media/ringcentral-tutorial/ringcentral6.png) 

    f. Op dezelfde pagina bevinden, gaat u naar **SSO inschakelen** sectie en voer de volgende stappen uit:

    ![image](./media/ringcentral-tutorial/ringcentral5.png)

    * Selecteer **SSO-Service inschakelen**.

    * Selecteer **toestaan dat gebruikers zich kunnen aanmelden met referenties voor eenmalige aanmelding of RingCentral**.

    * Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan RingCentral.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **RingCentral**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **RingCentral**.

    ![De koppeling RingCentral in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-ringcentral-test-user"></a>RingCentral testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in RingCentral. Werken met [RingCentral Client ondersteuningsteam](https://success.ringcentral.com/RCContactSupp) om toe te voegen de gebruikers in het RingCentral-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel RingCentral in het toegangsvenster, moet u worden automatisch aangemeld bij de RingCentral waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
