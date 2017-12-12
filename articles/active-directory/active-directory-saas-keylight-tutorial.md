---
title: 'Zelfstudie: Azure Active Directory-integratie met LockPath Keylight | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LockPath Keylight.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 74d9d80a1ce35f1766261038a985cc6f0cc39fd3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Zelfstudie: Azure Active Directory-integratie met LockPath Keylight

In deze zelfstudie leert u hoe LockPath Keylight integreren met Azure Active Directory (Azure AD).

LockPath Keylight integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LockPath Keylight heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LockPath Keylight (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LockPath Keylight, moet u de volgende items:

- Een Azure AD-abonnement
- Een LockPath Keylight eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LockPath Keylight uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-lockpath-keylight-from-the-gallery"></a>LockPath Keylight uit de galerie toevoegen
Voor het configureren van de integratie van LockPath Keylight in Azure AD, moet u LockPath Keylight uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LockPath Keylight uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **LockPath Keylight**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_search.png)

5. Selecteer in het deelvenster resultaten **LockPath Keylight**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en test eenmalige aanmelding Azure AD met LockPath Keylight op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in LockPath Keylight is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in LockPath Keylight tot stand worden gebracht.

Wijs in LockPath Keylight, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met LockPath Keylight, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker LockPath Keylight](#creating-a-lockpath-keylight-test-user)**  - LockPath Keylight die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing LockPath Keylight configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met LockPath Keylight, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **LockPath Keylight** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_samlbase.png)

3. Op de **LockPath Keylight domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.keylightgrc.com/`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.keylightgrc.com`

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.keylightgrc.com/Login.aspx`
    
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [LockPath Keylight Client ondersteuningsteam](https://www.lockpath.com/contact/) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Raw)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_general_400.png)
    
6. Op de **LockPath Keylight configuratie** sectie, klikt u op **configureren LockPath Keylight** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_configure.png) 

7. Om SSO in LockPath Keylight, moet u de volgende stappen uitvoeren:
   
    a. Aanmelding bij je account LockPath Keylight als administrator.
    
    b. Klik in het menu bovenaan op **persoon**, en selecteer **Keylight Setup**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. Klik in de structuurweergave links **SAML**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. Op de **SAML instellingen** dialoogvenster, klikt u op **bewerken**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/404.png) 

8. Op de **SAML-instellingen bewerken** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    a. Stel **SAML-verificatie** naar **Active**.

    b. Plak de **SAML Single Sign-On Service-URL** waarde die u hebt gekopieerd vanuit de Azure-portal in de **identiteit Provider aanmeldings-URL** textbox.

    c. Plak de **Service-URL met eenmalige Sign-Out** waarde die u hebt gekopieerd vanuit de Azure-portal in de **identiteit Provider afmelding URL** textbox.

    d. Klik op **bestand kiezen** uw gedownloade LockPath Keylight certificaat selecteren en klik vervolgens op **Open** om het certificaat te uploaden.

    e. Ingesteld **SAML-gebruikersnaam locatie** naar **NameIdentifier-element van het onderwerp overzicht**.
    
    f. Geef de **Keylight serviceprovider** met het volgende patroon volgen: **https://&lt;NaamBedrijf&gt;. keylightgrc.com**.
    
    g. Stel **automatisch inrichten gebruikers** naar **Active**.

    h. Stel **automatisch inrichten accounttype** naar **volledige gebruiker**.

    ik. Stel **automatisch inrichten beveiligingsrol**, selecteer **standaardgebruiker met SAML**.
    
    j. Stel **automatisch inrichten beveiliging config**, selecteer **standaardconfiguratie van de gebruiker**.
     
    k. In de **e kenmerk** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    l. In de **voornaam kenmerk** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    m. In de **laatste naamkenmerk** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
    
    n. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-lockpath-keylight-test-user"></a>Een testgebruiker LockPath Keylight maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in LockPath Keylight maken. LockPath Keylight ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt bij het openen van LockPath Keylight als de gebruiker nog niet bestaat. 

>[!NOTE]
>Als u een gebruiker handmatig maken wilt, moet u contact op met de [LockPath Keylight Client ondersteuningsteam](https://www.lockpath.com/contact/). 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan LockPath Keylight.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen LockPath Keylight, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **LockPath Keylight**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel LockPath Keylight in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing LockPath Keylight. 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png

