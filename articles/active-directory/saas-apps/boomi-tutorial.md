---
title: 'Zelfstudie: Azure Active Directory-integratie met Boomi | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Boomi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 40d034ff-7394-4713-923d-1f8f2ed8bf36
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: jeedes
ms.openlocfilehash: e0128d4422c462d4424583306af0b30174178bac
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049249"
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Zelfstudie: Azure Active Directory-integratie met Boomi

In deze zelfstudie leert u hoe u Boomi integreren met Azure Active Directory (Azure AD).

Boomi integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Boomi heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Boomi (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Boomi, moet u de volgende items:

- Een Azure AD-abonnement
- Een Boomi eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Boomi uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-boomi-from-the-gallery"></a>Boomi uit de galerie toe te voegen
Voor het configureren van de integratie van Boomi in Azure AD, moet u Boomi uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Boomi uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Boomi**, selecteer **Boomi** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Boomi in de lijst met resultaten](./media/boomi-tutorial/tutorial_boomi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Boomi op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Boomi is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Boomi tot stand worden gebracht.

In Boomi, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Boomi, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Boomi](#create-a-boomi-test-user)**  : als u wilt een equivalent van Britta Simon in Boomi die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Boomi.

**Voor het configureren van Azure AD eenmalige aanmelding met Boomi, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Boomi** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/boomi-tutorial/tutorial_boomi_samlbase.png)

3. Op de **Boomi domein en URL's** sectie, voert u de volgende stappen uit:

    ![Boomi domein en URL's, eenmalige aanmelding informatie](./media/boomi-tutorial/tutorial_boomi_url.png)

    a. In de **id** tekstvak, een URL typen: `https://platform.boomi.com/`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://platform.boomi.com/sso/<boomi-tenant>/saml`

    > [!NOTE] 
    > De antwoord-URL-waarde is niet echt. Werk de waarde bij met de werkelijke antwoord-URL. Neem contact op met [Boomi ondersteuningsteam](https://boomi.com/company/contact/) om de waarde.
 
4. Boomi toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/boomi-tutorial/tutorial_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster uitvoeren voor elke rij in de onderstaande tabel wordt weergegeven op de volgende stappen uit:

    | Naam kenmerk | Waarde kenmerk |
    | -------------- | --------------- |
    | FEDERATION_ID | User.mail |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/boomi-tutorial/tutorial_officespace_04.png)
    
    ![Eenmalige aanmelding configureren](./media/boomi-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/boomi-tutorial/tutorial_boomi_certificate.png) 

7. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/boomi-tutorial/tutorial_general_400.png)

8. Op de **Boomi configuratie** sectie, klikt u op **configureren Boomi** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Boomi configuratie](./media/boomi-tutorial/tutorial_boomi_configure.png) 

9. Meld u in een ander browservenster in uw bedrijf Boomi site als beheerder. 

10. Navigeer naar **bedrijfsnaam** en Ga naar **instellen**.

11. Klik op de **opties voor eenmalige aanmelding** tabblad en Voer onderstaande stappen.

    ![Single Sign-On aan App configureren](./media/boomi-tutorial/tutorial_boomi_11.png)

    a. Controleer **inschakelen SAML Single Sign-On** selectievakje.

    b. Klik op **importeren** voor het uploaden van het gedownloade certificaat uit Azure AD naar **Provider identiteitscertificaat**.
    
    c. In de **aanmeldings-URL van id-Provider** tekstvak plaatst de waarde van **Single Sign-On Service URL voor SAML** van het venster van Azure AD-toepassing-configuratie.

    d. Als **Federation-Id locatie**, selecteer **Federation-Id is in FEDERATION_ID kenmerkelement** keuzerondje. 

    e. Klik op **opslaan** knop.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/boomi-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/boomi-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/boomi-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/boomi-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-boomi-test-user"></a>Maak een testgebruiker Boomi

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Boomi, moeten ze worden ingericht voor Boomi. In het geval van Boomi is inrichten een handmatige taak.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1. Meld u aan bij uw bedrijf Boomi site aan als beheerder.

2. Na het aanmelden, gaat u naar **Gebruikersbeheer** en Ga naar **gebruikers**.

    ![Gebruikers](./media/boomi-tutorial/tutorial_boomi_001.png "gebruikers")

3. Klik op **+** pictogram en de **toevoegen/onderhouden gebruikersrollen** dialoogvenster wordt geopend.

    ![Gebruikers](./media/boomi-tutorial/tutorial_boomi_002.png "gebruikers")

    ![Gebruikers](./media/boomi-tutorial/tutorial_boomi_003.png "gebruikers")

    a. In de **e-mailadres gebruiker** tekstvak, typ het e-mailadres van gebruiker, zoals BrittaSimon@contoso.com.
    
    b. In de **voornaam** tekstvak, de eerste naam van gebruiker, zoals Julia.

    c. In de **achternaam** tekstvak typt u de achternaam van de gebruiker, zoals Simon.
    
    d. Voer van de gebruiker **Federatie-ID**. Elke gebruiker moet een unieke identificatie van de gebruiker binnen het account Federation-ID hebben.
    
    e. Toewijzen de **standaardgebruiker** rol aan de gebruiker. Wijs de rol beheerder geen omdat dat zou hem normale lucht toegang, evenals de eenmalige aanmelding toegang geven.
    
    f. Klik op **OK**.
    
    > [!NOTE]
    > De gebruiker ontvangt geen een e-mailmelding Welkom met een wachtwoord dat kan worden gebruikt voor aanmelding bij het account AtomSphere omdat zijn/haar wachtwoord wordt beheerd via de id-provider. U kunt alle andere Boomi gebruiker-account maken van hulpprogramma's of API's geleverd door Boomi aan inrichten AAD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Boomi.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Boomi toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Boomi**.

    ![De koppeling Boomi in de lijst met toepassingen](./media/boomi-tutorial/tutorial_boomi_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Boomi in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Boomi.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/boomi-tutorial/tutorial_general_01.png
[2]: ./media/boomi-tutorial/tutorial_general_02.png
[3]: ./media/boomi-tutorial/tutorial_general_03.png
[4]: ./media/boomi-tutorial/tutorial_general_04.png

[100]: ./media/boomi-tutorial/tutorial_general_100.png

[200]: ./media/boomi-tutorial/tutorial_general_200.png
[201]: ./media/boomi-tutorial/tutorial_general_201.png
[202]: ./media/boomi-tutorial/tutorial_general_202.png
[203]: ./media/boomi-tutorial/tutorial_general_203.png

