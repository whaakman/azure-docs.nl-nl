---
title: 'Zelfstudie: Azure Active Directory-integratie met RingCentral | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RingCentral.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/09/2019
ms.author: jeedes
ms.openlocfilehash: 2e4f45deb8c63640a328e38cebc2ecbe67233c3b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66143142"
---
# <a name="tutorial-integrate-ringcentral-with-azure-active-directory"></a>Zelfstudie: RingCentral integreren met Azure Active Directory

In deze zelfstudie leert u hoe u RingCentral integreert met Azure Active Directory (Azure AD). Wanneer u RingCentral met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot RingCentral heeft.
* Kunnen uw gebruikers worden automatisch aangemeld RingCentral met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement RingCentral eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor RingCentral **IDP** gestart door SSO

## <a name="adding-ringcentral-from-the-gallery"></a>RingCentral uit de galerie toe te voegen

Voor het configureren van de integratie van RingCentral in Azure AD, moet u RingCentral uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **RingCentral** in het zoekvak in.
1. Selecteer **RingCentral** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met RingCentral met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in RingCentral vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met RingCentral, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van SSO RingCentral](#configure-ringcentral-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak RingCentral testgebruiker](#create-ringcentral-test-user)**  : als u wilt een equivalent van Britta Simon in RingCentral die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **RingCentral** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Voer in de sectie **Standaard SAML-configuratie** de volgende stappen uit als u beschikt over een **bestand met metagegevens van de serviceprovider**:

    1. Klik op **metagegevensbestand uploaden**.
    1. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.
    1. Nadat het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld **SAML-basisconfiguratie** sectie.

    > [!Note]
    > U krijgt de **Service Provider-bestand met metagegevens** op de pagina voor RingCentral SSO-configuratie die later in de zelfstudie wordt uitgelegd.

1. Als u geen **Service Provider-bestand met metagegevens**, voer de waarden voor de volgende velden:

    a. In de **id** tekstvak, een URL typen:

    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    b. Typ een URL in het tekstvak **Antwoord-URL**:

    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

1. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op de kopieerknop om de **URL voor federatieve metagegevens van de app** te kopiëren en slaat u deze op uw computer op.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-ringcentral-sso"></a>RingCentral eenmalige aanmelding configureren

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

In deze sectie maakt u een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `Britta Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `BrittaSimon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door toegang te verlenen aan RingCentral gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **RingCentral**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-ringcentral-test-user"></a>RingCentral testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in RingCentral. Werken met [RingCentral Client ondersteuningsteam](https://success.ringcentral.com/RCContactSupp) om toe te voegen de gebruikers in het RingCentral-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel RingCentral in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de RingCentral waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
