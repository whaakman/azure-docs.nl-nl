---
title: 'Zelfstudie: Azure Active Directory-integratie met Leapsome | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Leapsome.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cb523e97-add8-4289-b106-927bf1a02188
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.openlocfilehash: 4b2c23745a5e624bcf668dfbfe5d085392d7a583
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052448"
---
# <a name="tutorial-azure-active-directory-integration-with-leapsome"></a>Zelfstudie: Azure Active Directory-integratie met Leapsome

In deze zelfstudie leert u hoe u Leapsome integreren met Azure Active Directory (Azure AD).

Leapsome integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Leapsome heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Leapsome (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Leapsome, moet u de volgende items:

- Een Azure AD-abonnement
- Een Leapsome eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Leapsome uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-leapsome-from-the-gallery"></a>Leapsome uit de galerie toe te voegen
Voor het configureren van de integratie van Leapsome in Azure AD, moet u Leapsome uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Leapsome uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Leapsome**, selecteer **Leapsome** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Leapsome in de lijst met resultaten](./media/leapsome-tutorial/tutorial_leapsome_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Leapsome op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Leapsome is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Leapsome tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Leapsome, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Leapsome](#create-a-leapsome-test-user)**  : als u wilt een equivalent van Britta Simon in Leapsome die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Leapsome.

**Voor het configureren van Azure AD eenmalige aanmelding met Leapsome, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Leapsome** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/leapsome-tutorial/tutorial_leapsome_samlbase.png)

3. Op de **Leapsome domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Leapsome domein en URL's, eenmalige aanmelding informatie](./media/leapsome-tutorial/tutorial_leapsome_url.png)

    a. In de **id** tekstvak, een URL typen: `https://www.leapsome.com`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/assert`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Leapsome domein en URL's, eenmalige aanmelding informatie](./media/leapsome-tutorial/tutorial_leapsome_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/login`
     
    > [!NOTE] 
    > De voorgaande antwoord-URL en de aanmeldings-URL-waarde is geen echte waarde. U werkt met de werkelijke waarden, die later in de zelfstudie wordt uitgelegd.

5. Leapsome toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld.
    
    ![Eenmalige aanmelding configureren](./media/leapsome-tutorial/tutorial_Leapsome_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk | Naamruimte |
    | ---------------| --------------- | --------- |   
    | Voornaam | User.givenName | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | Achternaam | User.surname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | titel | user.jobtitle | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | afbeelding | URL van afbeelding van de werknemer | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

    > [!Note]
    > De waarde van afbeelding kenmerk is niet echt. Deze waarde bijwerken met de werkelijke afbeeldings-URL. Om op te halen deze waarde contact op met [Leapsome Client ondersteuningsteam](mailto:support@leapsome.com).
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/leapsome-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/leapsome-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. In de **Namespace** tekstvak typt u de naamruimte-uri voor die rij.
    
    e. Klik op **Ok**

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/leapsome-tutorial/tutorial_leapsome_certificate.png) 

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/leapsome-tutorial/tutorial_general_400.png)
    
9. Op de **Leapsome configuratie** sectie, klikt u op **configureren Leapsome** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Leapsome configuratie](./media/leapsome-tutorial/tutorial_leapsome_configure.png)

10. In een ander browservenster aanmelden bij Leapsome als een beveiligingsbeheerder.

11. Klik op instellingen logo in de rechterbovenhoek, en klik vervolgens op **beheerdersinstellingen**. 

    ![Leapsome instellen](./media/leapsome-tutorial/tutorial_leapsome_admin.png)

12. Klik op de linkermenubalk **eenmalige aanmelding (SSO)**, en klik op de **SAML gebaseerde eenmalige aanmelding (SSO)** pagina de volgende stappen uitvoeren:
    
    ![Leapsome saml](./media/leapsome-tutorial/tutorial_leapsome_samlsettings.png)

    a. Selecteer **inschakelen SAML gebaseerde eenmalige aanmelding**.

    b. Kopieer de **aanmeldings-URL (uw gebruikers hier punt voor het starten van aanmelding)** waarde en plak deze in de **aanmeldings-URL** -tekstvak in **Leapsome domein en URL's** sectie in Azure portal.

    c. Kopieer de **antwoord-URL (reactie ontvangen van uw id-provider)** waarde en plak deze in de **antwoord-URL** -tekstvak in **Leapsome domein en URL's** sectie in Azure portal.

    d. In de **SSO-aanmeldings-URL (geleverd door de id-provider)** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    e. Kopieer het certificaat dat u hebt gedownload vanuit Azure portal zonder--certificaat BEGIN en einde certificaat--opmerkingen en plak deze in de **certificaat (geleverd door de id-provider)** tekstvak.

    f. Klik op **UPDATE SSO-instellingen**.
    
### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/leapsome-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/leapsome-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/leapsome-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/leapsome-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-leapsome-test-user"></a>Maak een testgebruiker Leapsome

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Leapsome. Werken met [Leapsome Client ondersteuningsteam](mailto:support@leapsome.com) om toe te voegen de gebruikers of het domein dat moet worden opgenomen in de whitelist in het Leapsome-platform. Als het domein is toegevoegd door het team, krijgen automatisch gebruikers ingericht voor het Leapsome-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Leapsome.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Leapsome toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Leapsome**.

    ![De koppeling Leapsome in de lijst met toepassingen](./media/leapsome-tutorial/tutorial_leapsome_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Leapsome in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Leapsome.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/leapsome-tutorial/tutorial_general_01.png
[2]: ./media/leapsome-tutorial/tutorial_general_02.png
[3]: ./media/leapsome-tutorial/tutorial_general_03.png
[4]: ./media/leapsome-tutorial/tutorial_general_04.png

[100]: ./media/leapsome-tutorial/tutorial_general_100.png

[200]: ./media/leapsome-tutorial/tutorial_general_200.png
[201]: ./media/leapsome-tutorial/tutorial_general_201.png
[202]: ./media/leapsome-tutorial/tutorial_general_202.png
[203]: ./media/leapsome-tutorial/tutorial_general_203.png

