---
title: 'Zelfstudie: Azure Active Directory-integratie met iWellnessNow | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en iWellnessNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24ffc841-7a77-481c-9cc4-6f8bda58fe66
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: c260b32dc6f659ca4cc1b4c3f59859f75ba999d0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447370"
---
# <a name="tutorial-azure-active-directory-integration-with-iwellnessnow"></a>Zelfstudie: Azure Active Directory-integratie met iWellnessNow

In deze zelfstudie leert u hoe u iWellnessNow integreren met Azure Active Directory (Azure AD).

IWellnessNow integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot iWellnessNow heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij iWellnessNow (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iWellnessNow, moet u de volgende items:

- Een Azure AD-abonnement
- Een iWellnessNow eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. IWellnessNow uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-iwellnessnow-from-the-gallery"></a>IWellnessNow uit de galerie toe te voegen
Voor het configureren van de integratie van iWellnessNow in Azure AD, moet u iWellnessNow uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iWellnessNow uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **iWellnessNow**, selecteer **iWellnessNow** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![iWellnessNow in de lijst met resultaten](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met iWellnessNow op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in iWellnessNow is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in iWellnessNow tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met iWellnessNow, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker iWellnessNow](#create-an-iwellnessnow-test-user)**  : als u wilt een equivalent van Britta Simon in iWellnessNow die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing iWellnessNow.

**Voor het configureren van Azure AD eenmalige aanmelding met iWellnessNow, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **iWellnessNow** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_samlbase.png)

1. Op de **iWellnessNow domein en URL's** sectie, hebt u **Service Provider-bestand met metagegevens** en wilt configureren van de toepassing in **IDP** modus gestart uitvoeren van de volgende stappen uit:

    ![uploaden van iWellnessNow domein en URL's eenmalige aanmelding](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_upload.png)

    a. Klik op **metagegevensbestand uploaden**.

    ![uploadconfig iWellnessNow domein en URL's eenmalige aanmelding](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_uploadconfig.png)

    b. Klik op **map logo** voor het selecteren van het bestand met metagegevens en klikt u op **uploaden**.
    
    c. Na voltooiing van het uploaden van **Service Provider-bestand met metagegevens** de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld  **iWellnessNow domein en URL's** sectie tekstvak, zoals hieronder weergegeven:

    ![iWellnessNow domein en URL's één aanmeldings-informatie](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url3.png)

1. Als u geen **Service Provider-bestand met metagegevens** en wilt configureren van de toepassing in **IDP** gestart modus, voert u de volgende stappen uit:

    ![iWellnessNow domein en URL's één aanmeldings-informatie](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `http://<CustomerName>.iwellnessnow.com`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<CustomerName>.iwellnessnow.com/ssologin`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![iWellnessNow domein en URL's één aanmeldings-informatie](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<CustomerName>.iwellnessnow.com/`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [iWellnessNow Client ondersteuningsteam](mailto:info@iwellnessnow.com) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/iwellnessnow-tutorial/tutorial_general_400.png)
    
1. Het configureren van eenmalige aanmelding op **iWellnessNow** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [iWellnessNow ondersteuningsteam](mailto:info@iwellnessnow.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/iwellnessnow-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/iwellnessnow-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/iwellnessnow-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/iwellnessnow-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-iwellnessnow-test-user"></a>Maak een testgebruiker iWellnessNow

In deze sectie maakt u een gebruiker met de naam van Britta Simon in iWellnessNow. Werken met [iWellnessNow ondersteuningsteam](mailto:info@iwellnessnow.com) om toe te voegen de gebruikers in het iWellnessNow-platform. Gebruikers moeten worden gemaakt en geactiveerd voordat u eenmalige aanmelding gebruiken

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot iWellnessNow.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan iWellnessNow toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **iWellnessNow**.

    ![De koppeling iWellnessNow in de lijst met toepassingen](./media/iwellnessnow-tutorial/tutorial_iwellnessnow_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel iWellnessNow in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing iWellnessNow.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/iwellnessnow-tutorial/tutorial_general_01.png
[2]: ./media/iwellnessnow-tutorial/tutorial_general_02.png
[3]: ./media/iwellnessnow-tutorial/tutorial_general_03.png
[4]: ./media/iwellnessnow-tutorial/tutorial_general_04.png

[100]: ./media/iwellnessnow-tutorial/tutorial_general_100.png

[200]: ./media/iwellnessnow-tutorial/tutorial_general_200.png
[201]: ./media/iwellnessnow-tutorial/tutorial_general_201.png
[202]: ./media/iwellnessnow-tutorial/tutorial_general_202.png
[203]: ./media/iwellnessnow-tutorial/tutorial_general_203.png

