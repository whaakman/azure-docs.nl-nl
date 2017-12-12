---
title: 'Zelfstudie: Azure Active Directory-integratie met Pingboard | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Pingboard.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: jeedes
ms.openlocfilehash: 733eff519f75368fd78824deea56f63940c0b49f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Zelfstudie: Azure Active Directory-integratie met Pingboard

In deze zelfstudie leert u hoe Pingboard integreren met Azure Active Directory (Azure AD).

Pingboard integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Pingboard heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Pingboard (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Pingboard, moet u de volgende items:

- Een Azure AD-abonnement
- Een Pingboard eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Pingboard uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-pingboard-from-the-gallery"></a>Pingboard uit de galerie toevoegen
Voor het configureren van de integratie van Pingboard in Azure AD, moet u Pingboard uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Pingboard uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De Enterprise-toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Pingboard**, selecteer **Pingboard** van resultaat deelvenster en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Pingboard in de lijst met resultaten](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Pingboard op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Pingboard is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Pingboard tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Pingboard.

Om te configureren en testen van Azure AD eenmalige aanmelding met Pingboard, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Pingboard](#create-a-pingboard-test-user)**  - Pingboard die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Pingboard configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Pingboard, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Pingboard** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2.  Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. Op de **Pingboard domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Pingboard domein eenmalige aanmelding informatie IDP](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    a. In de **id** textbox, typ de waarde als:`http://app.pingboard.com/sp`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<entity-id>.pingboard.com/auth/saml/consume`

4. Controleer **weergeven geavanceerde instellingen voor URL**, als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en Pingboard domein eenmalige aanmelding informatie SP](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

     In de **aanmeldings-URL** textbox, typ de URL die met het volgende patroon volgen:`https://<sub-domain>.pingboard.com/sign_in`

    > [!NOTE] 
    > Houd er rekening mee dat deze waarden niet echt zijn. Deze waarden bijwerken met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [Pingboard Client ondersteuningsteam](https://support.pingboard.com/) ophalen van deze waarden.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Pingboard van metagegevens-xml](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. Eenmalige aanmelding configureren Pingboard zijde, open een nieuw browservenster en meld u aan uw Pingboard Account bij. U moet een beheerder Pingboard voor het instellen van eenmalige aanmelding.

8. Selecteer in het bovenste menu,, **Apps > integraties**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  Op de **integraties** pagina, zoek de **'Azure Active Directory'** tegel en klik erop.

    ![Pingboard Single Sign-On-integratie](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. In de modale klikt u op volgende **'Configureren'**

    ![Knop Pingboard-configuratie](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. Op de volgende pagina ziet u dat 'Azure SSO-integratie is ingeschakeld'. Open het gedownloade Metadata XML-bestand in Kladblok en plak de inhoud van **IDP metagegevens**.

    ![Scherm Pingboard SSO-configuratie](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. Het bestand wordt gevalideerd, en als alles juist is eenmalige aanmelding nu worden ingeschakeld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Knop toevoegen](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-pingboard-test-user"></a>Een testgebruiker Pingboard maken

Om in te schakelen gebruikers van Azure AD aan te melden bij Pingboard, moeten ze worden ingericht in Pingboard. Voor Pingboard is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Pingboard site als beheerder.

2. Klik op **'Werknemer toevoegen'** knop op **Directory** pagina.

    ![Werknemer toevoegen](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. Op de **'Werknemer toevoegen'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Personen uitnodigen](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    a. In de **volledige naam** textbox, typ de volledige naam van gebruiker, zoals **Britta Simon**.

    b. In de **e** textbox, typ het e-mailadres van gebruiker, zoals  **brittasimon@contoso.com** .

    c. In de **taaktitel** textbox, typt u de functie van Britta Simon.

    d. In de **locatie** vervolgkeuzelijst, selecteer de locatie van Britta Simon.
    
    e. Klik op **Add**.   

4. Om te bevestigen dat het toevoegen van gebruiker wordt een bevestigingsvenster.
    
    ![Bevestigen](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Pingboard.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Pingboard, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Pingboard**.

    ![De koppeling Pingboard in de lijst met toepassingen](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

Als u op de tegel Pingboard in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Pingboard.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
