---
title: 'Zelfstudie: Azure Active Directory-integratie met HubSpot | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en HubSpot.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 57343ccd-53ea-4e62-9e54-dee2a9562ed5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: jeedes
ms.openlocfilehash: 2806288378e5fa080164155b97a47a7046ecbba1
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814899"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Zelfstudie: Azure Active Directory-integratie met HubSpot

In deze zelfstudie leert u hoe u HubSpot integreren met Azure Active Directory (Azure AD).

HubSpot integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot HubSpot heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij HubSpot (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met HubSpot, moet u de volgende items:

- Een Azure AD-abonnement
- Een HubSpot eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. HubSpot uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-hubspot-from-the-gallery"></a>HubSpot uit de galerie toe te voegen

Voor het configureren van de integratie van HubSpot in Azure AD, moet u HubSpot uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen HubSpot uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

4. Typ in het zoekvak **HubSpot**. Selecteer **HubSpot** vanuit het deelvenster voor resultaten en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/hubspot-tutorial/tutorial_hubspot_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met HubSpot op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in HubSpot is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in HubSpot tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met HubSpot, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker HubSpot](#creating-a-hubspot-saml-test-user)**  : als u wilt een equivalent van Britta Simon in HubSpot die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing HubSpot.

**Voor het configureren van Azure AD eenmalige aanmelding met HubSpot, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **HubSpot** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/tutorial_general_301.png)

3. Als u wijzigen wilt in **SAML** modus van een andere modus, klikt u op **modus wijzigen in één aanmelding** boven op het scherm.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/tutorial_general_300.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/tutorial_general_302.png)

5. Op de **SAML-basisconfiguratie** sectie de volgende stappen uitvoeren als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![HubSpot domein en URL's, eenmalige aanmelding informatie](./media/hubspot-tutorial/tutorial_hubspot_url.png)

    a. In de **id** tekstvak typt u de URL met behulp van het volgende patroon: `https://api.hubspot.com/login-api/v1/saml/login?portalId=<CUSTOMER ID>`

    b. In de **antwoord-URL** tekstvak typt u de URL met behulp van het volgende patroon: `https://api.hubspot.com/login-api/v1/saml/acs?portalId=<CUSTOMER ID>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke id en de antwoord-URL die verderop in deze zelfstudie wordt uitgelegd. Neem contact op met [HubSpot Client ondersteuningsteam](https://help.hubspot.com/) om deze waarden te verkrijgen.

    c. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![HubSpot domein en URL's, eenmalige aanmelding informatie](./media/hubspot-tutorial/tutorial_hubspot_url1.png)

    In de **aanmeldings-URL** tekstvak typt u de URL: `https://app.hubspot.com/login`

6. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/tutorial_hubspot_certificate.png)

7. Op de **instellen HubSpot** sectie, klikt u op de knop kopiëren om te kopiëren de **aanmeldings-URL** en **Azure AD-id** waarden.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/tutorial_hubspot_configure.png)

8. Open een nieuw tabblad in uw browser en aan te melden bij uw HubSpot administrator-account.

9. Klik op **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/config1.png)

10. Klik op **standaardwaarden Account**.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/config2.png)

11. Schuif omlaag naar de **Security** sectie en klikt u op **instellen**.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/config3.png)

12. Op de **instellen van eenmalige aanmelding** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/config4.png)

    a. Klik op **kopie** knop om te kopiëren de **doelgroep URl(Service Provider Entity ID)** waarde en plak deze in de **id** -tekstvak in de **Basic SAML Configuratie** sectie in Azure portal.

    b. Klik op **kopie** knop om te kopiëren de **Meld u aan bij de URl, ACS, ontvanger of omleiden** waarde en plak deze in de **antwoord-URL** -tekstvak in de **Basic SAML Configuratie** sectie in Azure portal.

    c. In de **Identity Provider-id of URL-verlener** tekstvak, plak de **Azure AD-id** waarde die u hebt gekopieerd vanuit Azure portal.

    d. In de **Identity Provider aanmeldings-URL voor eenmalige** tekstvak, plak de **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    e. Open uw gedownloade **Certificate(Base64)** bestand in Kladblok. Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** vak.

    f. Klik op **Controleren**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](./media/hubspot-tutorial/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](./media/hubspot-tutorial/create_aaduser_02.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-hubspot-test-user"></a>Het maken van een testgebruiker HubSpot

Als u wilt dat Azure AD-gebruikers zich aanmelden bij HubSpot, moeten ze worden ingericht voor HubSpot.
In het geval van HubSpot is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **HubSpot** bedrijf site als administrator.

2. Klik op **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/config1.png)

3. Klik op **gebruikers en Teams**.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/user1.png)

4. Klik op **Gebruiker maken**.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/user2.png)

5. Voer het e-mailadres van de gebruiker, zoals **brittasimon@contoso.com** in de **toevoegen e addess(es)** tekstvak en klikt u op **volgende**.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/user3.png)

6. Op de **maken gebruikers** sectie, Ga via de elke afzonderlijke tabblad en selecteer opties die van toepassing en machtigingen voor de gebruiker en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/user4.png)

7. Klik op **verzenden** de uitnodiging voor de gebruiker te verzenden.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > Gebruiker wordt geactiveerd nadat u hebt de uitnodiging geaccepteerd.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan HubSpot.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **HubSpot**.

    ![Eenmalige aanmelding configureren](./media/hubspot-tutorial/tutorial_hubspot_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel HubSpot in het toegangsvenster, moet u de aanmeldingspagina van HubSpot toepassing automatisch beschikt.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/hubspot-tutorial/tutorial_general_01.png
[2]: ./media/hubspot-tutorial/tutorial_general_02.png
[3]: ./media/hubspot-tutorial/tutorial_general_03.png
[4]: ./media/hubspot-tutorial/tutorial_general_04.png
[100]: ./media/hubspot-tutorial/tutorial_general_100.png
[200]: ./media/hubspot-tutorial/tutorial_general_200.png
[201]: ./media/hubspot-tutorial/tutorial_general_201.png
[202]: ./media/hubspot-tutorial/tutorial_general_202.png
[203]: ./media/hubspot-tutorial/tutorial_general_203.png