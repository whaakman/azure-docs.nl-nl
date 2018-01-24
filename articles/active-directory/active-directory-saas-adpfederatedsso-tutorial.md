---
title: 'Zelfstudie: Azure Active Directory-integratie met ADP federatieve SSO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ADP federatieve eenmalige aanmelding.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: jeedes
ms.openlocfilehash: d2049e3bc8a35792703d06374263466ddc35b5ce
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-adp-federated-sso"></a>Zelfstudie: Azure Active Directory-integratie met ADP federatieve SSO

In deze zelfstudie leert u hoe ADP federatieve SSO integreren met Azure Active Directory (Azure AD).

ADP federatieve SSO integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ADP federatieve eenmalige aanmelding heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ADP federatieve SSO (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ADP federatieve SSO, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement ADP federatieve SSO is ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ADP federatieve SSO uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adp-federated-sso-from-the-gallery"></a>ADP federatieve SSO uit de galerie toevoegen
Voor het configureren van de integratie van ADP federatieve SSO in Azure AD, moet u ADP federatieve SSO uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ADP federatieve SSO uit de galerie, moet u de volgende stappen uitvoeren:**

1.  Meld u aan uw Microsoft Azure-omgeving identiteit provider aan als beheerder.

2. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

3. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
4. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

5. Typ in het zoekvak **ADP federatieve SSO**, selecteer **ADP federatieve SSO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ADP federatieve SSO in de lijst met resultaten](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met ADP federatieve eenmalige aanmelding op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ADP federatieve eenmalige aanmelding is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in ADP federatieve eenmalige aanmelding tot stand worden gebracht.

Wijs in ADP federatieve SSO, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ADP federatieve SSO, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker ADP federatieve SSO](#create-an-adp-federated-sso-test-user)**  - ADP federatieve eenmalige aanmelding die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing ADP federatieve eenmalige aanmelding.

**Voor het configureren van Azure AD eenmalige aanmelding met ADP federatieve SSO, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ADP federatieve SSO** integratie toepassingspagina, klik op **tabblad Eigenschappen** en voer de volgende stappen uit: 

    ![Eigenschappen voor eenmalige aanmelding](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_prop.png)

    a. Stel de **ingeschakeld voor gebruikers om aan te melden** veldwaarde naar **Ja**.

    b. Kopiëren de **toegangs-URL van gebruiker** en u plak deze in **configureren aanmeldings-URL sectie**, die verderop in de zelfstudie wordt uitgelegd.

    c. Stel de **Gebruikerstoewijzing vereist** veld waarde te **Ja**.

    d. Stel de **zichtbaar voor gebruikers** veldwaarde naar **Nee**.

2. Klik op **eenmalige aanmelding** op **ADP federatieve SSO** pagina van de integratie van toepassingen.

    ![Koppeling voor eenmalige aanmelding configureren][4]

3. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_samlbase.png)

4. Op de **ADP federatieve SSO-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![ADP federatieve SSO-domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_url.png)

    In de **id** textbox, typ een URL:`https://fed.adp.com/` 
    
5. De toepassing ADP federatieve SSO verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. De naam van de claim wordt altijd worden **'PersonImmutableID'** en waarvan de waarde die we hebben toegewezen aan **werknemer-id**. 

    Hier wordt de gebruiker toewijzen van Azure AD aan ADP federatieve SSO worden uitgevoerd op de **werknemer-id** , maar u kunt dit toewijzen aan een andere waarde op basis van de toepassingsinstellingen van uw. Dus neem werk met [ADP ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) eerst voor het gebruik van de juiste id van een gebruiker en wijs die waarde met de **'PersonImmutableID'** claim.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | PersonImmutableID | user.employeeid |
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

    > [!NOTE] 
    > Voordat u de SAML-bevestiging configureren kunt, moet u contact op met uw [ADP ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) en vraagt u de waarde van het kenmerk unieke id voor uw tenant. U moet deze waarde voor het configureren van de aangepaste claim voor uw toepassing. 

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_certificate.png) 

