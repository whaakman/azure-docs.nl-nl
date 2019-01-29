---
title: 'Zelfstudie: Azure Active Directory-integratie met Land echte reus Client | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en echte reus Land.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: jeedes
ms.openlocfilehash: f8f62736c803ec58c473d563728f68cfc729b974
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179188"
---
# <a name="tutorial-azure-active-directory-integration-with-land-gorilla-client"></a>Zelfstudie: Azure Active Directory-integratie met Land echte reus-Client

In deze zelfstudie leert u hoe u Land echte reus Client integreren met Azure Active Directory (Azure AD).

Land echte reus Client integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Land echte reus Client heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Land echte reus Client (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - de Azure Management portal beheren

Zie [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.


## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Land echte reus-Client, moet u de volgende items:

- Een Azure AD-abonnement
- Een Land echte reus Client eenmalige aanmelding ingeschakeld abonnement


> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.


Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.


## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Land echte reus Client uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-land-gorilla-client-from-the-gallery"></a>Land echte reus Client uit de galerie toe te voegen
Voor het configureren van de integratie van Land echte reus Client in Azure AD, moet u Land echte reus Client uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Land echte reus Client uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Land echte reus Client**.

    ![Het maken van een Azure AD-testgebruiker](./media/landgorilla-tutorial/tutorial_landgorilla_search.png)

1. Selecteer in het deelvenster resultaten **Land echte reus Client**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/landgorilla-tutorial/tutorial_landgorilla_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Land echte reus-Client op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Land echte reus Client is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Land echte reus Client tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Land echte reus-Client.

Om te configureren en testen van Azure AD eenmalige aanmelding met Land echte reus-Client, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met een beperkte groep.
1. **[Het maken van een testgebruiker Land echte reus](#creating-a-land-gorilla-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure Management portal inschakelen en configureren van eenmalige aanmelding in uw Land echte reus-clienttoepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Land echte reus-Client, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, op de **Land echte reus Client** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **SAML gebaseerde aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/landgorilla-tutorial/tutorial_landgorilla_samlbase.png)

1. Op de **Land echte reus clientdomein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/landgorilla-tutorial/tutorial_landgorilla_url_02.png)

    a. In de **id** tekstvak typt u de waarde met een van de volgende patroon: 
    
    `https://<customer domain>.landgorilla.com/` 
    
    `https://www.<customer domain>.landgorilla.com`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van een van de volgende patroon:

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`
    
    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`

    > [!NOTE] 
    > Houd er rekening mee dat dit niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met de werkelijke id en de antwoord-URL. Wij raden u aan hiervoor de unieke waarde van de tekenreeks in de id te gebruiken. Neem contact op met [Land echte reus Client team](https://www.landgorilla.com/support/) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/landgorilla-tutorial/tutorial_landgorilla_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/landgorilla-tutorial/tutorial_general_400.png) 

1. Als u SSO-configuratie voltooid voor uw toepassing aan Land echte reus einde, neem contact op met [Land echte reus Client ondersteuningsteam](https://www.landgorilla.com/support/) en geef ze de gedownloade **"Metadata XML** bestand.


### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in de Azure Management portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/landgorilla-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/landgorilla-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/landgorilla-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/landgorilla-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 

### <a name="creating-a-land-gorilla-test-user"></a>Het maken van een testgebruiker Land echte reus

Neem contact op met [Land echte reus ondersteuningsteam](https://www.landgorilla.com/support/) om toe te voegen de gebruikers in het Land echte reus-platform.
    
### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon haar door toegang te verlenen aan de grond echte reus Client gebruik van Azure eenmalige aanmelding.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Land echte reus client, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-beheerportal, en vervolgens gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Land echte reus Client**.

    ![Eenmalige aanmelding configureren](./media/landgorilla-tutorial/tutorial_landgorilla_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    


### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Land echte reus Client in het toegangsvenster, u moet u automatisch aangemeld bij uw Land echte reus-clienttoepassing.


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/landgorilla-tutorial/tutorial_general_01.png
[2]: ./media/landgorilla-tutorial/tutorial_general_02.png
[3]: ./media/landgorilla-tutorial/tutorial_general_03.png
[4]: ./media/landgorilla-tutorial/tutorial_general_04.png

[100]: ./media/landgorilla-tutorial/tutorial_general_100.png
[200]: ./media/landgorilla-tutorial/tutorial_general_200.png
[201]: ./media/landgorilla-tutorial/tutorial_general_201.png
[202]: ./media/landgorilla-tutorial/tutorial_general_202.png
[203]: ./media/landgorilla-tutorial/tutorial_general_203.png
