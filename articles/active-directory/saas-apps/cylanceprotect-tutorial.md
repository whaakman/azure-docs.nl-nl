---
title: 'Zelfstudie: Azure Active Directory-integratie met CylancePROTECT | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en CylancePROTECT.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ea392d8c-c8aa-4475-99d0-b08524ef0f3a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: jeedes
ms.openlocfilehash: 62709817e6f906922ff1008608770bcfa5605078
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39042704"
---
# <a name="tutorial-azure-active-directory-integration-with-cylanceprotect"></a>Zelfstudie: Azure Active Directory-integratie met CylancePROTECT

In deze zelfstudie leert u hoe u CylancePROTECT integreren met Azure Active Directory (Azure AD).

CylancePROTECT integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot CylancePROTECT heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij CylancePROTECT (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met CylancePROTECT, moet u de volgende items:

- Een Azure AD-abonnement
- Een CylancePROTECT eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. CylancePROTECT uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-cylanceprotect-from-the-gallery"></a>CylancePROTECT uit de galerie toe te voegen
Voor het configureren van de integratie van CylancePROTECT in Azure AD, moet u CylancePROTECT uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen CylancePROTECT uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **CylancePROTECT**, selecteer **CylancePROTECT** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![CylancePROTECT in de lijst met resultaten](./media/cylanceprotect-tutorial/tutorial_cylanceprotect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met CylancePROTECT op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in CylancePROTECT is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in CylancePROTECT tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met CylancePROTECT, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker CylancePROTECT](#create-a-cylanceprotect-test-user)**  : als u wilt een equivalent van Britta Simon in CylancePROTECT die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing CylancePROTECT.

**Voor het configureren van Azure AD eenmalige aanmelding met CylancePROTECT, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **CylancePROTECT** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/cylanceprotect-tutorial/tutorial_cylanceprotect_samlbase.png)

3. Op de **CylancePROTECT domein en URL's** sectie, voert u de volgende stappen uit:

    ![CylancePROTECT domein en URL's, eenmalige aanmelding informatie](./media/cylanceprotect-tutorial/tutorial_cylanceprotect_url.png)

    a. In de **id** tekstvak typt u de URL:
    
    | Regio | URL-waarde |
    |----------|---------|
    | Azië en Stille Oceaan-Noordoost (APNE1)| ` https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Azië-Pacific (Zuidoost) (AU) | `https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml` |
    | Europa centraal (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Noord-Amerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Zuid-Amerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    
    b. In de **antwoord-URL** tekstvak typt u de URL:
    
    | Regio | URL-waarde |
    |----------|---------|
    | Azië en Stille Oceaan-Noordoost (APNE1)|`https://login-apne1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Azië-Pacific (Zuidoost) (AU)|`https://login-au.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Europa centraal (EUC1)|`https://login-euc1.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Noord-Amerika|`https://login.cylance.com/EnterpriseLogin/ConsumeSaml`|
    | Zuid-Amerika (SAE1)|`https://login-sae1.cylance.com/EnterpriseLogin/ConsumeSaml`|

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en sla het bestand van het certificaat op uw computer.

    ![De downloadkoppeling certificaat](./media/cylanceprotect-tutorial/tutorial_cylanceprotect_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/cylanceprotect-tutorial/tutorial_general_400.png)

6. Op de **CylancePROTECT configuratie** sectie, klikt u op **configureren CylancePROTECT** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![CylancePROTECT configuratie](./media/cylanceprotect-tutorial/tutorial_cylanceprotect_configure.png) 

7. Het configureren van eenmalige aanmelding op **CylancePROTECT** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64), URL van de afmelding, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar console-beheerder. Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/cylanceprotect-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/cylanceprotect-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/cylanceprotect-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/cylanceprotect-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-cylanceprotect-test-user"></a>Maak een testgebruiker CylancePROTECT

In deze sectie maakt u een gebruiker met de naam van Britta Simon in CylancePROTECT. Werken met de beheerder om toe te voegen de gebruikers in het platform CylancePROTECT console. De houder van Azure Active Directory-account ontvangt een e-mailbericht en gaat u als volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan CylancePROTECT.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan CylancePROTECT toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **CylancePROTECT**.

    ![De koppeling CylancePROTECT in de lijst met toepassingen](./media/cylanceprotect-tutorial/tutorial_cylanceprotect_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel CylancePROTECT in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing CylancePROTECT.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cylanceprotect-tutorial/tutorial_general_01.png
[2]: ./media/cylanceprotect-tutorial/tutorial_general_02.png
[3]: ./media/cylanceprotect-tutorial/tutorial_general_03.png
[4]: ./media/cylanceprotect-tutorial/tutorial_general_04.png

[100]: ./media/cylanceprotect-tutorial/tutorial_general_100.png

[200]: ./media/cylanceprotect-tutorial/tutorial_general_200.png
[201]: ./media/cylanceprotect-tutorial/tutorial_general_201.png
[202]: ./media/cylanceprotect-tutorial/tutorial_general_202.png
[203]: ./media/cylanceprotect-tutorial/tutorial_general_203.png

