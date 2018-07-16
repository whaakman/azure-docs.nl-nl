---
title: 'Zelfstudie: Azure Active Directory-integratie met Skytap | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Skytap.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6cb7ab2-da1a-4015-8e6f-c0c47bb6210f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: jeedes
ms.openlocfilehash: c2efad63d489bf81b6e0f7c03baecbc9d6571425
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050660"
---
# <a name="tutorial-azure-active-directory-integration-with-skytap"></a>Zelfstudie: Azure Active Directory-integratie met Skytap

In deze zelfstudie leert u hoe u Skytap integreren met Azure Active Directory (Azure AD).

Skytap integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Skytap heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Skytap (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Skytap, moet u de volgende items:

- Een Azure AD-abonnement
- Een Skytap eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Skytap uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-skytap-from-the-gallery"></a>Skytap uit de galerie toe te voegen
Voor het configureren van de integratie van Skytap in Azure AD, moet u Skytap uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Skytap uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Skytap**, selecteer **Skytap** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Skytap in de lijst met resultaten](./media/skytap-tutorial/tutorial_skytap_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Skytap op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Skytap is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Skytap tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Skytap, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Skytap](#create-a-skytap-test-user)**  : als u wilt een equivalent van Britta Simon in Skytap die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Skytap.

**Voor het configureren van Azure AD eenmalige aanmelding met Skytap, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Skytap** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/skytap-tutorial/tutorial_skytap_samlbase.png)

3. Op de **Skytap domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Skytap domein en URL's, eenmalige aanmelding informatie](./media/skytap-tutorial/tutorial_skytap_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `http://pingone.com/<custom EntityID>`

    b. In de **antwoord-URL** tekstvak, een URL typen: `https://sso.connect.pingidentity.com/sso/sp/ACS.saml2`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Skytap domein en URL's, eenmalige aanmelding informatie](./media/skytap-tutorial/tutorial_skytap_url1.png)

    c. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://sso.connect.pingidentity.com/sso/sp/initsso?saasid=<saasid>&idpid=<idpid>`
     
    d. In de **Relaystatus** tekstvak, een URL met behulp van het volgende patroon: `https://pingone.com/1.0/<custom ID>`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden met de werkelijke-id, aanmeldings-URL en Relaystatus bijwerken. Neem contact op met [Skytap Client ondersteuningsteam](mailto:support@skytap.com) om deze waarden te verkrijgen. 

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/skytap-tutorial/tutorial_skytap_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/skytap-tutorial/tutorial_general_400.png)
    
7. Het configureren van eenmalige aanmelding op **Skytap** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [Skytap ondersteuningsteam](mailto:support@skytap.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/skytap-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/skytap-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/skytap-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/skytap-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-skytap-test-user"></a>Maak een testgebruiker Skytap

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Skytap. Werken met [Skytap ondersteuningsteam](mailto:support@skytap.com) om toe te voegen de gebruikers in het Skytap-platform. Gebruikers moeten worden gemaakt en geactiveerd voordat u eenmalige aanmelding gebruiken

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Skytap.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Skytap toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Skytap**.

    ![De koppeling Skytap in de lijst met toepassingen](./media/skytap-tutorial/tutorial_skytap_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Skytap in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Skytap.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skytap-tutorial/tutorial_general_01.png
[2]: ./media/skytap-tutorial/tutorial_general_02.png
[3]: ./media/skytap-tutorial/tutorial_general_03.png
[4]: ./media/skytap-tutorial/tutorial_general_04.png

[100]: ./media/skytap-tutorial/tutorial_general_100.png

[200]: ./media/skytap-tutorial/tutorial_general_200.png
[201]: ./media/skytap-tutorial/tutorial_general_201.png
[202]: ./media/skytap-tutorial/tutorial_general_202.png
[203]: ./media/skytap-tutorial/tutorial_general_203.png

