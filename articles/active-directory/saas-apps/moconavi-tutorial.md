---
title: 'Zelfstudie: Azure Active Directory-integratie met moconavi | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en moconavi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: 5b2321713660cb3bd16d95a386b68a5864e32c40
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57886538"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Zelfstudie: Azure Active Directory-integratie met moconavi

In deze zelfstudie leert u hoe u moconavi integreren met Azure Active Directory (Azure AD).
Moconavi integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot moconavi heeft.
* U kunt uw gebruikers worden automatisch aangemeld moconavi (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met moconavi, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* eenmalige aanmelding moconavi ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* biedt ondersteuning voor moconavi **SP** gestart door SSO

## <a name="adding-moconavi-from-the-gallery"></a>Moconavi uit de galerie toe te voegen

Voor het configureren van de integratie van moconavi in Azure AD, moet u moconavi uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen moconavi uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **moconavi**, selecteer **moconavi** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![moconavi in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met moconavi op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in moconavi tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met moconavi, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van moconavi Single Sign-On](#configure-moconavi-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker moconavi](#create-moconavi-test-user)**  : als u wilt een equivalent van Britta Simon in moconavi die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met moconavi, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **moconavi** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![moconavi domein en URL's één aanmeldings-informatie](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<yourserverurl>/moconavi-saml2/saml/login`

    b. In het tekstvak **Id** typt u een URL met het volgende patroon: `https://<yourserverurl>/moconavi-saml2`

    c. In het tekstvak **Antwoord-URL** typt u een URL met het volgende patroon: `https://<yourserverurl>/moconavi-saml2/saml/SSO`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [moconavi Client ondersteuningsteam](mailto:support@recomot.co.jp) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **moconavi instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-moconavi-single-sign-on"></a>Moconavi eenmalige aanmelding configureren

Het configureren van eenmalige aanmelding op **moconavi** zijde, moet u voor het verzenden van de gedownloade **federatieve metagegevens-XML** en toepassing van de gekopieerde URL's van Azure portal om te [moconavi ondersteuningsteam](mailto:support@recomot.co.jp). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot moconavi.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **moconavi**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **moconavi**.

    ![De koppeling moconavi in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-moconavi-test-user"></a>Moconavi testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in moconavi. Werken met [moconavi ondersteuningsteam](mailto:support@recomot.co.jp) om toe te voegen de gebruikers in het moconavi-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

1. Installeer moconavi vanuit Microsoft store.

2. Start moconavi.

3. Klik op **verbinding maken met de instelling** knop.

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing1.png)

4. Voer `https://mcs-admin.moconavi.biz/gateway` in **verbinding maken met URL** tekstvak en klik vervolgens op **gedaan** knop.

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing2.png)

5. Voer de volgende stappen uit op de volgende schermafbeelding:

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing3.png)

    a. Voer **invoer verificatiesleutel**:`azureAD` in **invoer verificatiesleutel** tekstvak.

    b. Voer **invoer van gebruikers-ID**: `your ad account` in **invoer van gebruikers-ID** tekstvak.

    c. Klik op **aanmelding**.

6. Voer uw Azure AD-wachtwoord in **wachtwoord** tekstvak en klik vervolgens op **aanmelding** knop.

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing4.png)

7. Azure AD-verificatie is geslaagd, wanneer het menu wordt weergegeven.

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

