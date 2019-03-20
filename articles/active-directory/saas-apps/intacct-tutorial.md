---
title: 'Zelfstudie: Azure Active Directory-integratie met Intacct | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Intacct.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: cd10adbc509812a360ae59fdac3f16bd2af0324c
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58188149"
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Zelfstudie: Azure Active Directory-integratie met Intacct

In deze zelfstudie leert u hoe u Intacct integreren met Azure Active Directory (Azure AD).
Intacct integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Intacct heeft.
* U kunt uw gebruikers worden automatisch aangemeld Intacct (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Intacct, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Eenmalige aanmelding Intacct ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Intacct **IDP** gestart door SSO

## <a name="adding-intacct-from-the-gallery"></a>Intacct uit de galerie toe te voegen

Voor het configureren van de integratie van Intacct in Azure AD, moet u Intacct uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Intacct uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Intacct**, selecteer **Intacct** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Intacct in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Intacct op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Intacct tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Intacct, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding Intacct](#configure-intacct-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker Intacct](#create-intacct-test-user)**  : als u wilt een equivalent van Britta Simon in Intacct die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Intacct, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Intacct** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Intacct domein en URL's, eenmalige aanmelding informatie](common/idp-reply.png)

    In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE]
    > De waarde is niet echt. Werk de waarde bij met de werkelijke antwoord-URL. Neem contact op met [Intacct Client ondersteuningsteam](https://us.intacct.com/support) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Op de **Intacct instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-intacct-single-sign-on"></a>Intacct voor eenmalige aanmelding configureren

1. In een ander browservenster aanmelden bij uw bedrijf Intacct site als beheerder.

1. Klik op de **bedrijf** tabblad en klik vervolgens op **bedrijfsgegevens**.

    ![Company] (Bedrijf)(./media/intacct-tutorial/ic790037.png "Company") (Bedrijf)

1. Klik op de **Security** tabblad en klik vervolgens op **bewerken**.

    ![Beveiliging](./media/intacct-tutorial/ic790038.png "Beveiliging")

1. In de **eenmalige aanmelding (SSO)** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding](./media/intacct-tutorial/ic790039.png "eenmalige aanmelding")

    a. Selecteer **eenmalige aanmelding inschakelen**.

    b. Als **type id-provider**, selecteer **SAML 2.0**.

    c. In **URL-verlener** tekstvak, plak de waarde van **Azure Ad-id** die u hebt gekopieerd vanuit Azure portal.

    d. Plak in het tekstvak **Login URL** de waarde van **Aanmeldings-URL** die u hebt gekopieerd uit de Azure-portal.

    e. Open uw **base-64** gecodeerd certificaat in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **certificaat** vak.

    f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Intacct.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Intacct**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Intacct**.

    ![De koppeling Intacct in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-intacct-test-user"></a>Intacct testgebruiker maken

Als u Azure AD-gebruikers instelt, zodat ze kunnen zich aanmelden bij Intacct, moeten ze worden ingericht voor Intacct. Voor Intacct is het inrichten van een handmatige taak.

**Voor het inrichten van gebruikersaccounts, moet u de volgende stappen uitvoeren:**

1. Aanmelden bij uw **Intacct** tenant.

1. Klik op de **bedrijf** tabblad en klik vervolgens op **gebruikers**.

    ![Gebruikers](./media/intacct-tutorial/ic790041.png "Gebruikers")

1. Klik op de **toevoegen** tabblad.

    ![Add](./media/intacct-tutorial/ic790042.png "Add")

1. In de **gebruikersgegevens** sectie, voert u de volgende stappen uit:

    ![Gebruikersgegevens](./media/intacct-tutorial/ic790043.png "gebruikersgegevens")

    a. Voer de **gebruikers-ID**, wordt de **achternaam**, **voornaam**, wordt de **e-mailadres**, wordt de **titel**, en de **Phone** van een Azure AD-account dat u wilt inrichten in de **gebruikersgegevens** sectie.

    b. Selecteer de **beheerdersbevoegdheden** van een Azure AD-account dat u inrichten wilt.

    c. Klik op **Opslaan**. De houder van Azure AD-account ontvangt een e-mailbericht en volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

> [!NOTE]
> U kunt andere Intacct gebruiker-account maken-hulpprogramma's of API's die worden geleverd door Intacct gebruiken voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Intacct in het toegangsvenster, moet u worden automatisch aangemeld bij de Intacct waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

