---
title: 'Zelfstudie: Azure Active Directory-integratie met bepaalde Admin SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en bepaalde Admin SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98ba0174-be02-408a-8634-c8113b12dedb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeedes
ms.openlocfilehash: cf23620eb3a3b0a9cb8e5877f517281a9c21809c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320616"
---
# <a name="tutorial-azure-active-directory-integration-with-certain-admin-sso"></a>Zelfstudie: Azure Active Directory-integratie met bepaalde Admin SSO

In deze zelfstudie leert u hoe bepaalde Admin SSO integreren met Azure Active Directory (Azure AD).

Bepaalde Admin SSO integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot bepaalde Admin eenmalige aanmelding heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij bepaalde Admin SSO (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met bepaalde Admin SSO, moet u de volgende items:

- Een Azure AD-abonnement
- Een bepaalde Admin SSO eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Bepaalde Admin SSO uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-certain-admin-sso-from-the-gallery"></a>Bepaalde Admin SSO uit de galerie toevoegen
Voor het configureren van de integratie van bepaalde Admin SSO in Azure AD, moet u bepaalde Admin SSO uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen bepaalde Admin SSO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **bepaalde Admin SSO**, selecteer **bepaalde Admin SSO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Bepaalde SSO Admin in de lijst met resultaten](./media/certainadminsso-tutorial/tutorial_certainadminsso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met bepaalde Admin eenmalige aanmelding op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in bepaalde Admin eenmalige aanmelding is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in bepaalde Admin eenmalige aanmelding tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met bepaalde Admin SSO, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een bepaalde Admin SSO testgebruiker](#create-a-certain-admin-sso-test-user)**  - hebben een equivalent van Britta Simon in bepaalde Admin eenmalige aanmelding die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing bepaalde Admin eenmalige aanmelding.

**Voor het configureren van Azure AD eenmalige aanmelding met bepaalde Admin SSO, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **bepaalde Admin SSO** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/certainadminsso-tutorial/tutorial_certainadminsso_samlbase.png)

3. Op de **domein van de SSO-bepaalde beheer- en URL's** sectie, voert u de volgende stappen uit:

    ![Bepaalde domein van de SSO-beheer en de URL's van eenmalige aanmelding informatie](./media/certainadminsso-tutorial/tutorial_certainadminsso_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<YOUR DOMAIN URL>/svcs/sso_admin_login/handleRequest/<ID>`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<SUBDOMAIN>.certain.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [bepaalde Beheerclient SSO-ondersteuningsteam](mailto:integrations@certain.com) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Raw)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/certainadminsso-tutorial/tutorial_certainadminsso_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/certainadminsso-tutorial/tutorial_general_400.png)

6. Op de **bepaalde SSO beheerdersconfiguratie** sectie, klikt u op **bepaalde Admin SSO configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Bepaalde Admin SSO-configuratie](./media/certainadminsso-tutorial/tutorial_certainadminsso_configure.png) 

7. Eenmalige aanmelding configureren op **bepaalde Admin SSO** zijde, moet u de gedownloade verzenden **certificaat (Raw)**, **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL**naar [bepaalde Admin SSO-ondersteuningsteam](mailto:integrations@certain.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/certainadminsso-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/certainadminsso-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/certainadminsso-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/certainadminsso-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-certain-admin-sso-test-user"></a>Een bepaalde Admin SSO testgebruiker maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in bepaalde Admin SSO maken. Werken met [bepaalde Admin SSO-ondersteuningsteam](mailto:integrations@certain.com) toevoegen van de gebruikers in de bepaalde Admin SSO-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan bepaalde Admin eenmalige aanmelding.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon aan bepaalde Admin SSO, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **bepaalde Admin SSO**.

    ![De bepaalde Admin SSO-koppeling in de lijst met toepassingen](./media/certainadminsso-tutorial/tutorial_certainadminsso_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel bepaalde Admin SSO in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing bepaalde Admin eenmalige aanmelding.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/certainadminsso-tutorial/tutorial_general_01.png
[2]: ./media/certainadminsso-tutorial/tutorial_general_02.png
[3]: ./media/certainadminsso-tutorial/tutorial_general_03.png
[4]: ./media/certainadminsso-tutorial/tutorial_general_04.png

[100]: ./media/certainadminsso-tutorial/tutorial_general_100.png

[200]: ./media/certainadminsso-tutorial/tutorial_general_200.png
[201]: ./media/certainadminsso-tutorial/tutorial_general_201.png
[202]: ./media/certainadminsso-tutorial/tutorial_general_202.png
[203]: ./media/certainadminsso-tutorial/tutorial_general_203.png

