---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke Access-beheerder | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler persoonlijke Access-beheerder.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c87392a7-e7fe-4cdc-a8e6-afe1ed975172
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: bf0b7cbd8047dfdbc1a4503775e6d36f8e8a67c1
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-administrator"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke Access-beheerder

In deze zelfstudie leert u hoe Zscaler persoonlijke toegang beheerder integreren met Azure Active Directory (Azure AD).

Zscaler persoonlijke toegang beheerder integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang voor Zscaler persoonlijke beheerder toegang heeft.
- U kunt uw gebruikers automatisch ophalen aangemelde Zscaler persoonlijke toegang beheerder (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler persoonlijke Access-beheerder, moet u de volgende items:

- Een Azure AD-abonnement
- Een beheerder van Zscaler persoonlijke toegang eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zscaler persoonlijke toegang beheerder toe te voegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zscaler-private-access-administrator-from-the-gallery"></a>Zscaler persoonlijke toegang beheerder toe te voegen uit de galerie
Voor het configureren van de integratie van Zscaler persoonlijke Access-beheerder in Azure AD, moet u Zscaler persoonlijke toegang beheerder uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler persoonlijke toegang beheerder uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Zscaler persoonlijke Access-beheerder**, selecteer **Zscaler persoonlijke toegang beheerder** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Zscaler persoonlijke toegang beheerder in de lijst met resultaten](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Zscaler persoonlijke beheerder toegang op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Zscaler persoonlijke Access-beheerder is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker beheerder in Zscaler persoonlijke toegang tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zscaler persoonlijke Access-beheerder, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Zscaler persoonlijke toegang beheerder](#create-a-zscaler-private-access-administrator-test-user)**  - hebben een equivalent van Britta Simon in Zscaler persoonlijke toegang die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Zscaler persoonlijke Access-beheerder.

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler persoonlijke Access-beheerder, kunt u de volgende stappen uitvoeren:**

1. In de Azure portal op de **Zscaler persoonlijke toegang beheerder** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_samlbase.png)

3. Op de **Zscaler persoonlijke toegang beheerder domein en de URL's** sectie als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Domein van de beheerder van Zscaler persoonlijke toegang en URL's eenmalige aanmelding informatie](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.private.zscaler.com/auth/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.private.zscaler.com/auth/sso`

    c. Controleer **weergeven geavanceerde instellingen voor de URL**

    d. In de **RelayState** textbox in een waarde: `idpadminsso`

4.  Als u wilt configureren van de toepassing in **SP** geïnitieerd modus de volgende stappen uitvoeren:

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<subdomain>.private.zscaler.com/auth/sso`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke URL voor de identificatie, antwoord-URL en eenmalige aanmelding. Neem contact op met [Zscaler persoonlijke toegang beheerder ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) ophalen van deze waarden.
 
5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_400.png)

7. In een ander browservenster, aanmelding naar Zscaler persoonlijke beheerder toegang als beheerder.

8. Klik op de bovenkant op **beheer** en navigeer naar **verificatie** sectie Klik **IdP configuratie**.

    ![Zscaler persoonlijke toegang beheerder admin](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

9. Klik in de rechterbovenhoek op **IdP-configuratie toevoegen**. 

    ![Addidp Zscaler persoonlijke Access-beheerder](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addpidp.png)

10. Op de **IdP-configuratie toevoegen** pagina de volgende stappen uitvoeren:
 
    ![Idpselect Zscaler persoonlijke Access-beheerder](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_idpselect.png)

    a. Klik op **bestand selecteren** voor het uploaden van het gedownloade bestand in de metagegevens van Azure AD in de **IdP metagegevens bestand laden** veld.

    b. Deze leest de **IdP metagegevens** van Azure AD en alle informatie in de velden gevuld zoals hieronder wordt weergegeven.

    ![Idpconfig Zscaler persoonlijke Access-beheerder](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/idpconfig.png)

    c. Selecteer **eenmalige aanmelding** als **beheerder**.

    d. Selecteer uw domein uit **domeinen** veld.
    
    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-zscaler-private-access-administrator-test-user"></a>Maak een testgebruiker Zscaler persoonlijke Access-beheerder

Om Azure AD-gebruikers zich aanmelden voor persoonlijke Zscaler Access-beheerder, als ze in Zscaler persoonlijke toegang beheerder worden ingericht. In het geval van Zscaler persoonlijke toegang beheerder inrichting is een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Zscaler persoonlijke toegang beheerder site als beheerder.

2. Klik op de bovenkant op **beheer** en navigeer naar **verificatie** sectie Klik **IdP configuratie**.

    ![Zscaler persoonlijke toegang beheerder admin](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Klik op **beheerders** vanaf de linkerkant van het menu.

    ![De beheerder Zscaler persoonlijke Access-beheerder](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_adminstrator.png)

4. Klik in de rechterbovenhoek op **beheerder toevoegen**:

    ![Zscaler persoonlijke toegang beheerder beheerder toevoegen](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addadmin.png)

5. In de **beheerder toevoegen** pagina, voert u de volgende stappen uit:

    ![Gebruikerbeheerder Zscaler persoonlijke Access-beheerder](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_useradmin.png)

    a. In de **gebruikersnaam** textbox, voer het e-mailadres van de gebruiker zoals  **BrittaSimon@contoso.com** .

    b. In de **wachtwoord** textbox, typt u het wachtwoord.

    c. In de **wachtwoord bevestigen** textbox, typt u het wachtwoord.

    d. Selecteer **rol** als **Zscaler persoonlijke toegang beheerder**.

    e. In de **e** textbox, voer het e-mailadres van de gebruiker zoals  **BrittaSimon@contoso.com** .

    f. In de **Phone** textbox, typt u het telefoonnummer.

    g. In de **tijdzone** textbox, selecteert u de tijdzone.

    h. Klik op **Opslaan**.  

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Zscaler persoonlijke Access-beheerder.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon naar Zscaler persoonlijke Access-beheerder, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Zscaler persoonlijke toegang beheerder**.

    ![De koppeling Zscaler persoonlijke Access-beheerder in de lijst met toepassingen](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Zscaler persoonlijke Access-beheerder in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Zscaler persoonlijke Access-beheerder.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_203.png

