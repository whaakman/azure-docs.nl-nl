---
title: 'Zelfstudie: Azure Active Directory-integratie met Proxyclick | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Proxyclick.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5c58a859-71c2-4542-ae92-e5f16a8e7f18
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: jeedes
ms.openlocfilehash: 71b9b54e3b8eef1be9f6da7fa812bd8f9d246f47
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051683"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Zelfstudie: Azure Active Directory-integratie met Proxyclick

In deze zelfstudie leert u hoe u Proxyclick integreren met Azure Active Directory (Azure AD).

Proxyclick integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Proxyclick heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Proxyclick (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Proxyclick, moet u de volgende items:

- Een Azure AD-abonnement
- Een Proxyclick eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Proxyclick uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-proxyclick-from-the-gallery"></a>Proxyclick uit de galerie toe te voegen
Voor het configureren van de integratie van Proxyclick in Azure AD, moet u Proxyclick uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Proxyclick uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Proxyclick**, selecteer **Proxyclick** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Proxyclick in de lijst met resultaten](./media/proxyclick-tutorial/tutorial_proxyclick_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Proxyclick op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Proxyclick is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Proxyclick tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Proxyclick, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Proxyclick](#create-a-proxyclick-test-user)**  : als u wilt een equivalent van Britta Simon in Proxyclick die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Proxyclick.

**Voor het configureren van Azure AD eenmalige aanmelding met Proxyclick, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Proxyclick** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/proxyclick-tutorial/tutorial_proxyclick_samlbase.png)

3. Op de **Proxyclick domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Proxyclick domein en URL's, eenmalige aanmelding informatie](./media/proxyclick-tutorial/tutorial_proxyclick_url2.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://saml.proxyclick.com/init/<companyId>`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://saml.proxyclick.com/consume/<companyId>`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Proxyclick domein en URL's, eenmalige aanmelding informatie](./media/proxyclick-tutorial/tutorial_proxyclick_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://saml.proxyclick.com/init/<companyId>`

    > [!NOTE]
    > Deze waarden zijn niet echt. U kunt deze waarden wordt bijgewerkt met de werkelijke-id, de antwoord-URL en aanmeldings-URL, die later in de zelfstudie wordt uitgelegd.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/proxyclick-tutorial/tutorial_proxyclick_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/proxyclick-tutorial/tutorial_general_400.png)

7. Op de **Proxyclick configuratie** sectie, klikt u op **configureren Proxyclick** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Proxyclick configuratie](./media/proxyclick-tutorial/tutorial_proxyclick_configure.png)

8. In een ander browservenster aanmelden bij uw bedrijf Proxyclick site als beheerder.

9. Selecteer **& Accountinstellingen**.

    ![Proxyclick configuratie](./media/proxyclick-tutorial/configure1.png)

10. Schuif omlaag naar de **INTEGRATIES** en selecteer **SAML**.

    ![Proxyclick configuratie](./media/proxyclick-tutorial/configure2.png)

11. In de **SAML** sectie, voert u de volgende stappen uit:

    ![Proxyclick configuratie](./media/proxyclick-tutorial/configure3.png)

    a. Kopie **URL voor SAML-consument** waarde en plak deze in **antwoord-URL** -tekstvak in **Proxyclick domein en URL's** sectie in Azure portal.

    b. Kopie **Omleidings-URL voor SAML SSO-** waarde en plak deze in **aanmeldings-URL** en **id** tekstvakken in **Proxyclick domein en URL's** sectie in Azure portal.

    c. Selecteer **SAML-aanvraagmethode** als **HTTP-omleiding**.

    d. In de **verlener** tekstvak, plak de waarde van **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure portal.

    e. In de **eindpunt-URL voor SAML 2.0** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** gekopieerd vanuit Azure portal.

    f. Open het gedownloade certificaat-bestand vanuit Azure portal in Kladblok en plak deze in de **certificaat** tekstvak.

    g. Klik op **wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/proxyclick-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/proxyclick-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/proxyclick-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/proxyclick-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-proxyclick-test-user"></a>Maak een testgebruiker Proxyclick

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Proxyclick, moeten ze worden ingericht voor Proxyclick. In het geval van Proxyclick is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Proxyclick site aan als beheerder.

2. Klik op **collega's** in de bovenste navigatiebalk.

    ![Werknemer toevoegen](./media/proxyclick-tutorial/user1.png)

3. Klik op **collega toevoegen**

    ![Werknemer toevoegen](./media/proxyclick-tutorial/user2.png)

4. In de **toevoegen van een collega** sectie, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/proxyclick-tutorial/user3.png)

    a. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    b. In de **voornaam** tekstvak, de eerste naam van gebruiker, zoals Julia.

    c. In de **achternaam** tekstvak typt u de achternaam van de gebruiker, zoals Simon.

    d. Klik op **gebruiker toevoegen**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Proxyclick.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan Proxyclick toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Proxyclick**.

    ![De koppeling Proxyclick in de lijst met toepassingen](./media/proxyclick-tutorial/tutorial_proxyclick_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Proxyclick in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Proxyclick.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/proxyclick-tutorial/tutorial_general_01.png
[2]: ./media/proxyclick-tutorial/tutorial_general_02.png
[3]: ./media/proxyclick-tutorial/tutorial_general_03.png
[4]: ./media/proxyclick-tutorial/tutorial_general_04.png

[100]: ./media/proxyclick-tutorial/tutorial_general_100.png

[200]: ./media/proxyclick-tutorial/tutorial_general_200.png
[201]: ./media/proxyclick-tutorial/tutorial_general_201.png
[202]: ./media/proxyclick-tutorial/tutorial_general_202.png
[203]: ./media/proxyclick-tutorial/tutorial_general_203.png

