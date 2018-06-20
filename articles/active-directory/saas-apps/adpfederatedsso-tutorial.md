---
title: 'Zelfstudie: Azure Active Directory-integratie met ADP | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ADP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2018
ms.author: jeedes
ms.openlocfilehash: 7e7ecce9129f3bf9f70a2ba153ba6ba47bd27731
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209881"
---
# <a name="tutorial-azure-active-directory-integration-with-adp"></a>Zelfstudie: Azure Active Directory-integratie met ADP

In deze zelfstudie leert u hoe ADP integreren met Azure Active Directory (Azure AD).

ADP integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ADP heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ADP (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ADP, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement ADP ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ADP uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adp-from-the-gallery"></a>ADP uit de galerie toevoegen
Voor het configureren van de integratie van ADP in Azure AD, moet u ADP uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ADP uit de galerie, moet u de volgende stappen uitvoeren:**

1.  Meld u aan uw Microsoft Azure-omgeving identiteit provider aan als beheerder.

2. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

3. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
4. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

5. Typ in het zoekvak **ADP**, selecteer **ADP** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ADP in de lijst met resultaten](./media/adpfederatedsso-tutorial/tutorial_adp_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met ADP op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ADP is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in ADP tot stand worden gebracht.

Wijs in ADP, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ADP, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker ADP](#create-an-adp-test-user)**  - ADP die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing ADP configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met ADP, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ADP** integratie toepassingspagina, klik op **tabblad Eigenschappen** en voer de volgende stappen uit: 

    ![Eigenschappen voor eenmalige aanmelding](./media/adpfederatedsso-tutorial/tutorial_adp_prop.png)

    a. Stel de **ingeschakeld voor gebruikers om aan te melden** veldwaarde naar **Ja**.

    b. Kopiëren de **toegangs-URL van gebruiker** en u plak deze in **configureren aanmeldings-URL sectie**, die verderop in de zelfstudie wordt uitgelegd.

    c. Stel de **Gebruikerstoewijzing vereist** veld waarde te **Ja**.

    d. Stel de **zichtbaar voor gebruikers** veldwaarde naar **Nee**.

2. Klik op **eenmalige aanmelding** op **ADP** pagina van de integratie van toepassingen.

    ![Koppeling voor eenmalige aanmelding configureren][4]

3. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/adpfederatedsso-tutorial/tutorial_adp_samlbase.png)

4. Op de **ADP domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en ADP domein eenmalige aanmelding informatie](./media/adpfederatedsso-tutorial/tutorial_adp_url.png)

    In de **id** textbox, typ een URL: `https://fed.adp.com` 
    
5. De toepassing ADP verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. De naam van de claim wordt altijd worden **'PersonImmutableID'** en waarvan de waarde die we hebben toegewezen aan **werknemer-id**. 

    Hier wordt de gebruiker toewijzen van Azure AD aan ADP worden uitgevoerd op de **werknemer-id** , maar u kunt dit toewijzen aan een andere waarde op basis van de toepassingsinstellingen van uw. Dus neem werk met [ADP ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) eerst voor het gebruik van de juiste id van een gebruiker en wijs die waarde met de **'PersonImmutableID'** claim.

    ![Eenmalige aanmelding configureren](./media/adpfederatedsso-tutorial/tutorial_adp_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | PersonImmutableID | User.EmployeeID |
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/adpfederatedsso-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/adpfederatedsso-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

    > [!NOTE] 
    > Voordat u de SAML-bevestiging configureren kunt, moet u contact op met uw [ADP ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) en vraagt u de waarde van het kenmerk unieke id voor uw tenant. U moet deze waarde voor het configureren van de aangepaste claim voor uw toepassing. 

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/adpfederatedsso-tutorial/tutorial_adp_certificate.png) 

8. Eenmalige aanmelding configureren op **ADP** zijde, moet u de gedownloade uploaden **Metadata XML** op de [ADP website](https://adpfedsso.adp.com/public/login/index.fcc).

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

4. Typ in het zoekvak **ADP**, selecteer **ADP** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ADP in de lijst met resultaten](./media/adpfederatedsso-tutorial/tutorial_adp_addservicegallery.png)

5. In de Azure-portal op uw **ADP** integratie toepassingspagina, klik op **tabblad Eigenschappen** en voer de volgende stappen uit:  

    ![Linkedproperties voor eenmalige aanmelding](./media/adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

    a.  Stel de **ingeschakeld voor gebruikers om aan te melden** veldwaarde naar **Ja**.

    b.  Stel de **Gebruikerstoewijzing vereist** veld waarde te **Ja**.

    c.  Stel de **zichtbaar voor gebruikers** veldwaarde naar **Ja**.

6. Klik op **eenmalige aanmelding** op **ADP** pagina van de integratie van toepassingen.

    ![Koppeling voor eenmalige aanmelding configureren][4]

7. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **gekoppelde aanmelding**. koppelen van uw toepassing **ADP**.

    ![Eenmalige aanmelding gekoppeld](./media/adpfederatedsso-tutorial/tutorial_adp_linked.png)

8. Navigeer naar de **configureren aanmeldings-URL** sectie, voert u de volgende stappen uit:

    ![Prop voor eenmalige aanmelding](./media/adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)
                                                              
    a. Plak de **toegangs-URL van gebruiker**, die u hebt gekopieerd vanaf boven **tabblad Eigenschappen** (van de belangrijkste ADP app).
                                                             
    b. Hieronder vindt u de 5 apps die ondersteuning bieden voor verschillende **Relay status URL's**. U moet de juiste toevoegen **Relay status URL** waarde voor een bepaalde toepassing handmatig de **toegangs-URL van gebruiker**.
    
    * **Nu ADP medewerkers**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP medewerkers nu een verbeterde tijd**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Sla** uw wijzigingen.

10. Bij ontvangst van de bevestiging van uw vertegenwoordiger ADP test met één of twee gebruikers te starten.

    a. Enkele gebruikers toewijzen aan de service ADP App federatieve toegang krijgen tot testen.

    b. Test is geslaagd als gebruikers toegang de app ADP-service op de galerie tot en toegang hun ADP-service tot.
 
11. Toewijzen op bevestiging van een geslaagde test de federatieve ADP-service op individuele gebruikers of gebruikersgroepen, die verderop in de zelfstudie wordt uitgelegd en implementeer uw werknemers. 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/adpfederatedsso-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/adpfederatedsso-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/adpfederatedsso-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/adpfederatedsso-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-adp-test-user"></a>Een testgebruiker ADP maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in ADP genoemd. Werken met [ADP ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) de gebruikers in het account ADP toevoegen.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan ADP.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen ADP, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ADP**.

    ![De koppeling ADP in de lijst met toepassingen](./media/adpfederatedsso-tutorial/tutorial_adp_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel ADP in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing ADP.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/adpfederatedsso-tutorial/tutorial_general_01.png
[2]: ./media/adpfederatedsso-tutorial/tutorial_general_02.png
[3]: ./media/adpfederatedsso-tutorial/tutorial_general_03.png
[4]: ./media/adpfederatedsso-tutorial/tutorial_general_04.png

[100]: ./media/adpfederatedsso-tutorial/tutorial_general_100.png

[200]: ./media/adpfederatedsso-tutorial/tutorial_general_200.png
[201]: ./media/adpfederatedsso-tutorial/tutorial_general_201.png
[202]: ./media/adpfederatedsso-tutorial/tutorial_general_202.png
[203]: ./media/adpfederatedsso-tutorial/tutorial_general_203.png

