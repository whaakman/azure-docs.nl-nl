---
title: 'Zelfstudie: Azure Active Directory-integratie met Figma | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Figma.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8569cae1-87dd-4c40-9bbb-527ac80d6a96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: jeedes
ms.openlocfilehash: b57fdb3f039a9395133854f8b4d4f2095e3a4f9b
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095242"
---
# <a name="tutorial-azure-active-directory-integration-with-figma"></a>Zelfstudie: Azure Active Directory-integratie met Figma

In deze zelfstudie leert u hoe u Figma integreren met Azure Active Directory (Azure AD).

Figma integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Figma heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Figma (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Figma, moet u de volgende items:

- Een Azure AD-abonnement
- Een Figma [eenmalige aanmelding ingeschakeld abonnement](https://www.figma.com/pricing/)

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving. Nieuwe klanten en actieve abonnees van Figma Professional-Team contact opnemen met Figma naar [hun abonnement](https://www.figma.com/pricing/) naar de prijscategorie van de organisatie Figma.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Figma uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-figma-from-the-gallery"></a>Figma uit de galerie toe te voegen

Voor het configureren van de integratie van Figma in Azure AD, moet u Figma uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Figma uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Figma**. Selecteer **Figma** vanuit het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Figma in de lijst met resultaten](./media/figma-tutorial/tutorial_figma_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Figma op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD worden gekoppeld aan Figma.  Als u wilt configureren en Azure AD eenmalige aanmelding met Figma testen, voert u de volgende stappen uit:

1. [**Neem contact op met het ondersteuningsteam Figma** ](mailto:support@figma.com?subject=SAML+Config) starten van een SAML-configuratie voor uw organisatie en een ORG_SAML_CONFIG_ID ophalen.
2. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
3. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Maak een testgebruiker Figma](#creating-a-figma-test-user)**  : als u wilt een equivalent van Britta Simon in Figma die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
5. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
6. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Figma.

**Voor het configureren van Azure AD eenmalige aanmelding met Figma, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Figma** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/figma-tutorial/tutorial_general_301.png)

3. Als u wijzigen wilt in **SAML** modus van een andere modus, klikt u op **modus wijzigen in één aanmelding** boven op het scherm.

    ![Eenmalige aanmelding configureren](./media/figma-tutorial/tutorial_general_300.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/figma-tutorial/tutorial_general_302.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Figma domein en URL's, eenmalige aanmelding informatie](./media/figma-tutorial/tutorial_figma_url1.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://www.figma.com/saml/<ORG_SAML_CONFIG_ID>`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.figma.com/saml/<ORG_SAML_CONFIG_ID>/consume`

6. Klik op **extra URL's instellen** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Figma domein en URL's, eenmalige aanmelding informatie](./media/figma-tutorial/tutorial_figma_url2.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.figma.com/saml/<ORG_SAML_CONFIG_ID>/start`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Figma ondersteuningsteam](mailto:support@figma.com?subject=SAML+Config) om deze waarden te verkrijgen.

7. Figma toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **gebruikerskenmerken** dialoogvenster.

    ![Kenmerk-sectie](./media/figma-tutorial/edit_attribute.png)

8. In de **gebruikersclaims** sectie op de **gebruikerskenmerken** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:

    | Naam | Kenmerk van de gegevensbron|
    | ---------------| --------- |
    | `externalId` | `user.mailnickname` |
    | `displayName` | `user.displayname` |
    | `title` | `user.jobtitle` |
    | `emailaddress` | `user.mail` |
    | `familyName` | `user.surname` |
    | `givenName` | `givenName` |
    | `userName` | `user.userprincipalname` |

    a. Klik op **toevoegen nieuwe claim** openen de **gebruikersclaims beheren** dialoogvenster.

    ![Nieuw kenmerk](./media/figma-tutorial/new_save_attribute.png)

    ![Kenmerk toevoegen](./media/figma-tutorial/new_attribute_details.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Laat de **Namespace** leeg.

    d. Selecteer de bron als **kenmerk**.

    e. Uit de **bronkenmerk** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    f. Klik op **Ok**

    g. Klik op **Opslaan**.

9. Op de **instellen van eenmalige aanmelding met SAML** pagina, In de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en sla deze op uw computer.

    ![De downloadkoppeling certificaat](./media/figma-tutorial/tutorial_figma_certificate.png)

10. Voor het configureren van eenmalige aanmelding aan Figma, vul dit formulier: [ https://goo.gl/forms/XkRB1z5ed4eVUzXn2 ](https://goo.gl/forms/XkRB1z5ed4eVUzXn2). Deze accepteert uw **App-Url voor federatieve metagegevens** uit stap 9.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](./media/figma-tutorial/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](./media/figma-tutorial/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-figma-test-user"></a>Het maken van een testgebruiker Figma

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Figma. Figma biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Figma als deze nog niet bestaat.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan HubSpot SAML.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **HubSpot SAML**.

    ![Eenmalige aanmelding configureren](./media/figma-tutorial/tutorial_figma_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Figma in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Figma.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/figma-tutorial/tutorial_general_01.png
[2]: ./media/figma-tutorial/tutorial_general_02.png
[3]: ./media/figma-tutorial/tutorial_general_03.png
[4]: ./media/figma-tutorial/tutorial_general_04.png

[100]: ./media/figma-tutorial/tutorial_general_100.png

[200]: ./media/figma-tutorial/tutorial_general_200.png
[201]: ./media/figma-tutorial/tutorial_general_201.png
[202]: ./media/figma-tutorial/tutorial_general_202.png
[203]: ./media/figma-tutorial/tutorial_general_203.png