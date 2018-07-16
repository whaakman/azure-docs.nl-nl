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
ms.openlocfilehash: 75b84c2856373126ceba0fc536e41d270f4d2d05
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048776"
---
# <a name="tutorial-azure-active-directory-integration-with-adp"></a>Zelfstudie: Azure Active Directory-integratie met ADP

In deze zelfstudie leert u hoe u ADP integreren met Azure Active Directory (Azure AD).

ADP integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ADP heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ADP (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ADP, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement ADP ingeschakeld

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ADP uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adp-from-the-gallery"></a>ADP uit de galerie toe te voegen
Voor het configureren van de integratie van ADP in Azure AD, moet u ADP uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ADP uit de galerie, moet u de volgende stappen uitvoeren:**

1.  Meld u aan uw Microsoft Azure identity provider-omgeving aan als beheerder.

2. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

3. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
4. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

5. Typ in het zoekvak **ADP**, selecteer **ADP** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ADP in de lijst met resultaten](./media/adpfederatedsso-tutorial/tutorial_adp_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ADP op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in ADP is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ADP tot stand worden gebracht.

In ADP, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ADP, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker ADP](#create-an-adp-test-user)**  : als u wilt een equivalent van Britta Simon in ADP die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing ADP.

**Voor het configureren van Azure AD eenmalige aanmelding met ADP, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ADP** integratie toepassingspagina, klikt u op **tabblad Eigenschappen** en voer de volgende stappen uit: 

    ![Eigenschappen voor eenmalige aanmelding](./media/adpfederatedsso-tutorial/tutorial_adp_prop.png)

    a. Stel de **ingeschakeld voor gebruikers om aan te melden** veld waarde **Ja**.

    b. Kopieer de **URL van gebruikerstoegang** en u hebt te plakken in **sectie configureren aanmeldings-URL**, die later in de zelfstudie wordt uitgelegd.

    c. Stel de **Gebruikerstoewijzing vereist** veld waarde **Ja**.

    d. Stel de **zichtbaar voor gebruikers** veld de waarde die moet worden **Nee**.

2. Klik op **eenmalige aanmelding** op **ADP** toepassingspagina integratie.

    ![Koppeling voor eenmalige aanmelding configureren][4]

3. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/adpfederatedsso-tutorial/tutorial_adp_samlbase.png)

4. Op de **ADP domein en URL's** sectie, voert u de volgende stappen uit:

    ![ADP domein en URL's, eenmalige aanmelding informatie](./media/adpfederatedsso-tutorial/tutorial_adp_url.png)

    In de **id** tekstvak, een URL typen: `https://fed.adp.com` 
    
5. De toepassing ADP wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. Naam van de claim is altijd **"PersonImmutableID"** en de waarde van die we hebben toegewezen aan **employeeid**. 

    Hier wordt de Gebruikerstoewijzing van Azure AD naar ADP worden uitgevoerd op de **employeeid** , maar kunt u dit toewijzen aan een andere waarde op basis van de toepassingsinstellingen van uw. Dus Geef werk met [ADP ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) eerst het gebruik van de juiste id van een gebruiker en wijs die waarde met de **"PersonImmutableID"** claim.

    ![Eenmalige aanmelding configureren](./media/adpfederatedsso-tutorial/tutorial_adp_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | PersonImmutableID | User.EmployeeID |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/adpfederatedsso-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/adpfederatedsso-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

    > [!NOTE] 
    > Voordat u de SAML-verklaring configureren kunt, moet u contact op met uw [ADP ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) en vraagt u de waarde van het kenmerk unieke id voor uw tenant. U moet deze waarde voor het configureren van de aangepaste claim voor uw toepassing. 

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/adpfederatedsso-tutorial/tutorial_adp_certificate.png) 

8. Het configureren van eenmalige aanmelding op **ADP** zijde, moet u de gedownloade uploaden **Metadata XML** op de [ADP website](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Dit proces kan enkele dagen duren. 

### <a name="configure-your-adp-services-for-federated-access"></a>Uw ADP service (s) voor federatieve toegang krijgen tot configureren

>[!Important]
> Uw werknemers die behoefte hebben aan federatieve toegang tot uw services ADP moeten worden toegewezen aan de ADP service-app en vervolgens, gebruikers moeten opnieuw worden toegewezen aan de specifieke ADP-service.
Na ontvangst van de bevestiging van uw vertegenwoordiger ADP door uw ADP service (s) en toewijzen/beheer gebruikers voor het beheren van toegang tot de desbetreffende ADP-service voor gebruikers te configureren.

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **ADP**, selecteer **ADP** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ADP in de lijst met resultaten](./media/adpfederatedsso-tutorial/tutorial_adp_addservicegallery.png)

5. In de Azure-portal op uw **ADP** integratie toepassingspagina, klikt u op **tabblad Eigenschappen** en voer de volgende stappen uit:  

    ![Linkedproperties voor eenmalige aanmelding](./media/adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

    a.  Stel de **ingeschakeld voor gebruikers om aan te melden** veld waarde **Ja**.

    b.  Stel de **Gebruikerstoewijzing vereist** veld waarde **Ja**.

    c.  Stel de **zichtbaar voor gebruikers** veld waarde **Ja**.

6. Klik op **eenmalige aanmelding** op **ADP** toepassingspagina integratie.

    ![Koppeling voor eenmalige aanmelding configureren][4]

7. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **gekoppelde Sign-on**. om te koppelen van uw toepassing **ADP**.

    ![Eenmalige aanmelding gekoppeld](./media/adpfederatedsso-tutorial/tutorial_adp_linked.png)

8. Navigeer naar de **configureren aanmeldings-URL** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding prop](./media/adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)
                                                              
    a. Plak de **URL van gebruikerstoegang**, die u hebt gekopieerd hierboven **tabblad Eigenschappen** (van de belangrijkste ADP-app).
                                                             
    b. Hieronder vindt u de 5 apps die ondersteuning bieden voor verschillende **Relay status URL's**. U moet toevoegen de juiste **Relay status URL** handmatig naar de waarde voor een bepaalde toepassing de **URL van gebruikerstoegang**.
    
    * **Nu ADP personeel**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP werknemers nu een verbeterde tijd**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Sla** uw wijzigingen.

10. Beginnen bij ontvangst van de bevestiging van uw vertegenwoordiger ADP test met een of twee gebruikers.

    a. Enkele gebruikers toewijzen aan de service ADP App federatieve toegang krijgen tot testen.

    b. Test is geslaagd, wanneer gebruikers toegang krijgen de ADP service-app in de galerie tot en toegang hun ADP-service tot krijg.
 
11. Toewijzen op de bevestiging van de test is geslaagd, de federatieve service van ADP aan individuele gebruikers of gebruikersgroepen, die later in de zelfstudie wordt uitgelegd en implementeer uw werknemers. 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/adpfederatedsso-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/adpfederatedsso-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/adpfederatedsso-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/adpfederatedsso-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-adp-test-user"></a>Maak een testgebruiker ADP

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in ADP. Werken met [ADP ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) om toe te voegen de gebruikers in de ADP-account.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ADP.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan ADP toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **ADP**.

    ![De koppeling ADP in de lijst met toepassingen](./media/adpfederatedsso-tutorial/tutorial_adp_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel ADP in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing ADP.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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

