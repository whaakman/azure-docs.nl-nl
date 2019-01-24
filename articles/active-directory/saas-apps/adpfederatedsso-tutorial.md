---
title: 'Zelfstudie: Azure Active Directory-integratie met ADP | Microsoft Docs'
description: Leer hoe u eenmalige aanmelding configureert tussen Azure Active Directory en ADP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/04/2019
ms.author: jeedes
ms.openlocfilehash: 981f4d78a351b6d2b61253407e5021dbeb285204
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819540"
---
# <a name="tutorial-azure-active-directory-integration-with-adp"></a>Zelfstudie: Azure Active Directory-integratie met ADP

In deze zelfstudie leert u hoe u ADP kunt integreren met Azure Active Directory (Azure AD).
ADP integreren met Azure AD biedt de volgende voordelen:

* U kunt in Azure AD bepalen wie er toegang heeft tot ADP.
* U kunt inschakelen dat gebruikers automatisch met hun Azure AD-account worden aangemeld bij ADP (eenmalige aanmelding).
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ADP hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Een abonnement op ADP waarvoor eenmalige aanmelding is ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* ADP biedt ondersteuning voor door **IDP** geïnitieerde eenmalige aanmelding

## <a name="adding-adp-from-the-gallery"></a>ADP toevoegen vanuit de galerie

