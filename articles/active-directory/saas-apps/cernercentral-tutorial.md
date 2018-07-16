---
title: 'Zelfstudie: Azure Active Directory-integratie met Cerner Central | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cerner centraal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d2bc549d-d286-4679-854e-bb67c62b0475
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jeedes
ms.openlocfilehash: 77cd9f90de1d50e91061a6a7222d01c72aadf3f3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047906"
---
# <a name="tutorial-azure-active-directory-integration-with-cerner-central"></a>Zelfstudie: Azure Active Directory-integratie met Cerner Central

In deze zelfstudie leert u hoe u Cerner centraal integreren met Azure Active Directory (Azure AD).

Cerner centraal integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Cerner-centraal heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Cerner centraal (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Cerner Central, moet u de volgende items:

- Een Azure AD-abonnement
- Een goedgekeurde Cerner centrale systeemaccount

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cerner centraal uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-cerner-central-from-the-gallery"></a>Cerner centraal uit de galerie toe te voegen
Voor het configureren van de integratie van Cerner centrale in Azure AD, moet u centraal Cerner uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cerner centraal vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven op het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Cerner centraal**.

    ![Het maken van een Azure AD-testgebruiker](./media/cernercentral-tutorial/tutorial_cernercentral_search.png)

5. Selecteer in het deelvenster resultaten **Cerner centraal**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/cernercentral-tutorial/tutorial_cernercentral_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Cerner Central op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent Cerner centraal-India is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker Cerner centraal-India tot stand worden gebracht.

Als u wilt configureren en Azure AD eenmalige aanmelding met Cerner Central testen, moet u uitvoeren van de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Cerner centraal](#creating-a-cerner-central-test-user)**  : als u wilt een equivalent van Britta Simon hebben in Centraal Cerner die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Cerner centraal.

**Voor het configureren van Azure AD eenmalige aanmelding met Cerner Central, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Cerner centraal** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/cernercentral-tutorial/tutorial_cernercentral_samlbase.png)

3. Op de **Cerner centrale domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/cernercentral-tutorial/tutorial_cernercentral_url.png)

    a. In de **id** tekstvak typt u de waarde met behulp van de volgende patronen:

    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    
    b. In de **antwoord-URL** tekstvak, een URL met behulp van de volgende patronen:
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/sso` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/sso` |

    > [!NOTE]
    > Deze waarden zijn niet de werkelijke. Werk deze waarden met de werkelijke id en de antwoord-URL. Neem contact op met [Cerner centraal ondersteuningsteam](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![Eenmalige aanmelding configureren](./media/cernercentral-tutorial/tutorial_metadataurl.png)

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/cernercentral-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **Cerner centraal** zijde, moet u voor het verzenden van de **App-Url voor federatieve metagegevens** naar [Cerner centraal ondersteuning](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations). Ze configureren de eenmalige aanmelding op de toepassing zijde voltooien van de integratie.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/cernercentral-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.

    ![Het maken van een Azure AD-testgebruiker](./media/cernercentral-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen**.

    ![Het maken van een Azure AD-testgebruiker](./media/cernercentral-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Het maken van een Azure AD-testgebruiker](./media/cernercentral-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van Britta Simon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-cerner-central-test-user"></a>Het maken van een testgebruiker Cerner-centraal

**Cerner centraal** toepassing staat verificatie van een federatieve id-provider. Als een gebruiker kan zich aanmelden bij de startpagina van de toepassing, worden ze federatief zijn en niet nodig is voor de handmatige inrichting. Meer informatie vindt u [hier](cernercentral-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Cerner centraal.

![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon bij Cerner Central, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Cerner centraal**.

    ![Eenmalige aanmelding configureren](./media/cernercentral-tutorial/tutorial_cernercentral_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Cerner centraal in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Cerner centraal. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Inrichten van gebruikers configureren](cernercentral-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/cernercentral-tutorial/tutorial_general_01.png
[2]: ./media/cernercentral-tutorial/tutorial_general_02.png
[3]: ./media/cernercentral-tutorial/tutorial_general_03.png
[4]: ./media/cernercentral-tutorial/tutorial_general_04.png

[100]: ./media/cernercentral-tutorial/tutorial_general_100.png

[200]: ./media/cernercentral-tutorial/tutorial_general_200.png
[201]: ./media/cernercentral-tutorial/tutorial_general_201.png
[202]: ./media/cernercentral-tutorial/tutorial_general_202.png
[203]: ./media/cernercentral-tutorial/tutorial_general_203.png