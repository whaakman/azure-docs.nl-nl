---
title: 'Zelfstudie: Azure Active Directory-integratie met OpsGenie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en OpsGenie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9858fc38698ae2c5bd272a3494bcf02bce2d8e9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194594"
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Zelfstudie: Azure Active Directory-integratie met OpsGenie

In deze zelfstudie leert u hoe u OpsGenie integreren met Azure Active Directory (Azure AD).

OpsGenie integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot OpsGenie heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij OpsGenie (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met OpsGenie, moet u de volgende items:

- Een Azure AD-abonnement
- Een OpsGenie eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. OpsGenie uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-opsgenie-from-the-gallery"></a>OpsGenie uit de galerie toe te voegen
Voor het configureren van de integratie van OpsGenie in Azure AD, moet u OpsGenie uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen OpsGenie uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **OpsGenie**.

    ![Het maken van een Azure AD-testgebruiker](./media/opsgenie-tutorial/tutorial_opsgenie_search.png)

1. Selecteer in het deelvenster resultaten **OpsGenie**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met OpsGenie op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in OpsGenie is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in OpsGenie tot stand worden gebracht.

In OpsGenie, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met OpsGenie, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker OpsGenie](#creating-a-opsgenie-test-user)**  : als u wilt een equivalent van Britta Simon in OpsGenie die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing OpsGenie.

**Azure AD eenmalige aanmelding configureren met OpsGenie, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **OpsGenie** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

1. Op de **OpsGenie domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/opsgenie-tutorial/tutorial_opsgenie_url.png)

    In de **aanmeldings-URL** tekstvak typt u de URL: `https://app.opsgenie.com/auth/login`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De link om het certificaat te downloaden](./media/opsgenie-tutorial/tutorial_opsgenie_certificate.png)

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/opsgenie-tutorial/tutorial_general_400.png)

1. Op de **OpsGenie configuratie** sectie, klikt u op **configureren OpsGenie** openen **aanmelding configureren** venster. Kopieer de **Single Sign-On Service URL voor SAML** uit de sectie Naslaggids.

    ![Eenmalige aanmelding configureren](./media/opsgenie-tutorial/tutorial_opsgenie_configure.png)

1. Open een ander browserexemplaar, en vervolgens Meld u aan OpsGenie als beheerder.

1. Klik op **instellingen**, en klik vervolgens op de **Single Sign On** tabblad.
   
    ![OpsGenie Single Sign-On](./media/opsgenie-tutorial/tutorial_opsgenie_06.png)

1. Als u eenmalige aanmelding, schakelt **ingeschakeld**.
   
    ![Instellingen voor OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_07.png) 

1. In de **Provider** sectie, klikt u op de **Azure Active Directory** tabblad.
   
    ![Instellingen voor OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_08.png) 

1. Voer de volgende stappen uit op de pagina van het dialoogvenster Azure Active Directory:
   
    ![Instellingen voor OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    a. In de **SAML 2.0-eindpunt** tekstvak plakken **eenmalige aanmelding op Service-URL**waarde die u hebt gekopieerd vanuit Azure portal.
    
    b. In de **metagegevens-Url:** tekstvak plakken **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd vanuit Azure portal.
    
    c. Klik op **Wijzigingen opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/opsgenie-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/opsgenie-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/opsgenie-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/opsgenie-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-opsgenie-test-user"></a>Het maken van een testgebruiker OpsGenie

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in OpsGenie. 

1. In een browservenster, meld u aan bij uw tenant OpsGenie als beheerder.

1. Navigeer naar de lijst met gebruikers door te klikken op **gebruiker** in het linkerdeelvenster.
   
   ![Instellingen voor OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_10.png) 

1. Klik op **Add User**.

1. Op de **gebruiker toevoegen** dialoogvenster, voer de volgende stappen uit:
   
   ![Instellingen voor OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   a. In de **e** tekstvak, typ het e-mailadres van BrittaSimon behandeld in Azure Active Directory.
   
   b. In de **volledige naam** tekstvak, type **Britta Simon**.
   
   c. Klik op **Opslaan**. 

>[!NOTE]
>Julia ontvangt een e-mail met instructies voor het instellen van haar profiel.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan OpsGenie.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan OpsGenie toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **OpsGenie**.

    ![Eenmalige aanmelding configureren](./media/opsgenie-tutorial/tutorial_opsgenie_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel OpsGenie in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing OpsGenie.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/opsgenie-tutorial/tutorial_general_203.png

