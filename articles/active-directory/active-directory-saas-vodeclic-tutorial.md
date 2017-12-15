---
title: 'Zelfstudie: Azure Active Directory-integratie met Vodeclic | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Vodeclic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d77a0f53-e3a3-445e-ab3e-119cef6e2e1d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: jeedes
ms.openlocfilehash: 940c7bb5040fb91a03b01dc43ee07d52e3d4e63b
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="tutorial-azure-active-directory-integration-with-vodeclic"></a>Zelfstudie: Azure Active Directory-integratie met Vodeclic

In deze zelfstudie leert u hoe Vodeclic integreren met Azure Active Directory (Azure AD).

Vodeclic integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Vodeclic heeft.
- U kunt uw gebruikers te automatisch ophalen ondertekend op Vodeclic (eenmalige aanmelding of SSO) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Vodeclic, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement Vodeclic SSO-ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productieomgeving geen tenzij dit noodzakelijk is.
- Als u een proefabonnement Azure AD-omgeving geen [ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Vodeclic uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-vodeclic-from-the-gallery"></a>Vodeclic uit de galerie toevoegen
Voor het configureren van de integratie van Vodeclic in Azure AD, moet u Vodeclic uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Vodeclic uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Vodeclic**. Selecteer **Vodeclic** vanuit het deelvenster resultaten en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Vodeclic in de lijst met resultaten](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Vodeclic op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de gebruiker equivalent in Vodeclic is een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de betreffende gebruiker in Vodeclic vast te stellen.

Geef de waarde in Vodeclic, **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. U kunt de koppeling tussen de twee gebruikers nu hebt vastgesteld.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met Vodeclic, voert u de volgende elementen:

1. [Eenmalige aanmelding Azure AD configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker Vodeclic](#create-a-vodeclic-test-user) Vodeclic die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Vodeclic configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Vodeclic, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Vodeclic** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** dialoogvenster onder **eenmalige aanmelding modus**, selecteer **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_samlbase.png)

3. Als u wilt configureren van de toepassing in **IDP** modus gestart in de **Vodeclic domein en de URL's** sectie, voert de volgende stappen uit:

    ![Vodeclic domein en één URL's aanmelding informatie](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url.png)

    a. In de **id** vak een URL met het volgende patroon volgen:`https://<companyname>.lms.vodeclic.net/auth/saml`

    b. In de **antwoord-URL** vak een URL met het volgende patroon volgen:`https://<companyname>.lms.vodeclic.net/auth/saml/callback`

4. Als u wilt configureren van de toepassing in **SP** geïnitieerd modus, selecteer de **weergeven geavanceerde instellingen voor URL** selectievakje en de volgende stap uitvoeren:

    ![Vodeclic domein en één URL's aanmelding informatie](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url1.png)

    In de **aanmeldings-URL** vak een URL met het volgende patroon volgen:`https://<companyname>.lms.vodeclic.net/auth/saml`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke-id en antwoord-URL en de aanmeldings-URL. Neem contact op met de [Vodeclic Client ondersteuningsteam](mailto:hotline@vodeclic.com) ophalen van deze waarden.

5. In de **SAML-certificaat voor ondertekening van** sectie **Metadata XML**. Sla het metagegevensbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_certificate.png) 

6. Selecteer **Opslaan**.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-vodeclic-tutorial/tutorial_general_400.png)
    
7. Eenmalige aanmelding configureren op de **Vodeclic** zijde, verzenden de gedownloade **Metadata XML** naar de [Vodeclic ondersteuningsteam](mailto:hotline@vodeclic.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie op [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure-portal in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**. Selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** dialoogvenster, **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-vodeclic-test-user"></a>Een testgebruiker Vodeclic maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Vodeclic maken. Werken met de [Vodeclic ondersteuningsteam](mailto:hotline@vodeclic.com) de gebruikers van het platform Vodeclic toevoegen. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

> [!NOTE]
> Volgens de toepassingsvereisten van moet u mogelijk uw machine goedgekeurde lijst ophalen. Dat gebeurt, moet u uw openbare IP-adres met de [Vodeclic ondersteuningsteam](mailto:hotline@vodeclic.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Vodeclic.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Vodeclic, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure portal en gaat u naar de directoryweergave. Ga vervolgens naar **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Vodeclic**.

    ![De koppeling Vodeclic in de lijst met toepassingen](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_app.png)  

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** dialoogvenster, **Britta Simon** in de **gebruikers** lijst.

6. In de **gebruikers en groepen** selecteert u de **Selecteer** knop.

7. In de **toevoegen toewijzing** selecteert u de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u de tegel Vodeclic in het deelvenster toegang selecteert, krijgen u automatisch aangemeld bij uw toepassing Vodeclic.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_203.png

