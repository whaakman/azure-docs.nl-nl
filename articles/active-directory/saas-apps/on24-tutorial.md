---
title: 'Zelfstudie: Azure Active Directory-integratie met ON24 virtuele omgeving SAML-verbinding | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ON24 virtuele omgeving SAML verbinding.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d4028fb5-b2ad-4c5d-b123-7b675c509d64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0827895d58b0b7633ee4543495014c62b5394312
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56209486"
---
# <a name="tutorial-azure-active-directory-integration-with-on24-virtual-environment-saml-connection"></a>Zelfstudie: Azure Active Directory-integratie met ON24 virtuele omgeving SAML-verbinding

In deze zelfstudie leert u hoe u ON24 virtuele omgeving SAML verbinding integreren met Azure Active Directory (Azure AD).

ON24 virtuele omgeving SAML verbinding integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ON24 virtuele omgeving SAML verbinding heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ON24 virtuele omgeving SAML verbinding (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ON24 virtuele omgeving SAML-verbinding, moet u de volgende items:

- Een Azure AD-abonnement
- Een virtuele omgeving SAML-verbinding ON24 eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ON24 virtuele omgeving SAML verbinding uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-on24-virtual-environment-saml-connection-from-the-gallery"></a>ON24 virtuele omgeving SAML verbinding uit de galerie toevoegen
Voor het configureren van de integratie van ON24 virtuele omgeving SAML verbinding in Azure AD, moet u ON24 virtuele omgeving SAML verbinding uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ON24 virtuele omgeving SAML verbinding uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/on24-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/on24-tutorial/a_select_app.png)
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![image](./media/on24-tutorial/a_new_app.png)

4. Typ in het zoekvak **ON24 virtuele omgeving SAML verbinding**, selecteer **ON24 virtuele omgeving SAML verbinding** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de toepassing.

     ![image](./media/on24-tutorial/tutorial_on24_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ON24 virtuele omgeving SAML verbinding op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in ON24 virtuele omgeving SAML verbinding is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ON24 virtuele omgeving SAML verbinding tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met ON24 virtuele omgeving SAML-verbinding, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker ON24 virtuele omgeving SAML verbinding](#create-an-on24-virtual-environment-saml-connection-test-user)**  : als u wilt een equivalent van Britta Simon in ON24 virtuele omgeving SAML-verbinding die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing ON24 virtuele omgeving SAML verbinding.

**Voor het configureren van Azure AD eenmalige aanmelding met ON24 virtuele omgeving SAML-verbinding, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com/)op de **ON24 virtuele omgeving SAML verbinding** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/on24-tutorial/B1_B2_Select_SSO.png)

2. Klik op **modus wijzigen in één aanmelding** boven op het scherm selecteren de **SAML** modus.

      ![image](./media/on24-tutorial/b1_b2_saml_ssso.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/on24-tutorial/b1_b2_saml_sso.png)

4. Klik op de pagina **Eenmalige aanmelding met SAML instellen** u de knop **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![image](./media/on24-tutorial/b1-domains_and_urlsedit.png)

5. Op de **SAML-basisconfiguratie** sectie de volgende stappen uitvoeren als u wilt configureren van de toepassing in **IDP** intiated modus:

    ![image](./media/on24-tutorial/tutorial_on24_url.png)

    a. In de **id** tekstvak typt u een URL:

     **URL van de productie-omgeving**
    
    `SAML-VSHOW.on24.com`

    `SAML-Gateway.on24.com` 

    `SAP PROD SAML-EliteAudience.on24.com` 
                
     **URL van de QA-omgeving**
    
    `SAMLQA-VSHOW.on24.com` 

    `SAMLQA-Gateway.on24.com` 

    `SAMLQA-EliteAudience.on24.com`
 
    b. In de **antwoord-URL** tekstvak typt u een URL:
    
     **URL van de productie-omgeving**
    
    `https://federation.on24.com/sp/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1WU2hvdy5vbjI0LmNvbSJ9/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1HYXRld2F5Lm9uMjQuY29tIn0/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1FbGl0ZUF1ZGllbmNlLm9uMjQuY29tIn0/ACS.saml2`

     **URL van de QA-omgeving**
    
    `https://qafederation.on24.com/sp/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLVZzaG93Lm9uMjQuY29tIn0/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUdhdGV3YXkub24yNC5jb20ifQ/ACS.saml2`
     
    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUVsaXRlQXVkaWVuY2Uub24yNC5jb20ifQ/ACS.saml2` 

    c. Klik op **Extra URL's instellen**. 

    d. In de **Relaystatus** tekstvak typt u een URL: `https://vshow.on24.com/vshow/ms_azure_saml_test?r=<ID>`

    e. Als u wilt configureren van de toepassing in **SP** intiated modus in de **aanmeldings-URL** tekstvak typt u een URL: `https://vshow.on24.com/vshow/<INSTANCENAME>`

6. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** voor het downloaden van het juiste certificaat volgens uw vereiste en sla deze op uw computer.

    ![image](./media/on24-tutorial/tutorial_on24_certificate.png) 

7. Het configureren van eenmalige aanmelding op **ON24 virtuele omgeving SAML verbinding** zijde, moet u het certificaat/metagegevens die u hebt gedownload van Azure portal om te verzenden [ON24 virtuele omgeving SAML verbinding ondersteuning voor team](https://www.on24.com/about-us/support/). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![image](./media/on24-tutorial/d_users_and_groups.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![image](./media/on24-tutorial/d_adduser.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![image](./media/on24-tutorial/d_userproperties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-an-on24-virtual-environment-saml-connection-test-user"></a>Maak een testgebruiker ON24 virtuele omgeving SAML verbinding

In deze sectie maakt u een gebruiker met de naam van Britta Simon in ON24 virtuele omgeving SAML verbinding. Werken met [ON24 virtuele omgeving SAML verbinding ondersteuningsteam](https://www.on24.com/about-us/support/) om toe te voegen de gebruikers in het platform ON24 virtuele omgeving SAML verbinding. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ON24 virtuele omgeving SAML verbinding.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/on24-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **ON24 virtuele omgeving SAML verbinding**.

    ![image](./media/on24-tutorial/tutorial_on24_app.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![image](./media/on24-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/on24-tutorial/d_assign_user.png)

4. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

5. Selecteer in het dialoogvenster **Toewijzing toevoegen** de knop **Toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel ON24 virtuele omgeving SAML verbinding in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing ON24 virtuele omgeving SAML verbinding.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

