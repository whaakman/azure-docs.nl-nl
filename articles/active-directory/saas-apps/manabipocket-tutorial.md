---
title: 'Zelfstudie: Azure Active Directory-integratie met Manabi Pocket | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Manabi Pocket.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e521099-bf7d-43ab-a0e0-86aa1c9e577e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: d1aec5ac1506d1c6151042a65dd8525f29353379
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39040419"
---
# <a name="tutorial-azure-active-directory-integration-with-manabi-pocket"></a>Zelfstudie: Azure Active Directory-integratie met Manabi Pocket

In deze zelfstudie leert u hoe u Manabi Pocket integreren met Azure Active Directory (Azure AD).

Manabi Pocket integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Manabi Pocket heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Manabi Pocket (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Manabi Pocket, moet u de volgende items:

- Een Azure AD-abonnement
- Een Manabi Pocket eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Manabi Pocket uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-manabi-pocket-from-the-gallery"></a>Manabi Pocket uit de galerie toe te voegen
Voor het configureren van de integratie van Manabi Pocket in Azure AD, moet u Manabi Pocket uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Manabi Pocket uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Manabi Pocket**, selecteer **Manabi Pocket** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Manabi Pocket in de lijst met resultaten](./media/manabipocket-tutorial/tutorial_manabipocket_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Manabi Pocket op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Manabi Pocket is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Manabi Pocket tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Manabi Pocket, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Manabi Pocket](#create-a-manabi-pocket-test-user)**  : als u wilt een equivalent van Britta Simon in Manabi Pocket die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Manabi Pocket.

**Voor het configureren van Azure AD eenmalige aanmelding met Manabi Pocket, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Manabi Pocket** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/manabipocket-tutorial/tutorial_manabipocket_samlbase.png)

3. Op de **Manabi Pocket domein en URL's** sectie, voert u de volgende stappen uit:

    ![Manabi Pocket domein en URL's, eenmalige aanmelding informatie](./media/manabipocket-tutorial/tutorial_manabipocket_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u de URL: `https://ed-cl.com/`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<SERVER-NAME>.ed-cl.com/<TENANT-ID>/idp/provider`

    > [!NOTE]
    > De id-waarde is niet echt. Deze waarde bijwerken met de werkelijke-id. Neem contact op met [Manabi Pocket Client ondersteuningsteam](mailto:info-ed-cl@ntt.com) deze waarde op te halen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/manabipocket-tutorial/tutorial_manabipocket_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/manabipocket-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **Manabi Pocket** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [Manabi Pocket ondersteuningsteam](mailto:info-ed-cl@ntt.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/manabipocket-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/manabipocket-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/manabipocket-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/manabipocket-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-manabi-pocket-test-user"></a>Maak een testgebruiker Manabi Pocket

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Manabi Pocket. Werken met [Manabi Pocket ondersteuningsteam](mailto:info-ed-cl@ntt.com) om toe te voegen de gebruikers in het Manabi Pocket-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Manabi Pocket.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon bij Manabi Pocket, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Manabi Pocket**.

    ![De koppeling Manabi Pocket in de lijst met toepassingen](./media/manabipocket-tutorial/tutorial_manabipocket_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Manabi Pocket in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Manabi Pocket.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/manabipocket-tutorial/tutorial_general_01.png
[2]: ./media/manabipocket-tutorial/tutorial_general_02.png
[3]: ./media/manabipocket-tutorial/tutorial_general_03.png
[4]: ./media/manabipocket-tutorial/tutorial_general_04.png

[100]: ./media/manabipocket-tutorial/tutorial_general_100.png

[200]: ./media/manabipocket-tutorial/tutorial_general_200.png
[201]: ./media/manabipocket-tutorial/tutorial_general_201.png
[202]: ./media/manabipocket-tutorial/tutorial_general_202.png
[203]: ./media/manabipocket-tutorial/tutorial_general_203.png