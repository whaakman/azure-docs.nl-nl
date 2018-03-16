---
title: 'Zelfstudie: Azure Active Directory integreren met Palo Alto Networks - gebruikersinterface Admin | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Palo Alto Networks - Admin UI.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: c5be53f06e009cb2d5180e43318c8670139a68db
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="integrate-azure-active-directory-with-palo-alto-networks---admin-ui"></a>Azure Active Directory integreren met netwerken Palo Alto - Admin-gebruikersinterface

In deze zelfstudie leert u hoe Azure Active Directory (Azure AD) integreren met Palo Alto Networks - Admin UI.

Door integratie van Azure AD met netwerken Palo Alto - Admin UI, profiteren van de volgende voordelen:

- U kunt beheren in Azure AD die toegang hebben tot netwerken Palo Alto - Admin UI.
- U kunt uw gebruikers krijgen automatisch aangemeld bij Palo Alto Networks - Admin UI (eenmalige aanmelding of SSO) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie van SaaS-app met Azure AD [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met netwerken Palo Alto - Admin UI, moet u de volgende items:

- Een Azure AD-abonnement
- Een Palo Alto netwerken Next Generation Firewall of Panorama (systeem voor de firewalls gecentraliseerd beheer)

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, raden we aan dat u doen *niet* gebruik van een productieomgeving.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

* Netwerken Palo Alto - Admin UI uit de galerie toevoegen
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-palo-alto-networks---admin-ui-from-the-gallery"></a>Netwerken Palo Alto - Admin UI uit de galerie toevoegen
Toevoegen voor het configureren van de integratie van Azure AD met netwerken Palo Alto - Admin UI Palo Alto Networks - Admin UI uit de galerie aan de lijst met beheerde SaaS-apps als volgt:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het venster 'Bedrijfstoepassingen'][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop bovenaan in het venster.

    !['Nieuwe application' knop][3]

4. Typ in het zoekvak **Palo Alto Networks - Admin UI**, selecteer **Palo Alto Networks - Admin UI** in de lijst met resultaten en selecteer vervolgens **toevoegen**.

    ![Netwerken Palo Alto - Admin UI in de lijst met resultaten](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_step4-add-from-the-gallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en test eenmalige aanmelding Azure AD met netwerken Palo Alto - Admin UI, op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD de netwerken Palo Alto - Admin UI-gebruiker en het bijbehorende equivalent in Azure AD te identificeren. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en dezelfde gebruiker in netwerken Palo Alto - Admin UI worden ingesteld.

Voor het opzetten van de relatie koppeling toewijzen als de netwerken Palo Alto - Admin UI *gebruikersnaam* de waarde van de *gebruikersnaam* in Azure AD.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met netwerken Palo Alto - Admin UI, voltooi de bouwstenen in de volgende vijf secties.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw netwerken Palo Alto - Admin UI toepassing als volgt configureren:

1. In de Azure-portal op de **Palo Alto Networks - Admin UI** toepassing Integratiepagina **eenmalige aanmelding**.

    ![De koppeling "Single sign-on"][4]

2. In de **eenmalige aanmelding** venster in de **modus voor één aanmelding** de optie **op basis van SAML aanmelding**.
 
    ![Het venster "Single sign-on"](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

3. Onder **Palo Alto Networks - domein voor beheer-UI en URL's**, het volgende doen:

    !["Netwerken Palo Alto - URL's en domein voor beheer-UI" eenmalige aanmelding informatie](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_show_advanced_url.png)
    
    a. In de **aanmeldings-URL** vak, typt u een URL in de volgende indeling: *https://\<klant Firewall FQDN > /php/login.php*.

    b. In de **id** vak, typt u een URL in de volgende indeling: *https://\<klant Firewall FQDN >: 443/SAML20/SP*.
    
    c. In de **antwoord-URL** vak, typ de URL van de verklaring Consumer Service (ACS) in de volgende indeling: *https://\<klant Firewall FQDN >: 443/SAML20/SP/ACS*.
    
    > [!NOTE] 
    > De voorgaande waarden zijn niet echt. Deze bijwerken met de werkelijke aanmeldings-URL en de id. De waarden, neem contact op met [Palo Alto Networks - gebruikersinterface Beheerclient ondersteuningsteam](https://support.paloaltonetworks.com/support). 
 
4. Omdat de netwerken Palo Alto - Admin UI toepassing de SAML-asserties in een specifieke indeling verwacht, configureert u de claims zoals weergegeven in de volgende afbeelding. Beheren van de waarden van het kenmerk in de **gebruikerskenmerken** sectie van de **toepassingsintegratie** pagina door de volgende te doen:
    
    ![De lijst met SAML-Token kenmerken](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
   > [!NOTE]
   > Omdat de kenmerkwaarden alleen voorbeelden, wijs de juiste waarden voor *gebruikersnaam* en *adminrole*. Er is een andere optionele kenmerk, *accessdomain*, dat wordt gebruikt voor beheerderstoegang tot specifieke virtuele systemen op de firewall te beperken.
   >
        
    | Naam van kenmerk | De waarde van kenmerk |
    | --- | --- |    
    | gebruikersnaam | user.userprincipalname |
    | adminrole | customadmin |

    a. Selecteer **toevoegen kenmerk**.  
    
    ![De knop 'Add kenmerk'](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_04.png)

    De **kenmerk toevoegen** venster wordt geopend.

    ![Het venster 'Add kenmerk'](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** typt u de naam van het kenmerk dat wordt weergegeven voor die rij.
    
    c. In de **waarde** typt u de waarde van het kenmerk dat wordt weergegeven voor die rij.
    
    d. Selecteer **OK**.

    > [!NOTE]
    > Zie de volgende artikelen voor meer informatie over de kenmerken:
    > * [Beheerdersrol profiel voor Admin UI (adminrole)](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Apparaat toegang domein voor Admin UI (accessdomain)](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain)
    >

5. Onder **SAML-certificaat voor ondertekening van**, selecteer **Metadata XML**, en selecteer vervolgens **opslaan**.

    ![De Metadata XML downloadkoppeling](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

    ![De knop Opslaan](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_400.png)

6. Open de Palo Alto netwerken Firewall Admin gebruikersinterface als een beheerder in een nieuw venster.

7. Selecteer de **apparaat** tabblad.

    ![Het tabblad apparaat](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

8. Selecteer in het linkerdeelvenster **SAML-identiteitsprovider**, en selecteer vervolgens **importeren** het metagegevensbestand te importeren.

    ![De knop importeren metagegevens bestand](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

9. In de **SAML identificeren Provider Server-profiel importeren** venster de volgende handelingen uit:

    ![Het venster 'SAML identificeren Provider Server-profiel importeren'](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. In de **profielnaam** Geef een naam (bijvoorbeeld **AzureAD Admin UI**).
    
    b. Onder **identiteit Provider metagegevens**, selecteer **Bladeren**, en selecteer de metadata.xml-bestand dat u eerder hebt gedownload vanuit de Azure-portal.
    
    c. Schakel de **identiteitscertificaat Provider valideren** selectievakje.
    
    d. Selecteer **OK**.
    
    e. Selecteren om door te voeren de configuraties van de firewall, **doorvoeren**.

10. Selecteer in het linkerdeelvenster **SAML-identiteitsprovider**, en selecteer vervolgens het profiel van SAML-id-Provider (bijvoorbeeld **AzureAD Admin UI**) die u in de vorige stap hebt gemaakt. 

    ![Het profiel van SAML-id-Provider](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

11. In de **SAML identiteit Provider Server-profiel** venster de volgende handelingen uit:

    ![Het venster 'SAML identiteit Provider Server-profiel'](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. In de **identiteit Provider SLO URL** vak, wordt de URL van de eerder geïmporteerd SLO vervangen door de volgende URL:  **https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0** .
  
    b. Selecteer **OK**.

12. Selecteer op de Palo Alto netwerken Firewall Admin UI, **apparaat**, en selecteer vervolgens **beheerdersrollen**.

13. Selecteer de **toevoegen** knop. 

14. In de **Admin rol profiel** venster in de **naam** Geef een naam voor de beheerdersrol (bijvoorbeeld **fwadmin**).  
    De naam van de administrator-rol moet overeenkomen met de naam van het SAML-beheerdersrol-kenmerk dat is verzonden door de identiteitsprovider. De naam van de rol beheerder en de waarde zijn gemaakt in stap 4.

    ![Beheerdersrol Palo Alto-netwerken configureren](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
15. Selecteer op de Firewall Admin UI, **apparaat**, en selecteer vervolgens **Authentication profiel**.

16. Selecteer de **toevoegen** knop. 

17. In de **Authentication profiel** venster de volgende handelingen uit: 

    ![Het venster 'Verificatie profiel'](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. In de **naam** Geef een naam (bijvoorbeeld **AzureSAML_Admin_AuthProfile**).
    
    b. In de **Type** vervolgkeuzelijst, selecteer **SAML**. 
   
    c. In de **IdP-serverprofiel** vervolgkeuzelijst, selecteert u het juiste SAML Identity Provider Server-profiel (bijvoorbeeld **AzureAD Admin UI**).
   
    c. Selecteer de **één afmelding inschakelen** selectievakje.
    
    d. In de **Admin Role-kenmerk** en voer de kenmerknaam (bijvoorbeeld **adminrole**). 
    
    e. Selecteer de **Geavanceerd** tabblad en klik vervolgens onder **lijst toestaan**, selecteer **toevoegen**. 
    
    ![De knop toevoegen op het tabblad Geavanceerd](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)
    
    f. Selecteer de **alle** selectievakje of Selecteer de gebruikers en groepen die kunnen worden geverifieerd met dit profiel.  
    Wanneer een gebruiker zich verifieert, de firewall overeenkomt met de gekoppelde gebruikersnaam of de groep op basis van de vermeldingen in de lijst. Als u geen vermeldingen toevoegt, kunnen er zijn geen gebruikers verifiëren.

    g. Selecteer **OK**.

18. Als u beheerders SAML SSO met behulp van Azure gebruiken, schakelt **apparaat** > **Setup**. In de **Setup** deelvenster, selecteer de **Management** tabblad en klik vervolgens onder **verificatie-instellingen**, selecteer de **instellingen** knop ('stemmen') . 

 ![De knop Instellingen](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

19. Selecteer het SAML-verificatie-profiel dat u hebt gemaakt in stap 17 (bijvoorbeeld **AzureSAML_Admin_AuthProfile**).

 ![Het veld Authentication profiel](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

20. Selecteer **OK**.

21. Selecteren om door te voeren in de configuratie, **doorvoeren**.


> [!TIP]
> Als u de app instelt, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Nadat u hebt toegevoegd in de app de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en vervolgens naar de Embedded-documentatie in de **configuratie** sectie onderaan. Zie voor meer informatie over de functie ingesloten documentatie [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie kunt u testgebruiker Britta Simon in de Azure portal maken door het volgende te doen:

![Een Azure AD-testgebruiker maken][100]

1. Selecteer in de Azure-portal in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-koppeling](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_01.png)

2. Selecteer voor een lijst met actieve gebruikers, **gebruikers en groepen** > **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_02.png)

3. Aan de bovenkant van de **alle gebruikers** Selecteer **toevoegen**.

    ![De knop toevoegen](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_03.png)
    
    De **gebruiker** venster wordt geopend.

4. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster gebruiker](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Een netwerken Palo Alto - Admin UI testgebruiker maken

Netwerken Palo Alto - Admin UI ondersteunt just in time gebruikers inrichten. Als een gebruiker niet al bestaat, wordt deze automatisch gemaakt in het systeem na een geslaagde verificatie. Er is geen actie vereist van kunt maken van de gebruiker.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u gebruiker Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot netwerken Palo Alto - Admin UI. Ga als volgt te werk om dit te doen:

![Toewijzen van de gebruikersrol][200] 

1. Open in de Azure-portal, de **toepassingen** weergeven, gaat u naar de **Directory** weergeven en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![De 'Bedrijfstoepassingen' en 'Alle toepassingen' koppelingen][201] 

2. In de **toepassingen** selecteert **Palo Alto Networks - Admin UI**.

    ![De netwerken Palo Alto - Admin UI-koppeling](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** deelvenster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** venster in de **gebruikers** selecteert **Britta Simon**.

6. Selecteer de **Selecteer** knop.

7. In de **toevoegen toewijzing** Selecteer **toewijzen**.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u de netwerken Palo Alto - Admin UI-tegel in het paneel voor Apptoegang, selecteert moet u worden automatisch aangemeld met uw netwerken Palo Alto - Admin UI-toepassing.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_203.png

