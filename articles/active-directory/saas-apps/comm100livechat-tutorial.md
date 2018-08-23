---
title: 'Zelfstudie: Azure Active Directory-integratie met Comm100 Live Chat | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Comm100 Live Chat.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0340d7f3-ab54-49ef-b77c-62a0efd5d49c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: jeedes
ms.openlocfilehash: b85162c8392e8ecdb08a3ed04ff5b9de835808a1
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "42057191"
---
# <a name="tutorial-azure-active-directory-integration-with-comm100-live-chat"></a>Zelfstudie: Azure Active Directory-integratie met Comm100 Live Chat

In deze zelfstudie leert u hoe u Comm100 Live Chat integreert met Azure Active Directory (Azure AD).

Comm100 Live Chat integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Comm100 Live Chat heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Comm100 Live Chat (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Comm100 Live Chat, moet u de volgende items:

- Een Azure AD-abonnement
- Een Comm100 Live Chat eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Uit de galerie toe te voegen Comm100 Live Chat
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-comm100-live-chat-from-the-gallery"></a>Uit de galerie toe te voegen Comm100 Live Chat
Voor het configureren van de integratie van Comm100 Live Chat in Azure AD, moet u Comm100 Live Chat toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Comm100 Live Chat uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Comm100 Live Chat**, selecteer **Comm100 Live Chat** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Comm100 Live Chat in de lijst met resultaten](./media/comm100livechat-tutorial/tutorial_comm100livechat_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Comm100 Live Chat op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Comm100 Live Chat is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Comm100 Live Chat tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Comm100 Live Chat, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Comm100 Live Chat](#create-a-comm100-live-chat-test-user)**  : als u wilt een equivalent van Britta Simon in Comm100 Live Chat die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Comm100 Live Chat.

**Voor het configureren van Azure AD eenmalige aanmelding met Comm100 Live Chat, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Comm100 Live Chat** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/comm100livechat-tutorial/tutorial_comm100livechat_samlbase.png)

3. Op de **Comm100 Live Chat domein en URL's** sectie, voert u de volgende stappen uit:

    ![Comm100 Live Chat domein en URL's, eenmalige aanmelding informatie](./media/comm100livechat-tutorial/tutorial_comm100livechat_url.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<SUBDOMAIN>.comm100.com/AdminManage/LoginSSO.aspx?siteId=<SITEID>`
    
    > [!NOTE] 
    > De aanmeldings-URL-waarde is niet echt. U kunt de aanmeldings-URL-waarde wordt bijgewerkt met de werkelijke aanmeldings-URL, die later in de zelfstudie wordt uitgelegd.

4. Toepassing Comm100 Live Chat wordt verwacht dat de SAML-asserties ondertekend naar specifieke kenmerken bevatten. Configureer de volgende kenmerken voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/comm100livechat-tutorial/tutorial_attribute_03.png)
    
5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    |  Naam kenmerk  | Waarde kenmerk |
    | --------------- | -------------------- |    
    |   e-mailen    | User.mail |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/comm100livechat-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/comm100livechat-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/comm100livechat-tutorial/tutorial_comm100livechat_certificate.png) 

7. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/comm100livechat-tutorial/tutorial_general_400.png)

8. Op de **Comm100 Live Chat configuratie** sectie, klikt u op **configureren Comm100 Live Chat** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Configuratie van Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_configure.png)

9. In een ander browservenster, meld u aan bij Comm100 Live Chat als een beveiligingsbeheerder.

10. Op boven aan de rechterkant van de pagina, klikt u op **Mijn Account**.

    ![Myaccount Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

11. Aan de linkerkant van het menu, klikt u op **Security** en klik vervolgens op **Agent Single Sign-On**.

    ![Beveiliging Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_security.png)

12. Op de **Agent Single Sign-On** pagina, voert u de volgende stappen uit:

    ![Beveiliging Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_singlesignon.png)

    a. De eerste koppeling in de gemarkeerde Kopieer en plak deze in **aanmeldings-URL** -tekstvak in **Comm100 Live Chat domein en URL's** sectie in Azure portal.

    b. In de **URL voor SAML SSO-** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **externe URL voor afmelden** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **kiest u een bestand** voor het uploaden van de base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal, in de **certificaat**.

    e. Klik op **wijzigingen opslaan**

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/comm100livechat-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/comm100livechat-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/comm100livechat-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/comm100livechat-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-comm100-live-chat-test-user"></a>Maak een testgebruiker Comm100 Live Chat

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Comm100 Live Chat, moeten ze worden ingericht voor het Comm100 Live Chat. In Comm100 Live Chat is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij Comm100 Live Chat als een beveiligingsbeheerder.

2. Op boven aan de rechterkant van de pagina, klikt u op **Mijn Account**.

    ![Myaccount Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

3. Aan de linkerkant van het menu, klikt u op **Agents** en klik vervolgens op **nieuwe Agent**.

    ![Agent Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_agent.png)

4. Op de **nieuwe Agent** pagina, voert u de volgende stappen uit:

    ![Nieuwe agent Comm100 Live Chat](./media/comm100livechat-tutorial/tutorial_comm100livechat_newagent.png)

    a. a. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals **Brittasimon@contoso.com**.

    b. In **voornaam** tekst voert u de voornaam van de gebruiker, zoals **Julia**.

    c. In **achternaam** tekst voert u de achternaam van de gebruiker, zoals **simon**.

    d. In de **weergavenaam** tekstvak, voer de weergavenaam van de gebruiker, zoals **Julia steen**

    e. In de **wachtwoord** tekstvak typt u het wachtwoord.

    f. Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Comm100 Live Chat.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon bij Comm100 Live Chat, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Comm100 Live Chat**.

    ![De koppeling Comm100 Live Chat in de lijst met toepassingen](./media/comm100livechat-tutorial/tutorial_comm100livechat_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Comm100 Live Chat in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Comm100 Live Chat.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/comm100livechat-tutorial/tutorial_general_01.png
[2]: ./media/comm100livechat-tutorial/tutorial_general_02.png
[3]: ./media/comm100livechat-tutorial/tutorial_general_03.png
[4]: ./media/comm100livechat-tutorial/tutorial_general_04.png

[100]: ./media/comm100livechat-tutorial/tutorial_general_100.png

[200]: ./media/comm100livechat-tutorial/tutorial_general_200.png
[201]: ./media/comm100livechat-tutorial/tutorial_general_201.png
[202]: ./media/comm100livechat-tutorial/tutorial_general_202.png
[203]: ./media/comm100livechat-tutorial/tutorial_general_203.png