Om de integratie van ADP te configureren in Azure AD moet u ADP vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om ADP toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **ADP** in het zoekvak, selecteer **ADP** in het deelvenster met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![ADP toevoegen vanuit de galerie](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In dit gedeelte configureert en test u eenmalige aanmelding van Azure AD met ADP op basis van een testgebruiker met de naam **Britta Simon**.
Eenmalige aanmelding werkt alleen als er een koppelingsrelatie tussen een Azure AD-gebruiker en de daaraan gerelateerde gebruiker in ADP tot stand is gebracht.

Om eenmalige aanmelding van Azure AD te configureren en testen met ADP, moet u de volgende procedures voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding bij ADP configureren](#configure-adp-single-sign-on)**: als u de instellingen voor eenmalige aanmelding aan de clientzijde wilt configureren.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[ADP-testgebruiker maken](#create-adp-test-user)**: als u een tegenhanger van Britta Simon wilt maken in ADP die is gekoppeld aan de Azure AD-weergave van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met ADP moet u de volgende stappen uitvoeren:

1. Ga in de Azure-portal naar de overzichtspagina van de integratie **ADP**, klik op het tabblad **Eigenschappen** en voer de volgende stappen uit: 

    ![Eigenschappen voor eenmalige aanmelding](./media/adpfederatedsso-tutorial/tutorial_adp_prop.png)

    a. Stel het veld **Ingeschakeld zodat gebruikers zich kunnen aanmelden** in op **Ja**.

    b. Kopieer de waarde van **URL van gebruikerstoegang** om deze verderop in de zelfstudie te plakken in de sectie **Aanmeldings-URL configureren**.

    c. Stel de waarde van het veld **Gebruikerstoewijzing vereist** in op **Ja**.

    d. Stel de waarde van het veld **Zichtbaar voor gebruikers** in op **Nee**.

2. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina van de integratie **ADP** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

3. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

4. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

5. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Gegevens van domein en URL's voor eenmalige aanmelding bij ADP](common/idp-identifier.png)

    Typ een URL in het vak **Id (Entiteits-id)**: `https://fed.adp.com`

6. ADP verwacht dat de SAML-asserties een specifieke indeling hebben. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit de sectie **Gebruikerskenmerken** op de integratiepagina van de toepassing-beheren. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op de knop **Bewerken** om het dialoogvenster **Gebruikerskenmerken** te openen. De naam van de claim is altijd **PersonImmutableID** en de waarde hiervan wordt zoals u kunt zien toegewezen aan **employeeid**. 

    De gebruikerstoewijzing van Azure AD naar ADP wordt uitgevoerd op basis van de **employeeid**, maar u kunt een andere waarde gebruiken op basis van de toepassingsinstellingen. Overleg eerst met het [ADP-ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) om de juiste id van een gebruiker te gebruiken en wijs die waarde toe met de claim **PersonImmutableID**.

    ![image](common/edit-attribute.png)

7. Bewerk in het gedeelte **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** de claims met het **pictogram Bewerken** of voeg de claims toe door met **Nieuwe claim toevoegen** het kenmerk van het SAML-token te configureren, zoals wordt weergegeven in de bovenstaande afbeelding. Hierna voert u de volgende stappen uit:
    
    | Naam | Bronkenmerk | 
    | ---------------| --------------- |
    | PersonImmutableID  | user.employeeid |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **OK**.

    g. Klik op **Opslaan**.

    > [!NOTE] 
    > Voordat u de SAML-assertie kunt configureren, moet u contact opnemen met het [ADP-ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) en de waarde opvragen van het kenmerk voor de unieke gebruikers-id voor uw tenant. U hebt deze waarde nodig voor het configureren van de aangepaste claim voor uw toepassing. 

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

### <a name="configure-adp-single-sign-on"></a>Eenmalige aanmelding bij ADP configureren

Om eenmalige aanmelding te configureren in **ADP**, moet u het gedownloade **XML-bestand met metagegevens** uploaden naar de [ADP-website](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Dit proces kan enkele dagen duren.

### <a name="configure-your-adp-services-for-federated-access"></a>De ADP service(s) configureren voor federatieve toegang

>[!Important]
> Werknemers die federatieve toegang tot ADP-services nodig hebben, moeten worden toegewezen aan de ADP-service-app en daarna aan de specifieke ADP-service.
Nadat uw ADP-vertegenwoordiger de toewijzing heeft bevestigd, configureert u de ADP-service(s) en wijst u gebruikers toe of beheert u deze om de toegang van gebruikers tot de desbetreffende ADP-service te bepalen.

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **ADP** in het zoekvak, selecteer **ADP** in het deelvenster met resultaten en klik vervolgens op de knop **Toevoegen** om de toepassing toe te voegen.

     ![ADP toevoegen vanuit de galerie](common/search-new-app.png)

5. Ga in de Azure-portal naar de overzichtspagina van de integratie **ADP**, klik op het tabblad **Eigenschappen** en voer de volgende stappen uit:  

    ![Gekoppelde eigenschappen voor eenmalige aanmelding](./media/adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

    a.  Stel het veld **Ingeschakeld zodat gebruikers zich kunnen aanmelden** in op **Ja**.

    b.  Stel de waarde van het veld **Gebruikerstoewijzing vereist** in op **Ja**.

    c.  Stel de waarde van het veld **Zichtbaar voor gebruikers** in op **Ja**.

6. Ga in de [Azure-portal](https://portal.azure.com/) naar de pagina van de integratie **ADP** en selecteer **Eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

7. Selecteer in het dialoogvenster **Selecteer een methode voor eenmalige aanmelding** de **modus** **Gekoppeld** om uw toepassing aan **ADP** te koppelen.

    ![Eenmalige aanmelding in de modus Gekoppeld](./media/adpfederatedsso-tutorial/tutorial_adp_linked.png)

8. Ga naar de sectie **Aanmeldings-URL configureren** en voer de volgende stappen uit:

    ![Aanmeldings-URL voor eenmalige aanmelding configureren](./media/adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)

    a. Plak de **User access URL**, die u eerder hebt gekopieerd van het **tabblad Properties** van de ADP-app.
                                                             
    b. Hieronder ziet u de vijf apps die ondersteuning bieden voor verschillende **relaystate-URL's**. U moet de juiste **relaystate-URL** voor een bepaalde toepassing handmatig toevoegen aan de **User access URL**.
    
    * **ADP Workforce Now**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP Workforce Now Enhanced Time**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. Kies **Opslaan** om de wijzigingen op te slaan.

10. Nadat uw bevestiging heeft ontvangen van de ADP-vertegenwoordiger, begint u te testen met een of twee gebruikers.

    a. Wijs enkele gebruikers toe aan de ADP-service-app om federatieve toegang te testen.

    b. De test is geslaagd wanneer gebruikers via de ADP service-app in de galerie toegang krijgen tot hun ADP-service.
 
11. Als u bericht krijgt dat de test is geslaagd, wijst u de federatieve ADP-service toe aan individuele gebruikers of gebruikersgroepen en implementeert u de service voor uw werknemers. Deze toewijzing wordt later in de zelfstudie uitgelegd.
 
### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon** in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In dit gedeelte gaat u Britta Simon toestemming geven voor gebruik van eenmalige aanmelding met Azure door haar toegang te geven tot ADP.

1. Selecteer in de Azure-portal achtereenvolgens **Bedrijfstoepassingen**, **Alle toepassingen** en **ADP**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Typ en selecteer **ADP** in de lijst met toepassingen.

    ![De koppeling ADP in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-adp-test-user"></a>ADP-testgebruiker maken

Het doel van deze sectie is om in ADP een gebruiker met de naam Britta Simon te maken. Neem contact op met het [ADP-ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) om gebruikers toe te voegen aan het ADP-account. 

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u in het toegangsvenster op de tegel ADP klikt, wordt u automatisch aangemeld bij de instantie van ADP waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

