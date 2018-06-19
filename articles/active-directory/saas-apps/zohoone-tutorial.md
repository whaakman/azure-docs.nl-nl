---
title: 'Zelfstudie: Azure Active Directory-integratie met één Zoho | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en één Zoho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 89f8df4e14bb93b1594cbb492a29c16b58d18785
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35907308"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Zelfstudie: Azure Active Directory-integratie met één Zoho

In deze zelfstudie leert u hoe een Zoho integreren met Azure Active Directory (Azure AD).

Een Zoho integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot één Zoho heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij een Zoho (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met één Zoho, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zoho een eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van een Zoho uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zoho-one-from-the-gallery"></a>Toevoegen van een Zoho uit de galerie
Voor het configureren van de integratie van één Zoho in Azure AD, moet u één Zoho uit de galerie toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen één Zoho uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Zoho één**, selecteer **Zoho één** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Zoho in de lijst met resultaten](./media/zohoone-tutorial/tutorial_zohoone_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Zoho één, op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in één Zoho is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in één Zoho tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met een Zoho, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een Zoho een testgebruiker](#create-a-zoho-one-test-user)**  - bevatten een equivalent van Britta Simon Zoho één die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Zoho één toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met een Zoho, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zoho één** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/zohoone-tutorial/tutorial_zohoone_samlbase.png)

3. Op de **Zoho één domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![Zoho één domein en URL's van eenmalige aanmelding informatie](./media/zohoone-tutorial/tutorial_zohoone_url.png)

    a. In de **id (entiteit-ID)** textbox, typ een URL: `one.zoho.com`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://accounts.zoho.com/samlresponse/<saml-identifier>`

    c. Controleer **weergeven geavanceerde instellingen voor URL**.

    d. In de **Relay status** textbox, typ een URL:`https://one.zoho.com`

4. Als u wilt configureren van de toepassing in **SP** geïnitieerd modus Voer de volgende stap:

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com`
     
    > [!NOTE] 
    > De voorgaande **antwoord-URL** en **aanmeldings-URL** waarde is geen echte. U kunt de waarde wordt bijgewerkt met de werkelijke antwoord-URL en de aanmeldings-URL die verderop in de zelfstudie wordt beschreven. 

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/zohoone-tutorial/tutorial_zohoone_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/zohoone-tutorial/tutorial_general_400.png)
    
7. Op de **Zoho één configuratie** sectie, klikt u op **configureren met één Zoho** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Zoho één configuratie](./media/zohoone-tutorial/tutorial_zohoone_configure.png) 

8. In een ander browservenster, meld u aan bij uw site met één Zoho bedrijf als beheerder.

9. Op de **organisatie** tabblad, klikt u op **Setup** onder **SAML-verificatie**.

    ![Een Zoho org](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

10. Voer de volgende stappen uit op de pop-pagina:

    ![Een Zoho sig](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. In de **aanmelden URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    b. In de **Sign-out URL** textbox, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal.

    c. Klik op **Bladeren** voor het uploaden van de **certificaat (Base64)** die u hebt gedownload vanuit Azure-portal.

    d. Klik op **Opslaan**.

11. Na het opslaan van de SAML-verificatie-instellingen, kopieert u de **SAML-Identfier** waarde en gebruikt deze waarde in de **antwoord-URL** in de Azure-portal onder **Zoho één domein en URL's** sectie.

    ![Een Zoho saml](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

12. Ga naar de **domeinen** tabblad en klik vervolgens op **domein toevoegen**.

    ![Zoho één domein](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

13. Op de **domein toevoegen** pagina, voert u de volgende stappen uit:

    ![Zoho één toevoegen domein](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. In de **domeinnaam** textbox type domein zoals **contoso.com**.

    b. Klik op **Add**.

    >[!Note]
    >Na het toevoegen van het domein volgen [deze](https://www.zoho.com/one/help/admin-guide/domain-verification.html) stappen om te controleren of uw domein. Nadat het domein verfified is, gebruikt u de domeinnaam van uw in **aanmeldings-URL** in **Zoho één domein en URL's** sectie in Azure-portal.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/zohoone-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/zohoone-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/zohoone-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/zohoone-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-zoho-one-test-user"></a>Een Zoho een testgebruiker maken

Om Azure AD-gebruikers zich aanmelden bij een Zoho, moeten ze worden ingericht in één Zoho. In één Zoho is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan als een beheerder een Zoho bij.

2. Op de **gebruikers** tabblad, klik op **gebruiker logo**.

    ![Zoho één gebruiker](./media/zohoone-tutorial/tutorial_zohoone_users.png)

3. Op de **gebruiker toevoegen** pagina, voert u de volgende stappen uit:

    ![Zoho één toevoegen gebruiker](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. In **naam** tekst en voer de naam van gebruiker zoals **Britta simon**.
    
    b. In **e-mailadres** tekst en voer het e-mailadres van de gebruiker zoals **brittasimon@contoso.com**.

    >[!Note]
    >Selecteer het geverifieerde domein in de domeinlijst.

    c. Klik op **Add**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan één Zoho.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon aan één Zoho, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Zoho één**.

    ![De Zoho één koppeling in de lijst met toepassingen](./media/zohoone-tutorial/tutorial_zohoone_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de Zoho een tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Zoho één toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zohoone-tutorial/tutorial_general_01.png
[2]: ./media/zohoone-tutorial/tutorial_general_02.png
[3]: ./media/zohoone-tutorial/tutorial_general_03.png
[4]: ./media/zohoone-tutorial/tutorial_general_04.png

[100]: ./media/zohoone-tutorial/tutorial_general_100.png

[200]: ./media/zohoone-tutorial/tutorial_general_200.png
[201]: ./media/zohoone-tutorial/tutorial_general_201.png
[202]: ./media/zohoone-tutorial/tutorial_general_202.png
[203]: ./media/zohoone-tutorial/tutorial_general_203.png

