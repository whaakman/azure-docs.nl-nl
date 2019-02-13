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
ms.collection: M365-identity-device-management
ms.openlocfilehash: e334795a63ff298ff8f0695340f35fdff1ad03e0
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56166518"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Zelfstudie: Azure Active Directory-integratie met geavanceerde inzichten

In deze zelfstudie leert u hoe u adaptieve Insights integreert met Azure Active Directory (Azure AD).

Adaptieve Insights integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot geavanceerde inzichten heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij adaptieve Insights (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met adaptieve Insights, hebt u de volgende items nodig:

- Een Azure AD-abonnement
- Een adaptieve Insights eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
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
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![image](./media/adaptivesuite-tutorial/a_new_app.png)

4. Typ in het zoekvak **adaptieve Insights**, selecteer **adaptieve Insights** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met geavanceerde inzichten op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in adaptieve Insights is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in adaptieve Insights tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met adaptieve Insights, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker adaptieve Insights](#create-an-adaptive-insights-test-user)**  : als u wilt een equivalent van Britta Simon in adaptieve inzichten die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing adaptieve inzichten.

**Voor het configureren van Azure AD eenmalige aanmelding met adaptieve Insights, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **adaptieve Insights** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/adaptivesuite-tutorial/B1_B2_Select_SSO.png)

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/adaptivesuite-tutorial/b1_b2_saml_sso.png)

3. Klik op de pagina **Eenmalige aanmelding met SAML instellen** u de knop **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![image](./media/adaptivesuite-tutorial/b1-domains_and_urlsedit.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** intiated modus:

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    a. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > U kunt de id (entiteits-ID) en waarden voor antwoord-URL ophalen uit de adaptieve inzichten **SAML SSO-instellingen** pagina.
 
5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **certificaat (Base64)** te downloaden, en slaat u dit op uw computer op.

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_certficate.png) 

6. Op de **instellen van adaptieve Insights** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    Houd er rekening mee dat de URL kan bijvoorbeeld het volgende:

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![image](./media/adaptivesuite-tutorial/d1_samlsonfigure.png) 

7. In een ander browservenster aanmelden bij uw bedrijf adaptieve Insights site als beheerder.

8. Ga naar **Admin**.

    ![Admin](./media/adaptivesuite-tutorial/IC805644.png "Admin")

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

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![image](./media/adaptivesuite-tutorial/d_users_and_groups.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![image](./media/adaptivesuite-tutorial/d_adduser.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![image](./media/adaptivesuite-tutorial/d_userproperties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-an-adaptive-insights-test-user"></a>Maak een testgebruiker adaptieve Insights

Om Azure AD-gebruikers zich aanmelden bij geavanceerde inzichten, moeten ze worden ingericht in adaptieve inzichten. In het geval van adaptieve Insights is inrichten een handmatige taak.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:** 

1. Meld u aan bij uw **adaptieve Insights** bedrijf site als beheerder.
2. Ga naar **Admin**.

   ![Admin](./media/adaptivesuite-tutorial/IC805644.png "Admin")

3. In de **gebruikers en rollen** sectie, klikt u op **gebruiker toevoegen**.

   ![Gebruiker toevoegen](./media/adaptivesuite-tutorial/IC805648.png "Gebruiker toevoegen")
   
4. In de **nieuwe gebruiker** sectie, voert u de volgende stappen uit:

   ![Indienen](./media/adaptivesuite-tutorial/IC805649.png "verzenden")

   a. Type de **naam**, **aanmelding**, **e**, **wachtwoord** van een geldige Azure Active Directory-gebruiker u wilt inrichten in de gerelateerde tekstvakken.

   b. Selecteer een **rol**.

   c. Klik op **Submit**

>[!NOTE]
>Kunt u een andere adaptieve Insights gebruiker-account maken-hulpprogramma's of API's die door adaptieve Insights inrichten AAD-gebruikersaccounts.
>

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot geavanceerde inzichten.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/adaptivesuite-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **adaptieve Insights**.

    ![image](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_app.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![image](./media/adaptivesuite-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/adaptivesuite-tutorial/d_assign_user.png)

4. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

5. Selecteer in het dialoogvenster **Toewijzing toevoegen** de knop **Toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel adaptieve inzichten in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing adaptieve inzichten.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
