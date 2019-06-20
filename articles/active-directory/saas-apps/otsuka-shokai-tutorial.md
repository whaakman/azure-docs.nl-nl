---
title: 'Zelfstudie: Azure Active Directory-integratie met Otsuka Shokai | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Otsuka Shokai.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: c04bb636-a3c7-4940-9b36-810425b855d1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd06eafca2c508bc73fa2b327235621797be417c
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274192"
---
# <a name="tutorial-integrate-otsuka-shokai-with-azure-active-directory"></a>Zelfstudie: Otsuka Shokai integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Otsuka Shokai integreert met Azure Active Directory (Azure AD). Wanneer u Otsuka Shokai met Azure AD integreert, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Otsuka Shokai heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Otsuka Shokai met hun Azure AD-accounts.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement Otsuka Shokai eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Otsuka Shokai **IDP** gestart door SSO.

## <a name="adding-otsuka-shokai-from-the-gallery"></a>Otsuka Shokai uit de galerie toe te voegen

Voor het configureren van de integratie van Otsuka Shokai in Azure AD, moet u Otsuka Shokai uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Otsuka Shokai** in het zoekvak in.
1. Selecteer **Otsuka Shokai** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.
1. Klik op **eigenschappen** tabblad, kopieert u de **toepassings-ID** en sla deze op uw computer voor later gebruik.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Otsuka Shokai met behulp van een testgebruiker met de naam **B. Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Otsuka Shokai vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Otsuka Shokai, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
2. **[Configureren van Otsuka Shokai](#configure-otsuka-shokai)**  de SSO-instellingen configureren op de kant van de toepassing.
3. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B. Simon.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B. Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Maken van de testgebruiker Otsuka Shokai](#create-otsuka-shokai-test-user)**  hebben een equivalent van B. Simon in Otsuka Shokai die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Otsuka Shokai** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **eenmalige aanmelding**.
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, de toepassing is vooraf geconfigureerd en de vereiste URL's al vooraf zijn ingevuld met Azure. De gebruiker moet de configuratie op te slaan door te klikken op de **opslaan** knop.

1. Otsuka Shokai toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende schermafbeelding ziet u de lijst met standaardkenmerken, waar u als **nameidentifier** is toegewezen met **user.userprincipalname**. Otsuka Shokai toepassing verwacht **nameidentifier** worden toegewezen met **user.objectid**, dus u de kenmerktoewijzing van het bewerken moet door te klikken op **Bewerken**  pictogram en wijzig de kenmerktoewijzing.

    ![image](common/edit-attribute.png)

1. Bovendien hierboven verwacht Otsuka Shokai toepassing paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel:

    | Name | Bronkenmerk|
    | ---------------| --------------- |
    | toepassings-id | `<Application ID>` |

    >[!NOTE]
    >`<Application ID>` is de waarde die u hebt gekopieerd uit de **eigenschappen** tabblad van Azure-portal.

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

### <a name="configure-otsuka-shokai"></a>Otsuka Shokai configureren

1. Wanneer u verbinding met mijn pagina van de klant van SSO-app, de wizard van SSO-instelling wordt gestart.

2. Als Otsuka-ID is niet geregistreerd, gaat u verder met de registratie van nieuwe Otsuka-ID.   Als u Otsuka-ID hebt geregistreerd, gaat u verder met de instelling opeenvolging van koppelingen.

3. Gaat u verder met het einde en wanneer het bovenste scherm wordt weergegeven na het aanmelden op de pagina van de klant, de SSO-instellingen zijn voltooid.

4. De volgende keer dat u verbinding met mijn pagina van de klant van de SSO-app maken nadat het scherm richtlijnen wordt geopend, wordt het bovenste scherm weergegeven na het aanmelden bij mijn pagina van de klant.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B. Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B. Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon Azure eenmalige aanmelding door toegang te verlenen aan Otsuka Shokai gebruiken.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Otsuka Shokai**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B. Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-otsuka-shokai-test-user"></a>Otsuka Shokai testgebruiker maken

Registratie van uw SaaS-account wordt op de eerste toegang tot Otsuka Shokai worden uitgevoerd. Bovendien wordt er ook Azure AD-account en SaaS-account koppelen op het moment dat nieuwe gemaakt.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de tegel Otsuka Shokai in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij de Otsuka Shokai waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
