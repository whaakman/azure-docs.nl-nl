---
title: 'Zelfstudie: Azure Active Directory-integratie met FiscalNote | Microsoft-documenten'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FiscalNote.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 55274f26-be7e-4514-964c-7186ecb55c4a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 2e0d0f2d6fc428dbeb20ce5e056a8d6ed9a0ac97
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-azure-active-directory-integration-with-fiscalnote"></a>Zelfstudie: Azure Active Directory-integratie met FiscalNote

In deze zelfstudie leert u hoe FiscalNote integreren met Azure Active Directory (Azure AD).

FiscalNote integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FiscalNote heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij FiscalNote (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FiscalNote, moet u de volgende items:

- Een Azure AD-abonnement
- Een FiscalNote eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FiscalNote uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-fiscalnote-from-the-gallery"></a>FiscalNote uit de galerie toevoegen
Voor het configureren van de integratie van FiscalNote in Azure AD, moet u FiscalNote uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen FiscalNote uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **FiscalNote**, selecteer **FiscalNote** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![FiscalNote in de lijst met resultaten](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met FiscalNote op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in FiscalNote is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in FiscalNote tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met FiscalNote, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker FiscalNote](#create-a-fiscalnote-test-user)**  - FiscalNote die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing FiscalNote configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met FiscalNote, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **FiscalNote** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_samlbase.png)

3. Op de **FiscalNote domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en FiscalNote domein eenmalige aanmelding informatie](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_url.png)
    
    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<InstanceName>.fiscalnote.com/login?client=<ClientID>&redirect_uri=https://app.fiscalnote.com/saml-login.html&audience=https://api.fiscalnote.com/&connection=<CONNECTION_NAME>&response_type=id_token%20token`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `urn:auth0:fiscalnote:<CONNECTIONNAME>`

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [FiscalNote Client ondersteuningsteam](mailto:support@fiscalnote.com) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Raw)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_certificate.png)

5. De toepassing FiscalNote verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-fiscalnote-tutorial/tutorial_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en de volgende stappen uitvoeren:
           
    | Naam kenmerk | Waarde kenmerk |
    | ---------------| ----------------|
    | naam | User.userPrincipalName|
    | givenName| User.givenName|
    | familyName| User.surname|
    | e-mail| User.mail|
    
    a. Verwijder bestaande kenmerken en nieuwe kenmerken toevoegen. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-fiscalnote-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-fiscalnote-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat leeg naamruimte.
    
    e. Klik op **OK**.

7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_400.png)

8. Op de **FiscalNote configuratie** sectie, klikt u op **configureren FiscalNote** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![FiscalNote configuratie](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_configure.png) 

9. Eenmalige aanmelding configureren op **FiscalNote** zijde, moet u de gedownloade verzenden **Certificate(Raw), Sign-Out URL SAML entiteit-ID en SAML Single Sign-On Service-URL** naar [FiscalNote ondersteuningsteam](mailto:support@fiscalnote.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-fiscalnote-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-fiscalnote-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-fiscalnote-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-fiscalnote-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-fiscalnote-test-user"></a>Een testgebruiker FiscalNote maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in FiscalNote genoemd. FiscalNote ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot FiscalNote als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [FiscalNote ondersteuningsteam](mailto:support@fiscalnote.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan FiscalNote.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen FiscalNote, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **FiscalNote**.

    ![De koppeling FiscalNote in de lijst met toepassingen](./media/active-directory-saas-fiscalnote-tutorial/tutorial_fiscalnote_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel FiscalNote in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing FiscalNote.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fiscalnote-tutorial/tutorial_general_203.png

