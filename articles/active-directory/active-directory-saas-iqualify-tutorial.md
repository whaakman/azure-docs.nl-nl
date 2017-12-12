---
title: 'Zelfstudie: Azure Active Directory-integratie met iQualify LMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en iQualify LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 8a3caaff-dd8d-4afd-badf-a0fd60db3d2c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: jeedes
ms.openlocfilehash: 6f8a7b7fd155a6ad0df7cb1f9026b4acca2401cb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-iqualify-lms"></a>Zelfstudie: Azure Active Directory-integratie met iQualify LMS

In deze zelfstudie leert u hoe iQualify LMS integreren met Azure Active Directory (Azure AD).

IQualify LMS integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot iQualify LMS heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij iQualify LMS (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iQualify LMS, moet u de volgende items:

- Een Azure AD-abonnement
- Een iQualify LMS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. IQualify LMS uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-iqualify-lms-from-the-gallery"></a>IQualify LMS uit de galerie toevoegen
Voor het configureren van de integratie van iQualify LMS in Azure AD, moet u iQualify LMS uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iQualify LMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **iQualify LMS**, selecteer **iQualify LMS** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![iQualify LMS in de lijst met resultaten](./media/active-directory-saas-iqualify-tutorial/tutorial_iqualify_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met iQualify die LMS op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de equivalente gebruiker in iQualify LMS aan een gebruiker in Azure AD is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in iQualify LMS moet tot stand worden gebracht.

Wijs in iQualify LMS, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met iQualify LMS, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maakt u een gebruiker iQualify LMS test](#create-an-iqualify-lms-test-user)**  - hebben een equivalent van Britta Simon in iQualify LMS die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw iQualify LMS-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met iQualify LMS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **iQualify LMS** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-iqualify-tutorial/tutorial_iqualify_samlbase.png)

3. Op de **iQualify LMS domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de IDP geïnitieerd modus:

    ![informatie iQualify LMS domein en de URL's eenmalige aanmelding](./media/active-directory-saas-iqualify-tutorial/tutorial_iqualify_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: 
    | |
    |--|--|
    | Productie-omgeving:`https://<yourorg>.iqualify.com/`|
    | Testomgeving:`https://<yourorg>.iqualify.io`|
    
    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: 
    | |
    |--|--|
    | Productie-omgeving:`https://<yourorg>.iqualify.com/auth/saml2/callback` |
    | Testomgeving:`https://<yourorg>.iqualify.io/auth/saml2/callback` |

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![informatie iQualify LMS domein en de URL's eenmalige aanmelding](./media/active-directory-saas-iqualify-tutorial/tutorial_iqualify_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:
    | |
    |--|--|
    | Productie-omgeving:`https://<yourorg>.iqualify.com/login` |
    | Testomgeving:`https://<yourorg>.iqualify.io/login` |
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [iQualify LMS Client ondersteuningsteam](https://www.iqualify.com) ophalen van deze waarden. 

5. De toepassing van de LMS iQualify verwacht de asserties Security Assertion Markup Language (SAML) moet worden weergegeven in een specifieke indeling. De claims configureren en beheren van de waarden van de kenmerken in de **gebruikerskenmerken** sectie van de iQualify Integratiepagina toepassingen, zoals wordt weergegeven in de volgende schermafbeelding:
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-iqualify-tutorial/atb.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster voert de volgende stappen uit voor elke rij in de onderstaande tabel wordt weergegeven:
    
    | Kenmerknaam | Waarde kenmerk |
    | --- | --- |    
    | e-mailen | User.userPrincipalName |
    | Voornaam | User.givenName |
    | Achternaam | User.surname |
    | person_id | 'het kenmerk' | 

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-iqualify-tutorial/atb2.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-iqualify-tutorial/atb3.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **Ok**

    e. Herhaal stap "a" via "d" voor de volgende rijen. 

    > [!Note]
    > Herhaalt u stap "a" via "d" voor de **person_id** kenmerk **optioneel**

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base 64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-iqualify-tutorial/tutorial_iqualify_certificate.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-iqualify-tutorial/tutorial_general_400.png)
    
9. Op de **iQualify LMS configuratie** sectie, klikt u op **iQualify LMS configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's, en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![iQualify LMS configuratie](./media/active-directory-saas-iqualify-tutorial/tutorial_iqualify_configure.png) 

10.  Open een nieuw browservenster en meld u aan uw omgeving iQualify als beheerder.

11. Nadat u bent aangemeld, klikt u op uw avatar in de rechterbovenhoek en klik vervolgens op **'Instellingen Account'.**

    ![Accountinstellingen](./media/active-directory-saas-iqualify-tutorial/setting1.png) 
12. In het gebied van account instellingen op in het lintmenu aan de linkerkant en klik op **"INTEGRATIES."**
    
    ![INTEGRATIES](./media/active-directory-saas-iqualify-tutorial/setting2.png)

13. Klik onder INTEGRATIES, op de **SAML** pictogram.

    ![SAML-pictogram](./media/active-directory-saas-iqualify-tutorial/setting3.png)

14. In de **SAML-verificatie-instellingen** dialoogvenster vak, voert u de volgende stappen uit:

    ![SAML-verificatie-instellingen](./media/active-directory-saas-iqualify-tutorial/setting4.png)

    a. In de **SAML SINGLE SIGN-ON SERVICE-URL** vak, plak de **SAML Sign‑On Service-URL met eenmalige** waarde opgehaald uit het Configuratievenster van Azure AD-toepassing.
    
    b. In de **SAML AFMELDING URL** vak, plak de **Sign‑Out URL** waarde opgehaald uit het Configuratievenster van Azure AD-toepassing.
    
    c. Open het gedownloade certificaat-bestand in Kladblok, Kopieer de inhoud en plak deze in de **openbaar certificaat** vak.
    
    d. In **aanmelding KNOPLABEL** Voer de naam voor de knop moet worden weergegeven op de aanmeldingspagina.
    
    e. Klik op **OPSLAAN**.

    f. Klik op **UPDATE**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-iqualify-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-iqualify-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-iqualify-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-iqualify-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-iqualify-lms-test-user"></a>Maken van een gebruiker iQualify LMS testen

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in iQualify. iQualify LMS ondersteunt just‑in‑time gebruikersinrichting, die standaard is ingeschakeld.

Er is geen actie-item voor u in deze sectie. Als een gebruiker in iQualify nog niet bestaat, wordt een nieuw gemaakt wanneer u probeert te krijgen tot iQualify LMS.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot iQualify LMS.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen iQualify LMS, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **iQualify LMS**.

    ![De iQualify LMS-koppeling in de lijst met toepassingen](./media/active-directory-saas-iqualify-tutorial/tutorial_iqualify_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de iQualify LMS tegel in het deelvenster toegang, krijgt u de aanmeldingspagina van uw iQualify LMS-toepassing. 

   ![aanmeldingspagina](./media/active-directory-saas-iqualify-tutorial/login.png) 

Klik op **aanmelden met Azure AD** knop en u moet ophalen automatisch aangemeld bij uw iQualify LMS-toepassing.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-iqualify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-iqualify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-iqualify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-iqualify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-iqualify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-iqualify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-iqualify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-iqualify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-iqualify-tutorial/tutorial_general_203.png

