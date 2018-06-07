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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 5acaf8141c7136c50f2aed202286d7afbee7ed96
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654885"
---
# <a name="tutorial-azure-active-directory-integration-with-iwellnessnow"></a>Zelfstudie: Azure Active Directory-integratie met iWellnessNow

In deze zelfstudie leert u hoe iWellnessNow integreren met Azure Active Directory (Azure AD).

IWellnessNow integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot iWellnessNow heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij iWellnessNow (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iWellnessNow, moet u de volgende items:

- Een Azure AD-abonnement
- Een iWellnessNow eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. IWellnessNow uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-iwellnessnow-from-the-gallery"></a>IWellnessNow uit de galerie toevoegen
Voor het configureren van de integratie van iWellnessNow in Azure AD, moet u iWellnessNow uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iWellnessNow uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **iWellnessNow**, selecteer **iWellnessNow** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![iWellnessNow in de lijst met resultaten](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_iwellnessnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met iWellnessNow op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in iWellnessNow is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in iWellnessNow tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met iWellnessNow, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker iWellnessNow](#create-an-iwellnessnow-test-user)**  - hebben een equivalent van Britta Simon in iWellnessNow die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing iWellnessNow configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met iWellnessNow, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **iWellnessNow** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_iwellnessnow_samlbase.png)

3. Op de **iWellnessNow domein en de URL's** sectie, hebt u **metagegevensbestand van de serviceprovider** en het configureren van de toepassing in wilt **IDP** geïnitieerd modus uitvoeren de volgende stappen uit:

    ![het uploaden van iWellnessNow domein en de URL's eenmalige aanmelding](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_iwellnessnow_upload.png)

    a. Klik op **uploaden metagegevensbestand**.

    ![uploadconfig iWellnessNow domein en de URL's eenmalige aanmelding](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_iwellnessnow_uploadconfig.png)

    b. Klik op **map logo** het metagegevensbestand selecteren en op **uploaden**.
    
    c. Na voltooiing van het uploaden van **metagegevensbestand van de serviceprovider** de **id** en **antwoord-URL** waarden ophalen automatisch gevuld  **iWellnessNow domein en de URL's** textbox sectie zoals hieronder wordt weergegeven:

    ![iWellnessNow domein en de URL's eenmalige aanmelding informatie](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_iwellnessnow_url3.png)

4. Als u geen **metagegevensbestand van de serviceprovider** en het configureren van de toepassing in wilt **IDP** geïnitieerd modus, de volgende stappen uitvoeren:

    ![iWellnessNow domein en de URL's eenmalige aanmelding informatie](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_iwellnessnow_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: `http://<CustomerName>.iwellnessnow.com`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<CustomerName>.iwellnessnow.com/ssologin`

5. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![iWellnessNow domein en de URL's eenmalige aanmelding informatie](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_iwellnessnow_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<CustomerName>.iwellnessnow.com/`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [iWellnessNow Client ondersteuningsteam](mailto:info@iwellnessnow.com) ophalen van deze waarden.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_iwellnessnow_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_400.png)
    
7. Eenmalige aanmelding configureren op **iWellnessNow** zijde, moet u de gedownloade verzenden **Metadata XML** naar [iWellnessNow ondersteuningsteam](mailto:info@iwellnessnow.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-iwellnessnow-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-iwellnessnow-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-iwellnessnow-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-iwellnessnow-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-iwellnessnow-test-user"></a>Een testgebruiker iWellnessNow maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in iWellnessNow maken. Werken met [iWellnessNow ondersteuningsteam](mailto:info@iwellnessnow.com) de gebruikers van het platform iWellnessNow toevoegen. Gebruikers moeten worden gemaakt en geactiveerd voordat u eenmalige aanmelding gebruiken

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot iWellnessNow.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen iWellnessNow, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **iWellnessNow**.

    ![De koppeling iWellnessNow in de lijst met toepassingen](./media/active-directory-saas-iwellnessnow-tutorial/tutorial_iwellnessnow_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel iWellnessNow in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing iWellnessNow.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-iwellnessnow-tutorial/tutorial_general_203.png

