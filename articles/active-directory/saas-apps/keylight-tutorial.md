---
title: 'Zelfstudie: Azure Active Directory-integratie met LockPath Keylight | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LockPath Keylight.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 18fbcc785491ca8a0631f54750412bc0f12254c1
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421444"
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Zelfstudie: Azure Active Directory-integratie met LockPath Keylight

In deze zelfstudie leert u hoe u LockPath Keylight integreren met Azure Active Directory (Azure AD).

LockPath Keylight integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LockPath Keylight heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LockPath Keylight (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LockPath Keylight, moet u de volgende items:

- Een Azure AD-abonnement
- Een LockPath Keylight eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LockPath Keylight uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-lockpath-keylight-from-the-gallery"></a>LockPath Keylight uit de galerie toe te voegen
Voor het configureren van de integratie van LockPath Keylight in Azure AD, moet u LockPath Keylight uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LockPath Keylight uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **LockPath Keylight**.

    ![Het maken van een Azure AD-testgebruiker](./media/keylight-tutorial/tutorial_keylight_search.png)

1. Selecteer in het deelvenster resultaten **LockPath Keylight**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met LockPath Keylight op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in LockPath Keylight is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in LockPath Keylight tot stand worden gebracht.

In LockPath Keylight, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met LockPath Keylight, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker LockPath Keylight](#creating-a-lockpath-keylight-test-user)**  : als u wilt een equivalent van Britta Simon in LockPath Keylight die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing LockPath Keylight.

**Voor het configureren van Azure AD eenmalige aanmelding met LockPath Keylight, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **LockPath Keylight** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/tutorial_keylight_samlbase.png)

1. Op de **LockPath Keylight domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/tutorial_keylight_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<company name>.keylightgrc.com/`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<company name>.keylightgrc.com`

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<company name>.keylightgrc.com/Login.aspx`
    
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [LockPath Keylight Client ondersteuningsteam](https://www.lockpath.com/contact/) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Raw)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/tutorial_keylight_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/tutorial_general_400.png)
    
1. Op de **LockPath Keylight configuratie** sectie, klikt u op **configureren LockPath Keylight** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/tutorial_keylight_configure.png) 

1. Schakel eenmalige aanmelding in LockPath Keylight door de volgende stappen uitvoeren:
   
    a. Aanmelding bij uw account LockPath Keylight als administrator.
    
    b. Klik in het menu aan de bovenkant op **persoon**, en selecteer **Keylight Setup**.
   
    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/401.png) 

    c. Klik in de structuurweergave aan de linkerkant op **SAML**.
   
    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/402.png) 

    d. Op de **SAML-instellingen** dialoogvenster, klikt u op **bewerken**.
   
    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/404.png) 

1. Op de **SAML-instellingen bewerken** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/405.png) 
   
    a. Stel **SAML-verificatie** naar **Active**.

    b. Plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit de Azure portal in de **aanmeldings-URL van id-Provider** tekstvak.

    c. Plak de **Service-URL voor eenmalige afmelding** waarde die u hebt gekopieerd vanuit de Azure portal in de **afmeldings-URL van id-Provider** tekstvak.

    d. Klik op **bestand kiezen** uw gedownloade LockPath Keylight certificaat selecteren en klik vervolgens op **Open** om het certificaat te uploaden.

    e. Instellen **SAML gebruikers-Id locatie** naar **NameIdentifier-element van het onderwerp overzicht**.
    
    f. Geef de **Keylight serviceprovider** met behulp van het volgende patroon: **https://&lt;CompanyName&gt;. keylightgrc.com**.
    
    g. Stel **automatisch inrichten gebruikers** naar **Active**.

    h. Stel **automatisch inrichten accounttype** naar **volledige gebruiker**.

    i. Stel **beveiligingsrol automatisch inrichten**, selecteer **standaardgebruiker met SAML**.
    
    j. Stel **automatisch inrichten security config**, selecteert **Standard Gebruikersconfiguratie**.
     
    k. In de **e kenmerk** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    l. In de **voornaam kenmerk** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    m. In de **laatste naamkenmerk** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
    
    n. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/keylight-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/keylight-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/keylight-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/keylight-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-lockpath-keylight-test-user"></a>Het maken van een testgebruiker LockPath Keylight

In deze sectie maakt u een gebruiker met de naam van Britta Simon in LockPath Keylight. LockPath Keylight biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt bij het openen van LockPath Keylight als de gebruiker nog niet bestaat. 

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [LockPath Keylight Client ondersteuningsteam](https://www.lockpath.com/contact/). 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan LockPath Keylight.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan LockPath Keylight toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **LockPath Keylight**.

    ![Eenmalige aanmelding configureren](./media/keylight-tutorial/tutorial_keylight_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel LockPath Keylight in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing LockPath Keylight. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/keylight-tutorial/tutorial_general_01.png
[2]: ./media/keylight-tutorial/tutorial_general_02.png
[3]: ./media/keylight-tutorial/tutorial_general_03.png
[4]: ./media/keylight-tutorial/tutorial_general_04.png

[100]: ./media/keylight-tutorial/tutorial_general_100.png

[200]: ./media/keylight-tutorial/tutorial_general_200.png
[201]: ./media/keylight-tutorial/tutorial_general_201.png
[202]: ./media/keylight-tutorial/tutorial_general_202.png
[203]: ./media/keylight-tutorial/tutorial_general_203.png

