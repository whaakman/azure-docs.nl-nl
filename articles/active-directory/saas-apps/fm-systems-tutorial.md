---
title: 'Zelfstudie: Azure Active Directory-integratie met FM:Systems | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FM:Systems.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f78c58c5-6e98-458b-8991-78624a245665
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: a1d12784fb3f88889cd9ba0d622d1bc5d84374a8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165333"
---
# <a name="tutorial-azure-active-directory-integration-with-fmsystems"></a>Zelfstudie: Azure Active Directory-integratie met FM:Systems

In deze zelfstudie leert u hoe u FM:Systems integreren met Azure Active Directory (Azure AD).

FM:Systems integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FM:Systems heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij FM:Systems (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FM:Systems, moet u de volgende items:

- Een Azure AD-abonnement
- Een FM:Systems eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FM:Systems uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-fmsystems-from-the-gallery"></a>FM:Systems uit de galerie toe te voegen
Voor het configureren van de integratie van FM:Systems in Azure AD, moet u FM:Systems uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen FM:Systems uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **FM:Systems**.

    ![Het maken van een Azure AD-testgebruiker](./media/fm-systems-tutorial/tutorial_fmsystems_search.png)

1. Selecteer in het deelvenster resultaten **FM:Systems**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/fm-systems-tutorial/tutorial_fmsystems_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met FM:Systems op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in FM:Systems is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in FM:Systems tot stand worden gebracht.

In FM:Systems, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met FM:Systems, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker FM:Systems](#creating-an-fmsystems-test-user)**  : als u wilt een equivalent van Britta Simon in FM:Systems die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing FM:Systems.

**Voor het configureren van Azure AD eenmalige aanmelding met FM:Systems, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **FM:Systems** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/fm-systems-tutorial/tutorial_fmsystems_samlbase.png)

1. Op de **FM:Systems domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/fm-systems-tutorial/tutorial_fmsystems_url.png)

    In de **antwoord-URL** tekstvak typt u uw FM:Systems **antwoord-URL**, typt u de URL met behulp van het volgende patroon: `https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke antwoord-URL. Neem contact op met [FM:Systems ondersteuningsteam](https://fmsystems.com/ask-us/) deze waarde op te halen.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/fm-systems-tutorial/tutorial_fmsystems_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/fm-systems-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **FM:Systems** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [FM:Systems ondersteuningsteam](https://fmsystems.com/ask-us/). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld. U krijgt een melding wanneer eenmalige aanmelding is ingeschakeld voor uw abonnement.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/fm-systems-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/fm-systems-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/fm-systems-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/fm-systems-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-fmsystems-test-user"></a>Het maken van een testgebruiker FM:Systems

1. Meld u in een browservenster in uw bedrijf FM:Systems site als beheerder.

1. Ga naar **Systeembeheer \> beveiliging beheren \> gebruikers \> gebruikerslijst**.
   
    ![Systeembeheer](./media/fm-systems-tutorial/ic795905.png "Systeembeheer")

1. Klik op **nieuwe gebruiker maken**.
   
    ![Nieuwe gebruiker maken](./media/fm-systems-tutorial/ic795906.png "nieuwe gebruiker maken")

1. In de **Create User** sectie, voert u de volgende stappen uit:
   
    ![Create User](./media/fm-systems-tutorial/ic795907.png "Create User")
   
    a. Type de **gebruikersnaam**, wordt de **wachtwoord**, **wachtwoord bevestigen**, **e** en de **werknemer-ID** van een geldige Azure Active Directory-account die u inrichten in de bijbehorende tekstvakken wilt.
   
    b. Klik op **volgende**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot FM:Systems.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan FM:Systems toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **FM:Systems**.

    ![Eenmalige aanmelding configureren](./media/fm-systems-tutorial/tutorial_fmsystems_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel FM:Systems in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing FM:Systems.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/fm-systems-tutorial/tutorial_general_01.png
[2]: ./media/fm-systems-tutorial/tutorial_general_02.png
[3]: ./media/fm-systems-tutorial/tutorial_general_03.png
[4]: ./media/fm-systems-tutorial/tutorial_general_04.png

[100]: ./media/fm-systems-tutorial/tutorial_general_100.png

[200]: ./media/fm-systems-tutorial/tutorial_general_200.png
[201]: ./media/fm-systems-tutorial/tutorial_general_201.png
[202]: ./media/fm-systems-tutorial/tutorial_general_202.png
[203]: ./media/fm-systems-tutorial/tutorial_general_203.png

