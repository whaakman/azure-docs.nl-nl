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
ms.openlocfilehash: 77625296797ec8ed8364e7d8bff3e5a15b4b74b5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048035"
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>Zelfstudie: Azure Active Directory-integratie met BambooHR

In deze zelfstudie leert u hoe u BambooHR integreren met Azure Active Directory (Azure AD).

BambooHR integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BambooHR heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij BambooHR met behulp van eenmalige aanmelding (SSO) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BambooHR, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement BambooHR SSO is ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, raden wij aan dat u niet een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat geeft een overzicht van deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BambooHR uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-bamboohr-from-the-gallery"></a>BambooHR uit de galerie toevoegen
Voor het configureren van de integratie van BambooHR in Azure AD toevoegen BambooHR uit de galerie aan de lijst met beheerde SaaS-apps door het volgende te doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het deelvenster met Enterprise-toepassingen][2]
    
3. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing**.

    ![De 'nieuwe toepassing"knop][3]

4. Typ in het zoekvak **BambooHR**. Selecteer in de lijst met resultaten **BambooHR**, en selecteer vervolgens **toevoegen**.

    ![BambooHR in de lijst met resultaten](./media/bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen van Azure AD-eenmalige aanmelding met BambooHR met behulp van de testgebruiker "Britta Simon."

Azure AD moet weten wat de gebruiker equivalent is in BambooHR voor eenmalige aanmelding om te werken. Met andere woorden, moet u een koppeling relatie tussen de Azure AD-gebruiker en de gerelateerde gebruiker maken in BambooHR.

Toewijzen voor het maken van de relatie van de koppeling in BambooHR, de Azure AD **gebruikersnaam** waarde als de BambooHR **gebruikersnaam** waarde.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met BambooHR, voltooi de bouwstenen die u in de volgende vijf secties.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD-eenmalige aanmelding inschakelen in Azure portal en het configureren van eenmalige aanmelding in uw toepassing BambooHR door het volgende te doen:

1. In de Azure-portal op de **BambooHR** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** venster in de **modus** vervolgkeuzelijst, selecteer **SAML gebaseerde aanmelding**.
 
    ![Venster voor eenmalige aanmelding](./media/bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. Onder **BambooHR domein en URL's**, doet u het volgende:

    ![De sectie BambooHR domein en URL 's](./media/bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    a. In de **aanmeldings-URL** vak, typ een URL in de volgende indeling: `https://<company>.bamboohr.com`.

    b. In de **id** typt u een waarde: `BambooHR-SAML`.

    > [!NOTE] 
    > De **aanmeldings-URL** waarde is niet echt. Bijwerken met de werkelijke aanmeldings-URL. Als u wilt de waarde niet ophalen, neem contact op met de [BambooHR client ondersteuningsteam](https://www.bamboohr.com/contact.php). 
 
4. Onder **SAML-handtekeningcertificaat**, selecteer **certificaat (Base64)**, en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. Selecteer **Opslaan**.

    ![De knop Opslaan](./media/bamboo-hr-tutorial/tutorial_general_400.png)

6. Onder **BambooHR configuratie**, selecteer **configureren BambooHR** openen de **aanmelding configureren** venster. In de **Naslaggids** sectie, Kopieer de **Single Sign-On Service URL voor SAML** voor later gebruik.

    ![BambooHR configuratie](./media/bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

7. In een nieuw venster aanmelden bij uw bedrijf BambooHR site als beheerder.

8. Op de startpagina, door het volgende te doen:
   
    ![De pagina BambooHR Single Sign-On](./media/bamboo-hr-tutorial/ic796691.png "Single Sign-On")   

    a. Selecteer **Apps**.
   
    b. In de **Apps** venster **Single Sign-On**.
   
    c. Selecteer **SAML Single Sign-On**.

9. In de **SAML Single Sign-On** in het deelvenster de volgende handelingen uit:
   
    ![Het deelvenster SAML Single Sign-On](./media/bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")
   
    a. In de **aanmeldings-Url voor eenmalige aanmelding** vak, plak de **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal in stap 6.
      
    b. In Kladblok, opent u het base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal, Kopieer de inhoud en plak deze in de **X.509-certificaat** vak.
   
    c. Selecteer **Opslaan**.

> [!TIP]
> Tijdens het instellen van de app, vindt u een beknopte versie van deze instructies in de [Azure-portal](https://portal.azure.com). Nadat u de app vanuit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad, en vervolgens toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. Zie voor meer informatie, [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker Britta Simon aangeroepen in de Azure-portal.

   ![Azure AD-testgebruiker Britta Simon maken][100]

Als u wilt een testgebruiker maken in Azure AD, het volgende doen:

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-knop](./media/bamboo-hr-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/bamboo-hr-tutorial/create_aaduser_02.png)

3. Aan de bovenkant van de **alle gebruikers** venster **toevoegen**.

    ![De knop toevoegen](./media/bamboo-hr-tutorial/create_aaduser_03.png)

4. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster van de gebruiker](./media/bamboo-hr-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-bamboohr-test-user"></a>Maak een testgebruiker BambooHR

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij BambooHR, handmatig instellen in BambooHR door het volgende te doen:

1. Aanmelden bij uw **BambooHR** site als beheerder.

2. Selecteer in de werkbalk boven **instellingen**.
   
    ![De knop instellingen](./media/bamboo-hr-tutorial/IC796694.png "instelling")

3. Selecteer **Overzicht**.

4. Selecteer in het linkerdeelvenster **Security** > **gebruikers**.

5. Typ de gebruikersnaam, wachtwoord en e-mailadres van de geldige Azure AD-account dat u wilt instellen.

6. Selecteer **Opslaan**.
        
>[!NOTE]
>Als u Azure AD-gebruikersaccounts instelt, kunt u ook BambooHR gebruiker accountaanmaak hulpprogramma's of API's gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

Gebruiker Britta Simon voor de Azure-eenmalige aanmelding toegang verlenen tot BambooHR inschakelen.

![De de gebruikersrol toewijzen][200] 

Gebruiker Britta Simon om aan te wijzen BambooHR, het volgende doen:

1. In de Azure-portal, opent u de weergave van toepassingen, gaat u naar de directoryweergave en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. In de **bedrijfstoepassingen** in de lijst met **BambooHR**.

    ![De koppeling BambooHR in de lijst met Enterprise-toepassingen](./media/bamboo-hr-tutorial/tutorial_bamboohr_app.png)  

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop en klik vervolgens op de **toevoegen toewijzing** venster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** venster in de **gebruikers** in de lijst met **Britta Simon**.

6. Selecteer de **Selecteer** knop.

7. In de **toevoegen toewijzing** venster de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

De configuratie van de Azure AD-eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u selecteert de **BambooHR** tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing BambooHR.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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

