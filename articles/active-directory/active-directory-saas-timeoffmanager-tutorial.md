---
title: 'Zelfstudie: Azure Active Directory-integratie met TimeOffManager | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TimeOffManager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 3f944ffbf704694b293b4b1e5bdb4f2c93ae35a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Zelfstudie: Azure Active Directory-integratie met TimeOffManager

In deze zelfstudie leert u hoe TimeOffManager integreren met Azure Active Directory (Azure AD).

TimeOffManager integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TimeOffManager heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TimeOffManager (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TimeOffManager, moet u de volgende items:

- Een Azure AD-abonnement
- Een TimeOffManager eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TimeOffManager uit de galerie toevoegen
2. Configureren en testen eenmalige aanmelding Azure AD

## <a name="add-timeoffmanager-from-the-gallery"></a>TimeOffManager uit de galerie toevoegen
Voor het configureren van de integratie van TimeOffManager in Azure AD, moet u TimeOffManager uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TimeOffManager uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **TimeOffManager**, selecteer **TimeOffManager** van resultaat deelvenster en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Uit de galerie toevoegen](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie configureert en test eenmalige aanmelding Azure AD met TimeOffManager op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in TimeOffManager is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in TimeOffManager tot stand worden gebracht.

Wijs in TimeOffManager, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TimeOffManager, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker TimeOffManager](#create-a-timeoffmanager-test-user)**  - TimeOffManager die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing TimeOffManager configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met TimeOffManager, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TimeOffManager** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![SAML op basis van eenmalige aanmelding](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. Op de **TimeOffManager domein en de URL's** sectie, voert u het volgende:

     ![Sectie TimeOffManager domein en URL 's](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde bijwerken met de werkelijke antwoord-URL. U krijgt deze waarde van **voor eenmalige aanmelding instellingenpagina** waarmee wordt uitgelegd later in de zelfstudie of neem contact op met [TimeOffManager ondersteuningsteam](http://www.timeoffmanager.com/contact-us.aspx).
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Certificaat voor ondertekening van SAML-sectie](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. Het doel van deze sectie is om een overzicht van gebruikers om te verifiëren TimeOffManger aan hun account in Azure AD dat gebruikmaakt van Federatie op basis van het SAML-protocol in staat te.
    
    Uw toepassing TimeOffManger verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![SAML-token kenmerken](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml-token kenmerken")
    
    | Naam van kenmerk | De waarde van kenmerk |
    | --- | --- |
    | Voornaam |User.givenName |
    | Achternaam |User.surname |
    | E-mail |User.mail |
    
    a.  Voor elke gegevensrij in de bovenstaande tabel, klikt u op **gebruikerskenmerk toevoegen**.
    
    ![SAML-token kenmerken](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml-token kenmerken")
    
    ![SAML-token kenmerken](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml-token kenmerken")
    
    b.  In de **kenmerknaam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c.  In de **kenmerkwaarde** textbox, selecteert u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d.  Klik op **OK**.
    
6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. Op de **TimeOffManager configuratie** sectie, klikt u op **configureren TimeOffManager** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Configuratiesectie TimeOffManager](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. In een ander browservenster, meld u bij uw bedrijf TimeOffManager site als beheerder.

9. Ga naar **Account \> Opties Account \> eenmalige aanmelding instellingen**.
   
   ![Eenmalige aanmelding instellingen](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "eenmalige aanmelding-instellingen")
7. In de **instellingen voor eenmalige aanmelding** sectie, voert u de volgende stappen uit:
   
   ![Eenmalige aanmelding instellingen](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "eenmalige aanmelding-instellingen")
   
   a. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan naar het Klembord en plak het gehele certificaat in **X.509-certificaat** textbox.
   
   b. In **Idp verlener** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.
   
   c. In **IdP eindpunt-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
   
   d. Als **afdwingen SAML**, selecteer **Nee**.
   
   e. Als **gebruikers automatisch maken**, selecteer **Ja**.
   
   f. In **afmelding URL** textbox, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.
   
   g. Klik op **wijzigingen opslaan**.

11. In **voor eenmalige aanmelding instellingen** pagina, Kopieer de waarde van **Assertion Consumer Service-URL** en plak deze in de **antwoord-URL** in het tekstvak onder **TimeOffManager Domein- en URL's** sectie in Azure-portal. 

      ![Eenmalige aanmelding instellingen](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "eenmalige aanmelding-instellingen")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Gebruikers en groepen--> alle gebruikers](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Knop toevoegen](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Dialoogvenster op de gebruikerspagina](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-timeoffmanager-test-user"></a>Een testgebruiker TimeOffManager maken

Om in te schakelen gebruikers van Azure AD aan te melden bij TimeOffManager, moeten ze worden ingericht op TimeOffManager.  

TimeOffManager ondersteunt alleen in de tijd van gebruikersinrichting. Er is geen actie-item voor u.  

De gebruikers worden automatisch toegevoegd tijdens de eerste aanmelding met eenmalige aanmelding op.

>[!NOTE]
>U kunt andere TimeOffManager gebruiker account hulpmiddelen voor het maken of API's die is geleverd door TimeOffManager voor het inrichten van Azure AD-gebruikersaccounts.
> 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan TimeOffManager.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen TimeOffManager, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **TimeOffManager**.

    ![TimeOffManager in lijst met Apps](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel TimeOffManager in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing TimeOffManager. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

