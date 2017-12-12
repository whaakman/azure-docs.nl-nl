---
title: 'Zelfstudie: Azure Active Directory-integratie met Litmos | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Litmos.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 2447cc5dbb01aa718e4418386499c6e50e9f5184
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Zelfstudie: Azure Active Directory-integratie met Litmos

In deze zelfstudie leert u hoe Litmos integreren met Azure Active Directory (Azure AD).

Litmos integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Litmos heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Litmos (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Litmos, moet u de volgende items:

- Een Azure AD-abonnement
- Een Litmos eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Litmos uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-litmos-from-the-gallery"></a>Litmos uit de galerie toevoegen
Voor het configureren van de integratie van Litmos in Azure AD, moet u Litmos uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Litmos uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Litmos**, selecteer **Litmos** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Litmos in de lijst met resultaten](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Litmos op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Litmos is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Litmos tot stand worden gebracht.

Wijs in Litmos, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Litmos, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Litmos](#create-a-litmos-test-user)**  - Litmos die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Litmos configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Litmos, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Litmos** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_samlbase.png)

3. Op de **Litmos domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Litmos domein eenmalige aanmelding informatie](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.litmos.com/account/Login`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.litmos.com/integration/samllogin`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id en de antwoord-URL, die worden beschreven verderop in de zelfstudie of neem contact op met [Litmos ondersteuningsteam](https://www.litmos.com/contact-us/) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_certificate.png)

5. Als onderdeel van de configuratie, moet u voor het aanpassen van de **SAML-Token kenmerken** voor uw toepassing Litmos.

    ![Kenmerk-sectie](./media/active-directory-saas-litmos-tutorial/tutorial_attribute.png)
           
    | Kenmerknaam   | Waarde kenmerk |   
    | ---------------  | ----------------|
    | Voornaam |User.givenName |
    | Achternaam  |User.surname |
    | E-mail |User.mail |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Kenmerk toevoegen](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_04.png)

    ![Kenmerk Dailog toevoegen](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.     

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-litmos-tutorial/tutorial_general_400.png)

7. In een ander browservenster aanmelding bij uw bedrijf Litmos site als administrator.

8. Klik in de navigatiebalk aan de linkerkant op **Accounts**.
   
    ![Het gedeelte accounts App-zijde][22] 

9. Klik op de **integraties** tabblad.
   
    ![Tabblad-integratie][23] 

10. Op de **integraties** tabblad, schuif omlaag naar **3e partij integraties**, en klik vervolgens op **SAML 2.0** tabblad.
   
    ![SAML 2.0 sectie][24] 

11. Kopieer de waarde onder **de SAML-eindpunt voor litmos is:** en plak deze in de **antwoord-URL** textbox in de **Litmos domein en de URL's** sectie in Azure-portal. 
   
    ![SAML-eindpunt][26] 

12. In uw **Litmos** toepassing, de volgende stappen uitvoeren:
    
     ![Litmos toepassing][25] 
     
     a. Klik op **SAML inschakelen**.
    
     b. Open uw base-64 gecodeerde certificaat in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **SAML X.509-certificaat** textbox.
     
     c. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-litmos-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-litmos-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-litmos-test-user"></a>Een testgebruiker Litmos maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Litmos genoemd.  
De toepassing Litmos ondersteunt Just-in-Time-inrichting. Dit betekent een gebruikersaccount wordt automatisch gemaakt indien nodig tijdens een poging tot toegang tot de toepassing die het toegangsvenster gebruikt.

**Als u wilt een gebruiker Britta Simon aangeroepen in Litmos maakt, moet u de volgende stappen uitvoeren:**

1. In een ander browservenster aanmelding bij uw bedrijf Litmos site als administrator.

2. Klik in de navigatiebalk aan de linkerkant op **Accounts**.
   
    ![Het gedeelte accounts App-zijde][22] 

3. Klik op de **integraties** tabblad.
   
    ![Tabblad integraties][23] 

4. Op de **integraties** tabblad, schuif omlaag naar **3e partij integraties**, en klik vervolgens op **SAML 2.0** tabblad.
   
    ![SAML 2.0][24] 
    
5. Selecteer **Autogenerate gebruikers**
   
    ![Gebruikers automatisch genereren][27] 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Litmos.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Litmos, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Litmos**.

    ![De koppeling Litmos in de lijst met toepassingen](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.  

Als u op de tegel Litmos in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Litmos. 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png

