---
title: 'Zelfstudie: Azure Active Directory-integratie met BambooHR | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en BambooHR.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: d31d8d92590f665b847bb92370967e88ef4590ff
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36210700"
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>Zelfstudie: Azure Active Directory-integratie met BambooHR

In deze zelfstudie leert u hoe BambooHR integreren met Azure Active Directory (Azure AD).

BambooHR integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BambooHR heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij BambooHR via eenmalige aanmelding (SSO) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BambooHR, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement BambooHR SSO-ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie test, wordt u aangeraden niet een productie-omgeving te gebruiken.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een gratis evaluatieversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat geeft een overzicht van deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BambooHR uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-bamboohr-from-the-gallery"></a>BambooHR uit de galerie toevoegen
Voor het configureren van de integratie van BambooHR in Azure AD BambooHR uit de galerie toevoegt aan de lijst met beheerde SaaS-apps als volgt:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het deelvenster Enterprise-toepassingen][2]
    
3. Selecteer om een toepassing toe **nieuwe toepassing**.

    !['Nieuwe application' knop][3]

4. Typ in het zoekvak **BambooHR**. Selecteer in de lijst met resultaten **BambooHR**, en selecteer vervolgens **toevoegen**.

    ![BambooHR in de lijst met resultaten](./media/bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen van Azure AD-SSO met BambooHR met behulp van de testgebruiker "Britta Simon."

Azure AD moet weten wat de gebruiker equivalent is BambooHR voor eenmalige aanmelding werkt. Met andere woorden, moet u een koppeling relatie tussen de Azure AD-gebruiker en de betreffende gebruiker instellen in BambooHR.

Wijs de Azure AD om vast te stellen de relatie van de koppeling in BambooHR, **gebruikersnaam** waarde als de BambooHR **gebruikersnaam** waarde.

Als u wilt configureren en testen van Azure AD-SSO met BambooHR, voltooi de bouwstenen in de volgende vijf secties.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD-eenmalige aanmelding inschakelen in de Azure-portal en eenmalige aanmelding in uw toepassing BambooHR als volgt configureren:

1. In de Azure-portal op de **BambooHR** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** venster in de **modus** vervolgkeuzelijst, selecteer **op basis van SAML aanmelding**.
 
    ![Venster voor eenmalige aanmelding](./media/bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. Onder **BambooHR domein en de URL's**, het volgende doen:

    ![De sectie BambooHR domein en de URL 's](./media/bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    a. In de **aanmelden URL** vak, typt u een URL in de volgende indeling: `https://<company>.bamboohr.com`.

    b. In de **id** typt u een waarde: `BambooHR-SAML`.

    > [!NOTE] 
    > De **aanmelden URL** waarde is geen echte. Bijwerken met de werkelijke aanmeldings-URL. De waarde, neem contact op met de [BambooHR client ondersteuningsteam](https://www.bamboohr.com/contact.php). 
 
4. Onder **SAML-certificaat voor ondertekening van**, selecteer **certificaat (Base64)**, en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. Selecteer **Opslaan**.

    ![De knop Opslaan](./media/bamboo-hr-tutorial/tutorial_general_400.png)

6. Onder **BambooHR configuratie**, selecteer **configureren BambooHR** openen de **eenmalige aanmelding configureren** venster. In de **Naslaggids** sectie, Kopieer de **SAML Single Sign-On Service-URL** voor later gebruik.

    ![BambooHR configuratie](./media/bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

7. In een nieuw venster aanmelden bij uw bedrijf BambooHR site als beheerder.

8. Ga als volgt te werk op de startpagina:
   
    ![De pagina BambooHR Single Sign-On](./media/bamboo-hr-tutorial/ic796691.png "eenmalige aanmelding")   

    a. Selecteer **Apps**.
   
    b. In de **Apps** deelvenster **Single Sign-On**.
   
    c. Selecteer **SAML Single Sign-On**.

9. In de **SAML Single Sign-On** deelvenster de volgende handelingen uit:
   
    ![Het deelvenster SAML Single Sign-On](./media/bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")
   
    a. In de **aanmeldings-Url voor eenmalige aanmelding** vak, plak de **SAML Single Sign-On Service-URL** die u vanuit de Azure-portal in stap 6 hebt gekopieerd.
      
    b. Het base-64 gecodeerde certificaat dat u hebt gedownload vanuit de Azure-portal openen in Kladblok, Kopieer de inhoud ervan en plakt u deze in de **X.509-certificaat** vak.
   
    c. Selecteer **Opslaan**.

> [!TIP]
> Terwijl u de app instelt, vindt u een beknopte versie van deze instructies in de [Azure-portal](https://portal.azure.com). Na het toevoegen van de app vanuit de **Active Directory** > **bedrijfstoepassingen** sectie gewoon de **Single Sign-On** tabblad en vervolgens naar de Embedded-documentatie via de **configuratie** sectie onderaan. Zie voor informatie [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker Britta Simon aangeroepen in de Azure-portal.

   ![Azure AD-testgebruiker Britta Simon maken][100]

Als u wilt een testgebruiker maken in Azure AD, het volgende doen:

1. Selecteer in de Azure-portal in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-knop](./media/bamboo-hr-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/bamboo-hr-tutorial/create_aaduser_02.png)

3. Aan de bovenkant van de **alle gebruikers** deelvenster **toevoegen**.

    ![De knop toevoegen](./media/bamboo-hr-tutorial/create_aaduser_03.png)

4. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster gebruiker](./media/bamboo-hr-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-bamboohr-test-user"></a>Een testgebruiker BambooHR maken

Om Azure AD-gebruikers aan te melden bij BambooHR, handmatig instellen in BambooHR als volgt:

1. Aanmelden bij uw **BambooHR** site als beheerder.

2. Selecteer in de werkbalk boven **instellingen**.
   
    ![De knop instellingen](./media/bamboo-hr-tutorial/IC796694.png "instelling")

3. Selecteer **Overzicht**.

4. Selecteer in het linkerdeelvenster **beveiliging** > **gebruikers**.

5. Typ de gebruikersnaam, wachtwoord en e-mailadres van de geldige Azure AD-account dat u wilt instellen.

6. Selecteer **Opslaan**.
        
>[!NOTE]
>Als u Azure AD-gebruikersaccounts instelt, kunt u ook BambooHR hulpmiddelen voor het account maken van gebruiker of -API's gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

Gebruiker Britta Simon BambooHR toegang verlenen voor het gebruik van Azure eenmalige aanmelding inschakelen.

![Toewijzen van de gebruikersrol][200] 

Gebruiker Britta Simon om aan te wijzen BambooHR, het volgende doen:

1. In de Azure portal, opent u de weergave van toepassingen, Ga naar de directoryweergave en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. In de **bedrijfstoepassingen** selecteert **BambooHR**.

    ![De koppeling BambooHR in de lijst met Enterprise-toepassingen](./media/bamboo-hr-tutorial/tutorial_bamboohr_app.png)  

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop en klikt u op de **toevoegen toewijzing** deelvenster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** venster in de **gebruikers** selecteert **Britta Simon**.

6. Selecteer de **Selecteer** knop.

7. In de **toevoegen toewijzing** Selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

De configuratie van de Azure AD SSO testen met behulp van het toegangsvenster.

Wanneer u selecteert de **BambooHR** tegel in het deelvenster toegang u moet ophalen automatisch aangemeld bij uw toepassing BambooHR.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/bamboo-hr-tutorial/tutorial_general_203.png

