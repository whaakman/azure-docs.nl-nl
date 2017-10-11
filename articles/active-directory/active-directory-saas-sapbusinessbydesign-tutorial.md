---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Business ByDesign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ab76a0ac1ef954efd3c66e6f565514b889ed9444
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign

In deze zelfstudie leert u hoe SAP Business ByDesign integreren met Azure Active Directory (Azure AD).

SAP Business ByDesign integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot een SAP Business ByDesign heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAP Business ByDesign (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Business ByDesign, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP Business ByDesign eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP Business ByDesign uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP Business ByDesign uit de galerie toevoegen
Voor het configureren van de integratie van SAP Business ByDesign in Azure AD, moet u een SAP Business ByDesign uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP Business ByDesign uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SAP Business ByDesign**, selecteer **SAP Business ByDesign** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SAP Business ByDesign in de lijst met resultaten](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met SAP Business ByDesign op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SAP Business ByDesign is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in een SAP Business ByDesign tot stand worden gebracht.

In een SAP Business ByDesign, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP Business ByDesign, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SAP Business ByDesign testgebruiker](#create-an-sap-business-bydesign-test-user)**  - SAP Business ByDesign die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing SAP Business ByDesign configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP Business ByDesign, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP Business ByDesign** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. Op de **SAP Business ByDesign domein en URL's** sectie, voert u de volgende stappen uit:

    ![URL's en SAP Business ByDesign domein eenmalige aanmelding informatie](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<servername>.sapbydesign.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<servername>.sapbydesign.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [SAP Business ByDesign Client ondersteuningsteam](https://www.sap.com/products/cloud-analytics.support.html) ophalen van deze waarden.

4. Op de **gebruikerskenmerken** sectie, voert u de volgende stappen uit:

    ![SAP Business ByDesign kenmerk sectie](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    a. In **gebruikers-id** lijst, selecteert de **ExtractMailPrefix()** functie.
    
    b. Van de **Mail** , selecteert u het gebruikerskenmerk die u wilt gebruiken voor uw implementatie. Bijvoorbeeld, als u wilt gebruiken van de werknemer-id als unieke gebruikers-id en u de waarde van het kenmerk in de ExtensionAttribute2 hebt opgeslagen, selecteert u user.extensionattribute2.     

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. Op de **SAP Business ByDesign configuratie** sectie, klikt u op **configureren SAP Business ByDesign** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![SAP Business ByDesign-configuratie](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. Als u eenmalige aanmelding voor uw toepassing is geconfigureerd, moet u de volgende stappen uitvoeren:
   
    a. Meld u aan op uw portal SAP Business ByDesign met beheerdersrechten.
   
    b. Navigeer naar **toepassings- en algemene beheertaak gebruiker** en klik op de **identiteitsprovider** tabblad.
   
    c. Klik op **nieuwe identiteitsprovider** en selecteert u de XML-bestand met metagegevens die u hebt gedownload vanuit de Azure-portal. U importeert de metagegevens, uploadt het systeem automatisch de vereiste handtekeningcertificaat en een versleutelingscertificaat.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    d. Om op te nemen de **Assertion Consumer Service-URL** selecteren in het SAML-aanvraag **Assertion Consumer Service-URL opnemen**.
   
    e. Klik op **activeren Single Sign-On**.
   
    f. Sla uw wijzigingen op.
   
    g. Klik op de **mijn systeem** tabblad.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    h. Plakken **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit de Azure-portal in de **Azure AD aanmelding URL** textbox.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    ik. Opgeven of de werknemer handmatig kiezen kunt tussen het aanmelden met gebruikersnaam en wachtwoord of eenmalige aanmelding door te selecteren **handmatige identiteit Provider selectie**.
   
    j. In de **URL SSO** sectie, de URL opgeven die moet worden gebruikt door de werknemer voor aanmelding bij het systeem. 
    In de URL naar verzonden werknemer vervolgkeuzelijst wordt weergegeven, kunt u kiezen uit de volgende opties:
   
    **Niet-SSO-URL**
   
    Het systeem wordt de URL van de normale system verzendt naar de werknemer. De werknemer kan niet aanmelden met eenmalige aanmelding, en moet gebruik wachtwoord of certificaat in plaats daarvan.
   
    **URL VOOR EENMALIGE AANMELDING** 
   
    Het systeem verzendt alleen de SSO-URL naar de werknemer. De werknemer kunt aanmelden met behulp van eenmalige aanmelding. Authenticatie-aanvraag wordt via de IdP omgeleid.
   
    **Automatische selectie**
   
    Als eenmalige aanmelding niet actief is, wordt de URL van de normale systeem door het systeem verzonden naar de werknemer. Als eenmalige aanmelding actief is, controleert het systeem of de werknemer een wachtwoord heeft. Als een wachtwoord beschikbaar is, worden zowel de URL van de SSO- en Non-SSO-verzonden naar de werknemer. Als de werknemer geen wachtwoord heeft, wordt echter alleen de URL voor eenmalige aanmelding verzonden naar de werknemer.
   
    k. Sla uw wijzigingen op.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-sap-business-bydesign-test-user"></a>Een SAP Business ByDesign testgebruiker maken

In deze sectie maakt maken u een gebruiker Britta Simon aangeroepen in een SAP Business ByDesign. Neem contact op met [SAP Business ByDesign Client ondersteuningsteam](https://www.sap.com/products/cloud-analytics.support.html) om toe te voegen de gebruikers van het SAP Business ByDesign-platform. 

> [!NOTE]
> Zorg dat NameID waarde met het veld username van het SAP Business ByDesign-platform overeenkomen moet.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SAP Business ByDesign.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen SAP Business ByDesign, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP Business ByDesign**.

    ![De koppeling SAP Business ByDesign in de lijst met toepassingen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel SAP Business ByDesign in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing SAP Business ByDesign.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png

