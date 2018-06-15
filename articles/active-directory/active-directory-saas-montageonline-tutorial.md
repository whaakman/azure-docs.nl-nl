---
title: 'Zelfstudie: Azure Active Directory-integratie met Montage Online | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding van Azure Active Directory en Montage Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5c5e8c6f-e4fb-43fe-8841-e371f568ebed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeedes
ms.openlocfilehash: e5cabbd26c978bc8bcdabeadfca31ae99c951558
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34343000"
---
# <a name="tutorial-azure-active-directory-integration-with-montage-online"></a>Zelfstudie: Azure Active Directory-integratie met Montage Online

In deze zelfstudie leert u hoe Montage Online integreren met Azure Active Directory (Azure AD).

Online Montage integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Montage Online heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Montage Online (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Montage Online, moet u de volgende items:

- Een Azure AD-abonnement
- Een Montage Online eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van Online Montage uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-montage-online-from-the-gallery"></a>Toevoegen van Online Montage uit de galerie
Voor het configureren van de integratie van Online Montage in Azure AD, moet u Montage Online uit de galerie toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen in de galerie Montage Online, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Montage Online**, selecteer **Montage Online** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Montage Online in de lijst met resultaten](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Montage Online op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Montage Online is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker Montage online worden gemaakt.

Om te configureren en testen van Azure AD eenmalige aanmelding met Montage Online, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een Montage Online testgebruiker](#create-a-montage-online-test-user)**  - bevatten een equivalent van Britta Simon Montage Online die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Montage Online configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Montage Online, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Montage Online** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_samlbase.png)

3. Op de **Montage Online domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Montage Online domein en één URL's aanmelding informatie](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:

    Voor de productie-omgeving: `https://<subdomain>.montageonline.co.nz/`

    Voor testomgeving: `https://build-<subdomain>.montageonline.co.nz/`

    b. In de **id** textbox, typ een URL:

    Voor de productie-omgeving: `MOL_Azure`

    Voor testomgeving: `MOL_Azure_Build`

    > [!NOTE] 
    > De waarde van de aanmeldings-URL is geen echte. Werk de waarde met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Montage Online Client ondersteuningsteam](https://www.montage.co.nz/contact-us/) de waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-montageonline-tutorial/tutorial_general_400.png)

6. Op de **Montage Online configuratie** sectie, klikt u op **Montage Online configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Montage Online configureren](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_configure.png) 

7. Eenmalige aanmelding configureren op **Montage Online** zijde, moet u de gedownloade verzenden **certificaat (Base64), Sign-Out URL SAML entiteit-ID en SAML Single Sign-On Service-URL** naar [Montage Online ondersteuningsteam](https://www.montage.co.nz/contact-us/). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-montageonline-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-montageonline-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-montageonline-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-montageonline-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-montage-online-test-user"></a>Een Montage Online testgebruiker maken

In deze sectie maakt maken u een gebruiker die is aangeroepen terwijl Britta Simon Montage Online. Werken met [Montage Online ondersteuningsteam](https://www.montage.co.nz/contact-us/) toevoegen van de gebruikers in het Online Montage platform. Gebruikers moeten worden gemaakt en geactiveerd voordat u eenmalige aanmelding gebruiken

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verleent tot Montage Online.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Montage Online, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Montage Online**.

    ![De Montage Online koppeling in de lijst met toepassingen](./media/active-directory-saas-montageonline-tutorial/tutorial_montageonline_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Online Montage in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Montage Online-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-montageonline-tutorial/tutorial_general_203.png

