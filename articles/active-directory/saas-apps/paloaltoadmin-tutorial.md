---
title: 'Zelfstudie: Azure Active Directory-integratie met Palo Alto Networks - Admin UI | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Palo Alto Networks - Admin UI.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: fed368c0df265495d9fee764f86825957fae8bab
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447421"
---
# <a name="integrate-azure-active-directory-with-palo-alto-networks---admin-ui"></a>Azure Active Directory integreren met Palo Alto Networks - beheerders-UI

In deze zelfstudie leert u hoe u Azure Active Directory (Azure AD) integreren met Palo Alto Networks - Admin UI.

Door integratie van Azure AD met Palo Alto Networks - Admin UI, krijgt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Palo Alto Networks - Admin UI heeft.
- U kunt uw gebruikers krijgen automatisch aangemeld bij Palo Alto Networks - Admin-UI (eenmalige aanmelding of SSO) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over SaaS-app-integratie met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Palo Alto Networks - Admin UI, moet u de volgende items:

- Een Azure AD-abonnement
- Een Palo Alto Networks volgende-Generatiefirewall of Panorama (systeem voor de firewalls gecentraliseerd beheer)

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, raden we aan dat u doen *niet* een productie-omgeving gebruiken.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

* Palo Alto Networks - Admin UI uit de galerie toevoegen
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-palo-alto-networks---admin-ui-from-the-gallery"></a>Palo Alto Networks - Admin UI uit de galerie toevoegen
Toevoegen voor het configureren van de integratie van Azure AD met Palo Alto Networks - Admin UI Palo Alto Networks - Admin UI uit de galerie aan de lijst met beheerde SaaS-apps door het volgende te doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

1. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het venster 'Enterprise Application'][2]
    
1. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het venster.

    ![De 'nieuwe toepassing"knop][3]

1. Typ in het zoekvak **Palo Alto Networks - Admin UI**, selecteer **Palo Alto Networks - Admin UI** in de lijst met resultaten en selecteer vervolgens **toevoegen**.

    ![Palo Alto Networks - Admin UI in de lijst met resultaten](./media/paloaltoadmin-tutorial/tutorial_step4-add-from-the-gallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Palo Alto Networks - Admin UI, op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD bepalen het Palo Alto Networks - Admin UI-gebruiker en het bijbehorende equivalent in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en dezelfde gebruiker in Palo Alto Networks - Admin UI worden ingesteld.

Voor het maken van de relatie koppeling toewijzen als de Palo Alto Networks - Admin UI *gebruikersnaam* de waarde van de *gebruikersnaam* in Azure AD.

Voltooi de bouwstenen die u in de volgende vijf secties om te configureren en testen van Azure AD eenmalige aanmelding met Palo Alto Networks - Admin UI.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw Palo Alto Networks - Admin UI toepassing door de volgende te doen:

1. In de Azure-portal op de **Palo Alto Networks - Admin UI** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![De koppeling "Single sign-on"][4]

1. In de **eenmalige aanmelding** venster in de **modus voor één aanmelding** Schakel **SAML gebaseerde aanmelding**.
 
    ![Het venster "Single sign-on"](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

1. Onder **Palo Alto Networks - Admin-gebruikersinterface domein en URL's**, doet u het volgende:

    !['Palo Alto Networks - Admin-gebruikersinterface domein en URL's ' eenmalige aanmelding informatie](./media/paloaltoadmin-tutorial/tutorial_general_show_advanced_url.png)
    
    a. In de **aanmeldings-URL** vak, typ een URL in de volgende indeling: *https://\<klant Firewall FQDN-naam > /php/login.php*.

    b. In de **id** vak, typ een URL in de volgende indeling: *https://\<klant Firewall Fully Qualified Domain Name >: 443/SAML20/SP*.
    
    c. In de **antwoord-URL** vak, typ de URL Assertion Consumer Service (ACS) in de volgende indeling: *https://\<klant Firewall Fully Qualified Domain Name >: 443/SAML20/SP/ACS*.
    
    > [!NOTE] 
    > De bovenstaande waarden zijn niet echt. Deze bijwerken met de werkelijke aanmeldings-URL en -id. De waarden, neem contact op met [Palo Alto Networks - Beheerclient UI-ondersteuningsteam](https://support.paloaltonetworks.com/support). 
 
1. Omdat de Palo Alto Networks - Admin UI-toepassing wordt verwacht de SAML-asserties ondertekend in een specifieke indeling dat, configureert u de claims, zoals wordt weergegeven in de volgende afbeelding. Beheren van de waarden van het kenmerk in de **gebruikerskenmerken** sectie van de **toepassingsintegratie** pagina door het volgende te doen:
    
    ![De kenmerkenlijst SAML-Token](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
   > [!NOTE]
   > Omdat de kenmerkwaarden slechts voorbeelden zijn, wijzen de juiste waarden voor *gebruikersnaam* en *adminrole*. Er is een andere optionele kenmerk *accessdomain*, die wordt gebruikt om te beperken van beheerderstoegang tot specifieke virtuele systemen in de firewall.
   >
        
    | De naam van kenmerk | De waarde van kenmerk |
    | --- | --- |    
    | gebruikersnaam | User.userPrincipalName |
    | adminrole | customadmin |

    a. Selecteer **kenmerk toevoegen**.  
    
    ![De knop 'Kenmerk toevoegen'](./media/paloaltoadmin-tutorial/tutorial_attribute_04.png)

    De **kenmerk toevoegen** venster wordt geopend.

    ![Het venster 'Kenmerk toevoegen'](./media/paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** typt u de naam van het kenmerk dat wordt weergegeven voor die rij.
    
    c. In de **waarde** typt u de waarde van het kenmerk dat wordt weergegeven voor die rij.
    
    d. Selecteer **OK**.

    > [!NOTE]
    > Zie de volgende artikelen voor meer informatie over de kenmerken:
    > * [Beheerdersrol profiel Admin-UI (adminrole)](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Toegang tot het apparaatdomein Admin-UI (accessdomain)](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain)
    >

1. Onder **SAML-handtekeningcertificaat**, selecteer **Metadata XML**, en selecteer vervolgens **opslaan**.

    ![De XML Metadata downloadkoppeling](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

    ![De knop Opslaan](./media/paloaltoadmin-tutorial/tutorial_general_400.png)

1. Open de Palo Alto Networks Firewall beheerders-UI als een beheerder in een nieuw venster.

1. Selecteer de **apparaat** tabblad.

    ![Het tabblad apparaat](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

1. Selecteer in het linkerdeelvenster **SAML-identiteitsprovider**, en selecteer vervolgens **importeren** voor het importeren van het bestand met metagegevens.

    ![De knop importeren van de metagegevens van bestand](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

1. In de **SAML identificeren Provider Server-profiel importeren** venster de volgende handelingen uit:

    ![Het venster 'SAML identificeren Provider Server-profiel importeren'](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. In de **profielnaam** Geef een naam (bijvoorbeeld **AzureAD beheerders-UI**).
    
    b. Onder **Identity Provider metagegevens**, selecteer **Bladeren**, en selecteer het metadata.xml-bestand dat u eerder hebt gedownload vanuit Azure portal.
    
    c. Schakel de **identiteitscertificaat Provider valideren** selectievakje.
    
    d. Selecteer **OK**.
    
    e. Selecteer voor het doorvoeren van de configuraties op de firewall, **doorvoeren**.

1. Selecteer in het linkerdeelvenster **SAML-identiteitsprovider**, en selecteer vervolgens het profiel SAML-id-Provider (bijvoorbeeld **AzureAD beheerders-UI**) die u in de vorige stap hebt gemaakt. 

    ![Het profiel SAML-id-Provider](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

1. In de **SAML-id-Provider Server-profiel** venster de volgende handelingen uit:

    ![Het venster 'SAML-id-Provider Server-profiel'](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. In de **SLO-URL van identiteitsprovider** vak, wordt de eerder geïmporteerd SLO-URL vervangen door de volgende URL: **https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0**.
  
    b. Selecteer **OK**.

1. Selecteer op het Palo Alto Networks-Firewall Admin UI **apparaat**, en selecteer vervolgens **-beheerdersrollen**.

1. Selecteer de **toevoegen** knop. 

1. In de **Admin rol profiel** venster in de **naam** Geef een naam op voor de rol van beheerder (bijvoorbeeld **fwadmin**).  
    De naam van de administrator-rol moet overeenkomen met de naam van het SAML-beheerdersrol-kenmerk dat is verzonden door de id-Provider. De naam van de rol beheerder en de waarde zijn gemaakt in stap 4.

    ![Palo Alto Networks-beheerdersrol configureren](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
1. Selecteer op de Firewall Admin UI **apparaat**, en selecteer vervolgens **verificatie profiel**.

1. Selecteer de **toevoegen** knop. 

1. In de **verificatie profiel** venster de volgende handelingen uit: 

    ![Het venster 'Verificatie profiel'](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. In de **naam** Geef een naam (bijvoorbeeld **AzureSAML_Admin_AuthProfile**).
    
    b. In de **Type** vervolgkeuzelijst, selecteer **SAML**. 
   
    c. In de **IdP Server-profiel** vervolgkeuzelijst, selecteert u het juiste profiel van de Server van SAML-id-Provider (bijvoorbeeld **AzureAD beheerders-UI**).
   
    c. Selecteer de **eenmalige afmelding inschakelen** selectievakje.
    
    d. In de **Admin Role-kenmerk** vak, voer de kenmerknaam (bijvoorbeeld **adminrole**). 
    
    e. Selecteer de **Geavanceerd** tabblad en klik vervolgens onder **acceptatielijst**, selecteer **toevoegen**. 
    
    ![De knop toevoegen op het tabblad Geavanceerd](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)
    
    f. Selecteer de **alle** selectievakje, of Selecteer de gebruikers en groepen die kunnen worden geverifieerd met dit profiel.  
    Wanneer een gebruiker zich verifieert, de firewall overeenkomt met de gekoppelde gebruikersnaam of de groep op basis van de vermeldingen in de lijst. Als u geen vermeldingen toevoegt, er zijn geen gebruikers worden geverifieerd.

    g. Selecteer **OK**.

1. Als beheerders SAML SSO met behulp van Azure gebruiken, schakelt **apparaat** > **Setup**. In de **Setup** venster de **Management** tabblad en klik vervolgens onder **verificatie-instellingen**, selecteer de **instellingen** knop ("tandwielpictogram") . 

 ![De knop Instellingen](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

1. Selecteer het profiel SAML-verificatie die u hebt gemaakt in stap 17 (bijvoorbeeld **AzureSAML_Admin_AuthProfile**).

 ![Het veld voor de verificatie-profiel](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

1. Selecteer **OK**.

1. Selecteer voor het doorvoeren van de configuratie, **doorvoeren**.


> [!TIP]
> Als u de app instelt, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Nadat u hebt toegevoegd in de app de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad, en vervolgens toegang tot de ingesloten in de documentatie bij de **configuratie** sectie aan de onderkant. Zie voor meer informatie over de functie ingesloten documentatie [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt maken u testgebruiker Britta Simon in Azure portal door het volgende te doen:

![Maak een testgebruiker Azure AD][100]

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-koppeling](./media/paloaltoadmin-tutorial/create_aaduser_01.png)

1. Selecteer voor een lijst van de huidige gebruikers, **gebruikers en groepen** > **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/paloaltoadmin-tutorial/create_aaduser_02.png)

1. Aan de bovenkant van de **alle gebruikers** venster **toevoegen**.

    ![De knop toevoegen](./media/paloaltoadmin-tutorial/create_aaduser_03.png)
    
    De **gebruiker** venster wordt geopend.

1. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster van de gebruiker](./media/paloaltoadmin-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Een Palo Alto Networks - Admin UI testgebruiker maken

Palo Alto Networks - Admin UI biedt ondersteuning voor de just-in-time gebruikersinrichting. Als een gebruiker nog niet bestaat, wordt deze automatisch gemaakt in het systeem na een geslaagde verificatie. Er is geen actie vereist van u te maken van de gebruiker.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u gebruiker Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Palo Alto Networks - Admin UI. Om dit te doen, het volgende doen:

![De de gebruikersrol toewijzen][200] 

1. Open in de Azure-portal, de **toepassingen** weergeven, gaat u naar de **Directory** weergeven en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![De 'Zakelijke toepassingen' en 'Alle Application' koppelingen][201] 

1. In de **toepassingen** in de lijst met **Palo Alto Networks - Admin UI**.

    ![De Palo Alto Networks - Admin UI-koppeling](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

1. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** venster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

1. In de **gebruikers en groepen** venster in de **gebruikers** in de lijst met **Britta Simon**.

1. Selecteer de **Selecteer** knop.

1. In de **toevoegen toewijzing** venster **toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de Palo Alto Networks - Admin UI-tegel in het toegangsvenster, selecteert moet u worden aangemeld automatisch bij uw Palo Alto Networks - Admin UI-toepassing.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/paloaltoadmin-tutorial/tutorial_general_203.png

