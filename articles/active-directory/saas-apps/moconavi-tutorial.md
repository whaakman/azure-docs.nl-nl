---
title: 'Zelfstudie: Azure Active Directory-integratie met moconavi | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en moconavi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: 3009cb42ac477b18d45ab5968d6f5793ce1cd36c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165894"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Zelfstudie: Azure Active Directory-integratie met moconavi

In deze zelfstudie leert u hoe u moconavi integreren met Azure Active Directory (Azure AD).

Moconavi integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot moconavi heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij moconavi (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met moconavi, moet u de volgende items:

- Een Azure AD-abonnement
- Een moconavi eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Moconavi uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-moconavi-from-the-gallery"></a>Moconavi uit de galerie toe te voegen
Voor het configureren van de integratie van moconavi in Azure AD, moet u moconavi uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen moconavi uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **moconavi**, selecteer **moconavi** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![moconavi in de lijst met resultaten](./media/moconavi-tutorial/tutorial_moconavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met moconavi op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in moconavi is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in moconavi tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met moconavi, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker moconavi](#create-a-moconavi-test-user)**  : als u wilt een equivalent van Britta Simon in moconavi die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing moconavi.

**Voor het configureren van Azure AD eenmalige aanmelding met moconavi, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **moconavi** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/moconavi-tutorial/tutorial_moconavi_samlbase.png)

3. Op de **moconavi domein en URL's** sectie, voert u de volgende stappen uit:

    ![moconavi domein en URL's één aanmeldings-informatie](./media/moconavi-tutorial/tutorial_moconavi_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<yourserverurl>/moconavi-saml2/saml/login`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<yourserverurl>/moconavi-saml2`

    C. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<yourserverurl>/moconavi-saml2/saml/SSO`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [moconavi Client ondersteuningsteam](mailto:support@recomot.co.jp) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/moconavi-tutorial/tutorial_moconavi_certificate.png)

5. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/moconavi-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **moconavi** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [moconavi ondersteuningsteam](mailto:support@recomot.co.jp). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/moconavi-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/moconavi-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/moconavi-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/moconavi-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-moconavi-test-user"></a>Maak een testgebruiker moconavi

In deze sectie maakt u een gebruiker met de naam van Britta Simon in moconavi. Werken met [moconavi ondersteuningsteam](mailto:support@recomot.co.jp) om toe te voegen de gebruikers in het moconavi-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot moconavi.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan moconavi toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **moconavi**.

    ![De koppeling moconavi in de lijst met toepassingen](./media/moconavi-tutorial/tutorial_moconavi_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

1. Installeer moconavi vanuit Microsoft store.

2. Start moconavi.

3. Klik op **verbinding maken met de instelling** knop.

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing1.png)

4. Voer `https://mcs-admin.moconavi.biz/gateway` in **verbinding maken met URL** tekstvak en klik vervolgens op **gedaan** knop.

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing2.png)

5. Voer de volgende stappen uit op de volgende schermafbeelding:

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing3.png)

    a. Voer **invoer verificatiesleutel**:`azureAD` in **invoer verificatiesleutel** tekstvak.

    b. Voer **invoer van gebruikers-ID**: `your ad account` in **invoer van gebruikers-ID** tekstvak.

    c. Klik op **aanmelding**.

6. Voer uw Azure AD-wachtwoord in **wachtwoord** tekstvak en klik vervolgens op **aanmelding** knop.

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing4.png)

7. Azure AD-verificatie is geslaagd, wanneer het menu wordt weergegeven.

    ![Eenmalige aanmelding testen](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/moconavi-tutorial/tutorial_general_01.png
[2]: ./media/moconavi-tutorial/tutorial_general_02.png
[3]: ./media/moconavi-tutorial/tutorial_general_03.png
[4]: ./media/moconavi-tutorial/tutorial_general_04.png

[100]: ./media/moconavi-tutorial/tutorial_general_100.png

[200]: ./media/moconavi-tutorial/tutorial_general_200.png
[201]: ./media/moconavi-tutorial/tutorial_general_201.png
[202]: ./media/moconavi-tutorial/tutorial_general_202.png
[203]: ./media/moconavi-tutorial/tutorial_general_203.png

