---
title: 'Zelfstudie: Azure Active Directory-integratie met adaptieve Insights | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en adaptieve inzichten.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeedes
ms.openlocfilehash: 74f38ef3043cfa0e6df40c264b72cc5eae38544e
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430286"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Zelfstudie: Azure Active Directory-integratie met geavanceerde inzichten

In deze zelfstudie leert u hoe u adaptieve Insights integreert met Azure Active Directory (Azure AD).

Adaptieve Insights integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot geavanceerde inzichten heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij adaptieve Insights (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met adaptieve Insights, hebt u de volgende items nodig:

- Een Azure AD-abonnement
- Een adaptieve Insights eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Adaptieve inzichten uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adaptive-insights-from-the-gallery"></a>Adaptieve inzichten uit de galerie toe te voegen
Voor het configureren van de integratie van adaptieve inzicht in Azure AD, moet u adaptieve Insights vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen adaptieve inzichten uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/adaptivesuite-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/adaptivesuite-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/adaptivesuite-tutorial/a_new_app.png)

4. Typ in het zoekvak **adaptieve Insights**, selecteer **adaptieve Insights** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met geavanceerde inzichten op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in adaptieve Insights is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in adaptieve Insights tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met adaptieve Insights, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker adaptieve Insights](#create-an-adaptive-insights-test-user)**  : als u wilt een equivalent van Britta Simon in adaptieve inzichten die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing adaptieve inzichten.

**Voor het configureren van Azure AD eenmalige aanmelding met adaptieve Insights, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **adaptieve Insights** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/adaptivesuite-tutorial/B1_B2_Select_SSO.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/adaptivesuite-tutorial/b1_b2_saml_sso.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/adaptivesuite-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** intiated modus:

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    a. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > U kunt de id (entiteits-ID) en waarden voor antwoord-URL ophalen uit de adaptieve inzichten **SAML SSO-instellingen** pagina.
 
5. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van de **certificaat (Base64)** en sla deze op uw computer.

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_certficate.png) 

6. Op de **instellen van adaptieve Insights** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    Houd er rekening mee dat de URL kan bijvoorbeeld het volgende:

    a. Aanmeldings-URL

    b. Azure Ad-id

    c. URL voor afmelden

    ![image](./media/adaptivesuite-tutorial/d1_samlsonfigure.png) 

7. In een ander browservenster aanmelden bij uw bedrijf adaptieve Insights site als beheerder.

8. Ga naar **Admin**.

    ![Beheerder](./media/adaptivesuite-tutorial/IC805644.png "Admin")

9. In de **gebruikers en rollen** sectie, klikt u op **SAML SSO-instellingen beheren**.

    ![SAML SSO-instellingen beheren](./media/adaptivesuite-tutorial/IC805645.png "SAML SSO-instellingen beheren")

10. Op de **SAML SSO-instellingen** pagina, voert u de volgende stappen uit:

    ![SAML SSO-instellingen](./media/adaptivesuite-tutorial/IC805646.png "SAML SSO-instellingen")

    a. In de **identiteit providernaam** tekstvak, typ een naam voor uw configuratie.

    b. Plak de **Azure Ad-id** waarde opgehaald uit Azure portal in de **id-provider entiteit-ID** tekstvak.

    c. Plak de **aanmeldings-URL** waarde opgehaald uit Azure portal in de **id-provider voor eenmalige aanmelding URL** tekstvak.

    d. Plak de **afmeldings-URL van** waarde opgehaald uit Azure portal in de **afmeldings-URL van aangepaste** tekstvak.

    e. Als u wilt uw gedownloade certificaat uploaden, klikt u op **bestand kiezen**.

    f. Selecteer het volgende voor:

    * **SAML-gebruikers-id**, selecteer **adaptieve Insights-gebruikersnaam van de gebruiker**.

    * **SAML-id gebruikerslocatie**, selecteer **gebruikers-id NameID van onderwerp**.

    * **SAML NameID-indeling**, selecteer **e-mailadres**.

    * **SAML inschakelen**, selecteer **toestaan SAML SSO en direct adaptieve Insights aanmelden**.

    g. Kopie **adaptieve Insights SSO URL** en plak deze in de **id (entiteits-ID)** en **antwoord-URL** tekstvakken in de **adaptieve Insights domein en URL's** sectie in Azure portal.

    h. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/adaptivesuite-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/adaptivesuite-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/adaptivesuite-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-an-adaptive-insights-test-user"></a>Maak een testgebruiker adaptieve Insights

Om Azure AD-gebruikers zich aanmelden bij geavanceerde inzichten, moeten ze worden ingericht in adaptieve inzichten. In het geval van adaptieve Insights is inrichten een handmatige taak.

**Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:** 

1. Meld u aan bij uw **adaptieve Insights** bedrijf site als beheerder.
2. Ga naar **Admin**.

   ![Beheerder](./media/adaptivesuite-tutorial/IC805644.png "Admin")

3. In de **gebruikers en rollen** sectie, klikt u op **gebruiker toevoegen**.

   ![Gebruiker toevoegen](./media/adaptivesuite-tutorial/IC805648.png "gebruiker toevoegen")
   
4. In de **nieuwe gebruiker** sectie, voert u de volgende stappen uit:

   ![Indienen](./media/adaptivesuite-tutorial/IC805649.png "verzenden")

   a. Type de **naam**, **aanmelding**, **e**, **wachtwoord** van een geldige Azure Active Directory-gebruiker u wilt inrichten in de gerelateerde tekstvakken.

   b. Selecteer een **rol**.

   c. Klik op **indienen**.

>[!NOTE]
>Kunt u een andere adaptieve Insights gebruiker-account maken-hulpprogramma's of API's die door adaptieve Insights inrichten AAD-gebruikersaccounts.
>

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot geavanceerde inzichten.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/adaptivesuite-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **adaptieve Insights**.

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_app.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/adaptivesuite-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/adaptivesuite-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel adaptieve inzichten in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing adaptieve inzichten.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
