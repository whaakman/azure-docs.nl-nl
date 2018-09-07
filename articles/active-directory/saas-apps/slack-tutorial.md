---
title: 'Zelfstudie: Azure Active Directory-integratie met Slack | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Slack.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: b742f3eb9124093bcf0c3c912bbae0367cdcce56
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44053836"
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Zelfstudie: Azure Active Directory-integratie met Slack

In deze zelfstudie leert u hoe u Slack integreren met Azure Active Directory (Azure AD).

Slack integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Slack heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Slack (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Slack, moet u de volgende items:

- Een Azure AD-abonnement
- Een Slack eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Slack uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-slack-from-the-gallery"></a>Slack uit de galerie toe te voegen
Voor het configureren van de integratie van Slack in Azure AD, moet u Slack uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Slack uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **Slack**.

    ![Het maken van een Azure AD-testgebruiker](./media/slack-tutorial/tutorial_slack_search.png)

1. Selecteer in het deelvenster resultaten **Slack**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Slack op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Slack is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Slack tot stand worden gebracht.

In Slack, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Slack, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een Slack testgebruiker](#creating-a-slack-test-user)**  : als u wilt een equivalent van Britta Simon in Slack die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Slack-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Slack, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Slack** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/slack-tutorial/tutorial_slack_samlbase.png)

1. Op de **Slack-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/slack-tutorial/tutorial_slack_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.slack.com`

    b. In de **id** tekstvak, werkt de waarde bij met de aanmelding URL. Dit is het domein van uw werkruimte. Bijvoorbeeld: `https://contoso.slack.com`

1. Slack-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/slack-tutorial/tutorial_slack_attribute.png)

    > [!NOTE] 
    > Als u gebruikers hebt die is toegewezen **e-mailadres** zich niet op een licentie voor Office 365 de **User.Email** claim wordt niet weergegeven in het SAML-Token. In dergelijke gevallen beste **user.userprincipalname** als de **User.Email** waarde om toe te wijzen als van het kenmerk **unieke id** in plaats daarvan.

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster, selecteer **user.mail** als **gebruikers-id** en voor elke rij wordt weergegeven in de onderstaande tabel, voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | --- | --- |
    | first_name | User.givenName |
    | last_name | User.surname |
    | User.Email | User.mail |  
    | User.Username | User.userPrincipalName |

    a. Klik op **kenmerk** openen **kenmerk bewerken** dialoogvenster vak en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/slack-tutorial/tutorial_slack_attribute1.png)

    a. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    b. Uit de **waarde** , selecteert u de waarde van het kenmerk wordt weergegeven voor die rij.

    c. Laat de **Namespace** leeg.

    d. Klik op **OK**

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/slack-tutorial/tutorial_slack_certificate.png)

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/slack-tutorial/tutorial_general_400.png)

1. Op de **Slack configuratie** sectie, klikt u op **configureren Slack** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/slack-tutorial/tutorial_slack_configure.png)

1. In een ander browservenster aanmelden bij uw site Slack bedrijf als beheerder.

1. Navigeer naar **Microsoft Azure AD** Ga vervolgens naar **Teaminstellingen**.

     ![Single Sign-On aan App configureren](./media/slack-tutorial/tutorial_slack_001.png)

1. In de **Teaminstellingen** sectie, klikt u op de **verificatie** tabblad en klik vervolgens op **Wijzigingsinstellingen**.

    ![Single Sign-On aan App configureren](./media/slack-tutorial/tutorial_slack_002.png)

1. Op de **SAML-verificatie-instellingen** dialoogvenster, voer de volgende stappen uit:

    ![Single Sign-On aan App configureren](./media/slack-tutorial/tutorial_slack_003.png)

    a.  In de **SAML 2.0-eindpunt (HTTP)** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    b.  In de **Identity Provider Issuer** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    c.  Open het gedownloade certificaat-bestand in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **openbaar certificaat** tekstvak.

    d. De bovenstaande drie instellingen configureren als geschikt is voor uw Slack-team. Raadpleeg voor meer informatie over de instellingen voor de **configuratiehandleiding voor eenmalige aanmelding van Slack** hier. `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    e.  Klik op **configuratie op te slaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/slack-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/slack-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.

    ![Het maken van een Azure AD-testgebruiker](./media/slack-tutorial/create_aaduser_03.png)

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Het maken van een Azure AD-testgebruiker](./media/slack-tutorial/create_aaduser_04.png)

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-slack-test-user"></a>Het maken van een gebruiker Slack testen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Slack. Slack biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Slack als deze nog niet bestaat. Slack biedt ook ondersteuning voor automatisch gebruikers inrichten, vindt u meer details [hier](slack-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact opnemen met [Slack ondersteuningsteam](https://slack.com/help/contact).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Slack.

![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon naar Slack, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Slack**.

    ![Eenmalige aanmelding configureren](./media/slack-tutorial/tutorial_slack_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op het Slack-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Slack-toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](slack-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/slack-tutorial/tutorial_general_01.png
[2]: ./media/slack-tutorial/tutorial_general_02.png
[3]: ./media/slack-tutorial/tutorial_general_03.png
[4]: ./media/slack-tutorial/tutorial_general_04.png

[100]: ./media/slack-tutorial/tutorial_general_100.png

[200]: ./media/slack-tutorial/tutorial_general_200.png
[201]: ./media/slack-tutorial/tutorial_general_201.png
[202]: ./media/slack-tutorial/tutorial_general_202.png
[203]: ./media/slack-tutorial/tutorial_general_203.png
