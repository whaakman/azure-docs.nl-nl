---
title: 'Zelfstudie: Azure Active Directory-integratie met organigram nu | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en organigram nu.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 50a1522f-81de-4d14-9b6b-dd27bb1338a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: jeedes
ms.openlocfilehash: 0320ccdf2e7c7e032e153fa46605d98637246f31
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-azure-active-directory-integration-with-orgchart-now"></a>Zelfstudie: Azure Active Directory-integratie met organigram nu

In deze zelfstudie leert u hoe organigram nu integreren met Azure Active Directory (Azure AD).

Organigram nu integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot organigram nu heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij organigram nu (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met organigram nu configureren, moet u de volgende items:

- Een Azure AD-abonnement
- Een organigram nu eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Organigram nu vanuit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-orgchart-now-from-the-gallery"></a>Organigram nu vanuit de galerie toevoegen
Voor het configureren van de integratie van organigram nu in Azure AD, moet u organigram nu vanuit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen organigram nu vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **organigram nu**, selecteer **organigram nu** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Organigram nu in de lijst met resultaten](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie u configureert en test eenmalige aanmelding Azure AD met organigram nu gebaseerd op een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in organigram nu is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in organigram nu tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met organigram nu, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker organigram nu](#create-an-orgchart-now-test-user)**  - organigram nu die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing organigram nu configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met organigram nu, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **organigram nu** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_samlbase.png)

3. Op de **organigram nu domein en de URL's** sectie als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Organigram nu domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_url.png)

    In de **id** textbox, typ een URL: `https://sso2.orgchartnow.com`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Organigram nu domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://sso2.orgchartnow.com/Shibboleth.sso/Login?entityID=<YourEntityID>&target=https://sso2.orgchartnow.com`
     
    > [!NOTE]
    > `<YourEntityID>` de SAML-entiteit-ID opgehaald uit de sectie Naslaggids verderop in de zelfstudie wordt beschreven.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_400.png)
    
7. Op de **organigram nu configuratie** sectie, klikt u op **organigram nu configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID** van de **Naslaggids punt** en deze gebruiken om te voltooien **aanmeldings-URL** in **organigram nu domein en de URL's sectie**.

    ![Organigram nu configuratie](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_configure.png) 

8. Eenmalige aanmelding configureren op **organigram nu** zijde, moet u de gedownloade verzenden **Metadata XML** naar [organigram nu ondersteuningsteam](mailto:ocnsupport@officeworksoftware.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-orgchart-now-test-user"></a>Een organigram nu testgebruiker maken

Om Azure AD-gebruikers zich aanmelden bij organigram nu, moeten ze worden ingericht in organigram nu. 

1. Organigram nu ondersteuning biedt voor just-in-time inrichting, dit is standaard ingeschakeld. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot organigram nu als deze nog niet bestaat. De functie inrichting just-in-time-gebruiker maakt alleen een **alleen-lezen** gebruiker wanneer een aanvraag voor eenmalige aanmelding is afkomstig uit een herkende IDP en de e-mailadres in van het SAML-verklaring niet in de lijst met gebruikers gevonden is. Voor deze automatische inrichting van de functie die u wilt maken van een toegangsgroep met de titel **algemene** in organigram nu. Volg de onderstaande stappen voor het maken van een groep voor toegang tot:

    a. Ga naar de **groepen beheren** optie wanneer u op de **stemmen** in de rechterbovenhoek van de gebruikersinterface.

    ![Organigram nu groepen](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_manage.png)  

    b. Selecteer de **toevoegen** pictogram en de naam van de groep **algemene** klikt u vervolgens op **OK**. 

    ![Organigram nu toevoegen](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_add.png)

    c. Selecteer de mappen die u wenst dat de algemene of alleen-lezen-gebruikers moeten toegang hebben tot:

    ![Organigram nu mappen](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_chart.png)

    d. **Vergrendeling** de mappen zodat alleen gebruikers met beheerdersrechten deze kunnen wijzigen. Druk op **OK**.

    ![Organigram nu vergrendelen](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_lock.png)

2. Maken van **Admin** gebruikers en **lezen/schrijven** gebruikers, moet u handmatig een gebruiker maken om te kunnen toegang krijgen tot hun machtigingsniveau via eenmalige aanmelding. Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

    a. Aanmelden bij organigram nu als een beveiligingsbeheerder.

    b.  Klik op **instellingen** in de rechterbovenhoek hoek en navigeer vervolgens naar **gebruikers beheren**.

    ![Organigram nu instellingen](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_settings.png)

    c. Klik op **toevoegen** en voer de volgende stappen uit:

    ![Organigram nu beheren](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_manageusers.png)

    * In de **gebruikers-ID** textbox, geef de gebruikers-ID zoals **brittasimon@contoso.com**.

    * In **e-mailadres** tekst en voer het e-mailadres van de gebruiker zoals **brittasimon@contoso.com**.

    * Klik op **Add**.
    
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan organigram nu.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen organigram nu, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **organigram nu**.

    ![De koppeling organigram nu in de lijst met toepassingen](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel organigram nu in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing organigram nu.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_203.png

