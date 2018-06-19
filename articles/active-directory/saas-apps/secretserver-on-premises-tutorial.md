---
title: 'Zelfstudie: Azure Active Directory-integratie met Secret Server (On-Premises) | Microsoft-documenten'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Secret-Server (On-Premises).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: be4ba84a-275d-4f71-afce-cb064edc713f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jeedes
ms.openlocfilehash: 552ba0ac9591f6c753e79ab5a1b406f2035457c0
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35907622"
---
# <a name="tutorial-azure-active-directory-integration-with-secret-server-on-premises"></a>Zelfstudie: Azure Active Directory-integratie met Secret Server (On-Premises)

In deze zelfstudie leert u het geheim Server (On-Premises) integreren met Azure Active Directory (Azure AD).

Secret Server (On-Premises) integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Secret Server (On-Premises heeft).
- U kunt uw gebruikers automatisch ophalen aangemeld bij Secret Server (On-Premises) (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Secret Server (On-Premises), moet u de volgende items:

- Een Azure AD-abonnement
- Een geheim Server (On-Premises) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Secret Server (On-Premises) uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-secret-server-on-premises-from-the-gallery"></a>Secret Server (On-Premises) uit de galerie toevoegen
Voor het configureren van de integratie van geheim Server (On-Premises) met Azure AD, moet u geheim-Server (On-Premises) uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Secret Server (On-Premises) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Secret Server (On-Premises)**, selecteer **Secret Server (On-Premises)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Geheime Server (On-Premises) in de lijst met resultaten](./media/secretserver-on-premises-tutorial/tutorial_secretserver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Secret Server (On-Premises) op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Secret Server (On-Premises) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Secret Server (On-Premises) tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Secret Server (On-Premises), moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Secret Server (On-Premises)](#create-a-secret-server-on-premises-test-user)**  - Secret Server (On-Premises) die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Secret Server (On-Premises).

**Voor het configureren van Azure AD eenmalige aanmelding met Secret Server (On-Premises), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Secret Server (On-Premises)** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/secretserver-on-premises-tutorial/tutorial_secretserver_samlbase.png)

3. Op de **geheim Server (On-Premises)-domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![Geheime Server (On-Premises)-domein en URL's van eenmalige aanmelding informatie](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url.png)

    a. In de **id** textbox, voert u de gebruiker gekozen waarde als een voorbeeld: `https://secretserveronpremises.azure`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<SecretServerURL>/SAML/AssertionConsumerService.aspx `

    > [!NOTE]
    > De entiteit-ID die hierboven is slechts een voorbeeld en u gratis een unieke waarde die uw geheim Server-exemplaar in Azure AD te kiezen. U moet deze entiteit-ID te verzenden [geheim Server (On-Premises) Client ondersteuningsteam](https://thycotic.force.com/support/s/) en configureer ze deze op hun kant. Lees voor meer informatie [in dit artikel](https://thycotic.force.com/support/s/article/Configuring-SAML-in-Secret-Server).

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Geheime Server (On-Premises)-domein en URL's van eenmalige aanmelding informatie](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<SecretServerURL>/login.aspx`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [geheim Server (On-Premises) Client ondersteuningsteam](https://thycotic.force.com/support/s/) ophalen van deze waarden.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/secretserver-on-premises-tutorial/tutorial_secretserver_certificate.png)

6. Controleer **weergeven geavanceerde instellingen voor het ondertekenen van certificaat** en selecteer **ondertekening optie** als **aanmelding SAML-reactie en assertion**.

    ![Opties voor ondertekening](./media/secretserver-on-premises-tutorial/signing.png)

7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/secretserver-on-premises-tutorial/tutorial_general_400.png)
    
8. Op de **geheim serverconfiguratie (On-Premises)** sectie, klikt u op **geheim Server configureren (On-Premises)** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Configuratie van de geheime Server (On-Premises)](./media/secretserver-on-premises-tutorial/tutorial_secretserver_configure.png)

9. Eenmalige aanmelding configureren op **Secret Server (On-Premises)** zijde, moet u de gedownloade verzenden **Certificate(Base64), Sign-Out-URL, SAML Single Sign-On Service-URL**, en **SAML-entiteit ID** naar [Secret Server (On-Premises) ondersteuningsteam](https://thycotic.force.com/support/s/). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/secretserver-on-premises-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/secretserver-on-premises-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/secretserver-on-premises-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/secretserver-on-premises-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-secret-server-on-premises-test-user"></a>Maak een testgebruiker Secret Server (On-Premises)

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in Secret Server (On-Premises) maken. Werken met [Secret Server (On-Premises) ondersteuningsteam](https://thycotic.force.com/support/s/) om toe te voegen de gebruikers van het platform Secret Server (On-Premises). Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Secret Server (On-Premises).

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen Secret Server (On-Premises), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **Secret Server (On-Premises)**.

    ![De koppeling Secret Server (On-Premises) in de lijst met toepassingen](./media/secretserver-on-premises-tutorial/tutorial_secretserver_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Secret Server (On-Premises) in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Secret Server (On-Premises).
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/secretserver-on-premises-tutorial/tutorial_general_01.png
[2]: ./media/secretserver-on-premises-tutorial/tutorial_general_02.png
[3]: ./media/secretserver-on-premises-tutorial/tutorial_general_03.png
[4]: ./media/secretserver-on-premises-tutorial/tutorial_general_04.png

[100]: ./media/secretserver-on-premises-tutorial/tutorial_general_100.png

[200]: ./media/secretserver-on-premises-tutorial/tutorial_general_200.png
[201]: ./media/secretserver-on-premises-tutorial/tutorial_general_201.png
[202]: ./media/secretserver-on-premises-tutorial/tutorial_general_202.png
[203]: ./media/secretserver-on-premises-tutorial/tutorial_general_203.png

