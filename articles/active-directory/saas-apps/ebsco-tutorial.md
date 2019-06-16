---
title: 'Zelfstudie: Azure Active Directory-integratie met EBSCO | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en EBSCO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 8fd0ecf67e3e6492fa3e6d9e12379e06ee3f814d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67103770"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Zelfstudie: Azure Active Directory-integratie met EBSCO

In deze zelfstudie leert u hoe u EBSCO integreren met Azure Active Directory (Azure AD).
EBSCO integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot EBSCO heeft.
* U kunt uw gebruikers worden automatisch aangemeld EBSCO (Single Sign-On) met hun Azure AD-accounts inschakelen.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met EBSCO, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* Eenmalige aanmelding EBSCO ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor EBSCO **SP** en **IDP** gestart door SSO

* Biedt ondersteuning voor EBSCO **Just In Time** inrichten van gebruikers

## <a name="adding-ebsco-from-the-gallery"></a>EBSCO uit de galerie toe te voegen

Voor het configureren van de integratie van EBSCO in Azure AD, moet u EBSCO uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen EBSCO uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)** klikt u in het navigatievenster aan de linkerkant op het pictogram **Azure Active Directory**.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u wilt een nieuwe toepassing toevoegen, klikt u op de **nieuwe toepassing** knop aan de bovenkant van het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **EBSCO**, selecteer **EBSCO** vanuit het deelvenster resultaat klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

     ![EBSCO in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met EBSCO op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in EBSCO tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met EBSCO, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)** : als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van eenmalige aanmelding EBSCO](#configure-ebsco-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)** : als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)** : als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maken van de testgebruiker EBSCO](#create-ebsco-test-user)**  : als u wilt een equivalent van Britta Simon in EBSCO die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)** : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met EBSCO, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **EBSCO** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus, de volgende stap uitvoeren:

    ![EBSCO domein en URL's, eenmalige aanmelding informatie](common/idp-identifier.png)

    In het tekstvak **Id** typt u een URL: `pingsso.ebscohost.com`

5. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    ![image](common/both-preintegrated-signon.png)

    In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`

    > [!NOTE]
    > De waarde voor de aanmeldings-URL is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [EBSCO Client ondersteuningsteam](mailto:sso@ebsco.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

    o **unieke elementen:**  

    o **Custid** Enter unieke EBSCO klant-ID = 

    o **profiel** = Clients de koppeling om de gebruikers aan een specifiek profiel (afhankelijk van wat ze bij EBSCO aanschaffen) kunnen aanpassen. Ze kunnen een specifiek profiel-ID invoeren De belangrijkste-id's zijn eds (EBSCO Discovery-Service) en ehost (EBSOCOhost databases). Instructies voor dezelfde krijgen [hier](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

6. EBSCO toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram  **Bewerken**  om het dialoogvenster  **Gebruikerskenmerken**  te openen.

    ![image](common/edit-attribute.png)

     > [!Note]
    > De **naam** kenmerk verplicht is en is toegewezen met **naam-id-waarde** in EBSCO toepassing. Dit is standaard toegevoegd zodat u niet hoeft op dit handmatig toevoegen.

7. Bovendien hierboven verwacht EBSCO toepassing paar meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de sectie **Gebruikersclaims** in het dialoogvenster **Gebruikerskenmerken** voert u de volgende stappen uit om het kenmerk van het SAML-token toe te voegen zoals wordt weergegeven in de onderstaande tabel: 

    | Name | Bronkenmerk|
    | ---------------| --------------- |    
    | FirstName   | user.givenname |
    | LastName   | user.surname |
    | Email   | user.mail |

    a. Klik op **Nieuwe claim toevoegen** om het dialoogvenster **Gebruikersclaims beheren** te openen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Laat **Naamruimte** leeg.

    d. Selecteer Bron bij **Kenmerk**.

    e. Typ de kenmerkwaarde voor die rij in de lijst met **bronkenmerken**.

    f. Klik op **Opslaan**.

8. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

9. Op de **EBSCO instellen** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-ebsco-single-sign-on"></a>Configureer EBSCO Single Sign-On

Het configureren van eenmalige aanmelding op **EBSCO** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** en toepassing van de gekopieerde URL's van Azure portal om te [EBSCO ondersteuningsteam](mailto:sso@ebsco.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype brittasimon@yourcompanydomain.extension. Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan EBSCO.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **EBSCO**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **EBSCO**.

    ![De koppeling EBSCO in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-ebsco-test-user"></a>EBSCO testgebruiker maken

In het geval van EBSCO wordt het automatisch inrichten van gebruikers.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

Azure AD wordt de vereiste gegevens doorgegeven aan EBSCO toepassing. EBSCO van gebruikersinrichting kan worden automatische of vereisen dat een eenmalige formulier. Dat hangt ervan af of de client heeft een groot aantal bestaande EBSCOhost accounts met persoonlijke instellingen die zijn opgeslagen. Hetzelfde kan worden besproken met de [EBSCO ondersteuningsteam](mailto:sso@ebsco.com) tijdens de implementatie. In beide gevallen de client hoeft te maken van accounts EBSCOhost vóór testen.

   >[!Note]
   >U kunt EBSCOhost inrichting/persoonlijke instellingen automatiseren. Neem contact op met [EBSCO ondersteuningsteam](mailto:sso@ebsco.com) over Just-In-Time inrichting van gebruikers. 

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

1. Wanneer u op de tegel EBSCO in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing EBSCO.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

2. Als u zich bij de toepassing aanmelden, klikt u op de **aanmelden** knop in de rechterbovenhoek.

    ![De aanmelding EBSCO in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. Ontvangt u een eenmalige prompt om te koppelen van de institutionele/SAML-aanmelding met een **nu uw bestaand MyEBSCOhost-account koppelt aan uw account instelling** of **een nieuw MyEBSCOhost-account maken en een koppeling naar uw instelling account**. Het account wordt gebruikt voor persoonlijke instellingen op de toepassing EBSCOhost. Selecteer de optie **een nieuw account maken** en u ziet dat het formulier voor persoonlijke instellingen vooraf ingevuld met de waarden van het saml-antwoord, is zoals wordt weergegeven in de onderstaande schermafbeelding. Klik op **'Doorgaan'** om op te slaan deze selectie.
    
     ![De gebruiker EBSCO in de lijst met toepassingen](./media/ebsco-tutorial/tutorial_ebsco_user.png)

1. Na het voltooien van de bovenstaande instellingen, cookies/cache leegmaken en aanmelding opnieuw. U hoeft te handmatig opnieuw aan te melden en de persoonlijke instellingen worden onthouden

## <a name="additional-sesources"></a>Aanvullende sesources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

