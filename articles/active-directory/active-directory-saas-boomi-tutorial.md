---
title: 'Zelfstudie: Azure Active Directory-integratie met Boomi | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Boomi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 1121d22beddf73fd2109a4b410422f76dd37478e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Zelfstudie: Azure Active Directory-integratie met Boomi

In deze zelfstudie leert u hoe Boomi integreren met Azure Active Directory (Azure AD).

Boomi integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Boomi heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Boomi (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Boomi, moet u de volgende items:

- Een Azure AD-abonnement
- Een Boomi eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Boomi uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-boomi-from-the-gallery"></a>Boomi uit de galerie toevoegen
Voor het configureren van de integratie van Boomi in Azure AD, moet u Boomi uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Boomi uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Boomi**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. Selecteer in het deelvenster resultaten **Boomi**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Boomi op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Boomi is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Boomi tot stand worden gebracht.

Wijs in Boomi, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Boomi, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Boomi](#creating-a-boomi-test-user)**  - Boomi die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Boomi configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Boomi, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Boomi** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. Op de **Boomi domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://platform.boomi.com/sso/<accountname>/saml`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://platform.boomi.com/sso/<accountname>/saml`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL. Neem contact op met [Boomi ondersteuningsteam](https://boomi.com/company/contact/) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. De SAML-asserties verwacht Boomi toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster uitvoeren voor elke rij in de onderstaande tabel wordt weergegeven op de volgende stappen uit:

    | Naam van kenmerk | De waarde van kenmerk |
    | -------------- | --------------- |
    | FEDERATION_ID | User.mail |
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. Op de **Boomi configuratie** sectie, klikt u op **configureren Boomi** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. In een ander browservenster, meld u bij uw bedrijf Boomi site als beheerder. 

9. Navigeer naar **bedrijfsnaam** en Ga naar **instellen**.

10. Klik op de **SSO opties** tabblad en Voer onderstaande stappen te volgen.

    ![App-zijde eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    a. Controleer **inschakelen SAML Single Sign-On** selectievakje.

    b. Klik op **importeren** voor het uploaden van het gedownloade certificaat uit Azure AD **Provider identiteitscertificaat**.
    
    c. In de **identiteit Provider aanmeldings-URL** textbox, plaatst u de waarde van **SAML Single Sign-On Service-URL** van het venster voor configuratie van Azure AD-toepassing.

    d. Als **Federation Id locatie**, selecteer **Federation-Id wordt FEDERATION_ID kenmerk element** keuzerondje. 

    e. Klik op **opslaan** knop.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-boomi-test-user"></a>Een testgebruiker Boomi maken

Om Azure AD-gebruikers zich aanmelden bij Boomi inschakelt, moeten ze worden ingericht in Boomi. In het geval van Boomi is inrichting een handmatige taak.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1. Meld u aan bij uw bedrijf Boomi site als beheerder.

2. Na het aanmelden, gaat u naar **Gebruikersbeheer** en Ga naar **gebruikers**.

    ![Gebruikers](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "gebruikers")

3. Klik op  **+**  pictogram en de **toevoegen/onderhouden gebruikersrollen** dialoogvenster wordt geopend.

    ![Gebruikers](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "gebruikers")

    ![Gebruikers](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "gebruikers")

    a. In de **e-mailadres gebruiker** textbox, typ het e-mailadres van gebruiker, zoals BrittaSimon@contoso.com.
    
    b. In de **voornaam** textbox, de eerste naam van gebruiker zoals Britta.

    c. In de **achternaam** textbox, typt u de achternaam van de gebruiker zoals Simon.
    
    d. Voer de gebruiker **Federatie-ID**. Elke gebruiker moet een Federation-ID die een unieke identificatie van de gebruiker binnen het account hebben.
    
    e. Wijs de **standaardgebruiker** rol aan de gebruiker. Wijs de rol beheerder niet toe omdat die hem normale lucht toegang als één aanmelding toegang wilt geven.
    
    f. Klik op **OK**.
    
    > [!NOTE]
    > De gebruiker ontvangt een e-mailmelding Welkom met een wachtwoord dat kan worden gebruikt voor aanmelding bij het account AtomSphere omdat het wachtwoord wordt beheerd via de id-provider niet. U kunt andere Boomi gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Boomi aan inrichten AAD-gebruikersaccounts. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Boomi.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Boomi, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Boomi**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Boomi in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Boomi.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

