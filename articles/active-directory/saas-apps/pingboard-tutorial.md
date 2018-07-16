---
title: 'Zelfstudie: Azure Active Directory-integratie met Pingboard | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Pingboard.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: f3dd1c0043a4516bfba0801d403f4b475fa2817a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041922"
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Zelfstudie: Azure Active Directory-integratie met Pingboard

In deze zelfstudie leert u hoe u Pingboard integreren met Azure Active Directory (Azure AD).

Pingboard integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Pingboard heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Pingboard (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Pingboard, moet u de volgende items:

- Een Azure AD-abonnement
- Een Pingboard eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Pingboard uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-pingboard-from-the-gallery"></a>Pingboard uit de galerie toe te voegen
Voor het configureren van de integratie van Pingboard in Azure AD, moet u Pingboard uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Pingboard uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De zakelijke toepassingen][2]

3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Pingboard**, selecteer **Pingboard** van resultaat deelvenster en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Pingboard in de lijst met resultaten](./media/pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Pingboard op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Pingboard is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Pingboard tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Pingboard.

Om te configureren en testen van Azure AD eenmalige aanmelding met Pingboard, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Pingboard](#create-a-pingboard-test-user)**  : als u wilt een equivalent van Britta Simon in Pingboard die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Pingboard.

**Voor het configureren van Azure AD eenmalige aanmelding met Pingboard, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Pingboard** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2.  Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. Op de **Pingboard domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Pingboard domein en URL's één informatie-aanmeldings-id-provider](./media/pingboard-tutorial/tutorial_pingboard_url.png)

    a. In de **id** tekstvak typt u de waarde als: `http://app.pingboard.com/sp`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<entity-id>.pingboard.com/auth/saml/consume`

4. Controleer **geavanceerde URL-instellingen weergeven**, als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Pingboard domein en URL's, eenmalige aanmelding informatie SP](./media/pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

     In de **aanmeldings-URL** tekstvak typt u de URL met behulp van het volgende patroon: `https://<sub-domain>.pingboard.com/sign_in`

    > [!NOTE]
    > Houd er rekening mee dat deze waarden niet echt zijn. Werk deze waarden met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [Pingboard Client ondersteuningsteam](https://support.pingboard.com/) om deze waarden te verkrijgen.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Pingboard metagegevens-xml](./media/pingboard-tutorial/tutorial_pingboard_certificate.png)

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/pingboard-tutorial/tutorial_general_400.png)

7. Voor het configureren van eenmalige aanmelding Pingboard aan, opent u een nieuw browservenster en meld u aan bij uw Pingboard Account. U moet een beheerder Pingboard voor het instellen van eenmalige aanmelding.

8. Selecteer in het bovenste menu,, **Apps > integraties**

    ![Eenmalige aanmelding configureren](./media/pingboard-tutorial/Pingboard_integration.png)

9. Op de **integraties** pagina, zoek de **'Azure Active Directory'** tegel en klik erop.

    ![Pingboard Single Sign-On-integratie](./media/pingboard-tutorial/Pingboard_aad.png)

10. In het modaal klikt u op volgende **'Configureren'**

    ![Knop voor Pingboard-configuratie](./media/pingboard-tutorial/Pingboard_configure.png)

11. Klik op de volgende pagina ziet u dat 'Azure SSO-integratie is ingeschakeld'. Open het gedownloade Metadata XML-bestand in Kladblok en plak de inhoud in **IDP metagegevens**.

    ![Scherm Pingboard SSO-configuratie](./media/pingboard-tutorial/Pingboard_sso_configure.png)

12. Het bestand is gevalideerd, en als alles klopt, eenmalige aanmelding wordt nu zijn ingeschakeld.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop](./media/pingboard-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/pingboard-tutorial/create_aaduser_02.png)

3. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.

    ![Knop Toevoegen](./media/pingboard-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/pingboard-tutorial/create_aaduser_04.png)

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="create-a-pingboard-test-user"></a>Maak een testgebruiker Pingboard

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Pingboard. Pingboard ondersteunt automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](pingboard-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

**Als u moet de gebruiker handmatig hebt gemaakt, voert u de volgende stappen uit:**

1. Meld u aan bij uw bedrijf Pingboard site aan als beheerder.

2. Klik op **"Werknemer toevoegen"** op knop **Directory** pagina.

    ![Werknemer toevoegen](./media/pingboard-tutorial/create_testuser_add.png)

3. Op de **"Werknemer toevoegen"** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Anderen uitnodigen](./media/pingboard-tutorial/create_testuser_name.png)

    a. In de **volledige naam** tekstvak typt u de volledige naam van gebruiker, zoals **Britta Simon**.

    b. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    c. In de **functietitel** tekstvak typt u de functie van Britta Simon.

    d. In de **locatie** vervolgkeuzelijst, selecteer de locatie van Britta Simon.

    e. Klik op **Add**.

4. Er wordt een bevestigingsvenster weergegeven om te bevestigen van het toevoegen van gebruiker.

    ![bevestigen](./media/pingboard-tutorial/create_testuser_confirm.png)

    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Pingboard.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Pingboard toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Pingboard**.

    ![De koppeling Pingboard in de lijst met toepassingen](./media/pingboard-tutorial/tutorial_pingboard_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

Wanneer u op de tegel Pingboard in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Pingboard.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Inrichten van gebruikers configureren](pingboard-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/pingboard-tutorial/tutorial_general_01.png
[2]: ./media/pingboard-tutorial/tutorial_general_02.png
[3]: ./media/pingboard-tutorial/tutorial_general_03.png
[4]: ./media/pingboard-tutorial/tutorial_general_04.png

[100]: ./media/pingboard-tutorial/tutorial_general_100.png

[200]: ./media/pingboard-tutorial/tutorial_general_200.png
[201]: ./media/pingboard-tutorial/tutorial_general_201.png
[202]: ./media/pingboard-tutorial/tutorial_general_202.png
[203]: ./media/pingboard-tutorial/tutorial_general_203.png