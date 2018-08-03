---
title: 'Zelfstudie: Azure Active Directory-integratie met Menlo beveiliging | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Menlo beveiliging.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: a1f7458d52ffdee4cb48e4be0f553e3d57413249
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428842"
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a>Zelfstudie: Azure Active Directory-integratie met Menlo beveiliging

In deze zelfstudie leert u hoe u Menlo beveiliging integreren met Azure Active Directory (Azure AD).

Menlo beveiliging integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Menlo beveiliging heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de beveiliging Menlo (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, raadpleegt u. [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Menlo beveiliging, moet u de volgende items:

- Een Azure AD-abonnement
- Een Menlo Security eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Menlo beveiliging toe te voegen uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-menlo-security-from-the-gallery"></a>Menlo beveiliging toe te voegen uit de galerie
Voor het configureren van de integratie van Menlo beveiliging in Azure AD, moet u Menlo beveiliging uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Menlo beveiliging uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **Menlo Security**.

    ![Het maken van een Azure AD-testgebruiker](./media/menlosecurity-tutorial/tutorial_menlosecurity_search.png)

1. Selecteer in het deelvenster resultaten **Menlo Security**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Menlo beveiliging op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Menlo Security is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Menlo beveiliging tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Menlo Security.

Om te configureren en testen van Azure AD eenmalige aanmelding met Menlo beveiliging, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Menlo Security](#creating-a-menlo-security-test-user)**  : als u wilt hebben van een equivalent van Britta Simon in Menlo Security die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Menlo beveiliging.

**Voor het configureren van Azure AD eenmalige aanmelding met Menlo beveiliging, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Menlo Security** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

1. Op de **Menlo Security domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.menlosecurity.com/account/login`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Menlo Beveiligingsclient ondersteuningsteam](https://www.menlosecurity.com/menlo-contact) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/menlosecurity-tutorial/tutorial_general_400.png)

1. Op de **Menlo Beveiligingsconfiguratie** sectie, klikt u op **Configure Menlo Security** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID**, en **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

1. Het configureren van eenmalige aanmelding op **Menlo Security** side, meld u aan bij de **Menlo Security** website als beheerder.

1. Onder **instellingen** Ga naar **verificatie** en de volgende acties uitvoeren:
    
    ![Eenmalige aanmelding configureren](./media/menlosecurity-tutorial/menlo_user_setup.png)

    a. Vink het selectievakje **inschakelen gebruikersverificatie via SAML**.

    b. Selecteer **toestaan van externe toegang** naar **Ja**.

    c. Onder **SAML-Provider**, selecteer **Azure Active Directory**.

    d. **SAML 2.0-eindpunt** : plak de **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    e. **Service-id (verlener)** : plak de **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.

    f. **X.509-certificaat** : Open de **certificaat (Base64)** gedownload vanuit de Azure-Portal in Kladblok en plak deze in dit vak.

    g. Klik op **Opslaan** om de instellingen op te slaan.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/menlosecurity-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/menlosecurity-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/menlosecurity-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/menlosecurity-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-menlo-security-test-user"></a>Het maken van een testgebruiker Menlo beveiliging
 
In deze sectie maakt u een gebruiker met de naam van Britta Simon in Menlo Security. Werken met [Menlo Beveiligingsclient ondersteuningsteam](https://www.menlosecurity.com/menlo-contact) om toe te voegen de gebruikers in het platform voor Menlo beveiliging. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Menlo beveiliging.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Menlo beveiliging, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Menlo Security**.

    ![Eenmalige aanmelding configureren](./media/menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw configuratie Azure AD eenmalige aanmelding.

Open een browservenster in de modus 'InPrivate' of 'Incognito' voor het activeren van een nieuwe verificatie.  Gebruik Ctrl + Shift + P in Internet Explorer.  In Chrome, gebruikt u Ctrl + Shift + N.  Blader naar een beveiligde bron in het persoonlijke gebruik van browser venster en uitvoeren van een Azure AD-aanmelding.  Bij geslaagde aanmelding gaat u naar de gewenste site in een sessie isolatie.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/menlosecurity-tutorial/tutorial_general_203.png

