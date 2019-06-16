---
title: 'Zelfstudie: Azure Active Directory-integratie met Azure AD SAML Toolkit | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Azure AD-SAML-Toolkit.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 3f4348e7-c34e-43c7-926e-f1b26ffacf6d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4baeda4fca69969fae3f866e642ad3dc8ad46509
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67091937"
---
# <a name="tutorial-integrate-azure-ad-saml-toolkit-with-azure-active-directory"></a>Zelfstudie: Azure AD-SAML-Toolkit integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Azure AD SAML Toolkit integreert met Azure Active Directory (Azure AD). Wanneer u Azure AD SAML Toolkit met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Azure AD-SAML-Toolkit heeft.
* Kunnen uw gebruikers worden automatisch aangemeld bij Azure AD-SAML-Toolkit met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u de gratis proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).
* Azure AD SAML Toolkit eenmalige aanmelding (SSO) ingeschakeld abonnement.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Azure AD SAML Toolkit **SP** gestart door SSO.

## <a name="adding-azure-ad-saml-toolkit-from-the-gallery"></a>Azure AD SAML Toolkit uit de galerie toe te voegen

Voor het configureren van de integratie van Azure AD SAML Toolkit in Azure AD, moet u Azure AD SAML Toolkit uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Azure AD SAML Toolkit** in het zoekvak in.
1. Selecteer **Azure AD SAML Toolkit** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Azure AD-SAML-Toolkit met behulp van een testgebruiker met de naam **B.Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in Azure AD-SAML-Toolkit.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Azure AD SAML Toolkit, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van Azure AD-SAML-Toolkit SSO](#configure-azure-ad-saml-toolkit-sso)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B.Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B.Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maak Toolkit voor Azure AD SAML testgebruiker](#create-azure-ad-saml-toolkit-test-user)**  hebben een equivalent van B.Simon in Azure AD-SAML-Toolkit die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Azure AD SAML Toolkit** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

    1. In het tekstvak **Aanmeldings-URL** typt u een URL: `https://samltoolkit.azurewebsites.net/`

    1. Typ een URL in het vak **Id (Entiteits-id)** : `https://samltoolkit.azurewebsites.net`

    1. In het tekstvak **Antwoord-URL** typt u een URL: `https://samltoolkit.azurewebsites.net/SAML/Consume`

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **federatieve metagegevens-XML**  uit de opgegeven opties aan de hand van uw behoeften en bewaar deze op uw computer.

   ![De link om het certificaat te downloaden](common/metadataxml.png)

1. Op de **instellen van Azure AD SAML Toolkit** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-azure-ad-saml-toolkit-sso"></a>Azure AD SAML Toolkit SSO configureren

1. Open een nieuw browservenster, als u niet hebt geregistreerd in de Azure AD SAML Toolkit-website, eerst registreren door te klikken op de **registreren**. Als u al hebt geregistreerd, meld u aan bij uw Azure AD SAML Toolkit bedrijf site met behulp van de geregistreerde sign in referenties.

    ![Azure AD SAML Toolkit registreren](./media/saml-toolkit-tutorial/register.png)

1. Klik op de **SAML-configuratie**.

    ![Azure AD-SAML Toolkit SAML-configuratie](./media/saml-toolkit-tutorial/saml-configure.png)

1. Klik op **Create**.

    ![Azure AD-SAML-Toolkit maakt eenmalige aanmelding](./media/saml-toolkit-tutorial/createsso.png)

1. Op de **SAML SSO-configuratie** pagina, voert u de volgende stappen uit:

    ![Azure AD-SAML-Toolkit maakt eenmalige aanmelding](./media/saml-toolkit-tutorial/fill-details.png)

    1. In de **aanmeldings-URL** tekstvak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    1. In de **Azure AD-id** tekstvak, plak de **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure portal.

    1. Plak in het tekstvak voor de **afmeldings-URL** de waarde van de **afmeldings-URL** die u uit Azure Portal hebt gekopieerd.

    1. Klik op **bestand kiezen** en upload het **federatiemetagegevens XML** -bestand dat u hebt gedownload vanuit Azure portal.

    1. Klik op **Create**.

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

In deze sectie schakelt u B.Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Azure AD-SAML-Toolkit.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Azure AD SAML Toolkit**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B.Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-azure-ad-saml-toolkit-test-user"></a>Azure AD SAML Toolkit testgebruiker maken

In deze sectie wordt een gebruiker met de naam B.Simon gemaakt in Azure AD-SAML-Toolkit. Azure AD-SAML-Toolkit ondersteunt just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in Azure AD SAML Toolkit bestaat, wordt een nieuw gemaakt nadat verificatie.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Azure AD SAML Toolkit in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Azure AD-SAML-Toolkit waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
