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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jeedes
ms.openlocfilehash: 5679c815b26bf0b5e3a427e9551dde7b5dffa3d2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046539"
---
# <a name="tutorial-azure-active-directory-integration-with-orgchart-now"></a>Zelfstudie: Azure Active Directory-integratie met organigram nu

In deze zelfstudie leert u hoe u organigram nu integreren met Azure Active Directory (Azure AD).

Organigram nu integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot organigram nu heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij organigram nu (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met organigram nu, moet u de volgende items:

- Een Azure AD-abonnement
- Een organigram nu eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Organigram nu uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-orgchart-now-from-the-gallery"></a>Organigram nu uit de galerie toe te voegen
Voor het configureren van de integratie van organigram nu in Azure AD, moet u organigram nu uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen organigram nu vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **organigram nu**, selecteer **organigram nu** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Organigram nu in de lijst met resultaten](./media/orgchartnow-tutorial/tutorial_orgchartnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met organigram nu op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in organigram nu is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in organigram nu tot stand worden gebracht.

Als u wilt configureren en Azure AD eenmalige aanmelding met organigram nu testen, moet u uitvoeren van de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker organigram nu](#create-an-orgchart-now-test-user)**  : als u wilt een equivalent van Britta Simon in organigram nu die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in uw toepassing organigram nu configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met organigram nu, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **organigram nu** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/orgchartnow-tutorial/tutorial_orgchartnow_samlbase.png)

3. Op de **organigram nu domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Organigram nu domein en URL's, eenmalige aanmelding informatie](./media/orgchartnow-tutorial/tutorial_orgchartnow_url.png)

    In de **id** tekstvak, een URL typen: `https://sso2.orgchartnow.com`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Organigram nu domein en URL's, eenmalige aanmelding informatie](./media/orgchartnow-tutorial/tutorial_orgchartnow_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://sso2.orgchartnow.com/Shibboleth.sso/Login?entityID=<YourEntityID>&target=https://sso2.orgchartnow.com`
     
    > [!NOTE]
    > `<YourEntityID>` de SAML-entiteit-ID opgehaald uit de sectie ter referentie, verderop in de zelfstudie wordt beschreven.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/orgchartnow-tutorial/tutorial_orgchartnow_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/orgchartnow-tutorial/tutorial_general_400.png)
    
7. Op de **organigram nu configuratie** sectie, klikt u op **organigram nu configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** uit de **Naslaggids sectie** en deze gebruiken om te voltooien **aanmeldings-URL** in **organigram nu domein en URL's sectie**.

    ![Organigram nu configureren](./media/orgchartnow-tutorial/tutorial_orgchartnow_configure.png) 

8. Het configureren van eenmalige aanmelding op **organigram nu** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [organigram nu ondersteuningsteam](mailto:ocnsupport@officeworksoftware.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/orgchartnow-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/orgchartnow-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/orgchartnow-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/orgchartnow-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-orgchart-now-test-user"></a>Maak een testgebruiker organigram nu

Als u wilt dat gebruikers zich aanmelden bij organigram nu Azure AD, moeten ze worden ingericht voor organigram nu. 

1. Organigram nu biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot organigram nu als deze nog niet bestaat. De just-in-time-functie voor gebruikersinrichting maakt alleen een **alleen-lezen** gebruiker wanneer een SSO-aanvraag afkomstig van een bekende id-provider is en de e-mailadres in het SAML-verklaring niet in de lijst met gebruikers gevonden is. Voor deze automatische inrichting van de functie die u wilt maken van een groep voor toegang tot met de titel **algemene** in organigram nu. Volg de onderstaande stappen voor het maken van een groep voor toegang tot:

    a. Ga naar de **groepen beheren** optie wanneer u op de **tandwielpictogram** in de rechterbovenhoek van de gebruikersinterface.

    ![Organigram nu groepen](./media/orgchartnow-tutorial/tutorial_orgchartnow_manage.png)    

    b. Selecteer de **toevoegen** pictogram en de naam van de groep **algemene** klikt u vervolgens op **OK**. 

    ![Organigram nu toevoegen](./media/orgchartnow-tutorial/tutorial_orgchartnow_add.png)

    c. Selecteer de mappen die u wilt dat de gebruikers algemeen of alleen-lezen toegang tot:

    ![Organigram nu mappen](./media/orgchartnow-tutorial/tutorial_orgchartnow_chart.png)

    d. **Vergrendeling** de mappen zodat alleen gebruikers met beheerdersrechten kunnen ze wijzigen. Druk op **OK**.

    ![Organigram nu vergrendelen](./media/orgchartnow-tutorial/tutorial_orgchartnow_lock.png)

2. Maken **Admin** gebruikers en **lezen/schrijven** gebruikers, moet u handmatig een gebruiker maken om te kunnen toegang krijgen tot hun niveau van bevoegdheden via eenmalige aanmelding. Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

    a. Meld u aan bij organigram nu als een beveiligingsbeheerder.

    b.  Klik op **instellingen** in de rechterbovenhoek hoek en navigeer vervolgens naar **gebruikers beheren**.

    ![Organigram nu instellingen](./media/orgchartnow-tutorial/tutorial_orgchartnow_settings.png)

    c. Klik op **toevoegen** en voer de volgende stappen uit:

    ![Organigram nu beheren](./media/orgchartnow-tutorial/tutorial_orgchartnow_manageusers.png)

    * In de **gebruikers-ID** tekstvak, geef de gebruikers-ID, zoals **brittasimon@contoso.com**.

    * In **e-mailadres** tekst vak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    * Klik op **Add**.
    
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon door toegang te verlenen tot organigram nu gebruik van Azure eenmalige aanmelding.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon tot organigram nu, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **organigram nu**.

    ![De koppeling organigram nu in de lijst met toepassingen](./media/orgchartnow-tutorial/tutorial_orgchartnow_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel organigram nu in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing organigram nu.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/orgchartnow-tutorial/tutorial_general_01.png
[2]: ./media/orgchartnow-tutorial/tutorial_general_02.png
[3]: ./media/orgchartnow-tutorial/tutorial_general_03.png
[4]: ./media/orgchartnow-tutorial/tutorial_general_04.png

[100]: ./media/orgchartnow-tutorial/tutorial_general_100.png

[200]: ./media/orgchartnow-tutorial/tutorial_general_200.png
[201]: ./media/orgchartnow-tutorial/tutorial_general_201.png
[202]: ./media/orgchartnow-tutorial/tutorial_general_202.png
[203]: ./media/orgchartnow-tutorial/tutorial_general_203.png

