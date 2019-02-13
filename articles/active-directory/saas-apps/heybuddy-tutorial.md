---
title: 'Zelfstudie: Azure Active Directory-integratie met HeyBuddy | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en HeyBuddy.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d51b5af6-018e-4678-9a3f-b70438394f67
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 67c3bf168ae056b1205d6eeaa8e5676c880eab97
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56167113"
---
# <a name="tutorial-azure-active-directory-integration-with-heybuddy"></a>Zelfstudie: Azure Active Directory-integratie met HeyBuddy

In deze zelfstudie leert u hoe u HeyBuddy integreren met Azure Active Directory (Azure AD).

HeyBuddy integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot HeyBuddy heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij HeyBuddy (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met HeyBuddy, moet u de volgende items:

- Een Azure AD-abonnement
- Een HeyBuddy eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. HeyBuddy uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-heybuddy-from-the-gallery"></a>HeyBuddy uit de galerie toe te voegen

Voor het configureren van de integratie van HeyBuddy in Azure AD, moet u HeyBuddy uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen HeyBuddy uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **HeyBuddy**, selecteer **HeyBuddy** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![HeyBuddy in de lijst met resultaten](./media/heybuddy-tutorial/tutorial_heybuddy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met HeyBuddy op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in HeyBuddy is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in HeyBuddy tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met HeyBuddy, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker HeyBuddy](#creating-a-heybuddy-test-user)**  : als u wilt een equivalent van Britta Simon in HeyBuddy die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing HeyBuddy.

**Voor het configureren van Azure AD eenmalige aanmelding met HeyBuddy, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **HeyBuddy** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![HeyBuddy domein en URL's, eenmalige aanmelding informatie](./media/heybuddy-tutorial/tutorial_heybuddy_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://api.heybuddy.com/auth/<ENTITY ID>`

    b. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon: `YourCompanyInstanceofHeyBuddy`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke aanmeldings-URL en -id (entiteits-ID). De `Entity ID` in de aanmeldings-url is automatisch gegenereerd voor elke organisatie. Neem contact op met [HeyBuddy Client ondersteuningsteam](mailto:support@heybuddy.com) om deze waarden te verkrijgen.

5. HeyBuddy toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken en claims** op de integratiepagina van de toepassing-beheren. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **gebruikerskenmerken en Claims** dialoogvenster.

    ![image](./media/heybuddy-tutorial/tutorial_heybuddy_attribute.png)

    >[!NOTE]
    >Raadpleeg dit [koppeling](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) over het configureren en instellen van de functies voor de toepassing.

6. In de **gebruikersclaims** sectie op de **gebruikerskenmerken en Claims** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    | Name  | Bronkenmerk  |
    | --------------- | --------------- |
    | Rollen               | user.assignedroles |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](./common/new_save_attribute.png)

    ![image](./common/new_attribute_details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat de **Namespace** waarde leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **Opslaan**.

7. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op de kopiëren **pictogram** kopiëren **App federatieve metagegevens Url**  en plak deze in Kladblok.

    ![De link om het certificaat te downloaden](./media/heybuddy-tutorial/tutorial_heybuddy_certificate.png) 

8. Het configureren van eenmalige aanmelding op **HeyBuddy** zijde, moet u voor het verzenden van de gekopieerde **App-Url voor federatieve metagegevens** naar [HeyBuddy ondersteuningsteam](mailto:support@heybuddy.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-heybuddy-test-user"></a>Het maken van een testgebruiker HeyBuddy

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in HeyBuddy. HeyBuddy biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot HeyBuddy als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [HeyBuddy ondersteuningsteam](mailto:support@heybuddy.com).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan HeyBuddy.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **HeyBuddy**.

    ![Eenmalige aanmelding configureren](./media/heybuddy-tutorial/tutorial_heybuddy_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel HeyBuddy in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing HeyBuddy.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
