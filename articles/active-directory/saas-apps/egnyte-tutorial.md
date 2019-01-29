---
title: 'Zelfstudie: Azure Active Directory-integratie met Egnyte | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Egnyte.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2018
ms.author: jeedes
ms.openlocfilehash: e33fc71e0e43864d7d70495fc5056a8acaf4ad56
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159008"
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Zelfstudie: Azure Active Directory-integratie met Egnyte

In deze zelfstudie leert u hoe u Egnyte integreren met Azure Active Directory (Azure AD).

Egnyte integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Egnyte heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Egnyte (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Egnyte, moet u de volgende items:

- Een Azure AD-abonnement
- Een Egnyte eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Egnyte uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-egnyte-from-the-gallery"></a>Egnyte uit de galerie toe te voegen

Voor het configureren van de integratie van Egnyte in Azure AD, moet u Egnyte uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Egnyte uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Egnyte**, selecteer **Egnyte** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Egnyte in de lijst met resultaten](./media/egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Egnyte op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Egnyte is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Egnyte tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Egnyte, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Egnyte](#creating-an-egnyte-test-user)**  : als u wilt een equivalent van Britta Simon in Egnyte die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Egnyte.

**Voor het configureren van Azure AD eenmalige aanmelding met Egnyte, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Egnyte** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Egnyte domein en URL's, eenmalige aanmelding informatie](./media/egnyte-tutorial/tutorial_egnyte_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.egnyte.com`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Egnyte Client ondersteuningsteam](https://www.egnyte.com/corp/contact_egnyte.html) deze waarde op te halen. 

5. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/egnyte-tutorial/tutorial_egnyte_certificate.png) 

6. Op de **Egnyte instellen** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

    ![Egnyte configuratie](common/configuresection.png)

7. In een ander browservenster aanmelden bij uw bedrijf Egnyte site als beheerder.

8. Klik op **Instellingen**.
   
    ![Settings](./media/egnyte-tutorial/ic787819.png "Settings")

9. In het menu, klikt u op **instellingen**.

    ![Settings](./media/egnyte-tutorial/ic787820.png "Settings")

10. Klik op de **configuratie** tabblad en klik vervolgens op **Security**.

    ![Beveiliging](./media/egnyte-tutorial/ic787821.png "Beveiliging")

11. In de **verificatie met eenmalige aanmelding** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding op verificatie](./media/egnyte-tutorial/ic787822.png "eenmalige aanmelding op verificatie")   
    
    a. Als **verificatie met eenmalige aanmelding**, selecteer **SAML 2.0**.
   
    b. Als **id-provider**, selecteer **AzureAD**.
   
    c. Plakken **aanmeldings-URL** gekopieerd vanuit Azure portal in de **aanmeldings-URL van id-provider** tekstvak.
   
    d. Plakken **Azure AD-id** die u hebt gekopieerd vanuit Azure portal in de **Identity provider entiteit-ID** tekstvak.
      
    e. Uw base-64 gecodeerde certificaat openen in Kladblok gedownload vanuit Azure portal, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **provider identiteitscertificaat** tekstvak.
   
    f. Als **standaard Gebruikerstoewijzing**, selecteer **e-mailadres**.
   
    g. Als **gebruiken domeinspecifieke uitgeverwaarde**, selecteer **uitgeschakeld**.
   
    h. Klik op **Opslaan**.

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

### <a name="creating-an-egnyte-test-user"></a>Het maken van een testgebruiker Egnyte

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Egnyte, moeten ze worden ingericht voor Egnyte. In het geval van Egnyte is inrichten een handmatige taak.

**Voer de volgende stappen uit om een gebruikersaccount in te richten:**

1. Meld u aan bij uw **Egnyte** bedrijf site als administrator.

2. Ga naar **instellingen \> gebruikers en groepen**.

3. Klik op **Add New User**, en selecteer vervolgens het type van de gebruiker die u wilt toevoegen.
   
    ![Gebruikers](./media/egnyte-tutorial/ic787824.png "Gebruikers")

4. In de **nieuwe hoofdgebruiker** sectie, voert u de volgende stappen uit:
    
    ![Nieuwe standaardgebruiker](./media/egnyte-tutorial/ic787825.png "nieuwe standaardgebruiker")   

    a. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals **Brittasimon@contoso.com**.

    b. In **gebruikersnaam** tekst voert u de gebruikersnaam van gebruiker, zoals **Brittasimon**.

    c. Selecteer **Single Sign-On** als **verificatietype**.
   
    d. Klik op **Opslaan**.
    
    >[!NOTE]
    >De houder van Azure Active Directory-account ontvangt een e-mailmelding.
    >

>[!NOTE]
>U kunt alle andere Egnyte gebruiker-account maken van hulpprogramma's of API's geleverd door Egnyte aan inrichten AAD-gebruikersaccounts.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Egnyte.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Egnyte**.

    ![Eenmalige aanmelding configureren](./media/egnyte-tutorial/tutorial_egnyte_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Egnyte in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Egnyte.
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
