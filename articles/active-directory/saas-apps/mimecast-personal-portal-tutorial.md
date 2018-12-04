---
title: 'Zelfstudie: Azure Active Directory-integratie met de persoonlijke Portal Mimecast | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en persoonlijke Mimecast-Portal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: jeedes
ms.openlocfilehash: 88952d09672fb4dd2d31878d56a59cb425f53de2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850445"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Zelfstudie: Azure Active Directory-integratie met persoonlijke Mimecast-Portal

In deze zelfstudie leert u hoe u persoonlijke Portal Mimecast integreren met Azure Active Directory (Azure AD).

Persoonlijke Portal Mimecast integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de persoonlijke Mimecast-Portal heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Mimecast persoonlijke Portal (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met persoonlijke Mimecast-Portal, moet u de volgende items:

- Een Azure AD-abonnement
- Een persoonlijke Portal Mimecast eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Persoonlijke Portal Mimecast uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Persoonlijke Portal Mimecast uit de galerie toe te voegen
Voor het configureren van de integratie van Mimecast persoonlijke Portal in Azure AD, moet u Mimecast persoonlijke Portal vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Mimecast persoonlijke Portal vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Mimecast persoonlijke Portal**, selecteer **Mimecast persoonlijke Portal** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Mimecast persoonlijke Portal in de lijst met resultaten](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met persoonlijke Mimecast-Portal op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de persoonlijke Portal Mimecast is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Mimecast persoonlijke Portal tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met persoonlijke Mimecast-Portal, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Mimecast persoonlijke Portal](#create-a-mimecast-personal-portal-test-user)**  : als u wilt een equivalent van Britta Simon in Mimecast persoonlijke Portal die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Mimecast persoonlijke Portal.

**Voor het configureren van Azure AD eenmalige aanmelding met persoonlijke Mimecast-Portal, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Mimecast persoonlijke Portal** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

1. Op de **Mimecast persoonlijke Portal domein en URL's** sectie, voert u de volgende stappen uit:

    ![Mimecast persoonlijke Portal domein en URL's, eenmalige aanmelding informatie](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL typen: 

    | Regio  |  Waarde | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Verenigde Staten   | `https://us-api.mimecast.com/login/saml`|
    | Zuid-Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Australië       | `https://au-api.mimecast.com/login/saml`|
    | Offshore        | `https://jer-api.mimecast.com/login/saml`|

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon:

    | Regio  |  Waarde | 
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Verenigde Staten   | `https://us-api.mimecast.com/sso/<accountcode>`|    
    | Zuid-Afrika    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Australië       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Offshore        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    c. In de **antwoord-URL** tekstvak, een URL typen: 

    | Regio  |  Waarde | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Verenigde Staten   | `https://us-api.mimecast.com/login/saml`|
    | Zuid-Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Australië       | `https://au-api.mimecast.com/login/saml`|
    | Offshore        | `https://jer-api.mimecast.com/login/saml`|
    
    > [!NOTE] 
    > De id-waarde is niet echt. Werk de waarde bij met de werkelijke-id. Neem contact op met [Mimecast persoonlijke Portal Client ondersteuningsteam](https://www.mimecast.com/customer-success/technical-support/) om de waarde. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/mimecast-personal-portal-tutorial/tutorial_general_400.png)

1. Op de **Mimecast de persoonlijke configuratie** sectie, klikt u op **Mimecast persoonlijke Portal configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Mimecast persoonlijke Portalconfiguratie](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

1. In een ander browservenster aanmelden bij uw persoonlijke Mimecast-Portal als beheerder.

1. Ga naar **Services \> toepassingen**.
   
    ![Toepassingen](./media/mimecast-personal-portal-tutorial/ic794998.png "toepassingen")

1. Klik op **verificatie profielen**.
   
    ![Verificatie-profielen](./media/mimecast-personal-portal-tutorial/ic794999.png "verificatie-profielen")

1. Klik op **nieuwe verificatie-profiel**.
   
    ![Nieuwe verificatie-profiel](./media/mimecast-personal-portal-tutorial/ic795000.png "nieuwe verificatie-profiel")

1. In de **verificatie profiel** sectie, voert u de volgende stappen uit:
   
    ![Verificatie-profiel](./media/mimecast-personal-portal-tutorial/ic795001.png "verificatie-profiel")
   
    a. In de **beschrijving** tekstvak, typ een naam voor uw configuratie.
   
    b. Selecteer **afdwingen van SAML-verificatie voor Mimecast persoonlijke Portal**.
   
    c. Als **Provider**, selecteer **Azure Active Directory**.
   
    d. In **URL-verlener** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.
   
    e. In **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.
   
    f. In **afmeldings-URL van** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    g. Open uw **base-64** gecodeerd certificaat in Kladblok gedownload vanuit Azure portal, de inhoud van het kopiëren naar het Klembord en plakt u deze naar de **id-Provider-certificaat (metagegevens)** tekstvak.

    h. Selecteer **eenmalige aanmelding op toestaan**.
   
    i. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/mimecast-personal-portal-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/mimecast-personal-portal-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/mimecast-personal-portal-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/mimecast-personal-portal-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-mimecast-personal-portal-test-user"></a>Maak een testgebruiker Mimecast persoonlijke Portal

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij Mimecast persoonlijke Portal, moeten ze worden ingericht voor persoonlijke Mimecast-Portal. In het geval van persoonlijke Mimecast-Portal is inrichten een handmatige taak.

U moet een domein registreren voordat u gebruikers kunt maken.

**Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Mimecast persoonlijke Portal** als administrator.

1. Ga naar **mappen \> interne**.
   
    ![Mappen](./media/mimecast-personal-portal-tutorial/ic795003.png "mappen")

1. Klik op **nieuw domein registreren**.
   
    ![Nieuwe domein registreren](./media/mimecast-personal-portal-tutorial/ic795004.png "nieuw domein registreren")

1. Nadat het nieuwe domein is gemaakt, klikt u op **nieuw adres**.
   
    ![Nieuw adres](./media/mimecast-personal-portal-tutorial/ic795005.png "nieuw adres")

1. In het dialoogvenster Nieuw adres Voer de volgende stappen uit van een geldige Azure AD-account die u inrichten wilt:
   
    ![Sla](./media/mimecast-personal-portal-tutorial/ic795006.png "opslaan")
   
    a. In de **e-mailadres** tekstvak, type **e-mailadres** van de gebruiker als **BrittaSimon@contoso.com**.
    
    b. In de **globale naam** tekstvak, type de **gebruikersnaam** als **BrittaSimon**.

    c. In de **wachtwoord**, en **wachtwoord bevestigen** tekstvakken, type de **wachtwoord** van de gebruiker.
   
    b. Klik op **Opslaan**.

>[!NOTE]
>U kunt een andere hulpprogramma's voor persoonlijke Portal Mimecast gebruiker-account maken of API's die worden geleverd door Mimecast persoonlijke Portal gebruiken voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot de persoonlijke Portal Mimecast.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Mimecast persoonlijke portal, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Mimecast persoonlijke Portal**.

    ![De koppeling Mimecast persoonlijke Portal in de lijst met toepassingen](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Mimecast persoonlijke Portal in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Mimecast persoonlijke Portal.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/mimecast-personal-portal-tutorial/tutorial_general_203.png

