---
title: 'Zelfstudie: Azure Active Directory-integratie met Replicon | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Replicon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: jeedes
ms.openlocfilehash: 130e13b0bb801c498f6de6e4bfd61bd3d2c3bf00
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Zelfstudie: Azure Active Directory-integratie met Replicon

In deze zelfstudie leert u hoe Replicon integreren met Azure Active Directory (Azure AD).

Replicon integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Replicon heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Replicon (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Replicon, moet u de volgende items:

- Een Azure AD-abonnement
- Een Replicon eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Replicon uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-replicon-from-the-gallery"></a>Replicon uit de galerie toevoegen
Voor het configureren van de integratie van Replicon in Azure AD, moet u Replicon uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Replicon uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Replicon**, selecteer **Replicon** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Replicon in de lijst met resultaten](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Replicon op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Replicon is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Replicon tot stand worden gebracht.

Wijs in Replicon, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Replicon, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Replicon](#create-a-replicon-test-user)**  - Replicon die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Replicon configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Replicon, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Replicon** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_samlbase.png)

3. Op de **Replicon domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en replicon domein eenmalige aanmelding informatie](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://na2.replicon.com/<companyname>/saml2/sp-sso/post`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://global.replicon.com/<companyname>`

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://global.replicon.com/!/saml2/<companyname>/sso/post`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [Replicon Client ondersteuningsteam](https://www.replicon.com/customerzone/contact-support) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-replicon-tutorial/tutorial_general_400.png)

6. In een ander browservenster, meld u bij uw bedrijf Replicon site als beheerder.

7. Voor het configureren van SAML 2.0, moet u de volgende stappen uitvoeren:
   
    ![SAML-verificatie inschakelen](./media/active-directory-saas-replicon-tutorial/ic777805.png "inschakelen SAML-verificatie")
    
    a. Om weer te geven de **EnableSAML Authentication2** dialoogvenster het volgende toevoegen aan de URL na de sleutel van uw bedrijf:`/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`
    
    * Hieronder ziet u het schema van de volledige URL:  
   `https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`
   
   b. Klik op de  **+**  uit te breiden de **v20Configuration** sectie.

   c. Klik op de  **+**  uit te breiden de **metaDataConfiguration** sectie.

   d. Klik op **bestand kiezen**, zodat uw identiteit provider metagegevens-XML-bestand selecteren en op **indienen**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-replicon-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-replicon-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-replicon-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-replicon-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-replicon-test-user"></a>Een testgebruiker Replicon maken

Om in te schakelen gebruikers van Azure AD aan te melden bij Replicon, moeten ze worden ingericht in Replicon. In het geval van Replicon is inrichting een handmatige taak.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. In een browservenster geopend, meld u bij uw bedrijf Replicon site als beheerder.

2. Ga naar **beheer \> gebruikers**.
   
    ![Gebruikers](./media/active-directory-saas-replicon-tutorial/ic777806.png "gebruikers")

3. Klik op **+ gebruiker toevoegen**.
   
    ![Gebruiker toevoegen](./media/active-directory-saas-replicon-tutorial/ic777807.png "gebruiker toevoegen")

4. In de **gebruikersprofiel** sectie, voert u de volgende stappen uit:
   
    ![Gebruikersprofiel](./media/active-directory-saas-replicon-tutorial/ic777808.png "gebruikersprofiel")
    
    a. In de **aanmeldingsnaam** textbox type de Azure AD-e-mailadres van de Azure AD-gebruiker die u inrichten wilt zoals  **BrittaSimon@contoso.com** .
    
    b. Als **verificatietype**, selecteer **SSO**.
    
    c. In de **afdeling** textbox, typ de afdeling van de gebruiker.

    d. Als **werknemertype**, selecteer **beheerder**.

    e. Klik op **gebruikersprofiel opslaat**.

>[!NOTE]
>U kunt andere Replicon gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Replicon voor het inrichten van Azure AD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Replicon.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Replicon, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Replicon**.

    ![De koppeling Replicon in de lijst met toepassingen](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Replicon in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Replicon.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_203.png

