---
title: 'Zelfstudie: Azure Active Directory-integratie met MyWorkDrive | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en MyWorkDrive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.openlocfilehash: 61173d21c52d061f0d02ab02eb2f1083507983c2
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991648"
---
# <a name="tutorial-integrate-myworkdrive-with-azure-active-directory"></a>Zelfstudie: MyWorkDrive integreren met Azure Active Directory

In deze zelfstudie leert u over het integreren van MyWorkDrive met Azure Active Directory (Azure AD). Wanneer u MyWorkDrive met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot MyWorkDrive heeft.
* Kunnen uw gebruikers worden automatisch aangemeld MyWorkDrive met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement MyWorkDrive eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. MyWorkDrive biedt ondersteuning voor **SP** en **IDP** gestart door SSO

## <a name="adding-myworkdrive-from-the-gallery"></a>MyWorkDrive uit de galerie toe te voegen

Voor het configureren van de integratie van MyWorkDrive met Azure AD, moet u MyWorkDrive uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **MyWorkDrive** in het zoekvak in.
1. Selecteer **MyWorkDrive** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met MyWorkDrive met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in MyWorkDrive vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met MyWorkDrive, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van MyWorkDrive SSO](#configure-myworkdrive-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[MyWorkDrive testgebruiker maken](#create-myworkdrive-test-user)**  : als u wilt een equivalent van Britta Simon in MyWorkDrive die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **MyWorkDrive** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina als u wilt configureren van de toepassing in **IDP** gestart modus, voer de waarden in voor het volgende veld:

    In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<SERVER.DOMAIN.COM>/Account/Login-saml`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de echte antwoord-URL en aanmeldings-URL. Voer uw eigen bedrijf MyWorkDrive Server host name:e.g.
    > 
    > Antwoord-URL: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`
    > 
    > Aanmeldings-URL:`https://yourserver.yourdomain.com/Account/Login-saml`
    > 
    > Neem contact op met [MyWorkDrive ondersteuningsteam](mailto:support@myworkdrive.com) als u twijfelt over het instellen van uw eigen hostnaam en SSL-certificaat voor deze waarden.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** naar het Klembord.

    ![De link om het certificaat te downloaden](common/copy-metadataurl.png)

### <a name="configure-myworkdrive-sso"></a>Configure MyWorkDrive SSO

1. In een ander browservenster aanmelden bij MyWorkDrive als een beveiligingsbeheerder.

2. Klik op de MyWorkDrive Server in het deelvenster Beheer op **ENTERPRISE** en voer de volgende stappen uit:

    ![De beheerder](./media/myworkdrive-tutorial/tutorial_myworkdrive_admin.png)

    a. Schakel **SAML/ADFS SSO**.

    b. Selecteer **SAML - Azure AD**

    c. In de **Url voor federatieve metagegevens van Azure App** tekstvak, plak de waarde van **App-Url voor federatieve metagegevens** die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **Opslaan**

    > [!NOTE]
    > Voor meer informatie raadpleegt de [MyWorkDrive Azure AD-ondersteuningsartikel](https://www.myworkdrive.com/support/saml-single-sign-on-azure-ad/).

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

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door toegang te verlenen aan MyWorkDrive gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **MyWorkDrive**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-myworkdrive-test-user"></a>MyWorkDrive testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in MyWorkDrive. Werken met [MyWorkDrive ondersteuningsteam](mailto:support@myworkdrive.com) om toe te voegen de gebruikers in het MyWorkDrive-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel MyWorkDrive in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de MyWorkDrive waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)