8. Eenmalige aanmelding configureren op **ADP federatieve SSO** zijde, moet u de gedownloade uploaden **Metadata XML** op de [ADP federatieve SSO website](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Dit proces duurt een paar dagen. 

### <a name="configure-your-adp-services-for-federated-access"></a>Uw ADP service (s) voor federatieve toegang configureren

>[!Important]
> Uw werknemers die behoefte hebben aan federatieve toegang tot uw services ADP moeten worden toegewezen aan de ADP service-app en vervolgens, gebruikers moeten opnieuw worden toegewezen aan de specifieke ADP-service.
Bij ontvangst van de bevestiging van uw vertegenwoordiger ADP uw ADP (s) en het toewijzen of beheren de gebruikers om te bepalen van de gebruikerstoegang tot de specifieke ADP-service te configureren.

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **ADP federatieve SSO**, selecteer **ADP federatieve SSO** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ADP federatieve SSO in de lijst met resultaten](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_addservicegallery.png)

5. In de Azure-portal op uw **ADP federatieve SSO** integratie toepassingspagina, klik op **tabblad Eigenschappen** en voer de volgende stappen uit:  

    ![Linkedproperties voor eenmalige aanmelding](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linkedproperties.png)

    a.  Stel de **ingeschakeld voor gebruikers om aan te melden** veldwaarde naar **Ja**.

    b.  Stel de **Gebruikerstoewijzing vereist** veld waarde te **Ja**.

    c.  Stel de **zichtbaar voor gebruikers** veldwaarde naar **Ja**.

6. Klik op **eenmalige aanmelding** op **ADP federatieve SSO** pagina van de integratie van toepassingen.

    ![Koppeling voor eenmalige aanmelding configureren][4]

7. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **gekoppelde aanmelding**. koppelen van uw toepassing **ADP federatieve SSO**.

    ![Eenmalige aanmelding gekoppeld](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linked.png)

8. Navigeer naar de **configureren aanmeldings-URL** sectie, voert u de volgende stappen uit:

    ![Prop voor eenmalige aanmelding](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linkedsignon.png)
                                                              
    a. Plak de **toegangs-URL van gebruiker**, die u hebt gekopieerd vanaf boven **tabblad Eigenschappen** (van de belangrijkste ADP federatieve SSO-app).
                                                             
    b. Hieronder vindt u de 5 apps die ondersteuning bieden voor verschillende **Relay status URL's**. U moet de juiste toevoegen **Relay status URL** waarde voor een bepaalde toepassing handmatig de **toegangs-URL van gebruiker**.
    
    * **Nu ADP medewerkers**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP medewerkers nu een verbeterde tijd**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Sla** uw wijzigingen.

10. Bij ontvangst van de bevestiging van uw vertegenwoordiger ADP test met één of twee gebruikers te starten.

    a. Enkele gebruikers toewijzen aan de service ADP App federatieve toegang krijgen tot testen.

    b. Test is geslaagd als gebruikers toegang de app ADP-service op de galerie tot en toegang hun ADP-service tot.
 
11. Toewijzen op bevestiging van een geslaagde test de federatieve ADP-service op individuele gebruikers of gebruikersgroepen, die verderop in de zelfstudie wordt uitgelegd en implementeer uw werknemers. 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-adp-federated-sso-test-user"></a>Maken van een testgebruiker ADP federatieve SSO

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in ADP federatieve eenmalige aanmelding. Werken met [ADP ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) de gebruikers in het ADP federatieve SSO-account toevoegen.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan ADP federatieve eenmalige aanmelding.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen ADP federatieve SSO, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ADP federatieve SSO**.

    ![De koppeling ADP federatieve eenmalige aanmelding in de lijst met toepassingen](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel ADP federatieve SSO in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing ADP federatieve eenmalige aanmelding.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_203.png

