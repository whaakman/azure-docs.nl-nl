---
title: 'Zelfstudie: Azure Active Directory-integratie met Envi MMIS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Envi MMIS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: 0d338f7f56e668c25378403e88a6532b8b8573c5
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34344266"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Zelfstudie: Azure Active Directory-integratie met Envi MMIS

In deze zelfstudie leert u hoe Envi MMIS integreren met Azure Active Directory (Azure AD).

Envi MMIS integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Envi MMIS heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Envi MMIS (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Envi MMIS, moet u de volgende items:

- Een Azure AD-abonnement
- Een Envi MMIS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Envi MMIS uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-envi-mmis-from-the-gallery"></a>Envi MMIS uit de galerie toevoegen
Voor het configureren van de integratie van Envi MMIS in Azure AD, moet u Envi MMIS uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Envi MMIS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Envi MMIS**, selecteer **Envi MMIS** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Envi MMIS in de lijst met resultaten](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie maakt u configureert en test eenmalige aanmelding Azure AD met Envi MMIS op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Envi MMIS is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Envi MMIS tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Envi MMIS, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Envi MMIS](#create-an-envi-mmis-test-user)**  - Envi MMIS die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Envi MMIS configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Envi MMIS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Envi MMIS** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_samlbase.png)

3. Op de **landen MMIS domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en landen MMIS domein eenmalige aanmelding informatie](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://www.<CUSTOMER DOMAIN>.com/Account`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en landen MMIS domein eenmalige aanmelding informatie](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://www.<CUSTOMER DOMAIN>.com/Account`
     
    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [landen MMIS Client ondersteuningsteam](mailto:support@ioscorp.com) ophalen van deze waarden.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-envimmis-tutorial/tutorial_general_400.png)

7. In een ander browservenster, meld u bij uw site Envi MMIS als beheerder.

8. Klik op **mijn domein** tabblad.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-envimmis-tutorial/configure1.png)

9. Klik op **Bewerken**.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-envimmis-tutorial/configure2.png)

10. Selecteer **externe verificatie gebruiken** selectievakje in en selecteer vervolgens **HTTP-omleiding** van de **verificatietype** vervolgkeuzelijst.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-envimmis-tutorial/configure3.png)

11. Selecteer **Resources** tabblad en klik vervolgens op **metagegevens uploaden**.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-envimmis-tutorial/configure4.png)

12. In de **metagegevens uploaden** pop-up van de volgende stappen uitvoeren:

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-envimmis-tutorial/configure5.png)

    a. Selecteer **bestand** optie van de **uploaden van** vervolgkeuzelijst.

    b. Uploaden van het metagegevensbestand van de gedownloade vanuit Azure-portal door het selecteren van de **bestandspictogram kiezen**.

    c. Klik op **OK**.

13. Na het uploaden van het metagegevensbestand van de gedownloade wordt ophalen automatisch de velden ingevuld. Klik op **Update**

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-envimmis-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-envimmis-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-envimmis-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-envimmis-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-envi-mmis-test-user"></a>Een testgebruiker Envi MMIS maken

Om Azure AD-gebruikers zich aanmelden bij Envi MMIS, moeten ze worden ingericht in Envi MMIS.  
In het geval van Envi MMIS is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Envi MMIS site als beheerder.

2. Klik op **gebruikerslijst** tabblad.

    ![Werknemer toevoegen](./media/active-directory-saas-envimmis-tutorial/user1.png)

3. Klik op **gebruiker toevoegen** knop.

    ![Werknemer toevoegen](./media/active-directory-saas-envimmis-tutorial/user2.png)

4. In de **gebruiker toevoegen** sectie, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/active-directory-saas-envimmis-tutorial/user3.png)

    a. In de **gebruikersnaam** textbox type Britta Simon account de gebruikersnaam, zoals **brittasimon@contoso.com**.
    
    b. In de **voornaam** textbox, typ de naam van de eerste van BrittaSimon zoals **Britta**.

    c. In de **achternaam** textbox type de naam van de laatste BrittaSimon zoals **Simon**.

    d. Voer de titel van de gebruiker in de **titel** van het tekstvak.
    
    e. In de **e-mailadres** textbox type Britta Simon account het e-mailadres, zoals **brittasimon@contoso.com**.

    f. In de **SSO-gebruikersnaam** textbox type Britta Simon account de gebruikersnaam, zoals **brittasimon@contoso.com**.

    g. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Envi MMIS.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Envi MMIS, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Envi MMIS**.

    ![De koppeling Envi MMIS in de lijst met toepassingen](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Envi MMIS in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Envi MMIS.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_203.png

