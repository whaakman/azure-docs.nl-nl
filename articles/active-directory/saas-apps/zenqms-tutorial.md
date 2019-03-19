---
title: 'Zelfstudie: Azure Active Directory-integratie met ZenQMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ZenQMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 72857c30-8896-438d-90c9-aeb21bf5fec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d5d2e734ff9bd34a176d08e36019c826dac355bb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58003795"
---
# <a name="tutorial-azure-active-directory-integration-with-zenqms"></a>Zelfstudie: Azure Active Directory-integratie met ZenQMS

In deze zelfstudie leert u hoe u ZenQMS integreren met Azure Active Directory (Azure AD).

ZenQMS integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ZenQMS heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ZenQMS (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ZenQMS, moet u de volgende items:

- Een Azure AD-abonnement
- Een ZenQMS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ZenQMS uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zenqms-from-the-gallery"></a>ZenQMS uit de galerie toe te voegen

Voor het configureren van de integratie van ZenQMS in Azure AD, moet u ZenQMS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ZenQMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **ZenQMS**, selecteer **ZenQMS** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ZenQMS in de lijst met resultaten](./media/zenqms-tutorial/tutorial_zenqms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ZenQMS op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in ZenQMS is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ZenQMS tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met ZenQMS, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker ZenQMS](#create-a-zenqms-test-user)**  : als u wilt een equivalent van Britta Simon in ZenQMS die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing ZenQMS.

**Voor het configureren van Azure AD eenmalige aanmelding met ZenQMS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ZenQMS** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/zenqms-tutorial/tutorial_zenqms_samlbase.png)

3. Op de **ZenQMS domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![ZenQMS domein en URL's, eenmalige aanmelding informatie](./media/zenqms-tutorial/tutorial_zenqms_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `urn:zenqms:<INSTANCE>`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<INSTANCE>.zenqms.com/SAML/AssertionConsumerService`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![ZenQMS domein en URL's, eenmalige aanmelding informatie](./media/zenqms-tutorial/tutorial_zenqms_url1.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon:
    
    | |
    |-|-|
    | `https://<INSTANCE>.zenqms.com/<ID>`|
    | `https://<INSTANCE>.zenqms.com/<EMAIL DOMAIN>/`|
    | |

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [ZenQMS Client ondersteuningsteam](mailto:help@zenqms.com) om deze waarden te verkrijgen.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.

    ![De link om het certificaat te downloaden](./media/zenqms-tutorial/tutorial_zenqms_certificate.png) 

6. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/zenqms-tutorial/tutorial_general_400.png)

7. Het configureren van eenmalige aanmelding op **ZenQMS** zijde, moet u voor het verzenden van de **App-Url voor federatieve metagegevens** naar [ZenQMS ondersteuningsteam](mailto:help@zenqms.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/zenqms-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/zenqms-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/zenqms-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/zenqms-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-zenqms-test-user"></a>Maak een testgebruiker ZenQMS

In deze sectie maakt u een gebruiker met de naam van Britta Simon in ZenQMS. Werken met [ZenQMS ondersteuningsteam](mailto:help@zenqms.com) om toe te voegen de gebruikers in het ZenQMS-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ZenQMS.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan ZenQMS toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **ZenQMS**.

    ![De koppeling ZenQMS in de lijst met toepassingen](./media/zenqms-tutorial/tutorial_zenqms_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel ZenQMS in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing ZenQMS.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/zenqms-tutorial/tutorial_general_01.png
[2]: ./media/zenqms-tutorial/tutorial_general_02.png
[3]: ./media/zenqms-tutorial/tutorial_general_03.png
[4]: ./media/zenqms-tutorial/tutorial_general_04.png

[100]: ./media/zenqms-tutorial/tutorial_general_100.png

[200]: ./media/zenqms-tutorial/tutorial_general_200.png
[201]: ./media/zenqms-tutorial/tutorial_general_201.png
[202]: ./media/zenqms-tutorial/tutorial_general_202.png
[203]: ./media/zenqms-tutorial/tutorial_general_203.png
