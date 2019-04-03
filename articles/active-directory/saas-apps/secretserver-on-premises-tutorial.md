---
title: 'Zelfstudie: Azure Active Directory-integratie met Secret Server (On-Premises) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en geheim-Server (On-Premises).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: be4ba84a-275d-4f71-afce-cb064edc713f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9167a5ed72e6fec2ca03cc97d1d41dd6cd4aaba6
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885839"
---
# <a name="tutorial-azure-active-directory-integration-with-secret-server-on-premises"></a>Zelfstudie: Azure Active Directory-integratie met Secret Server (On-Premises)

In deze zelfstudie leert u hoe u geheim Server (On-Premises) integreren met Azure Active Directory (Azure AD).

Geheim Server (On-Premises) integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot geheim Server (On-Premises heeft).
- U kunt uw gebruikers automatisch ophalen aangemeld bij geheim Server (On-Premises) (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Secret Server (On-Premises), moet u de volgende items:

- Een Azure AD-abonnement
- Een geheim Server (On-Premises) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Geheim Server (On-Premises) uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-secret-server-on-premises-from-the-gallery"></a>Geheim Server (On-Premises) uit de galerie toevoegen
Als u wilt de integratie van geheim-Server (On-Premises) in Azure AD configureert, moet u geheim-Server (On-Premises) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen geheim Server (On-Premises) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **geheim Server (On-Premises)**, selecteer **geheim Server (On-Premises)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Geheime Server (On-Premises) in de lijst met resultaten](./media/secretserver-on-premises-tutorial/tutorial_secretserver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met geheim Server (On-Premises) op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in geheim Server (On-Premises) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in geheim Server (On-Premises) tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Secret Server (On-Premises), moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker geheim Server (On-Premises)](#create-a-secret-server-on-premises-test-user)**  : als u wilt een equivalent van Britta Simon hebt in een geheim Server (On-Premises) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing geheim Server (On-Premises).

**Voor het configureren van Azure AD eenmalige aanmelding met Secret Server (On-Premises), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **geheim Server (On-Premises)** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/secretserver-on-premises-tutorial/tutorial_secretserver_samlbase.png)

1. Op de **geheim Server (On-Premises)-domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Geheime Server (On-Premises)-domein en URL's eenmalige aanmelding informatie](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url.png)

    a. In de **id** tekstvak, voert u de gebruiker gekozen waarde als een voorbeeld: `https://secretserveronpremises.azure`

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: `https://<SecretServerURL>/SAML/AssertionConsumerService.aspx`

    > [!NOTE]
    > De entiteit-ID die hierboven is slechts een voorbeeld en u bent kiezen van een unieke waarde die uw geheim Server-exemplaar in Azure AD. U wilt verzenden van deze entiteit-ID aan [geheim Server (On-Premises)-Client-ondersteuningsteam](https://thycotic.force.com/support/s/) en configureer ze deze op hun kant. Lees voor meer informatie, [in dit artikel](https://thycotic.force.com/support/s/article/Configuring-SAML-in-Secret-Server).

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Geheime Server (On-Premises)-domein en URL's eenmalige aanmelding informatie](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url1.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<SecretServerURL>/login.aspx`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de echte antwoord-URL en aanmeldings-URL. Neem contact op met [geheim Server (On-Premises)-Client-ondersteuningsteam](https://thycotic.force.com/support/s/) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/secretserver-on-premises-tutorial/tutorial_secretserver_certificate.png)

1. Controleer **geavanceerde instellingen voor het ondertekenen van certificaat weergeven** en selecteer **ondertekening optie** als **aanmelding SAML-antwoord en -bewering**.

    ![Opties voor ondertekening](./media/secretserver-on-premises-tutorial/signing.png)

1. Klik op de knop **Save**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/secretserver-on-premises-tutorial/tutorial_general_400.png)
    
1. Op de **geheim-serverconfiguratie (On-Premises)** sectie, klikt u op **geheim Server (On-Premises) configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Configuratie van geheime Server (On-Premises)](./media/secretserver-on-premises-tutorial/tutorial_secretserver_configure.png)

1. Het configureren van eenmalige aanmelding op **geheim Server (On-Premises)** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64), URL zijn afmelding, Single Sign-On Service URL voor SAML**, en **SAML-entiteit ID** naar [geheim Server (On-Premises)-ondersteuningsteam](https://thycotic.force.com/support/s/). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/secretserver-on-premises-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/secretserver-on-premises-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/secretserver-on-premises-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/secretserver-on-premises-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-secret-server-on-premises-test-user"></a>Maak een testgebruiker geheim Server (On-Premises)

In deze sectie maakt u een gebruiker met de naam van Britta Simon in geheim Server (On-Premises). Werken met [geheim Server (On-Premises)-ondersteuningsteam](https://thycotic.force.com/support/s/) om toe te voegen de gebruikers in het geheim Server (On-Premises)-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan geheim Server (On-Premises).

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon geheim server (On-Premises), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

1. Selecteer in de lijst met toepassingen, **geheim Server (On-Premises)**.

    ![De koppeling geheim Server (On-Premises) in de lijst met toepassingen](./media/secretserver-on-premises-tutorial/tutorial_secretserver_app.png)

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op de knop **Add**. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel geheim Server (On-Premises) in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing geheim Server (On-Premises).
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

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

