---
title: 'Zelfstudie: Azure Active Directory-integratie met Freshservice | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Freshservice configureert.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fbf69ba814b99434e933ed700fb0c8c842c3312
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67101845"
---
# <a name="tutorial-integrate-freshservice-with-azure-active-directory"></a>Zelfstudie: Freshservice integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Freshservice integreert met Azure Active Directory (Azure AD). Wanneer u Freshservice met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Freshservice heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Freshservice met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Ingeschakeld abonnement Freshservice eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Freshservice biedt ondersteuning voor door **SP** geïnitieerde eenmalige aanmelding

## <a name="adding-freshservice-from-the-gallery"></a>Freshservice toevoegen vanuit de galerie

Om de integratie van Freshservice te configureren in Azure AD, moet u Freshservice vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Freshservice** in het zoekvak in.
1. Selecteer **Freshservice** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Freshservice met behulp van een testgebruiker met de naam **Britta Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Freshservice vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Freshservice, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van SSO Freshservice](#configure-freshservice-sso)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Een testgebruiker voor Freshservice maken](#create-freshservice-test-user)** : als u een tegenhanger van Britta Simon in Freshservice wilt hebben die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Freshservice** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<democompany>.freshservice.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<democompany>.freshservice.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met het [klantondersteuningsteam van Freshservice](https://support.freshservice.com/) om deze waarden op te vragen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Klik in de sectie **SAML-handtekeningcertificaat** op de knop **Bewerken** om het dialoogvenster **SAML-handtekeningcertificaat** te openen.

    ![SAML-handtekeningcertificaat bewerken](common/edit-certificate.png)

1. Kopieer in de sectie **SAML-handtekeningcertificaat** de waarde voor **VINGERAFDRUK** en sla deze op de computer op.

    ![Waarde van vingerafdruk kopiëren](common/copy-thumbprint.png)

1. Kopieer in de sectie **Freshservice instellen** de juiste URL('s) op basis van uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-freshservice-sso"></a>Freshservice eenmalige aanmelding configureren

1. Voor het automatiseren van de configuratie in Freshservice, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![Mijn apps-extensie](common/install-myappssecure-extension.png)

2. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup Freshservice** wordt u doorgeleid naar de toepassing Freshservice. Van daaruit, geef de beheerdersreferenties aan te melden bij Freshservice. De browserextensie wordt automatisch configureren van de toepassing voor u en stap 3-6 automatiseren.

    ![Configuratie voor de installatie](common/setup-sso.png)

3. Als u Freshservice handmatig instellen wilt, opent u een nieuw browservenster en meld u in uw site van het bedrijf Freshservice als beheerder en voer de volgende stappen uit:

4. Klik in het menu bovenaan op **Admin**.

    ![Admin](./media/freshservice-tutorial/ic790814.png "Admin")

5. Klik in de **Customer Portal** op **Security**.

    ![Beveiliging](./media/freshservice-tutorial/ic790815.png "Beveiliging")

6. Voer in de sectie **Security** de volgende stappen uit:

    ![Single Sign On](./media/freshservice-tutorial/ic790816.png "Single Sign On")

    a. Zet **Single Sign On** op ON.

    b. Selecteer **SAML SSO**.

    c. Plak in het tekstvak **SAML Login URL** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    d. Plak in het tekstvak **Logout URL** de waarde van **Afmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    e. Plak in het tekstvak **Security Certificate Fingerprint** de waarde van **VINGERAFDRUK** die u hebt gekopieerd uit de Azure-portal.

    f. Klik op **Opslaan**

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

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Freshservice.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer **Freshservice** in de lijst met toepassingen.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-freshservice-test-user"></a>Een testgebruiker maken voor Freshservice

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij FreshService, moeten ze worden ingericht voor FreshService. In het geval van FreshService is dat een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Aanmelden bij uw **FreshService** bedrijf site als beheerder.

2. Klik in het menu bovenaan op **Admin**.

    ![Admin](./media/freshservice-tutorial/ic790814.png "Admin")

3. Klik in de sectie **User Management** op **Requesters**.

    ![Requesters](./media/freshservice-tutorial/ic790818.png "Requesters")

4. Klik op **New Requester**.

    ![New Requester](./media/freshservice-tutorial/ic790819.png "New Requester")

5. Voer de volgende stappen uit in de sectie **New Requester**:

    ![New Requester](./media/freshservice-tutorial/ic790820.png "New Requester")  

    a. Typ in de tekstvakken **First Name** en **Email** kenmerken van een geldig Azure Active Directory-account dat u wilt inrichten.

    b. Klik op **Opslaan**.

    > [!NOTE]
    > De houder van het Azure Active Directory-account ontvangt een e-mail met een koppeling om het account te bevestigen voordat het actief wordt.
    >  

> [!NOTE]
> U kunt andere hulpprogramma's of API's van FreshService gebruiken om AAD-gebruikersaccounts in te richten.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Freshservice in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Freshservice waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
