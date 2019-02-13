---
title: 'Zelfstudie: Azure Active Directory-integratie met IMPAC risico Manager | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en IMPAC risico Manager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4d77390e-898c-4258-a562-a1181dfe2880
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec119c9e11b21828b1b056fb97598ca7434cfc68
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56197706"
---
# <a name="tutorial-azure-active-directory-integration-with-impac-risk-manager"></a>Zelfstudie: Azure Active Directory-integratie met IMPAC risico Manager

In deze zelfstudie leert u hoe u IMPAC risico Manager integreert met Azure Active Directory (Azure AD).

IMPAC risico Manager integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD wie toegang tot IMPAC risico Manager heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld IMPAC risico Manager (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met IMPAC risico als Manager wilt configureren, moet u de volgende items:

- Een Azure AD-abonnement
- Een IMPAC risico Manager eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. IMPAC risico Manager uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-impac-risk-manager-from-the-gallery"></a>IMPAC risico Manager uit de galerie toe te voegen
Voor het configureren van de integratie van IMPAC risico Manager in Azure AD, moet u IMPAC risico Manager uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen IMPAC risico Manager uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **IMPAC risico Manager**, selecteer **IMPAC risico Manager** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![IMPAC risico Manager in de lijst met resultaten](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met IMPAC risico Manager op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in IMPAC risico Manager is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in IMPAC risico Manager tot stand worden gebracht.

In IMPAC risico Manager, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met IMPAC risico Manager, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker IMPAC risico Manager](#create-a-impac-risk-manager-test-user)**  : als u wilt een equivalent van Britta Simon in IMPAC risico Manager die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing IMPAC risico Manager.

**Azure AD eenmalige aanmelding met IMPAC risico als Manager wilt configureren, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **IMPAC risico Manager** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_samlbase.png)

1. Op de **IMPAC risico Manager-domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de modus voor IDP gestart:

    ![IMPAC risico Manager-domein en URL's, eenmalige aanmelding informatie](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_url_new.png)

    a. In de **id** tekstvak, typ een waarde die is opgegeven door IMPAC

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie:
    | Omgeving | URL-patroon |
    | ---------------|--------------- |    
    | Voor productie |`https://www.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Voor fasering en Training  |`https://staging.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Voor ontwikkeling  |`https://dev.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Voor QA |`https://QA.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | Voor testen |`https://test.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![IMPAC risico Manager-domein en URL's, eenmalige aanmelding informatie](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_url1_new.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    | Omgeving | URL-patroon |
    | ---------------|--------------- |    
    | Voor productie |`https://www.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Voor fasering en Training  |`https://staging.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Voor ontwikkeling  |`https://dev.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Voor QA |`https://QA.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | Voor testen |`https://test.riskmanager.co.nz/SSOv2/<ClientSuffix>`|

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [IMPAC risico Manager-Client-ondersteuningsteam](mailto:rmsupport@Impac.co.nz) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/impacriskmanager-tutorial/tutorial_general_400.png)
    
1. Op de **IMPAC risico Manager configuratie** sectie, klikt u op **configureren IMPAC risico Manager** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML, SAML-entiteit-ID** en **afmelding URL** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_configure.png)

1. Het configureren van eenmalige aanmelding op **IMPAC risico Manager** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64)**, **URL van de afmelding, SAML entiteit-ID,** en  **Single Sign-On Service URL voor SAML** naar [IMPAC risico Manager ondersteuningsteam](mailto:rmsupport@Impac.co.nz). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/impacriskmanager-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/impacriskmanager-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/impacriskmanager-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/impacriskmanager-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-impac-risk-manager-test-user"></a>Maak een testgebruiker IMPAC risico Manager

In deze sectie maakt u een gebruiker met de naam Britta Simon in IMPAC risico Manager. Werken met [IMPAC risico Manager ondersteuningsteam](mailto:rmsupport@Impac.co.nz) om toe te voegen de gebruikers in het platform IMPAC risico Manager. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan IMPAC risico Manager.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar IMPAC risico Manager, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **IMPAC risico Manager**.

    ![De koppeling IMPAC risico Manager in de lijst met toepassingen](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel IMPAC risico Manager in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing IMPAC risico Manager.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/impacriskmanager-tutorial/tutorial_general_01.png
[2]: ./media/impacriskmanager-tutorial/tutorial_general_02.png
[3]: ./media/impacriskmanager-tutorial/tutorial_general_03.png
[4]: ./media/impacriskmanager-tutorial/tutorial_general_04.png

[100]: ./media/impacriskmanager-tutorial/tutorial_general_100.png

[200]: ./media/impacriskmanager-tutorial/tutorial_general_200.png
[201]: ./media/impacriskmanager-tutorial/tutorial_general_201.png
[202]: ./media/impacriskmanager-tutorial/tutorial_general_202.png
[203]: ./media/impacriskmanager-tutorial/tutorial_general_203.png

