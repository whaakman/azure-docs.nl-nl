---
title: 'Zelfstudie: Azure Active Directory-integratie met QPrism | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en QPrism.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 72ab75ba-132b-4f83-a34b-d28b81b6d7bc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2017
ms.author: jeedes
ms.openlocfilehash: 1f697b95074e0fc9dbb3e8c7800e69f8ece9e0b3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qprism"></a>Zelfstudie: Azure Active Directory-integratie met QPrism

In deze zelfstudie leert u hoe QPrism integreren met Azure Active Directory (Azure AD).

QPrism integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot QPrism heeft.
- U kunt uw gebruikers te automatisch ophalen ondertekend op QPrism (eenmalige aanmelding) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één locatie beheren: de Azure-portal.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met QPrism, moet u de volgende items:

- Een Azure AD-abonnement
- Een QPrism eenmalige aanmelding ingeschakeld abonnement

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. QPrism uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-qprism-from-the-gallery"></a>QPrism uit de galerie toevoegen
Voor het configureren van de integratie van QPrism in Azure AD, moet u QPrism uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**QPrism uit de galerie toevoegen:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Selecteer om een nieuwe toepassing toe aan de bovenkant van het dialoogvenster **nieuwe toepassing**.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **QPrism**, en selecteer **QPrism** van resultaat deelvenster. Klik vervolgens op **toevoegen** de toepassing toevoegen.

    ![QPrism in de lijst met resultaten](./media/active-directory-saas-qprism-tutorial/tutorial_qprism_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met QPrism, op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de gebruiker equivalent in QPrism is een gebruiker in Azure AD. Met andere woorden, moet er een gekoppelde relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in QPrism.

Als u wilt deze relatie in QPrism tot stand brengen, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam**.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met QPrism, voert u de volgende elementen:

1. [Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker QPrism](#create-a-qprism-test-user) QPrism die gekoppeld is aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie kunt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing QPrism configureren.

1. In de Azure-portal op de **QPrism** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-qprism-tutorial/tutorial_qprism_samlbase.png)

3. In de **QPrism domein en de URL's** sectie, het volgende doen:

    ![URL's en QPrism domein eenmalige aanmelding informatie](./media/active-directory-saas-qprism-tutorial/tutorial_qprism_url.png)

    a. In de **aanmeldings-URL** tekstvak, typt u een URL die gebruikmaakt van het volgende patroon volgen:`https://<customer domain>.qmyzone.com/login`

    b. In de **id** tekstvak, typt u een URL die gebruikmaakt van het volgende patroon volgen:`https://<customer domain>.qmyzone.com/metadata.php`
         
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke identificatie en eenmalige aanmelding URL. Neem contact op met [QPrism Client ondersteuningsteam](mailto:qsupport-ce@quatrro.com) ophalen van deze waarden. 

4. Voor het genereren van de **metagegevens** -URL, het volgende doen:

    a. Selecteer **App registraties**.
    
    ![Eenmalige aanmelding app registraties configureren](./media/active-directory-saas-qprism-tutorial/tutorial_qprism_appregistrations.png)
   
    b. Selecteer **eindpunten** openen **eindpunten** in het dialoogvenster.  
    
    ![Eindpunt voor eenmalige aanmelding configureren](./media/active-directory-saas-qprism-tutorial/tutorial_qprism_endpointicon.png)

    c. Selecteer de knop kopiëren om te kopiëren de **DOCUMENT met federatieve metagegevens** -URL, en plak deze in Kladblok.
    
    ![Eindpunt voor eenmalige aanmelding configureren](./media/active-directory-saas-qprism-tutorial/tutorial_qprism_endpoint.png)
     
    d. Nu gaat u naar de eigenschappenpagina van **QPrism**, en kopieer de **toepassings-ID** met behulp van **kopie**. Plak deze in Kladblok.
 
    ![Eenmalige aanmelding toepassings-ID configureren](./media/active-directory-saas-qprism-tutorial/tutorial_qprism_appid.png)

    e. Genereren van de **metagegevens-URL** met behulp van het volgende patroon volgen:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>` 

5. Selecteer **Opslaan**.

    ![Configureren van eenmalige aanmelding knop Opslaan](./media/active-directory-saas-qprism-tutorial/tutorial_general_400.png)
    
6. Eenmalige aanmelding configureren op de **QPrism** zijde, verzenden de **metagegevens-URL** naar de [QPrism ondersteuningsteam](mailto:qsupport-ce@quatrro.com). Ze ervoor te zorgen dat de SAML eenmalige aanmelding verbinding aan beide zijden juist is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt. Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Een testgebruiker maken in Azure AD:**

1. Selecteer in de Azure-portal in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-knop](./media/active-directory-saas-qprism-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-qprism-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster aan de bovenkant van de **alle gebruikers** dialoogvenster, **toevoegen**.

    ![De knop toevoegen](./media/active-directory-saas-qprism-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster de volgende handelingen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-qprism-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-qprism-test-user"></a>Een testgebruiker QPrism maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in QPrism maken. Werken met de [QPrism ondersteuningsteam](mailto:qsupport-ce@quatrro.com) gebruikers van het platform QPrism toevoegen. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan QPrism.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon toewijzen aan QPrism:**

1. Open de toepassingen in de Azure portal en navigeer vervolgens naar de directoryweergave. Ga naar **bedrijfstoepassingen**, en selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **QPrism**.

    ![De koppeling QPrism in de lijst met toepassingen](./media/active-directory-saas-qprism-tutorial/tutorial_qprism_app.png)  

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer **Toevoegen**. Klik vervolgens onder **toevoegen toewijzing**, selecteer **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de **gebruikers** lijst.

6. Op de **gebruikers en groepen** dialoogvenster, **Selecteer**.

7. Onder **toevoegen toewijzing**, selecteer **toewijzen**.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

In het deelvenster toegang wanneer u de tegel QPrism selecteert moet u ophalen automatisch aangemeld voor uw toepassing QPrism.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-qprism-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qprism-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qprism-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qprism-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qprism-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qprism-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qprism-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qprism-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qprism-tutorial/tutorial_general_203.png

