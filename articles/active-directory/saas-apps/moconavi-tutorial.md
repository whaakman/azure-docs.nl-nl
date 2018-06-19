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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: 032a674662fb2c55ee9a16b406418367ee7c797d
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35907356"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Zelfstudie: Azure Active Directory-integratie met moconavi

In deze zelfstudie leert u hoe moconavi integreren met Azure Active Directory (Azure AD).

Moconavi integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot moconavi heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij moconavi (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met moconavi, moet u de volgende items:

- Een Azure AD-abonnement
- Een moconavi eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Moconavi uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-moconavi-from-the-gallery"></a>Moconavi uit de galerie toevoegen
Voor het configureren van de integratie van moconavi in Azure AD, moet u moconavi uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen moconavi uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **moconavi**, selecteer **moconavi** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![moconavi in de lijst met resultaten](./media/moconavi-tutorial/tutorial_moconavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met moconavi op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in moconavi is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in moconavi tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met moconavi, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker moconavi](#create-a-moconavi-test-user)**  - hebben een equivalent van Britta Simon in moconavi die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing moconavi configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met moconavi, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **moconavi** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/moconavi-tutorial/tutorial_moconavi_samlbase.png)

3. Op de **moconavi domein en de URL's** sectie, voert u de volgende stappen uit:

    ![moconavi domein en de URL's eenmalige aanmelding informatie](./media/moconavi-tutorial/tutorial_moconavi_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<yourserverurl>/moconavi-saml2/saml/login`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<yourserverurl>/moconavi-saml2`

    C. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<yourserverurl>/moconavi-saml2/saml/SSO`

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL, -id en antwoord-URL. Neem contact op met [moconavi Client ondersteuningsteam](mailto:support@recomot.co.jp) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/moconavi-tutorial/tutorial_moconavi_certificate.png)

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/moconavi-tutorial/tutorial_general_400.png)

6. Eenmalige aanmelding configureren op **moconavi** zijde, moet u de gedownloade verzenden **Metadata XML** naar [moconavi ondersteuningsteam](mailto:support@recomot.co.jp). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/moconavi-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/moconavi-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/moconavi-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/moconavi-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-moconavi-test-user"></a>Een testgebruiker moconavi maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in moconavi maken. Werken met [moconavi ondersteuningsteam](mailto:support@recomot.co.jp) de gebruikers van het platform moconavi toevoegen. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot moconavi.

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen moconavi, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **moconavi**.

    ![De koppeling moconavi in de lijst met toepassingen](./media/moconavi-tutorial/tutorial_moconavi_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

1. Installeer moconavi vanuit Microsoft store.

2. Moconavi starten.

3. Klik op **instelling verbinding** knop.

    ![Testen van eenmalige aanmelding](./media/moconavi-tutorial/testing1.png)

4. Voer `https://mcs-admin.moconavi.biz/gateway` in **verbinding maken met URL** tekstvak en klik vervolgens op **gedaan** knop.

    ![Testen van eenmalige aanmelding](./media/moconavi-tutorial/testing2.png)

5. Voer de volgende stappen uit op de volgende schermafbeelding:

    ![Testen van eenmalige aanmelding](./media/moconavi-tutorial/testing3.png)

    a. Voer **invoer verificatiesleutel**:`azureAD` in **invoer verificatiesleutel** textbox.

    b. Voer **invoer gebruikers-ID**: `your ad account` in **invoer gebruikers-ID** textbox.

    c. Klik op **aanmelding**.

6. Voer uw Azure AD-wachtwoord aan **wachtwoord** tekstvak en klik vervolgens op **aanmelding** knop.

    ![Testen van eenmalige aanmelding](./media/moconavi-tutorial/testing4.png)

7. Azure AD-verificatie is geslaagd, wanneer het menu wordt weergegeven.

    ![Testen van eenmalige aanmelding](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

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

