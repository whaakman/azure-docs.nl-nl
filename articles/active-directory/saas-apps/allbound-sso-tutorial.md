---
title: 'Zelfstudie: Azure Active Directory-integratie met Allbound SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Allbound SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 15011ddf-941f-4da2-b993-40ad94a08e42
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeedes
ms.openlocfilehash: e0658841e121dc8ea5bd4c704d99fd6bac98a32f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55171383"
---
# <a name="tutorial-azure-active-directory-integration-with-allbound-sso"></a>Zelfstudie: Azure Active Directory-integratie met Allbound SSO

In deze zelfstudie leert u hoe u Allbound SSO integreren met Azure Active Directory (Azure AD).

Allbound SSO integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Allbound SSO heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Allbound SSO (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Allbound SSO, moet u de volgende items:

- Een Azure AD-abonnement
- Een Allbound SSO eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Allbound SSO uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-allbound-sso-from-the-gallery"></a>Allbound SSO uit de galerie toe te voegen

Voor het configureren van de integratie van Allbound SSO in Azure AD, moet u Allbound SSO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Allbound SSO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Allbound SSO**, selecteer **Allbound SSO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Allbound SSO in de lijst met resultaten](./media/allbound-sso-tutorial/tutorial_allbound-sso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Allbound SSO op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Allbound SSO is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Allbound SSO tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Allbound SSO, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Allbound SSO](#creating-an-allbound-sso-test-user)**  : als u wilt een equivalent van Britta Simon in Allbound SSO die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Allbound SSO.

**Voor het configureren van Azure AD eenmalige aanmelding met Allbound SSO, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Allbound SSO** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. Voer in het gedeelte **Standaard SAML-configuratie** de volgende stappen uit als u de toepassing in de door **IDP geïnitieerde** modus wilt configureren:

    ![Allbound SSO-domein en URL's, eenmalige aanmelding informatie](./media/allbound-sso-tutorial/tutorial_allbound-sso_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<SUBDOMAIN>.allbound.com/`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<SUBDOMAIN>.allbound.com/acs`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![Allbound SSO-domein en URL's, eenmalige aanmelding informatie](./media/allbound-sso-tutorial/tutorial_allbound-sso_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<SUBDOMAIN>.allbound.com/`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Allbound SSO-Client-ondersteuningsteam](mailto:engineering@allbound.com) om deze waarden te verkrijgen.

6. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **voorfederatievemetagegevens-XML** en sla het bestand met metagegevens op uw computer.

    ![De link om het certificaat te downloaden](./media/allbound-sso-tutorial/tutorial_allbound-sso_certificate.png) 

7. Op de **Allbound SSO instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![Allbound SSO-configuratie](common/configuresection.png)

8. Het configureren van eenmalige aanmelding op **Allbound SSO** zijde, moet u voor het verzenden van de gedownloade **federatieve metagegevens-XML** naar [Allbound SSO-ondersteuningsteam](mailto:engineering@allbound.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-an-allbound-sso-test-user"></a>Het maken van een testgebruiker Allbound SSO

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Allbound SSO. Allbound SSO biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Allbound SSO als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Allbound SSO-ondersteuningsteam](mailto:engineering@allbound.com).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Allbound SSO.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Allbound SSO**.

    ![Eenmalige aanmelding configureren](./media/allbound-sso-tutorial/tutorial_allbound-sso_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Allbound SSO in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Allbound SSO.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
