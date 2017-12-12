---
title: 'Zelfstudie: Azure Active Directory-integratie met Hightail | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Hightail.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2333a56cd9369cb66c4944304472462d2f2f7511
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Zelfstudie: Azure Active Directory-integratie met Hightail

In deze zelfstudie leert u hoe Hightail integreren met Azure Active Directory (Azure AD).

Hightail integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Hightail heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Hightail (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Hightail, moet u de volgende items:

- Een Azure AD-abonnement
- Een Hightail eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Hightail uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-hightail-from-the-gallery"></a>Hightail uit de galerie toevoegen
Voor het configureren van de integratie van Hightail in Azure AD, moet u Hightail uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Hightail uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Hightail**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. Selecteer in het deelvenster resultaten **Hightail**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Hightail op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Hightail is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Hightail tot stand worden gebracht.

Wijs in Hightail, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Hightail, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Hightail](#creating-a-hightail-test-user)**  - Hightail die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Hightail configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Hightail, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Hightail** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. Op de **Hightail domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     In de **antwoord-URL** textbox, typ de URL als:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`

    > [!NOTE] 
    > De voorgaande waarde is geen echte waarde. U kunt de waarde wordt bijgewerkt met de werkelijke antwoord-URL, die verderop in de zelfstudie wordt beschreven.
 
4. Op de **Hightail domein en de URL's** sectie als u wilt configureren van de toepassing in **SP geïnitieerd modus**, voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    a. Klik op de **weergeven geavanceerde instellingen voor URL**.

    b. In de **aanmelding op URL** textbox, typ de URL als:`https://www.hightail.com/loginSSO`

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. Hightail toepassing de SAML-asserties verwacht in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **'Atrribute'** tabblad van de toepassing. De volgende Schermafbeelding toont een voorbeeld voor deze. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Kenmerknaam | Waarde kenmerk |
    | ------------------- | -------------------- |
    | Voornaam | User.givenName |
    | Achternaam | User.surname |
    | E-mail | User.mail |    
    | UserIdentity | User.mail |
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. Op de **Hightail configuratie** sectie, klikt u op **configureren Hightail** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    >Voordat u configureert de eenmalige aanmelding op Hightail app, neemt u witte lijst uw e-maildomein met Hightail team, zodat alle gebruikers die dit domein functionaliteit voor eenmalige aanmelding kunnen gebruiken.


9. Als u eenmalige aanmelding voor uw toepassing is geconfigureerd, moet u eenmalige aanmelding voor uw tenant Hightail als beheerder.
   
    a. Klik in het menu bovenaan op de **Account** tabblad en selecteer **SAML configureren**.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    b. Schakel het selectievakje van **SAML-verificatie inschakelen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    c. De base-64 gecodeerde certificaat openen in Kladblok van Azure portal hebt gedownload, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **handtekeningcertificaat van SAML-Token** textbox.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    d. In de **SAML-instantie (id-Provider)** textbox, plak de waarde van **SAML Single Sign-On Service-URL** gekopieerd vanuit Azure-portal.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    e. Als u wilt configureren van de toepassing in **IDP geïnitieerd modus** Selecteer **'Identiteitsprovider (IdP) geïnitieerd aanmelden'**. Als **SP geïnitieerd modus** Selecteer **'Serviceprovider (SP) geïnitieerd aanmelden'**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    f. Kopieer de URL van de consument SAML voor uw exemplaar en plak deze in **antwoord-URL** textbox in **Hightail domein en de URL's** sectie op Azure-portal.
    
    g. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-hightail-test-user"></a>Een testgebruiker Hightail maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Hightail genoemd. 

Er is geen actie-item voor u in deze sectie. Hightail ondersteunt just in time gebruikers inrichten op basis van de aangepaste claims. Als u de aangepaste claims hebt geconfigureerd, zoals wordt weergegeven in de sectie  **[eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)**  , een gebruiker is gemaakt in de toepassing nog niet bestaat. 

>[!NOTE]
>Als u een gebruiker handmatig maken wilt, moet u contact op met de [Hightail ondersteuningsteam](mailto:support@hightail.com). 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Hightail.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Hightail, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Hightail**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u op de tegel Hightail in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Hightail.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

