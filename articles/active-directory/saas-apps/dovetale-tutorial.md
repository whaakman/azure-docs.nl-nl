---
title: 'Zelfstudie: Azure Active Directory-integratie met Dovetale | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Dovetale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81da50c3-df94-458a-8b6a-a30827ee6358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: jeedes
ms.openlocfilehash: dd2aa699c22518ebbe7f29ba8dfd296da7385476
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "40162004"
---
# <a name="tutorial-azure-active-directory-integration-with-dovetale"></a>Zelfstudie: Azure Active Directory-integratie met Dovetale

In deze zelfstudie leert u hoe u Dovetale integreren met Azure Active Directory (Azure AD).

Dovetale integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Dovetale heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Dovetale (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Dovetale, moet u de volgende items:

- Een Azure AD-abonnement
- Een Dovetale eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Dovetale uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-dovetale-from-the-gallery"></a>Dovetale uit de galerie toe te voegen

Voor het configureren van de integratie van Dovetale in Azure AD, moet u Dovetale uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Dovetale uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Dovetale**, selecteer **Dovetale** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Dovetale in de lijst met resultaten](./media/dovetale-tutorial/tutorial_dovetale_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Dovetale op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Dovetale is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Dovetale tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Dovetale, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Dovetale](#create-a-dovetale-test-user)**  : als u wilt een equivalent van Britta Simon in Dovetale die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Dovetale.

**Voor het configureren van Azure AD eenmalige aanmelding met Dovetale, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Dovetale** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/dovetale-tutorial/tutorial_dovetale_samlbase.png)

3. Op de **Dovetale domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus wordt de gebruiker hoeft niet te werk als de app al vooraf geïntegreerd in Azure is:

    ![Dovetale domein en URL's, eenmalige aanmelding informatie](./media/dovetale-tutorial/tutorial_dovetale_url1.png)

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Dovetale domein en URL's, eenmalige aanmelding informatie](./media/dovetale-tutorial/tutorial_dovetale_url2.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `<COMPANYNAME>.dovetale.com`

    > [!NOTE]
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Dovetale Client ondersteuningsteam](mailto:support@dovetale.com) deze waarde op te halen.

5. Dovetale toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/dovetale-tutorial/tutorial_attribute.png)

6. Klik op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie om uit te breiden de kenmerken. De volgende stappen uitvoeren op elk van de kenmerken weergegeven:

    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |
    | e-mailen | User.mail |
    | first_name | User.givenName |
    | naam | User.userPrincipalName |
    | last_name | User.surname |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/dovetale-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/dovetale-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak, type de **kenmerknaam** wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat **NAAMRUIMTE** waarde leeg.

    e. Klik op **OK**.

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De downloadkoppeling certificaat](./media/dovetale-tutorial/tutorial_dovetale_certificate.png) 

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/dovetale-tutorial/tutorial_general_400.png)

9. Op de **Dovetale configuratie** sectie, klikt u op **configureren Dovetale** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Dovetale configuratie](./media/dovetale-tutorial/tutorial_dovetale_configure.png)

10. Het configureren van eenmalige aanmelding op **Dovetale** zijde, moet u voor het verzenden van de **App-Url voor federatieve metagegevens, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [Dovetale ondersteuningsteam](mailto:support@dovetale.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/dovetale-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/dovetale-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/dovetale-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/dovetale-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-dovetale-test-user"></a>Maak een testgebruiker Dovetale

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Dovetale. Dovetale biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Dovetale als deze nog niet bestaat.

> [!Note]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Dovetale ondersteuningsteam](mailto:support@dovetale.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Dovetale.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan Dovetale toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Dovetale**.

    ![De koppeling Dovetale in de lijst met toepassingen](./media/dovetale-tutorial/tutorial_dovetale_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Dovetale in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Dovetale.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/dovetale-tutorial/tutorial_general_01.png
[2]: ./media/dovetale-tutorial/tutorial_general_02.png
[3]: ./media/dovetale-tutorial/tutorial_general_03.png
[4]: ./media/dovetale-tutorial/tutorial_general_04.png

[100]: ./media/dovetale-tutorial/tutorial_general_100.png

[200]: ./media/dovetale-tutorial/tutorial_general_200.png
[201]: ./media/dovetale-tutorial/tutorial_general_201.png
[202]: ./media/dovetale-tutorial/tutorial_general_202.png
[203]: ./media/dovetale-tutorial/tutorial_general_203.png