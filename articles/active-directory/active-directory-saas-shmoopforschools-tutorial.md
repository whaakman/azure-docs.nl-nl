---
title: 'Zelfstudie: Azure Active Directory-integratie met Shmoop voor scholen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Shmoop voor scholen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1d75560a-55b3-42e9-bda1-92b01c572d8e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jeedes
ms.openlocfilehash: 48db70834f96adbb7097457caca8489ea1a57da5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-shmoop-for-schools"></a>Zelfstudie: Azure Active Directory-integratie met Shmoop voor scholen

In deze zelfstudie leert u hoe Shmoop voor scholen integreren met Azure Active Directory (Azure AD).

Shmoop voor scholen integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Shmoop voor scholen heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Shmoop voor scholen met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Shmoop voor scholen, moet u de volgende items:

- Een Azure AD-abonnement
- Een Shmoop voor scholen eenmalige aanmelding op ingeschakeld-abonnement

> [!NOTE]
> Gebruik een productie-omgeving voor het testen van de stappen in deze zelfstudie aanbevolen niet.

Test de stappen in deze zelfstudie, raden we aan:

- Met behulp van uw productie-evironment alleen als het nodig is.
- Ophalen van een [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/) als u nog een proefabonnement Azure AD-omgeving hebt.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Shmoop voor scholen uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-shmoop-for-schools-from-the-gallery"></a>Shmoop voor scholen uit de galerie toevoegen
Voor het configureren van de integratie van Shmoop voor scholen in Azure AD, moet u Shmoop voor scholen uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen voor scholen Shmoop uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Shmoop voor scholen**. Selecteer **Shmoop voor scholen** uit de resultaten, selecteer de **toevoegen** om toe te voegen van de toepassing.

    ![Shmoop voor scholen in de lijst met resultaten](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Shmoop voor scholen op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de gebruiker equivalent in Shmoop voor scholen is een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de betreffende gebruiker in Shmoop voor scholen tot stand brengen.

In Shmoop voor scholen, geven de **gebruikersnaam** waarde dezelfde waarde voor de **gebruikersnaam** in Azure AD. U hebt nu de relatie koppeling ingesteld.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Shmoop voor scholen, voert u de volgende elementen:

1. [Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker Shmoop voor scholen](#create-a-shmoop-for-schools-test-user) Shmoop voor scholen die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Shmoop voor scholen.

**Voor het configureren van Azure AD eenmalige aanmelding met Shmoop voor scholen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Shmoop voor scholen** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** in de vervolgkeuzelijst in het dialoogvenster **modus voor één aanmelding**, selecteer **op basis van SAML aanmelding**.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_samlbase.png)

3. In de **Shmoop voor scholen domein en URL's** sectie, voert de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_url.png)

    a. In de **aanmeldings-URL** vak een URL met het volgende patroon volgen:`https://schools.shmoop.com/public-api/saml2/start/<uniqueid>`

    b. In de **id** vak een URL met het volgende patroon volgen:`https://schools.shmoop.com/<uniqueid>`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de id. Neem contact op met de [Shmoop voor scholen Client ondersteuningsteam](mailto:support@shmoop.com) ophalen van deze waarden. 
 
4. De toepassing Shmoop voor scholen verwacht de SAML-asserties in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerken** sectie op de pagina van de integratie van toepassingen. De volgende schermafbeelding ziet u hoe de asserties configureren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute.png)

    > [!NOTE]
    > Shmoop voor School ondersteunt twee rollen voor gebruikers: **leraar** en **studenten**. Deze rollen in Azure AD zo instellen dat gebruikers de juiste rollen kunnen worden toegewezen. Zie voor informatie over het configureren van rollen in Azure AD, [toegangsbeheer op basis van rollen in de cloud-toepassingen die gebruikmaken van Azure AD](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/).
    
5. In de **gebruikerskenmerken** sectie het **eenmalige aanmelding** dialoogvenster het SAML-token kenmerk configureren zoals wordt weergegeven in de vorige afbeelding.  Vervolgens voert u de volgende stappen uit:

    | Naam van kenmerk | De waarde van kenmerk |
    | -------------- | --------------- |
    | rol           | User.assignedroles |

    a. Openen van de **kenmerk toevoegen** dialoogvenster, **toevoegen kenmerk**.
    
    ![Eenmalige aanmelding configureren ](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** typt u de naam van het kenmerk dat wordt weergegeven voor die rij.
    
    c. Van de **waarde** , selecteert u de waarde van het kenmerk dat wordt weergegeven voor die rij.

    d. Laat de **Namespace** vak leeg.
    
    e. Selecteer **Ok**.

6. Selecteer de knop **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_400.png)

7. Voor het genereren van de **metagegevens** -URL, de volgende stappen uit:

    a. Selecteer **App registraties**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_appregistrations.png)
   
    b. Openen van de **eindpunten** dialoogvenster, **eindpunten**.  
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_endpointicon.png)

    c. Selecteer de knop kopiëren om te kopiëren de **DOCUMENT met federatieve metagegevens** URL en plak deze in Kladblok.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_endpoint.png)
     
    d. Ga naar de eigenschappenpagina van **Shmoop voor scholen**. Kopieer de **toepassings-ID** met behulp van de **kopie** knop. Plak deze in Kladblok.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_appid.png)

    e. Genereren van de **metagegevens-URL** met behulp van het volgende patroon volgen: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`.   

8. Eenmalige aanmelding configureren op de **Shmoop voor scholen** kant die u wilt verzenden de **metagegevens-URL** naar de [Shmoop voor scholen ondersteuningsteam](mailto:support@shmoop.com).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie op [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker Britta Simon aangeroepen in de Azure-portal.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure-portal in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**. Selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** dialoogvenster, **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-shmoopforschools-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-shmoop-for-schools-test-user"></a>Een testgebruiker Shmoop voor scholen maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Shmoop voor scholen genoemd. Shmoop voor scholen ondersteunt just-in-time-inrichting, die standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een nieuwe gebruiker nog niet bestaat, wordt deze is gemaakt tijdens de poging tot toegang tot Shmoop voor scholen.

>[!NOTE]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [Shmoop voor scholen ondersteuningsteam](mailto:support@shmoop.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan Shmoop voor scholen.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Shmoop voor scholen, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure portal. Ga vervolgens naar **bedrijfstoepassingen** in de directoryweergave.  Selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Shmoop voor scholen**.

    ![De koppeling Shmoop voor scholen in de lijst met toepassingen](./media/active-directory-saas-shmoopforschools-tutorial/tutorial_shmoopforschools_app.png)  

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Klik in de **toevoegen toewijzing** dialoogvenster, **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de gebruikerslijst.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop. 

7. In de **toevoegen toewijzing** selecteert u de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u selecteert de **Shmoop voor scholen** tegel in het deelvenster toegang u moet ophalen automatisch aangemeld bij uw toepassing Shmoop voor scholen.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies voor het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shmoopforschools-tutorial/tutorial_general_203.png

