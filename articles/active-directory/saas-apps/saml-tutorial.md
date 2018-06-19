---
title: 'Zelfstudie: Azure Active Directory-integratie met SAML 1.1 Token ingeschakeld LOB-App | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAML 1.1 Token ingeschakeld LOB-App.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ced1d88d-0e48-40d5-9aea-ef991cd9d270
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: jeedes
ms.openlocfilehash: c5a806bb586ceeb4ce68774a1b5d874dfc1e4f26
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35907157"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-11-token-enabled-lob-app"></a>Zelfstudie: Azure Active Directory-integratie met SAML 1.1 Token ingeschakeld LOB-App

In deze zelfstudie leert u hoe integreren SAML 1.1 Token LOB-App met Azure Active Directory (Azure AD) ingeschakeld.

Integratie van SAML 1.1 Token biedt ingeschakelde LOB-App met Azure AD de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAML 1.1 Token heeft ingeschakelde LOB-App.
- U kunt uw gebruikers automatisch aangemeld bij inschakelen SAML 1.1 Token LOB-App (Single Sign-On) ingeschakeld voor accounts die hun Azure AD.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAML 1.1 Token LOB-App ingeschakeld, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAML 1.1 Token LOB-App eenmalige aanmelding ingeschakeld abonnement ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LOB-App uit de galerie toe te voegen SAML 1.1 Token ingeschakeld
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-saml-11-token-enabled-lob-app-from-the-gallery"></a>LOB-App uit de galerie toe te voegen SAML 1.1 Token ingeschakeld
Ingeschakeld voor het configureren van de integratie van SAML 1.1 Token LOB-App met Azure AD, moet u om toe te voegen dat SAML 1.1 Token ingeschakeld LOB-App uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen dat SAML 1.1 Token LOB-App uit de galerie is ingeschakeld, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SAML 1.1 Token LOB-App ingeschakeld**, selecteer **SAML 1.1 Token LOB-App ingeschakeld** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SAML 1.1 Token ingeschakeld LOB-App in de lijst met resultaten](./media/saml-tutorial/tutorial_saml_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SAML 1.1 Token ingeschakelde LOB-App op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de equivalente gebruiker in SAML 1.1 Token ingeschakelde LOB-App aan een gebruiker in Azure AD is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SAML 1.1 Token ingeschakeld LOB-App moet worden gemaakt.

Configureren en testen van Azure AD eenmalige aanmelding met SAML 1.1 Token LOB-App ingeschakeld, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SAML 1.1-Token ingeschakeld testgebruiker LOB-App](#create-a-saml-11-token-enabled-lob-app-test-user)**  - hebben een equivalent van Britta Simon in SAML 1.1 Token LOB-App die is gekoppeld aan de Azure AD-weergave van de gebruiker ingeschakeld.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie kunt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw SAML 1.1 Token-App LOB-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met SAML 1.1 Token ingeschakeld LOB-App, voer de volgende stappen uit:**

1. In de Azure-portal op de **SAML 1.1 Token LOB-App ingeschakeld** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/saml-tutorial/tutorial_saml_samlbase.png)

3. Op de **SAML 1.1 Token ingeschakeld LOB-App-Domain en URL's** sectie, voert u de volgende stappen uit:

    ![SAML 1.1 Token LOB-App-Domain en URL's eenmalige aanmelding informatie ingeschakeld](./media/saml-tutorial/tutorial_saml_url.png)

    a. In de **aanmelden URL** textbox, typ een URL met het volgende patroon volgen: `https://your-app-url`

    b. In de **id (entiteit-ID)** textbox, typ een URL met het volgende patroon volgen: `https://your-app-url`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Vervang deze waarden met specifieke URL's van toepassing.  

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/saml-tutorial/tutorial_saml_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/saml-tutorial/tutorial_general_400.png)
    
7. Op de **SAML 1.1 Token ingeschakeld LOB-App-configuratie** sectie, klikt u op **SAML 1.1 Token configureren LOB-App ingeschakeld** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![SAML 1.1 Token ingeschakeld LOB-App-configuratie](./media/saml-tutorial/tutorial_saml_configure.png) 

8. Eenmalige aanmelding configureren op **SAML 1.1 Token LOB-App ingeschakeld** zijde, moet u de gedownloade verzenden **certificaat (Base64), Sign-Out URL SAML entiteit-ID en SAML Single Sign-On Service-URL** naar toepassing ondersteuningsteam. Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/saml-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/saml-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/saml-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/saml-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-saml-11-token-enabled-lob-app-test-user"></a>Maak een SAML 1.1-Token ingeschakeld testgebruiker LOB-App

In deze sectie maakt u een gebruiker die is aangeroepen terwijl Britta Simon maken SAML 1.1 Token ingeschakelde LOB-App. Werken met de toepassing ondersteuningsteam maken van gebruiker aan de toepassing. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie, schakelt u Britta Simon Azure gebruiken eenmalige aanmelding door het verlenen van toegang aan SAML 1.1 Token LOB-App ingeschakeld.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen SAML 1.1 Token ingeschakeld LOB-App, voer de volgende stappen uit:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAML 1.1 Token LOB-App ingeschakeld**.

    ![De SAML 1.1 Token koppeling LOB-App in de lijst met toepassingen ingeschakeld](./media/saml-tutorial/tutorial_saml_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u klikt op het SAML 1.1 Token ingeschakeld LOB-App-tegel in het deelvenster toegang, krijgt u automatisch aangemeld bij uw SAML 1.1 Token-App LOB-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/saml-tutorial/tutorial_general_01.png
[2]: ./media/saml-tutorial/tutorial_general_02.png
[3]: ./media/saml-tutorial/tutorial_general_03.png
[4]: ./media/saml-tutorial/tutorial_general_04.png

[100]: ./media/saml-tutorial/tutorial_general_100.png

[200]: ./media/saml-tutorial/tutorial_general_200.png
[201]: ./media/saml-tutorial/tutorial_general_201.png
[202]: ./media/saml-tutorial/tutorial_general_202.png
[203]: ./media/saml-tutorial/tutorial_general_203.png

